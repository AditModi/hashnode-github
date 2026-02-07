---
title: "ECS Task Rebalancing: How AWS Finally Solved a 9-Year-Old Problem"
datePublished: Fri Jan 16 2026 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: cmlcgw0fe000602jsd2tcgmvr
slug: ecs-task-rebalancing-how-aws-finally-solved-a-9-year-old-problem
tags: aws

---

I still remember the first time I hit this problem. It was 2019, I was managing an ECS cluster running a dozen microservices, and I had just scaled up the cluster by adding new instances. Looking at the ECS console, I saw something that made no sense: three instances were running at 90% CPU utilization, while two freshly launched instances sat there doing absolutely nothing.​

"Why won't ECS move some tasks to the new instances?" I asked my team. The answer: **it doesn't**. ECS doesn't rebalance tasks after they've been placed. Once a task lands on an instance, it stays there until it terminates naturally or you force a redeployment. Your brand new instances? They'll sit idle until new tasks get scheduled, which might never happen if your services are already at their desired count.​

This wasn't just my problem. GitHub issue #105, opened in October 2015 by Abby Fuller (a Principal Developer Advocate at AWS at the time), captured the frustration perfectly: "We're probably getting 75% utilization, and 25% is going to waste". The issue accumulated 120 comments over nine years, with developers sharing workarounds, complaining about wasted resources, and asking—sometimes desperately—when AWS would fix it.​

In November 2024, AWS finally shipped the solution: **Availability Zone Rebalancing**. Then in September 2025, they made it the **default behavior** for all new ECS services. This fundamentally changes how ECS manages task distribution and high availability. Let me show you what was broken, why it mattered, and how the new feature actually works.

## The Problem: Static Task Placement

To understand why this was such a big deal, let's look at what ECS task placement actually does.

## How ECS Schedules Tasks

When you create or scale up an ECS service, the scheduler needs to decide which EC2 instances (or Fargate availability zones) should run your tasks. You can configure placement strategies to influence this decision:​

```json
"placementStrategy": [
  {
    "field": "attribute:ecs.availability-zone",
    "type": "spread"
  },
  {
    "field": "memory",
    "type": "binpack"
  }
]
```

This common strategy tells ECS: "Spread tasks evenly across availability zones for high availability, then within each AZ, pack tasks densely by memory to maximize resource utilization".​

The strategy works great **at task launch time**. When you initially deploy 12 tasks across 3 AZs with 2 instances per AZ, ECS nicely distributes 4 tasks per AZ, roughly 2 tasks per instance.​

## Where It All Falls Apart

The problem is that placement strategies are **only evaluated when tasks are placed**. They're not continuously enforced. Consider this scenario:​

**Initial state:**

* 3 Availability Zones (us-east-1a, us-east-1b, us-east-1c)
    
* 2 EC2 instances per AZ = 6 total instances
    
* Service with 12 tasks using "spread across AZ, binpack on memory"
    
* Result: 4 tasks per AZ, nicely balanced
    

**Scenario 1: AZ disruption**

* AZ us-east-1c has a disruption
    
* 4 tasks in us-east-1c fail and need to be replaced
    
* ECS launches 4 replacement tasks in us-east-1a and us-east-1b
    
* **New state**: 6 tasks in us-east-1a, 6 tasks in us-east-1b, 0 tasks in us-east-1c
    
* AZ us-east-1c recovers, instances come back online
    
* **Problem**: Those instances sit idle. ECS never moves tasks back
    

**Scenario 2: Cluster scale-up**

* You add 3 more instances (one per AZ) to handle expected traffic growth
    
* All 12 tasks are already running on the original 6 instances
    
* **Problem**: The new instances remain empty unless you force redeployment or scale up the service​
    

**Scenario 3: Uneven depletion**

* You're running 10 tasks across 10 t2.medium instances (2 vCPU each)
    
* Over time, tasks naturally churn—some finish, some fail, some get replaced
    
* Due to timing and resource availability, tasks gradually cluster on certain instances
    
* **Result**: 5 instances at 80-90% utilization, 5 instances at 10-20% utilization
    
* The heavily loaded instances burn CPU credits faster (for t2/t3 instances) and risk throttling
    
* **Problem**: ECS never rebalances to even out the load​
    

This last scenario was particularly painful. Nathan Peck (Principal Developer Advocate on the ECS team, now at Fermyon) commented on the issue in 2015: "Right now the only way to get ECS to operate smoothly is to allocate tons of extra server capacity... I'd expect to need maybe 10%-15% extra spare capacity, but it seems that things only work properly if there is nearly 50% spare capacity available".​

That's an enormous waste. You're paying for infrastructure that sits idle just to ensure you have enough fragmented capacity to deploy updates.

## The Real-World Impact

Let me quantify this with a real example from a team I worked with in 2021:

* **Cluster**: 30 c5.xlarge instances (4 vCPU, 8GB RAM each) across 3 AZs
    
* **Workload**: 120 microservice tasks, each needing 1 vCPU, 2GB RAM
    
