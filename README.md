# One-shot GitHub-to-Coolify application delivery

This repository documents one standard workflow for building an application on
another computer and deploying it to Coolify without giving that computer LAN
access.

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

1. Creates a private repository under `wzha0190-software-factory` with `main` as
   the production branch.
2. Gives the existing Coolify GitHub App access to that repository.
3. Creates exactly one application in the `Software Factory` project and
   `production` environment from that GitHub App source and branch.
4. Configures the build method, internal application port, public hostname,
   runtime variables, persistent storage, and health behavior required by the
   requested application.
5. Enables Coolify Auto Deploy and verifies that GitHub can deliver events to
   the configured Coolify webhook endpoint.
6. Prepares a standard GitHub-supported repository write credential for the
   development environment and verifies that it can clone and push without
   GitHub CLI. The credential can use HTTPS or SSH according to the target
   environment's ordinary Git support.
7. Returns one confidential, self-contained development prompt.

The setup agent prepares the repository and deployment resource. It does not
implement the application.

## Required handoff prompt

The returned prompt contains:

- the repository clone URL and required Git authentication details;
- the production branch, `main`;
- the complete application specification and acceptance criteria;
- the build, port, health, persistence, and runtime-variable contract;
- the public production URL;
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
3. Implements every stated requirement.
4. Runs relevant tests and a production build, including a local container test
   when the application is containerized.
5. Checks that no supplied credential is part of the staged content.
6. Commits and pushes `main` with ordinary Git.
7. Waits for the public application to reflect the pushed revision.
8. Verifies the rendered application and representative interactions against
   every acceptance criterion.
9. If an application defect is found, fixes it, retests, pushes again, and
   repeats production verification automatically.

The push is the deployment trigger. The development agent needs no access to
the deployment LAN.

## Completion contract

The run is complete only when:

- the implementation satisfies every requirement;
- tests and the production build pass;
- the working tree is clean and `main` is pushed;
- the deployed application reflects the current remote `main` revision;
- health and required persistence behavior pass; and
- the public rendered application and representative interactions work.

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

## Official references

- [GitHub remote repository access](https://docs.github.com/en/get-started/git-basics/about-remote-repositories)
- [GitHub repository authentication options](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/managing-deploy-keys)
- [Coolify GitHub App setup](https://coolify.io/docs/applications/sources/github/app)
- [Coolify automatic deployments](https://coolify.io/docs/applications/deployments/automatic-deployments)

Current official product documentation is authoritative when product behavior
changes.
