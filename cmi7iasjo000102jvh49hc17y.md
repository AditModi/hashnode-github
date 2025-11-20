---
title: "NAT Gateway Goes Regional: One Gateway, All AZs — Automatically"
datePublished: Thu Nov 20 2025 14:08:12 GMT+0000 (Coordinated Universal Time)
cuid: cmi7iasjo000102jvh49hc17y
slug: nat-gateway-goes-regional-one-gateway-all-azs-automatically
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1763647670957/8a891089-942a-4258-a2ae-fd599aec6c84.png
tags: aws

---

If you've ever managed NAT Gateways in production, you know the drill:  
**One NAT Gateway per Availability Zone, plus separate public subnets, plus multiple route tables…**  
It works — but it’s a *lot* of plumbing.

AWS just made this dramatically simpler with **Regional Availability Mode for NAT Gateway**.

---

## 🛠️ What’s New?

You can now create **one single NAT Gateway** in *regional mode*, and AWS automatically:

* Expands it into every AZ where you have workloads
    
* Shrinks it from AZs where there are no private subnets or traffic
    
* Keeps it highly available across the Region
    
* Removes the need for public subnets to host NAT Gateways
    

No more creating one NAT Gateway per AZ.  
No more managing multiple route tables per AZ.  
No more remembering to add/delete NAT Gateways when workloads scale into new AZs.

---

## 🧠 The Analogy:

### **Think of the new Regional NAT Gateway like a “smart power strip” for your entire Region.**

Before:  
You had to bring a separate adapter for every plug socket (every AZ). If you used 3 rooms, you carried 3 adapters.

Now:  
AWS gives you one power strip that automatically extends adapters into every room you walk into — and retracts them when you leave.

You just plug in once and it works everywhere.

---

## 💡 Why This Matters (Practical Benefits)

### 1\. **Massive Simplification**

No more:

* One NAT per AZ
    
* Public subnet requirement
    
* Manual AZ expansion steps
    
* Route table edits
    

You create **one** NAT Gateway. That’s it.

---

### 2\. **High Availability out-of-the-box**

Regional NAT automatically becomes multi-AZ internally, following your workloads.

If your compute moves into 3 AZs → NAT expands to 3  
If only 1 AZ is used → NAT contracts to 1  
All without you touching anything.

---

### 3\. **Cost Savings Potential**

Depending on patterns, you may replace multiple NAT Gateways with a single regional one — simplifying billing and potentially reducing cost.

---

### 4\. **BYOIP or AWS IP**

You can choose:

* AWS-provided public IP
    
* Bring Your Own IP (BYOIP) to keep existing approved allowlists
    

---

## 🧭 How To Use It (Immediate Steps)

1. **Go to VPC Console → NAT Gateways → Create NAT Gateway**
    
2. Select **Availability Mode: Regional**
    
3. Choose your **VPC**
    
4. Pick your IP option:
    
    * AWS-provided
        
    * BYOIP
        
5. Create it
    
6. Update your **private subnet route tables** to use this new regional NAT Gateway ID
    

You're done.

No public subnets.  
No per-AZ NAT Gateways.  
No per-AZ route tables.

---

## 🧪 Quick Test You Can Try Today (Hands-on)

Here’s a simple validation flow:

### **Step 1 — Create a Regional NAT Gateway**

### **Step 2 — Launch EC2 instances in two different private subnets (in two AZs)**

* No internet gateway
    
* No public subnet
    

### **Step 3 — Add route: 0.0.0.0/0 → NAT Gateway**

### **Step 4 — SSH the instances via SSM Session Manager**

Run:

```bash
curl https://checkip.amazonaws.com
```

You’ll see:

* Both instances use the NAT Gateway’s IP
    
* No errors even though the NAT isn’t “manually” deployed in each AZ
    
* Traffic automatically routed via the regional NAT infrastructure
    

### **Step 5 — Try removing one subnet**

Watch how NAT continues working without manual adjustment.

This is the “smart power strip” effect in action.

---

## TL;DR

AWS just made NAT simple again:

* **One NAT Gateway for the whole Region**
    
* **No public subnet required**
    
* **Auto-expands into every AZ you use**
    
* **Less setup, fewer moving parts, simpler ops**
    

If your architecture uses NAT at all, this update is an immediate quality-of-life improvement.

---

## Part of *Road to re:Invent: Cloud Concepts Made Simple*

This series breaks down AWS updates in:

* Simple language
    
* Practical context
    
* With guidance you can use immediately
    

More updates coming as launches roll in.  
Stay tuned. 👀