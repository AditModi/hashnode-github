---
title: "Amazon ECS adds native Linear and Canary deployments"
datePublished: Mon Nov 03 2025 18:30:45 GMT+0000 (Coordinated Universal Time)
cuid: cmhjh6ycm000002la2ljw5b51
slug: amazon-ecs-adds-native-linear-and-canary-deployments
tags: aws

---

Amazon ECS now supports two new deployment strategies—linear and canary—letting teams shift production traffic gradually and validate changes safely without leaving ECS-native workflows. These strategies complement ECS blue/green, add lifecycle hooks for custom checks, integrate with CloudWatch alarms for auto-rollback, and work with ALB and Service Connect across all commercial AWS Regions. This makes safer, incremental rollouts a first-class experience on ECS for both new and existing services via Console, CLI, SDKs, CloudFormation, CDK, and Terraform.​

## Why this matters

* Safer releases: Shift real user traffic in controlled steps, observe, and roll back quickly on issues, reducing blast radius and MTTR.​
    
* Operational consistency: Use ECS-native constructs with ALB or Service Connect; no external traffic-shifting service required.​
    
* End-to-end automation: Lifecycle hooks and CloudWatch alarms enable pre/post checks and automatic rollback across phases.​
    

## What are linear deployments?

Linear deployments move traffic from blue (current) to green (new) in equal increments, pausing between steps to monitor health before proceeding. You configure a step percent and a step bake time; ECS performs each shift, waits, evaluates alarms and hooks, then continues until 100% on green, followed by an optional deployment bake time before terminating blue.​

* Step percent: Double value from 3.0 to 100.0 controls increment size per shift.​
    
* Step bake time: 0–1440 minutes wait between shifts for metrics and checks.​
    
* Deployment bake time: Post-cutover wait to keep blue alive for rapid rollback.​
    
* Hooks: Lambda lifecycle hooks run at defined stages (for example, PRODUCTION\_TRAFFIC\_SHIFT at each step).​
    

Example flow (10% steps):

* 10% green / 90% blue → wait → 20/80 → wait → … → 100/0 → deployment bake time → terminate blue.​
    

## What are canary deployments?

Canary deployments first send a small percentage of production traffic to green for a defined canary bake time. If healthy, ECS shifts the remainder to green in one move, then observes during a deployment bake time before retiring blue. This minimizes exposure during initial validation while still finishing quickly.​

Key elements:

* Canary percentage: Small portion (for example, 5–10%) receives production traffic during canary.​
    
* Canary bake time: Observation window to validate KPIs and alarms before full cutover.​
    
* One-step completion: After canary passes, traffic jumps to 100% on green.​
    
* Hooks and alarms: Lifecycle hooks and CloudWatch alarms operate across phases and can trigger rollbacks.​
    

Illustrative sequence:

* Start at 100% blue; green receives test traffic only. Shift to, for example, 10% green / 90% blue; observe; if healthy, shift to 100% green / 0% blue; deployment bake time; terminate blue.​
    

## Linear vs canary: when to choose which

| **Consideration** | **Linear** | **Canary** |
| --- | --- | --- |
| Risk tolerance | Prefer granular risk control with multiple checkpoints. ​ | Accept brief limited exposure before full cutover. ​ |
| Time to complete | Longer (multiple steps and waits). ​ | Faster (two phases: small canary then complete). ​ |
| Metrics stability | Helps stabilize noisy metrics across steps. ​ | Suits clear pass/fail KPIs during canary window. ​ |
| Operational overhead | More steps and evaluations. ​ | Fewer steps to manage. ​ |
| Use cases | Major version changes, infra/runtime upgrades, sensitive latency/SLI risk. ​ | Minor releases, config changes, low-risk patches. ​ |

## Core building blocks

* Traffic shifting engine: ECS orchestrates ALB listener rules and target groups or Service Connect routing to move production traffic between blue and green.​
    
* Lifecycle stages and hooks: Defined deployment stages can invoke Lambda functions for smoke tests, synthetic checks, or external control-plane coordination. Hooks defined at PRODUCTION\_TRAFFIC\_SHIFT execute at every shift.​
    
* CloudWatch alarms and rollback: You can wire alarms for error rate, latency, 5XXs, saturation, or business KPIs; ECS halts and rolls back on alarm breach.​
    
