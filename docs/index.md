---
search: false
redirect_to:
  - /docs/installation/
---
Appending docs/configuration/overview.md
---
# This file has been generated from the Kamal source, do not edit directly.
# Find the source of this file at lib/kamal/configuration/docs/configuration.yml in the Kamal repository.
title: Kamal Configuration
---

# Kamal Configuration

Configuration is read from the `config/deploy.yml`.

## [Destinations](#destinations)

When running commands, you can specify a destination with the `-d` flag,
e.g., `kamal deploy -d staging`.

In this case, the configuration will also be read from `config/deploy.staging.yml`
and merged with the base configuration.

## [Extensions](#extensions)

Kamal will not accept unrecognized keys in the configuration file.

However, you might want to declare a configuration block using YAML anchors
and aliases to avoid repetition.

You can prefix a configuration section with `x-` to indicate that it is an
extension. Kamal will ignore the extension and not raise an error.

## [The service name](#the-service-name)

This is a required value. It is used as the container name prefix.

```yaml
service: myapp
```

## [The Docker image name](#the-docker-image-name)

The image will be pushed to the configured registry.

```yaml
image: my-image
```

## [Labels](#labels)

Additional labels to add to the container:

```yaml
labels:
  my-label: my-value
```

## [Volumes](#volumes)

Additional volumes to mount into the container:

```yaml
volumes:
  - /path/on/host:/path/in/container:ro
```

## [Registry](#registry)

The Docker registry configuration, see [Docker Registry](../docker-registry):

```yaml
registry:
  ...
```

## [Servers](#servers)

The servers to deploy to, optionally with custom roles, see [Servers](../servers):

```yaml
servers:
  ...
```

## [Environment variables](#environment-variables)

See [Environment variables](../environment-variables):

```yaml
env:
  ...
```

## [Asset path](#asset-path)

Used for asset bridging across deployments, default to `nil`.

If there are changes to CSS or JS files, we may get requests
for the old versions on the new container, and vice versa.

To avoid 404s, we can specify an asset path.
Kamal will replace that path in the container with a mapped
volume containing both sets of files.
This requires that file names change when the contents change
(e.g., by including a hash of the contents in the name).

To configure this, set the path to the assets:

```yaml
asset_path: /path/to/assets
```

## [Hooks path](#hooks-path)

Path to hooks, defaults to `.kamal/hooks`.
See [Hooks](/docs/hooks) for more information:

```yaml
hooks_path: /user_home/kamal/hooks
```

## [Require destinations](#require-destinations)

Whether deployments require a destination to be specified, defaults to `false`:

```yaml
require_destination: true
```

## [Primary role](#primary-role)

This defaults to `web`, but if you have no web role, you can change this:

```yaml
primary_role: workers
```

## [Allowing empty roles](#allowing-empty-roles)

Whether roles with no servers are allowed. Defaults to `false`:

```yaml
allow_empty_roles: false
```

## [Retain containers](#retain-containers)

How many old containers and images we retain, defaults to 5:

```yaml
retain_containers: 3
```

## [Minimum version](#minimum-version)

The minimum version of Kamal required to deploy this configuration, defaults to `nil`:

```yaml
minimum_version: 1.3.0
```

## [Readiness delay](#readiness-delay)

Seconds to wait for a container to boot after it is running, default 7.

This only applies to containers that do not run a proxy or specify a healthcheck:

```yaml
readiness_delay: 4
```

## [Deploy timeout](#deploy-timeout)

How long to wait for a container to become ready, default 30:

```yaml
deploy_timeout: 10
```

## [Drain timeout](#drain-timeout)

How long to wait for a container to drain, default 30:

```yaml
drain_timeout: 10
```

## [Run directory](#run-directory)

Directory to store kamal runtime files in on the host, default `.kamal`:

```yaml
run_directory: /etc/kamal
```

## [SSH options](#ssh-options)

See [SSH](../ssh):

```yaml
ssh:
  ...
```

## [Builder options](#builder-options)

See [Builders](../builders):

```yaml
builder:
  ...
```

## [Accessories](#accessories)

Additional services to run in Docker, see [Accessories](../accessories):

```yaml
accessories:
  ...
```

## [Proxy](#proxy)

Configuration for kamal-proxy, see [Proxy](../proxy):

```yaml
proxy:
  ...
```

## [SSHKit](#sshkit)

See [SSHKit](../sshkit):

```yaml
sshkit:
  ...
```

## [Boot options](#boot-options)

See [Booting](../booting):

```yaml
boot:
  ...
```

## [Logging](#logging)

Docker logging configuration, see [Logging](../logging):

```yaml
logging:
  ...
```

## [Aliases](#aliases)

Alias configuration, see [Aliases](../aliases):

```yaml
aliases:
  ...
```
Appending docs/configuration/builders.md
---
# This file has been generated from the Kamal source, do not edit directly.
# Find the source of this file at lib/kamal/configuration/docs/builder.yml in the Kamal repository.
title: Builder
---

# Builder

The builder configuration controls how the application is built with `docker build`.

See [Builder examples](/docs/configuration/builder-examples/) for more information.

## [Builder options](#builder-options)

Options go under the builder key in the root configuration.

```yaml
builder:
```

## [Arch](#arch)

The architectures to build for — you can set an array or just a single value.

Allowed values are `amd64` and `arm64`:

```yaml
  arch:
    - amd64
```

## [Remote](#remote)

The connection string for a remote builder. If supplied, Kamal will use this
for builds that do not match the local architecture of the deployment host.

```yaml
  remote: ssh://docker@docker-builder
```

## [Local](#local)

If set to false, Kamal will always use the remote builder even when building
the local architecture.

Defaults to true:

```yaml
  local: true
```

## [Builder cache](#builder-cache)

The type must be either 'gha' or 'registry'.

The image is only used for registry cache and is not compatible with the Docker driver:

```yaml
  cache:
    type: registry
    options: mode=max
    image: kamal-app-build-cache
```

## [Build context](#build-context)

If this is not set, then a local Git clone of the repo is used.
This ensures a clean build with no uncommitted changes.

To use the local checkout instead, you can set the context to `.`, or a path to another directory.

```yaml
  context: .
```

## [Dockerfile](#dockerfile)

The Dockerfile to use for building, defaults to `Dockerfile`:

```yaml
  dockerfile: Dockerfile.production
```

## [Build target](#build-target)

If not set, then the default target is used:

```yaml
  target: production
```

## [Build arguments](#build-arguments)

Any additional build arguments, passed to `docker build` with `--build-arg <key>=<value>`:

```yaml
  args:
    ENVIRONMENT: production
```

## [Referencing build arguments](#referencing-build-arguments)

```shell
ARG RUBY_VERSION
FROM ruby:$RUBY_VERSION-slim as base
```

## [Build secrets](#build-secrets)

Values are read from `.kamal/secrets`:

```yaml
  secrets:
    - SECRET1
    - SECRET2
```

## [Referencing build secrets](#referencing-build-secrets)

```shell
# Copy Gemfiles
COPY Gemfile Gemfile.lock ./

# Install dependencies, including private repositories via access token
# Then remove bundle cache with exposed GITHUB_TOKEN
RUN --mount=type=secret,id=GITHUB_TOKEN \
  BUNDLE_GITHUB__COM=x-access-token:$(cat /run/secrets/GITHUB_TOKEN) \
  bundle install && \
  rm -rf /usr/local/bundle/cache
```

## [SSH](#ssh)

SSH agent socket or keys to expose to the build:

```yaml
  ssh: default=$SSH_AUTH_SOCK
```

## [Driver](#driver)

The build driver to use, defaults to `docker-container`:

```yaml
  driver: docker
```


