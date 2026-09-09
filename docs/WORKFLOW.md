# Development and deployment workflow

## 1. Record the work

The application brief, constraints and acceptance criteria are recorded in a
GitHub issue or a versioned repository specification. The work item is the
authoritative requirement source.

The development client receives only:

- the repository URL;
- the work-item reference;
- the production branch (`main`); and
- the public application URL when remote production verification is required.

## 2. Prepare the control plane

Before remote development starts, the administrator:

1. creates the private repository under the required organization;
2. creates an initial `main` branch;
3. grants the existing Coolify GitHub App access to the repository;
4. creates one Coolify application from that repository;
5. configures the project, environment, server, branch and build settings;
6. configures domains, persistent storage and runtime variables as required;
7. enables Auto Deploy; and
8. verifies that GitHub can reach the configured webhook endpoint.

The initial commit may contain only ordinary repository metadata and the work
specification. It does not need to masquerade as a deployable application.

## 3. Build from any network

The remote development client clones the prepared repository and verifies that
`origin` points to it. It implements every acceptance criterion, adds appropriate
tests, runs the production build locally and checks container behavior when the
repository uses containers.

A normal single-service web application should:

- listen on the port configured in Coolify;
- bind to `0.0.0.0` inside the container;
- provide a meaningful `GET /healthz` endpoint; and
- define a tolerant container health check.

The client scans staged content for secrets and generated artifacts, commits the
completed implementation and pushes `main`.

## 4. Deploy through the GitHub App

The push event is delivered to Coolify through the installed GitHub App. Coolify
pulls the current `main` revision and runs its normal build-and-deploy pipeline.

No remote command calls the Coolify API. No deploy webhook, GitHub Action,
wrapper, credential broker or copied Coolify token is required for this path.

## 5. Verify production

When the application has a public hostname, the development client waits for the
new deployment and verifies the rendered application through that hostname.
GitHub branch state identifies the expected revision.

Operational deployment state and logs remain available to the LAN-side
administrator. If the deployment fails for infrastructure reasons, the
administrator diagnoses Coolify without giving the remote client management
credentials.

For private-only applications, the administrator performs production-network
verification because the remote client cannot reach the private hostname.

## 6. Continue normal development

Subsequent changes follow the same standard loop:

```text
GitHub issue or pull request
    → development and tests
    → push or merge to main
    → GitHub App webhook
    → Coolify Auto Deploy
    → production verification
```

Additional Coolify provisioning is needed only when infrastructure requirements
change, such as a new service, domain, volume or secret.
