# Portable App Builder for Coolify

This repository documents a harness-agnostic workflow that turns one application
request into a tested application, a private GitHub repository, and a verified
Coolify deployment.

The intended steady-state interaction is one prompt:

```text
BUILD APP COOLIFY:

[Describe the application to build.]
```

That prompt is sufficient only **after a one-time secure bootstrap** has
configured and verified the selected coding harness, GitHub CLI, and the official
Coolify CLI. A completely fresh computer still requires the user to approve
GitHub authentication and securely supply deployment credentials once.

## What this project achieved

- Separated bootstrap responsibilities from future application tasks.
- Kept runtime instructions independent of any particular coding harness.
- Used the official GitHub CLI and official Coolify CLI instead of custom glue.
- Published applications to private repositories under a required GitHub owner.
- Created or safely reused normal branch-tracking Coolify applications.
- Routed applications through per-repository LAN hostnames without fixed host
  ports.
- Added authoritative container health checks and multi-cycle production
  verification.
- Added optional server-side AI configuration through Coolify shared variables
  without exposing provider credentials to source code or browsers.
- Strengthened completion so HTTP 200 or a single healthy observation is not
  mistaken for a finished application.

## Architecture

```text
one-time setup agent
  ├─ configures a user-selected coding harness through documented native settings
  ├─ installs runtime instructions through the harness's native instruction scope
  ├─ authenticates the official GitHub and Coolify CLIs
  └─ validates the restarted harness's real command environment

future application task
  ├─ receives only the installed runtime instructions and BUILD APP COOLIFY prompt
  ├─ builds, tests, and containerizes the application
  ├─ creates and pushes a private GitHub repository on main
  ├─ creates or reuses the Coolify application through the official CLI
  ├─ deploys through the official CLI
  └─ verifies stable rendered production behavior
```

## Design boundaries

This project intentionally does not use:

- credential brokers or wrapper CLIs;
- helper deployment scripts or direct Coolify API clients;
- plugins, hooks, skills, MCP servers, or custom slash commands;
- generated GitHub Actions or webhook relays;
- fixed public host-port mappings; or
- credentials in prompts, repositories, runtime instructions, or browser code.

The private bootstrap used on the reference installation is deliberately **not
included** here because it contains deployment and model-provider credentials.
This repository explains the design and validation contract, not those secrets.

## Documentation

- [Architecture and trust boundaries](docs/ARCHITECTURE.md)
- [Bootstrap procedure](docs/BOOTSTRAP.md)
- [Application workflow](docs/WORKFLOW.md)
- [Production validation contract](docs/VALIDATION.md)
- [Security model](docs/SECURITY.md)
- [Problems found and lessons learned](docs/LESSONS-LEARNED.md)
- [Change history](CHANGELOG.md)

## Official components

- [GitHub CLI](https://cli.github.com/)
- [Coolify](https://coolify.io/)
- [Official Coolify CLI repository](https://github.com/coollabsio/coolify-cli)

`coollabsio/coolify-cli` is the project name. The executable is `coolify`; it is
not an npm package and is never invoked as `coollabsio`.

## Status

The reference installation was validated with Coolify CLI 1.8.0 and Coolify
server 4.3.14. Live detected capabilities remain authoritative because CLI and
server behavior can change.
