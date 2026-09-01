# Bootstrap procedure

Bootstrap prepares one selected coding harness for future prompt-only application
runs. It is not an application build.

## Procedure

1. Identify the selected harness and installed version.
2. Consult its current official documentation for provider configuration,
   instructions, command execution, continuation, unattended permissions,
   persistence, and restart behavior.
3. Install required official tools through their documented distribution paths.
4. Configure and validate the live model/provider instead of accepting defaults.
5. Prove representative multi-step tool use completes without response
   truncation or an unfinished tool call.
6. Configure the harness's native unattended-permission categories for project
   files, package installation, testing, network access, Git, GitHub CLI, and
   Coolify CLI operations.
7. Verify that no approval or request ceiling interrupts representative work.
8. Configure a native command environment that reliably returns stdout, stderr,
   and exit status and persists after restart.
9. Install only the sanitized runtime payload through one documented native
   instruction mechanism and verify its installed structure or hash.
10. Authenticate GitHub using the official GitHub CLI workflow in the command
    environment the harness actually uses.
11. Configure an official named Coolify CLI context in that same environment.
12. Resolve the target Coolify project, environment, server, destination, GitHub
    source, and shared-variable metadata with non-sensitive official CLI output.
13. Validate the server and wait until Coolify reports it as both reachable and
    usable. A message that validation merely started is not completion.
14. Validate LAN wildcard DNS and reverse-proxy reachability before an
    application exists.
15. Fully close and restart the harness and its host application.
16. Run a sanitized non-mutating task inside the restarted harness and repeat the
    complete preflight through its normal command tool.
17. Repair failures using documented native mechanisms, restart again, and repeat
    the acceptance gate.

## Why in-harness validation is authoritative

Several coding harnesses can run commands using a different user profile,
credential view, environment, or terminal backend from the setup agent's shell.
Authentication succeeding externally therefore proves very little.

The post-restart validation task must independently resolve official tools,
authenticate GitHub, verify organization access, verify Coolify, inspect required
resource metadata, and capture output and exit status. The setup shell is useful
only for comparison.

## Coolify authorization without changing resources

Successful context verification and inventory listing prove authentication and
read access, but not configuration, deployment, or operational-log permissions.
The reference workflow therefore uses official CLI requests against a guaranteed
nonexistent application identifier.

The expected resource-not-found response shows that authentication and the
relevant authorization middleware passed before resource lookup. Authentication
or authorization errors fail the gate. No real application is modified, created,
or deployed during bootstrap.

This behavior is stack-sensitive and must be confirmed against the installed
official CLI and server version rather than blindly copied.

## Completion claim

The honest completion claim is:

> Prompt-only builds after a one-time secure host and harness bootstrap.

It is not “zero interaction from a bare computer.”