* Bake times: Canary/step bake times gate progress; deployment bake time preserves blue after full cutover for rapid reversions.​
    

## Prerequisites and compatibility

* Load balancing: Works with Application Load Balancer target groups and listener rules, or ECS Service Connect for service-to-service traffic.​
    
* Regions: Available in all commercial regions where ECS is available.​
    
* IaC and tooling: Supported via Console, SDK, CLI, CloudFormation, CDK, and Terraform; provider updates for new deployment\_configuration fields are in progress in open-source ecosystems.​
    

## How deployments progress internally

Both strategies follow a common six-phase lifecycle:

1. Preparation: Stand up green alongside blue. 2) Deployment: Launch new tasks for green. 3) Testing: Route test traffic to green while blue serves production. 4) Traffic shift: Linear increments or canary-then-complete. 5) Monitoring: Observe alarms and metrics; rollback on failure. 6) Completion: Terminate blue after deployment bake time.​
    

## Designing your strategy

* Choose step or canary percentages based on error budget and service SLOs; tighter SLOs usually favor smaller steps and longer waits.​
    
* Align bake times to detection latency of your KPIs (for example, p95 latency stability and sustained 5XX rates).​
    
* Define clear rollback criteria via CloudWatch alarms; avoid only manual judgment.​
    
* Use hooks for end-to-end validation: smoke tests, synthetic journeys, schema checks, and dependency readiness.​
    

## Example configurations

Linear example (safe, gradual):

* Step percent: 10.0
    
* Step bake time: 10 minutes
    
* Deployment bake time: 30 minutes
    
* Alarms: HTTP 5XX &gt; 1%, p95 latency +20%, error spikes on key endpoints
    
* Hooks: PRODUCTION\_TRAFFIC\_SHIFT smoke tests and business transaction probes per step​
    

Canary example (fast validation):

* Canary percent: 5.0
    
* Canary bake time: 15 minutes
    
* Deployment bake time: 15 minutes
    
* Alarms: Cart checkout failure rate, auth 5XX/latency, CPU/memory saturation
    
* Hooks: PRODUCTION\_TRAFFIC\_SHIFT synthetic canary journey; AFTER\_TRAFFIC\_SHIFT contract tests​
    

## Operational best practices

* Keep blue warm: Use deployment bake time to ensure instant rollback path.​
    
* Test hooks offline: Validate Lambda hooks independently to avoid false positives halting deployment.​
    
* Scope alarms to green: Use dimensions/target-group metrics to avoid blue noise influencing decisions.​
    
* Guard data changes: Pair DB migrations with forward/backward compatibility; decouple schema rollouts from traffic shifts.​
    
* Simulate failure: Regularly run game days to verify rollback triggers, hook behavior, and alarm thresholds.​
    

## Implementation outline

With ALB:

* Prepare two target groups (blue, green) behind a listener; ECS manages routing rules during deployment.​
    
* Define deployment strategy (linear step percent/time, or canary percent/time), bake times, lifecycle hooks, and alarms.​
    
* Start deployment; monitor health via service events, CloudWatch dashboards, and alarm states; approve or roll back as required.​
    

With Service Connect:

* Configure service discovery and traffic shifting via ECS-managed routing; apply the same strategy and bake-time parameters with hooks and alarms.​
    

## Rolling up from blue/green

If already using ECS blue/green, linear and canary introduce finer-grained traffic movement: linear replaces the “all-at-once” cutover with stepwise increments, while canary inserts a protective canary window before a single-step completion. Both preserve the same hooks, alarms, and bake-time concepts across the lifecycle, minimizing learning curve.​

## Tooling and IaC notes

* CloudFormation/CDK: Use new deployment configuration fields to specify strategy, step or canary percentages, and bake times.​
    
* Terraform: Provider support is being tracked; expect new arguments under ECS service deployment configuration for linear/canary strategies.​
    
* Pipelines: Integrate with CodePipeline or your CI/CD to trigger deployments while keeping health checks and alarms as gates
    

## Conclusion

Native Linear and Canary deployments make ECS rollouts safer and calmer without extra moving parts. Pick linear when you want confidence through multiple checkpoints; pick canary when you want a quick, low-exposure prove-out. Wire KPIs and hooks close to the user experience, keep blue warm with a deployment bake time, and let ECS handle the routing while your alarms guard the gate. This is the right kind of boring for production change management—and it’s now built in.