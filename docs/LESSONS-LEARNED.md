# Problems found and lessons learned

The workflow evolved through repeated real-world failures across more than one
coding harness. The important corrections are collected here.

## Personal GitHub account versus required owner

Being authenticated as a person does not mean the repository should be created
under that person's account. The required owner must be explicit, and the runtime
must verify membership, repository-creation policy, private visibility, remote
URL, and push access before publication.

## Bootstrap shell success is not harness success

GitHub and Coolify authentication succeeded in an external shell but failed
after restart inside harness command environments. Post-restart checks through
the harness's normal command tool are now authoritative. External-shell checks
are comparison evidence only.

## Setup and runtime were being conflated

Earlier drafts allowed the first application task to become credential onboarding.
The corrected design makes the setup agent own installation and authentication.
Runtime can only preflight existing state and report `bootstrap incomplete`.

## Harness-specific wording harmed portability

References to particular editors, agents, terminal modes, paths, model limits,
and approval modes were removed. The bootstrap now discovers and uses only the
selected harness version's documented native mechanisms.

## Official project name was confused with an executable

`coollabsio/coolify-cli` is the repository/project name. The executable is
`coolify`. npm installation and `npx coolify-cli` are not the official workflow.

## Authentication did not prove deployment authority

Context verification and resource listing did not demonstrate configuration,
deployment, or log access. Non-mutating negative probes through the official CLI
were added to test those authorization paths without touching real resources.

## Server validation is asynchronous

The validation command can return after merely starting validation. The workflow
must poll official server metadata until the server is both reachable and usable.

## DNS and proxy assumptions were untested

The workflow depended on wildcard DNS and a portless LAN reverse proxy but did
not test them during bootstrap. It now validates both before accepting the host.

## Coolify domain syntax carries routing metadata

The application domain requires the internal container port suffix for routing,
while users still visit a portless URL. Omitting the suffix can produce proxy
404/503 responses. This is not a fixed public host-port mapping.

## Duplicate health checks caused healthy apps to disappear

An aggressive platform-level probe duplicated the application's container health
check and could remove a functioning container from routing. The corrected design
uses one authoritative tolerant Docker/Compose health check and externally tests
`/healthz`.

## Tags are not placement or source proof

A managed tag can show workflow ownership, but an application can later move.
Placement is reasserted through the official environment operation and the primary
destination is independently verified. Hidden source fields are not invented.

## Some official read models omit important fields

Application reads may omit host-port mappings, placement identifiers, and source
identifiers. Desired safe state is therefore authoritatively reapplied where the
official update operation supports it, then verified through observable behavior.

## Deployment logs and application logs serve different purposes

Deployment logs diagnose builds and orchestration. Application logs diagnose a
running container. Stopped-container logs may be unavailable. Debug-log modes
that expose hidden commands are never used as a workaround.

## A healthy status or HTTP 200 is not completion

The workflow now reconciles every prompt requirement, verifies rendered behavior,
checks the deployed commit, observes multiple health cycles, and tests persistence
across a normal redeployment.

## Repair-to-completion is not proof of one-shot reliability

One-shot capability requires a fresh empty-project run to reach verified stable
production without corrective user interaction. A repaired run is useful evidence
but not that proof.

