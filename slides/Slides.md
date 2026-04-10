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
_Azure CXP AzRel_  
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

![bg right:30% fit](./img/sla-slo-sli.png)

| | Definition | Example |
|-----|-----------|--------|
| **SLI** | Metric of user experience | API success rate per request |
| **SLO** | Reliability target over window | 99.95% successful responses (30d) |
| **Error Budget** | 1 − SLO (consumable failure) | 0.05% ≈ 21.9 min @ 99.95% |
| **SLA** | Contract with penalties | 99.9% monthly w/ credits |

---

# Understanding RPOs and RTOs

- **RTO**: Max acceptable **downtime** before services must be restored
- **RPO**: Max acceptable **data loss** measured in time

![width:850px center](./img/rpo-rto.drawio.png)

---

# Business & Non-Functional Requirements Layer

## From Intent to Quantified Reliability

Establish resilience expectations before selecting technology:
- Define critical user journeys & classify components (critical vs. degradable)
- Quantify reliability targets (SLOs, error budgets, RTO/RPO)
- Align trade-offs early (cost, performance, security, operations)

---

# Business Requirements

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

# Resilience Requirements

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

# Recovery Requirement

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

![bg fit](./img/user-flow-criticality.drawio.png)

---

![bg fit](./img/composite-sla.drawio.png)

---

# Operational Readiness

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

![bg](img/tradeoff-cost.jpg)

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

# Architecture Layer

## Structuring for Failure Containment

Focus on failure domains, redundancy strategy, and dependency design before implementation.

---

![bg fit](./img/dependency-map.drawio.png)

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

> SLO cannot exceed composite SLA of critical-path dependencies

---


# Failure Mode Analysis (FMA)

<div class="columns">
<div>

## Proactive Identification

- Recognize potential weaknesses before outages occur
- Use checklists, post-mortems, dependency mapping
- Prioritize by user impact & blast radius
- Separate rare/high-impact from frequent/low-impact

</div>
<div>

## Effective Mitigation

- Architect graceful degradation & fallbacks
- Instrument for fast anomaly detection
- Automate failover & data replication where feasible
- Document hypotheses & validate via game days

</div>
</div>

