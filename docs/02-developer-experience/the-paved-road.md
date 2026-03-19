# The Paved Road (Golden Paths)

## 1. The Operational Problem: Architectural Fragmentation

As an engineering org scales, the sheer number of decisions required to ship a basic service explodes. Which language? Which web framework? Which CI tool? Which database? How do we route logs to Datadog? How do we terminate TLS?

If you force every product team to answer these questions independently, you get severe architectural fragmentation. You end up with ten teams running ten totally different tech stacks, relying on custom deployment scripts and suffering from unique failure modes.

Fragmentation kills operational leverage. An SRE team cannot effectively support a heterogeneous environment at scale. When a Sev-1 hits, responders burn precious time just trying to understand the weird, bespoke topology of the failing service before they can actually start debugging. On top of that, security and compliance degrade into a nightmare of manual audits across a dozen different toolchains.

## 2. The Incorrect Solution: Authoritarian Mandates

The classic enterprise fix for fragmentation is standing up an Architecture Review Board and publishing an "Approved Technologies List."

Leadership dictates that every service must be written in Java, deployed on VMs, and backed by Oracle. Any deviation requires a multi-month exception process. This authoritarian posture breeds deep resentment. It assumes a central committee understands product requirements better than the developers actually building the features. It kills innovation, forces teams to use the wrong tool for the job, and predictably drives "Shadow IT" where teams build unapproved systems in secret just to hit launch dates.

## 3. The Platform Approach: The Paved Road

A modern platform team solves fragmentation using economics, not mandates. We build a **Paved Road** (also known as a Golden Path).

The Paved Road is a highly opinionated, heavily supported set of tools and workflows. It is the path of absolute least resistance from an empty Git repository to a secure, observable microservice running in production.

Crucially, **the Paved Road is opt-in**. The platform team does not force anyone to use it. We make it so overwhelmingly frictionless that developers actively *choose* it.

When a team stays on the Paved Road (for example, using our standard Go scaffold, deploying via GitOps to the central Kubernetes cluster, and attaching a managed Postgres DB), they get massive operational benefits for free:

* **Zero-to-Production in Minutes:** CI/CD pipelines are pre-wired.
* **Security by Default:** Container scanning and least-privilege IAM policies are automatically injected.
* **Out-of-the-Box Observability:** Standardized Dashboards, SLI tracking, and structured logging are configured automatically.
* **On-Call Support:** The SRE team shares the operational burden, providing baseline runbooks and active incident support.

## 4. The "Rugged Road" Exception

Because the Paved Road is voluntary, teams are absolutely allowed to wander off into the "Rugged Road" if the product demands it (for instance, they genuinely need a specialized graph database the platform doesn't currently support).

However, that freedom comes with a strict, codified consequence: **Operational Liability**.

If a team chooses the Rugged Road, they forfeit the platform's benefits. They have to write their own GitHub Actions, maintain their own Terraform, configure their own Datadog monitors, and pass manual InfoSec reviews. Most importantly, **they hold the pager alone**. The SRE team will not wake up at 3:00 AM to debug a bespoke, unmanaged graph database.

## 5. Emphasizing Decision-Making and Behavior

This model perfectly aligns organizational incentives.

Developers want to ship code fast and log off at 5:00 PM. The Paved Road lets them do that by stripping away the operational toil. When faced with the grim reality of building, securing, and maintaining a bespoke infrastructure stack from scratch, 95% of teams will gladly adopt the standardized tools.

For the platform team, the Paved Road provides deep focus. We do not have to support every database engine on the market. We can deeply engineer, optimize, and secure a narrow set of tools, which maximizes our operational leverage. When a zero-day hits a core library, we patch the Paved Road template once and instantly secure the vast majority of the company's production workloads.
