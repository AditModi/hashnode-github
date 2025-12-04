---
title: "My Takeaways from Swami’s re:Invent 2025 Keynote — The Era of Agentic AI Is Here"
datePublished: Thu Dec 04 2025 12:50:41 GMT+0000 (Coordinated Universal Time)
cuid: cmirfp1fk000602lhe0a501p0
slug: my-takeaways-from-swamis-reinvent-2025-keynote-the-era-of-agentic-ai-is-here
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1764852626456/4fd14f95-9fba-4705-96f2-09044a37f32e.png
tags: aws

---

Day 3 of re:Invent brought one of the most thoughtfully constructed keynotes I’ve seen from AWS. Instead of launching straight into features, **Dr. Swami Sivasubramanian** started with the actual challenges we all deal with when building AI systems: hallucinations, fragmented tooling, complex infra, and the difficulty of putting agents into real workflows.

From there, he walked through how AWS is solving these problems — one layer at a time — and immediately showed each solution in action with engineers and customers. The result was a keynote that felt honest, practical, and very builder-focused.

---

# **Agents, Not Chatbots**

Swami made a clear distinction early on:

**Chatbots answer. Agents act.**

Agents:

* Investigate
    
* Diagnose
    
* Plan
    
* Call tools
    
* Execute
    
* Check results
    
* Learn over time
    

He broke an agent down into three components — the model, the code, and the tools — and somehow made the architecture of agentic systems feel very approachable.

---

# **Strands Agent SDK — Now for Everyone**

One of the biggest unlocks was the evolution of the **Strands Agent SDK**.

Key updates:

* **TypeScript support**, massively opening the door for web developers
    
* **Edge device support** for robotics, automotive, gaming, and more
    
* A more streamlined, model-driven approach to building agents with minimal code
    

This is the moment where agentic AI moves beyond ML circles and into mainstream development.

---

# **Amazon Bedrock AgentCore — The Runtime Agents Needed**

**AgentCore** was easily one of the most impactful announcements.

It tackles everything that makes production agents hard:

* Memory systems (short-term, long-term, episodic)
    
* Identity and permissions
    
* Tool orchestration
    
* Observability
    
* Multi-step execution
    
* Deployment and scaling
    

And episodic memory really stood out — agents can now remember past experiences, learn from them, and adapt over time.

This is the beginning of agents that actually get better the more you use them.

---

# **Model Customization — Accessible to Every Developer**

Swami spent a big chunk of time on customization, but for the first time, the entire stack felt simple and usable.

### Techniques he covered:

* **Supervised Fine-Tuning (SFT)**
    
* **Model Distillation**
    
* **Reinforcement Learning (RL)** with human or AI feedback
    
* **Reinforcement Fine-Tuning (RFT)** on Bedrock
    

RFT was the standout:  
It automates reward modeling, evaluation, training, and safety checks — something that traditionally needed research-level ML teams.

This is AWS taking advanced model engineering and making it accessible.

---

# **SageMaker AI — Full Control When You Need It**

While Bedrock focuses on simplicity, **SageMaker AI** remains the place for deep control.

Swami highlighted:

* End-to-end pipelines for training custom models
    
* Full ownership of data, methods, and infrastructure
    
* A new serverless customization experience guided by agentic workflows
    

Bedrock and SageMaker now feel intentionally complementary:  
**Speed vs. control.**

---

# **Nova Forge — Build Your Own Frontier Model**

This one surprised me.

**Nova Forge** gives enterprises the ability to build their own frontier-scale models by:

* Accessing intermediate checkpoints
    
* Mixing proprietary data into Nova’s training cycle
    
* Retaining Nova’s safety and foundational knowledge
    
* Significantly reducing the cost of training from scratch
    

This is frontier model development without needing frontier-scale resources.

---

# **SageMaker HyperPod & Checkpointless Training**

HyperPod continues to mature into a powerhouse for training large models.

What really changed the game was **checkpointless training**:

* The model state is continuously preserved across the cluster
    
* Node failures automatically recover in minutes
    
* No manual checkpointing
    
* No lost training runs
    
* Large-scale training becomes more efficient and cost-effective
    

This solves one of the most painful problems in LLM training.

---

# **Real Customer Stories — Grounding Everything**

Hearing from teams like **Vercel**, **Blue Origin**, and others helped connect the announcements to real-world use cases.

Examples included:

* Developer workflows powered by agentic tools
    
* Production use of Bedrock and Nova
    
* Reinforcement learning applied at scale
    
* Real latency and cost improvements
    

These weren’t theoretical demos — they were practical, in-production stories.

---

# **A Glimpse Into the Future**

Swami closed the keynote by painting a future where agents:

* Handle complex, multi-step workflows
    
* Reason about tasks and coordinate with each other
    
* Become more reliable through episodic memory
    
* Operate across cloud, edge, and devices
    
* Are trained with proprietary industry knowledge
    
* Move toward autonomous systems
    

It didn’t feel like hype. It felt like a roadmap.

---

# **Nova Act — The Final Launch**

Right before wrapping up, Swami introduced **Nova Act** — a service for enabling highly reliable agent actions inside web browsers.

This unlocks:

* Workflow automation
    
* Browser-based RPA
    
* UI-heavy enterprise workflows
    
* Agents that actually take actions, not just provide instructions
    

It ties the entire agentic stack together.

---

# **Celebrating Builders — Hackathons, Programs, and Winners**

After the announcements, Swami shifted to something that genuinely made the keynote more personal — recognizing the **developers** who make all of this possible.

He highlighted:

* Ongoing **developer-focused hackathons**
    
* Community-driven agent-building initiatives
    
* Global innovation programs leading up to re:Invent
    
* Grassroots developer momentum around agentic AI
    

He also congratulated the winners of:

* **Road to re:Invent**
    
* **AI League**
    

It was a nice moment — a reminder that behind all this powerful tech are actual builders pushing the boundaries.

---

# **Final Thoughts**

This keynote wasn’t about flashy announcements. It was a blueprint for the next era of AI — an era where everyday developers can build powerful, reliable, and intelligent agents without needing a PhD.

For me, the biggest shift is clear:

**AI is moving from answering questions to solving real problems.  
From chatbots to agents.  
From models to systems.**

And AWS is quietly building every layer to make that future real for all of us.

If you want a more unfiltered, real-time version of my thoughts during the keynote, I also live-tweeted the entire session on X. It includes reactions, screenshots, and moment-by-moment highlights that didn’t make it into this blog. You can check out the full thread here: [https://x.com/adi\_12\_modi/status/1996257235731955907?s=20](https://x.com/adi_12_modi/status/1996257235731955907?s=20)