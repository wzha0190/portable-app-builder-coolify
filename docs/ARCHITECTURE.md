# Architecture and trust boundaries

## Three separate roles

The design works only when three roles remain distinct.

### Setup agent

The setup agent receives the confidential bootstrap handoff. It may configure
the selected harness, authenticate official CLIs, install the sanitized runtime
payload, restart the harness, and run non-mutating acceptance checks.

It owns credential onboarding. A later application task must never become an
authentication or workstation-setup task.

### Target harness

The target harness is the user-selected coding product. It receives runtime
instructions through its own officially documented native instruction mechanism.
It must provide:

- durable instructions;
- a persistent command environment;
- native unattended permissions sufficient for the workflow;
- reliable stdout, stderr, and exit status capture;
- task continuation and context management; and
- rendered-browser testing or permission to use ordinary browser tools.

The design does not prescribe a specific harness, shell, editor, configuration
file, path, approval mode, or execution backend.

### Runtime application agent

The runtime agent receives only:

1. the installed, sanitized runtime instructions; and
2. a prompt beginning `BUILD APP COOLIFY:`.

It consumes already configured official CLI authentication. It does not read the
bootstrap handoff, receive credentials, authenticate accounts, or repair CLI
contexts.

## Compatibility contract

A harness is compatible only if its documented native mechanisms can preserve
the effective model, permissions, command environment, instruction scope, and
credential access after a complete restart. If it intentionally isolates those
resources and has no supported persistent alternative, it is incompatible.

The correct response to incompatibility is to report it—not to add a wrapper,
credential broker, hidden integration, or undocumented configuration hack.

## Portability definition

Portable means the workflow can be bootstrapped on another authorized computer
on the same network without copying workstation paths or harness internals. It
does not mean that one bootstrap automatically targets arbitrary GitHub owners,
networks, or Coolify servers.

Resource identifiers are discovered from configured project, environment,
server, destination, and source names at runtime. Machine paths and saved UUIDs
are never treated as portable configuration.
