# Setup and reproduction

This repository documents an approach, not a ready-to-run credential bundle.
Reproduce it with your own authorized accounts and infrastructure.

## Prerequisites

- A coding harness with documented durable instructions, unattended permissions,
  persistent command execution, reliable exit-status capture, and rendered-
  browser testing.
- Git and the official [GitHub CLI](https://cli.github.com/).
- A GitHub account with permission to create private repositories under the
  intended owner.
- A reachable Coolify installation, project, environment, deployment server,
  destination, and GitHub source.
- The official [`coolify` CLI](https://github.com/coollabsio/coolify-cli).
- A wildcard or explicitly managed DNS route for application hostnames.
- A secure credential handoff that is never committed to the project.

## 1. Define a deployment profile

Choose and record non-secret identifiers for:

- required GitHub owner;
- production branch;
- Coolify context name and endpoint;
- Coolify project and environment;
- GitHub App/source name;
- application container port; and
- application-domain convention.

Discover server, destination, source, project, environment, application, and
deployment identifiers dynamically. Do not make workstation paths or copied
UUIDs part of the portable contract.

## 2. Create a confidential bootstrap handoff

Keep provider and deployment credentials in a separate confidential bootstrap
handoff outside every application project. It should explain the setup/runtime
boundary and contain one clearly delimited sanitized runtime payload.

Restrict access using native operating-system permissions. Never publish this
handoff, paste it into a runtime task, or install its confidential section as
harness instructions.

## 3. Configure the selected harness

Use only the installed harness version's current documented native mechanisms:

1. configure the live model/provider;
2. determine sufficient context and output capacity by representative tool use;
3. configure continuation and context management;
4. enable native unattended permissions required by the complete workflow;
5. select a persistent normal command backend;
6. install the sanitized runtime payload through one durable instruction scope;
7. restart the harness and its host application; and
8. verify the effective configuration after restart.

If the harness cannot meet these requirements without a wrapper or custom
integration, treat it as incompatible.

## 4. Authenticate official tools

Authenticate GitHub through GitHub CLI's official workflow in the command
environment the harness actually uses. Configure the official Coolify CLI context
there as well. Do not rely on successful authentication from a different shell.

Keep credentials out of prompts, source files, repositories, logs, shell history,
and global plaintext environment variables.

## 5. Run the post-restart acceptance gate

From a sanitized non-mutating task inside the restarted harness:

- resolve the required executables;
- validate GitHub identity, owner access, and HTTPS Git transport;
- validate the Coolify context and inventory;
- prove required authorization without changing a real resource;
- validate and poll the deployment server until ready;
- test wildcard DNS and reverse-proxy reachability;
- inspect required shared-variable metadata without secret values;
- confirm runtime instructions load in a newly opened project; and
- prove multi-step tool use, continuation, unattended permissions, command output,
  exit status, and rendered-browser capability.

Repair failures through official native mechanisms, restart, and repeat the
entire gate. Do not start an application build until it passes.

## 6. Run a fresh application test

Open an empty authorized project and send one prompt:

```text
BUILD APP COOLIFY:

Build a small production application with at least one persistent or interactive
feature and clear acceptance criteria.
```

Require the runtime to complete implementation, tests, private GitHub publication,
Coolify provisioning, deployment, rendered-browser checks, stability observation,
revision equality, and applicable persistence verification.

Record whether any corrective user interaction occurred. Only an unrepaired fresh
run demonstrates one-shot capability for that harness and environment.

## 7. Capture portfolio evidence

Before sharing results publicly, sanitize all evidence. Useful artifacts include:

- a short screen recording from the single prompt to stable production;
- the generated application's public-safe interface;
- a Coolify status view with identifiers and hostnames redacted;
- GitHub repository visibility and branch evidence without account secrets;
- the final requirement-to-evidence matrix; and
- timing and failure-recovery observations.

Never publish tokens, CLI configurations, private repository contents, internal
addresses, application UUIDs, private hostnames, or raw operational logs.