* **Expected capacity**: Each instance can host 4 tasks = 120 tasks across 30 instances (perfect fit)
    
* **Actual state after 3 months**:
    
    * 20 instances running 5-6 tasks each (oversubscribed, frequently failing)
        
    * 10 instances running 0-2 tasks each (severely underutilized)
        
    * Effective cluster utilization: ~65%
        
    * Deployments frequently failed due to "insufficient resources" errors​
        

We were paying for 30 instances but effectively using capacity equivalent to ~20 instances. The other ~10 instances' worth of capacity was fragmented across the cluster, unusable because tasks were poorly distributed.

**Our workaround**: A Lambda function that ran every 4 hours, forced service redeployments with controlled rollout parameters, and drained underutilized instances. It worked, but it was 200+ lines of code that shouldn't need to exist. And forced redeployments meant unnecessary container restarts and temporary capacity reduction.​

## The Community Response: Nine Years of Workarounds

The GitHub issue became a monument to developer frustration and ingenuity. Let me highlight some of the community workarounds:

## Workaround 1: Forced Periodic Redeployments

Multiple users suggested forcing service redeployments to rebalance tasks:​

```bash
# Force ECS to redeploy all tasks
aws ecs update-service \
  --cluster my-cluster \
  --service my-service \
  --force-new-deployment
```

This triggers ECS to launch new tasks and terminate old ones, giving the scheduler a fresh opportunity to apply placement strategies.​

**The catch**: Your application must tolerate restarts. For services with long-lived connections or in-memory state, forced redeployments can be disruptive. And you need to carefully manage deployment parameters to avoid capacity issues:

```bash
# Use constrained deployment configuration
aws ecs update-service \
  --cluster my-cluster \
  --service my-service \
  --force-new-deployment \
  --deployment-configuration '{
    "maximumPercent": 200,
    "minimumHealthyPercent": 100
  }'
```

This ensures you have enough capacity to run both old and new tasks during redeployment, but it temporarily doubles your resource usage.

## Workaround 2: Draining Underutilized Instances

User @lobsterdore shared a clever Lambda-based solution in February 2022:​

```python
import boto3

ecs = boto3.client('ecs')
ec2 = boto3.client('ec2')

def lambda_handler(event, context):
    """
    Find the least-utilized container instance.
    If its tasks can fit on other instances, drain it.
    Auto Scaling will eventually terminate the drained instance.
    """
    cluster = 'my-cluster'
    
    # Get all container instances
    instances = ecs.list_container_instances(cluster=cluster)
    
    # Describe instances to get resource utilization
    descriptions = ecs.describe_container_instances(
        cluster=cluster,
        containerInstances=instances['containerInstanceArns']
    )
    
    # Find least utilized instance
    min_utilization = float('inf')
    target_instance = None
    
    for instance in descriptions['containerInstances']:
        registered_cpu = next(r['integerValue'] for r in instance['registeredResources'] if r['name'] == 'CPU')
        remaining_cpu = next(r['integerValue'] for r in instance['remainingResources'] if r['name'] == 'CPU')
        utilization = (registered_cpu - remaining_cpu) / registered_cpu
        
        if utilization < min_utilization:
            min_utilization = utilization
            target_instance = instance
    
    # Check if tasks on this instance can fit elsewhere
    tasks_on_target = ecs.list_tasks(
        cluster=cluster,
        containerInstance=target_instance['containerInstanceArn']
    )
    
    # Calculate total resource requirements of tasks on target
    task_descriptions = ecs.describe_tasks(
        cluster=cluster,
        tasks=tasks_on_target['taskArns']
    )
    
    total_cpu_needed = sum(
        task['cpu'] for task in task_descriptions['tasks']
    )
    
    # Calculate available capacity on other instances
    available_capacity = sum(
        next(r['integerValue'] for r in inst['remainingResources'] if r['name'] == 'CPU')
        for inst in descriptions['containerInstances']
        if inst['containerInstanceArn'] != target_instance['containerInstanceArn']
    )
    
    # If tasks can fit elsewhere, drain the instance
    if total_cpu_needed <= available_capacity:
        ecs.update_container_instances_state(
            cluster=cluster,
            containerInstances=[target_instance['containerInstanceArn']],
            status='DRAINING'
        )
        print(f"Draining instance {target_instance['ec2InstanceId']}")
    else:
        print("Cannot drain instance - insufficient capacity elsewhere")
```

Run this Lambda on a schedule (e.g., every 10 minutes), and it gradually consolidates tasks onto fewer instances by draining underutilized ones. Auto Scaling then terminates the drained instances, reducing your cluster size and improving overall utilization.​

**The downsides**:

* Custom code to maintain
    
* Requires careful tuning to avoid draining too aggressively
    
* Can cause temporary capacity crunches if it drains an instance while you're deploying
    
* Doesn't solve the AZ imbalance problem—it optimizes for cost, not high availability
    

## Workaround 3: Zonal Capacity Providers

A more sophisticated approach, documented on Containers on AWS, involves creating separate capacity providers for each AZ:​

