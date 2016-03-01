route-registrar
===============

A standalone executable written in golang that continuously broadcasts a route using NATS to the CF router.

This uses [nats-io/nats](https://github.com/nats-io/nats) for connecting to the NATS bus.

## Usage

### Installation
1. Run the following command to install route-registrar
  ```
  go get github.com/cloudfoundry-incubator/route-registrar
  ```

1. The route-registrar expects a configuration YAML file like the one below:
  ```yaml
  message_bus_servers:
  - host: REPLACE_WITH_NATS_URL
    user: REPLACE_WITH_NATS_USERNAME
    password: REPLACE_WITH_NATS_PASSWORD
  host: HOSTNAME_OR_IP_OF_ROUTE_DESTINATION
  routes:
  - name: SOME_ROUTE_NAME
    port: REPLACE_WITH_VM_PORT
    tags:
      optional_tag_field: some_tag_value
      another_tag_field: some_other_value
    uris:
    - some_uri_for_the_router_should_listen_on
    - some_other_uri_for_the_router_to_listen_on
    registration_interval: REGISTRATION_INTERVAL
    health_check: # optional
      name: my-healthcheck
      script_path: /path/to/check/executable
      timeout: 1 # optional
  ```
  - routes `name` must be provided and be a string
  - routes `port` must be provided and must be a positive integer > 1
  - routes `uris` must be provided and be a non empty array of strings
  - `registration_interval` must be provided and be a string with units (e.g. "20s").
  It must parse to a positive time duration i.e. "-5s" is not permitted.
  - `health_check.timeout` is optional; if it not provided it defaults to half
  of the value of `registration_interval`
  If provided, it must be a string with units (e.g. "10s") and be less than
  the registration interval.
  It must parse to a positive time duration i.e. "-5s" is not permitted.

1. Run route-registrar binaries using the following command
  ```
  ./bin/route-registrar -configPath=FILE_PATH_TO_CONFIG_YML --pidFile=PATH_TO_PIDFILE
  ```

### Health check

If the `health_check` is not configured for a route, the route is continually
registered.

If the `health_check` is configured, the executable is invoked and the following applies:
- if the executable exits with success, the route is registered.
- if the executable exits with error, the route is unregistered.
- if a timeout is configured, the executable must exit within the timeout,
  otherwise it is terminated (with `SIGKILL`) and the route is unregistered.

### BOSH release

As this is a job and a package in the [cf-release](https://github.com/cloudfoundry/cf-release)
BOSH release, it can be colocated with the following manifest changes:

```yaml
releases:
- name: cf
- name: my-release

jobs:
- name: myJob
  templates:
  - name: my-job
    release: my-release
  - name: route_registrar
    release: cf
  properties:
    route_registrar:
      # ...
      # [ see bosh job spec ]

```

## Development

### Dependencies

Dependencies are saved using [Godep](https://github.com/tools/godep) with `godep save -r` (import path re-writing).
Just clone the repo to your `GOPATH` and the dependencies should be available.

### Running tests

Tests are triggered on new commits to master by our
[concourse](https://runtime.ci.cf-app.com/pipelines/route-registrar).

1. Install the ginkgo binary with `go get`:
  ```
  go get github.com/onsi/ginkgo/ginkgo
  ```

1. Run tests, by running the following command from root of this repository
  ```
  bin/test
  ```
