# Security model

## Public repository scope

This repository contains architecture and workflow documentation only. It must
never contain:

- bootstrap credentials;
- model-provider tokens;
- GitHub or Coolify tokens;
- official CLI credential files;
- private SSH keys;
- internal application identifiers;
- workstation-specific paths;
- copied terminal transcripts containing authentication material; or
- the private self-contained bootstrap handoff.

## Credential onboarding

The setup agent performs one-time onboarding through official mechanisms:

- GitHub authentication through GitHub CLI's official browser flow or another
  documented native GitHub CLI mechanism supported by the selected environment;
- Coolify authentication through the official CLI context mechanism; and
- model-provider configuration through the selected harness's documented native
  provider and secret settings.

Credentials are never pasted into application prompts or runtime instructions.
When a value must cross an interactive boundary, use the platform's native
hidden-input or protected secret facility and avoid command echo, shell history,
task transcripts, and logs.

## Runtime restrictions

The runtime may consume existing official CLI authentication but must never:

- initiate login;
- request a token;
- create or update a credential store or Coolify context;
- reveal shared-variable values;
- use sensitive-output or debug-log modes;
- transfer deployment credentials into application source; or
- work around failed authentication with a wrapper or alternate API client.

## Application secrets

Deployment-provider secrets belong in the deployment platform's native shared or
resource variable storage. Server-side applications receive only the variables
they require. Secrets are runtime-only where possible and must not reach build
arguments, browser code, logs, health endpoints, diagnostics, or committed files.

## Permission validation

Authentication alone does not prove sufficient authorization. The bootstrap and
runtime separately verify read, configuration, deployment, and operational-log
abilities without changing production resources.

Because authorization middleware order can change between releases, expected
negative-probe behavior is validated against the live official CLI/server stack.
Unexpected success, authentication failure, authorization failure, or a different
error is not treated as a pass.

## Reporting security issues

Do not open a public issue containing credentials, private infrastructure
addresses, configuration dumps, or log excerpts with sensitive content. Revoke
any accidentally exposed credential before sharing a sanitized report.

