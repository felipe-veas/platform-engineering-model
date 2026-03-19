# Developer Interaction with Production

## 1. The Operational Problem: Debugging the Black Box

When a system breaks, your recovery speed depends directly on how engineers interact with production. If a service starts throwing 500s, spiking in latency, or exhausting connection pools, the owning team needs immediate diagnostic context.

The problem lies in the debugging interface. In older architectures, production is a black box. Apps dump unstructured text to stdout, metrics are an afterthought, and distributed tracing is non-existent. When the pager goes off, responders fly blind.

## 2. The Incorrect Solution: Direct Infrastructure Access

To compensate for bad observability, organizations often fall back on a dangerous anti-pattern: granting direct, imperative access to production.

This manifests as permanent SSH keys for EC2 instances, `kubectl exec` permissions in production namespaces, or write access to database consoles. The usual justification is mean-time-to-recovery (MTTR). The assumption is that dropping an engineer onto a live server to grep logs or kill processes is faster than running a pipeline.

## 3. The Reliability Risks of Cowboy Engineering

Direct access breeds cowboy engineering and destroys long-term system stability.

When a responder SSHs into a node at 3:00 AM, their only priority is stopping the bleeding. They install ad-hoc debugging tools, hand-edit configuration files, or manually bump memory limits on live processes.

This creates immediate operational debt. The fix now exists only in system memory or on a single disk. The environment has drifted. After the incident cools down, nobody remembers to backport those panicked CLI commands into Terraform or application repositories.

The inevitable result: the next automated deployment wipes out the manual fix. The bad config in Git overwrites the running state, and the exact same incident triggers again. We traded long-term reliability for a temporary, undocumented patch. Furthermore, direct access circumvents audit logs. You lose track of who ran what command during the outage.

## 4. The Platform Approach: Telemetry as the Primary Interface

In a mature platform, production is strictly immutable and read-only. We don't SSH into instances. We don't exec into containers. Telemetry is the only debugging interface.

The platform injects a comprehensive observability stack—metrics, structured JSON logging, and distributed tracing—into every service by default. Responders debug from the outside in. They query log aggregators to find stack traces, use APM to identify latency bottlenecks, and trace requests across network boundaries to find the failing component.

When they identify the fix, they push it through the deployment pipeline. They update the code or manifest, commit to Git, and let the reconciliation loop handle the rollout.

## 5. Emphasizing Decision-Making and Behavior

Locking down production forces better engineering behavior. If developers know they cannot SSH into a box to fix state, they build observable applications from the start. They add structured logging, expose `/health` endpoints, and instrument business logic because telemetry is their only lifeline during an incident.

The platform team's job is making this observability tooling so reliable and frictionless that nobody actually wants to SSH into a server anyway.

When direct access is unavoidable—a true break-glass scenario where the control plane is completely down—we use short-lived, just-in-time (JIT) credentials that expire automatically. Crucially, pulling the break-glass is treated as a systemic failure. It mandates an incident review. The primary action item is always building an automation or platform capability so we never need human access for that failure mode again. We continuously encode operational knowledge, replacing manual heroics with deterministic tooling.