> [Failure Examples](https://learn.microsoft.com/en-us/azure/well-architected/reliability/failure-mode-analysis#example)

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

## Mitigation Strategies

- Redundancy & diversity (multi-zone / multi-instance)
- Load balancing & partitioning
- Automated failover runbooks
- Regular design & dependency reviews

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

# Azure Availability Zones

![width:800px center](img/az-diagram.png)

---

# Azure Availability Zones Infrastructure

- AZs are physically separate datacenters within an Azure region
- Each zone has independent power, cooling, and networking
- Low-latency (<2ms) connections reduce local outage impact
- Azure deploys updates one AZ at a time for continuous service
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

- Deploy and manage resources in each zone
- Configure and manage data replication
- Use a load balancer to distribute requests
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

# Data Replication: Storage Options

| Option | Scope | RPO | Use Case |
|--------|-------|-----|----------|
| **LRS** | Single datacenter | 0 (sync) | Dev/test, easily reconstructed data |
| **ZRS** | Across AZs | 0 (sync) | Production, zone-level protection |
| **GRS** | Cross-region | ~15 min | DR across regions |
| **GZRS** | AZs + cross-region | ~15 min | Best durability |
| **RA-GRS/RA-GZRS** | + read access | ~15 min | Read from secondary during outage |

- Hot vs. Cool vs. Archive affects **recovery time**
- **Immutable storage** for ransomware protection

---

![bg fit](./img/scaling-strategies.drawio.png)

---

# Scaling Strategy

<div class="columns">
<div>

## Scale Up vs. Scale Out

| Approach | Description |
|----------|-------------|
| **Scale Up** | Bigger instances (more CPU/RAM) |
| **Scale Out** | More instances behind load balancer |

- Scale out provides **better resilience** but needs stateless design
- Scale up is simpler but has **ceiling limits**

</div>
<div>

## Auto-Scaling Best Practices

- Base on **actual or predicted** usage patterns
- Use **multiple signals**: CPU, memory, queue depth
- **Overprovision headroom** — failures don't wait
- Set **minimum instance counts** for critical workloads

</div>
</div>

---

# Azure Reliability Services

<div class="columns">
<div>

## Traffic & Load Balancing

| Service | Purpose |
|---------|---------|
| **Azure Front Door** | Global HTTP load balancing, failover, WAF |
| **Traffic Manager** | DNS-based global traffic routing |
| **Load Balancer** | Regional L4 load balancing |
| **Application Gateway** | Regional L7 load balancing, WAF |

</div>
<div>

## Backup & Recovery

| Service | Purpose |
|---------|---------|
| **Azure Site Recovery** | DR replication & orchestrated failover |
| **Azure Backup** | Managed backup for VMs, SQL, files |
| **Service Health** | Platform outage awareness & alerts |
| **Resource Health** | Individual resource health status |
| **Azure Advisor** | Proactive reliability recommendations |

</div>
</div>

---

# Software / Workload Layer

## Runtime Resilience Behaviors

Embed failure-aware logic: timeouts, retries, backoff, bulkheads, circuit breakers, idempotency, hedging.

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

![bg fit](./img/safe-deployment.drawio.png)

---

# Safe Deployment Practices

<div class="columns">
<div>

## Deployment Strategies

| Strategy | Description |
|----------|-------------|
| **Blue-Green** | Two identical environments; instant swap |
| **Canary** | Route small % of traffic to new version |
| **Progressive Rollout** | Ring-based expansion (internal → canary → all) |
| **Feature Flags** | Toggle features without redeploying |

</div>
<div>

## Health Gates & Rollback

- **Automated health checks** at each deployment stage
- Monitor **error rate, latency, SLIs** during bake time
- **Auto-rollback** if thresholds are breached
- **Never skip health gates** — bad deploys cause outages
- Use **deployment slots** for zero-downtime swaps

</div>
</div>

---

![bg fit](./img/dr-strategies.drawio.png)

---

# Disaster Recovery Strategies

<div class="columns">
<div>

| Strategy | RTO | RPO | Cost |
|----------|-----|-----|------|
| **Backup & Restore** | Hours–Days | Hours | $ |
| **Pilot Light** | 30 min–Hours | Minutes | $$ |
| **Warm Standby** | Minutes | Seconds–Min | $$$ |
| **Multi-Site Active** | Near-zero | Near-zero | $$$$ |

</div>
<div>

## DR Plan Essentials

- **Document** failover/failback procedures
- **Test regularly** — untested plans are not plans
- **Automate** failover (Azure Site Recovery)
- **Include data layer** — backup, replication

</div>
</div>

---

# Resilience Patterns
| Pattern | Problem Solved | Key Considerations |
|---------|----------------|--------------------|
| Timeout | Prevent hanging on slow dependency | Set < expected p95 latency; combine with retries |
| Retry + Backoff + Jitter | Transient faults (throttling, blips) | Cap attempts; respect idempotency |
| Circuit Breaker | Failing dependency cascading | Trip on error rate/latency; half-open probes |
| Bulkhead Isolation | One noisy component  | Resource partitioning |
| Dead Letter Queue | Bad messages blocking progress | Monitor & replay with alerting |
| Graceful Degradation | Maintain partial service | Feature flags, fallback data |

---

# Additional Resilience Patterns

| Pattern | Problem Solved | Key Considerations |
|---------|----------------|--------------------|
| Queue-Based Load Leveling | Traffic spikes overwhelming backend | Buffer with queue; consumers process at steady pace |
| Throttling / Rate Limiting | Resource exhaustion from excess demand | Protect services; return 429 with Retry-After |
| Saga / Compensating Transaction | Distributed transaction failures | Automatic rollback via compensation steps |
| Cache-Aside | Dependency downtime for read-heavy data | Serve from cache; fall back to origin |
| Health Endpoint Monitoring | Unknown service state | Expose `/health` endpoints for probes & routing |
| Gateway Routing / Geode | Unhealthy backends; multi-geo | Route to healthy nodes; active-active across regions |

---

![bg fit](./img/async-patterns.drawio.png)

---

# Async & Event-Driven Patterns

<div class="columns">
<div>

## Queue-Based Load Leveling

- **Queue** absorbs spikes; consumers process at steady pace

## Competing Consumers

- Multiple workers — if one fails, **others continue**

## Publisher / Subscriber

- **Decouples** producers from consumers

</div>
<div>

## Saga Pattern

- **Distributed transactions** with compensating rollback

## Azure Services

- **Service Bus** — Queues & Topics
- **Event Grid** — Event routing
- **Event Hubs** — Streaming

</div>
</div>

---

# Azure SDK Resiliency Best Practices

| Concern | Guidance | Notes / Azure SDK Features |
|---------|----------|-----------------------------|
| Retry Policy | Use built-in exponential backoff + jitter; cap total elapsed time | SDK defaults (3–5 retries). Tune max attempts & overall operation timeout |
| Idempotency | Make write & queue ops safely repeatable | Use idempotency keys / message IDs to dedupe |
| Timeouts | Combine per-attempt timeout + overall deadline | Use CancellationToken / request context to abort quickly |
| Throttling (429 / 503) | Honor Retry-After; escalate if sustained | Log retry reason & emit metric for throttled calls |
| Circuit Breaking | Wrap high-risk dependencies with Polly / resilience lib | Trip on error rate/latency. Half-open probes restore flow |

---

# Azure SDK Resiliency Best Practices Continued

| Concern | Guidance | Notes / Azure SDK Features |
|---------|----------|-----------------------------|
| Connection Reuse | Reuse client instances (thread-safe) | Avoid per-request instantiation to prevent socket exhaustion |
| Observability | Correlate Request-Id / Traceparent; record retry count | Enable SDK logging + OpenTelemetry exporters |
| Partial Failure | Process successes; isolate & retry failed items | Batch ops return per-item status; implement granular retry |
| Backpressure | Limit concurrency & queue length; shed excess | Use bounded channels / semaphores; fail fast when saturated |

> Engineer for transient failure as the norm: fast fail, bounded retries, measurable outcomes.

---

# Operations & Observability Layer

## Detect, Respond, Learn

Emphasize fast detection, validated recovery paths, and continuous improvement through data & drills.

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

![bg fit](./img/incident-lifecycle.drawio.png)

---

# Incident Response & Continuous Learning

<div class="columns">
<div>

## Incident Response Process

| Phase | Key Actions |
|-------|-------------|
| **Detect** | Alerts, health probes, anomaly detection |
| **Triage** | Severity, blast radius, assign owner |
| **Mitigate** | Runbooks, failover, rollback |
| **Communicate** | Status page, stakeholder updates |
| **Review** | Blameless postmortem, action items |

</div>
<div>

## Continuous Learning

- Conduct **blameless postmortems** after incidents
- Document **root cause**, timeline, **action items**
- Track **MTTR trends** — reduce over time
- Share learnings **across teams**

</div>
</div>

---

# Governance & Enablement Layer

## Guardrails, Standards, Acceleration

Tools & frameworks that reduce variance, enforce policy, and speed safe delivery.

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
| **AI/ML** | AI Landing Zone | Preview | AI services, responsible AI,  governance |
| **Containers** | AKS, ARO | GA/Preview | Security, scalability, monitoring |
| **Applications** | App Service, Spring Apps | GA/Preview | Performance, security, DevOps |
| **Data** | SQL Migration, SAP | Preview/GA | Migration, performance, compliance |
| **Networking** | Application Delivery | GA | Load balancing, security, performance |
| **Cost Management** | Cost Optimization | GA | Resource optimization, monitoring |
| **DevOps** | Azure DevOps | Preview | Pipeline security, compliance |

---

# Conclusion

- **Design Principles**: Focus on resilience, recovery, operations, and simplicity
- **Know Your Flows**: Identify critical user journeys, set per-flow SLOs, and understand composite SLAs
- **Trade-offs**: Every decision impacts cost, security, operational excellence, and performance
- **Proactive Reliability**: Use FMA, dependency mapping, safe deployments, and tested DR plans
- **Continuous Improvement**: Chaos engineering, load testing, incident response, and blameless postmortems

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