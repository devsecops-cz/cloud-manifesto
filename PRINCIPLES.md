# Principles

Each value in [The Cloud Ownership Manifesto](README.md) has one principle that says what it means in daily work.

## 1. Clear ownership levels

We call this model **Four-Level Ownership**: every cloud resource belongs to exactly one of four levels, and every level is owned by a role, not by a person.

| Level | Scope | Examples | Owning role |
| --- | --- | --- | --- |
| Global | Foundation of the whole cloud | Management groups, subscriptions or accounts, policy, hub network, identity, central logging | Cloud Architect |
| Shared | Components used by several projects | Shared AKS/EKS cluster and its add-ons, container registry, APIM, CI/CD templates | Cloud Architect and Senior Platform Engineer |
| Project | Infrastructure of one project, shared by its components | Spoke network, database server, Key Vault, cluster namespace, alerts | Cloud Engineer |
| Component | Infrastructure of one service | The service's database, queue, storage, managed identity, app, Helm chart | Cloud Engineer |

- The level is set by who uses a resource, not by its technology. Used by one service → Component; by several services of a project → Project; by several projects → Shared; the foundation → Global.
- Everyone works on their own level. The architect does not write component Terraform; a junior does not choose the network topology.
- A change above your level is a request to the role that owns that level, never a direct edit.

DevOps is our culture, not a job title. Roles are named after what they own:

- **Cloud Architect** — owns Global and co-owns Shared. Designs the target architecture, sets the standards and approves exceptions to them.
- **Senior Platform Engineer** — co-owns Shared. Runs the shared platform, maintains the modules and pipeline templates, and mentors the Cloud Engineers.
- **Cloud Engineer (middle, junior)** — owns Project and Component together. Builds and runs a project's infrastructure and pipelines from inside its development team.

## 2. Code and pipelines

The state of the infrastructure is what is declared in version control and applied by a pipeline.

- Infrastructure is written in Terraform/OpenTofu; cluster configuration is delivered by GitOps.
- A manual change is drift: it is reverted or brought into code.
- Emergency console access is logged, and the change is written into code within one working day.

## 3. Secure by default

Security is built into the modules and pipelines, so the secure way is also the easiest way.

- Every pipeline runs IaC, secret and image scanning plus policy checks; a failed check stops the change.
- Access goes through groups, with least privilege and an expiry date.
- Each environment has its own subscription or account. Environments that share network or identity are treated as one environment at the strictest classification.

## 4. Self-service and automation

What is done twice gets automated or turned into a module or template.

- Developers get infrastructure through approved modules and self-service, not through tickets.
- A ticket is the fallback, and a recurring ticket is a backlog item to remove it.

## 5. Enabling your teams

We make the client's teams stronger rather than dependent on us.

- We work in the client's repositories, tools and rituals, alongside their internal teams.
- Knowledge is handed over continuously through pairing, reviews and documentation, not at the end.

## 6. Cost as a design input

Cost is decided when infrastructure is designed, not discovered on the invoice.

- Every resource carries owner, level, environment and cost centre tags.
- Every pull request with infrastructure changes shows a cost estimate.
- Every project has a budget with alerts.

## 7. Written runbooks

If only one person knows how something works, it is not finished.

- Every production component has a runbook: how to deploy, roll back, diagnose and recover.
- Documentation lives in the repository next to the code; important decisions are recorded as ADRs.

## 8. AI that proposes, humans who decide

AI agents speed up our work; people stay accountable for every change.

- AI may draft code, plans, reviews and pull requests.
- A human reviews and approves every AI-made change before it is applied.
- AI agents never hold apply rights to production.

## 9. Measured outcomes

We judge our work by results for the client, not by hours or tickets closed.

- We track the four DORA metrics: deployment frequency, lead time for changes, change failure rate and recovery time.
- We also track how long developers wait for an infrastructure change.
- Metrics improve the system; they are never used to rate individuals.
