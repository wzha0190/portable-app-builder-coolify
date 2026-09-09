# Security model

## Credential separation

The administrator environment may hold GitHub organization administration and
Coolify credentials. The remote development environment needs only repository
access through GitHub.

The remote environment must not receive:

- a Coolify token or CLI context;
- private deployment-network addresses;
- deployment-provider credentials;
- Coolify database or operational logs; or
- secret values stored in Coolify.

## GitHub App boundary

Install the Coolify GitHub App only for the repositories it must read. Use the
GitHub App's authenticated webhook for branch deployments and keep TLS
verification enabled.

The public webhook endpoint should expose only the required Coolify webhook
route. The dashboard and general management API can remain private.

## Application secrets

Store application secrets in Coolify's native shared or resource variable
storage. Inject only the variables required by the server-side application.

Secrets must not appear in:

- source or commit history;
- GitHub issues or pull requests;
- client-side bundles;
- container build arguments unless explicitly required and protected;
- logs, health responses or diagnostics; or
- screenshots and ordinary development summaries.

## Public repository scope

This documentation repository contains architecture and operating guidance only.
Report security issues using sanitized evidence. Revoke any credential exposed in
an issue, commit or transcript before sharing the report.
