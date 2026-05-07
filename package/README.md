# aux4 registry

Base package for container registry operations. This package provides the `registry` command namespace that provider packages extend with their own subcommands.

Provider packages (such as `community/registry-aws` or `community/registry-docker`) depend on this package and add commands under the `registry` profile to integrate with specific container registries.

## Installation

```bash
aux4 aux4 pkger install aux4/registry
```

You also need to install at least one provider package to use registry commands:

```bash
aux4 aux4 pkger install community/registry-aws
aux4 aux4 pkger install community/registry-docker
```

## How It Works

This package defines the `registry` command group with an empty profile. Provider packages add their commands to the `registry` profile, creating a unified command structure:

```text
aux4 registry <provider> <command>
```

For example:

```bash
aux4 registry aws login
aux4 registry aws push --image my-app:latest
aux4 registry docker login --registry ghcr.io
aux4 registry docker push --image my-app:latest
```

## Available Providers

| Provider | Package | Registry |
|----------|---------|----------|
| `aws` | `community/registry-aws` | Amazon ECR |
| `docker` | `community/registry-docker` | Docker Hub, GHCR, and other OCI registries |

## Building a Registry Provider

A registry provider is a standard aux4 package that adds commands under the `registry` profile. The package must:

1. Declare `aux4/registry` as a dependency
2. Include the `registry` tag
3. Add a command to the `registry` profile using the provider name (e.g., `aws`, `docker`, `gcp`, `azure`)
4. Route that command to a `registry:<provider>` profile containing the provider's subcommands
5. Implement at minimum: `login` and `push`

### Provider Command Contract

Every registry provider should implement the following commands:

#### login (required)

Authenticates with the container registry.

```bash
aux4 registry <provider> login [--registry <url>]
```

#### push (required)

Pushes a container image to the registry.

```bash
aux4 registry <provider> push --image <name:tag> [--registry <url>]
```

See the `community/registry-aws` or `community/registry-docker` packages for complete provider implementation examples.
