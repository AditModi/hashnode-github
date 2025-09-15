---
title: "Data Platform Blueprints: Launching Enterprise Data Workloads on EKS"
datePublished: Fri Jan 24 2025 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: cmfln95xr000102jvg5sihtap
slug: data-platform-blueprints-launching-enterprise-data-workloads-on-eks
tags: aws

---

When I first stumbled across the Data on EKS repository a few months ago, I'll be honest—I was skeptical. Another framework? Another set of "best practices" that would probably work great in demos but fall apart when you tried to run real workloads? But as someone who's spent the better part of three years building and breaking data platforms on Kubernetes, I couldn't resist taking a deeper look.

What I found changed how I think about running enterprise data workloads entirely.

## The Reality Check We All Need

Let's start with some uncomfortable truths. If you've tried building a data platform on Kubernetes from scratch, you know the pain. You spend weeks getting Apache Spark to play nicely with your cluster autoscaler. You wrestle with RBAC policies until 2 AM. You discover that your "production-ready" setup can't handle more than a few concurrent jobs without everything grinding to a halt.

I've been there. In 2023, while working on a large-scale analytics platform, our team burned through months trying to get EMR on EKS working reliably at scale. We had Spark jobs failing mysteriously, nodes not scaling properly, and monitoring that told us everything was fine right up until it wasn't.

The frustrating part? All the individual pieces worked. Kubernetes is incredible. Amazon EKS is rock-solid. Apache Spark is battle-tested. But putting them together in a way that actually works for enterprise data workloads? That's where things get messy.

## Enter Data on EKS: Not Your Typical Framework

This is where Data on EKS (DoEKS) becomes genuinely interesting. It's not trying to reinvent Kubernetes or create yet another abstraction layer. Instead, it's AWS engineers and the open-source community saying, "Here's how we've seen this work at scale, with all the sharp edges filed off".

The framework provides **battle-tested blueprints** for the entire data ecosystem: Apache Spark, Ray, Dask, Apache Kafka, Flink, Airflow, Argo Workflows, and more. But here's what makes it different—these aren't just YAML files someone threw together. These are configurations that have been tested at enterprise scale, with performance benchmarks, cost optimization patterns, and real-world operational experience baked in.

## The Architecture That Actually Makes Sense

![image](https://user-images.githubusercontent.com/19464259/208900860-a7ccdaeb-158d-4767-baad-fbc76388bc09.png align="left")

What impressed me most about DoEKS is how thoughtfully it approaches the complete data lifecycle. Most frameworks give you a way to deploy Spark and call it a day. DoEKS gives you the entire platform:

**Data Ingestion**: Whether you're using Amazon MSK, Kinesis, or running Kafka directly on EKS, the blueprints handle the networking, security, and scaling patterns that actually work in production.

**Processing Layer**: This is where DoEKS shines. The Spark configurations aren't just "it works on my laptop" setups. They include proper resource management, storage optimization (SSD vs. PVC with Amazon EBS), instance type selection for different workload patterns, and integration with Karpenter for intelligent autoscaling.

**Orchestration**: Integration with Apache Airflow, Argo Workflows, and Amazon MWAA isn't an afterthought—it's architected from the ground up to handle complex dependency management and workflow reliability.

**Analytics Layer**: Query engines like Trino, Presto, and ClickHouse are configured to actually perform at scale, not just demonstrate proof-of-concept queries.

## The Terraform and GitOps Reality

Here's where my experience really resonates with what DoEKS provides. The framework uses Terraform and ArgoCD templates, which means Infrastructure as Code from day one. But it's not just IaC—it's IaC that understands data workloads.

The blueprints handle things like:

* **Multi-tenancy patterns** that actually work for data teams
    
* **Security configurations** that don't break when you try to run distributed jobs
    
* **Monitoring and observability** integrated with Amazon Managed Prometheus and Grafana
    
* **Cost optimization** patterns, including spot instance usage and right-sizing recommendations
    

## Real Performance, Real Scale

One thing that sets DoEKS apart is the emphasis on benchmarking and performance validation. When I deployed the EMR on EKS with Karpenter blueprint, I wasn't just getting a cluster that started—I was getting configurations that had been tested with real workloads processing terabytes of data.

The difference is tangible. Jobs that used to take hours were completing in minutes. Autoscaling that used to be unpredictable became smooth and cost-effective. Most importantly, the platform became something the data team could actually rely on for production workloads.

## The Honest Assessment

Is Data on EKS perfect? No framework is. You still need to understand your workload patterns, tune for your specific use cases, and invest time in proper testing. But what DoEKS gives you is a foundation that actually works—not just in demos, but in production.

After deploying several DoEKS blueprints across different environments, I can say this: it eliminates months of trial and error. Instead of spending time figuring out why your Spark driver keeps getting killed or why your cluster autoscaler isn't scaling fast enough, you can focus on what actually matters—processing data and generating insights.

## Where to Start

If you're considering DoEKS for your organization, my recommendation is to start with the EMR on EKS blueprint. It's comprehensive, well-documented, and gives you a feel for how all the pieces fit together. The deployment takes about 20 minutes, and you'll have a production-grade data platform that can handle enterprise workloads from day one.

The framework isn't just about technology—it's about time. Time to value, time to production, and time you can spend on your actual data problems instead of fighting with infrastructure.

**Links:**

* [**Data on EKS Repository**](https://awslabs.github.io/data-on-eks/)
    
* [**Getting Started Guide**](https://github.com/awslabs/data-on-eks)
    
* [**EMR on EKS Blueprint**](https://github.com/awslabs/data-on-eks/tree/main/analytics/terraform/emr-eks-karpenter)