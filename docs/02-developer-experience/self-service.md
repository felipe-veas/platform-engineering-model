# Self-Service in Production Systems

## 1. The Operational Problem: The Provisioning Bottleneck

Waiting on infrastructure is a massive bottleneck for any scaling engineering org. When a product team needs a new Kafka topic, a Postgres database, or a fresh namespace, they hit a wall waiting for operations to act.

The root cause is treating infrastructure provisioning as a bespoke, manual chore. An SRE has to read a Jira ticket, translate the request into Terraform, provision the resources, configure the VPC, attach IAM policies, and hook up Datadog monitors. This loop is slow, prone to human error, and demands deep context. Developers end up waiting weeks just to get a database, which tanks velocity and burns trust between product and infrastructure teams.

## 2. The Incorrect Solution: "Shadow IT" or ITIL Catalogs

To clear the backlog, leadership usually attempts one of two flawed fixes.

First is the "Shadow IT" approach, often disguised as developer empowerment. The org hands out raw AWS or GCP console access and tells developers to build what they need. Lacking operational experience, developers click through the cloud console to manually spin up resources just to get unblocked.

The second fix is a heavy ITSM catalog—the dreaded Jira service desk. Developers are forced to fill out massive forms detailing CPU requirements and network topologies. These tickets route to an architecture board for manual review before landing squarely at the bottom of the SRE backlog.

## 3. The Reliability Risks of Unconstrained Access and Bureaucracy

Both of these models wreck system reliability and security.

Wide-open console access is an operational disaster. It immediately causes IAM sprawl. Services get `AdministratorAccess` because developers don't have the time to learn least-privilege scoping. It inevitably leads to public S3 buckets, unencrypted volumes, and ballooning cloud bills. Worst of all, it generates a fragile web of snowflake infrastructure. Because the resources were built via manual UI clicks, you cannot reliably rebuild, scale, or restore them during a regional outage.

On the other hand, the bureaucratic ITIL catalog structurally enforces SLA breaches. Because requesting a new microservice is so painful, developers just cram new features into existing monoliths to avoid the ticketing process. It breeds an adversarial culture where SREs are seen entirely as gatekeepers instead of partners.

## 4. The Platform Approach: Self-Service as a Safety Mechanism

In a mature platform, self-service is not about developer convenience. It is a core mechanism for operational safety and architectural control.

A real self-service model provides an API or developer portal (like Backstage or Crossplane) so teams can provision infrastructure autonomously, instantly. Crucially, this is not raw cloud access. The platform exposes high-level domain abstractions. A developer does not provision an "AWS RDS Instance with custom VPC peering and IAM instance profiles." They request a "High-Availability PostgreSQL Database."

The platform control plane catches that declarative request and stamps out a validated, secure-by-default infrastructure template. The platform automatically encrypts the volume at rest, places it in a private subnet, attaches least-privilege IAM roles, and wires up standard Prometheus alerts. We encode our operational best practices directly into the provisioning engine.

## 5. Emphasizing Decision-Making and Behavior

Real self-service rewires the behavioral dynamics of the engineering org. It completely decouples infrastructure scaling from the SRE team's headcount.

For the platform team, the daily question shifts from "How do we build this database for Team A?" to "How do we build a highly reliable database abstraction that any team can safely consume?" SREs act like product managers, continuously hardening the reliability of their self-service modules.

For product engineers, self-service provides bounded autonomy. They have the freedom to spin up whatever they need, whenever they need it, provided they stay on the paved path. This strips away the coordination overhead of requesting infrastructure. By forcing all provisioning through validated self-service tooling, we guarantee every new resource is compliant, secure, and observable from day one. We shift reliability left, killing massive categories of incidents before the code even ships.
