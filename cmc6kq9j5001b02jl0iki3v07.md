---
title: "🔥 Building an AWS Solution Architect Research Agent with Strands: From Design to Deployment"
datePublished: Sat Jun 21 2025 18:30:12 GMT+0000 (Coordinated Universal Time)
cuid: cmc6kq9j5001b02jl0iki3v07
slug: building-an-aws-solution-architect-research-agent-with-strands-from-design-to-deployment
tags: aws, strands-agents

---

## **🚀 The Solution Architect's New Superpower**

As an AWS Solution Architect, I constantly face complex design challenges:  
*"How do I optimize this serverless architecture?" "What's the most cost-effective ECS deployment pattern?" "Is this DynamoDB schema truly web-scale?"*

Traditional research methods meant hours of digging through docs, whitepapers, and reinvented wheels. Then I discovered **Strands Agents SDK** - and built an AI partner that transforms days of research into minutes of actionable insights. Here's how I created an agent that:

* Decomposes complex architectural challenges into research sub-tasks
    
* Cross-references AWS docs, Well-Architected Framework, and real-world patterns
    
* Generates architecture diagrams through MCP integration
    
* Recommends service combinations with cost/performance tradeoffs
    
* Self-validates against AWS best practices
    

All with **under 100 lines of code** and no hardcoded workflows.

---

## **🧠 Why Strands Becomes an Architect's Best Friend**

Traditional agent frameworks force rigid decision trees. Strands embraces the chaos of real-world cloud architecture through:

**Model-Driven Flexibility**  
The LLM dynamically adapts to novel design challenges - no pre-defined "if-else" chains for infinite AWS service combinations.

**Native AWS Tool Integration**  
Direct access to:

* AWS CLI for live environment checks
    
* AWS Documentation MCP Server
    
* Architecture Diagram Generator MCP
    
* Cost Explorer API
    
* Security Hub findings
    

**Self-Reflective Validation**  
The agent critiques its own recommendations:  
*"Wait - does this S3 cross-region replication strategy comply with GDPR? Let me check the Data Sovereignty docs..."*

---

## **🛠️ Building the Ultimate Architecture Assistant**

## **1\. Define Specialized Tools**

```python
from strands.tools import aws_cli, waf_analyzer
from strands.tools.mcp import MCPClient

# Connect to AWS Documentation MCP Server
aws_docs_client = MCPClient(...)
aws_docs = aws_docs_client.list_tools_sync()

# Connect to Architecture Diagram MCP
arch_diagram_client = MCPClient(...)
diagram_tools = arch_diagram_client.list_tools_sync()

# Custom tool for cost estimation
@tool
def estimate_cost(service_config: dict) -> dict:
    """Predict AWS costs using historical data and pricing API"""
    return calculate(service_config)
```

## **2\. Craft the Architect's Prompt**

```python
system_prompt = """You are an AWS Certified Solutions Architect with 10+ years experience.

When given a design challenge:
1. Break into sub-problems (scaling, security, cost, reliability, performance)
2. Research using AWS docs, whitepapers, and Well-Architected Framework
3. Generate architecture diagrams for key components
4. Validate against 6 Pillars
5. Recommend 3 options with tradeoffs
6. Calculate estimated costs
7. Check for anti-patterns

Always:
- Cite AWS docs using [doc-link] notation
- Flag GDPR/HIPAA considerations
- Compare serverless vs containers vs EC2
- Suggest monitoring/alerting strategy"""
```

## **3\. Create the Agent**

```python
from strands.models import BedrockModel
from strands import Agent

model = BedrockModel(
    model_id="us.amazon.nova-pro-v1:0",  # Best for technical reasoning
    region_name="us-east-1",
    model_config={"temperature": 0.3}
)

arch_agent = Agent(
    tools=[aws_cli, waf_analyzer, estimate_cost] + aws_docs + diagram_tools,
    model=model,
    system_prompt=system_prompt
)
```

---

## **🔄 The Agentic Design Process in Action**

**Sample Query:**  
*"Design a real-time analytics pipeline for 100K events/sec with &lt;100ms latency"*

**Agent Workflow:**

1. **Decompose**
    
    * Ingestion → Processing → Storage → Visualization
        
    * Cross-cutting concerns: Security, Monitoring, Cost
        
