# Administrator provisioning

Provisioning prepares one repository and one Coolify application before remote
development begins.

## Installation-wide prerequisites

Configure these once for the Coolify installation:

- a GitHub App installed for the required organization;
- a publicly reachable HTTPS webhook endpoint selected for that GitHub App;
- a Coolify project, environment and usable deployment server;
- DNS and reverse-proxy routing for public application hostnames; and
- native Coolify storage for shared or application-specific secrets.

Follow Coolify's official GitHub App setup and Auto Deploy documentation. Do not
replace the GitHub App with a custom relay.

## Per-application checklist

1. Create a private repository under the required GitHub organization.
2. Create and push an initial `main` branch.
3. Record the application brief and acceptance criteria in a GitHub issue or
   repository specification.
4. Confirm the Coolify GitHub App installation can access the repository.
5. Create exactly one Coolify application from the private repository.
6. Select the intended project, production environment, server and `main` branch.
7. Configure the expected build pack or Dockerfile, exposed container port,
   domain and any required persistent storage.
8. Add runtime variables through Coolify's native variable system. Do not put
   secrets in the repository or work item.
9. Enable Auto Deploy for pushes to `main`.
10. Confirm the GitHub App webhook endpoint is publicly reachable with valid TLS.
11. If the app will be public, configure DNS and the normal HTTPS reverse-proxy
    route before handing off development.
12. Record the repository URL, work-item reference and production URL.

Do not trigger a deployment merely to prove that an empty seed repository can
fail. The first implementation pushed by the development client is the first
meaningful deployment.

## Handoff

The development handoff is ordinary project information:

```text
Repository: <organization>/<repository>
Work item: <GitHub issue or specification path>
Production branch: main
Production URL: <public hostname, when applicable>
```

It contains no Coolify token, LAN address, deployment UUID, private variable or
harness configuration.

## Completion

Provisioning is complete when the repository and work item exist, the GitHub App
can read the repository, the Coolify resource tracks `main`, Auto Deploy is
enabled, required runtime configuration exists and the webhook route is usable.
