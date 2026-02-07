---
title: "ECS Linear and Canary Deployments: Finally, Native Traffic Shifting Without the Glue Code"
datePublished: Tue Jan 06 2026 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: cmlcg1by9000102la3kofewda
slug: ecs-linear-and-canary-deployments-finally-native-traffic-shifting-without-the-glue-code
tags: aws

---

I've been deploying services on ECS for years, and one problem has consistently frustrated me: rolling out new versions safely. Sure, ECS had blue/green deployments, but they were all-or-nothing—100% of traffic instantly switches from the old version to the new one. That's terrifying for any production service with real users. If your new version has a subtle bug that only manifests under production load, congrats—every single user just hit it simultaneously.

The workaround? Build your own traffic shifting system. I've seen teams cobble together Step Functions that gradually adjust ALB target group weights, implement custom monitoring to detect failures, and manually trigger rollbacks when things go wrong. It works, but it's a lot of undifferentiated heavy lifting. You're essentially rebuilding progressive delivery infrastructure that should be a platform feature.

In October 2025, AWS finally shipped what the community had been requesting for years (GitHub issue #2651): native Linear and Canary deployment strategies for ECS. No more custom Step Functions. No more manually managing ALB target group weights. The platform now handles gradual traffic shifting, bake times for validation, CloudWatch alarm integration for automatic rollback, and deployment lifecycle hooks for custom validation.

This isn't just a nice-to-have—it fundamentally changes how you think about ECS deployments. Let me show you why this matters and how to actually use it.

## The Problem: All-or-Nothing Deployments Are Risky

Before we dive into the solution, let's be clear about what made traditional ECS blue/green deployments problematic.

## What Traditional Blue/Green Looked Like

With classic ECS blue/green deployments (pre-October 2025), the deployment process was straightforward but nerve-wracking:

1. ECS spins up new tasks with your updated container image (the "green" environment)
    
2. Once the green tasks pass health checks, ECS updates the ALB target group
    
3. **All production traffic instantly switches to the green environment**
    
4. After a configurable wait period, the old blue tasks are terminated
    

The critical issue is step 3. From the user's perspective, every request suddenly starts hitting the new version at the exact same moment. If there's a problem—a memory leak, a database query that's slow under production load, a race condition that only manifests with high concurrency—100% of your users experience it immediately.

## The Real-World Scenario

Here's a scenario I've personally experienced: You deploy a service update that passes all pre-production testing. Health checks are green. Everything looks good. Traffic switches over. Within 60 seconds, error rates spike from 0.1% to 15% because a third-party API timeout you configured works fine in staging (low traffic) but causes cascading failures in production (high traffic).

By the time you realize there's a problem, check dashboards, confirm it's not a false alarm, and initiate a rollback, you've had 3-5 minutes of degraded user experience affecting everyone. Your SLO is toast for the month.

## The Workaround: Custom Traffic Shifting

The common workaround was to build custom deployment orchestration. One team I worked with had a Step Function that looked like this:

```python
# Pseudo-code for custom traffic shifting
def deploy_with_gradual_traffic_shift():
    # Deploy green environment
    new_target_group = create_target_group_for_green()
    launch_ecs_tasks(new_target_group)
    wait_for_health_checks()
    
    # Gradually shift traffic
    traffic_weights = [(10, 90), (25, 75), (50, 50), (75, 25), (100, 0)]
    
    for green_weight, blue_weight in traffic_weights:
        # Update ALB target group weights
        update_alb_weights(
            green_target_group=new_target_group,
            green_weight=green_weight,
            blue_target_group=old_target_group,
            blue_weight=blue_weight
        )
        
        # Wait and monitor
        sleep(300)  # 5 minute bake time
        
        # Check CloudWatch alarms
        if any_alarms_triggered():
            rollback_to_blue()
            raise DeploymentFailed("Alarms triggered during traffic shift")
    
    # Complete deployment
    terminate_blue_tasks()
```

This worked, but you're responsible for:

* Implementing the Step Function state machine
    
* Managing ALB target group weights via API calls
    
* Polling CloudWatch alarms and interpreting results
    
* Handling rollback logic and edge cases
    
* Maintaining this code as AWS APIs evolve
    

It's hundreds of lines of code that every team running ECS at scale ends up writing. And if you're using ECS Service Connect instead of ALB, you need a completely different implementation.

## The Solution: Native Linear and Canary Deployments

AWS finally made gradual traffic shifting a first-class ECS feature. Let's break down what's actually new and how it works.

## Two Deployment Strategies

ECS now supports two progressive delivery patterns:

**Linear Deployments**: Traffic shifts in equal percentage increments at regular intervals. You configure a step percentage (e.g., 10%) and a step bake time (e.g., 5 minutes). ECS automatically shifts 10% of traffic every 5 minutes until 100% is on the new version.

Example: 0% → 10% → 20% → 30% → 40% → 50% → 60% → 70% → 80% → 90% → 100%

**Canary Deployments**: A small percentage of traffic (typically 5-10%) goes to the new version for an evaluation period. If everything looks good, ECS shifts the remaining 90-95% all at once.

Example: 0% → 10% (bake for 15 minutes) → 100%

Both strategies support:

* **Deployment bake time**: After reaching 100%, ECS waits before terminating old tasks, allowing quick rollback if issues appear
    
* **CloudWatch alarm integration**: Automatic rollback if configured alarms trigger
    
* **Deployment lifecycle hooks**: Lambda functions at specific deployment stages for custom validation
    

## Which Strategy to Choose

**Use Linear deployments when:**

* You want multiple validation checkpoints during rollout
    
* Your monitoring needs time to detect subtle issues
    
* You're deploying high-risk changes to critical services
    
* You have sufficient traffic volume to validate at each increment
    

**Use Canary deployments when:**

* You want faster rollouts with one validation checkpoint
    
* You can detect issues quickly with a small traffic percentage
    
* You're confident in your monitoring and health checks
    
* You want to minimize resource usage (less time running both versions)​
    

For most production services, I default to Linear with 10% increments and 5-minute bake times. It's a good balance of safety and deployment speed. Canary makes sense for services with extremely high traffic where even 10% provides massive request volume for validation.

## Implementing Linear Deployments

Let's walk through a real implementation. I'll show you the actual Terraform and AWS CLI configurations I use.

## Prerequisites

Before you configure Linear deployments, you need:

1. **Application Load Balancer** with two target groups (blue and green), or **ECS Service Connect** enabled
    
2. **CloudWatch alarms** for your service metrics (error rate, latency, etc.)
    
3. **Appropriate IAM permissions** for ECS to manage ALB and invoke lifecycle hooks
    

## Terraform Configuration

Here's a complete Linear deployment configuration:

```python
resource "aws_ecs_service" "api" {
  name            = "api-service"
  cluster         = aws_ecs_cluster.main.id
  task_definition = aws_ecs_task_definition.api.arn
  desired_count   = 10
  
  # Linear deployment configuration
  deployment_configuration {
    deployment_type = "LINEAR"
    
    linear_configuration {
      step_percentage         = 10.0
      step_bake_time_in_minutes = 5
    }
    
    bake_time_in_minutes = 10
    
    # Automatic rollback on alarm
    deployment_circuit_breaker {
      enable   = true
      rollback = true
    }
    
    # CloudWatch alarms that trigger rollback
    alarms {
      alarm_names = [
        aws_cloudwatch_metric_alarm.api_error_rate.alarm_name,
        aws_cloudwatch_metric_alarm.api_latency_p99.alarm_name
      ]
      enable   = true
      rollback = true
    }
  }
  
  load_balancer {
    target_group_arn = aws_lb_target_group.api_blue.arn
    container_name   = "api"
    container_port   = 8000
  }
  
  network_configuration {
    subnets          = var.private_subnet_ids
    security_groups  = [aws_security_group.api.id]
    assign_public_ip = false
  }
}
```

This configuration tells ECS:

* Shift traffic in 10% increments
    
* Wait 5 minutes after each increment (step bake time)
    
* After reaching 100%, wait another 10 minutes before terminating old tasks (deployment bake time)
    
* Automatically rollback if the specified CloudWatch alarms trigger
    

## CloudWatch Alarms for Automatic Rollback

The alarms you configure are critical—they're your safety net. Here are the alarms I typically configure:

```python
# Error rate alarm
resource "aws_cloudwatch_metric_alarm" "api_error_rate" {
  alarm_name          = "api-error-rate-high"
  comparison_operator = "GreaterThanThreshold"
  evaluation_periods  = 2
  metric_name         = "5XXError"
  namespace           = "AWS/ApplicationELB"
  period              = 60
  statistic           = "Sum"
  threshold           = 10
  alarm_description   = "API error rate exceeded threshold"
  treat_missing_data  = "notBreaching"
  
  dimensions = {
    LoadBalancer = aws_lb.main.arn_suffix
    TargetGroup  = aws_lb_target_group.api_green.arn_suffix
  }
}

# Latency alarm
resource "aws_cloudwatch_metric_alarm" "api_latency_p99" {
  alarm_name          = "api-latency-p99-high"
  comparison_operator = "GreaterThanThreshold"
  evaluation_periods  = 2
  metric_name         = "TargetResponseTime"
  namespace           = "AWS/ApplicationELB"
  period              = 60
  statistic           = "p99"
  threshold           = 2.0  # 2 seconds
  alarm_description   = "API p99 latency exceeded threshold"
  treat_missing_data  = "notBreaching"
  
  dimensions = {
    LoadBalancer = aws_lb.main.arn_suffix
    TargetGroup  = aws_lb_target_group.api_green.arn_suffix
  }
}

# Task health alarm
resource "aws_cloudwatch_metric_alarm" "api_unhealthy_tasks" {
  alarm_name          = "api-unhealthy-tasks"
  comparison_operator = "GreaterThanThreshold"
  evaluation_periods  = 1
  metric_name         = "UnHealthyHostCount"
  namespace           = "AWS/ApplicationELB"
  period              = 60
  statistic           = "Average"
  threshold           = 0
  alarm_description   = "Unhealthy tasks detected in target group"
  treat_missing_data  = "notBreaching"
  
  dimensions = {
    LoadBalancer = aws_lb.main.arn_suffix
    TargetGroup  = aws_lb_target_group.api_green.arn_suffix
  }
}
```

**Critical configuration detail**: The alarms must monitor the **green target group** specifically. If you monitor aggregate metrics across both blue and green, the alarm won't accurately reflect the new version's health—it'll be averaged out by the still-healthy blue environment.

I also set `evaluation_periods = 2` for most alarms to avoid false positives from transient spikes. You want high confidence that there's a real problem before triggering an automatic rollback.

## AWS CLI Configuration

If you prefer the AWS CLI, here's the equivalent configuration:

```bash
aws ecs create-service \
  --cluster production \
  --service-name api-service \
  --task-definition api:latest \
  --desired-count 10 \
  --deployment-configuration '{
    "deploymentType": "LINEAR",
    "linearConfiguration": {
      "stepPercentage": 10.0,
      "stepBakeTimeInMinutes": 5
    },
    "bakeTimeInMinutes": 10,
    "deploymentCircuitBreaker": {
      "enable": true,
      "rollback": true
    },
    "alarms": {
      "alarmNames": [
        "api-error-rate-high",
        "api-latency-p99-high",
        "api-unhealthy-tasks"
      ],
      "enable": true,
      "rollback": true
    }
  }' \
  --load-balancers '[{
    "targetGroupArn": "arn:aws:elasticloadbalancing:us-east-1:123456789012:targetgroup/api-blue/abc123",
    "containerName": "api",
    "containerPort": 8000
  }]' \
  --network-configuration '{
    "awsvpcConfiguration": {
      "subnets": ["subnet-abc123", "subnet-def456"],
      "securityGroups": ["sg-abc123"],
      "assignPublicIp": "DISABLED"
    }
  }'
```

## What Happens During a Linear Deployment

Let's trace through what actually happens when you update the service:

**T+0:00** - You update the service with a new task definition

* ECS creates a new "green" task set with the updated image
    
* Green tasks start launching
    
* 100% of traffic still goes to the blue task set
    

**T+2:00** - Green tasks pass health checks

* ECS begins the traffic shift process
    
* 10% of production traffic now routes to green tasks
    
* 90% still goes to blue tasks
    
* Step bake time (5 minutes) begins
    

**T+7:00** - First step bake time completes

* ECS checks configured CloudWatch alarms
    
* If alarms are OK, traffic shifts to 20% green / 80% blue
    
* Another 5-minute step bake time begins
    

**T+12:00, T+17:00, T+22:00...** - Pattern continues

* Every 5 minutes, another 10% shifts to green
    
* Each shift is preceded by alarm checks
    

**T+47:00** - 100% of traffic reaches green

* Deployment bake time (10 minutes) begins
    
* Both blue and green task sets are still running
    
* Quick rollback is still possible by shifting traffic back to blue
    

**T+57:00** - Deployment bake time completes

* Final alarm check
    
* If everything is healthy, ECS scales blue tasks down to zero
    
* Green becomes the new production task set
    
* Deployment is complete
    

If at any point during this process a CloudWatch alarm triggers, ECS immediately:

1. Shifts all traffic back to blue
    
2. Scales green tasks to zero
    
3. Marks the deployment as failed
    
4. Leaves blue running as the production task set
    

Total deployment time in this example: ~57 minutes. That's slower than an all-at-once switch, but you had nine validation checkpoints along the way, each with real production traffic hitting your new version.

## Implementing Canary Deployments

Canary deployments follow a different pattern: validate with a small slice of traffic, then commit fully if it looks good.

## Configuration

```bash
resource "aws_ecs_service" "api_canary" {
  name            = "api-service"
  cluster         = aws_ecs_cluster.main.id
  task_definition = aws_ecs_task_definition.api.arn
  desired_count   = 10
  
  deployment_configuration {
    deployment_type = "CANARY"
    
    canary_configuration {
      canary_percentage         = 10.0
      canary_bake_time_in_minutes = 15
    }
    
    bake_time_in_minutes = 10
    
    deployment_circuit_breaker {
      enable   = true
      rollback = true
    }
    
    alarms {
      alarm_names = [
        aws_cloudwatch_metric_alarm.api_error_rate.alarm_name,
        aws_cloudwatch_metric_alarm.api_latency_p99.alarm_name
      ]
      enable   = true
      rollback = true
    }
  }
  
  load_balancer {
    target_group_arn = aws_lb_target_group.api_blue.arn
    container_name   = "api"
    container_port   = 8000
  }
  
  network_configuration {
    subnets          = var.private_subnet_ids
    security_groups  = [aws_security_group.api.id]
    assign_public_ip = false
  }
}
```

The key differences:

* `canary_percentage`: Percentage of traffic to route to the new version during evaluation (typically 5-10%)
    
* `canary_bake_time_in_minutes`: How long to wait during the canary phase before shifting remaining traffic
    

## Canary Deployment Timeline

Here's what happens with a Canary deployment:

**T+0:00** - Service update triggered

* Green task set launches with new revision
    
* 100% traffic remains on blue
    

**T+2:00** - Green tasks healthy

* 10% of traffic shifts to green (canary phase begins)
    
* 90% remains on blue
    
* Canary bake time (15 minutes) starts
    

**T+17:00** - Canary bake time completes

* ECS checks CloudWatch alarms
    
* If alarms are OK, **remaining 90% shifts to green all at once**
    
* Deployment bake time (10 minutes) begins
    

**T+27:00** - Deployment bake time completes

* Final alarm check
    
* Blue tasks scale to zero
    
* Deployment complete
    

Total time: ~27 minutes. Much faster than Linear, but with only one validation checkpoint at the 10% canary level.

## Canary vs. Linear: When to Use Each

From my experience:

**Canary is better when:**

* You have high traffic volume (10% still means millions of requests)
    
* You can detect issues quickly (comprehensive monitoring, fast alerting)
    
* Deployment speed matters (you deploy frequently)
    
* Resource costs are a concern (less time running both blue and green)
    

**Linear is better when:**

* You have moderate traffic (need multiple increments to validate)
    
* Issues might be subtle and need more exposure to detect
    
* You're deploying risky changes (major refactors, new dependencies)
    
* You want multiple decision points to abort
    

For a high-traffic API serving 10,000 requests per second, a 10% canary means 1,000 requests/second hit the new version—plenty to detect issues. For a lower-traffic service at 10 requests per second, 10% canary is only 1 request per second, which might not expose problems. In that case, Linear with multiple increments gives you better validation.

## Advanced: Deployment Lifecycle Hooks

One of the most powerful features is lifecycle hooks—Lambda functions that run at specific points during deployment. This lets you implement custom validation logic beyond just CloudWatch alarms.

## Use Cases for Lifecycle Hooks

I've used lifecycle hooks for:

* **Synthetic transaction validation**: Run automated tests against the canary environment
    
* **Database migration verification**: Ensure schema changes applied correctly
    
* **External dependency checks**: Verify third-party APIs are reachable and responding
    
* **Custom metric validation**: Check business metrics from internal systems not in CloudWatch
    
* **Notification**: Send Slack/PagerDuty alerts at specific deployment stages
    

## Configuring a Lifecycle Hook

First, create a Lambda function for validation:

```python
import boto3
import requests

ecs = boto3.client('ecs')

def lambda_handler(event, context):
    """
    Lifecycle hook invoked during PRODUCTION_TRAFFIC_SHIFT.
    Validates the green environment health.
    """
    deployment_id = event['deploymentId']
    lifecycle_event_name = event['lifecycleEventName']
    
    print(f"Validating deployment {deployment_id} at stage {lifecycle_event_name}")
    
    # Run synthetic transactions against green environment
    try:
        green_alb_dns = event['greenEnvironmentDns']
        
        # Test critical endpoints
        health_response = requests.get(f"https://{green_alb_dns}/health", timeout=5)
        api_response = requests.get(f"https://{green_alb_dns}/api/v1/test", timeout=5)
        
        if health_response.status_code != 200:
            print(f"Health check failed: {health_response.status_code}")
            return {
                'status': 'Failed',
                'message': 'Health endpoint returned non-200 status'
            }
        
        if api_response.status_code != 200:
            print(f"API check failed: {api_response.status_code}")
            return {
                'status': 'Failed',
                'message': 'API endpoint returned non-200 status'
            }
        
        # Validate response time
        if api_response.elapsed.total_seconds() > 2.0:
            print(f"Response time too high: {api_response.elapsed.total_seconds()}s")
            return {
                'status': 'Failed',
                'message': f'Response time exceeded threshold: {api_response.elapsed.total_seconds()}s'
            }
        
        print("All validation checks passed")
        return {
            'status': 'Succeeded',
            'message': 'All validation checks passed'
        }
        
    except Exception as e:
        print(f"Validation error: {str(e)}")
        return {
            'status': 'Failed',
            'message': f'Validation error: {str(e)}'
        }
```

Then configure it in your ECS service:

```bash
resource "aws_ecs_service" "api_with_hooks" {
  name            = "api-service"
  cluster         = aws_ecs_cluster.main.id
  task_definition = aws_ecs_task_definition.api.arn
  desired_count   = 10
  
  deployment_configuration {
    deployment_type = "LINEAR"
    
    linear_configuration {
      step_percentage         = 10.0
      step_bake_time_in_minutes = 5
    }
    
    bake_time_in_minutes = 10
    
    # Lifecycle hooks for custom validation
    lifecycle_event_hooks {
      lifecycle_event = "PRODUCTION_TRAFFIC_SHIFT"
      lambda_arn      = aws_lambda_function.deployment_validator.arn
    }
    
    lifecycle_event_hooks {
      lifecycle_event = "POST_PRODUCTION_TRAFFIC_SHIFT"
      lambda_arn      = aws_lambda_function.post_deployment_validator.arn
    }
    
    alarms {
      alarm_names = [
        aws_cloudwatch_metric_alarm.api_error_rate.alarm_name,
        aws_cloudwatch_metric_alarm.api_latency_p99.alarm_name
      ]
      enable   = true
      rollback = true
    }
  }
  
  # ... rest of configuration
}
```

Available lifecycle stages for hooks:

* `PRE_SCALE_UP`: Before green environment starts
    
* `POST_SCALE_UP`: After green tasks launch
    
* `TEST_TRAFFIC_SHIFT`: During test traffic shift to green
    
* `POST_TEST_TRAFFIC_SHIFT`: After test traffic shift completes
    
* `PRODUCTION_TRAFFIC_SHIFT`: During each production traffic shift step (invoked at every increment for Linear, once for Canary)
    
* `POST_PRODUCTION_TRAFFIC_SHIFT`: After all production traffic shifts
    
* `BAKE_TIME`: During the final bake time
    

If your Lambda function returns `status: 'Failed'`, ECS automatically triggers a rollback. This gives you programmatic control over deployment success criteria beyond just CloudWatch alarms.

## Real-World Example: Deploying a Node.js API

Let me show a complete real-world example: deploying a Node.js Express API with Linear deployments.

## Application Setup

**Dockerfile:**

```bash
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./
RUN npm ci --only=production

COPY . .

EXPOSE 3000

# Graceful shutdown handling
STOPSIGNAL SIGTERM

CMD ["node", "server.js"]
```

**server.js with health check:**

```javascript
const express = require('express');
const app = express();

let isHealthy = true;

// Health check endpoint
app.get('/health', (req, res) => {
  if (isHealthy) {
    res.status(200).json({ status: 'healthy' });
  } else {
    res.status(503).json({ status: 'unhealthy' });
  }
});

// Main API endpoint
app.get('/api/v1/users', async (req, res) => {
  try {
    // Your business logic here
    res.json({ users: [] });
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});

// Graceful shutdown
process.on('SIGTERM', () => {
  console.log('SIGTERM received, starting graceful shutdown');
  isHealthy = false;
  
  // Give load balancer time to deregister (30 seconds)
  setTimeout(() => {
    server.close(() => {
      console.log('Server closed, exiting');
      process.exit(0);
    });
  }, 30000);
});

const server = app.listen(3000, () => {
  console.log('Server listening on port 3000');
});
```

The critical part is graceful shutdown handling. When ECS sends SIGTERM during a rollback or deployment completion, you want:

1. Health check to immediately return unhealthy (stops new requests)
    
2. Wait for ALB deregistration delay (typically 30 seconds)
    
3. Close server and exit cleanly
    

## Complete Terraform Configuration

```yaml
# Task definition
resource "aws_ecs_task_definition" "api" {
  family                   = "node-api"
  requires_compatibilities = ["FARGATE"]
  network_mode             = "awsvpc"
  cpu                      = "512"
  memory                   = "1024"
  execution_role_arn       = aws_iam_role.ecs_execution.arn
  task_role_arn            = aws_iam_role.ecs_task.arn
  
  container_definitions = jsonencode([{
    name  = "api"
    image = "123456789012.dkr.ecr.us-east-1.amazonaws.com/node-api:${var.image_tag}"
    
    portMappings = [{
      containerPort = 3000
      protocol      = "tcp"
    }]
    
    healthCheck = {
      command     = ["CMD-SHELL", "curl -f http://localhost:3000/health || exit 1"]
      interval    = 30
      timeout     = 5
      retries     = 3
      startPeriod = 60
    }
    
    logConfiguration = {
      logDriver = "awslogs"
      options = {
        "awslogs-group"         = "/ecs/node-api"
        "awslogs-region"        = "us-east-1"
        "awslogs-stream-prefix" = "api"
      }
    }
    
    environment = [
      { name = "NODE_ENV", value = "production" }
    ]
  }])
}

# Blue target group
resource "aws_lb_target_group" "api_blue" {
  name        = "api-blue-tg"
  port        = 3000
  protocol    = "HTTP"
  vpc_id      = var.vpc_id
  target_type = "ip"
  
  health_check {
    enabled             = true
    healthy_threshold   = 2
    interval            = 30
    matcher             = "200"
    path                = "/health"
    protocol            = "HTTP"
    timeout             = 5
    unhealthy_threshold = 3
  }
  
  deregistration_delay = 30
}

# Green target group
resource "aws_lb_target_group" "api_green" {
  name        = "api-green-tg"
  port        = 3000
  protocol    = "HTTP"
  vpc_id      = var.vpc_id
  target_type = "ip"
  
  health_check {
    enabled             = true
    healthy_threshold   = 2
    interval            = 30
    matcher             = "200"
    path                = "/health"
    protocol            = "HTTP"
    timeout             = 5
    unhealthy_threshold = 3
  }
  
  deregistration_delay = 30
}

# ALB listener rule
resource "aws_lb_listener_rule" "api" {
  listener_arn = var.alb_listener_arn
  priority     = 100
  
  action {
    type             = "forward"
    target_group_arn = aws_lb_target_group.api_blue.arn
  }
  
  condition {
    path_pattern {
      values = ["/api/*"]
    }
  }
}

# CloudWatch alarms
resource "aws_cloudwatch_metric_alarm" "api_error_rate" {
  alarm_name          = "api-error-rate-high"
  comparison_operator = "GreaterThanThreshold"
  evaluation_periods  = 2
  metric_name         = "5XXError"
  namespace           = "AWS/ApplicationELB"
  period              = 60
  statistic           = "Sum"
  threshold           = 20
  alarm_description   = "API 5XX errors exceeded threshold"
  treat_missing_data  = "notBreaching"
  
  dimensions = {
    LoadBalancer = var.alb_arn_suffix
    TargetGroup  = aws_lb_target_group.api_green.arn_suffix
  }
}

resource "aws_cloudwatch_metric_alarm" "api_latency" {
  alarm_name          = "api-latency-p99-high"
  comparison_operator = "GreaterThanThreshold"
  evaluation_periods  = 2
  metric_name         = "TargetResponseTime"
  namespace           = "AWS/ApplicationELB"
  period              = 60
  extended_statistic  = "p99"
  threshold           = 3.0
  alarm_description   = "API p99 latency exceeded 3 seconds"
  treat_missing_data  = "notBreaching"
  
  dimensions = {
    LoadBalancer = var.alb_arn_suffix
    TargetGroup  = aws_lb_target_group.api_green.arn_suffix
  }
}

# ECS Service with Linear deployment
resource "aws_ecs_service" "api" {
  name            = "node-api-service"
  cluster         = var.ecs_cluster_id
  task_definition = aws_ecs_task_definition.api.arn
  desired_count   = 6
  launch_type     = "FARGATE"
  
  deployment_configuration {
    deployment_type = "LINEAR"
    
    linear_configuration {
      step_percentage         = 20.0
      step_bake_time_in_minutes = 5
    }
    
    bake_time_in_minutes = 10
    
    deployment_circuit_breaker {
      enable   = true
      rollback = true
    }
    
    alarms {
      alarm_names = [
        aws_cloudwatch_metric_alarm.api_error_rate.alarm_name,
        aws_cloudwatch_metric_alarm.api_latency.alarm_name
      ]
      enable   = true
      rollback = true
    }
    
    maximum_percent         = 200
    minimum_healthy_percent = 100
  }
  
  load_balancer {
    target_group_arn = aws_lb_target_group.api_blue.arn
    container_name   = "api"
    container_port   = 3000
  }
  
  network_configuration {
    subnets          = var.private_subnet_ids
    security_groups  = [var.api_security_group_id]
    assign_public_ip = false
  }
}
```

With this configuration:

* Traffic shifts in 20% increments (20%, 40%, 60%, 80%, 100%)
    
* Each increment waits 5 minutes for validation
    
* If error rate exceeds 20 5XX errors/minute or p99 latency exceeds 3 seconds, automatic rollback triggers
    
* Total deployment time: ~30 minutes for a successful deployment
    

## Monitoring a Deployment in Progress

Once you trigger a deployment, you want visibility into what's happening. Here's how to monitor it:

## AWS Console

Navigate to your ECS service → Deployments tab. You'll see:

* Current traffic distribution (e.g., "Blue: 60%, Green: 40%")
    
* Deployment status and current stage
    
* Alarm status
    
* Timeline of traffic shifts
    

## AWS CLI

```bash
# Get deployment status
aws ecs describe-services \
  --cluster production \
  --services node-api-service \
  --query 'services[0].deployments[0]' \
  --output json

# Watch CloudWatch alarms
aws cloudwatch describe-alarms \
  --alarm-names api-error-rate-high api-latency-p99-high \
  --query 'MetricAlarms[*].[AlarmName,StateValue]' \
  --output table

# Stream service events
aws ecs describe-services \
  --cluster production \
  --services node-api-service \
  --query 'services[0].events[:10]' \
  --output table
```

## CloudWatch Logs Insights

Query deployment events:

```plaintext
fields @timestamp, @message
| filter @message like /deployment/
| sort @timestamp desc
| limit 100
```

## Handling Rollbacks

Rollbacks happen automatically when alarms trigger, but you can also initiate manual rollbacks.

## Automatic Rollback

If any configured CloudWatch alarm enters ALARM state during deployment, ECS:

1. Immediately stops the traffic shift
    
2. Routes all traffic back to the blue (stable) environment
    
3. Scales green tasks to zero
    
4. Marks deployment as FAILED
    

The rollback happens in seconds—much faster than the gradual rollout.

## Manual Rollback

If you detect an issue that alarms didn't catch:

```bash
# Option 1: Deploy previous task definition revision
aws ecs update-service \
  --cluster production \
  --service node-api-service \
  --task-definition node-api:42  # Previous revision

# Option 2: Update service to force new deployment with current task def
# (ECS will stop the in-progress deployment and use existing blue)
aws ecs update-service \
  --cluster production \
  --service node-api-service \
  --force-new-deployment
```

The key insight: during the bake time after reaching 100% traffic, both blue and green are still running. If you detect an issue during this window, rollback is instant—just shift ALB traffic back to blue. That's why setting an appropriate `bake_time_in_minutes` is important.

## Cost and Performance Considerations

## Resource Usage

Linear and Canary deployments run both blue and green task sets simultaneously during the deployment. This temporarily doubles your task count and associated costs.

For a service running 10 tasks normally:

* **Linear deployment**: 20 tasks running for the duration of the deployment (~30-60 minutes depending on configuration)
    
* **Canary deployment**: 20 tasks running for a shorter period (~15-30 minutes)
    

If you're running on Fargate at $0.04048 per vCPU-hour and $0.004445 per GB-hour:

* 10 additional tasks (0.5 vCPU, 1 GB each) for 60 minutes = ~$0.81 extra per deployment
    

Not a huge cost, but worth considering if you deploy very frequently.

## Deployment Duration

Linear deployments are slower than all-at-once:

* **Traditional blue/green**: 2-5 minutes (just health check time)
    
* **Canary**: 15-30 minutes (canary bake + deployment bake)
    
* **Linear (10% steps, 5 min bake)**: 45-60 minutes
    

This matters for deployment velocity. If you deploy 10 times per day and switch from 5-minute deployments to 45-minute Linear deployments, you've added 6.5 hours of active deployment time per day.

My approach: Use Linear for production environments where safety is paramount. Use traditional blue/green or rolling updates for development/staging where deployment speed matters more than risk mitigation.

## Traffic Volume Requirements

For meaningful validation, each traffic increment needs sufficient request volume.

If your service handles 1,000 requests/second:

* 10% increment = 100 req/sec for validation
    
* 5 minute bake time = 30,000 requests to validate the new version
    
* Plenty of data to detect issues
    

If your service handles 1 request/second:

* 10% increment = 0.1 req/sec for validation
    
* 5 minute bake time = 30 requests to validate
    
* Not enough data to reliably detect issues
    

For low-traffic services, you might need:

* Longer bake times to accumulate more requests
    
* Smaller increments (5% instead of 10%) but more of them
    
* Synthetic load testing during deployment
    
* Or just accept that Linear/Canary doesn't provide as much value
    

## Best Practices from Production

After using these features for a few months, here are patterns that work:

**Start Conservative**: Begin with 10% steps and 10-minute bake times. You can always make deployments faster once you're confident in your monitoring.

**Alarm Hygiene**: Review and tune your CloudWatch alarms regularly. False positives that trigger unnecessary rollbacks are frustrating. False negatives that miss real issues are dangerous.

**Monitor Business Metrics**: CloudWatch alarms catch technical issues (errors, latency), but not business issues (conversion drop, incorrect data). Use lifecycle hooks to validate business metrics from internal analytics systems.

**Test in Staging**: Use Linear/Canary in your staging environment first. You'll discover alarm configuration issues and timing problems before affecting production.

**Document Rollback Procedures**: Automatic rollbacks don't cover everything. Document manual rollback procedures for edge cases like database migration issues.

**Canary for High Traffic, Linear for Lower Traffic**: Services with millions of requests per day can validate quickly with canary. Services with thousands need Linear's multiple checkpoints.

**Set Realistic Bake Times**: 5 minutes is often too short to detect memory leaks or gradual performance degradation. For critical services, use 10-15 minute bake times.

**Graceful Shutdown**: Make sure your containers handle SIGTERM properly. Set ALB deregistration delay and give your application time to finish in-flight requests before exiting.

## What's Still Missing

While Linear and Canary deployments are a huge improvement, there are gaps:

**No Multi-Region Orchestration**: If you run the same service across multiple regions, you need to orchestrate deployments yourself. ECS won't automatically deploy region-by-region with validation.

**Limited Metric Analysis**: CloudWatch alarm integration is basic—just "is alarm triggered?" Third-party tools like Harness do statistical analysis comparing blue and green metrics.

**No Approval Gates**: You can't pause a Linear deployment after 50% and require human approval before continuing. It's fully automated based on alarms and bake times.

**Service Connect Limitations**: While Linear/Canary work with Service Connect, there's less documentation and fewer examples compared to ALB-based deployments.

**No Deployment Scheduling**: You can't configure deployments to only happen during business hours or require manual trigger for production.

For most teams, these limitations aren't blockers. But they're worth knowing if you have complex deployment requirements.

## Wrapping Up

Native Linear and Canary deployments fundamentally change the risk profile of ECS deployments. You no longer need to choose between "deploy fast with high risk" or "build custom traffic-shifting infrastructure." The platform gives you progressive delivery out of the box.

The configuration is straightforward—mostly just defining step percentages, bake times, and CloudWatch alarms. The hard parts are actually the same as before: having meaningful health checks, monitoring the right metrics, and designing your application to handle graceful shutdowns.

Start simple: pick Linear with 10% steps and 5-minute bake times, configure alarms for error rate and latency, and deploy to staging first. Once you're comfortable, tune the parameters for your specific needs.

For teams that previously avoided ECS because of deployment concerns, this addresses a major pain point. For teams already on ECS using custom Step Functions for gradual rollouts, you can finally delete that code and let the platform handle it.

The feature shipped in October 2025, three months after the community requested it on GitHub. That's remarkably fast for AWS to deliver, which suggests they understand how important safe deployments are for containerized workloads. Now go configure some gradual rollouts and sleep better during deployments.