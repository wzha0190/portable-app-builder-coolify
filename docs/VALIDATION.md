# Production validation

A successful push or HTTP response alone does not prove that the requested
application is complete.

## Development checks

Before pushing `main`, the development client verifies:

- every acceptance criterion in the GitHub work item is implemented;
- formatting, lint, type checks and relevant tests pass;
- the production build succeeds;
- the container starts with the configured port and health behavior;
- no secret or generated artifact is staged; and
- the pushed commit equals the current remote `main` revision.

## Deployment checks

After the push, confirm that:

- GitHub delivered the push event to the installed Coolify GitHub App;
- the Coolify application still tracks `main` rather than a fixed commit;
- the deployment finished for the expected `main` revision;
- exactly one intended production application exists;
- the container remains healthy through multiple health-check cycles; and
- persistent data survives a normal redeployment when persistence is required.

The LAN-side administrator performs checks requiring Coolify deployment state or
logs. The remote client does not receive control-plane credentials merely for
verification.

## Rendered application checks

Use the production hostname in a rendered browser and verify:

- primary content is visible;
- important features are discoverable;
- representative interactions work;
- loading, empty, stale, offline, partial-failure and error states are accurate;
- relevant console errors and warnings are absent; and
- generated or analytical output is verified in its rendered form, not only as
  an API response.

## Repair loop

Application failures are corrected in the repository and pushed normally. The
new push triggers another Coolify deployment through the same GitHub App path.

Infrastructure failures—such as an unreachable webhook, missing source access,
invalid runtime variable, unavailable server or reverse-proxy error—are repaired
by the administrator inside the deployment network.

## End-to-end proof

The workflow is proven when a freshly provisioned repository is implemented from
its recorded GitHub work item, pushed from a development client without LAN or
Coolify access, automatically deployed by Coolify and verified in production
without routine control-plane intervention.
