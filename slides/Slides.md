---
marp: true
theme: custom-default
footer: 'https://chris-ayers.com'
---

<div class="columns">
<div>

# Resilient by Design: Building Reliable Workloads on Azure

## Chris Ayers

Principal Software Engineer
Microsoft

</div>
<div>

![](./img/arch.png)

</div>
</div>

---

![bg left:40%](./img/portrait.png)

## Chris Ayers

_Principal Software Engineer_  
_Azure EngOps AzRel_  
_Microsoft_

<i class="fa-brands fa-bluesky"></i> BlueSky: [@chris-ayers.com](https://bsky.app/profile/chris-ayers.com)
<i class="fa-brands fa-linkedin"></i> LinkedIn: - [chris\-l\-ayers](https://linkedin.com/in/chris-l-ayers/)
<i class="fa fa-window-maximize"></i> Blog: [https://chris-ayers\.com/](https://chris-ayers.com/)
<i class="fa-brands fa-github"></i> GitHub: [Codebytes](https://github.com/codebytes)
<i class="fa-brands fa-mastodon"></i> Mastodon: [@Chrisayers@hachyderm.io](https://hachyderm.io/@Chrisayers)
~~<i class="fa-brands fa-twitter"></i> Twitter: @Chris_L_Ayers~~

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

# Understanding Reliability, Resiliency & Recoverability

- Failures are **inevitable** in distributed systems
- The WAF frames reliability as three distinct properties:
  - **Resilient** — detect and **withstand** faults, degrade gracefully
  - **Recoverable** — **restore** within agreed **RTO/RPO** when resiliency is exceeded
  - **Available** — users access the workload at the promised quality
- Failures impact _**revenue**_, _**reputation**_, and _**customer trust**_

---

# <!-- fit --> FAILURE IS ALWAYS AN OPTION

---

<!-- _footer: "" -->

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

<!-- _footer: "" -->

![bg fit](./img/one-does-not-simply.jpg)

---

# How Do We Measure Reliability?

![bg right:30% fit](./img/sla-slo-sli.png)

| | Definition | Example |
|-----|-----------|--------|
| **SLI** | Metric of user experience | API success rate per request |
| **SLO** | Reliability target over window | 99.95% successful responses (30d) |
| **Error Budget** | 1 − SLO (consumable failure) | 0.05% ≈ 21.9 min @ 99.95% |
| **SLA** | Contract with penalties | 99.9% monthly w/ credits |

---

<!-- _footer: "" -->

![bg fit](./img/composite-sla.drawio.png)

---

# Understanding RPOs and RTOs

- **RTO**: Max acceptable **downtime** before services must be restored
- **RPO**: Max acceptable **data loss** measured in time

![width:850px center](./img/rpo-rto.drawio.png)

---

# The Reliability Journey

<!-- _footer: "" -->

![w:900px center](./img/layers-roadmap.drawio.svg)

> We build reliability layer by layer — from requirements to scaling it across the org

---

# Business & Non-Functional Requirements Layer

## From Intent to Quantified Reliability

Establish resilience expectations before selecting technology:
- Define critical user journeys & classify components (critical vs. degradable)
- Quantify reliability targets (SLOs, error budgets, RTO/RPO)
- Align trade-offs early (cost, performance, security, operations)

---

# Requirements: Resilience & Recovery

<div class="columns">
<div>

### Business

- Quantify success targets for flows
- Understand platform SLAs, limits, constraints


### Key Principle

> Design for **how you respond**, not just prevention

</div>
<div>

### Recovery
- Recovery plans with regular drills
- Trusted backups & immutable copies
- Automated detection & self-healing


### Resilience

- Classify **critical** vs. **degradable** components
- Design for fault isolation & graceful degradation
- Eliminate single points of failure

</div>
</div>

---

# Identify & Rate User and System Flows

<div class="columns">
<div>

## Flow Identification

- Map **critical user journeys** end-to-end
- Identify all **system flows** (background jobs, sync, integrations)
- Rate each flow on a **criticality scale** tied to business impact

</div>
<div>

## Criticality Classification

| Tier | Reliability Target |
|------|--------------------|
| **Critical** | 99.99%, RTO < 5 min |
| **Important** | 99.9%, RTO < 30 min |
| **Non-Critical** | 99.5%, RTO < 4 hours |

> Set SLOs and RTO/RPO **per flow**, not per system
</div>

---

<!-- _footer: "" -->

![bg fit 95%](./img/user-flow-criticality.drawio.png)

---

# Operational Readiness

<div class="columns">
<div>

- Shift left to anticipate failures early
- Test failures in development
- Ensure cross-team visibility
- Use **health models** to track SLO attainment & workload health
- Use observability for rapid remediation

</div>
<div>

| Strategy | Implementation |
|----------|----------------|
| Observable Systems | Aggregate telemetry for holistic health views |
| Failure Simulation | Validate recovery metrics with realistic tests |
| Automation | Minimize human error and ensure consistency |
| Continuous Learning | Improve from real production incidents |
| Health Modeling | Map telemetry to SLO-based health state (Azure Monitor) |
| Proactive Monitoring | Prioritize alerts for active failures |

</div>
</div>

---

# Keep It Simple

![bg right:38% fit](./img/scales.png)

- Avoid overengineering architecture, code, and operations
- Simplicity reduces inefficiencies and misconfigurations
- Maintain a balanced approach to avoid single points of failure

---

# Trade-Offs

<div class="columns">
<div>

![w:960px center](img/tug-of-war.png)

</div>
<div>

- Align trade-offs with business priorities
- Use scenario planning to assess impacts
- Continuously iterate and monitor performance

</div>
</div>

---

<!-- _footer: "" -->

![bg](img/tradeoff-cost.jpg)

---

# Azure Well-Architected Framework

- Provides best practices and guidance for building high-quality Azure solutions.
- Ensures workloads are reliable, secure, efficient, and cost-effective.
- [Azure Well-Architected Framework](https://learn.microsoft.com/en-us/azure/well-architected/)
 
![bg right fit](img/waf.png)

---

<!-- _footer: "" -->

![bg fit](./img/well-architected-hub.png)

---

## Microsoft Azure Well-Architected Framework Pillars

| Reliability                        | Security                            | Cost Optimization                  | Operational Excellence                  | Performance Efficiency                  |
|------------------------------------|-------------------------------------|------------------------------------|-----------------------------------------|-----------------------------------------|
| Resiliency, availability, recovery | Protect data, detect threats, mitigate risks | Budgeting, reducing waste, efficiency | Observability, DevOps practices, safe deployments | Scalability, load testing, performance monitoring |

---

# Reliability Trade-Offs with Other Pillars

| Pillar | More Reliability Means... | Less Reliability Means... |
|--------|--------------------------|---------------------------|
| **Cost** | Redundancy, monitoring, over-provisioning | Right-sized, single-region, simpler ops |
| **Security** | Larger attack surface, may delay patches | Strict controls, frequent patching |
| **Ops Excellence** | Complex architecture, extensive DR testing | Simpler systems, less overhead |
| **Performance** | Replication latency, failover overhead | Lean deployments, speed-focused |

---

# Reliability Maturity Model

Assess your **current posture** and follow a staged path — five levels, each building on the previous.

![w:820px center](./img/maturity-model.drawio.svg)

<p style="font-size:0.7em;text-align:right;margin:0;"><a href="https://learn.microsoft.com/en-us/azure/well-architected/reliability/maturity-model">Reliability maturity model</a></p>

---

# Architecture Layer

## Structuring for Failure Containment

Focus on failure domains, redundancy strategy, and dependency design before implementation.

---

# Dependency Management

<div class="columns">
<div>

## Map Your Dependencies

- Identify all **upstream** and **downstream** dependencies
- Document each dependency's **SLA and failure modes**
- Classify: **strong** (required) vs. **weak** (degradable)

</div>
<div>

## Mitigation Strategies

| Strategy | When to Use |
|----------|-------------|
| **Caching** | Tolerate downtime for reads |
| **Async messaging** | Decouple from unreliable services |
| **Fallback values** | Defaults when dependency fails |
| **Circuit breaker** | Prevent cascading failures |
| **Timeout + retry** | Handle transient issues |

</div>
</div>

---

<!-- _footer: "" -->

![bg fit](./img/dependency-map.drawio.png)

---

# Failure Mode Analysis (FMA)

<!-- _footer: "" -->

<div class="columns">
<div>

## Proactive Identification

- Distinguish **failures** (unexpected, need intervention) from **errors** (expected in normal ops)
- Analyze **read** vs. **write** failures separately — impact & mitigation differ
- Prioritize by user impact & blast radius; use checklists, post-mortems, dependency maps

</div>
<div>

## Effective Mitigation

- Architect graceful degradation & fallbacks
- Instrument for fast anomaly detection
- Automate failover & data replication where feasible

</div>
</div>

> [Failure Examples](https://learn.microsoft.com/en-us/azure/well-architected/reliability/failure-mode-analysis#example)

---

# FMA: Failure vs. Error

![w:950px center](./img/fma-flow.drawio.svg)

---

# Single Points of Failure (SPOFs)

<div class="columns">
<div>

## Understanding SPOFs

- Single component whose failure halts critical flow
- Occur in infra, data, code paths, people/process
- Early detection reduces mean time to mitigation

</div>
<div>

## Eliminating SPOFs

- Redundancy & diversity (multi-zone / multi-instance)
- Load balancing & partitioning
- Automated failover runbooks
- **Delete protection** via Azure resource locks on redundant components

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

# Infrastructure / Platform Layer

## Foundation for Consistent Resilience

Automated, policy-driven environments (Landing Zones, AVM, APRL) reduce variance & misconfiguration risk.

---

# Azure-Customer Shared Responsibility Model

<div class="columns">
<div>

![width:500px](./img/shared-responsibility.svg)


> Azure guarantees platform SLA; you own workload SLA

</div>
<div>

## Your Reliability Responsibilities

| Layer | Customer Owns |
|-------|---------------|
| **Data** | Backup strategy, replication, encryption |
| **Application** | Retry logic, circuit breakers, health probes |
| **Identity** | MFA, conditional access, break-glass accounts |
| **Network** | Redundant paths, ExpressRoute resilience |
| **Infra Config** | Zone/region selection, scaling rules |

</div>
</div>

---

# Azure Regions and Regional Strategy

<div class="columns">
<div>

## Region Selection Criteria

- **Latency**: Proximity to users
- **Compliance**: Data residency, sovereignty
- **Service Availability**: Not all services in all regions
- **Cost**: Pricing varies by region

</div>
<div>

## Multi-Region Considerations

| Factor | Impact on Design |
|--------|------------------|
| Region Pairs | Sequential updates, prioritized recovery |
| Non-paired | Flexible, but plan your own DR |
| Sovereign Clouds | Isolated (Gov, China, Germany) |
| Distance | Affects replication lag & latency |

> Pairing ≠ automatic failover—you must design for it

</div>
</div>

---

# Azure Region Pairs

![w:1000px center](./img/region-pairs.drawio.svg)

---

# Azure Availability Zones

- Physically separate datacenters within a region
- Independent power, cooling, and networking
- Low-latency (<2ms) connections between zones
- Updates deployed one AZ at a time
- [View Region Support](https://learn.microsoft.com/en-us/azure/reliability/availability-zones-region-support)

![bg right fit](img/az-diagram.png)

---

# Types of Availability Zone Support

| Type | Deployment | Failover | Who Manages |
|------|-----------|----------|-------------|
| **Zonal** | Pinned to one zone | You handle failover | Customer |
| **Zone-Redundant** | Spread across zones | Automatic | Microsoft |
| **Zone-Resilient** | Zonal or zone-redundant | Survives zone outage | Varies |
| **Non-Zonal (Regional)** | No zone affinity | May go down with zone | Neither |

- [View Services Support](https://learn.microsoft.com/en-us/azure/reliability/availability-zones-service-support)

---

# Zonal Resources

- Pinned to a specific availability zone
- Combine multiple zonal deployments for high reliability
- **You** manage replication, load balancing, and failover
- Deploy across zones for zone-level protection

![bg right fit](./img/zonal-3.png)

---

# Zone-Redundant Resources

- Spread automatically across multiple availability zones.
- Microsoft manages request distribution and data replication.
- Automatic failover if a zone goes down.

![bg fit right](./img/zone-redundant.png)

---

# Data Replication: Storage Options

<!-- _footer: "" -->
<!-- _class: small -->

![bg left:60% fit](./img/storage-options.png)

- **RPO:** LRS/ZRS sync = 0; GRS/GZRS async ≈15 min. GZRS/RA-GZRS maximize durability.
- **Recovery:** Hot/Cool/Archive tiering changes restore time; immutable storage protects against ransomware.

---

# Scaling Strategies

![width:1080px](./img/scaling-strategies.drawio.png)

---

# Azure Reliability Services

<div class="columns">
<div>

## Traffic & Load Balancing

| Service | Purpose |
|---------|---------|
| **Azure Front Door** | Global HTTP LB, failover, WAF |
| **Traffic Manager** | DNS-based global routing |
| **Load Balancer** | Regional L4 LB |
| **Application Gateway** | Regional L7 LB, WAF |

</div>
<div>

## Backup & Recovery

| Service | Purpose |
|---------|---------|
| **Azure Site Recovery** | DR replication & failover |
| **Azure Backup** | Managed backup (VMs, SQL, files) |
| **Service Health** | Platform outage alerts |
| **Resource Health** | Individual resource status |
| **Azure Advisor** | Proactive recommendations |

</div>
</div>

---

# Software / Workload Layer

## Runtime Resilience Behaviors

Embed failure-aware logic: timeouts, retries, backoff, bulkheads, circuit breakers, idempotency, hedging.

---

# Resilience Patterns

<!-- _footer: "" -->

<style scoped>
table { font-size: 0.7em; }
td, th { padding: 3px 6px; }
</style>
| Pattern | Problem Solved | Key Considerations |
|---------|----------------|--------------------|
| Timeout | Prevent hanging on slow dependency | Set < expected p95 latency; combine with retries |
| Retry + Backoff + Jitter | Transient faults (throttling, blips) | Cap attempts; respect idempotency |
| Circuit Breaker | Failing dependency cascading | Trip on error rate/latency; half-open probes |
| Bulkhead Isolation | One noisy component  | Resource partitioning |
| Dead Letter Queue | Bad messages blocking progress | Monitor & replay with alerting |
| Throttling / Rate Limiting | Protect service from overload & noisy neighbors | Shed or queue excess load; return `Retry-After` |
| Graceful Degradation | Maintain partial service | Feature flags, fallback data |

> [Throttling design guide](https://learn.microsoft.com/en-us/azure/well-architected/design-guides/throttling)

---

# Circuit Breaker — State Machine

![w:920px center](./img/circuit-breaker.drawio.svg)

---

# Async & Event-Driven Patterns

![width:1080px](./img/async-patterns.drawio.png)

---

# Azure SDK Resiliency Best Practices

<div class="columns">
<div>

## Built-in SDK Features
- **Retry** — Exponential backoff + jitter
- **Timeouts** — Per-attempt + overall deadline
- **Throttling** — Honor `Retry-After`
- **Idempotency** — Keys/message IDs to dedupe

</div>
<div>

## Your Responsibilities
- **Circuit Breaking** — Polly / resilience libs
- **Connection Reuse** — Reuse clients (thread-safe)
- **Backpressure** — Bounded channels, fail fast
- **Observability** — Correlate traces, log retries

</div>
</div>

---

# Operations & Observability Layer

## Detect, Respond, Learn

Emphasize fast detection, validated recovery paths, and continuous improvement through data & drills.

---

# Safe Deployment Practices

![w:1080px](./img/safe-deployment.drawio.png)

---

# OpenTelemetry

- Open standard for generating, collecting, and exporting telemetry data (traces, metrics, logs)
- Supported by major cloud providers, including Azure Monitor, AWS CloudWatch, and Google Cloud Operations
- Enables vendor-neutral instrumentation and observability across distributed systems

---

# Metrics & Error Budgets

| Concept | Formula / Definition | Example |
|---------|----------------------|---------|
| Availability SLI | (Successful Requests) / (Total Requests) | 99,950 / 100,000 = 99.95% |
| Latency SLI | % of requests under threshold | 95% < 250ms (p95), 99% < 400ms |
| Error Budget  | 1 - SLO | SLO 99.9% => 0.1% budget |
| Burn Rate | Budget consumed / Time elapsed fraction | 2× burn -> intervene early |
| MTTR | Avg restore time for incidents | Track trend downwards |

- Burn Rate **> 4×** for 1h: Freeze deploys; incident review
- Burn Rate **2×** sustained: Reduce change volume
- Burn Rate **< 1×**: Continue roadmap; schedule chaos tests

---

# Error Budget Burn-Down

![w:900px center](./img/burn-rate.drawio.svg)

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

# Disaster Recovery Strategies

![w:1080px](./img/dr-strategies.drawio.png)

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

# Validating Resilience

<div class="columns">
<div>

## Why Validate?

- **Early Issue Detection**: Find problems before customers do
- **Increased Confidence**: Better team preparedness for incidents
- **Reduced Recovery Time**: Faster MTTR during real outages
- **Trust Building**: Demonstrate resilience to stakeholders

</div>
<div>

## How to Validate

- **Start Simple**: Begin with critical paths and core functions
- **Game Days**: Schedule cross-team incident response exercises
- **Iterate**: Evolve tests as your architecture changes
- **Track**: Document findings and improvements

</div>
</div>

---

# Incident Response & Continuous Learning

![width:1080px](./img/incident-lifecycle.drawio.png)

---

# Scale &amp; Acceleration Layer

## Making Reliability Repeatable

- Scale the reliability you designed across many workloads and teams
- Cut the effort with reusable modules (AVM), proactive assessment (APRL), and standardized landing zones

---

# [Azure Landing Zones](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/landing-zone/)

## Secure Structure

- Azure Landing Zones create a secure, organized foundation for Azure environments.
- Enforce identity, network, and governance policies at scale.

## Reliable Implementation

- A standardized environment supports consistent deployments.
- Simplifies resource management and reduces misconfigurations.

---

<!-- _footer: "" -->

![bg fit](img/azure-landing-zone-architecture-diagram-hub-spoke.svg)

---

# Reference Architectures & Guidance

<div class="columns">
<div>

## [Azure Architecture Center](https://learn.microsoft.com/azure/architecture/)
- Best practices, reference architectures, design patterns

## [Well-Architected Workloads](https://learn.microsoft.com/en-us/azure/well-architected/workloads)
- Workload-specific WAF guidance & trade-offs

</div>
<div>

## [Mission-Critical Workloads](https://learn.microsoft.com/en-us/azure/well-architected/mission-critical/mission-critical-overview)
- Designing for max availability & performance

## [Enterprise Web App Patterns](https://learn.microsoft.com/en-us/azure/architecture/web-apps/guides/enterprise-app-patterns/overview)
- Prescriptive architecture, code, and configuration

</div>
</div>

---

# Web App Patterns

<div class="columns">
<div>

**Reliable Web App**
![w:520px center](./img/reliable-web-app-architecture-plus-optional.svg)

</div>
<div>

**Modern Web App**
![h:480px center](./img/modern-web-app-architecture-plus-optional.svg)

</div>
</div>

---

# Enterprise & Mission-Critical

<div class="columns">
<div>

**Enterprise App**
![w:520px center](./img/enterprise-app.png)

</div>
<div>

**Mission-Critical**
![h:480px center](./img/mission-critical.png)

</div>
</div>

---

# WAF Design Guides (Design Essentials)

<div class="columns">
<div>

Prescriptive, **cross-pillar** guidance for specific practices — a newer addition to the framework.

- [Regions & availability zones](https://learn.microsoft.com/en-us/azure/well-architected/design-guides/regions-availability-zones)
- [Handle transient faults](https://learn.microsoft.com/en-us/azure/well-architected/design-guides/handle-transient-faults)
- [Throttling](https://learn.microsoft.com/en-us/azure/well-architected/design-guides/throttling)
- [Background jobs](https://learn.microsoft.com/en-us/azure/well-architected/design-guides/background-jobs)

</div>
<div>

- [Build a monitoring system](https://learn.microsoft.com/en-us/azure/well-architected/design-guides/monitoring)
- [Health modeling](https://learn.microsoft.com/en-us/azure/well-architected/design-guides/health-modeling)
- [Disaster recovery (multi-region)](https://learn.microsoft.com/en-us/azure/well-architected/design-guides/disaster-recovery)
- [Incident management](https://learn.microsoft.com/en-us/azure/well-architected/design-guides/incident-management)

> Bridge principles → implementation

</div>
</div>

---

# [Azure Verified Modules (AVM)](https://azure.github.io/Azure-Verified-Modules/)

<div class="columns">
<div>

## What is AVM?

- **Official Microsoft IaC initiative** — Bicep, Terraform
- **WAF-aligned** with built-in reliability defaults
- AZs, monitoring, security hardening out of the box

</div>
<div>

## Module Types

- **🏗️ Resource** — Single resource, secure defaults
- **🎯 Pattern** — Multi-resource proven architectures
- **⚙️ Utility** — Reusable helpers (Preview)

> Reduce misconfigurations with standardized, production-tested templates

</div>
</div>


---

# [APRL: Azure Proactive Resiliency Library](https://github.com/Azure/Azure-Proactive-Resiliency-Library-v2)

<div class="columns">
<div>

## What is APRL?

- **Curated catalog** of resiliency recommendations for 100+ Azure services
- **Azure Resource Graph queries** to find non-compliant resources
- **WAF-aligned** reliability best practices
- **Open source** — 75+ contributors

</div>
<div>

## What’s Inside

- 🏗️ **Resource-specific** configuration guidance
- 🔧 **Specialized workload** patterns
- 📊 **Ready-to-use ARG queries** for compliance
- 🛡️ **Assessment & review** tools

</div>
</div>

---

# [Azure Review Checklists](https://github.com/Azure/review-checklists)

<div class="columns">
<div>

## What They Are

- **Design validation tool** for Azure architectures
- **Community-driven** best practice checklists
- **Proactive issue detection** before deployment

</div>
<div>

## Coverage

- Landing Zones, AKS, App Service, SQL
- Networking, Cost Optimization, DevOps
- AI Landing Zone, Spring Apps, SAP
- Standardized reviews across teams & projects

</div>
</div>

---

# Conclusion

- **Design Principles**: Focus on resilience, recovery, operations, and simplicity
- **Know Your Flows**: Identify critical user journeys, set per-flow SLOs, and understand composite SLAs
- **Trade-offs**: Every decision impacts cost, security, operational excellence, and performance
- **Proactive Reliability**: Use FMA, dependency mapping, safe deployments, and tested DR plans
- **Continuous Improvement**: Chaos engineering, load testing, incident response, and blameless postmortems

---

# Questions?

![bg right:45%](./img/questions.jpg)

---

# Thank You!

<div class="columns">
<div>

## Links

- [Azure Well-Architected Framework](https://learn.microsoft.com/en-us/azure/well-architected/)
- [APRL](https://github.com/Azure/Azure-Proactive-Resiliency-Library-v2)
- [Azure Verified Modules](https://azure.github.io/Azure-Verified-Modules/)
- [Azure Review Checklists](https://github.com/Azure/review-checklists)
- [Enterprise Web Apps](https://learn.microsoft.com/en-us/azure/architecture/web-apps/guides/enterprise-app-patterns/overview)

</div>
<div>

## Chris Ayers

_Principal Software Engineer_
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