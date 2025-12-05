---
title: "My Takeaways from the Peter DeSantis & Dave Brown Keynote — re:Invent 2025"
datePublished: Fri Dec 05 2025 10:41:00 GMT+0000 (Coordinated Universal Time)
cuid: cmisqi4mh000f02l1cema2lut
slug: my-takeaways-from-the-peter-desantis-and-dave-brown-keynote-reinvent-2025
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1764930770940/30e0e173-55ab-4ba9-9144-52807a84d798.png
tags: aws

---

I’ll admit it upfront: I didn’t make it to the keynote hall for this one.  
Jetlag + late night writing = I woke up late and had to watch the keynote afterward.  
But honestly? Even on replay, this keynote delivered exactly what I expect from Peter and Dave every year — deep tech, real numbers, and a behind-the-scenes look at how AWS is engineered.

This keynote isn’t about AI announcements or product hype.  
It’s about the *infrastructure, silicon, and systems thinking* that make everything on AWS possible.

---

## **The Five Unchanging Pillars of AWS**

Peter opened with a reminder of the core attributes AWS has optimised around for nearly two decades:

* **Security**
    
* **Availability**
    
* **Elasticity**
    
* **Cost**
    
* **Agility**
    

Even in a rapidly evolving AI world, these don’t change — only the techniques to achieve them do.

It was a good grounding moment. Before we talk about the future of AI, we need to understand the foundation that makes any of it possible.

---

## **Nitro — The Engine Behind AWS Compute**

Peter used the **Nitro System** as the perfect example of AWS’s deep, multi-year investment strategy.

Nitro continues to be:

* Custom silicon
    
* Minimal overhead
    
* Isolation by design
    
* Virtually no “jitter”
    
* And now… literally part of CS textbooks
    

Nitro isn’t a feature. It’s AWS’s DNA.

---

## **Graviton — Cloud CPUs Done Right**

Dave Brown came on stage next, and he delivered absolute gold.

### **Highlights:**

* Graviton keeps delivering the best price-performance for cloud workloads.
    
* Real-world customer impact from Adobe, Epic Games, Formula 1, Pinterest, SAP.
    
* Direct-to-silicon cooling for better sustainability & thermal efficiency.
    
* **Graviton 4** doubling L2 cache.
    
* And then the big one: **Graviton 5**
    
    * 192 cores
        
    * More L3 cache
        
    * Up to 25% better performance
        

AWS isn’t slowing down. They’re accelerating.

---

## **Apple on Graviton — A Rare and Powerful Guest Moment**

One of the most interesting segments was Payam Mirrashidi from Apple.

He shared:

* How Apple uses Swift on Graviton for huge-scale services (App Store, Music, TV).
    
* Why Swift’s performance & safety make it ideal for modern cloud apps.
    
* How homomorphic encryption + Swift power privacy-preserving features like spam detection.
    
* And the announcement: **native Swift toolchain for Amazon Linux**.
    

That’s a big shift for Swift developers and cloud-native teams.

---

## **Lambda’s Evolution + Lambda Managed Instances**

Peter returned to talk serverless, tracing the journey from the *birth* of Lambda in 2014 to where we are now.

And then came something interesting:

### **Lambda Managed Instances**

A new way to blend:

* **Serverless simplicity**
    
* **EC2-like control**
    

Customers can pick instance types and hardware, while Lambda still manages scaling, patching, and provisioning.

This is AWS listening to real customer pain points — especially those needing predictable performance with serverless ergonomics.

---

## **Project Mantle — Rebuilding Inference for the AI Era**

This might be one of the most important (but underrated) announcements.

Peter introduced **Project Mantle**, the new inference engine that powers Amazon Bedrock.

Key capabilities:

* **Service tiers** for different latency/urgency needs
    
* **Customer-specific queues** to guarantee fairness
    
* **Adaptive capacity sharing**
    
* **A durable journal** for fault tolerance
    
* **Confidential computing** for high-security workloads
    

Inference is becoming the real bottleneck in AI — and Mantle feels like AWS’s long-term answer.

---

## **Vector Search Everywhere**

Peter then shifted to **vector search**, breaking it down in a way that actually makes sense:

* Embedding models convert data into multi-dimensional vectors
    
* Vector databases find nearest neighbors
    
* Suddenly, unstructured knowledge becomes searchable
    

This isn’t just for search engines — it’s for any org trying to unlock institutional knowledge across docs, media, chats, and more.

---

## **Agentic Development + AWS Agent**

This was a smooth transition into agentic development.

Peter explained how agents become a new abstraction layer —  
one that uses your existing systems, APIs, and knowledge bases,  
but automates multi-step tasks end-to-end.

Then came **AWS Agent** — the tool to actually build those agents:

* Connect to your tools & data
    
* Define goals
    
* Let agents plan & act
    
* Even generate pull requests
    

Seeing an agent analyze feedback → design a feature → submit a PR was surreal.  
This is no longer “future tech.” It’s happening.

---

## **Closing Thoughts**

Peter ended with a simple message:  
AWS solves the hard problems so builders can invent faster.

After watching this keynote, that mission feels more alive than ever — from silicon, to serverless, to inference engines, to agentic automation.

Even though I watched it late (because I definitely overslept), this keynote reminded me *why* AWS continues to lead:  
they build deep, they build long-term, and they build for builders.