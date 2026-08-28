![Evolve](https://docs.evolve-platform.com/img/evolve-poster.png)

# Evolve

Evolve is [Lab Digital](https://www.labdigital.nl/)'s composable commerce
platform, built for the agentic era. One GraphQL Federation API serves the
storefront, the mobile app and AI agents, so pricing, promotions and business
rules are the same on every channel — including the ones a customer reaches
through an assistant rather than a browser.

It is a set of building blocks rather than a monolith. A backend framework of
swappable integrations — commerce engines, CMSes, search, payment providers,
clouds — a Next.js storefront with its design system, a federation gateway, and
the tooling to ship all of it. A project picks the pieces it needs and wires
them together, and can run alongside an existing commerce engine instead of
replacing it outright.

| | |
| --- | --- |
| **[docs.evolve-platform.com](https://docs.evolve-platform.com)** | Platform documentation — architecture, getting started, how-to guides, GraphQL API |
| **[evolve-platform.com](https://www.evolve-platform.com/)** | What it does, and who it is for |
| **[demo.evolve-platform.com](https://www.demo.evolve-platform.com/en)** | A storefront you can click through |

## What is here

Evolve itself is not open source. The parts that are useful without it are, and
this is where they live — every repository below stands on its own, and none of
them needs an Evolve licence to be worth running.

**Deployment and runtime**

| Repository | What |
| --- | --- |
| [evolve-deploy](https://github.com/evolve-platform/evolve-deploy) | Stateless deploys to AWS, GCP, Azure and Kubernetes. No state file, no lock — it reads a config, compares it against what is actually running, and rolls out the difference. Docs at [deploy.evolve-platform.com](https://deploy.evolve-platform.com) |
| [setup-evolve-deploy](https://github.com/evolve-platform/setup-evolve-deploy) | GitHub Action that installs the `evolve-deploy` binary on a runner |
| [staticfiles-sync](https://github.com/evolve-platform/staticfiles-sync) | Syncs a directory to an S3, GCS or Azure bucket, once, guarded by a lockfile — for copying a build's static assets out at container startup |
| [docker-base-images](https://github.com/evolve-platform/docker-base-images) | Multi-arch Node.js base images with pnpm, `dumb-init`, curl and unzip |

**GraphQL**

| Repository | What |
| --- | --- |
| [hive-router-federated-token](https://github.com/evolve-platform/hive-router-federated-token) | A Rust plugin for [Hive Router](https://the-guild.dev/graphql/hive/docs/router) that ports `@labdigital/federated-token`'s gateway behaviour off Apollo Server, so a router can replace an Apollo Federation gateway without touching a single subgraph |

**CI**

| Repository | What |
| --- | --- |
| [turbo-detect-changes](https://github.com/evolve-platform/turbo-detect-changes) | Which packages of a turborepo a commit affected — comparing against the last commit the workflow *succeeded* on, and counting the files turbo cannot see |
| [registry-login](https://github.com/evolve-platform/registry-login) | Exchanges a GitHub Actions OIDC token for an npm registry token |
| [terraform-github-workflows](https://github.com/evolve-platform/terraform-github-workflows) | Reusable workflows and synced repository files for our Terraform module repositories |

**Terraform modules**

Small, single-purpose modules — the shapes an Evolve service is deployed as, and
the commercetools client credentials it reads.

| Cloud | Modules |
| --- | --- |
| AWS | [ecs-service](https://github.com/evolve-platform/terraform-aws-ecs-service) · [eventbridge-sqs](https://github.com/evolve-platform/terraform-aws-eventbridge-sqs) · [ct-client-secret](https://github.com/evolve-platform/terraform-aws-ct-client-secret) |
| Azure | [app-container](https://github.com/evolve-platform/terraform-azurerm-app-container) · [app-container-job](https://github.com/evolve-platform/terraform-azurerm-app-container-job) · [function-app](https://github.com/evolve-platform/terraform-azurerm-function-app) · [key-vault](https://github.com/evolve-platform/terraform-azurerm-key-vault) · [ct-client-secret](https://github.com/evolve-platform/terraform-azurerm-ct-client-secret) |
| Google Cloud | [cloud-run-service](https://github.com/evolve-platform/terraform-google-cloud-run-service) · [ct-client-secret](https://github.com/evolve-platform/terraform-google-ct-client-secret) |

## Elsewhere

The framework, the storefront and the reference implementations are private. If
you would like to see under the hood, or talk about running Evolve, get in touch
— [info@labdigital.nl](mailto:info@labdigital.nl).

For more open source from Lab Digital, much of which Evolve itself depends on,
see [labd](https://github.com/labd) and
[mach-composer](https://github.com/mach-composer).
