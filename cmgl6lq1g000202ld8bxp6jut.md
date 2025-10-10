---
title: "HashiConf 2025: Key Announcements & Why I’m Hyped (From a Builder’s Lens)"
datePublished: Fri Oct 10 2025 18:30:08 GMT+0000 (Coordinated Universal Time)
cuid: cmgl6lq1g000202ld8bxp6jut
slug: hashiconf-2025-key-announcements-and-why-im-hyped-from-a-builders-lens
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1759629104305/fb0397cc-3fae-45b6-908d-d1dfabd9bcbb.png
tags: hashicorp

---

A few reflections on what stood out technically from HashiConf 2025 — and what I’m watching closely for what’s next.

### Big Picture: Infrastructure + Security + Intelligence

One of the overarching themes this year was **bridging infrastructure, security, and intelligence**. HashiCorp leaned heavy into strengthening the core (Terraform, Vault, Packer, etc.) while also signaling a push toward more “agentic” infrastructure: systems that don’t just execute what you ask, but reason, observe, and act in context.

The introduction of **Project Infragraph** was a centerpiece of that vision. [HashiCorp | An IBM Company+3IBM Newsroom+3HashiCorp | An IBM Company+3](https://newsroom.ibm.com/2025-09-25-HashiCorp-Previews-the-Future-of-Agentic-Infrastructure-Automation-with-Project-infragraph?utm_source=chatgpt.com) In plain terms: Infragraph is intended to be a real-time graph model that unifies infrastructure state, security, relationships, and ownership into a single source of truth. Over time, it’s supposed to help AI or agentic systems understand the *context* in which they’re operating — which is crucial if you want agents to do more than just “run this command.” [IBM Newsroom+1](https://newsroom.ibm.com/2025-09-25-HashiCorp-Previews-the-Future-of-Agentic-Infrastructure-Automation-with-Project-infragraph?utm_source=chatgpt.com)

Infragraph is being positioned inside the HashiCorp Cloud Platform (HCP) first, with plans to extend it later to IBM’s ecosystem (e.g. Ansible, OpenShift, WatsonX, etc.). [IBM Newsroom+1](https://newsroom.ibm.com/2025-09-25-HashiCorp-Previews-the-Future-of-Agentic-Infrastructure-Automation-with-Project-infragraph?utm_source=chatgpt.com) They’re opening up a private beta (expected by Dec 2025) for people to start experimenting. [IBM Newsroom+1](https://newsroom.ibm.com/2025-09-25-HashiCorp-Previews-the-Future-of-Agentic-Infrastructure-Automation-with-Project-infragraph?utm_source=chatgpt.com)

From my standpoint, Infragraph is interesting because it could become the *context engine* under agentic infrastructure. If your AI/agent doesn’t understand relationships (what owns what, which resource depends on which, what the security posture is), you risk making decisions that are dangerous at scale. This could be the backbone for future runbooks, automated remediation, and reasoning about infrastructure.

---

### What Changed (or Was Upgraded) — Highlights & Technical Depth

Here are some of the major feature announcements that caught my eye, especially those intersecting with what I work on:

| Feature | What’s New / Changed | Why It Matters to Me |
| --- | --- | --- |
| **Terraform Stacks (GA)** | Terraform Stacks now is generally available for HCP Terraform. You can bundle multiple modules/configurations together into a “stack,” manage cross-dependencies automatically, and deploy with consistency across environments. [HashiCorp | An IBM Company+2HashiCorp | An IBM Company+2](https://www.hashicorp.com/en/blog/scale-infrastructure-with-new-terraform-and-packer-features-at-hashiconf-2025?utm_source=chatgpt.com) | As infrastructure grows in complexity, the mental overhead of orchestrating modules, dependencies, and environment splits becomes heavy. Stacks reduce that friction. For those building AI systems with multiple infra components (data pipelines, compute, storage, networking), this abstraction helps. |
| **Terraform Search / Bulk Import (Beta)** | A new search workflow lets you discover resources (e.g. AWS, Azure) and bulk-import them into Terraform using resource identity metadata. [HashiCorp | An IBM Company+2HashiCorp | An IBM Company+2](https://www.hashicorp.com/en/blog/scale-infrastructure-with-new-terraform-and-packer-features-at-hashiconf-2025?utm_source=chatgpt.com) | This closes one of the pain points I’ve seen: infrastructure drift or unmanaged resources that aren’t in IaC. Being able to “catch up” the environment more easily is a big win. |
| **Terraform MCP Server (Improved Authentication / AI integration)** | The MCP server (which lets Terraform interface with AI or tooling) now supports authenticating with HCP Terraform / Terraform Enterprise accounts. The idea is that devs/agents can work with context (private registries, existing workspaces) and reduce context-switching. [HashiCorp | An IBM Company+1](https://www.hashicorp.com/en/blog/scale-infrastructure-with-new-terraform-and-packer-features-at-hashiconf-2025?utm_source=chatgpt.com) | This is right in the alignment zone. If I have agents or GPTs that generate Terraform code, being able to authenticate, validate, and apply without jumping out of my environment is a huge productivity boost. |
| **Terraform Actions (Day‑2 operations)** | Actions allow you to codify Day 2 tasks (post-provisioning operations) as part of Terraform modules. These actions can attach to CRUD lifecycle events or be invoked manually. [HashiCorp | An IBM Company+1](https://www.hashicorp.com/en/blog/scale-infrastructure-with-new-terraform-and-packer-features-at-hashiconf-2025?utm_source=chatgpt.com) | This is a bridge toward full lifecycle automation. In AI systems, you often need post-deployment tasks (e.g. tweaks, bootstraps, alerts) that currently require manual scripts. Actions bring that back under IaC. |
| **Hold Your Own Key (HYOK) for Terraform Artifacts** | HYOK is now GA: users can keep control of their own encryption keys for Terraform artifacts (state, plan files) before sending them to HCP. [HashiCorp | An IBM Company](https://www.hashicorp.com/en/blog/scale-infrastructure-with-new-terraform-and-packer-features-at-hashiconf-2025?utm_source=chatgpt.com) | From a security & compliance lens, this is a strong step. For organizations with strict encryption and key policies (especially in regulated sectors), this gives more confidence. |
| **Pre‑written Sentinel policies for NIST SP 800‑53 (AWS)** | A large set of policies (350+ policy rules) to help enforce NIST controls across AWS accounts, out of the box. [HashiCorp | An IBM Company+2HashiCorp | An IBM Company+2](https://www.hashicorp.com/en/blog/scale-infrastructure-with-new-terraform-and-packer-features-at-hashiconf-2025?utm_source=chatgpt.com) | This is useful for teams who struggle to write policy-as-code from scratch. It lowers the entry barrier to governance, especially for enterprises. |
| **Packer: SBOM Storage & Package Visibility** | SBOM storage in HCP Packer is now GA. Also, in beta, you can view package-level metadata (name, version) of image artifacts from within Packer’s UI. [HashiCorp | An IBM Company+2HashiCorp | An IBM Company+2](https://www.hashicorp.com/en/blog/scale-infrastructure-with-new-terraform-and-packer-features-at-hashiconf-2025?utm_source=chatgpt.com) | For AI infra, where you might be building container images with dependencies, having visibility into package composition (and SBOMs) is critical for security and compliance auditing. |
| **Vault, Boundary, Radar Updates** | There were updates to their SLM (Security Lifecycle Management) tools: enhancements to Vault (version 1.21 upcoming), integration, and enhancements across Vault Radar, Boundary, etc. [Hawkdive+1](https://www.hawkdive.com/enhance-protection-with-vault-boundary-radar-at-hashiconf-2025/?utm_source=chatgpt.com) | These tools often form the backbone for secure systems. Any improvements here ripple across how you think about identity, secrets, access in your AI/cloud systems. |

---

### What I’m Most Excited About (and Will Be Experimenting With)

From all the announcements, here’s where my personal focus will be (and where I think the most interesting stuff will show up):

1. **Agentic Infrastructure & Infragraph**  
    This is the feature that feels like the “future” coming into view. If Infragraph can deliver a unified, real-time topology of infrastructure + security + ownership, that substrate becomes fertile ground for building intelligent agents that reason about infrastructure (not just doing).
    
2. **Terraform Actions + Day 2 Automation**  
    I’ve long seen the gap between provisioning and continuous operations as a tension point. With Actions, you can embed post-ops behaviors directly into your modules — something that often gets handled by external scripts. This keeps logic closer to infrastructure and more auditable.
    
3. **MCP Server AI Integration**  
    Bringing Terraform closer to AI/agent workflows (especially with proper authentication) is a big leap. In our AI paths, the smoother we can make the round-trip (generate → validate → apply), the less friction in building out real systems.
    
4. **Security / Compliance Enhancements (HYOK, Sentinel Policies, SBOMs, Vault upgrades)**  
    These are the foundational things you need if you want to build real AI systems in regulated environments. Getting visibility, being able to enforce rules, keys, audits — these aren’t sexy, but they’re non-negotiable.
    

---

### A Few Cautions & Considerations (From the Field)

* **Beta or GA?** Many of these features are new or in beta scope. Infragraph is a preview (private beta soon). [IBM Newsroom](https://newsroom.ibm.com/2025-09-25-HashiCorp-Previews-the-Future-of-Agentic-Infrastructure-Automation-with-Project-infragraph?utm_source=chatgpt.com) Some features may take time to stabilize before using them in production.
    
* **Context Gaps & Integration Cost**  
    The more capabilities you add (graph, agentic, audit), the more you have to ensure data consistency. In heterogeneous environments, bringing everything into one graph always reveals edge cases.
    
* **Agent Safety, Governance & Guardrails**  
    When giving agents the ability to act, mistakes can cascade. You’ll need guardrails, approvals, rollback strategies, etc.
    
* **Security in the “last mile”**  
    Features like HYOK, Sentinel, SBOMs etc. are helpful, but the real challenge is ensuring every AI/agent path (scripts, CLI, APIs) respects those controls. It’s the gaps you don’t see that bite.
    

---

### Final Thoughts & What I’ll Be Building

HashiConf 2025 wasn’t just about new features — it felt like an inflection point. The combination of infrastructure maturity + AI readiness is finally aligning. For me, the next few months will be about trying to build **agentic workflows over real infra**, seeing how well Infragraph (or graph-like systems) can support reasoning, and how far we can push automation while still maintaining security, observability, and trust.

If you’re experimenting in any of these spaces — especially around AI + infrastructure — I’d love to connect, exchange notes, or prototype together.

To everyone who built, designed, presented, or just showed up — thank you. The future of infrastructure is going to be more interesting than ever.