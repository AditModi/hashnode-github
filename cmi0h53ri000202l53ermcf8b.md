---
title: "Automatic Account Enrollment in AWS Control Tower — Explained the Simple Way"
datePublished: Sat Nov 15 2025 16:01:23 GMT+0000 (Coordinated Universal Time)
cuid: cmi0h53ri000202l53ermcf8b
slug: automatic-account-enrollment-in-aws-control-tower-explained-the-simple-way
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1763222410687/6e69750a-e38b-4b50-9aa2-4811f8644c4a.png
tags: aws

---

Imagine you run a large apartment building.  
Every time a new tenant moves in, you need to manually hand them:

* The door keys
    
* Wi-Fi password
    
* Safety rules
    
* Emergency contacts
    

Now imagine tenants moving between floors — and you having to redo that whole setup manually again.

That *was* AWS account governance before.  
Today’s update fixes that.

---

## 🎉 What’s New

**AWS Control Tower now automatically enrolls accounts** the moment you move them into an Organizational Unit (OU).

No more re-registering OUs.  
No more manual updates.  
No more “Did we apply the right guardrails yet?” moments.

Just drag → drop → done.

---

## 🧠 Why This Matters (in real teams)

### Before

When you created an account or moved it between OUs, you had to manually apply:

* Baselines
    
* Controls
    
* Logging and monitoring
    
* Guardrails
    
* IAM & security configurations
    

…and if you forgot one step, the account drifted from your standards.

### Now

Move the account to the right OU → **Control Tower auto-applies everything**:

* Best practice security baselines
    
* Governance controls
    
* Logging + monitoring standards
    
* Any OU-specific rules
    
* And removes the old OU’s settings automatically
    

It’s like having a **smart building** where tenants automatically receive the right keys, rules, and access the moment they enter a new floor.

---

## 🛠️ How it Works

To enable automatic enrollment, you need:

* **Landing Zone version 3.1+**
    
* Toggle **“Automatically enroll accounts”** in Landing Zone settings
    
* Or set `RemediationTypes = Inheritance_Drift` in the Control Tower API
    

Once enabled:

* Create an account
    
* Move it into your target OU via AWS Console or API
    
* Control Tower does the rest — instantly and consistently
    

---

## 🧭 Practical Guidance You Can Use Today

Here’s where this helps *immediately*:

### ✔️ 1. Faster onboarding

Create account → Move → Done. Governance applied automatically.

### ✔️ 2. Zero-drift migrations

Moving accounts between teams or environments (Dev → Stage → Prod) is now reliable and repeatable.

### ✔️ 3. Cleaner org structure

No need to re-register OUs or worry about stale configurations.

### ✔️ 4. Stronger governance at scale

Perfect for organizations with 50, 100, or 500+ accounts.

---

## **TL;DR (Plain and Simple)**

Control Tower just became **way more hands-off**:

> “Move an account to an OU and Control Tower will automatically set it up with the right guardrails, baselines, and configs.”

Less manual work.  
Less drift.  
More consistency.

---

## Part of *Road to re:Invent: Cloud Concepts Made Simple*

This series breaks down AWS updates in:

* Simple language
    
* Practical context
    
* With guidance you can use immediately
    

More updates coming as launches roll in.  
Stay tuned. 👀