```yaml
# Capacity provider for us-east-1a
resource "aws_ecs_capacity_provider" "us_east_1a" {
  name = "capacity-provider-us-east-1a"
  
  auto_scaling_group_provider {
    auto_scaling_group_arn = aws_autoscaling_group.us_east_1a.arn
    # ... managed scaling configuration
  }
}

# Capacity provider for us-east-1b
resource "aws_ecs_capacity_provider" "us_east_1b" {
  name = "capacity-provider-us-east-1b"
  
  auto_scaling_group_provider {
    auto_scaling_group_arn = aws_autoscaling_group.us_east_1b.arn
  }
}

# Capacity provider for us-east-1c
resource "aws_ecs_capacity_provider" "us_east_1c" {
  name = "capacity-provider-us-east-1c"
  
  auto_scaling_group_provider {
    auto_scaling_group_arn = aws_autoscaling_group.us_east_1c.arn
  }
}

# Service with capacity provider strategy
resource "aws_ecs_service" "app" {
  name    = "my-app"
  cluster = aws_ecs_cluster.main.id
  # ...
  
  capacity_provider_strategy {
    capacity_provider = aws_ecs_capacity_provider.us_east_1a.name
    weight            = 1
    base              = 0
  }
  
  capacity_provider_strategy {
    capacity_provider = aws_ecs_capacity_provider.us_east_1b.name
    weight            = 1
    base              = 0
  }
  
  capacity_provider_strategy {
    capacity_provider = aws_ecs_capacity_provider.us_east_1c.name
    weight            = 1
    base              = 0
  }
}
```

With this setup, ECS distributes tasks evenly across the three capacity providers, ensuring even AZ distribution. When scaling up, each capacity provider adds instances to its AZ, maintaining balance.​

**The trade-off**: You deliberately over-provision capacity. Each AZ needs spare capacity to accommodate rebalancing, which means running more instances than strictly necessary. As the documentation notes: "This approach will deliberately waste EC2 capacity in order to evenly distribute tasks across availability zones. This capacity provider strategy is not optimized for cost. It is optimized for high availability".​

For a 30-instance cluster, you might need to run 36-40 instances (20-30% overhead) to ensure smooth operation. That's thousands of dollars per year in wasted compute.

## The Community's Patience Wears Thin

As the years dragged on, comments became increasingly pointed:

* June 2016: "Please prioritize this issue. Thanks."​
    
* November 2017: "Anyone working on this? It's currently the second highest rated FR. I'm hesitant to even use ECS due to this."​
    

In August 2021, AWS finally responded. An ECS team member commented: "Thanks for the feedback. We are looking into this and have it on our roadmap. In interim, we will look into providing a workaround".​

In October 2024—nine years after the issue was opened—the label changed from "Under consideration" to "Work in Progress". In November 2024, AZ rebalancing finally shipped.

## The Solution: Availability Zone Rebalancing

Let's look at how AWS actually solved this and what you need to do to use it.

## How AZ Rebalancing Works

When enabled, ECS continuously monitors task distribution across Availability Zones. The algorithm is straightforward:​

1. **Calculate target distribution**: Divide the service's desired task count by the number of AZs
    
2. **Detect imbalance**: Compare actual distribution to target distribution
    
3. **Rebalance if needed**: Launch new tasks in under-scaled AZs, then terminate tasks in over-scaled AZs​
    

The rebalancing process prioritizes safety:​

* New tasks are launched first
    
* ECS waits for new tasks to reach `RUNNING` and `HEALTHY` status
    
* Only then does ECS terminate tasks in the over-scaled AZ
    
* This ensures you never drop below your desired task count during rebalancing
    

## Target Distribution Examples

The distribution algorithm handles non-divisible task counts intelligently:​

**Example 1: Even distribution**

* Desired count: 6 tasks
    
* AZs: 2 (us-east-1a, us-east-1b)
    
* Target: 3 tasks per AZ
    

**Example 2: Uneven distribution with remainder**

* Desired count: 7 tasks
    
* AZs: 2 (us-east-1a, us-east-1b)
    
* Target: 3 tasks in us-east-1a, 4 tasks in us-east-1b
    
* Each AZ gets at least ⌊7/2⌋ = 3 tasks, remainder distributed to one AZ​
    

**Example 3: Multiple remainders**

* Desired count: 11 tasks
    
* AZs: 3 (us-east-1a, us-east-1b, us-east-1c)
    
* Target: 3 tasks in one AZ, 4 tasks in each of the other two AZs
    
* Each AZ gets ⌊11/3⌋ = 3 tasks, two remainder tasks distributed evenly​
    

## When Rebalancing Triggers

Rebalancing activates when ECS detects an imbalance. Common triggers:​

* **AZ disruption recovery**: An AZ that was unavailable comes back online, but has fewer tasks than target distribution
    
* **Manual intervention**: You manually terminated tasks in one AZ for testing or maintenance
    
* **Capacity provider changes**: New instances were added to one AZ but not others
    
* **Failed placements**: Tasks couldn't be placed in the target AZ initially, so they landed in a different AZ
    

