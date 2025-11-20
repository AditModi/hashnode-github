---
title: "🚀 NLB Just Got Smarter: Weighted Target Groups for Network Load Balancer"
datePublished: Thu Nov 20 2025 14:06:33 GMT+0000 (Coordinated Universal Time)
cuid: cmi7i8opl000202k19rbugmiv
slug: nlb-just-got-smarter-weighted-target-groups-for-network-load-balancer
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1763646949662/17ad376e-91c7-40ee-a549-6f62a7d9ddd4.png
tags: aws

---

Imagine your Network Load Balancer (NLB) is a traffic controller at a busy intersection. Until now, it could send all cars (requests) either down just one road or split them evenly into fixed lanes. But **what if you want to send 70% of traffic down one road, 20% down another, and 10% down a testing lane**? That kind of flexible control was hard to do — but not anymore.

### What’s New

AWS has added **weighted target groups** for NLBs. You can now register multiple target groups behind a single NLB listener and assign a **weight (0–999)** to each.

* Traffic is distributed based on the weights you set, while still respecting availability zone balancing.
    
* Works with all target group types: instance, IP, **even other ALBs** behind the NLB.
    
* No extra charge — uses standard NLB LCU pricing.
    

---

### Why It Matters (Use Cases)

* **Blue-Green / Canary Deployments**: Gradually shift traffic from “green” target group to “blue” (or vice versa) by increasing weight, reducing risk during deployments.
    
* **Maintenance Windows**: Want to patch one subset of servers? Set its weight to zero temporarily, so no new traffic goes there.
    
* **A/B Testing**: Route a small percentage of users to a new feature or version (target group) without needing separate load balancers.
    

---

### How to Use It — Immediate Steps

1. Go to the **NLB Listener** in the AWS Console or use the AWS CLI / SDK.
    
2. Modify the listener’s **ForwardConfig** to include multiple target groups with weights. Example CLI:
    
    ```python
    aws elbv2 modify-listener \
      --listener-arn <listener-arn> \
      --default-actions '[{  
        "Type": "forward",  
        "ForwardConfig": {  
          "TargetGroups": [  
            {"TargetGroupArn": "<TG-A-ARN>", "Weight": 700},  
            {"TargetGroupArn": "<TG-B-ARN>", "Weight": 300}  
          ]  
        }  
      }]'
    ```
    
3. Monitor traffic using CloudWatch: the `NewFlowCount` metric now supports breakdown by target group, so you can confirm your weights are working as expected.
    
4. Adjust weights over time depending on how traffic shifts, or based on maintenance needs.
    

---

### Things to Watch Out For / Tips

* Stickiness is supported: if you enable stickiness, clients can remain “on” a particular target group even if weights change.
    
* Listener rules must use **consistent protocols**: you cannot mix TCP and TLS target groups under the same listener when using weights.
    
* Make sure all target groups under a weighted config use the **same IP version** (all IPv4 or all IPv6).
    
* Use the **Weighted Routing Evaluation** feature in the console to validate cross-zone distribution and expected behavior.
    

---

## TL;DR

With weighted target groups on NLB, you get **fine-grained control over how much traffic** goes to each target group — great for rolling updates, canary testing, or just more intelligent traffic management — **without adding more load balancers**.

---

## Part of *Road to re:Invent: Cloud Concepts Made Simple*

This series breaks down AWS updates in:

* Simple language
    
* Practical context
    
* With guidance you can use immediately
    

More updates coming as launches roll in.  
Stay tuned. 👀