# Failure Prevention by Design

## 1. The Operational Problem: Recurring Classes of Incidents

Nothing drains an SRE team faster than responding to the exact same incident week after week. Month after month, you get paged for identical failure modes: a microservice OOMKills because someone forgot to set resource limits, an internal API drops offline due to an expired TLS certificate, or a connection pool exhausts because an application lacks basic timeout configs.

These aren't novel distributed systems problems. They are basic, preventable configuration errors. Yet they keep happening across different teams, regardless of engineering seniority. The real problem is that the organization fails to learn from outages at a systemic level, allowing the same vulnerabilities to ship repeatedly.

## 2. The Incorrect Solution: Runbooks, Checklists, and "Being More Careful"

When recurring incidents happen, organizations usually try to patch human behavior instead of system architecture. The standard reaction is writing more documentation. SREs draft exhaustive wiki pages or detailed runbooks on how to configure memory limits or manually rotate certificates.

Management then adds procedural checklists. They mandate that every PR must pass a 20-point "Production Readiness" review. During post-mortems, the primary action item ends up being some variation of: "Engineers must remember to check expiration dates" or "We need to train developers to be more careful with load balancers."

## 3. The Reliability Risks of Relying on Human Perfection

Relying on runbooks, checklists, and vigilance to prevent outages guarantees failure. It ignores human psychology and the reality of operating systems at scale.

First, humans are fallible, especially under pressure or when doing rote tasks. A developer focused on shipping a complex feature will eventually skip step 14 on a readiness checklist. Second, runbooks rot. As infrastructure evolves, documentation gets stale, leading engineers to run the wrong remediation steps during the next outage.

Most importantly, trying to solve technical problems with organizational process creates a toxic engineering culture. When a developer inevitably ships a bad config that brings down a service, the blame falls on the individual for "not following process" or "not being careful." This blame culture discourages transparency, hides near-misses, and ensures root causes are never actually fixed. The system stays fragile, waiting for the next human error to trigger the next page.

## 4. The Platform Approach: Eliminating Entire Classes of Error

A mature platform organization recognizes that most outages aren't technical failures; they're workflow problems. If a developer can easily deploy a service without resource limits or with an expiring certificate, the platform itself is defective.

The platform approach completely eliminates entire classes of incidents through system design. We shift from relying on organizational process to enforcing system constraints.

If OOM kills keep happening, the platform team doesn't write a runbook. They deploy a mutating admission webhook that automatically injects sensible default memory and CPU limits into every pod. If certificate expirations cause outages, they don't set calendar reminders. They integrate cert-manager into the paved path to provision, rotate, and renew TLS certificates automatically. If misconfigured security groups are a risk, the platform abstracts networking entirely, ensuring developers can only deploy services that are secure by default and isolated by namespace.

## 5. Emphasizing Decision-Making and Behavior

This philosophy transforms how an organization handles incident response and post-mortems.

In a platform-oriented culture, post-mortems are genuinely blameless because we accept that human error is inevitable and systems must absorb it. When an incident happens, the investigation doesn't stop at "Developer X pushed a bad config." It asks: "Why did the platform let Developer X push a bad config? What automated guardrail was missing? How do we redesign the paved path so this failure mode is mathematically impossible to repeat?"

The output of a good post-mortem is never a new checklist or a mandate to "be more careful." It's a high-priority feature request for the internal developer platform. The platform team analyzes incident data to find common failure modes, then builds capabilities to eradicate them. By encoding operational knowledge into automated, self-service infrastructure, every incident permanently hardens the production environment.
