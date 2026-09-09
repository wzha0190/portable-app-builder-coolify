# Architecture and trust boundaries

## Four standard roles

### Administrator

The administrator operates from the deployment network and owns first-time
control-plane work: repository creation, GitHub App repository access, Coolify
resource creation, domains, variables, storage, health configuration and Auto
Deploy.

### Development client

The development client can operate from any network. It authenticates to GitHub,
clones the prepared repository, implements the recorded work item, tests locally
and pushes `main`. It does not receive Coolify credentials or direct LAN access.

The client may be a person, IDE, coding agent or CI runner. The deployment design
does not depend on a particular harness.

### GitHub

GitHub is the shared source of truth for code, branch state, repository access
and application requirements. A GitHub issue or versioned specification replaces
private agent-to-agent prompt handoffs.

### Coolify

Coolify owns build and deployment state. Its GitHub App reads the configured
repository and receives push events. The application resource tracks `main` and
deploys the pushed revision through Auto Deploy.

## Trust boundaries

```text
Remote development environment
    GitHub credentials and repository contents
                 |
                 | HTTPS Git push
                 v
GitHub organization and GitHub App
                 |
                 | authenticated webhook
                 v
Private deployment network
    Coolify credentials, runtime secrets and deployment logs
```

The webhook endpoint is the only inbound path GitHub needs. The Coolify dashboard
and API do not need to be exposed to development clients.

## Portability

Portability comes from the Git protocol and GitHub-hosted work item, not from
copying a harness configuration between computers. Any development client that
can authenticate to GitHub, edit the repository and push `main` can participate.

Infrastructure identifiers and credentials remain control-plane configuration.
They are not embedded in source, prompts or portable runtime rules.
