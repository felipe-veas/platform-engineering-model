# Platform Engineering Model

## Overview

This repository acts as the operational handbook for how we design, build, and run Internal Developer Platforms (IDPs). We built this to make production safer and product engineering faster.

The target audience is senior engineers, SREs, platform engineers, and engineering managers.

The guidelines here are not theoretical. They are hard lessons learned from running large-scale distributed systems and recovering from massive reliability failures. You will not find Kubernetes tutorials, GitOps setup scripts, or CI/CD pipelines here. Instead, this is a blueprint for organizational reliability, platform thinking, and system safety design.

Our goal is simple: let product teams ship code quickly and safely without forcing them to moonlight as cloud architects.

## The Core Problem: The Blast Radius of Human Error

As engineering headcount grows, system complexity compounds. The biggest mistake you can make is expecting product engineers to simultaneously act as infrastructure experts. In a sprawling microservices environment, the cognitive load needed to manage VPC peering, IAM roles, container orchestration, and stateful databases is crushing.

When product developers manage their own raw infrastructure without good abstractions, they burn cycles context-switching between application logic and systems administration. That context switching drives outages. Human capacity for complexity has a hard limit. If you push raw operational complexity onto product teams, your systems will inevitably fail at scale.

## Structure of the Model

To grasp our operational posture, read the following documents. We divide them into three pillars: **Platform Strategy**, **Operational Control**, and **Developer Interaction**.

### Pillar 1: Platform Strategy & Product Management

How we build platforms that developers actually want to use, track their real value, and drive standardization without dictating terms.

* **[Platform as a Product](docs/01-core-concepts/platform-as-a-product.md)**: Why we run the platform like a B2B SaaS product, relying on user research, DevEx, and internal evangelism instead of top-down mandates.
* **[The Paved Road (Golden Paths)](docs/02-developer-experience/the-paved-road.md)**: How we curb architectural sprawl by offering a heavily supported, opt-in "Paved Road" versus the unsupported "Rugged Road".
* **[Measuring Success](docs/01-core-concepts/measuring-success.md)**: Dropping vanity metrics to track actual developer outcomes using DORA metrics, SPACE, and Time-to-Hello-World (TTHW).

### Pillar 2: Operational Control & Safety

How the platform enforces safety, self-heals from config drift, and structurally prevents entire classes of outages.

* **[The Operating Model](docs/01-core-concepts/operating-model.md)**: Killing ticket-driven operations and manual bottlenecks by wrapping infrastructure in self-service APIs.
* **[GitOps as a Control Plane](docs/03-governance/gitops-control.md)**: Why relying on CI pipelines for deployments is dangerous, and how GitOps gives us a continuous, self-healing reconciliation loop.
* **[Guardrails vs. Permissions](docs/03-governance/guardrails.md)**: Swapping blunt IAM roles and slow CAB approvals for automated Policy-as-Code that evaluates technical risk instantly.
* **[Failure Prevention by Design](docs/03-governance/failure-prevention.md)**: Stop writing runbooks for human error. Redesign the platform to make those errors mathematically impossible.

### Pillar 3: Developer Interaction & Autonomy

How product teams interact with live environments safely and autonomously.

* **[Self-Service in Production](docs/02-developer-experience/self-service.md)**: How high-level infrastructure abstractions (rather than raw AWS/GCP access) empower teams while locking in security by default.
* **[Developer Interaction with Production](docs/02-developer-experience/developer-interaction.md)**: Why SSH and `kubectl exec` are anti-patterns, and why structured telemetry must be your only debugging interface.

## Emphasizing Decision-Making and Operational Behavior

Platform engineering is fundamentally about shaping human behavior and organizational culture. We are not trying to remove developer accountability. Product engineers still own the business logic, performance, and on-call health of their services. The platform simply removes the operational toil so they can actually focus on that ownership.

We need to shift how this organization makes decisions. We are moving away from tribal knowledge, heroic late-night firefighting, and stale runbooks. We are moving toward codified intent, automated guardrails, and deterministic reconciliation. These documents outline how to build systems that catch incidents before they happen, design safe self-service interfaces, and manage scale by choosing architectural control over human intervention.