2. **Research**
    
    * Compare Kinesis vs MSK vs SQS+SNS
        
    * Analyze Lambda vs Fargate vs KDA
        
    * Review Real-time Analytics Lens whitepaper
        
3. **Generate**
    
    ```python
    response = arch_agent(query)
    print(response)
    ```
    

**Example Output:**

```plaintext
Recommendation 1: Serverless Real-Time Pipeline  
- Kinesis Data Streams (On-demand) → Lambda (ARM) → Timestream  
Cost: $12.3K/month | Latency: 68ms ±15ms  
[Architecture Diagram: s3://diagrams/real-time-serverless.png]  
*Best for: Variable workloads, minimal ops overhead*

Recommendation 2: Managed Streaming  
- MSK (Kafka) → EKS (Fargate) → Redshift Streaming  
Cost: $18.7K/month | Latency: 83ms ±8ms  
[Whitepaper Reference: AWS Streaming Best Practices v3.2]  
*Best for: Existing Kafka shops, exactly-once processing*

Security Note:  
- All options encrypt data in transit+rest (KMS CMKs)  
- Consider VPC endpoints for private connectivity [doc-link]  

Validation:  
- Passes Well-Architected review  
- Anti-pattern avoided: Lambda function timeouts <5s  
```

---

## **💡 Key Architectural Benefits**

**1\. Context-Aware Research**  
The agent understands solution context:  
*"User mentioned healthcare data → automatically check HIPAA guides and suggest encrypted Glue Data Catalog"*

**2\. Live Environment Integration**  
Via AWS CLI tool:

```python
@tool
def check_existing_resources(service: str) -> list:
    """Use AWS CLI to find existing resources"""
    return subprocess.check_output(f"aws {service} list-resources")
```

*"Found existing VPC with /16 CIDR - recommend subnetting strategy"*

**3\. Diagram → Code Generation**  
Using MCP diagram tool + CDK integration:

```python
# After architecture validation
diagram_tool.generate_cdk_template(response['diagram_id'])
```

**4\. Cost Optimization Engine**  
Custom tool compares historical patterns:  
*"This S3 Intelligent-Tiering config saved 38% for similar workloads"*

---

## **🏗️ Production-Ready Architecture**

**Deployment Options**

```plaintext
graph TB
  Agent[Strands Agent] -->|gRPC| Lambda[Lambda Worker]
  Agent -->|WebSocket| Frontend[Architect Portal]
  Lambda --> Bedrock[Amazon Bedrock]
  Lambda --> MCP[MCP Servers]
  Lambda --> |CDK| Pipeline[Deployment Pipeline]
```

**Key Components:**

* **Multi-Agent Collaboration:** Specialized agents for security, cost, and reliability
    
* **State Management:** Saves context across sessions using DynamoDB
    
* **Observability:** X-Ray tracing for every design decision path
    

---

## **🚨 Lessons from the Trenches**

**Challenge:** Agent sometimes suggests deprecated services  
**Solution:** Added tool to check AWS Service Health Dashboard

**Challenge:** Complex pricing calculations  
**Solution:** Hybrid approach - LLM for patterns, Cost Explorer API for exact numbers

**Pro Tip:**

```python
# Force reflection on recent service updates
system_prompt += "\nAlways check https://aws.amazon.com/new/ for updates <6mo old"
```

---

## **🚀 Where to Take This Next**

1. **Multi-Agent Design Reviews**  
    Connect security/cost/reliability agents for automated WAF reviews
    
2. **Live Environment Probes**  
    "This design requires 3 AZs - checking target account limits..."
    
3. **GitHub Integration**  
    Auto-generate CDK code in PR comments
    
4. **Client-Specific Learning**  
    "Your team prefers Terraform → converting recommendation to HCL"
    

---

## **⚡ Get Started Today**

**For Solution Architects:**

```bash
pip install strands-agents
git clone https://github.com/aws-samples/solution-architect-agent
```

**For Teams:**

1. Deploy MCP servers for internal docs/patterns
    
2. Add custom tools for org-specific constraints
    
3. Bake into your design review process
    

---

*"The best architect is the one who knows when to stand on the shoulders of LLMs"*