---
marp: true
theme: custom-default
footer: 'https://chris-ayers.com'
---

![bg right fit](./img/arch.png)

# Reslient by Design

## Ensuring Azure Resiliency

## Chris Ayers

---

![bg left:40%](./img/portrait.png)

## Chris Ayers

_Senior Software Engineer_  
_Azure CXP AzRel_  
_Microsoft_

<i class="fa-brands fa-bluesky"></i> BlueSky: [@chris-ayers.com](https://bsky.app/profile/chris-ayers.com)
<i class="fa-brands fa-linkedin"></i> LinkedIn: - [chris\-l\-ayers](https://linkedin.com/in/chris-l-ayers/)
<i class="fa fa-window-maximize"></i> Blog: [https://chris-ayers\.com/](https://chris-ayers.com/)
<i class="fa-brands fa-github"></i> GitHub: [Codebytes](https://github.com/codebytes)
<i class="fa-brands fa-mastodon"></i> Mastodon: [@Chrisayers@hachyderm.io](https://hachyderm.io/@Chrisayers)
~~<i class="fa-brands fa-twitter"></i> Twitter: @Chris_L_Ayers~~

---

# Agenda Items

<div class="columns">
<div>

- **Introduction & Why Reliability Matters**
- **Reliability Fundamentals**
- **Design Principles**  
- **Trade-Offs with Other Pillars**  

</div>
<div>

- **Failure Mode Analysis (FMA)**
- **Single Points of Failure**
- **Azure Regions & Availability Zones**  
- **Load Testing & Chaos Engineering**
- **Reference Architectures & Next Steps**  

</div>
</div>

---

# Reliability

![bg right:65%](./img/sb-reliability.jpg)

---

# Why Reliability Matters

> A system is considered "reliable" if it can consistently serve users under normal or abnormal conditions.

- Reliability in distributed systems involves:
  - **Consistent performance** despite failures.
  - **Graceful degradation** when certain components become unavailable.
  - **Rapid recovery** within acceptable time limits.

---

# Understanding Reliability and Resiliency

- Failures are **inevitable** in distributed systems
- Workloads must **detect**, **withstand**, and **recover** from failures within _**acceptable**_ timeframes
- Ensuring availability for users to access workloads as promised
- Failures impact _**revenue**_, _**reputation**_, and _**customer trust**_

---

# <!-- fit --> FAILURE IS ALWAYS AN OPTION

---

## Financial Impact of Downtime

- **Revenue Loss**: Downtime can cost businesses over $1 million per hour, especially for e-commerce and online services.
- **Increased Expenses**: Includes emergency maintenance, staff overtime, and potential penalties for SLA breaches.
- **Legal Liabilities**: Potential lawsuits from customers or clients affected by downtime.
- **Insurance Challenges**: Downtime can affect insurance coverage and premiums.
- **Operational Costs**: Additional costs for restoring systems, data recovery, and preventive measures to avoid future downtime.

---

## Real-World Downtime Costs

- **77 hours** (≈3 days) median annual downtime for high-impact outages
- **$146 million** median annual cost of outages
- **30%** of engineering time spent addressing disruptions
  - Equals **12 hours** per engineer weekly (based on 40-hour week)

> Source: New Relic 2024 Observability Forecast

---

![bg fit](./img/reliability.jpg)

---

<style scoped>
table { display: table; }
tr { display: table-row; }
td, th { display: table-cell; }
table {
  width: 100%;
}
</style>

# Reliability Levels

| Level       | Monthly Downtime  | Annual Downtime | Cost  |
| ----------- | ----------------- | --------------- | ----- |
| **99.9%**   | 43.8 minutes      | 8.75 hours      | $     |
| **99.95%**  | 21.9 minutes      | 4.375 hours     | $$    |
| **99.99%**  | 4.38 minutes      | 52.6 minutes    | $$$   |
| **99.995%** | 2.19 minutes      | 26.3 minutes    | $$$$  |
| **99.999%** | 26 seconds        | 5.25 minutes    | $$$$$ |

>[https://uptime.is/five-nines](https://uptime.is/five-nines)

---

![bg fit](./img/one-does-not-simply.jpg)

---

# How Do We Measure Reliability?

| SLI | SLO | SLA |
|---|---|---|
| Service Level Indicator | Service Level Objective | Service Level Agreement |
| Metrics for service quality (e.g., error rate) | Targets (e.g., 99.9% uptime/month) | Contracts with set metrics and penalties |
| Assess service quality | Define service quality goals | Formalize commitments and consequences |

---

![SLA Pyramid](./img/sla-slo-sli.png)

---

# Understanding RPOs and RTOs

- **Recovery Time Objective (RTO)**: Maximum acceptable downtime before services must be restored
- **Recovery Point Objective (RPO)**: Maximum acceptable data loss during a disruption

<div align="center">

![width:900px](./img/rpo-rto.drawio.png)

</div>

---

# Azure Cloud Architecture

## Implementing Reliability

---

# Azure-Customer Shared Responsibility Model

![width:850px center](./img/shared-responsibility.svg)

---

# Azure Well-Architected Framework

- Provides best practices and guidance for building high-quality Azure solutions.
- Ensures workloads are reliable, secure, efficient, and cost-effective.
- [Azure Well-Architected Framework](https://learn.microsoft.com/en-us/azure/well-architected/)
 
![bg right fit](img/waf.png)

---

![bg fit](./img/well-architected-hub.png)

---

## Microsoft Azure Well-Architected Framework Pillars

| Reliability                        | Security                            | Cost Optimization                  | Operational Excellence                  | Performance Efficiency                  |
|------------------------------------|-------------------------------------|------------------------------------|-----------------------------------------|-----------------------------------------|
| Resiliency, availability, recovery | Protect data, detect threats, mitigate risks | Budgeting, reducing waste, efficiency | Observability, DevOps practices, safe deployments | Scalability, load testing, performance monitoring |

---

# Design Principles for Reliable Workloads

---

# Design for Business Requirements

<div class="columns">
<div>

- Gather business requirements focusing on the workload's intended utility
- Cover user experience, data, workflows, and unique constraints or sensitivities
- Clearly state expectations and confirm goals are feasible and documented

</div>
<div>

| Approach           | Description                                                     |
|--------------------|-----------------------------------------------------------------|
| Quantify Success   | Set targets for components, flows, and the system              |
| Compliance         | Ensure predictable outcomes for sensitive flows                |
| Platform Commitments | Understand SLAs, limits, and regional constraints            |
| Dependencies       | Track dependencies and implement resilient design patterns     |

</div>
</div>

---

# Design for Resilience

<div class="columns">
<div>

- Continue operating despite failures
- Prepare for outages and resource shortages
- Enable graceful degradation

</div>
<div>

| Strategy                | Implementation                     |
|-------------------------|-----------------------------------|
| Critical vs. Degradable | Prioritize by impact              |
| Failure Points          | Design for error handling         |
| Self-Preservation       | Isolate faults, mitigate failures |
| Scalability             | Handle spikes and regional issues |
| Redundancy              | Eliminate single points of failure|

</div>
</div>

---

# Design for Recovery

<div class="columns">
<div>

- Recover gracefully from all failure types
- Plan for disaster even in resilient systems
- Prepare for data layer corruption

</div>
<div>

| Strategy | Implementation |
|----------|----------------|
| Recovery Plans | Component coverage with regular drills |
| Data Repair | Trusted backups with immutable copies |
| Self-Healing | Automated detection and remediation |
| Ephemeral Units | Side-by-side deployments with zero downtime |

</div>
</div>

---

# Design for Operations

<div class="columns">
<div>

- Shift left to anticipate failures early
- Test failures in development
- Ensure cross-team visibility
- Use observability for rapid remediation

</div>
<div>

| Strategy | Implementation |
|----------|----------------|
| Observable Systems | Aggregate telemetry for holistic health views |
| Failure Simulation | Validate recovery metrics with realistic tests |
| Automation | Minimize human error and ensure consistency |
| Continuous Learning | Improve from real production incidents |
| Proactive Monitoring | Prioritize alerts for active failures |

</div>
</div>

---

# Keep It Simple

- Avoid overengineering architecture, code, and operations
- Simplicity reduces inefficiencies and misconfigurations
- Maintain a balanced approach to avoid single points of failure

---

# Trade-Offs

![w:960px center](img/tug-of-war.png)

---

# Trade-Offs in Applying the Framework

- Align trade-offs with business priorities
- Use scenario planning to assess impacts
- Continuously iterate and monitor performance

---

![bg](img/tradeoff-cost.jpg)

---

# Reliability Trade-Offs with Other Pillars

| Pillar | Prioritizing Reliability | Prioritizing Other Pillar |
|--------|--------------------------|---------------------------|
| **Cost Optimization** | • **Implement Redundancy** (multi-region)<br>• **Extensive Monitoring & Drills**<br>• **Over-Provision** for unexpected spikes | • **Single-Region** setups with minimal redundancy<br>• **Reduce Operational Costs** (simpler monitoring)<br>• **Right-Size** resources to save expenses |
| **Security** | • **Replication & Backups** expand attack surface<br>• **Faster Recovery** may bypass security controls<br>• **Delay Patches** to reduce downtime | • **Minimize Attack Surface**<br>• **Strict Controls** remain active under stress<br>• **Frequent Patching** even with disruptions |

---

# Reliability Trade-Offs with Other Pillars (Continued)

| Pillar | Prioritizing Reliability | Prioritizing Other Pillar |
|--------|--------------------------|---------------------------|
| **Operational Excellence** | • **Complex Architectures** (multi-region, failover)<br>• **Extensive Documentation & Training** for DR<br>• **Continuous Testing** adds overhead | • **Simple Architecture**<br>• **Streamlined Knowledge Base**<br>• **Less Overhead** in daily operations |
| **Performance Efficiency** | • **Redundant Writes & Replication** add latency<br>• **Failover Logic** can slow requests<br>• **Over-Provision** may reduce performance tuning | • **Lean Deployments** maximize throughput<br>• **On-Demand Scaling** with minimal buffer<br>• **Focus on Speed** over recovery measures |

---

# Failure Mode Analysis (FMA)

<div class="columns">
<div>

## Proactive Identification

- Recognize potential weaknesses before outages occur
- Use checklists, post-mortems, and dependency mapping
- Identify high-impact risks and allocate resources efficiently
- Distinguish between rare/high-impact and frequent/low-impact failures

</div>
<div>

## Effective Mitigation

- Architect for graceful degradation and fallback strategies
- Enhance observability for quick detection
- Implement immediate failover and data replication
- Document potential risks to guide resolution

</div>
</div>

---

# Single Points of Failure (SPOFs)

<div class="columns">
<div>

## Understanding SPOFs

- A component whose failure disrupts the entire workload
- Critical to identify for improving reliability and availability
- Can exist in infrastructure, code, data, or operations

</div>
<div>

## Mitigation Strategies

- **Redundancy**: Duplicate critical components or services
- **Failover Mechanisms**: Enable seamless switching to backups
- **Load Balancing**: Distribute traffic across instances
- **Design Reviews**: Regularly identify and eliminate SPOFs

</div>
</div>

---

# Failure Examples

[Failure Examples](https://learn.microsoft.com/en-us/azure/well-architected/reliability/failure-mode-analysis#example)

---

# Azure Regions and Regional Strategy

<div class="columns">
<div>

## Azure Regions

- 60+ regions worldwide with defined data residency boundaries
- Datacenters connected by low-latency, high-capacity networks
- Some regions are sovereign clouds (e.g., Azure Government)
- [Explore Regions](https://datacenters.microsoft.com/globe/explore/)

</div>
<div>

## Region Pairs & Alternatives

- **Region Pairs**: Built-in geo-redundancy (e.g., East US ⟷ West US)
  - Support **sequential updates** and **prioritized recovery**
  - Note: Pairing doesn't guarantee automatic failover
- **Non-paired Regions**: Rely on availability zones
  - Services can replicate across any region combination
  - Choose regions based on business needs and compliance

</div>
</div>

---

# Active-Active vs. Active-Passive

<div class="columns">
<div>

**Active-Active**: Multiple instances process requests simultaneously.
![width:500px center](img/active-active.png)

</div>
<div>

**Active-Passive**: Primary instance processes traffic; secondary is on standby.
![width:500px center](img/active-passive.png)

</div>
</div>

---

# Azure Availability Zones

![width:800px center](img/az-diagram.png)

---

# Azure Availability Zones Infrastructure

- AZs are physically separate datacenters within an Azure region
- Each zone has independent power, cooling, and networking
- Low-latency (<2ms) connections reduce local outage impact
- Not all regions have AZs; verify availability
- [View Region Support](https://learn.microsoft.com/en-us/azure/reliability/availability-zones-region-support)  

---

# Types of Availability Zone Support

- **Zone-Redundant**: Automatically distributed across zones
- **Zonal**: Pinned to a specific zone
- **Zone-Resilient**: Designed to survive zone failures
- **Non-Zonal (Regional)**: No zone-specific deployment
- [View Services Support](https://learn.microsoft.com/en-us/azure/reliability/availability-zones-service-support)

---

# Zonal Resources

- Pinned to a specific availability zone.
- Combine multiple zonal deployments for high reliability.

![bg right fit](./img/zonal-1.png)

---

# Zonal Resources

**Customer Responsibilities**:

- Deploy and manage resources in each availability zone
- Configure and manage data replication
- Use a load balancer to distribute requests
- Choose active-passive or active-active models
- Handle failover when an AZ becomes unavailable

![bg right fit](./img/zonal-3.png)

---

# Zone-Redundant Resources

- Spread automatically across multiple availability zones.
- Microsoft manages request distribution and data replication.
- Automatic failover if a zone goes down.

![bg fit right](./img/zone-redundant.png)

---

# Zone-Resilient vs. Non-Zonal (Regional)

<div class="columns">
<div>

## Zone-Resilient

- Created as **zone-redundant** or deployed **zonally** across multiple zones  
- Survives a single zone outage by design  
- Minimizes downtime as data and services continue in healthy zones  

</div>
<div>

## Non-Zonal (Regional)

- Not explicitly configured for zone redundancy  
- May be placed in any zone and moved automatically  
- Resources may go down if that zone experiences an outage  

</div>
</div>

---

# Availability Zones and Azure Updates

## Update Deployment Strategy

- Microsoft deploys updates to one AZ at a time
- Minimizes impact on active workloads
- Running across multiple zones ensures continuous service during updates

---

# Data Replication: Storage Options

![storage options center](img/storage-options.png)

---
# Enhancing Resiliency through Architecture

<div class="columns">
<div>

## Fault Isolation

- Prevents cascading failures and maintains availability
- Distributes workloads across multiple data centers
- Handles datacenter-specific failures with built-in mechanisms

</div>
<div>

## Reliability Best Practices

- Deploy across multiple Availability Zones
- Leverage multi-region approaches when necessary
- Use landing zones and reference architectures
- Apply design patterns appropriate to your workload

</div>
</div>

---

# Load Testing

<div class="columns">
<div>

## Key Benefits

- **Prevents Surprises**: Identify capacity issues before production
- **Validates Scaling**: Ensure your auto-scaling works properly
- **Finds Weaknesses**: Spot resource exhaustion and memory leaks

</div>
<div>

## Implementation

- **Cost-Effective**: Much cheaper than emergency scaling during incidents
- **Azure Tools**: Azure Load Testing service with JMeter support
- **Best Practice**: Test with realistic user patterns and data volumes

</div>
</div>

---

# Chaos Engineering

<div class="columns">
<div>

## Core Principles

- **Controlled Failure**: Introduce failures in controlled environments
- **Best Practice**: Start small with clear abort conditions
- **Continuous Process**: Build complexity as confidence increases

</div>
<div>

## Key Azure Scenarios

- **Availability Zone Outages**: Test region resiliency
- **Network Latency**: Simulate connectivity issues
- **Service Throttling**: Test quota and limit handling
- **Identity Failures**: Test credential expiration response

</div>
</div>

---

# Implementing Reliability Testing

<div class="columns">
<div>

## Business Benefits

- **Early Issue Detection**: Find problems before customers do
- **Increased Confidence**: Better team preparedness for incidents
- **Reduced Recovery Time**: Faster MTTR during real outages
- **Trust Building**: Demonstrate resilience to stakeholders

</div>
<div>

## Practical Implementation

- **Start Simple**: Begin with critical paths and core functions
- **Game Days**: Schedule cross-team incident response exercises
- **Continuous Process**: Iterate on tests as your architecture evolves
- **Documentation**: Track findings and improvements

</div>
</div>

---

# Reference Architectures & Resources

- Utilize established Azure reference architectures for reliability.

- Leverage available documentation and best practices for consistency and effectiveness.

---

# [Azure Landing Zones](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/landing-zone/)

## Secure Structure

- Azure Landing Zones create a secure, organized foundation for Azure environments.
- Enforce identity, network, and governance policies at scale.

## Reliable Implementation

- A standardized environment supports consistent deployments.
- Simplifies resource management and reduces misconfigurations.

---

![bg fit](img/azure-landing-zone-architecture-diagram-hub-spoke.svg)

---

# [Azure Architecture Center](https://learn.microsoft.com/azure/architecture/)

- Centralized repository of best practices, reference architectures, and design patterns for Azure solutions.
- Provides comprehensive guidance on building reliable, scalable, and secure cloud solutions.
- Offers scenario-specific architectures, recommended practices, and templates to accelerate your workload design.

---

# [Well-Architected Workloads](https://learn.microsoft.com/en-us/azure/well-architected/workloads)

- Align workloads with business outcomes using the Azure Well-Architected Framework  
- Balancing functional requirements and nonfunctional trade-offs  
- Integrate design fundamentals, trade-offs, and operational best practices

---

# [Mission-Critical Workloads](https://learn.microsoft.com/en-us/azure/well-architected/mission-critical/mission-critical-overview)

Learn about designing mission-critical applications on Azure for high availability, reliability, and performance:

![bg right fit](./img/mission-critical.png)

---

# [Enterprise Web App Patterns](https://learn.microsoft.com/en-us/azure/architecture/web-apps/guides/enterprise-app-patterns/overview)

Guidance for web apps on Azure, offering prescriptive architecture, code, and configuration aligned with the Well-Architected Framework:

<div align="center">

![width:900px](img/enterprise-app.png)

</div>

---

## [Azure Service Groups](https://learn.microsoft.com/en-us/azure/governance/service-groups/overview)

<div class="columns">
<div>

### Health Metrics Aggregation

- **Cross-environment health**: Aggregate health metrics from multiple apps and environments
- **Resource type inventory**: Unified views across subscriptions
- **Unified monitoring**: Single Service Group provides health visibility across management groups

</div>
<div>

### Least-Privilege Access

- **Selective permissions**: Service Groups don't inherit member permissions
- **Role-based viewing**: Assign different users access to same Service Group with varying resource visibility
- **Monitoring metrics**: Apply minimal privileges for metrics access without resource management rights

</div>
</div>

---

## [Mission-Critical Health Modeling](https://learn.microsoft.com/en-us/azure/well-architected/mission-critical/mission-critical-health-modeling)

<div class="columns">
<div>

### Core Concepts

- **Layered health model**: Define health from individual resources to user flows
- **Business-aligned scoring**: Health reflects business priorities with clear healthy/unhealthy states
- **Automated calculation**: Use metrics and thresholds for real-time health scores

</div>
<div>

### Implementation

- **Unified observability**: Aggregate logs, metrics, traces in Log Analytics workspaces
- **Visual dashboards**: Azure Monitor or Grafana for real-time insights
- **Proactive response**: Integrate with alerting and automated incident response

</div>
</div>

---

# [Azure Verified Modules (AVM)](https://azure.github.io/Azure-Verified-Modules/)

<div class="columns">
<div>

## What is AVM?

- **Official Microsoft initiative** to consolidate Infrastructure-as-Code standards
- **Multi-language support** - Bicep, Terraform, and more
- **WAF-aligned modules** with built-in best practices
- **Community-driven** with devolved ownership approach

</div>
<div>

## Core Value Proposition

- **Accelerate development** with pre-built, tested modules
- **Ensure compliance** with Azure best practices
- **Reduce misconfigurations** through standardized templates
- **Enhance security** with default secure configurations

</div>
</div>

---

# AVM Module Classifications

| Module Type | Purpose | Key Features |
|-------------|---------|--------------|
| **🏗️ Resource** | Single resource deployment | WAF best practices, secure defaults, shared interfaces |
| **🎯 Pattern** | Multi-resource architectures | Proven designs, composable blocks, enterprise-ready |
| **⚙️ Utility** (Preview) | Reusable functions | Common helpers, cross-module functionality |

---

# AVM and Reliability

<div class="columns">
<div>

## Built-in Resilience

- **Availability Zones** configured by default where supported
- **Disaster recovery** patterns in multi-region deployments
- **Monitoring integration** with Azure Monitor and Application Insights
- **Security hardening** with least-privilege access

</div>
<div>

## Enterprise Ready

- **Well-Architected Framework** alignment across all pillars
- **Production-tested** configurations and patterns
- **Automated compliance** checks and validation
- **Continuous updates** with latest Azure capabilities

</div>
</div>


---

# [APRL: Azure Proactive Resiliency Library](https://github.com/Azure/Azure-Proactive-Resiliency-Library-v2)

<div class="columns">
<div>

### What is APRL?

- **Curated catalog** of resiliency recommendations for Azure workloads
- **Azure Resource Graph (ARG) queries** for identifying non-compliant resources
- **Community-driven** with 75+ contributors and active maintenance
- **Open source** under MIT license

</div>
<div>

### Key Features

- **Resource-specific guidance** for 100+ Azure services
- **Automated compliance checks** with ready-to-use ARG queries
- **Well-Architected Framework alignment** with reliability best practices
- **Specialized workload patterns** for common scenarios

</div>
</div>

---

# APRL Library Structure

<div class="columns">
<div>

### 🏗️ **Azure Resources**
- Service-specific resiliency recommendations
- Individual Azure service best practices
- Resource configuration guidance

### 🔧 **Specialized Workloads**
- End-to-end workload patterns
- Industry-specific scenarios
- Multi-service architectures

</div>
<div>

### 📊 **Azure Resource Graph Queries**
- Automated compliance detection
- Resource inventory and assessment
- Policy enforcement support

### 🛡️ **WAF Integration**
- Well-Architected Framework alignment
- Reliability pillar implementation
- Assessment and review tools

</div>
</div>

---

# [Azure Review Checklists](https://github.com/Azure/review-checklists)

<div class="columns">
<div>

## What They Are

- **Design validation tool** for Azure architectures and workloads
- **Community-driven** checklists based on Microsoft best practices
- **Version-controlled** alternative to Excel spreadsheets
- **Collaborative** approach with GitHub integration

</div>
<div>

## Key Benefits

- **Proactive issue detection** before deployment
- **Standardized reviews** across teams and projects
- **Time-saving** with pre-built checklists
- **Continuous improvement** through community contributions

</div>
</div>

---

# Azure Review Checklists

## Supported Checklists & Services

<style scoped>
table { font-size: 0.8em; }
</style>

| Service Category | Checklist | Status | Key Focus Areas |
|------------------|-----------|---------|-----------------|
| **Foundation** | Azure Landing Zone | GA | Governance, Security, Networking |
| **AI/ML** | AI Landing Zone | Preview | AI services, responsible AI, data governance |
| **Containers** | AKS, ARO | GA/Preview | Security, scalability, monitoring |
| **Applications** | App Service, Spring Apps | GA/Preview | Performance, security, DevOps |
| **Data** | SQL Migration, SAP | Preview/GA | Migration, performance, compliance |
| **Networking** | Application Delivery | GA | Load balancing, security, performance |
| **Virtual Desktop** | AVD | GA | User experience, security, cost optimization |
| **Cost Management** | Cost Optimization | GA | Resource optimization, monitoring |
| **DevOps** | Azure DevOps | Preview | Pipeline security, compliance |

---

# Conclusion

- **Design Principles**: Focus on resilience, recovery, and simplicity for robust workloads
- **Trade-offs**: Every decision impacts cost, security, operational excellence, and performance
- **Proactive Reliability**: Use Failure Mode Analysis to anticipate and mitigate failures
- **Continuous Improvement**: Leverage Availability Zones, multi-region deployments, chaos engineering, and load testing

---

# Thank You!

<div class="columns">
<div>

## Links

- [APRL](https://github.com/Azure/Azure-Proactive-Resiliency-Library-v2)
- [Azure Verified Modules](https://azure.github.io/Azure-Verified-Modules/)
- [Azure Review Checklists](https://github.com/Azure/review-checklists)
- [Enterprise Web Apps](https://learn.microsoft.com/en-us/azure/architecture/web-apps/guides/enterprise-app-patterns/overview)

</div>
<div>

## Chris Ayers

_Senior Software Engineer_
_Azure CXP AzRel_
_Microsoft_

<i class="fa-brands fa-bluesky"></i> BlueSky: [@chris-ayers.com](https://bsky.app/profile/chris-ayers.com)  
<i class="fa-brands fa-linkedin"></i> LinkedIn: - [chris\-l\-ayers](https://linkedin.com/in/chris-l-ayers/)  
<i class="fa fa-window-maximize"></i> Blog: [https://chris-ayers\.com/](https://chris-ayers.com/)  
<i class="fa-brands fa-github"></i> GitHub: [Codebytes](https://github.com/codebytes)  
<i class="fa-brands fa-mastodon"></i> Mastodon: [@Chrisayers@hachyderm.io](https://hachyderm.io/@Chrisayers)
~~<i class="fa-brands fa-twitter"></i> Twitter: [@Chris_L_Ayers](https://twitter.com/Chris_L_Ayers)~~  

</div>

</div>
