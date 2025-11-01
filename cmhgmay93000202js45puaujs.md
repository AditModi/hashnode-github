---
title: "Slurm on Amazon EKS with Slinky: The Best of HPC and Kubernetes"
datePublished: Sat Nov 01 2025 18:30:31 GMT+0000 (Coordinated Universal Time)
cuid: cmhgmay93000202js45puaujs
slug: slurm-on-amazon-eks-with-slinky-the-best-of-hpc-and-kubernetes
tags: aws

---

## Why this matters

Most organizations now run two kinds of compute: long-running batch training on GPUs and always-on services, but operating separate Slurm and Kubernetes clusters duplicates cost and effort while leaving capacity underutilized. Slinky lets you run Slurm natively inside EKS so both HPC-style training jobs and cloud-native services share the same GPU fleet with clear controls, strong isolation, and elastic scaling.​

## What you will learn

This post covers the Slinky architecture on EKS, how the operator and components fit together, benefits and tradeoffs, AWS-native alternatives, Kubernetes-native scheduler options, deployment steps, operations best practices, observability, and cost/performance considerations.​

---

## What is Slurm, and what is Slinky

Slurm is a widely used, open-source workload manager and job scheduler that allocates compute, launches and monitors parallel jobs, and manages queues with accounting at scale across CPUs and GPUs. Slinky is an open-source toolkit from SchedMD that integrates Slurm into Kubernetes with a Slurm Operator, a Slurm Bridge scheduling plugin, Prometheus exporters, Helm charts, and container images, bringing Slurm’s job-centric model to a Kubernetes runtime on EKS.​

## The core idea in simple terms

Think of EKS as the office building with shared utilities and Slurm as a specialized lab you install inside the building so both can share the same reliable infrastructure without duplicating floors or staff, which is exactly what Slinky enables on AWS. Users keep their familiar Slurm commands and workflows while platform teams keep Kubernetes operations, creating a converged platform that flexes capacity between services and batch without reconfiguring clusters.​

---

## Reference architecture on EKS

AWS’s blueprint deploys Slurm components as pods on an EKS cluster with clear separation of control and data planes for reliability and performance. The slurmctld controller and core services run on general-purpose nodes, while slurmd worker pods run on accelerated GPU nodes to execute ML and HPC jobs efficiently. High-performance shared storage is delivered via Amazon FSx for Lustre mounted to slurmd pods for fast dataset access, and an AWS Network Load Balancer fronts login pods for SSH and API access to the cluster. A Slurm REST API powers the NodeSet controller and a metrics exporter that feeds Prometheus and can drive autoscaling via KEDA and Amazon Managed Service for Prometheus and Amazon Managed Grafana.​

---

## How Slinky works end-to-end

Researchers connect to login pods and submit jobs with familiar Slurm commands like sbatch, srun, and salloc without learning new tooling or APIs. The operator reconciles desired state, drives lifecycle for Slurm clusters and NodeSets, and scales compute pods while draining nodes gracefully so running jobs finish before scale-in or upgrades. The exporter exposes job, node, and partition metrics to Prometheus so autoscalers can react to pending jobs, allowing scale to or from zero replicas for cost-efficient elasticity on GPUs. The SchedMD toolkit also includes the Slurm Bridge plugin that enables converged scheduling across pods and Slurm jobs as Slinky matures alongside Slurm 25.05 and later releases.​

---

## Key components in the cluster

The controller pod runs slurmctld for central job and resource management to ensure deterministic scheduling and queue control at cluster scale. The accounting pod runs slurmdbd and persists records in MariaDB, which can be hosted in-cluster or externally via Amazon RDS for operational separation and durability. Login pods expose SSH and Slurm client access, with SACKD handling auth to the Slurm control plane and optional SSSD linking to directory services like AWS Directory Service via LDAP for enterprise identity. Worker node pods run slurmd on GPU nodes and can be partitioned to map to hardware or policy constraints while supporting draining for safe scale downs, upgrades, and reservation-based operations.​

---

## Benefits of Slurm on EKS