ECS monitors services continuously after they reach steady state, so rebalancing happens automatically without manual intervention.​

## Configuration

For new services created after September 5, 2025, AZ rebalancing is **enabled by default**. For existing services, you need to explicitly enable it.​

**AWS CLI:**

```bash
# Enable for new service
aws ecs create-service \
  --cluster my-cluster \
  --service-name my-service \
  --task-definition my-task:1 \
  --desired-count 12 \
  --availability-zone-rebalancing ENABLED

# Enable for existing service
aws ecs update-service \
  --cluster my-cluster \
  --service my-service \
  --availability-zone-rebalancing ENABLED
```

**Terraform:**

```yaml
resource "aws_ecs_service" "app" {
  name                           = "my-service"
  cluster                        = aws_ecs_cluster.main.id
  task_definition                = aws_ecs_task_definition.app.arn
  desired_count                  = 12
  availability_zone_rebalancing  = "ENABLED"
  
  # Placement strategy is optional
  # If not specified, ECS defaults to AZ spread
  placement_strategy {
    type  = "spread"
    field = "attribute:ecs.availability-zone"
  }
  
  placement_strategy {
    type  = "binpack"
    field = "memory"
  }
  
  # Rest of service configuration...
}
```

**CloudFormation:**

```plaintext
MyECSService:
  Type: AWS::ECS::Service
  Properties:
    ServiceName: my-service
    Cluster: !Ref MyCluster
    TaskDefinition: !Ref MyTaskDefinition
    DesiredCount: 12
    AvailabilityZoneRebalancing: ENABLED
    PlacementStrategies:
      - Type: spread
        Field: attribute:ecs.availability-zone
      - Type: binpack
        Field: memory
```

That's it. No Lambda functions, no custom draining logic, no zonal capacity providers (unless you want them for other reasons). ECS handles the rebalancing automatically.​

## Eligibility Requirements

Not all services can use AZ rebalancing. Your service must meet these criteria:​

✅ **Compatible:**

* Uses `REPLICA` scheduling strategy (default)
    
* Uses `FARGATE`, `FARGATE_SPOT`, or EC2 capacity providers
    
* Uses Application Load Balancer, Network Load Balancer, or no load balancer
    
* Has `maximumPercent` deployment configuration &gt; 100% (need headroom to launch new tasks)
    
* Specifies AZ spread as first placement strategy, or has no placement strategy
    

❌ **Incompatible:**

* Uses `DAEMON` scheduling strategy
    
* Uses `EXTERNAL` launch type (ECS Anywhere)
    
* Has `maximumPercent` deployment configuration = 100% (no room to launch additional tasks)
    
* Uses Classic Load Balancer
    
* Has `attribute:ecs.availability-zone` as a placement **constraint** (constraints are different from strategies)
    

The `maximumPercent` requirement is important. If your service has `maximumPercent = 100%`, ECS can't launch new tasks without first terminating old ones, which would temporarily reduce capacity during rebalancing. Set it to at least 150% (or higher) to allow rebalancing:​

```plaintext
deployment_configuration {
  maximum_percent         = 200
  minimum_healthy_percent = 100
}
```

## Placement Strategy Compatibility

AZ rebalancing works with placement strategies, but **AZ spread must be the first strategy in the list**. This ensures rebalancing takes priority over other placement considerations.​

**Compatible combinations:**

```json
// AZ spread only
"placementStrategy": [
  {
    "field": "attribute:ecs.availability-zone",
    "type": "spread"
  }
]

// AZ spread + binpack on memory
"placementStrategy": [
  {
    "field": "attribute:ecs.availability-zone",
    "type": "spread"
  },
  {
    "field": "memory",
    "type": "binpack"
  }
]

// AZ spread + binpack on CPU
"placementStrategy": [
  {
    "field": "attribute:ecs.availability-zone",
    "type": "spread"
  },
  {
    "field": "cpu",
    "type": "binpack"
  }
]
```

**Incompatible combinations:**

```json
// Binpack first, then spread - rebalancing won't work
"placementStrategy": [
  {
    "field": "memory",
    "type": "binpack"
  },
  {
    "field": "attribute:ecs.availability-zone",
    "type": "spread"
  }
]

// Spread by instance ID - rebalancing won't work
"placementStrategy": [
  {
    "field": "instanceId",
    "type": "spread"
  }
]
```

If you don't specify any placement strategy, ECS defaults to AZ spread, so rebalancing works automatically.​

## Real-World Example: Production API Service

Let me show you a complete example of migrating a production service to use AZ rebalancing.

## Before: Manual Rebalancing with Lambda

Here's what we had before AZ rebalancing was available:

```plaintext
# ECS Service
resource "aws_ecs_service" "api" {
  name            = "api-service"
  cluster         = aws_ecs_cluster.main.id
  task_definition = aws_ecs_task_definition.api.arn
  desired_count   = 30
  launch_type     = "EC2"
  
  placement_strategy {
    type  = "spread"
    field = "attribute:ecs.availability-zone"
  }
  
  placement_strategy {
    type  = "binpack"
    field = "memory"
  }
  
  deployment_configuration {
    maximum_percent         = 200
    minimum_healthy_percent = 100
  }
  
  load_balancer {
    target_group_arn = aws_lb_target_group.api.arn
    container_name   = "api"
    container_port   = 8080
  }
}

# Lambda function for periodic rebalancing
resource "aws_lambda_function" "rebalancer" {
  filename      = "rebalancer.zip"
  function_name = "ecs-task-rebalancer"
  role          = aws_iam_role.rebalancer.arn
  handler       = "index.handler"
  runtime       = "python3.11"
  timeout       = 300
  
  environment {
    variables = {
      CLUSTER_NAME = aws_ecs_cluster.main.name
      SERVICE_NAME = aws_ecs_service.api.name
    }
  }
}

# EventBridge rule to trigger Lambda every 6 hours
resource "aws_cloudwatch_event_rule" "rebalance_schedule" {
  name                = "ecs-rebalance-schedule"
  schedule_expression = "rate(6 hours)"
}

resource "aws_cloudwatch_event_target" "rebalancer" {
  rule      = aws_cloudwatch_event_rule.rebalance_schedule.name
  target_id = "rebalancer"
  arn       = aws_lambda_function.rebalancer.arn
}
```

The Lambda function forced redeployments every 6 hours to redistribute tasks. This worked but caused unnecessary container restarts and required careful orchestration to avoid conflicts with actual application deployments.​

## After: Native AZ Rebalancing

With AZ rebalancing, the configuration simplifies dramatically:

```plaintext
resource "aws_ecs_service" "api" {
  name            = "api-service"
  cluster         = aws_ecs_cluster.main.id
  task_definition = aws_ecs_task_definition.api.arn
  desired_count   = 30
  launch_type     = "EC2"
  
  # Enable native AZ rebalancing
  availability_zone_rebalancing = "ENABLED"
  
  placement_strategy {
    type  = "spread"
    field = "attribute:ecs.availability-zone"
  }
  
  placement_strategy {
    type  = "binpack"
    field = "memory"
  }
  
  deployment_configuration {
    maximum_percent         = 200
    minimum_healthy_percent = 100
  }
  
  load_balancer {
    target_group_arn = aws_lb_target_group.api.arn
    container_name   = "api"
    container_port   = 8080
  }
}
```

We deleted:

* The Lambda function (75 lines of Python)
    
* IAM roles and policies for the Lambda
    
* EventBridge rule and target
    
* CloudWatch Logs log group for the Lambda
    
* Monitoring and alerting for the Lambda function
    

**Total reduction**: ~150 lines of Terraform, one fewer AWS service to manage, simpler operational model.

## Monitoring Rebalancing Operations

ECS publishes service events when rebalancing occurs:​

```bash
# Watch for rebalancing events
aws ecs describe-services \
  --cluster my-cluster \
  --services my-service \
  --query 'services[0].events[?contains(message, `rebalancing`)]' \
  --output table
```

Example events you'll see:

```plaintext
SERVICE_REBALANCING_STARTED: (service my-service) has started rebalancing tasks across Availability Zones.

SERVICE_REBALANCING_IN_PROGRESS: (service my-service) has successfully started 2 tasks in us-east-1c.

SERVICE_REBALANCING_COMPLETED: (service my-service) has completed rebalancing. Tasks are now evenly distributed across Availability Zones.

SERVICE_REBALANCING_STOPPED: (service my-service) rebalancing stopped due to concurrent deployment.
```

You can set up CloudWatch alarms to notify you when rebalancing occurs:

```yaml
resource "aws_cloudwatch_log_metric_filter" "rebalancing_events" {
  name           = "ecs-rebalancing-events"
  log_group_name = "/aws/ecs/cluster/my-cluster"
  pattern        = "SERVICE_REBALANCING"
  
  metric_transformation {
    name      = "ECSRebalancingCount"
    namespace = "ECS/Rebalancing"
    value     = "1"
  }
}

resource "aws_cloudwatch_metric_alarm" "frequent_rebalancing" {
  alarm_name          = "ecs-frequent-rebalancing"
  comparison_operator = "GreaterThanThreshold"
  evaluation_periods  = 1
  metric_name         = "ECSRebalancingCount"
  namespace           = "ECS/Rebalancing"
  period              = 3600
  statistic           = "Sum"
  threshold           = 5
  alarm_description   = "ECS is rebalancing more than 5 times per hour"
}
```

Frequent rebalancing might indicate:

* Unstable AZ availability
    
* Capacity constraints causing placement failures
    
* Incorrectly configured placement strategies or constraints​
    

## Fargate vs. EC2: Different Behaviors

AZ rebalancing works differently depending on your capacity provider.​

## Fargate: Full Rebalancing

With Fargate, ECS has complete control over task placement. When rebalancing triggers:​

1. ECS launches new tasks in the under-scaled AZ(s)
    
2. Fargate provisions the necessary compute capacity automatically
    
