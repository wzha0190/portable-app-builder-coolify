# Lessons learned

## Keep development and deployment credentials separate

Trying to authenticate every coding harness directly to Coolify created repeated
profile, credential-store and command-environment failures. The standard GitHub
App workflow removes that dependency: development clients push to GitHub, while
Coolify credentials remain with the administrator.

## Use GitHub as the handoff

A generated agent-to-agent deployment prompt duplicates project state and can
become stale. A GitHub issue or versioned specification is visible to developers,
reviewable, linkable to commits and retained with the project workflow.

## A GitHub App does not create Coolify resources

The GitHub App grants repository access and delivers events. An administrator
must still create and configure each new Coolify application once. Later pushes
to its tracked branch deploy automatically.

## Auto Deploy needs a reachable webhook

Repository access can work while automatic deployment fails. GitHub must reach
the selected HTTPS webhook endpoint, TLS verification must succeed and Auto
Deploy must be enabled for the application.

## The repository owner must be explicit

The authenticated personal account is the acting identity, not the repository
owner. New repositories must be created under the intended organization and
verified as private before handoff.

## Routing metadata differs from public ports

Coolify domain configuration may include the internal container port as routing
metadata while users visit a normal portless hostname. The external reverse
proxy should forward to Coolify's proxy rather than allocating a fixed host port
for every application.

## Avoid duplicate aggressive health checks

One tolerant container health check plus external production checks is clearer
than competing probes that can remove a functioning application from routing.

## Deployment status is not product completion

The deployed revision, rendered interface, representative interactions, health
stability and required persistence must all be checked against the recorded work
item before completion is reported.

## Keep operational diagnosis on the control plane

A remote developer can correct source failures and push another revision. Server,
webhook, secret and reverse-proxy failures belong to the administrator with
access to Coolify deployment state and logs.