You get a unified, converged infrastructure where Kubernetes services and Slurm training jobs share the same GPU fleet while each team keeps its tools and mental models, reducing duplicated ops and stranded capacity. Autoscaling can expand or shrink compute based on queue depth or custom metrics so GPU spend aligns tightly with workload demand across business and off-hours patterns, improving efficiency and throughput. Containerized Slurm components bring consistency across environments with custom images for CUDA, NCCL, and frameworks, which simplifies reproducibility and dependency control for ML teams. Early evaluations also highlight backfilling and elasticity benefits that reclaim idle windows for AI training without impacting daytime serving loads.​

---

## When Slurm on EKS is the right choice

Choose Slinky on EKS if you run both real-time services and batch AI/HPC jobs and want one shared GPU platform with strong guardrails and familiar user experiences for researchers and platform engineers. It is especially compelling for orgs standardizing on Kubernetes that need Slurm’s deterministic scheduling for long-running training but Kubernetes’ agility for APIs and microservices on the same cluster.​

---

## AWS-native alternatives for Slurm

AWS ParallelCluster offers an AWS-supported open-source path to stand up self-managed Slurm clusters with deep customization and IaC, which is ideal when you want a pure HPC cluster with full control. AWS Parallel Computing Service (PCS) provides a managed Slurm experience that reduces controller and accounting ops overhead for teams that want turnkey HPC without managing Slurm internals. Amazon SageMaker HyperPod targets massive foundation model training with persistent, self-healing clusters, job auto-resume, multi-head nodes, and close integration with AWS accelerators, and it can incorporate Slurm orchestration when needed.​

---

## Kubernetes-native scheduler alternatives

If you are all-in on Kubernetes and prefer not to run Slurm, Volcano provides advanced batch scheduling with gang scheduling, GPU partitioning, and topology-aware placement for large-scale ML and HPC jobs natively. Apache YuniKorn focuses on multi-tenancy and fairness with hierarchical quotas and preemption for shared clusters where queue-based fairness is key across many ML teams. Kueue complements the default scheduler with job-level queuing and admission so you can add batch semantics incrementally without replacing kube-scheduler binaries in production.​

---

## High-level deployment blueprint on EKS

Provision an Amazon EKS cluster with separate node groups for general-purpose control components and GPU-accelerated workers to isolate reliability domains and optimize cost. Deploy the Slinky Slurm Operator and cluster CRDs via Helm, then define SlurmCluster and NodeSet resources that map partitions to GPU instance classes with taints and selectors. Mount Amazon FSx for Lustre to slurmd pods for high-throughput, low-latency access to datasets, model checkpoints, and shared artifacts used by distributed training. Expose login pods behind an AWS Network Load Balancer for SSH access and API interaction, and configure identity via SACKD and optionally SSSD for enterprise directory integration. Enable the Slurm exporter and wire metrics into Amazon Managed Service for Prometheus and Amazon Managed Grafana, and integrate KEDA or HPAs to scale compute pods based on queue and custom metrics.​

---

## Operations and scaling best practices on EKS

Use Kubernetes autoscaling alongside Karpenter or the Cluster Autoscaler to scale GPU nodes just-in-time, and consider On-Demand Capacity Reservations or Capacity Blocks for ML in constrained GPU markets. Follow EKS reliability practices such as avoiding singleton pods, running multiple replicas for critical controllers, and using health probes, HPA, and VPA where appropriate for robust control-plane workloads. Apply EKS security practices including private cluster endpoints, least-privilege IAM roles for service accounts, and in-cluster network policies enforced by your CNI like Cilium or Calico. Let the operator drain slurmd pods before termination during scale-in and upgrades so running jobs complete gracefully without user impact.​

---

## Observability and autoscaling

Enable the Slurm metrics exporter to publish job, node, and partition metrics into Prometheus so you can visualize queue health and trigger autoscaling policies reliably from a single source of truth. Use Amazon Managed Service for Prometheus with Amazon Managed Grafana to standardize dashboards across platform and research teams without managing storage and query layers yourself. Combine KEDA with exported metrics like pending jobs per partition to scale worker node pods to or from zero replicas for tighter GPU utilization.​

