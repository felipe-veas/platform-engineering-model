# GitOps as an Operational Control Mechanism

## 1. The Operational Problem: Manual Changes in Production

Unmanaged change causes a staggering percentage of production incidents. In most outages, the application code itself isn't broken. Instead, the environment running that code changed in an undocumented, untracked way.

Picture an active incident response: an engineer drops into a shell and uses a CLI to bump replica counts, hot-patch a network policy, or inject a missing environment variable. That manual tweak might stop the bleeding, but it leaves a massive operational trap behind. The environment has drifted. You have no audit trail, no peer review, and no clear path to rebuild that cluster for disaster recovery. When the next automated deployment rolls out, it overwrites the manual fix. The incident recurs, or the pipeline crashes because the live state no longer matches the expected state.

## 2. The Incorrect Solution: CI Pipelines as God-Mode Script Runners

To block manual changes, teams often route all infrastructure updates through Continuous Integration (CI). They revoke developer access and build sprawling Jenkins or GitHub Actions workflows that run imperative commands like `terraform apply` or `kubectl apply`.

This looks like progress, but it is the wrong operational model. The CI pipeline just becomes a god-mode script runner. It operates imperatively—shoving instructions at a cluster and hoping they land cleanly. If a deploy fails halfway through, you are left with a fractured, unknown state. Worse, this setup conflates building artifacts with managing state. If someone manually modifies the cluster, the CI pipeline has no idea drift occurred until the next time a job runs. You have no continuous state validation and zero self-healing.

## 3. The Reliability Risks of Imperative Automation

Imperative pipelines kill reliability. When you push deployments this way, you can never answer the most basic operational question: "What is actually running in production right now?" Your answer is just an assumption based on the last green pipeline run, which is useless if someone changed a setting out-of-band.

Audits become a nightmare. Security teams have to dig through CI logs and pray the scripts executed exactly as intended. Rollbacks become terrifying. Instead of reverting to a known-good state, responders have to run "reverse scripts" or trigger complex undo pipelines. Under the pressure of an outage, these reverse operations frequently fail and compound the damage.

## 4. The Platform Approach: GitOps as a Control Plane

The reliable way to manage change is GitOps. We treat it as an automated control mechanism, not just a deployment tool. Git acts as the single, immutable source of truth for the declarative state of the entire environment.

We do not push deployments from CI. Instead, a continuous reconciliation loop (like ArgoCD or Flux) runs inside the cluster and polls the Git repository. When the agent sees a diff between the Git commit and the live cluster state, it automatically pulls and applies the change. If an engineer manually alters a live resource, the agent instantly detects the drift and overwrites it. It self-heals the environment back to the declared state in Git.

GitOps acts as the control plane. It hard-separates CI (building artifacts) from CD (managing state) and guarantees production is an exact mirror of your audited repository.

## 5. Emphasizing Operational Behavior and Decision-Making

GitOps changes the operational posture of the engineering org. We stop "executing changes" and start "declaring intent."

The Pull Request becomes the actual change record. Instead of opening an IT ticket or running a risky CLI command, developers modify a declarative YAML file and submit a PR. Code review doubles as operational review. The team sees the exact state change before it hits the cluster.

This shift lowers cognitive load and reduces human error. It also enforces blameless operations through tooling. If a merged config takes down a service, responders don't frantically write rollback scripts; they just run `git revert`. By routing every change through version control and continuous reconciliation, we eliminate the entire category of incidents caused by untracked drift.
