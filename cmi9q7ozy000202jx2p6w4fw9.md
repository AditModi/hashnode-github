---
title: "Monitoring Network Performance on Amazon EKS Using AWS-Managed Open-Source Tools"
datePublished: Tue Nov 18 2025 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: cmi9q7ozy000202jx2p6w4fw9
slug: monitoring-network-performance-on-amazon-eks-using-aws-managed-open-source-tools
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1763781776850/b6883c59-e242-4e5f-a358-50acdde557f7.png
tags: aws

---

As your microservices grow on EKS, one of the hardest things to see clearly is how traffic is flowing between pods, which services are talking, and where latency or packet drops could be happening. VPC Flow Logs help somewhat, but they don’t know **which Kubernetes pod** is behind a connection — that makes troubleshooting real-world issues messy.

AWS’s new **Container Network Observability** in EKS gives you exactly that visibility — enriched with Kubernetes metadata + exported in open-metrics format to tools you already love (or want to use).

---

### What’s New (and Why It Helps)

* **Service Map**: Visualize which services talk to which, how pods communicate, and get an end-to-end map of traffic flows inside your cluster.
    
* **Network Flow Analysis**: Deep, flow-level insights like retransmissions (RT/RTO), flow latency, packet counts, and byte transfers — all annotated with pod, namespace, and service metadata.
    
* **Open Metrics Export**: The Flow Monitor agent (running on every node) emits system-level and flow-level metrics in Prometheus-compatible format.
    
* **Managed Integration**: You can push these metrics to **Amazon Managed Service for Prometheus** + visualize them in **Amazon Managed Grafana** dashboards — or use any Prometheus-based system.
    

---

### Analogy: Turning on a Traffic Radar for Your Kubernetes Cluster

Think of your EKS cluster like a city with many neighborhoods (namespaces) and buildings (pods). Until now, you only saw how many cars (packets) left the city, but not how they moved inside. With Container Network Observability:

* You get a **bird’s-eye traffic map** — which “buildings” talk to each other, and how often.
    
* You can zoom in on **congested roads** or “streets” (flows) and see how many cars are dropping.
    
* You collect this on-the-ground data in real-time and send it to a **traffic-control center** (Prometheus + Grafana) you manage (or AWS manages) — letting you set up alerts, dashboards, or even automated reactions.
    

---

### How to Use It — Actionable Steps

1. **Enable Network Observability**
    
    * In your EKS console or via IaC, turn on the *Network Flow Monitor* add-on.
        
2. **Deploy the Flow Monitor Agent**
    
    * It runs as a DaemonSet on each node, exporting metrics on port 9101 in open metrics format.
        
3. **Set up a Prometheus Workspace (Managed)**
    
    * Use Amazon Managed Service for Prometheus to collect the metrics without managing scrape agents yourself.
        
4. **Visualize with Grafana**
    
    * Use Amazon Managed Grafana to import dashboards (or build your own) to view flows, service maps, and trends.
        
5. **Correlate & Alert**
    
    * Track metrics like “bytes transferred,” “flows open,” “retransmissions,” or “connection limit exceeded” per pod or service.
        
    * Set up alerts or dashboards to catch anomalies, latency issues, or unexpected traffic.
        

---

### Why You Should Care (Real-World Impact)

* **Faster Troubleshooting**: Instead of guessing which service or pod is the bottleneck, you have real data — hit-points in the network graph, packet retrials, or network drops.
    
* **Better Capacity Planning**: See which pods or namespaces have the highest network usage, then size instances or scale clusters appropriately.
    
* **Security & Compliance**: Monitor unexpected east-west traffic, potentially spot compromised pods, or detect unusual connection patterns.
    
* **Cost Efficiency + Operational Clarity**: Eliminate blind spots — unmonitored internal traffic often hides performance inefficiencies, misconfigurations, or overprovisioning.
    

---

### Things to Keep in Mind

* **Region Support**: Make sure EKS + Network Flow Monitor is available in your AWS Region.
    
* **Data Volume & Cost**: Exporting very high-cardinality flow data may generate a lot of metrics. Tune what you scrape or alert on.
    
* **Agent Resource Use**: The Flow Monitor runs on every node — check resource usage to ensure it doesn’t interfere with your workloads.
    

---

## TL;DR

AWS now supports **Kubernetes-aware network observability** in EKS. You can capture pod-level flow data, export it via open metrics, and visualize it seamlessly using managed Prometheus and Grafana — giving you the full picture of how your microservices talk, where the bottlenecks are, and how to act fast.

---

## Part of *Road to re:Invent: Cloud Concepts Made Simple*

This series breaks down AWS updates in:

* Simple language
    
* Practical context
    
* With guidance you can use immediately
    

More updates coming as launches roll in.  
Stay tuned. 👀