---

## Example job workflow

A researcher SSHes to a login pod and submits a training job with sbatch that requests four A100 GPUs, a partition, and a maximum runtime as usual in Slurm, preserving existing practices. The operator ensures enough slurmd pods are available or scales them up while node autoscaling provisions GPUs, and jobs start once resources are ready and policies admit them. As daytime API traffic rises, Kubernetes scales service pods while Slurm training can scale down or backfill overnight, rebalancing the same GPU pool without manual reconfiguration.​

---

## Cost and performance considerations

Converging HPC and services on one EKS fleet reduces duplicate GPU capacity and can materially lower TCO while improving overall utilization and throughput for both workloads. FSx for Lustre accelerates data access for training so GPUs spend time on compute instead of waiting on I/O, which lifts effective performance and can reduce wall-clock costs per run. Capacity plans can blend on-demand, reservations, and time-of-day policies so Slurm training expands after-hours and services expand during peak traffic to balance spend and SLA risk.​

---

## Slinky maturity and roadmap notes

The Slinky operator has iterated from v0.1.0 to v0.3.0 in 2024–2025, and the Slurm Bridge plugin aligns with Slurm 25.05 to extend converged scheduling of pods and jobs on Kubernetes. The project includes a Go client for the Slurm REST API and a Prometheus exporter, plus Helm charts and container images to streamline cluster setup on EKS and other platforms.​

---

## When to choose each path

Pick Slurm on EKS with Slinky when you need deterministic batch scheduling with Slurm and elastic services with Kubernetes on one standardized platform that both teams can operate confidently. Pick AWS ParallelCluster or AWS PCS when you want pure Slurm clusters and do not need to colocate services, and pick SageMaker HyperPod when training FMs at very large scale with managed resilience and self-healing. Pick Volcano, YuniKorn, or Kueue if you prefer Kubernetes-native batch semantics without Slurm, matching each tool to your needs for gang scheduling, fairness, or incremental adoption.​

---

## Quick start checklist

* Create an EKS cluster and GPU node groups with Karpenter or Cluster Autoscaler for elastic capacity management.​
    
* Install the Slinky Slurm Operator and CRDs via Helm, and define SlurmCluster and NodeSet resources mapping partitions to GPU pools with taints and selectors.​
    
* Mount FSx for Lustre to worker pods, configure login pods behind an NLB, and enable SACKD and optional SSSD for access control and directory integration.​
    
* Enable the Slurm exporter to Amazon Managed Service for Prometheus, deploy dashboards in Amazon Managed Grafana, and configure KEDA or HPAs on queue metrics to scale compute.​
    
* Validate drain behavior, upgrade flows, and capacity reservations, and document runbooks for daytime service dominance and overnight training dominance patterns.​
    

---

## FAQ

* Do users need to learn Kubernetes to use Slinky on EKS? No, they keep using sbatch/srun/salloc on login pods while the platform team manages Kubernetes under the hood.​
    
* Can Slinky schedule GPUs and multi-node MPI jobs? Yes, Slurm remains the scheduler of record and maps partitions and nodes to Kubernetes constructs while retaining GPU- and MPI-aware behaviors.​
    
* How does observability work? A Slurm REST API plus an exporter publish job and node metrics into Prometheus, which you can visualize in Amazon Managed Grafana and use for autoscaling.​
    
* What about security and compliance? Use private endpoints, least-privilege IAM for service accounts, and network policies with a CNI like Cilium or Calico, and centralize identity via SSSD if needed.​
    
* How mature is Slinky? The operator has released multiple versions and the Slurm Bridge aligns with Slurm 25.05, with supporting client libraries, exporters, and Helm charts maintained by SchedMD.​
    

---

## Closing thoughts

Running Slurm on EKS with Slinky gives research and platform teams a unified, elastic, and observable platform that preserves familiar workflows while removing the operational penalty of separate clusters. If your organization spans APIs, microservices, and GPU-heavy training, converging on EKS with Slinky is a pragmatic way to increase utilization, reduce costs, and simplify day-two operations without forcing cultural or tooling resets.