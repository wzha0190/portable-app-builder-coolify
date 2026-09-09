# Setup and reproduction

Reproduce this workflow with authorized GitHub and Coolify infrastructure.

## Prerequisites

- a GitHub organization and account authorized to create private repositories;
- a Coolify installation with a usable deployment server;
- a Coolify GitHub App installed for the organization;
- a public HTTPS webhook endpoint reachable by GitHub;
- Git on the development computer; and
- DNS and reverse-proxy routing when the application must be publicly reachable.

The official GitHub and Coolify documentation is authoritative for installing
the GitHub App and enabling Auto Deploy.

## 1. Configure the GitHub App once

Create or reuse one Coolify GitHub App for the organization. Select the public
webhook endpoint, install the app for the required repositories and enable only
the permissions needed by the intended deployment features.

Verify webhook delivery before relying on automatic deployment.

## 2. Provision a repository and resource

Follow [Administrator provisioning](PROVISIONING.md). Create the private
repository, initial `main` branch, GitHub work item and matching Coolify
application. Configure Auto Deploy, build settings, domain, runtime variables,
storage and health behavior.

## 3. Develop remotely

From a computer outside the deployment LAN:

1. authenticate to GitHub using a supported mechanism;
2. clone the prepared repository;
3. read the linked GitHub issue or repository specification;
4. implement and test the complete work item;
5. commit the result; and
6. push `main`.

Do not configure a Coolify token or expose the private Coolify API to this
computer.

## 4. Observe Auto Deploy

Confirm that the GitHub push event starts a Coolify deployment and that the
deployment uses the expected revision. If the production URL is public, verify
the rendered application from the remote computer. Complete control-plane checks
from the deployment network.

## 5. Repeat

Create a new GitHub issue or pull request for later changes. Each merge or push
to `main` follows the same standard Auto Deploy path without new Coolify setup,
unless the infrastructure contract itself changes.

## Public documentation hygiene

Do not publish tokens, CLI credential files, private repository contents,
internal addresses, application identifiers or raw operational logs. Use
placeholders and sanitized screenshots when documenting a private installation.