If you want to use Docker Build Cloud (https://www.docker.com/products/build-cloud/), you can set the driver to:

```yaml
  driver: cloud org-name/builder-name
```

## [Provenance](#provenance)

It is used to configure provenance attestations for the build result.
The value can also be a boolean to enable or disable provenance attestations.

```yaml
  provenance: mode=max
```

## [SBOM (Software Bill of Materials)](#sbom-(software-bill-of-materials))

It is used to configure SBOM generation for the build result.
The value can also be a boolean to enable or disable SBOM generation.

```yaml
  sbom: true
```
Appending docs/configuration/ssh.md
---
# This file has been generated from the Kamal source, do not edit directly.
# Find the source of this file at lib/kamal/configuration/docs/ssh.yml in the Kamal repository.
title: SSH configuration
---

# SSH configuration

Kamal uses SSH to connect and run commands on your hosts.
By default, it will attempt to connect to the root user on port 22.

If you are using a non-root user, you may need to bootstrap your servers manually before using them with Kamal. On Ubuntu, you’d do:

```shell
sudo apt update
sudo apt upgrade -y
sudo apt install -y docker.io curl git
sudo usermod -a -G docker app
```

## [SSH options](#ssh-options)

The options are specified under the ssh key in the configuration file.

```yaml
ssh:
```

## [The SSH user](#the-ssh-user)

Defaults to `root`:

```yaml
  user: app
```

## [The SSH port](#the-ssh-port)

Defaults to 22:

```yaml
  port: "2222"
```

## [Proxy host](#proxy-host)

Specified in the form <host> or <user>@<host>:

```yaml
  proxy: root@proxy-host
```

## [Proxy command](#proxy-command)

A custom proxy command, required for older versions of SSH:

```yaml
  proxy_command: "ssh -W %h:%p user@proxy"
```

## [Log level](#log-level)

Defaults to `fatal`. Set this to `debug` if you are having SSH connection issues.

```yaml
  log_level: debug
```

## [Keys only](#keys-only)

Set to `true` to use only private keys from the `keys` and `key_data` parameters,
even if ssh-agent offers more identities. This option is intended for
situations where ssh-agent offers many different identities or you
need to overwrite all identities and force a single one.

```yaml
  keys_only: false
```

## [Keys](#keys)

An array of file names of private keys to use for public key
and host-based authentication:

```yaml
  keys: [ "~/.ssh/id.pem" ]
```

## [Key data](#key-data)

An array of strings, with each element of the array being
a raw private key in PEM format.

```yaml
  key_data: [ "-----BEGIN OPENSSH PRIVATE KEY-----" ]
```

## [Config](#config)

Set to true to load the default OpenSSH config files (~/.ssh/config,
/etc/ssh_config), to false ignore config files, or to a file path
(or array of paths) to load specific configuration. Defaults to true.

```yaml
  config: true
```
Appending docs/configuration/anchors.md
---
title: Anchors
---

# Anchors

You can re-use parts of your Kamal configuration by defining them as anchors and referencing them with aliases.

For example, you might need to define a shared healthcheck for multiple worker roles. Anchors
begin with `x-` and are defined at the root level of your deploy.yml file.

```yaml
x-worker-healthcheck: &worker-healthcheck
  health-cmd: bin/worker-healthcheck
  health-start-period: 5s
  health-retries: 5
  health-interval: 5s
```

To use this anchor in your deploy configuration, reference it via the alias.

```yaml
servers:
  worker:
    hosts:
      - 867.53.0.9
    cmd: bin/jobs
    options:
      <<: *worker-healthcheck
```
Appending docs/configuration/roles.md
---
# This file has been generated from the Kamal source, do not edit directly.
# Find the source of this file at lib/kamal/configuration/docs/role.yml in the Kamal repository.
title: Roles
---

# Roles

Roles are used to configure different types of servers in the deployment.
The most common use for this is to run web servers and job servers.

Kamal expects there to be a `web` role, unless you set a different `primary_role`
in the root configuration.

## [Role configuration](#role-configuration)

Roles are specified under the servers key:

```yaml
servers:
```

## [Simple role configuration](#simple-role-configuration)

This can be a list of hosts if you don't need custom configuration for the role.

You can set tags on the hosts for custom env variables (see [Environment variables](../environment-variables)):

```yaml
  web:
    - 172.1.0.1
    - 172.1.0.2: experiment1
    - 172.1.0.2: [ experiment1, experiment2 ]
```

## [Custom role configuration](#custom-role-configuration)

When there are other options to set, the list of hosts goes under the `hosts` key.

By default, only the primary role uses a proxy.

For other roles, you can set it to `proxy: true` to enable it and inherit the root proxy
configuration or provide a map of options to override the root configuration.

For the primary role, you can set `proxy: false` to disable the proxy.

You can also set a custom `cmd` to run in the container and overwrite other settings
from the root configuration.

```yaml
  workers:
    hosts:
      - 172.1.0.3
      - 172.1.0.4: experiment1
    cmd: "bin/jobs"
    options:
      memory: 2g
      cpus: 4
    logging:
      ...
    proxy:
      ...
    labels:
      my-label: workers
    env:
      ...
    asset_path: /public
```
Appending docs/configuration/proxy.md
---
# This file has been generated from the Kamal source, do not edit directly.
# Find the source of this file at lib/kamal/configuration/docs/proxy.yml in the Kamal repository.
title: Proxy
---

# Proxy

Kamal uses [kamal-proxy](https://github.com/basecamp/kamal-proxy) to provide
gapless deployments. It runs on ports 80 and 443 and forwards requests to the
application container.

The proxy is configured in the root configuration under `proxy`. These are
options that are set when deploying the application, not when booting the proxy.

They are application-specific, so they are not shared when multiple applications
run on the same proxy.

The proxy is enabled by default on the primary role but can be disabled by
setting `proxy: false`.

It is disabled by default on all other roles but can be enabled by setting
`proxy: true` or providing a proxy configuration.

```yaml
proxy:
```

## [Hosts](#hosts)

The hosts that will be used to serve the app. The proxy will only route requests
to this host to your app.

If no hosts are set, then all requests will be forwarded, except for matching
requests for other apps deployed on that server that do have a host set.

Specify one of `host` or `hosts`.

```yaml
  host: foo.example.com
  hosts:
    - foo.example.com
    - bar.example.com
```

## [App port](#app-port)

The port the application container is exposed on.

Defaults to 80:

```yaml
  app_port: 3000
```

## [SSL](#ssl)

kamal-proxy can provide automatic HTTPS for your application via Let's Encrypt.

This requires that we are deploying to one server and the host option is set.
The host value must point to the server we are deploying to, and port 443 must be
open for the Let's Encrypt challenge to succeed.

If you set `ssl` to `true`, `kamal-proxy` will stop forwarding headers to your app,
unless you explicitly set `forward_headers: true`

Defaults to `false`:

```yaml
  ssl: true
```

## [Forward headers](#forward-headers)

Whether to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers.

If you are behind a trusted proxy, you can set this to `true` to forward the headers.

By default, kamal-proxy will not forward the headers if the `ssl` option is set to `true`, and
will forward them if it is set to `false`.

```yaml
  forward_headers: true
```

## [Response timeout](#response-timeout)

How long to wait for requests to complete before timing out, defaults to 30 seconds:

```yaml
  response_timeout: 10
```

## [Healthcheck](#healthcheck)

When deploying, the proxy will by default hit `/up` once every second until we hit
the deploy timeout, with a 5-second timeout for each request.

Once the app is up, the proxy will stop hitting the healthcheck endpoint.

```yaml
  healthcheck:
    interval: 3
    path: /health
    timeout: 3
```

## [Buffering](#buffering)

Whether to buffer request and response bodies in the proxy.

By default, buffering is enabled with a max request body size of 1GB and no limit
for response size.

You can also set the memory limit for buffering, which defaults to 1MB; anything
larger than that is written to disk.

```yaml
  buffering:
    requests: true
    responses: true
    max_request_body: 40_000_000
    max_response_body: 0
    memory: 2_000_000
```

## [Logging](#logging)

Configure request logging for the proxy.
You can specify request and response headers to log.
By default, `Cache-Control`, `Last-Modified`, and `User-Agent` request headers are logged:

```yaml
  logging:
    request_headers:
      - Cache-Control
      - X-Forwarded-Proto
    response_headers:
      - X-Request-ID
      - X-Request-Start
```
Appending docs/configuration/docker-registry.md
---
# This file has been generated from the Kamal source, do not edit directly.
# Find the source of this file at lib/kamal/configuration/docs/registry.yml in the Kamal repository.
title: Registry
---

# Registry

The default registry is Docker Hub, but you can change it using `registry/server`.

By default, Docker Hub creates public repositories. To avoid making your images public,
set up a private repository before deploying, or change the default repository privacy
settings to private in your [Docker Hub settings](https://hub.docker.com/repository-settings/default-privacy).

A reference to a secret (in this case, `DOCKER_REGISTRY_TOKEN`) will look up the secret
in the local environment:

```yaml
registry:
  server: registry.digitalocean.com
  username:
    - DOCKER_REGISTRY_TOKEN
  password:
    - DOCKER_REGISTRY_TOKEN
```

## [Using AWS ECR as the container registry](#using-aws-ecr-as-the-container-registry)

You will need to have the AWS CLI installed locally for this to work.
AWS ECR’s access token is only valid for 12 hours. In order to avoid having to manually regenerate the token every time, you can use ERB in the `deploy.yml` file to shell out to the AWS CLI command and obtain the token:

```yaml
registry:
  server: <your aws account id>.dkr.ecr.<your aws region id>.amazonaws.com
  username: AWS
  password: <%= %x(aws ecr get-login-password) %>
```

## [Using GCP Artifact Registry as the container registry](#using-gcp-artifact-registry-as-the-container-registry)

To sign into Artifact Registry, you need to
[create a service account](https://cloud.google.com/iam/docs/service-accounts-create#creating)
and [set up roles and permissions](https://cloud.google.com/artifact-registry/docs/access-control#permissions).
Normally, assigning the `roles/artifactregistry.writer` role should be sufficient.

Once the service account is ready, you need to generate and download a JSON key and base64 encode it:

```shell
base64 -i /path/to/key.json | tr -d "\\n"
```

You'll then need to set the `KAMAL_REGISTRY_PASSWORD` secret to that value.

Use the environment variable as the password along with `_json_key_base64` as the username.
Here’s the final configuration:

```yaml
registry:
  server: <your registry region>-docker.pkg.dev
  username: _json_key_base64
  password:
    - KAMAL_REGISTRY_PASSWORD
```

## [Validating the configuration](#validating-the-configuration)

You can validate the configuration by running:

```shell
kamal registry login
```
Appending docs/configuration/servers.md
---
# This file has been generated from the Kamal source, do not edit directly.
# Find the source of this file at lib/kamal/configuration/docs/servers.yml in the Kamal repository.
title: Servers
---

# Servers

Servers are split into different roles, with each role having its own configuration.

For simpler deployments, though, where all servers are identical, you can just specify a list of servers.
They will be implicitly assigned to the `web` role.

```yaml
servers:
  - 172.0.0.1
  - 172.0.0.2
  - 172.0.0.3
```

## [Tagging servers](#tagging-servers)

Servers can be tagged, with the tags used to add custom env variables (see [Environment variables](../environment-variables)).

```yaml
servers:
  - 172.0.0.1
  - 172.0.0.2: experiments
  - 172.0.0.3: [ experiments, three ]
```

## [Roles](#roles)

For more complex deployments (e.g., if you are running job hosts), you can specify roles and configure each separately (see [Roles](../roles)):

```yaml
servers:
  web:
    ...
  workers:
    ...
```
Appending docs/configuration/cron.md
---
title: Cron
---

# Cron

You can use a specific container to run your Cron jobs:

```yaml
servers:
  cron:
    hosts:
      - 192.168.0.1
    cmd:
      bash -c "(env && cat config/crontab) | crontab - && cron -f"
```

This assumes that the Cron settings are stored in `config/crontab`. Cron does not automatically propagate environment variables, the example above copies them into the crontab.
Appending docs/configuration/aliases.md
---
# This file has been generated from the Kamal source, do not edit directly.
# Find the source of this file at lib/kamal/configuration/docs/alias.yml in the Kamal repository.
title: Aliases
---

# Aliases

Aliases are shortcuts for Kamal commands.

For example, for a Rails app, you might open a console with:

```shell
kamal app exec -i --reuse "bin/rails console"
```

By defining an alias, like this:

```yaml
aliases:
  console: app exec -i --reuse "bin/rails console"
```

You can now open the console with:

```shell
kamal console
```

## [Configuring aliases](#configuring-aliases)

Aliases are defined in the root config under the alias key.

Each alias is named and can only contain lowercase letters, numbers, dashes, and underscores:

```yaml
aliases:
  uname: app exec -p -q -r web "uname -a"
```
Appending docs/configuration/builder-examples.md
---
title: Builder examples
---

# Builder examples

## [Using remote builder for single-arch](#using-remote-builder-for-single-arch)

If you're developing on ARM64 (like Apple Silicon), but you want to deploy on AMD64 (x86 64-bit), by default, Kamal will set up a local buildx configuration that does this through QEMU emulation. However, this can be quite slow, especially on the first build.

If you want to speed up this process by using a remote AMD64 host to natively build the AMD64 part of the image, you can set a remote builder:

```yaml
builder:
  arch: amd64
  remote: ssh://root@192.168.0.1
```

Kamal will use the remote to build when deploying from an ARM64 machine, or build locally when deploying from an AMD64 machine.

**Note:** You must have Docker running on the remote host being used as a builder. This instance should only be shared for builds using the same registry and credentials.

## [Using remote builder for multi-arch](#using-remote-builder-for-native-multi-arch)

You can also build a multi-arch image. If a remote is set, Kamal will build the architecture matching your deployment server locally and the other architecture remotely.

So if you're developing on ARM64 (like Apple Silicon), it will build the ARM64 architecture locally and the AMD64 architecture remotely.

```yaml
builder:
  arch:
    - amd64
    - arm64
  remote: ssh://root@192.168.0.1
```

## [Using local builder for single-arch](#using-local-builder-for-single-arch)

If you always want to build locally for a single architecture, Kamal will build the image using a local buildx instance.

```yaml
builder:
  arch: amd64
```

## [Using a different Dockerfile or context when building](#using-a-different-dockerfile-or-context-when-building)

If you need to pass a different Dockerfile or context to the build command (e.g., if you're using a monorepo or you have different Dockerfiles), you can do so in the builder options:

```yaml
# Use a different Dockerfile
builder:
  dockerfile: Dockerfile.xyz

# Set context
builder:
  context: ".."

# Set Dockerfile and context
builder:
  dockerfile: "../Dockerfile.xyz"
  context: ".."
```

## [Using multistage builder cache](#using-multistage-builder-cache)

Docker multistage build cache can speed up your builds. Currently, Kamal only supports using the GHA cache or the Registry cache:

```yaml
# Using GHA cache
builder:
  cache:
    type: gha

# Using Registry cache
builder:
  cache:
    type: registry

# Using Registry cache with different cache image
builder:
  cache:
    type: registry
    # default image name is <image>-build-cache
    image: application-cache-image

# Using Registry cache with additional cache-to options
builder:
  cache:
    type: registry
    options: mode=max,image-manifest=true,oci-mediatypes=true
```

### [GHA cache configuration](#gha-cache-configuration)

To make it work on the GitHub action workflow, you need to set up the buildx and expose [authentication configuration for the cache](https://docs.docker.com/build/cache/backends/gha/#authentication).

Example setup (in .github/workflows/sample-ci.yml):

```yaml
- name: Set up Docker Buildx for cache
  uses: docker/setup-buildx-action@v3

- name: Expose GitHub Runtime for cache
  uses: crazy-max/ghaction-github-runtime@v3
```

When set up correctly, you should see the cache entry/entries on the GHA workflow actions cache section.

For further insights into build cache optimization, check out the documentation on Docker's official website: https://docs.docker.com/build/cache/.

## [Using build secrets for new images](#using-build-secrets-for-new-images)

Some images need a secret passed in during build time, like a GITHUB_TOKEN, to give access to private gem repositories. This can be done by setting the secret in `.kamal/secrets`, then referencing it in the builder configuration:

```bash
# .kamal/secrets

GITHUB_TOKEN=$(gh config get -h github.com oauth_token)
```

```yaml
# config/deploy.yml

builder:
  secrets:
    - GITHUB_TOKEN
```

This build secret can then be referenced in the Dockerfile:

```dockerfile
# Copy Gemfiles
COPY Gemfile Gemfile.lock ./

# Install dependencies, including private repositories via access token (then remove bundle cache with exposed GITHUB_TOKEN)
RUN --mount=type=secret,id=GITHUB_TOKEN \
  BUNDLE_GITHUB__COM=x-access-token:$(cat /run/secrets/GITHUB_TOKEN) \
  bundle install && \
  rm -rf /usr/local/bundle/cache
```

## [Configuring build args for new images](#configuring-build-args-for-new-images)

Build arguments that aren't secret can also be configured:

```yaml
builder:
  args:
    RUBY_VERSION: 3.2.0
```

This build argument can then be used in the Dockerfile:

```dockerfile
ARG RUBY_VERSION
FROM ruby:$RUBY_VERSION-slim as base
```
Appending docs/configuration/accessories.md
---
# This file has been generated from the Kamal source, do not edit directly.
# Find the source of this file at lib/kamal/configuration/docs/accessory.yml in the Kamal repository.
title: Accessories
---

# Accessories

Accessories can be booted on a single host, a list of hosts, or on specific roles.
The hosts do not need to be defined in the Kamal servers configuration.

Accessories are managed separately from the main service — they are not updated
when you deploy, and they do not have zero-downtime deployments.

Run `kamal accessory boot <accessory>` to boot an accessory.
See `kamal accessory --help` for more information.

## [Configuring accessories](#configuring-accessories)

First, define the accessory in the `accessories`:

```yaml
accessories:
  mysql:
```

## [Service name](#service-name)

This is used in the service label and defaults to `<service>-<accessory>`,
where `<service>` is the main service name from the root configuration:

```yaml
    service: mysql
```

## [Image](#image)

The Docker image to use.
Prefix it with its server when using root level registry different from Docker Hub.
Define registry directly or via anchors when it differs from root level registry.

```yaml
    image: mysql:8.0
```

## [Registry](#registry)

By default accessories use Docker Hub registry.
You can specify different registry per accessory with this option.
Don't prefix image with this registry server.
Use anchors if you need to set the same specific registry for several accessories.

```yml
registry:
  <<: *specific-registry
```

See [Docker Registry](../docker-registry) for more information:

```yaml
    registry:
      ...
```

## [Accessory hosts](#accessory-hosts)

Specify one of `host`, `hosts`, or `roles`:

```yaml
    host: mysql-db1
    hosts:
      - mysql-db1
      - mysql-db2
    roles:
      - mysql
```

## [Custom command](#custom-command)

You can set a custom command to run in the container if you do not want to use the default:

```yaml
    cmd: "bin/mysqld"
```

## [Port mappings](#port-mappings)

See [https://docs.docker.com/network/](https://docs.docker.com/network/), and
especially note the warning about the security implications of exposing ports publicly.

```yaml
    port: "127.0.0.1:3306:3306"
```

## [Labels](#labels)

```yaml
    labels:
      app: myapp
```

## [Options](#options)

These are passed to the Docker run command in the form `--<name> <value>`:

```yaml
    options:
      restart: always
      cpus: 2
```

## [Environment variables](#environment-variables)

See [Environment variables](../environment-variables) for more information:

```yaml
    env:
      ...
```

## [Copying files](#copying-files)

You can specify files to mount into the container.
The format is `local:remote`, where `local` is the path to the file on the local machine
and `remote` is the path to the file in the container.

They will be uploaded from the local repo to the host and then mounted.

ERB files will be evaluated before being copied.

```yaml
    files:
      - config/my.cnf.erb:/etc/mysql/my.cnf
      - config/myoptions.cnf:/etc/mysql/myoptions.cnf
```

## [Directories](#directories)

You can specify directories to mount into the container. They will be created on the host
before being mounted:

```yaml
    directories:
      - mysql-logs:/var/log/mysql
```

## [Volumes](#volumes)

Any other volumes to mount, in addition to the files and directories.
They are not created or copied before mounting:

```yaml
    volumes:
      - /path/to/mysql-logs:/var/log/mysql
```

## [Network](#network)

The network the accessory will be attached to.

Defaults to kamal:

```yaml
    network: custom
```

## [Proxy](#proxy)

You can run your accessory behind the Kamal proxy. See [Proxy](../proxy) for more information

```yaml
    proxy:
      ...
```
Appending docs/configuration/environment-variables.md
---
# This file has been generated from the Kamal source, do not edit directly.
# Find the source of this file at lib/kamal/configuration/docs/env.yml in the Kamal repository.
title: Environment variables
---

# Environment variables

Environment variables can be set directly in the Kamal configuration or
read from `.kamal/secrets`.

## [Reading environment variables from the configuration](#reading-environment-variables-from-the-configuration)

Environment variables can be set directly in the configuration file.

These are passed to the `docker run` command when deploying.

```yaml
env:
  DATABASE_HOST: mysql-db1
  DATABASE_PORT: 3306
```

## [Secrets](#secrets)

Kamal uses dotenv to automatically load environment variables set in the `.kamal/secrets` file.

If you are using destinations, secrets will instead be read from `.kamal/secrets.<DESTINATION>` if
it exists.

Common secrets across all destinations can be set in `.kamal/secrets-common`.

This file can be used to set variables like `KAMAL_REGISTRY_PASSWORD` or database passwords.
You can use variable or command substitution in the secrets file.

```shell
KAMAL_REGISTRY_PASSWORD=$KAMAL_REGISTRY_PASSWORD
RAILS_MASTER_KEY=$(cat config/master.key)
```

You can also use [secret helpers](../../commands/secrets) for some common password managers.

```shell
SECRETS=$(kamal secrets fetch ...)

REGISTRY_PASSWORD=$(kamal secrets extract REGISTRY_PASSWORD $SECRETS)
DB_PASSWORD=$(kamal secrets extract DB_PASSWORD $SECRETS)
```

If you store secrets directly in `.kamal/secrets`, ensure that it is not checked into version control.

To pass the secrets, you should list them under the `secret` key. When you do this, the
other variables need to be moved under the `clear` key.

Unlike clear values, secrets are not passed directly to the container
but are stored in an env file on the host:

```yaml
env:
  clear:
    DB_USER: app
  secret:
    - DB_PASSWORD
```

## [Tags](#tags)

Tags are used to add extra env variables to specific hosts.
See [Servers](../servers) for how to tag hosts.

Tags are only allowed in the top-level env configuration (i.e., not under a role-specific env).

The env variables can be specified with secret and clear values as explained above.

```yaml
env:
  tags:
    <tag1>:
      MYSQL_USER: monitoring
    <tag2>:
      clear:
        MYSQL_USER: readonly
      secret:
        - MYSQL_PASSWORD
```

## [Example configuration](#example-configuration)

```yaml
env:
  clear:
    MYSQL_USER: app
  secret:
    - MYSQL_PASSWORD
  tags:
    monitoring:
      MYSQL_USER: monitoring
    replica:
      clear:
        MYSQL_USER: readonly
      secret:
        - READONLY_PASSWORD
```
Appending docs/configuration/booting.md
---
# This file has been generated from the Kamal source, do not edit directly.
# Find the source of this file at lib/kamal/configuration/docs/boot.yml in the Kamal repository.
title: Booting
---

# Booting

When deploying to large numbers of hosts, you might prefer not to restart your services on every host at the same time.

Kamal’s default is to boot new containers on all hosts in parallel. However, you can control this with the boot configuration.

## [Fixed group sizes](#fixed-group-sizes)

Here, we boot 2 hosts at a time with a 10-second gap between each group:

```yaml
boot:
  limit: 2
  wait: 10
```

## [Percentage of hosts](#percentage-of-hosts)

Here, we boot 25% of the hosts at a time with a 2-second gap between each group:

```yaml
boot:
  limit: 25%
  wait: 2
```
Appending docs/configuration/logging.md
---
# This file has been generated from the Kamal source, do not edit directly.
# Find the source of this file at lib/kamal/configuration/docs/logging.yml in the Kamal repository.
title: Custom logging configuration
---

# Custom logging configuration

Set these to control the Docker logging driver and options.

## [Logging settings](#logging-settings)

These go under the logging key in the configuration file.

This can be specified at the root level or for a specific role.

```yaml
logging:
```

## [Driver](#driver)

The logging driver to use, passed to Docker via `--log-driver`:

```yaml
  driver: json-file
```

## [Options](#options)

Any logging options to pass to the driver, passed to Docker via `--log-opt`:

```yaml
  options:
    max-size: 100m
```
Appending docs/configuration/sshkit.md
---
# This file has been generated from the Kamal source, do not edit directly.
# Find the source of this file at lib/kamal/configuration/docs/sshkit.yml in the Kamal repository.
title: SSHKit
---

# SSHKit

[SSHKit](https://github.com/capistrano/sshkit) is the SSH toolkit used by Kamal.

The default, settings should be sufficient for most use cases, but
when connecting to a large number of hosts, you may need to adjust.

## [SSHKit options](#sshkit-options)

The options are specified under the sshkit key in the configuration file.

```yaml
sshkit:
```

## [Max concurrent starts](#max-concurrent-starts)

Creating SSH connections concurrently can be an issue when deploying to many servers.
By default, Kamal will limit concurrent connection starts to 30 at a time.

```yaml
  max_concurrent_starts: 10
```

## [Pool idle timeout](#pool-idle-timeout)

Kamal sets a long idle timeout of 900 seconds on connections to try to avoid
re-connection storms after an idle period, such as building an image or waiting for CI.

```yaml
  pool_idle_timeout: 300
```
Appending docs/installation/dockerized.md
---
title: Running Kamal via Docker
---

# Running Kamal via Docker

On macOS, use:

```sh
alias kamal='docker run -it --rm -v "${PWD}:/workdir" -v "/run/host-services/ssh-auth.sock:/run/host-services/ssh-auth.sock" -e SSH_AUTH_SOCK="/run/host-services/ssh-auth.sock" -v /var/run/docker.sock:/var/run/docker.sock ghcr.io/basecamp/kamal:latest'
```

On Linux, use:

```sh
alias kamal='docker run -it --rm -v "${PWD}:/workdir" -v "${SSH_AUTH_SOCK}:/ssh-agent" -v /var/run/docker.sock:/var/run/docker.sock -e "SSH_AUTH_SOCK=/ssh-agent" ghcr.io/basecamp/kamal:latest'
```

## Limitations

When using the docker alias, Kamal commands are run in the container and not directly on your host, so there are limitations.

To avoid these limitations, [install Kamal with Ruby](..).

### Agent forwarding only

The alias forwards the SSH agent into the container and avoids injecting your private keys. If you need the full SSH config in the container you can add `-v "$HOME/.ssh:/root/.ssh"`, but note that this exposes your private keys into the container.

### Secrets

You won't be able to use the Kamal secret adapters as the secret manager command line tools will not be available in the container.

### Environment variables

Environment variables from your host will not be available, unless you alter the command to inject them by adding something like `-e KAMAL_REGISTRY_PASSWORD=$KAMAL_REGISTRY_PASSWORD`.

Appending docs/hooks/docker-setup.md
---
title: docker-setup
---

# Hooks: docker-setup

Run once Docker is installed on a server but before taking any application-specific actions. This is designed for performing any necessary configuration of Docker itself.
Appending docs/hooks/overview.md
---
title: Hooks overview
---

# Hooks overview

You can run custom scripts at specific points with hooks.

Hooks should be stored in the **.kamal/hooks** folder. Running `kamal init` will build that folder and add some sample scripts.

You can change their location by setting `hooks_path` in the configuration file.

If the script returns a non-zero exit code, the command will be aborted.

`KAMAL_*` environment variables are available to the hooks command for fine-grained audit reporting, e.g., for triggering deployment reports or firing a JSON webhook. These variables include:

- `KAMAL_RECORDED_AT` — UTC timestamp in ISO 8601 format, e.g., `2023-04-14T17:07:31Z`
- `KAMAL_PERFORMER` — The local user performing the command (from `whoami`)
- `KAMAL_SERVICE` — The service name, e.g., app
- `KAMAL_SERVICE_VERSION` — An abbreviated service and version for use in messages, e.g., app@150b24f
- `KAMAL_VERSION` — The full version being deployed
- `KAMAL_HOSTS` — A comma-separated list of the hosts targeted by the command
- `KAMAL_COMMAND` — The command we are running
- `KAMAL_SUBCOMMAND` — _Optional:_ The subcommand we are running
- `KAMAL_DESTINATION` — _Optional:_ Destination, e.g., "staging"
- `KAMAL_ROLE` — _Optional:_ Role targeted, e.g., "web"

The available hooks are:

- [docker-setup](../docker-setup)
- [pre-connect](../pre-connect)
- [pre-build](../pre-build)
- [pre-deploy](../pre-deploy)
- [post-deploy](../post-deploy)
- [pre-app-boot](../pre-app-boot)
- [post-app-boot](../post-app-boot)
- [pre-proxy-reboot](../pre-proxy-reboot)
- [post-proxy-reboot](../post-proxy-reboot)

You can pass `--skip_hooks` to avoid running the hooks.

**Note:** The hook filename must be the hook name without any extension. For example, the [pre-deploy](../pre-deploy) hook should be named "pre-deploy" (without any file extension such as .sh or .rb).
Appending docs/hooks/post-deploy.md
---
title: post-deploy
---

# Hooks: post-deploy

Run after a deploy, redeploy, or rollback. This hook is also passed a `KAMAL_RUNTIME` env variable set to the total seconds the deploy took.

This could be used to broadcast a deployment message or register the new version with an APM.

The command could look something like:

```bash
#!/usr/bin/env bash
curl -q -d content="[My App] ${KAMAL_PERFORMER} Rolled back to version ${KAMAL_VERSION}" https://3.basecamp.com/XXXXX/integrations/XXXXX/buckets/XXXXX/chats/XXXXX/lines
```

That will post a line like the following to a preconfigured chatbot in Basecamp:

```
[My App] [dhh] Rolled back to version d264c4e92470ad1bd18590f04466787262f605de
```
Appending docs/hooks/post-app-boot.md
---
title: post-app-boot
---

# Hooks: post-app-boot

Run after booting the app container when you call `kamal app boot`, or indirectly via `kamal deploy`.

See [pre-app-boot](../pre-app-boot) for more information.
Appending docs/hooks/pre-build.md
---
title: pre-build
---

# Hooks: pre-build

Used for pre-build checks, e.g., ensuring there are no uncommitted changes or that CI has passed.
Appending docs/hooks/post-proxy-reboot.md
---
title: post-proxy-reboot
---

# Hooks: post-proxy-reboot

Runs after rebooting the kamal-proxy container. See [pre-proxy-reboot](../pre-proxy-reboot) for details.
Appending docs/hooks/pre-proxy-reboot.md
---
title: pre-proxy-reboot
---

# Hooks: pre-proxy-reboot

Run before rebooting the kamal-proxy container when you call `kamal proxy reboot`.

If you have the hook disabling the current server in an external load balancer and use the `--rolling` flag, you can use this for a zero-downtime proxy reboot.

With a rolling reboot, the hook will be called once for each server, with `KAMAL_HOSTS` containing the current server. With a non-rolling reboot, it will be called just once.

Use the [post-proxy-reboot](../post-proxy-reboot) hook to re-enable the server.
Appending docs/hooks/pre-deploy.md
---
title: pre-deploy
---

# Hooks: pre-deploy

For final checks before deploying, e.g., checking CI completed.
Appending docs/hooks/pre-connect.md
---
title: pre-connect
---

# Hooks: pre-connect

Runs before taking the deploy lock. For anything that needs to run before connecting to remote hosts, e.g., DNS warming, checking if you are on the VPN.
Appending docs/hooks/pre-app-boot.md
---
title: pre-app-boot
---

# Hooks: pre-app-boot

Run before booting the app container when you call `kamal app boot`, or indirectly via `kamal deploy`.

With a grouped boot strategy, the hook will be called once for each group, with `KAMAL_HOSTS` containing a list of servers in the group.

The [post-app-boot](../post-app-boot) will be called after the boot completes, again once per deployment group.
Appending docs/commands/upgrade.md
---
title: Upgrade
---

# kamal upgrade

Use `kamal upgrade` to upgrade your app running under Kamal 1.x to Kamal 2.0.

Please read the [Upgrade Guide](../../upgrading/overview) first.
Appending docs/commands/audit.md
---
title: Audit
---

# kamal audit

Show the latest commands that have been run on each server.

```bash
$ kamal audit
 kamal audit
  INFO [1ec52bf7] Running /usr/bin/env tail -n 50 .kamal/app-audit.log on server2
  INFO [54911c10] Running /usr/bin/env tail -n 50 .kamal/app-audit.log on server1
  INFO [2f3d32d0] Running /usr/bin/env tail -n 50 .kamal/app-audit.log on server3
  INFO [2f3d32d0] Finished in 0.232 seconds with exit status 0 (successful).
App Host: server3
[2024-04-05T07:14:23Z] [user] Pushed env files
[2024-04-05T07:14:29Z] [user] Pulled image with version 75bf6fa40b975cbd8aec05abf7164e0982f185ac
[2024-04-05T07:14:45Z] [user] [workers] Booted app version 75bf6fa40b975cbd8aec05abf7164e0982f185ac
[2024-04-05T07:14:53Z] [user] Tagging registry:4443/app:75bf6fa40b975cbd8aec05abf7164e0982f185ac as the latest image
[2024-04-05T07:14:53Z] [user] Pruned containers
[2024-04-05T07:14:53Z] [user] Pruned images

  INFO [54911c10] Finished in 0.232 seconds with exit status 0 (successful).
App Host: server1
[2024-04-05T07:14:23Z] [user] Pushed env files
[2024-04-05T07:14:29Z] [user] Pulled image with version 75bf6fa40b975cbd8aec05abf7164e0982f185ac
[2024-04-05T07:14:45Z] [user] [web] Booted app version 75bf6fa40b975cbd8aec05abf7164e0982f185ac
[2024-04-05T07:14:53Z] [user] Tagging registry:4443/app:75bf6fa40b975cbd8aec05abf7164e0982f185ac as the latest image
[2024-04-05T07:14:53Z] [user] Pruned containers
[2024-04-05T07:14:53Z] [user] Pruned images

  INFO [1ec52bf7] Finished in 0.233 seconds with exit status 0 (successful).
App Host: server2
[2024-04-05T07:14:23Z] [user] Pushed env files
[2024-04-05T07:14:29Z] [user] Pulled image with version 75bf6fa40b975cbd8aec05abf7164e0982f185ac
[2024-04-05T07:14:45Z] [user] [web] Booted app version 75bf6fa40b975cbd8aec05abf7164e0982f185ac
[2024-04-05T07:14:53Z] [user] Tagging registry:4443/app:75bf6fa40b975cbd8aec05abf7164e0982f185ac as the latest image
[2024-04-05T07:14:53Z] [user] Pruned containers
[2024-04-05T07:14:53Z] [user] Pruned images
```
Appending docs/commands/registry.md
---
title: Registry
---

# kamal registry

Log in and out of the Docker registry on your servers.

Examples:

```bash
$ kamal registry login
  INFO [60171eef] Running docker login registry:4443 -u [REDACTED] -p [REDACTED] on localhost
  INFO [60171eef] Finished in 0.069 seconds with exit status 0 (successful).
  INFO [427368d0] Running docker login registry:4443 -u [REDACTED] -p [REDACTED] on server1
  INFO [4c4ab467] Running docker login registry:4443 -u [REDACTED] -p [REDACTED] on server3
  INFO [f985bed4] Running docker login registry:4443 -u [REDACTED] -p [REDACTED] on server2
  INFO [427368d0] Finished in 0.232 seconds with exit status 0 (successful).
  INFO [f985bed4] Finished in 0.234 seconds with exit status 0 (successful).
  INFO [4c4ab467] Finished in 0.245 seconds with exit status 0 (successful).
$ kamal registry logout
  INFO [72b94e74] Running docker logout registry:4443 on server2
  INFO [d096054d] Running docker logout registry:4443 on server1
  INFO [8488da90] Running docker logout registry:4443 on server3
  INFO [72b94e74] Finished in 0.142 seconds with exit status 0 (successful).
  INFO [8488da90] Finished in 0.179 seconds with exit status 0 (successful).
  INFO [d096054d] Finished in 0.183 seconds with exit status 0 (successful).
```
Appending docs/commands/server.md
---
title: Server
---

# kamal server

```bash
$ kamal server
Commands:
  kamal server bootstrap       # Set up Docker to run Kamal apps
  kamal server exec            # Run a custom command on the server (use --help to show options)
  kamal server help [COMMAND]  # Describe subcommands or one specific subcommand
```

## [Bootstrap server](#bootstrap-server)

You can run `kamal server bootstrap` to set up Docker on your hosts.

It will check if Docker is installed and, if not, it will attempt to install it via [get.docker.com](https://get.docker.com/).

```bash
$ kamal server bootstrap
```

## [Execute command on all servers](#execute-command-on-all-servers)

Run a custom command on all servers.

```bash
$ kamal server exec "date"
Running 'date' on 867.53.0.9...
  INFO [e79c62bb] Running /usr/bin/env date on 867.53.0.9
  INFO [e79c62bb] Finished in 0.247 seconds with exit status 0 (successful).
App Host: 867.53.0.9
Thu Jun 13 08:06:19 AM UTC 2024
```

## [Execute command on primary server](#execute-command-on-primary-server)

Run a custom command on the primary server.

```bash
$ kamal server exec --primary "date"
Running 'date' on 867.53.0.9...
  INFO [8bbeb21a] Running /usr/bin/env date on 867.53.0.9
  INFO [8bbeb21a] Finished in 0.265 seconds with exit status 0 (successful).
App Host: 867.53.0.9
Thu Jun 13 08:07:09 AM UTC 2024
```

## [Execute interactive command on server](#execute-interactive-command-on-server)

Run an interactive command on the server.

```bash
$ kamal server exec --interactive "/bin/bash"
Running '/bin/bash' on 867.53.0.9 interactively...
root@server:~#
```
Appending docs/commands/proxy.md
---
title: Proxy
---

# kamal proxy

Kamal uses [kamal-proxy](https://github.com/basecamp/kamal-proxy) to proxy requests to the application containers, allowing us to have zero-downtime deployments.

```bash
$ kamal proxy
Commands:
  kamal proxy boot            # Boot proxy on servers
  kamal proxy boot_config <set|get|reset>  # Manage kamal-proxy boot configuration
  kamal proxy details         # Show details about proxy container from servers
  kamal proxy help [COMMAND]  # Describe subcommands or one specific subcommand
  kamal proxy logs            # Show log lines from proxy on servers
  kamal proxy reboot          # Reboot proxy on servers (stop container, remove container, start new container)
  kamal proxy remove          # Remove proxy container and image from servers
  kamal proxy restart         # Restart existing proxy container on servers
  kamal proxy start           # Start existing proxy container on servers
  kamal proxy stop            # Stop existing proxy container on servers
```

When you want to upgrade kamal-proxy, you can call `kamal proxy reboot`. This is going to cause a small outage on each server and will prompt for confirmation.

You can use a rolling reboot with `kamal proxy reboot --rolling` to avoid restarting on all servers simultaneously.

You can also use [pre-proxy-reboot](../../hooks/pre-proxy-reboot) and [post-proxy-reboot](../../hooks/post-proxy-reboot) hooks to remove and add the servers to upstream load balancers as you reboot them.

## Boot configuration

You can manage boot configuration for kamal-proxy with `kamal proxy boot_config`.

```bash
$ kamal proxy boot_config
Commands:
  kamal proxy boot_config set [OPTIONS]
  kamal proxy boot_config get
  kamal proxy boot_config reset

Options for 'set':
      [--publish], [--no-publish], [--skip-publish]   # Publish the proxy ports on the host
                                                      # Default: true
      [--http-port=N]                                 # HTTP port to publish on the host
                                                      # Default: 80
      [--https-port=N]                                # HTTPS port to publish on the host
                                                      # Default: 443
      [--log-max-size=LOG_MAX_SIZE]                   # Max size of proxy logs
                                                      # Default: 10m
      [--docker-options=option=value option2=value2]  # Docker options to pass to the proxy container
```

When set, the config will be stored on the server the proxy runs on.

If you are running more than one application on a single server, there is only one proxy, and the boot config is shared, so you'll need to manage it centrally.

The configuration will be loaded at boot time when calling `kamal proxy boot` or `kamal proxy reboot`.
Appending docs/commands/redeploy.md
---
title: Redeploy
---

# kamal redeploy

Deploy your app, but skip bootstrapping servers, starting kamal-proxy, pruning, and registry login.

You must run [`kamal deploy`](../deploy) at least once first.
Appending docs/commands/secrets.md
---
title: Secrets
---

# kamal secrets

```bash
$ kamal secrets
Commands:
  kamal secrets extract                                                     # Extract a single secret from the results of a fetch call
  kamal secrets fetch [SECRETS...] --account=ACCOUNT -a, --adapter=ADAPTER  # Fetch secrets from a vault
  kamal secrets help [COMMAND]                                              # Describe subcommands or one specific subcommand
  kamal secrets print                                                       # Print the secrets (for debugging)
```

Use these to read secrets from common password managers (currently 1Password, LastPass, and Bitwarden).

The helpers will handle signing in, asking for passwords, and efficiently fetching the secrets:

These are designed to be used with [command substitution](https://github.com/bkeepers/dotenv?tab=readme-ov-file#command-substitution) in `.kamal/secrets`

```shell
# .kamal/secrets

SECRETS=$(kamal secrets fetch ...)

REGISTRY_PASSWORD=$(kamal secrets extract REGISTRY_PASSWORD $SECRETS)
DB_PASSWORD=$(kamal secrets extract DB_PASSWORD $SECRETS)
```

## 1Password

First, install and configure [the 1Password CLI](https://developer.1password.com/docs/cli/get-started/).

Use the adapter `1password`:

```bash
# Fetch from item `MyItem` in the vault `MyVault`
kamal secrets fetch --adapter 1password --account myaccount --from MyVault/MyItem REGISTRY_PASSWORD DB_PASSWORD

# Fetch from sections of item `MyItem` in the vault `MyVault`
kamal secrets fetch --adapter 1password --account myaccount --from MyVault/MyItem common/REGISTRY_PASSWORD production/DB_PASSWORD

# Fetch from separate items MyItem, MyItem2
kamal secrets fetch --adapter 1password --account myaccount --from MyVault MyItem/REGISTRY_PASSWORD MyItem2/DB_PASSWORD

# Fetch from multiple vaults
kamal secrets fetch --adapter 1password --account myaccount MyVault/MyItem/REGISTRY_PASSWORD MyVault2/MyItem2/DB_PASSWORD

# All three of these will extract the secret
kamal secrets extract REGISTRY_PASSWORD <SECRETS-FETCH-OUTPUT>
kamal secrets extract MyItem/REGISTRY_PASSWORD <SECRETS-FETCH-OUTPUT>
kamal secrets extract MyVault/MyItem/REGISTRY_PASSWORD <SECRETS-FETCH-OUTPUT>
```

## LastPass

First, install and configure [the LastPass CLI](https://github.com/lastpass/lastpass-cli).

Use the adapter `lastpass`:

```bash
# Fetch passwords
kamal secrets fetch --adapter lastpass --account email@example.com REGISTRY_PASSWORD DB_PASSWORD

# Fetch passwords from a folder
kamal secrets fetch --adapter lastpass --account email@example.com --from MyFolder REGISTRY_PASSWORD DB_PASSWORD

# Fetch passwords from multiple folders
kamal secrets fetch --adapter lastpass --account email@example.com MyFolder/REGISTRY_PASSWORD MyFolder2/DB_PASSWORD

# Extract the secret
kamal secrets extract REGISTRY_PASSWORD <SECRETS-FETCH-OUTPUT>
kamal secrets extract MyFolder/REGISTRY_PASSWORD <SECRETS-FETCH-OUTPUT>
```

## Bitwarden

First, install and configure [the Bitwarden CLI](https://bitwarden.com/help/cli/).

Use the adapter `bitwarden`:

```bash
# Fetch passwords
kamal secrets fetch --adapter bitwarden --account email@example.com REGISTRY_PASSWORD DB_PASSWORD

# Fetch passwords from an item
kamal secrets fetch --adapter bitwarden --account email@example.com --from MyItem REGISTRY_PASSWORD DB_PASSWORD

# Fetch passwords from multiple items
kamal secrets fetch --adapter bitwarden --account email@example.com MyItem/REGISTRY_PASSWORD MyItem2/DB_PASSWORD

# Extract the secret
kamal secrets extract REGISTRY_PASSWORD <SECRETS-FETCH-OUTPUT>
kamal secrets extract MyItem/REGISTRY_PASSWORD <SECRETS-FETCH-OUTPUT>
```

## Bitwarden Secrets Manager

First, install and configure [the Bitwarden Secrets Manager CLI](https://bitwarden.com/help/secrets-manager-cli/#download-and-install).

Use the adapter 'bitwarden-sm':

```bash
# Fetch all secrets that the machine account has access to
kamal secrets fetch --adapter bitwarden-sm all

# Fetch secrets from a project
kamal secrets fetch --adapter bitwarden-sm MyProjectID/all

# Extract the secret
kamal secrets extract REGISTRY_PASSWORD <SECRETS-FETCH-OUTPUT>
```

## AWS Secrets Manager

First, install and configure [the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html).

Use the adapter `aws_secrets_manager`:

```bash
# Fetch passwords
kamal secrets fetch --adapter aws_secrets_manager --account default REGISTRY_PASSWORD DB_PASSWORD

# Fetch passwords from an item
kamal secrets fetch --adapter aws_secrets_manager --account default --from myapp/ REGISTRY_PASSWORD DB_PASSWORD

# Fetch passwords from multiple items
kamal secrets fetch --adapter aws_secrets_manager --account default myapp/REGISTRY_PASSWORD myapp/DB_PASSWORD

# Extract the secret
kamal secrets extract REGISTRY_PASSWORD <SECRETS-FETCH-OUTPUT>
kamal secrets extract MyItem/REGISTRY_PASSWORD <SECRETS-FETCH-OUTPUT>
```

**Note:** The `--account` option should be set to your AWS CLI profile name, which is typically `default`. Ensure that your AWS CLI is configured with the necessary permissions to access AWS Secrets Manager.

## Doppler

First, install and configure [the Doppler CLI](https://docs.doppler.com/docs/install-cli).

Use the adapter `doppler`:

```bash
# Fetch passwords
kamal secrets fetch --adapter doppler --from my-project/prd REGISTRY_PASSWORD DB_PASSWORD

# The project/config pattern is also supported in this way
kamal secrets fetch --adapter doppler my-project/prd/REGISTRY_PASSWORD my-project/prd/DB_PASSWORD

# Extract the secret
kamal secrets extract REGISTRY_PASSWORD <SECRETS-FETCH-OUTPUT>
kamal secrets extract DB_PASSWORD <SECRETS-FETCH-OUTPUT>
```

Doppler organizes secrets in "projects" (like `my-awesome-project`) and "configs" (like `prod`, `stg`, etc), use the pattern `project/config` when defining the `--from` option.

The doppler adapter does not use the `--account` option, if given it will be ignored.

## GCP Secret Manager

First, install and configure the [gcloud CLI](https://cloud.google.com/sdk/gcloud/reference/secrets).

The `--account` flag selects an account configured in `gcloud`, and the `--from` flag specifies the **GCP project ID** to be used. The string `default` can be used with the `--account` and `--from` flags to use `gcloud`'s default credentials and project, respectively.

Use the adapter `gcp`:

```bash
# Fetch a secret with an explicit project name, credentials, and secret version:
kamal secrets fetch --adapter=gcp --account=default --from=default my-secret/latest

# The project name can be added as a prefix to the secret name instead of using --from:
kamal secrets fetch --adapter=gcp --account=default default/my-secret/latest

# The 'latest' version is used by default, so it can be omitted as well:
kamal secrets fetch --adapter=gcp --account=default default/my-secret

# If the default project is used, the prefix can also be left out entirely, leading to the simplest
# way to fetch a secret using the default project and credentials, and the latest version of the
# secret:
kamal secrets fetch --adapter=gcp --account=default my-secret

# Multiple secrets can be fetched at the same time.
# Fetch `my-secret` and `another-secret` from the project `my-project`:
kamal secrets fetch --adapter=gcp \
  --account=default \
  --from=my-project \
  my-secret another-secret

# Secrets can be fetched from multiple projects at the same time.
# Fetch from multiple projects, using default to refer to the default project:
kamal secrets fetch --adapter=gcp \
  --account=default \
  default/my-secret my-project/another-secret

# Specific secret versions can be fetched.
# Fetch version 123 of the secret `my-secret` in the default project (the default behavior is to
# fetch `latest`)
kamal secrets fetch --adapter=gcp \
  --account=default \
  default/my-secret/123

# Credentials other than the default can also be used.
# Fetch a secret using the `user@example.com` credentials:
kamal secrets fetch --adapter=gcp \
  --account=user@example.com \
  my-secret

# Service account impersonation and delegation chains are available.
# Fetch a secret as `user@example.com`, impersonating `service-account@example.com` with
# `delegate@example.com` as a delegate
kamal secrets fetch --adapter=gcp \
  --account="user@example.com|delegate@example.com,service-account@example.com" \
  my-secret
```
Appending docs/commands/rollback.md
---
title: kamal rollback
---

# kamal rollback

You can rollback a deployment with `kamal rollback`.

If you've discovered a bad deploy, you can quickly rollback to a previous image. You can see what old containers are available for rollback by running `kamal app containers -q`. It'll give you a presentation similar to `kamal app details`, but include all the old containers as well.

```
App Host: 192.168.0.1
CONTAINER ID   IMAGE                                                                         COMMAND                    CREATED          STATUS                      PORTS      NAMES
1d3c91ed1f51   registry.digitalocean.com/user/app:6ef8a6a84c525b123c5245345a8483f86d05a123   "/rails/bin/docker-e..."   19 minutes ago   Up 19 minutes               3000/tcp   chat-6ef8a6a84c525b123c5245345a8483f86d05a123
539f26b28369   registry.digitalocean.com/user/app:e5d9d7c2b898289dfbc5f7f1334140d984eedae4   "/rails/bin/docker-e..."   31 minutes ago   Exited (1) 27 minutes ago              chat-e5d9d7c2b898289dfbc5f7f1334140d984eedae4

App Host: 192.168.0.2
CONTAINER ID   IMAGE                                                                         COMMAND                    CREATED          STATUS                      PORTS      NAMES
badb1aa51db4   registry.digitalocean.com/user/app:6ef8a6a84c525b123c5245345a8483f86d05a123   "/rails/bin/docker-e..."   19 minutes ago   Up 19 minutes               3000/tcp   chat-6ef8a6a84c525b123c5245345a8483f86d05a123
6f170d1172ae   registry.digitalocean.com/user/app:e5d9d7c2b898289dfbc5f7f1334140d984eedae4   "/rails/bin/docker-e..."   31 minutes ago   Exited (1) 27 minutes ago              chat-e5d9d7c2b898289dfbc5f7f1334140d984eedae4
```

From the example above, we can see that `e5d9d7c2b898289dfbc5f7f1334140d984eedae4` was the last version, so it's available as a rollback target. We can perform this rollback by running `kamal rollback e5d9d7c2b898289dfbc5f7f1334140d984eedae4`.

That'll stop `6ef8a6a84c525b123c5245345a8483f86d05a123` and then start a new container running the same image as `e5d9d7c2b898289dfbc5f7f1334140d984eedae4`. Nothing needs to be downloaded from the registry.

**Note:** By default, old containers are pruned after 3 days when you run `kamal deploy`.
Appending docs/commands/help.md
---
title: Help
---

# kamal help

Displays help messages. Run `kamal help [command]` for details on a specific command.

```bash
$ kamal help
  kamal accessory           # Manage accessories (db/redis/search)
  kamal app                 # Manage application
  kamal audit               # Show audit log from servers
  kamal build               # Build application image
  kamal config              # Show combined config (including secrets!)
  kamal deploy              # Deploy app to servers
  kamal details             # Show details about all containers
  kamal docs [SECTION]      # Show Kamal configuration documentation
  kamal help [COMMAND]      # Describe available commands or one specific command
  kamal init                # Create config stub in config/deploy.yml and env stub in .env
  kamal lock                # Manage the deploy lock
  kamal proxy               # Manage kamal-proxy
  kamal prune               # Prune old application images and containers
  kamal redeploy            # Deploy app to servers without bootstrapping servers, starting kamal-proxy, pruning, and registry login
  kamal registry            # Login and -out of the image registry
  kamal remove              # Remove kamal-proxy, app, accessories, and registry session from servers
  kamal rollback [VERSION]  # Rollback app to VERSION
  kamal secrets             # Helpers for extracting secrets
  kamal server              # Bootstrap servers with curl and Docker
  kamal setup               # Setup all accessories, push the env, and deploy app to servers
  kamal upgrade             # Upgrade from Kamal 1.x to 2.0
  kamal version             # Show Kamal version

Options:
  -v, [--verbose], [--no-verbose], [--skip-verbose]  # Detailed logging
  -q, [--quiet], [--no-quiet], [--skip-quiet]        # Minimal logging
      [--version=VERSION]                            # Run commands against a specific app version
  -p, [--primary], [--no-primary], [--skip-primary]  # Run commands only on primary host instead of all
  -h, [--hosts=HOSTS]                                # Run commands on these hosts instead of all (separate by comma, supports wildcards with *)
  -r, [--roles=ROLES]                                # Run commands on these roles instead of all (separate by comma, supports wildcards with *)
  -c, [--config-file=CONFIG_FILE]                    # Path to config file
                                                     # Default: config/deploy.yml
  -d, [--destination=DESTINATION]                    # Specify destination to be used for config file (staging -> deploy.staging.yml)
  -H, [--skip-hooks]                                 # Don't run hooks
                                                     # Default: false
```
Appending docs/commands/accessory.md
---
title: Accessory
---

# kamal accessory

Accessories are long-lived services that your app depends on. They are not updated when you deploy.

They are not proxied, so rebooting will have a small period of downtime. You can map volumes from the host server into your container for persistence across reboots.

Run `kamal accessory` to view and manage your accessories.

```bash
$ kamal accessory
Commands:
  kamal accessory boot [NAME]        # Boot new accessory service on host (use NAME=all to boot all accessories)
  kamal accessory details [NAME]     # Show details about accessory on host (use NAME=all to show all accessories)
  kamal accessory exec [NAME] [CMD]  # Execute a custom command on servers (use --help to show options)
  kamal accessory help [COMMAND]     # Describe subcommands or one specific subcommand
  kamal accessory logs [NAME]        # Show log lines from accessory on host (use --help to show options)
  kamal accessory reboot [NAME]      # Reboot existing accessory on host (stop container, remove container, start new container; use NAME=all to boot all accessories)
  kamal accessory remove [NAME]      # Remove accessory container, image and data directory from host (use NAME=all to remove all accessories)
  kamal accessory restart [NAME]     # Restart existing accessory container on host
  kamal accessory start [NAME]       # Start existing accessory container on host
  kamal accessory stop [NAME]        # Stop existing accessory container on host
```

To update an accessory, update the image in your config and run `kamal accessory reboot [NAME]`.

Example:

```bash
$ kamal accessory boot all
Running the pre-connect hook...
  INFO [bd04b11b] Running /usr/bin/env .kamal/hooks/pre-connect on localhost
  INFO [bd04b11b] Finished in 0.003 seconds with exit status 0 (successful).
  INFO [681a028b] Running /usr/bin/env mkdir -p .kamal on server2
  INFO [e3495d1d] Running /usr/bin/env mkdir -p .kamal on server1
  INFO [e7c5c159] Running /usr/bin/env mkdir -p .kamal on server3
  INFO [e3495d1d] Finished in 0.170 seconds with exit status 0 (successful).
  INFO [681a028b] Finished in 0.182 seconds with exit status 0 (successful).
  INFO [e7c5c159] Finished in 0.185 seconds with exit status 0 (successful).
  INFO [83153af3] Running /usr/bin/env mkdir -p .kamal/locks on server1
  INFO [83153af3] Finished in 0.028 seconds with exit status 0 (successful).
Acquiring the deploy lock...
  INFO [416a654c] Running docker login registry:4443 -u [REDACTED] -p [REDACTED] on server1
  INFO [3fb56559] Running docker login registry:4443 -u [REDACTED] -p [REDACTED] on server2
  INFO [3fb56559] Finished in 0.065 seconds with exit status 0 (successful).
  INFO [416a654c] Finished in 0.080 seconds with exit status 0 (successful).
  INFO [2705083f] Running docker run --name custom-busybox --detach --restart unless-stopped --log-opt max-size="10m" --env-file .kamal/env/accessories/custom-busybox.env --label service="custom-busybox" registry:4443/busybox:1.36.0 sh -c 'echo "Starting busybox..."; trap exit term; while true; do sleep 1; done' on server2
  INFO [3cb3adb6] Running docker run --name custom-busybox --detach --restart unless-stopped --log-opt max-size="10m" --env-file .kamal/env/accessories/custom-busybox.env --label service="custom-busybox" registry:4443/busybox:1.36.0 sh -c 'echo "Starting busybox..."; trap exit term; while true; do sleep 1; done' on server1
  INFO [3cb3adb6] Finished in 0.552 seconds with exit status 0 (successful).
  INFO [2705083f] Finished in 0.566 seconds with exit status 0 (successful).
Releasing the deploy lock...
```
Appending docs/commands/setup.md
---
title: Setup
---

# kamal setup

Kamal setup will run everything required to deploy an application to a fresh host.

It will:

1. Install Docker on all servers, if it has permission and it is not already installed.
2. Boot all accessories.
3. Deploy the app (see [`kamal deploy`](../deploy)).
Appending docs/commands/app.md
---
title: App
---

# kamal app

Run `kamal app` to manage your running apps.

To deploy new versions of the app, see `kamal deploy` and `kamal rollback`.

You can use `kamal app exec` to [run commands on servers](../running-commands-on-servers).

```bash
$ kamal app
Commands:
  kamal app boot              # Boot app on servers (or reboot app if already running)
  kamal app containers        # Show app containers on servers
  kamal app details           # Show details about app containers
  kamal app exec [CMD]        # Execute a custom command on servers within the app container (use --help to show options)
  kamal app help [COMMAND]    # Describe subcommands or one specific subcommand
  kamal app images            # Show app images on servers
  kamal app logs              # Show log lines from app on servers (use --help to show options)
  kamal app remove            # Remove app containers and images from servers
  kamal app stale_containers  # Detect app stale containers
  kamal app start             # Start existing app container on servers
  kamal app stop              # Stop app container on servers
  kamal app version           # Show app version currently running on servers
```
Appending docs/commands/build.md
---
title: Build
---

# kamal build

Build your app images and push them to your servers. These commands are called indirectly by `kamal deploy` and `kamal redeploy`.

By default, Kamal will only build files you have committed to your Git repository. However, you can configure Kamal to use the current context (instead of a Git archive of HEAD) by setting the [build context](https://kamal-deploy.org/docs/configuration/builders/#build-context).

```bash
$ kamal build
Commands:
  kamal build create          # Create a build setup
  kamal build deliver         # Build app and push app image to registry then pull image on servers
  kamal build dev             # Build using the working directory, tag it as dirty, and push to local image store.
  kamal build details         # Show build setup
  kamal build help [COMMAND]  # Describe subcommands or one specific subcommand
  kamal build pull            # Pull app image from registry onto servers
  kamal build push            # Build and push app image to registry
  kamal build remove          # Remove build setup
```

The `build dev` and `build push` commands also support an `--output` option which specifies where the image should be pushed. `build push` defaults to "registry", and `build dev` defaults to "docker" which is the local image store. Any exported type supported by the `docker buildx build` option [`--output`](https://docs.docker.com/reference/cli/docker/buildx/build/#output) is allowed.


Examples:

```bash
$ kamal build push
Running the pre-connect hook...
  INFO [92ebc200] Running /usr/bin/env .kamal/hooks/pre-connect on localhost
  INFO [92ebc200] Finished in 0.004 seconds with exit status 0 (successful).
  INFO [cbbad07e] Running /usr/bin/env mkdir -p .kamal on server1
  INFO [e6ac30e7] Running /usr/bin/env mkdir -p .kamal on server3
  INFO [a1adae3a] Running /usr/bin/env mkdir -p .kamal on server2
  INFO [cbbad07e] Finished in 0.145 seconds with exit status 0 (successful).
  INFO [a1adae3a] Finished in 0.179 seconds with exit status 0 (successful).
  INFO [e6ac30e7] Finished in 0.182 seconds with exit status 0 (successful).
  INFO [c6242009] Running /usr/bin/env mkdir -p .kamal/locks on server1
  INFO [c6242009] Finished in 0.009 seconds with exit status 0 (successful).
Acquiring the deploy lock...
  INFO [427ae9bc] Running docker --version on localhost
  INFO [427ae9bc] Finished in 0.039 seconds with exit status 0 (successful).
Running the pre-build hook...
  INFO [2f120630] Running /usr/bin/env .kamal/hooks/pre-build on localhost
  INFO [2f120630] Finished in 0.004 seconds with exit status 0 (successful).
  INFO [ad386911] Running /usr/bin/env git archive --format=tar HEAD | docker build -t registry:4443/app:75bf6fa40b975cbd8aec05abf7164e0982f185ac -t registry:4443/app:latest --label service="app" --build-arg [REDACTED] --file Dockerfile - && docker push registry:4443/app:75bf6fa40b975cbd8aec05abf7164e0982f185ac && docker push registry:4443/app:latest on localhost
 DEBUG [ad386911] Command: /usr/bin/env git archive --format=tar HEAD | docker build -t registry:4443/app:75bf6fa40b975cbd8aec05abf7164e0982f185ac -t registry:4443/app:latest --label service="app" --build-arg [REDACTED] --file Dockerfile - && docker push registry:4443/app:75bf6fa40b975cbd8aec05abf7164e0982f185ac && docker push registry:4443/app:latest
 DEBUG [ad386911] 	#0 building with "default" instance using docker driver
 DEBUG [ad386911]
 DEBUG [ad386911] 	#1 [internal] load remote build context
 DEBUG [ad386911] 	#1 CACHED
 DEBUG [ad386911]
 DEBUG [ad386911] 	#2 copy /context /
 DEBUG [ad386911] 	#2 CACHED
 DEBUG [ad386911]
 DEBUG [ad386911] 	#3 [internal] load metadata for registry:4443/nginx:1-alpine-slim
 DEBUG [ad386911] 	#3 DONE 0.0s
 DEBUG [ad386911]
 DEBUG [ad386911] 	#4 [1/5] FROM registry:4443/nginx:1-alpine-slim@sha256:558cdef0693faaa02c0b81c21b5d6f4b4fe24e3ac747581f3e6e8f5c4032db58
 DEBUG [ad386911] 	#4 DONE 0.0s
 DEBUG [ad386911]
 DEBUG [ad386911] 	#5 [4/5] RUN mkdir -p /usr/share/nginx/html/versions && echo "version" > /usr/share/nginx/html/versions/75bf6fa40b975cbd8aec05abf7164e0982f185ac
 DEBUG [ad386911] 	#5 CACHED
 DEBUG [ad386911]
 DEBUG [ad386911] 	#6 [2/5] COPY default.conf /etc/nginx/conf.d/default.conf
 DEBUG [ad386911] 	#6 CACHED
 DEBUG [ad386911]
 DEBUG [ad386911] 	#7 [3/5] RUN echo 75bf6fa40b975cbd8aec05abf7164e0982f185ac > /usr/share/nginx/html/version
 DEBUG [ad386911] 	#7 CACHED
 DEBUG [ad386911]
 DEBUG [ad386911] 	#8 [5/5] RUN mkdir -p /usr/share/nginx/html/versions && echo "hidden" > /usr/share/nginx/html/versions/.hidden
 DEBUG [ad386911] 	#8 CACHED
 DEBUG [ad386911]
 DEBUG [ad386911] 	#9 exporting to image
 DEBUG [ad386911] 	#9 exporting layers done
 DEBUG [ad386911] 	#9 writing image sha256:ed9205d697e5f9f735e84e341a19a3d379b9b4a8dc5d04b6219bda29e6126489 done
 DEBUG [ad386911] 	#9 naming to registry:4443/app:75bf6fa40b975cbd8aec05abf7164e0982f185ac done
 DEBUG [ad386911] 	#9 naming to registry:4443/app:latest done
 DEBUG [ad386911] 	#9 DONE 0.0s
 DEBUG [ad386911] 	The push refers to repository [registry:4443/app]
 DEBUG [ad386911] 	7e49189613ab: Preparing
 DEBUG [ad386911] 	054c18a8e0a6: Preparing
 DEBUG [ad386911] 	1552c04abfaa: Preparing
 DEBUG [ad386911] 	36f2f66132ea: Preparing
 DEBUG [ad386911] 	d5e2fb5f3301: Preparing
 DEBUG [ad386911] 	8fde05710e93: Preparing
 DEBUG [ad386911] 	fdf572380e92: Preparing
 DEBUG [ad386911] 	a031a04401d0: Preparing
 DEBUG [ad386911] 	ecb78d985cad: Preparing
 DEBUG [ad386911] 	3e0e830ccd77: Preparing
 DEBUG [ad386911] 	7c504f21be85: Preparing
 DEBUG [ad386911] 	fdf572380e92: Waiting
 DEBUG [ad386911] 	a031a04401d0: Waiting
 DEBUG [ad386911] 	ecb78d985cad: Waiting
 DEBUG [ad386911] 	3e0e830ccd77: Waiting
 DEBUG [ad386911] 	7c504f21be85: Waiting
 DEBUG [ad386911] 	8fde05710e93: Waiting
 DEBUG [ad386911] 	054c18a8e0a6: Layer already exists
 DEBUG [ad386911] 	7e49189613ab: Layer already exists
 DEBUG [ad386911] 	36f2f66132ea: Layer already exists
 DEBUG [ad386911] 	d5e2fb5f3301: Layer already exists
 DEBUG [ad386911] 	1552c04abfaa: Layer already exists
 DEBUG [ad386911] 	8fde05710e93: Layer already exists
 DEBUG [ad386911] 	fdf572380e92: Layer already exists
 DEBUG [ad386911] 	a031a04401d0: Layer already exists
 DEBUG [ad386911] 	3e0e830ccd77: Layer already exists
 DEBUG [ad386911] 	ecb78d985cad: Layer already exists
 DEBUG [ad386911] 	7c504f21be85: Layer already exists
 DEBUG [ad386911] 	75bf6fa40b975cbd8aec05abf7164e0982f185ac: digest: sha256:68e534dab98fc7c65c8e2353f6414e9c6c812481deea8d57ae6b0b0140ec40d5 size: 2604
 DEBUG [ad386911] 	The push refers to repository [registry:4443/app]
 DEBUG [ad386911] 	7e49189613ab: Preparing
 DEBUG [ad386911] 	054c18a8e0a6: Preparing
 DEBUG [ad386911] 	1552c04abfaa: Preparing
 DEBUG [ad386911] 	36f2f66132ea: Preparing
 DEBUG [ad386911] 	d5e2fb5f3301: Preparing
 DEBUG [ad386911] 	8fde05710e93: Preparing
 DEBUG [ad386911] 	fdf572380e92: Preparing
 DEBUG [ad386911] 	a031a04401d0: Preparing
 DEBUG [ad386911] 	ecb78d985cad: Preparing
 DEBUG [ad386911] 	3e0e830ccd77: Preparing
 DEBUG [ad386911] 	7c504f21be85: Preparing
 DEBUG [ad386911] 	fdf572380e92: Waiting
 DEBUG [ad386911] 	a031a04401d0: Waiting
 DEBUG [ad386911] 	ecb78d985cad: Waiting
 DEBUG [ad386911] 	3e0e830ccd77: Waiting
 DEBUG [ad386911] 	7c504f21be85: Waiting
 DEBUG [ad386911] 	8fde05710e93: Waiting
 DEBUG [ad386911] 	36f2f66132ea: Layer already exists
 DEBUG [ad386911] 	7e49189613ab: Layer already exists
 DEBUG [ad386911] 	054c18a8e0a6: Layer already exists
 DEBUG [ad386911] 	1552c04abfaa: Layer already exists
 DEBUG [ad386911] 	d5e2fb5f3301: Layer already exists
 DEBUG [ad386911] 	8fde05710e93: Layer already exists
 DEBUG [ad386911] 	fdf572380e92: Layer already exists
 DEBUG [ad386911] 	a031a04401d0: Layer already exists
 DEBUG [ad386911] 	ecb78d985cad: Layer already exists
 DEBUG [ad386911] 	3e0e830ccd77: Layer already exists
 DEBUG [ad386911] 	7c504f21be85: Layer already exists
 DEBUG [ad386911] 	latest: digest: sha256:68e534dab98fc7c65c8e2353f6414e9c6c812481deea8d57ae6b0b0140ec40d5 size: 2604
  INFO [ad386911] Finished in 0.502 seconds with exit status 0 (successful).
Releasing the deploy lock...
```
Appending docs/commands/init.md
---
title: Init
---

# kamal init

Creates the files needed to deploy your application with `kamal`.

```bash
$ kamal init
Created configuration file in config/deploy.yml
Created .kamal/secrets file
Created sample hooks in .kamal/hooks
```
Appending docs/commands/docs.md
---
title: Help
---

# kamal docs

Outputs configuration documentation.
Appending docs/commands/prune.md
---
title: Prune
---

# kamal prune

Prune old containers and images.

Kamal keeps the last 5 deployed containers and the images they are using. Pruning deletes all older containers and images.

```bash
$ kamal help prune
Commands:
  kamal prune all             # Prune unused images and stopped containers
  kamal prune containers      # Prune all stopped containers, except the last n (default 5)
  kamal prune help [COMMAND]  # Describe subcommands or one specific subcommand
  kamal prune images          # Prune unused images
```
Appending docs/commands/details.md
---
title: Details
---

# kamal details

Shows details of all your containers.

```bash
$ kamal details -q
Traefik Host: server2
CONTAINER ID   IMAGE                         COMMAND                  CREATED          STATUS          PORTS                NAMES
b220af815ea7   registry:4443/traefik:v2.10   "/entrypoint.sh --pr…"   52 minutes ago   Up 52 minutes   0.0.0.0:80->80/tcp   traefik

Traefik Host: server1
CONTAINER ID   IMAGE                         COMMAND                  CREATED          STATUS          PORTS                NAMES
e0abb837120a   registry:4443/traefik:v2.10   "/entrypoint.sh --pr…"   52 minutes ago   Up 52 minutes   0.0.0.0:80->80/tcp   traefik

App Host: server2
CONTAINER ID   IMAGE                                                        COMMAND                  CREATED          STATUS                    PORTS     NAMES
3ec7c8122988   registry:4443/app:75bf6fa40b975cbd8aec05abf7164e0982f185ac   "/docker-entrypoint.…"   52 minutes ago   Up 52 minutes (healthy)   80/tcp    app-web-75bf6fa40b975cbd8aec05abf7164e0982f185ac

App Host: server1
CONTAINER ID   IMAGE                                                        COMMAND                  CREATED          STATUS                    PORTS     NAMES
32ae39c98b29   registry:4443/app:75bf6fa40b975cbd8aec05abf7164e0982f185ac   "/docker-entrypoint.…"   52 minutes ago   Up 52 minutes (healthy)   80/tcp    app-web-75bf6fa40b975cbd8aec05abf7164e0982f185ac

App Host: server3
CONTAINER ID   IMAGE                                                        COMMAND                  CREATED          STATUS          PORTS     NAMES
df8990876d14   registry:4443/app:75bf6fa40b975cbd8aec05abf7164e0982f185ac   "/docker-entrypoint.…"   52 minutes ago   Up 52 minutes   80/tcp    app-workers-75bf6fa40b975cbd8aec05abf7164e0982f185ac

CONTAINER ID   IMAGE                          COMMAND                   CREATED          STATUS          PORTS     NAMES
14857a6cb6b1   registry:4443/busybox:1.36.0   "sh -c 'echo \"Starti…"   42 minutes ago   Up 42 minutes             custom-busybox
CONTAINER ID   IMAGE                          COMMAND                   CREATED          STATUS          PORTS     NAMES
17f3ff88ff9f   registry:4443/busybox:1.36.0   "sh -c 'echo \"Starti…"   42 minutes ago   Up 42 minutes             custom-busybox
```
Appending docs/commands/lock.md
---
title: Lock
---

# kamal lock

Manage deployment locks.

Commands that are unsafe to run concurrently will take a lock while they run. The lock is an atomically created directory in the `.kamal` directory on the primary server.

You can manage them directly — for example, clearing a leftover lock from a failed command or preventing deployments during a maintenance window.

```bash
$ kamal lock
Commands:
  kamal lock acquire -m, --message=MESSAGE  # Acquire the deploy lock
  kamal lock help [COMMAND]                 # Describe subcommands or one specific subcommand
  kamal lock release                        # Release the deploy lock
  kamal lock status                         # Report lock status
```

Example:

```bash
$ kamal lock status
  INFO [f085f083] Running /usr/bin/env mkdir -p .kamal on server1
  INFO [f085f083] Finished in 0.146 seconds with exit status 0 (successful).
There is no deploy lock
$ kamal lock acquire -m "Maintenance in progress"
  INFO [d9f63437] Running /usr/bin/env mkdir -p .kamal on server1
  INFO [d9f63437] Finished in 0.138 seconds with exit status 0 (successful).
Acquired the deploy lock
$ kamal lock status
  INFO [9315755d] Running /usr/bin/env mkdir -p .kamal on server1
  INFO [9315755d] Finished in 0.130 seconds with exit status 0 (successful).
Locked by: Deployer at 2024-04-05T08:32:46Z
Version: 75bf6fa40b975cbd8aec05abf7164e0982f185ac
Message: Maintenance in progress
$ kamal lock release
  INFO [7d5718a8] Running /usr/bin/env mkdir -p .kamal on server1
  INFO [7d5718a8] Finished in 0.137 seconds with exit status 0 (successful).
Released the deploy lock
$ kamal lock status
  INFO [f5900cc8] Running /usr/bin/env mkdir -p .kamal on server1
  INFO [f5900cc8] Finished in 0.132 seconds with exit status 0 (successful).
There is no deploy lock
```
Appending docs/commands/remove.md
---
title: Remove
---

# kamal remove

This will remove the app, kamal-proxy, and accessory containers and log out of the Docker registry.

It will prompt for confirmation unless you add the `-y` option.
Appending docs/commands/version.md
---
title: Version
---

# kamal version

Returns the version of Kamal you have installed.

```bash
$ kamal version
2.5.2
```
Appending docs/commands/deploy.md
---
title: Deploy
---

# kamal deploy

Build and deploy your app to all servers. By default, it will build the currently checked out version of the app.

Kamal will use [kamal-proxy](https://github.com/basecamp/kamal-proxy) to seamlessly move requests from the old version of the app to the new one without downtime.

The deployment process is:

1. Log in to the Docker registry locally and on all servers.
2. Build the app image, push it to the registry, and pull it onto the servers.
3. Ensure kamal-proxy is running and accepting traffic on ports 80 and 443.
4. Start a new container with the version of the app that matches the current Git version hash.
5. Tell kamal-proxy to route traffic to the new container once it is responding with `200 OK` to `GET /up`.
6. Stop the old container running the previous version of the app.
7. Prune unused images and stopped containers to ensure servers don't fill up.

```bash
Usage:
  kamal deploy

Options:
  -P, [--skip-push]                                  # Skip image build and push
                                                     # Default: false
  -v, [--verbose], [--no-verbose], [--skip-verbose]  # Detailed logging
  -q, [--quiet], [--no-quiet], [--skip-quiet]        # Minimal logging
      [--version=VERSION]                            # Run commands against a specific app version
  -p, [--primary], [--no-primary], [--skip-primary]  # Run commands only on primary host instead of all
  -h, [--hosts=HOSTS]                                # Run commands on these hosts instead of all (separate by comma, supports wildcards with *)
  -r, [--roles=ROLES]                                # Run commands on these roles instead of all (separate by comma, supports wildcards with *)
  -c, [--config-file=CONFIG_FILE]                    # Path to config file
                                                     # Default: config/deploy.yml
  -d, [--destination=DESTINATION]                    # Specify destination to be used for config file (staging -> deploy.staging.yml)
  -H, [--skip-hooks]                                 # Don't run hooks
                                                     # Default: false
```
Appending docs/commands/view-all-commands.md
---
title: View all commands
---

# View all commands

You can view all of the commands by running `kamal --help`.

```bash
$ kamal --help
Commands:
  kamal accessory           # Manage accessories (db/redis/search)
  kamal app                 # Manage application
  kamal audit               # Show audit log from servers
  kamal build               # Build application image
  kamal config              # Show combined config (including secrets!)
  kamal deploy              # Deploy app to servers
  kamal details             # Show details about all containers
  kamal docs [SECTION]      # Show Kamal configuration documentation
  kamal help [COMMAND]      # Describe available commands or one specific command
  kamal init                # Create config stub in config/deploy.yml and env stub in .env
  kamal lock                # Manage the deploy lock
  kamal proxy               # Manage kamal-proxy
  kamal prune               # Prune old application images and containers
  kamal redeploy            # Deploy app to servers without bootstrapping servers, starting kamal-proxy, pruning, and registry login
  kamal registry            # Login and -out of the image registry
  kamal remove              # Remove kamal-proxy, app, accessories, and registry session from servers
  kamal rollback [VERSION]  # Rollback app to VERSION
  kamal secrets             # Helpers for extracting secrets
  kamal server              # Bootstrap servers with curl and Docker
  kamal setup               # Setup all accessories, push the env, and deploy app to servers
  kamal upgrade             # Upgrade from Kamal 1.x to 2.0
  kamal version             # Show Kamal version

Options:
  -v, [--verbose], [--no-verbose], [--skip-verbose]  # Detailed logging
  -q, [--quiet], [--no-quiet], [--skip-quiet]        # Minimal logging
      [--version=VERSION]                            # Run commands against a specific app version
  -p, [--primary], [--no-primary], [--skip-primary]  # Run commands only on primary host instead of all
  -h, [--hosts=HOSTS]                                # Run commands on these hosts instead of all (separate by comma, supports wildcards with *)
  -r, [--roles=ROLES]                                # Run commands on these roles instead of all (separate by comma, supports wildcards with *)
  -c, [--config-file=CONFIG_FILE]                    # Path to config file
                                                     # Default: config/deploy.yml
  -d, [--destination=DESTINATION]                    # Specify destination to be used for config file (staging -> deploy.staging.yml)
  -H, [--skip-hooks]                                 # Don't run hooks
                                                     # Default: false
```
Appending docs/commands/config.md
---
title: Config
---

# kamal config

Displays your config.

```bash
$ kamal config
---
:roles:
- web
:hosts:
- vm1
- vm2
:primary_host: vm1
:version: 505f4f60089b262c693885596fbd768a6ab663e9
:repository: registry:4443/app
:absolute_image: registry:4443/app:505f4f60089b262c693885596fbd768a6ab663e9
:service_with_version: app-505f4f60089b262c693885596fbd768a6ab663e9
:volume_args: []
:ssh_options:
  :user: root
  :port: 22
  :keepalive: true
  :keepalive_interval: 30
  :log_level: :fatal
:sshkit: {}
:builder:
  driver: docker
  arch: arm64
  args:
    COMMIT_SHA: 505f4f60089b262c693885596fbd768a6ab663e9
:accessories:
  busybox:
    service: custom-busybox
    image: registry:4443/busybox:1.36.0
    cmd: sh -c 'echo "Starting busybox..."; trap exit term; while true; do sleep 1;
      done'
    roles:
    - web
:logging:
- "--log-opt"
- max-size="10m"
```
Appending docs/commands/running-commands-on-servers.md
---
title: Running commands on servers
---

# Running commands on servers

You can use [aliases](../../configuration/aliases) for common commands.

## [Run command on all servers](#run-command-on-all-servers)

```bash
$ kamal app exec 'ruby -v'
App Host: 192.168.0.1
ruby 3.1.3p185 (2022-11-24 revision 1a6b16756e) [x86_64-linux]

App Host: 192.168.0.2
ruby 3.1.3p185 (2022-11-24 revision 1a6b16756e) [x86_64-linux]
```

## [Run command on primary server](#run-command-on-primary-server)

```bash
$ kamal app exec --primary 'cat .ruby-version'
App Host: 192.168.0.1
3.1.3
```

## [Run Rails command on all servers](#run-rails-command-on-all-servers)

```bash
$ kamal app exec 'bin/rails about'
App Host: 192.168.0.1
About your application's environment
Rails version             7.1.0.alpha
Ruby version              ruby 3.1.3p185 (2022-11-24 revision 1a6b16756e) [x86_64-linux]
RubyGems version          3.3.26
Rack version              2.2.5
Middleware                ActionDispatch::HostAuthorization, Rack::Sendfile, ActionDispatch::Static, ActionDispatch::Executor, Rack::Runtime, Rack::MethodOverride, ActionDispatch::RequestId, ActionDispatch::RemoteIp, Rails::Rack::Logger, ActionDispatch::ShowExceptions, ActionDispatch::DebugExceptions, ActionDispatch::Callbacks, ActionDispatch::Cookies, ActionDispatch::Session::CookieStore, ActionDispatch::Flash, ActionDispatch::ContentSecurityPolicy::Middleware, ActionDispatch::PermissionsPolicy::Middleware, Rack::Head, Rack::ConditionalGet, Rack::ETag, Rack::TempfileReaper
Application root          /rails
Environment               production
Database adapter          sqlite3
Database schema version   20221231233303

App Host: 192.168.0.2
About your application's environment
Rails version             7.1.0.alpha
Ruby version              ruby 3.1.3p185 (2022-11-24 revision 1a6b16756e) [x86_64-linux]
RubyGems version          3.3.26
Rack version              2.2.5
Middleware                ActionDispatch::HostAuthorization, Rack::Sendfile, ActionDispatch::Static, ActionDispatch::Executor, Rack::Runtime, Rack::MethodOverride, ActionDispatch::RequestId, ActionDispatch::RemoteIp, Rails::Rack::Logger, ActionDispatch::ShowExceptions, ActionDispatch::DebugExceptions, ActionDispatch::Callbacks, ActionDispatch::Cookies, ActionDispatch::Session::CookieStore, ActionDispatch::Flash, ActionDispatch::ContentSecurityPolicy::Middleware, ActionDispatch::PermissionsPolicy::Middleware, Rack::Head, Rack::ConditionalGet, Rack::ETag, Rack::TempfileReaper
Application root          /rails
Environment               production
Database adapter          sqlite3
Database schema version   20221231233303
```

## [Run Rails runner on primary server](#run-rails-runner-on-primary-server)

```bash
$ kamal app exec -p 'bin/rails runner "puts Rails.application.config.time_zone"'
UTC
```

## [Run interactive commands over SSH](#run-interactive-commands-over-ssh)

You can run interactive commands, like a Rails console or a bash session, on a server (default is primary, use `--hosts` to connect to another).

Start a bash session in a new container made from the most recent app image:

```bash
kamal app exec -i bash
```

Start a bash session in the currently running container for the app:

```bash
kamal app exec -i --reuse bash
```

Start a Rails console in a new container made from the most recent app image:

```bash
kamal app exec -i 'bin/rails console'
```
Appending docs/upgrading/overview.md
---
title: "Kamal 2: Upgrade Guide"
---

# Kamal 2: Upgrade Guide

There are some significant differences between Kamal 1 and Kamal 2.

- The Traefik proxy has been [replaced by kamal-proxy](../proxy-changes).
- Kamal will run all containers in a [custom Docker network](../network-changes).
- There are some backward-incompatible [configuration changes](../configuration-changes).
- How we pass secrets to containers [has changed](../secrets-changes).

If you want to continue using Traefik, you can run it as an accessory; see [here](../continuing-to-use-traefik) for more details.

## [Upgrade steps](#upgrade-steps)

### Upgrade to Kamal 1.9.x

If you are planning to do in-place upgrades of servers, you should first upgrade to Kamal 1.9, as it has support for downgrading.

If using the gem directly, you can run:

```bash
gem install kamal --version 1.9.0
```

Confirm you can deploy your application with Kamal 1.9.

### Upgrade to Kamal 2

If using the gem directly, run:

```bash
gem install kamal
```

### Make configuration changes

You'll need to [convert your configuration](../configuration-changes) to work with Kamal 2.

You can test whether the new configuration is valid by running:

```bash
$ kamal config
```

If you have multiple destinations, you should test each ones configuration:

```bash
$ kamal config -d staging
$ kamal config -d beta
```

### Move from .env to .kamal/secrets

Follow the steps [here](../secrets-changes).

### Verify container port

The default app port was [changed from 3000 to 80](https://kamal-deploy.org/docs/upgrading/configuration-changes/#traefik-to-proxy), you'll need to either specify your `app_port` or update your `EXPOSE` port if not using port 80.

## [In-place upgrades](#in-place-upgrades)

**Warning: Test this in a non-production environment first, if possible.**

### Upgrading

You can then upgrade with:

```
$ kamal upgrade [-d <DESTINATION>]
```

You'll need to do this separately for each destination.

The `kamal upgrade` command will:

1. Stop and remove the Traefik proxy.
2. Create a `kamal` Docker network if one doesn't exist.
3. Start a `kamal-proxy` container in the new network.
4. Reboot the currently deployed version of the app container in the new network.
5. Tell `kamal-proxy` to send traffic to it.
6. Reboot all accessories in the new network.

### Avoiding downtime

If you are running your application on multiple servers and want to avoid downtime, you can do a rolling upgrade:

```bash
$ kamal upgrade --rolling [-d <DESTINATION>]
```

This will follow the same steps as above, but host by host.

Alternatively, you can run the command host by host:

```bash
$ kamal upgrade -h 127.0.0.1[,127.0.0.2]
```

You could additionally use the [pre-proxy-reboot](../hooks/pre-proxy-reboot.md) and [post-proxy-reboot](../hooks/post-proxy-reboot.md) hooks to manually remove your server from upstream load balancers, to ensure no requests are dropped during the upgrade process.

### Downgrading

If you want to reverse your changes and go back to Kamal 1.9:

1. Uninstall Kamal 2.0.
2. Confirm you are running Kamal 1.9 by running `kamal version`.
3. Run the `kamal downgrade` command. It has the same options as `kamal upgrade` and will reverse the process.

The upgrade and downgrade commands can be re-run against servers that have already been upgraded or downgraded.
Appending docs/upgrading/configuration-changes.md
---
title: Configuration changes
---

# Kamal 2: Configuration changes

## [Builder](#builder)

The [builder configuration](../../configuration/builders) has been simplified.

### Arch

You must specify the architecture(s) you are building for:

```yaml
# single arch
builder:
  arch: amd64

# multi arch
builder:
  arch:
    - amd64
    - arm64
```

### Remote builders

Set the remote directly with the remote option. By default, it will only be used if the arch you are building doesn't match the local machine:

```yaml
builder:
  arch: amd64
  remote: ssh://docker@docker-builder
```

You can force Kamal to only use the remote builder, by setting `local: false`:

```yaml
builder:
  arch: amd64
  local: false
  remote: ssh://docker@docker-builder
```

### Driver

Kamal will now always use the Docker container (link) driver by default. You can set the driver yourself to change this:

```yaml
builder:
  driver: docker
```

The Docker driver has limited capabilities — it doesn't support build caching or multiarch images.

## [Traefik &rarr; Proxy](#traefik-to-proxy)

The `traefik` configuration is no longer valid. Instead, you can configure kamal-proxy under [proxy](../../configuration/proxy).

If you were using custom Traefik labels or args, see the proxy configuration to determine whether you can convert them.

Be aware that by default kamal-proxy forwards traffic to the container port 80, this is because we assume your container is running Thruster, and it listens on the port 80. If you are running a different service or port, you can configure the app_port setting:

```yaml
proxy:
  app_port: 3000
```

kamal-proxy supports common requirements such as buffering, max request/response sizes, and forwarding headers, but it does not encompass the full breadth of everything Traefik can do.

If you don't see something you need, you can raise an issue and we'll look into it, but we don't promise to support everything — you might need to run Traefik or another proxy elsewhere in your stack to achieve what you want.

## [Healthchecks](#healthchecks)

The healthcheck section has been removed.

### Proxy roles

For roles running with a proxy, the healthchecks are performed externally by kamal-proxy, not via internal Docker healthchecks. You can configure them under [proxy/healthcheck](../../configuration/proxy#healthcheck).

```yaml
proxy:
  healthcheck:
    path: /health
    interval: 2
    timeout: 2
```

Please note that the healthchecks will use the `app_port` setting, which defaults to port 80. Previously, healthchecks defaulted to port 3000. You can change this back with:

```yaml
proxy:
  app_port: 3000
```

### Non-proxy roles

For roles that do not run the proxy, you can set a custom Docker healthcheck via the [options](../../configuration/roles#custom-role-configuration).

```yaml
servers:
  web:
    ...
  jobs:
    options:
      health-cmd: bin/jobs-healthy
```

For those containers, Kamal will wait for the `healthy` status if they have a healthcheck or `running` if they don't.

You can set a `readiness_delay`, which is used when we see the `running` status. We'll wait that long and confirm the container is still running before continuing.

### All roles

There are two timeouts you can set at the root of the config that are used across all roles, whether they use a proxy or not.

```yaml
# how long to wait for new containers to boot
deploy_timeout: 20

# how long to wait for requests to complete before stopping old containers
# Replaces stop_wait_time
drain_timeout: 20

# how long to wait for 'non-proxy role' containers without healthchecks to stay in the running state
readiness_delay: 10
```
Appending docs/upgrading/secrets-changes.md
---
title: Secrets changes
---

# Kamal 2: Secrets changes

Secrets have moved from `.env`/`.env.rb` to `.kamal/secrets`.

If you are using destinations, secrets will be read from `.kamal/secrets.<DESTINATION>` first or `.kamal/secrets-common` if it is not found.

## [Interpolating secrets](#interpolating-secrets)

The `kamal envify` and `kamal env` commands have been removed, and secrets no longer have a separate lifecycle.

If you were generating secrets with `kamal envify`, you can instead use dotenv's [command](https://github.com/bkeepers/dotenv?tab=readme-ov-file#command-substitution) and [variable](https://github.com/bkeepers/dotenv?tab=readme-ov-file#variable-substitution) substitution.

The substitution will be performed on demand when running Kamal commands that need them.

```
# .kamal/secrets

SECRET_FROM_ENV=$SECRET_FROM_ENV
SECRET_FROM_COMMAND=$(op read ...)
```

See [here](../../configuration/environment-variables#using-kamal-secrets) for more details.

## [Environment variables in deploy.yml](#environment-variables-in-deployyml)

In Kamal 1, `.env` was loaded into the environment, so you could refer to values from it via ERB in `deploy.yml`. This is no longer the case in Kamal 2. Values from `.kamal/secrets` are not loaded either.

Kamal 1:

```
# .env
SERVER_IP=127.0.0.1

# config/deploy.yml
servers
  - <%= ENV["SERVER_IP"] %>
```

To make this work in Kamal 2, you can manually load `.env`.

Kamal 2:

```
# .env
SERVER_IP=127.0.0.1

# config/deploy.yml

<% require "dotenv"; Dotenv.load(".env") %>

servers
  - <%= ENV["SERVER_IP"] %>
```
Appending docs/upgrading/continuing-to-use-traefik.md
---
title: "Kamal 2: Continuing to use Traefik"
---

# Kamal 2: Continuing to use Traefik

Kamal 2 requires kamal-proxy, but it's possible to continue to use Traefik if required.

You can run it as a Kamal accessory, and route requests through it and then on to kamal-proxy.

## Set the kamal-proxy boot config

We'll need to change kamal-proxy's default boot config so that:

1. It doesn't publish ports on the host.
2. It adds the labels Traefik needs to route requests to it.

Add a [pre-deploy hook](../../hooks/pre-deploy) for Traefik to pick up:

```shell
#!/bin/sh
kamal proxy boot_config set \
  --publish false \
  --docker_options label=traefik.http.services.kamal_proxy.loadbalancer.server.scheme=http \
                   label=traefik.http.routers.kamal_proxy.rule=PathPrefix\(\`/\`\)
```

You can add the `kamal proxy boot_config set` command to a [pre-deploy hook](../../hooks/pre-deploy). This will ensure that it is set correctly when deploying to a host for the first time.

## Create the accessory

Add Traefik as an accessory to `config/deploy.yml`, binding to the host port.

```yaml
accessories:
  traefik:
    service: traefik
    image: traefik:v2.10
    port: 80
    cmd: "--providers.docker"
    options:
      volume:
        - "/var/run/docker.sock:/var/run/docker.sock"
    roles:
      - web
```

## Running with Traefik

When you call `kamal setup`, it will boot the Traefik accessory, call the pre-deploy hook to update kamal-proxy's boot config, and then boot kamal-proxy and the app.

Requests will flow from Traefik to kamal-proxy to your app.

```
$ docker ps
CONTAINER ID   IMAGE                                                                     COMMAND                  CREATED              STATUS              PORTS                               NAMES
3729c50d9d94   registry:4443/app_with_traefik:30482914d55f9ca5e4302dd2d050e424d29d8f74   "/docker-entrypoint.…"   11 seconds ago       Up 10 seconds       80/tcp                              app_with_traefik-web-30482914d55f9ca5e4302dd2d050e424d29d8f74
3c87e1c649e3   basecamp/kamal-proxy:v0.4.0                                               "kamal-proxy run"        12 seconds ago       Up 11 seconds       80/tcp, 443/tcp                     kamal-proxy
609a18d8ecd7   traefik:v2.10                                                             "/entrypoint.sh --pr…"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp, :::80->80/tcp   traefik
```

## Switching on a host already running kamal-proxy

If you are already running kamal-proxy, you'll need to:

1. Manually run the `kamal proxy boot_config set` command from the deploy hook.
2. Run `kamal proxy reboot` to pick up those boot config changes.
3. Run `kamal accessory boot traefik` to start Traefik.
Appending docs/upgrading/proxy-changes.md
---
title: Proxy Changes
---

# Kamal 2: Proxy Changes

In Kamal 1, we used [Traefik](https://traefik.io/traefik) to enable gapless deployments.

For version 2, we are using [kamal-proxy](https://github.com/basecamp/kamal-proxy), a custom proxy built specifically for Kamal.

## [Why we are dropping Traefik](#dropping-traefik)

### Imperative vs. Declarative

Traefik is not a great fit for Kamal. Kamal is an imperative tool, while Traefik is declarative.

This means that we need to ask Traefik to do things, and then poll it to see when it's done.

### Labels

We used Traefik's Docker provider. It requires adding labels to containers, which Traefik uses to configure itself.

It is flexible, and there are a lot of options for things you can do with the labels. But it requires that you understand and use Traefik's [general purpose configuration](https://doc.traefik.io/traefik/providers/docker/) even to accomplish simple tasks.

Container labels are immutable, so you can't tell Traefik to stop routing requests. To successfully drain containers, we had to resort to modifying health checks to force the containers into an unhealthy state.

### Overly flexible

Using Traefik labels, it is possible to get Kamal to do things it was not initially designed to do, like integrating Let's Encrypt or running multiple applications on one server.

These use cases were unsupported and error-prone, though, and we wanted to provide simpler built solutions for those common requirements.

### Hard to understand errors

Traefik has its own domain language — Routers, Services, EntryPoints. So if it failed, the errors would be in that language and disconnected from what Kamal was doing. This made it tricky to diagnose failures.

### Other options

There are other proxies available, and Traefik has other configuration options, such as the file provider.

However, to evolve Kamal, it became clear that building our own proxy would give us the control we needed to efficiently build and develop new features.

We wanted:

- An imperative proxy — i.e., no polling
- A 1-to-1 mapping between Kamal commands and proxy commands
- Clear error messages
- Support for new commands
- Deploy-time rather than boot-time config, so we can change it without restarting

It was clear that to achieve this, we'd need to build the proxy ourselves.

## [kamal-proxy](#kamal-proxy)

[kamal-proxy](https://github.com/basecamp/kamal-proxy) is written in Go.

It has minimal configuration so that we can run multiple applications against a single proxy without configuration clashes.

Configuration (timeouts, logging, buffering, etc.) is supplied via commands at deploy time and only applies to the application being deployed.

It uses blocking commands, so when you deploy, the command will respond when the deployment is complete.

It has support for:

- Automatic TLS via Let's Encrypt
- Host-based routing
- Request and response buffering
- Maximum request and response sizes

And coming soon to Kamal:

- Pausing requests
- Maintenance mode
- Gradual rollouts

The proxy is distributed as a container via [Docker Hub](https://hub.docker.com/repository/docker/basecamp/kamal-proxy).

Kamal will ensure that a compatible version is in place before deploying.
Appending docs/upgrading/network-changes.md
---
title: Network changes
---

# Kamal 2: Network changes

`kamal-proxy` needs a stable hostname for the container that it is routing to. This is so that it can identify and route traffic to the container across restarts.

Using the default `bridge` network, application containers are assigned IP addresses, but they are not stable across restarts.

So instead, we will create and use a custom network called `kamal`.

If you have custom requirements for your network, you can create the `kamal` network yourself before deploying with Kamal, or use a `docker-setup` hook to configure the network when running `kamal setup`.

Accessories will also run from within the `kamal` network.
