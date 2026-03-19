# Failure Prevention by Design

## 1. The Operational Problem: Recurring Classes of Incidents

Nothing burns out an SRE team faster than fighting the exact same fire week after week. You get paged for identical failure modes: a pod OOMKills because someone forgot to declare memory limits, an internal API goes dark due to an expired TLS cert, or a database falls over because a new service deployed without connection pooling or query timeouts.

These are not complex distributed systems failures. They are baseline configuration misses. Yet they keep surfacing across different product teams, regardless of tenure. The core issue is an organizational failure to learn systemically. We allow the same vulnerability to ship repeatedly.

## 2. The Incorrect Solution: Runbooks, Checklists, and "Being More Careful"

Organizations normally try to fix these recurring outages by patching human behavior rather than architecture. The immediate reflex is to write more docs. Responders draft exhaustive runbooks detailing how to manually rotate a certificate or correctly size a JVM heap.

Management follows up by adding procedural friction. They introduce 20-point "Production Readiness" checklists for every release. The primary action item in the post-mortem inevitably boils down to "engineers need to remember to check expiration dates" or "we must train developers to be more careful with IAM policies."

## 3. The Reliability Risks of Relying on Human Perfection

Depending on checklists and vigilance guarantees failure. It completely ignores how humans operate under pressure at scale.

First, people make mistakes. A developer focused on shipping a tricky database migration will eventually skip step 14 on the deployment checklist. Second, runbooks decay. Infrastructure shifts, documentation rots, and responders end up executing stale, dangerous commands during the next incident.

Worst of all, using process to solve technical problems breeds a toxic culture. When a bad config takes down production, the organization blames the individual for "skipping steps." This discourages transparency, hides near-misses, and ensures the actual root cause remains untouched. The system stays brittle, just waiting for the next human slip-up.

## 4. The Platform Approach: Eliminating Entire Classes of Error

A mature engineering organization treats recurring outages as workflow defects, not technical failures. If a developer can easily ship a service without resource limits, the deployment pipeline is broken.

We must eliminate entire classes of incidents through system design. We replace organizational process with hard system constraints.

If OOMKills are a chronic issue, we do not write a runbook on memory profiling. We deploy a mutating admission controller that injects baseline CPU and memory limits into every pod definition. If expired certificates cause downtime, we do not set calendar alerts. We wire `cert-manager` into the ingress controller to handle provisioning and rotation automatically. If open security groups are a risk, we abstract network policies entirely, ensuring new services are locked down by default.

## 5. Emphasizing Decision-Making and Behavior

This mindset alters how we run post-mortems.

In a platform-driven environment, investigations are blameless because we expect human error; the system's job is to absorb it safely. When an outage occurs, the root cause is never "Developer X pushed a bad config." The real question is: "Why did the pipeline allow Developer X to push that config? What automated guardrail failed? How do we build a constraint that makes this failure mathematically impossible next time?"

The output of a post-mortem is never a checklist. It is a prioritized Jira ticket for the platform team. We analyze incident patterns to identify common failure domains, then build automated constraints to kill them. By baking operational safety into self-service tooling, every incident permanently hardens the paved road.
