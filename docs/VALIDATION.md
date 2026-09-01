# Production validation contract

A build, deployment command, platform status, or HTTP 200 is preliminary evidence.
Completion requires the whole production gate.

## Requirement reconciliation

Re-read the original application prompt and verify every material feature against
the deployed application. Omitted, reduced, undiscoverable, inaccessible, or
unverified requirements fail the gate.

## Source and deployment identity

Require all of the following:

- the repository is private and owned by the required GitHub owner;
- the working tree is clean;
- local `main` is pushed and tracks remote `main`;
- the Coolify resource tracks `main` rather than a fixed commit unless immutable
  deployment was explicitly requested;
- the latest finished deployment's official commit field equals the full current
  remote `main` SHA; and
- exactly one intended managed production application exists.

## Rendered-browser verification

Open the production hostname in a rendered browser and verify:

- primary content is visible;
- important features are discoverable;
- representative interactions work;
- loading, empty, stale, offline, partial-failure, and error states are not shown
  incorrectly; and
- relevant browser console errors and warnings are absent.

An HTTP client alone cannot prove rendered or interactive behavior.

For generated analysis or another core item-based interaction, open a
representative production item and verify the complete rendered result rather
than accepting only an API response.

## Stability observation

Use a meaningful container startup grace period, interval, timeout, and retry
count derived from observed behavior. Then wait through multiple complete health
cycles and repeatedly verify:

- deployment and application state;
- `/healthz`;
- the primary route;
- a representative API route; and
- the primary rendered interaction.

A single immediate healthy observation is insufficient.

## Persistence

For a stateful application, create or select safe representative data, perform a
normal redeployment of the same branch-tracking resource, wait through the full
stability window, and prove the data survives. A genuinely stateless application
records this check as not applicable instead of inventing persistence.

## Automatic repair loop

If any final check fails:

1. diagnose the failure;
2. repair it;
3. commit and push source changes when needed;
4. redeploy; and
5. restart the entire final gate from requirement reconciliation.

Do not report success and defer known repair work to the user.

## One-shot proof

A repaired run can prove that one application eventually completed. It does not
prove the workflow is one-shot capable.

One-shot capability is proven only when a fresh application prompt begins in an
empty authorized project and reaches stable verified production without
corrective user interaction.
