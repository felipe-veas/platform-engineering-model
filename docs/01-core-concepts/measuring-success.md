# Measuring Platform Success

## 1. The Operational Problem: Misaligned Metrics

A classic trap in platform engineering is measuring success based on infrastructure outputs instead of developer outcomes.

Platform teams love to report metrics like "Kubernetes clusters managed," "Terabytes of logs ingested," or "99.99% uptime on the CI runners." These are decent operational indicators for the infrastructure itself, but they are completely disconnected from the actual value delivered to the business.

A platform can boast five nines of availability, but if an engineer still needs three weeks of manual approvals and 500 lines of bespoke Terraform to deploy a simple background worker, the platform is failing. The business does not pay for Kubernetes clusters; it pays for feature velocity and operational safety.

## 2. The Incorrect Solution: Vanity Metrics and Mandates

When teams fail to measure developer outcomes, they usually default to vanity metrics or mandated adoption quotas.

Management might track the "Percentage of services migrated to the new platform." If that number hits 100% simply because leadership mandated the migration, it tells you absolutely nothing about whether the platform actually improved the engineering culture. Forcing 100% adoption of a terrible platform actively damages the company.

## 3. The Platform Approach: Outcome-Oriented Metrics

A mature platform organization tracks success by how effectively it lowers cognitive load and accelerates safe deployments. We rely on three frameworks to quantify this: DORA Metrics, the SPACE Framework, and specific Platform KPIs.

### DORA Metrics (Delivery Performance)

The DevOps Research and Assessment (DORA) metrics remain the gold standard for measuring delivery performance. A good platform must push these numbers in the right direction across all product teams:

* **Deployment Frequency:** How often does the org ship code to production? The platform should make deployments so routine and safe that teams ship multiple times a day.
* **Lead Time for Changes:** How long does it take a commit to reach production? Automated testing and efficient CI/CD pipelines should shrink this from weeks to minutes.
* **Time to Restore Service (MTTR):** How fast do we recover from an outage? Standardized observability, GitOps rollbacks, and automated runbooks should aggressively drive down MTTR.
* **Change Failure Rate:** What percentage of deployments break production? Automated guardrails, canary rollouts, and Policy-as-Code should push this near zero.

### The SPACE Framework (Developer Productivity)

Where DORA tracks delivery, SPACE tracks developer productivity and satisfaction. We monitor:

* **Satisfaction & Well-being:** Do engineers actually like using the tools? Do they feel the platform helps them ship? We measure this through regular Developer Experience (DevEx) surveys and internal Net Promoter Scores (NPS).
* **Performance:** Is the platform helping teams ship more reliable features?
* **Activity:** Raw output metrics (like PR throughput), though these are less critical for platform teams to focus on directly.
* **Communication & Collaboration:** Does the platform (e.g., via an internal developer portal) make it easier to discover and consume internal APIs?
* **Efficiency & Flow:** How often do developers break their flow state waiting on infrastructure provisioning, slow CI builds, or manual CAB approvals?

### Key Platform KPIs

Beyond DORA and SPACE, we track internal KPIs to measure the actual friction of our "Paved Road":

* **Time-to-Hello-World (TTHW):** This is the ultimate test of platform usability. How long does it take a new hire, on their first day, to deploy a secure, observable service to a production-like environment? If this takes more than a few hours, your self-service story is broken.
* **Paved Road Adoption Rate (Voluntary):** What percentage of *new* services voluntarily choose the Paved Road over building bespoke infrastructure? High voluntary adoption proves product-market fit.
* **Support Ticket Volume:** A working platform is self-service. If your SREs spend half their week answering "How do I configure this?" in Slack or Jira, your abstractions or documentation are failing.

## 4. Emphasizing Decision-Making and Behavior

Measuring outcomes completely reorients the platform roadmap.

If we see the Change Failure Rate climbing because teams keep pushing broken Dockerfiles, we do not add a manual approval step (which would destroy Lead Time). Instead, we build an automated container scanner and a mutating admission webhook to catch and reject the bad configs silently.

If TTHW is taking three days because developers cannot figure out how to configure Datadog, we do not write a 20-page wiki on observability. We build a standardized Terraform module that automatically wires up best-practice dashboards for every new repo.

By ruthlessly optimizing for developer velocity, low cognitive load, and automated safety, the platform stops being an IT cost center and becomes a massive competitive advantage.
