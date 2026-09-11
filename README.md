# One-shot GitHub-to-Coolify application delivery

This repository documents one standard workflow for building an application on
another computer and deploying it to Coolify without giving that computer LAN
access.

This README is the authoritative workflow specification. Every application
delivery must keep its repository documentation and confidential execution
record current as the setup, implementation, deployment, or verification state
changes.

## Deployment path

```text
application request
    -> LAN-side setup agent
    -> private GitHub repository + configured Coolify application
    -> one confidential development prompt
    -> development agent on another computer
    -> Git push to main
    -> Coolify GitHub App event
    -> Coolify Auto Deploy
    -> public application verification
```

The development computer does not need GitHub CLI or Coolify access. It uses
ordinary Git with working repository write credentials supplied in the
confidential handoff. GitHub CLI is optional and must not be assumed.

## Information supplied to the setup agent

Provide:

- the application name;
- the complete application requirements and acceptance criteria;
- the intended public hostname;
- required persistent storage or database behavior; and
- required server-side services or runtime variables.

## LAN-side setup

Before producing the development prompt, the setup agent:

1. Creates a private repository under `wzha0190-software-factory`, initializes
   `main` with a non-secret `DEPLOYMENT.md`, and makes `main` the production and
   default branch. This initial commit contains the delivery contract and no
   application implementation.
2. Gives the existing Coolify GitHub App access to that repository.
3. Creates exactly one application in the `Software Factory` project and
   `production` environment from that GitHub App source and branch.
4. Configures the build method, internal application port, public hostname,
   runtime variables, persistent storage, and health behavior required by the
   requested application.
5. Enables Coolify Auto Deploy and verifies the existing GitHub App webhook path
   can receive repository push events. It does not add a separate repository
   webhook, GitHub Actions deployment, relay, or second deployment trigger.
6. Configures an application-visible revision value from Coolify's predefined
   `SOURCE_COMMIT`. The application contract must expose that value through a
   lightweight version endpoint, response header, or rendered version marker so
   the remote development agent can prove which commit is in production. If the
   revision is needed during the image build, the setup agent enables Coolify's
   **Include Source Commit in Build** setting.
7. Prepares and verifies a standard GitHub-supported repository write
   credential that works with ordinary Git and does not require GitHub CLI.
8. Records the completed non-secret setup fields and verification results in
   `DEPLOYMENT.md`, without recording credential values or runtime secrets.
9. Returns one confidential, self-contained development prompt that is also the
   setup execution record.

The setup agent prepares the repository and deployment resource. It does not
implement the application.

## Configuration ownership

For a non-Compose application, Coolify owns the applicable build, domain, port,
environment-variable, persistent-storage, and dashboard health-check settings.
A Dockerfile `HEALTHCHECK` takes precedence over a dashboard health check, so
only one health-check owner is selected and documented.

For a Git-based Docker Compose application, the Compose definition is the source
of truth for service builds, commands, environment references, volumes,
dependencies, networking, and service health checks. The setup agent configures
the Git source, branch, domain, Auto Deploy, variables, and other available
application-level controls in Coolify. The development prompt requires the
development agent to implement the Compose-owned parts. This still creates one
Coolify application even when its Compose definition contains multiple
containers.

Persistent storage is not treated as a backup. When the application request
requires recoverability beyond normal redeployment, the handoff must state the
backup and restore requirement separately.

## Repository authentication

The preferred one-repository handoff credential is a unique SSH deploy key with
write access. A fine-grained personal access token with repository Contents
read/write permission is also valid when the target computer uses HTTPS and the
organization's token policy permits it.

The setup agent verifies the chosen credential by cloning the prepared private
repository and performing a non-mutating push authorization check with ordinary
Git. A successful API request or GitHub login elsewhere is not sufficient.

The confidential prompt tells the development agent how to use the credential
through SSH configuration or a credential helper. It must not place a token or
private key in a Git remote URL. The execution record identifies the credential
type and repository association, and states when it should be revoked or
rotated, without duplicating the secret outside the confidential credential
section.

## Required handoff prompt

The returned prompt contains:

- the repository clone URL and required Git authentication details;
- the production branch, `main`;
- the complete application specification and acceptance criteria;
- the build, port, health, persistence, and runtime-variable contract;
- the required `SOURCE_COMMIT` revision exposure contract;
- the public production URL;
- the completed LAN-side setup actions and their verification results;
- the required application documentation and change-record updates;
- instructions to build, test, commit, and push the complete application; and
- instructions to wait for Auto Deploy, verify production, and automatically
  repair application failures before reporting completion.