3. Once new tasks are healthy, ECS terminates tasks in over-scaled AZ(s)
    
4. Balance is restored
    

No infrastructure management needed. Fargate ensures capacity is available in every AZ (subject to service quotas and regional capacity).​

## EC2: Best-Effort Rebalancing

With EC2 capacity providers, rebalancing is **best-effort**. ECS can only place tasks on existing container instances—it can't launch new instances as part of rebalancing.​

**Scenario 1: Sufficient existing capacity**

* You have container instances with available capacity in all AZs
    
* Rebalancing works just like Fargate—ECS launches tasks where needed
    

**Scenario 2: Insufficient capacity in target AZ**

* You have instances in us-east-1a and us-east-1b, but none in us-east-1c
    
* Tasks are unevenly distributed (8 in 1a, 8 in 1b, 0 in 1c)
    
* **Problem**: ECS can't launch tasks in us-east-1c because there are no instances there
    
* **Result**: Tasks remain imbalanced until you manually add instances to us-east-1c​
    

This is a significant limitation. If you're using EC2 with Auto Scaling Groups, you need to ensure your ASGs are configured to maintain instances in all AZs:​

```plaintext
resource "aws_autoscaling_group" "ecs" {
  name                = "ecs-asg"
  vpc_zone_identifier = var.private_subnet_ids  # Subnets in all AZs
  min_size            = 3  # At least one per AZ
  max_size            = 30
  desired_capacity    = 9  # Distributed across AZs
  
  # Use AZ balancing in ASG to ensure instances in all AZs
  health_check_type = "ELB"
  
  launch_template {
    id      = aws_launch_template.ecs.id
    version = "$Latest"
  }
  
  tag {
    key                 = "AmazonECSManaged"
    value               = true
    propagate_at_launch = true
  }
}
```

Even better, use ECS Managed Instances (launched September 2025), which automatically handle multi-AZ distribution for you.​

## Edge Cases and Troubleshooting

Let me share some non-obvious scenarios I've encountered.

## Issue 1: Rebalancing Doesn't Start

**Symptom**: Service remains imbalanced even with `availability_zone_rebalancing = "ENABLED"`.

**Debugging checklist**:​

```bash
# 1. Verify rebalancing is actually enabled
aws ecs describe-services \
  --cluster my-cluster \
  --services my-service \
  --query 'services[0].availabilityZoneRebalancing'

# 2. Check service has reached steady state
aws ecs describe-services \
  --cluster my-cluster \
  --services my-service \
  --query 'services[0].[deployments, runningCount, desiredCount]'

# 3. Verify placement strategy compatibility
aws ecs describe-services \
  --cluster my-cluster \
  --services my-service \
  --query 'services[0].placementStrategy'

# 4. Check maximumPercent allows headroom
aws ecs describe-services \
  --cluster my-cluster \
  --services my-service \
  --query 'services[0].deploymentConfiguration.maximumPercent'
```

Common causes:

* Service hasn't reached steady state (deployment in progress)
    
* `maximumPercent = 100%` doesn't allow room for new tasks
    
* Placement strategy doesn't have AZ spread as first strategy
    
* Service uses DAEMON scheduling (incompatible with rebalancing)​
    

## Issue 2: Task Placement Failures During Rebalancing

**Symptom**: Rebalancing starts but stops with `SERVICE_TASK_PLACEMENT_FAILURE` events.

**For EC2**:​

* Check container instance availability in the target AZ:
    

```bash
aws ecs describe-container-instances \
  --cluster my-cluster \
  --container-instances $(aws ecs list-container-instances --cluster my-cluster --query 'containerInstanceArns' --output text) \
  --query 'containerInstances[*].[containerInstanceArn, ec2InstanceId, registeredResources[?name==`CPU`].integerValue | [0], remainingResources[?name==`CPU`].integerValue | [0], attributes[?name==`ecs.availability-zone`].value | [0]]' \
  --output table
```

This shows CPU capacity per instance and which AZ each instance is in. If the target AZ has no instances or insufficient capacity, rebalancing fails.​

**For Fargate**:

* Check service quotas:
    

```bash
aws service-quotas get-service-quota \
  --service-code fargate \
  --quota-code L-3032A538  # Fargate On-Demand vCPU quota
```

* Check for capacity constraints in the region/AZ (rare but possible during major outages)
    

## Issue 3: Rebalancing Stops Unexpectedly

**Symptom**: `SERVICE_REBALANCING_STOPPED` events appear before rebalancing completes.

Common causes:​

* **Task protection**: Tasks in the over-scaled AZ have task protection enabled, preventing termination
    
* **Concurrent deployment**: You triggered a service update while rebalancing was in progress
    
* **Service scaled down**: You reduced `desired_count` during rebalancing
    

Check for task protection:

```bash
aws ecs get-task-protection \
  --cluster my-cluster \
  --tasks $(aws ecs list-tasks --cluster my-cluster --service-name my-service --query 'taskArns' --output text)
```

If tasks are protected, rebalancing can't terminate them. You need to either:

