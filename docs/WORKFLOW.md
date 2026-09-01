# Application workflow

The workflow activates only when a user message begins with:

```text
BUILD APP COOLIFY:
```

Ordinary coding tasks do not publish or deploy merely because they mention
GitHub, containers, or Coolify.

## 1. Establish authorized project scope

The harness determines the authorized filesystem scope at runtime. An existing
opened project supplies the project root. Otherwise, a new project is created
only within a directory the harness identifies as authorized.

Paths are never inferred from documentation, previous computers, examples, or
model memory.

## 2. Run the same-environment preflight

Through the harness's normal command tool, the runtime independently verifies:

- required package tooling when applicable;
- Git and GitHub CLI resolution;
- GitHub authentication and identity;
- active membership and repository-creation authority for the required owner;
- HTTPS Git transport to an accessible private repository;
- official Coolify CLI resolution and version;
- Coolify context authentication;
- project, environment, server, destination, source, and application inventory;
- server reachability and usability;
- non-mutating write, deploy, and log authorization probes;
- LAN wildcard DNS and reverse-proxy reachability; and
- required shared-variable metadata without retrieving secrets.

Failure stops the run as `bootstrap incomplete`. Runtime never authenticates or
requests credentials.

## 3. Build the complete application

Every material prompt requirement becomes an explicit acceptance check. The
runtime implements the complete requested product, adds relevant automated tests,
runs formatting/lint/type checks where applicable, runs the production build,
and exercises representative local behavior.

A normal single-service web application:

- listens on `0.0.0.0:3000`;
- exposes an unauthenticated `GET /healthz` endpoint;
- includes a production Dockerfile; and
- defines a tolerant, meaningful container `HEALTHCHECK`.

The health endpoint validates required application and persistence readiness,
not merely that a process is listening.

## 4. Publish to GitHub

The runtime:

1. derives and normalizes a repository name;
2. initializes Git when needed while preserving existing history and remotes;
3. establishes `main` safely;
4. reviews staged content for secrets and generated artifacts;
5. creates a private repository under the configured required owner;
6. configures or verifies the exact `origin`;
7. commits and pushes `main`; and
8. verifies private visibility, upstream tracking, clean worktree state, and SHA
   equality between local and remote `main`.

The authenticated personal account is the acting identity, not an automatic
repository owner. Publication never silently falls back to that personal
account.

## 5. Provision and deploy with the official Coolify CLI

The runtime resolves live identifiers and verifies that the private repository
and `main` branch are visible through the configured GitHub App/source.

It reuses an existing application only when ownership is unambiguous. A managed
tag is an ownership marker, not proof of current project placement or source.
Current placement is reasserted through the official environment-move operation,
and the primary destination is independently validated.

For a new application, the workflow supplies the resolved server, project,
environment, destination, source, repository, branch, exposed port, domain, and
ownership tag. It does not deploy until resource variables and storage are ready.

The normal domain value includes the internal container port as routing metadata:

```text
http://<repository-name>.<lan-address>.sslip.io:3000
```

Users visit the portless hostname. No fixed host-port mapping is allocated, and
any possible stale mapping is authoritatively cleared before deployment.

The duplicate platform application-health switch remains disabled. Container
health is owned by the Dockerfile or Compose definition, while `/healthz` is
also tested externally through the production hostname.

## 6. Optional runtime AI

Application code uses a provider-neutral server-side contract:

```text
AI_API_KEY
AI_BASE_URL
AI_MODEL
```

When the application prompt requires AI, resource-level variables reference
environment-scoped shared variables through Coolify's native interpolation.
The key is runtime-only and build-time-disabled. It never enters source code,
browser bundles, logs, health responses, diagnostics, or task state.

Successful AI validation requires genuine provider-backed behavior. A silent
heuristic fallback does not count unless the prompt permits it and the UI labels
it clearly.

## 7. Monitor and repair

Deployment state and deployment logs are the primary evidence for build and
orchestration failures. Application logs are used for running containers. Debug
log modes that can expose hidden commands are prohibited.

Recoverable failures are fixed, committed, pushed, redeployed, and reverified
automatically. The runtime reports a blocker only for a missing decision,
credential, permission, or external infrastructure failure it cannot safely
correct within the authorized workflow.