The handoff is the complete work order. No second user prompt is required.

Repository credentials in the confidential handoff are for Git access only.
The development agent may configure and use them, but must not copy them into
application files, commits, build artifacts, logs, screenshots, browser code,
or the deployed application.

## Development-agent run

From the single handoff prompt, the development agent:

1. Configures the supplied Git repository authentication using the target
   environment's standard Git mechanism.
2. Clones the prepared repository and confirms the intended remote and `main`
   branch.
3. Reads `DEPLOYMENT.md`, implements every stated requirement, and updates the
   application and deployment documentation for every material implementation
   or configuration decision.
4. Runs relevant tests and a production build, including a local container test
   when the application is containerized.
5. Checks that no supplied credential is part of the staged content.
6. Commits and pushes `main` with ordinary Git.
7. Reads the full remote `main` commit SHA and waits until the public revision
   marker reports that exact SHA.
8. Verifies the rendered application and representative interactions against
   every acceptance criterion.
9. If an application defect is found, fixes it, retests, pushes again, and
   repeats production verification automatically.
10. Returns a completion record containing the final remote `main` SHA,
    production revision, verification time, tests performed, acceptance results,
    and any credential revocation or rotation action still due.

The push is the deployment trigger. The development agent needs no access to
the deployment LAN.

## Completion contract

The run is complete only when:

- the implementation satisfies every requirement;
- tests and the production build pass;
- the working tree is clean and `main` is pushed;
- the public revision marker exactly matches the full current remote `main` SHA;
- health and required persistence behavior pass; and
- the public rendered application and representative interactions work; and
- `DEPLOYMENT.md`, application documentation, and the completion record describe
  the final configuration and verification outcome without exposing secrets.

One-shot means the user supplies the application request once and pastes the
resulting development prompt once. Usable GitHub write access is included in
that handoff.

## Standard mechanisms only

This workflow uses:

- ordinary Git authentication and push;
- a Coolify GitHub App source;
- GitHub push events; and
- Coolify Auto Deploy.

No additional deployment integration is inserted between GitHub and Coolify.

## Required application record

Every prepared application repository starts with `DEPLOYMENT.md`. It records:

- application name, repository, production branch, and public URL;
- Coolify project and environment names, source type, and Auto Deploy state;
- build method, repository/base paths, internal port, and health contract;
- runtime variable names and scopes without secret values;
- persistence mount names and container paths without confidential host details;
- configuration ownership for Coolify, Dockerfile, and Docker Compose settings;
- revision exposure method;
- dated setup and change entries stating what changed, why, and how it was
  verified; and
- current deployment and verification status.

The confidential handoff separately records the secret credential material,
credential lifecycle, internal identifiers when needed, and complete execution
evidence. Secrets never enter `DEPLOYMENT.md`, application documentation, Git
history, logs, screenshots, health responses, version responses, or browser
code.

## Documentation history

### 2026-09-12

- Clarified that a GitHub App application uses its existing webhook path and
  must not receive a duplicate deployment integration.
- Standardized repository write authentication on ordinary Git with a unique
  write-enabled SSH deploy key preferred for one-repository handoffs.
- Documented Dockerfile and Docker Compose configuration ownership.
- Added exact production revision proof using Coolify `SOURCE_COMMIT`.
- Made `DEPLOYMENT.md` and confidential setup/completion records mandatory for
  every application delivery and every material change.

## Official references

- [GitHub remote repository access](https://docs.github.com/en/get-started/git-basics/about-remote-repositories)
- [GitHub repository authentication options](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/managing-deploy-keys)
- [Coolify GitHub App setup](https://coolify.io/docs/applications/sources/github/app)
- [Coolify automatic deployments](https://coolify.io/docs/applications/sources/github/auto-deploy)
- [Coolify environment and predefined variables](https://coolify.io/docs/applications/configuration/environment-variables)
- [Coolify persistent storage](https://coolify.io/docs/applications/configuration/persistent-storage)
- [Coolify health checks](https://coolify.io/docs/applications/configuration/health-checks)
- [Coolify Docker Compose applications](https://coolify.io/docs/applications/builds/docker-compose)

Current official product documentation is authoritative when product behavior
changes.