* Disable task protection temporarily
    
* Wait for the protection to expire
    
* Manually stop protected tasks after verifying they're safe to terminate​
    

## Issue 4: Rebalancing Too Frequently

**Symptom**: Rebalancing occurs multiple times per hour, causing unnecessary churn.

This usually indicates an underlying issue:

* **Flapping tasks**: Tasks are failing health checks shortly after launch, triggering replacements in different AZs
    
* **Capacity constraints**: Insufficient capacity in one AZ causes tasks to bounce between AZs
    
* **Misconfigured health checks**: Health checks are too aggressive, marking healthy tasks as unhealthy
    

Check task health trends:

```bash
aws ecs describe-services \
  --cluster my-cluster \
  --services my-service \
  --query 'services[0].events[:20]' \
  --output table
```

Look for patterns of tasks starting and stopping rapidly. Then investigate:

```bash
# Get stopped task details
aws ecs describe-tasks \
  --cluster my-cluster \
  --tasks $(aws ecs list-tasks --cluster my-cluster --service-name my-service --desired-status STOPPED --query 'taskArns[:10]' --output text) \
  --query 'tasks[*].[taskArn, stopCode, stoppedReason, containers[0].reason]' \
  --output table
```

Common stopCodes and what they mean:

* `TaskFailedToStart`: Container couldn't start (image pull failure, invalid configuration)
    
* `EssentialContainerExited`: Main container exited (application crash)
    
* `ServiceSchedulerInitiated`: ECS stopped the task (deployment, rebalancing, or service update)
    

## What AZ Rebalancing Doesn't Solve

Let's be clear about the limitations. AZ rebalancing is not a universal solution to all ECS resource management problems.

## Still No Within-AZ Rebalancing

AZ rebalancing only handles distribution **across** Availability Zones. It doesn't rebalance tasks **within** an AZ to better utilize instances.​

Scenario:

* AZ us-east-1a has 3 instances: Instance A, B, C
    
* 10 tasks are running in us-east-1a
    
* Distribution: Instance A has 7 tasks, Instance B has 2 tasks, Instance C has 1 task
    
* **Problem**: Poor resource utilization on instances B and C
    

AZ rebalancing won't fix this. The total number of tasks in us-east-1a is correct—only the within-AZ distribution is suboptimal.​

The original GitHub issue #105 requested this broader rebalancing capability. What AWS shipped solves the **high availability** problem (even AZ distribution) but not the **resource efficiency** problem (optimal within-AZ task placement).​

For within-AZ optimization, you still need workarounds:

* Periodic forced redeployments
    
