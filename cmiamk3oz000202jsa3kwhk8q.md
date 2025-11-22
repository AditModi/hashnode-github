---
title: "VPC Encryption Controls — Enforce Encryption In-Transit Across Your VPCs"
datePublished: Sat Nov 22 2025 18:30:43 GMT+0000 (Coordinated Universal Time)
cuid: cmiamk3oz000202jsa3kwhk8q
slug: vpc-encryption-controls-enforce-encryption-in-transit-across-your-vpcs
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1763818101503/c2e02f84-3b4f-4269-ae83-11c86c7d2ebd.png
tags: aws

---

AWS just launched **VPC Encryption Controls**, a capability to centrally **monitor and enforce encryption for traffic within and between VPCs**, all inside the same Region.

---

### Why This Matters

* In regulated environments (think Finance, Healthcare, PCI/FedRAMP), you often need **end-to-end encryption** inside your VPC — not just over public networks.
    
* While modern Nitro-based EC2 instances already encrypt traffic at the hardware level, not all resources or older instances do. Encryption Controls helps you identify and remediate those gaps.
    
* It gives you **visibility + control** through two modes:
    
    * **Monitor**: Audit who’s sending unencrypted traffic
        
    * **Enforce**: Drop unencrypted traffic and block creation of non-compliant resources
        

---

### How It Works (Analogy)

Think of your network traffic like mail running inside a giant building (your VPC):

* **Monitor mode** = You set up surveillance cameras in all hallways. You *see* who’s using sealed envelopes (encrypted) and who’s sending postcards (plaintext), but you don’t stop them.
    
* **Enforce mode** = You install a security checkpoint: no envelope, no delivery. And you only allow people (resources) that already use sealed envelopes or trusted packaging.
    

---

### Key Capabilities & Details

* **Encryption-status in VPC Flow Logs**: A new `encryption-status` field helps you audit traffic — whether it’s hardware-encrypted (Nitro), TLS-encrypted, or both.
    
* **Automatic migration**: For supported services (ALB, NLB, Fargate, EKS), AWS transparently moves resources to encryption-capable Nitro infrastructure when you turn on monitor mode.
    
* **Enforce mode restrictions**:
    
    * Non-compliant traffic is dropped.
        
    * You can exclude some resources (e.g. NAT Gateways, Internet Gateways) where encryption is not feasible.
        
* **Transit Gateway support**: If you use a Transit Gateway, you can enable encryption support there too. When both VPCs and the TGW are in enforce mode, all inter-VPC traffic is encrypted.
    

---

### How to Get Started — Practical Steps

1. **Enable VPC Encryption Control (Monitor Mode):**
    
    * In the AWS Console → go to the VPC → “Encryption Controls” tab → create a control for your VPC.
        
    * Or use AWS CLI:
        
        ```bash
        aws ec2 create-vpc-encryption-control --vpc-id <your-vpc-id>
        ```
        
2. **Set up VPC Flow Logs** to include `encryption-status`:
    
    * This lets you see which flows are not encrypted.
        
3. **Audit non-compliant resources**:
    
    * Use the console or CLI command:
        
        ```bash
        aws ec2 get-vpc-resources-blocking-encryption-enforcement --vpc-id <your-vpc-id>  
        ```
        
    * Identify which instances, ALBs, or other resources need migration or TLS configuration.
        
4. **Migrate or exclude**:
    
    * For unsupported resources, either **upgrade to Nitro-based instances** or configure TLS.
        
    * For resources that can’t be encrypted (like some gateways), create **exclusions** so enforce mode works correctly.
        
5. **Switch to Enforce Mode** when ready\*\*:
    
    * Once all resources are compliant (or excluded), enable enforce mode. Unencrypted traffic will be dropped; future resources must support encryption.
        
    * Use CLI:
        
        ```bash
        aws ec2 modify-vpc-encryption-control --vpc-id <your-vpc-id> --mode enforce  
        ```
        
6. **If using Transit Gateway**:
    
    * Ensure TGW encryption support is enabled. Then enforce mode works across VPCs.
        

---

### Trade-Offs / Things to Consider

* **Resource compatibility**: Not all AWS resources automatically support the hardware-layer encryption — some require migration.
    
* **Possible exclusions**: You’ll need to make decisions about what to exclude (e.g., IGW, NAT) because they can’t be encrypted in the same way.
    
* **Monitor first**: You *must* start in monitor mode to understand your current encryption posture and fix it before enforcing.
    
* **IAM / Permissions**: Ensure you have the right IAM roles to create and modify VPC encryption controls.
    
* **Audit/logging**: Use Flow Logs + encryption-status to continuously monitor post-enforcement.
    

---

### Why Teams Should Care (Impact)

* **Security & Compliance**: A centralized, auditable way to enforce in-transit encryption helps meet compliance needs (HIPAA, PCI, etc.).
    
* **Cost-effective**: Uses **native Nitro hardware encryption**, so you get strong protection without a custom PKI or expensive overlay encryption.
    
* **Simplicity**: Reduces operational burden — AWS handles migration for many services, and you get one “control pane” for encryption.
    
* **Long-term visibility**: Flow logs + control let you continuously monitor and evolve your encryption posture.
    

---

## TL;DR

**VPC Encryption Controls** let you audit and enforce encryption *in transit* across your VPCs using native hardware (Nitro) or TLS — with two modes (monitor → enforce), drop-unencrypted traffic, and track compliance centrally. Security teams love it; architects can simplify crypto complexity.

---

## Part of *Road to re:Invent: Cloud Concepts Made Simple*

This series breaks down AWS updates in:

* Simple language
    
* Practical context
    
* With guidance you can use immediately
    

More updates coming as launches roll in.  
Stay tuned. 👀