* Custom draining Lambda (like @lobsterdore's solution)
    
* Over-provisioning to ensure adequate fragmented capacity
    

## No Cross-Service Optimization

AZ rebalancing operates per-service. It doesn't consider the cluster-wide resource utilization.​

Scenario:

* Cluster with 30 instances across 3 AZs
    
* Service A: 20 tasks, evenly balanced across AZs (rebalancing working great)
    
* Service B: 10 tasks, evenly balanced across AZs (rebalancing working great)
    
* **Problem**: Service A tasks and Service B tasks might cluster on the same instances, leaving some instances underutilized
    

Each service individually meets its AZ distribution target, but the cluster as a whole might still be poorly balanced.

This is a harder problem to solve—it requires cluster-level task bin-packing optimization, which ECS doesn't provide. Kubernetes has similar limitations; even sophisticated schedulers like the Kubernetes default scheduler make placement decisions per-pod, not holistically across all workloads.​

## Still Requires Proper Capacity Planning

For EC2-based services, AZ rebalancing can't create capacity out of thin air. If your ASG only spans two AZs, rebalancing can't magically place tasks in a third AZ.​

You still need to:

* Configure ASGs with subnets in all AZs you want to use
    
* Maintain minimum instance counts to ensure capacity in each AZ
    
* Monitor capacity provider metrics to avoid resource exhaustion
    

This is less of an issue with Fargate or ECS Managed Instances, where AWS handles capacity provisioning.​

## Best Practices

Based on production experience with AZ rebalancing, here are patterns that work:

**1\. Enable AZ rebalancing for all multi-AZ services**

Unless you have a specific reason not to, enable it:​

```plaintext
resource "aws_ecs_service" "app" {
  availability_zone_rebalancing = "ENABLED"
  # ... rest of config
}
```

The operational benefits (automatic recovery from AZ disruptions, improved availability) far outweigh any concerns about occasional task churn.

**2\. Set appropriate maximumPercent**

Allow enough headroom for rebalancing:​

```plaintext
deployment_configuration {
  maximum_percent         = 200  # Can double task count temporarily
  minimum_healthy_percent = 100  # Never drop below desired count
}
```

If you're cost-constrained and can tolerate brief capacity reductions:

```plaintext
deployment_configuration {
  maximum_percent         = 150  # Only 50% extra during rebalancing
  minimum_healthy_percent = 75   # Can drop to 75% temporarily
}
```

**3\. Use AZ spread as first placement strategy**

Even if you want binpack for efficiency, put AZ spread first:​

```plaintext
placement_strategy {
  type  = "spread"
  field = "attribute:ecs.availability-zone"
}

placement_strategy {
  type  = "binpack"
  field = "memory"
}
```

This ensures rebalancing works while still optimizing resource utilization within each AZ.

**4\. Monitor rebalancing frequency**

If rebalancing happens more than a few times per day, investigate. Set up CloudWatch alarms (as shown earlier) to detect excessive rebalancing.​

**5\. For EC2, maintain instances in all AZs**

Configure ASGs to ensure at least one instance per AZ:​

```plaintext
resource "aws_autoscaling_group" "ecs" {
  name                = "ecs-asg"
  vpc_zone_identifier = [
    var.subnet_us_east_1a,
    var.subnet_us_east_1b,
    var.subnet_us_east_1c
  ]
  min_size         = 3  # Minimum one per AZ
  desired_capacity = 12 # Will distribute across AZs
  
  # ...
}
```

Better yet, use zonal ASGs (one per AZ) with separate capacity providers to guarantee instance availability in each AZ.​

**6\. Test rebalancing in staging**

Before enabling in production, test in a staging environment:​

```bash
# Enable rebalancing
aws ecs update-service \
  --cluster staging \
  --service my-service \
  --availability-zone-rebalancing ENABLED

# Simulate imbalance by draining instances in one AZ
aws ecs update-container-instances-state \
  --cluster staging \
  --container-instances <instance-arn-in-az-a> \
  --status DRAINING

# Watch for rebalancing events
watch -n 5 'aws ecs describe-services --cluster staging --services my-service --query "services[0].events[:5]" --output table'
```

Verify that rebalancing completes successfully and doesn't disrupt your application.

## Migration Path for Existing Services

If you have existing ECS services, here's how to migrate to AZ rebalancing safely:

**Step 1: Audit current services**

Identify services that could benefit:

```bash
# List all services
aws ecs list-services \
  --cluster my-cluster \
  --query 'serviceArns' \
  --output text | \
  xargs -I {} aws ecs describe-services \
    --cluster my-cluster \
    --services {} \
    --query 'services[0].[serviceName, desiredCount, runningCount, placementStrategy, availabilityZoneRebalancing]' \
    --output table
```

Look for services with:

* Multiple tasks (`desiredCount` &gt; 3)
    
* AZ spread placement strategy
    
* `availabilityZoneRebalancing` = `DISABLED` or null
    

**Step 2: Update Terraform/IaC**

Add the rebalancing parameter to your service definitions:

```yaml
resource "aws_ecs_service" "app" {
  # ... existing config ...
  availability_zone_rebalancing = "ENABLED"
}
```

**Step 3: Plan and review**

Run Terraform plan to see what will change:

```bash
terraform plan
```

You should see only the `availability_zone_rebalancing` parameter changing. If you see other changes (like task definitions or placement strategies), review them carefully—those might trigger a deployment.

**Step 4: Apply during low-traffic period**

Enable rebalancing during a maintenance window or low-traffic period:

```bash
terraform apply
```

Or via AWS CLI:

```bash
aws ecs update-service \
  --cluster my-cluster \
  --service my-service \
  --availability-zone-rebalancing ENABLED
```

This change itself doesn't trigger rebalancing—it just enables the feature. Rebalancing only happens when ECS detects an imbalance.​

**Step 5: Monitor**

Watch for rebalancing events over the next few days:

```bash
aws ecs describe-services \
  --cluster my-cluster \
  --services my-service \
  --query 'services[0].events[?contains(message, `rebalancing`)]'
```

If you see frequent rebalancing or failures, investigate using the troubleshooting steps above.

**Step 6: Clean up old workarounds**

Once you've confirmed rebalancing is working, remove any custom rebalancing infrastructure:

* Lambda functions that force redeployments
    
* EventBridge rules triggering rebalancing
    
* Custom monitoring for task distribution
    

This reduces your operational surface area and eliminates custom code to maintain.

## Wrapping Up

After nine years, AWS finally solved one of ECS's most frustrating limitations. Availability Zone rebalancing eliminates entire categories of operational problems—AZ disruption recovery, uneven task distribution, wasted capacity from poor placement.

The feature is straightforward to use: add `availability_zone_rebalancing = "ENABLED"` to your service definition, ensure your placement strategies are compatible, and ECS handles the rest. No Lambda functions, no custom scripts, no forced redeployments. The platform does what it should have done from the beginning.​

That said, this isn't a universal solution to all ECS resource management problems. It solves **cross-AZ** distribution for high availability, but not **within-AZ** optimization for resource efficiency. You might still over-provision capacity or need workarounds for perfect bin-packing. But the biggest pain point—manual recovery after AZ disruptions and uneven distribution—is finally handled automatically.

For new services, as of September 2025, you get this behavior by default. For existing services, enable it explicitly and watch a long-standing operational burden disappear. And if you maintained custom rebalancing infrastructure over the years (like I did), you can finally delete it. The platform has caught up.