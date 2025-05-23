# route-registrar

> [!CAUTION]
> This repository has been in-lined (using git-subtree) into routing-release. Please make any
> future contributions directly to routing-release.

[![Go Report
Card](https://goreportcard.com/badge/code.cloudfoundry.org/route-registrar)](https://goreportcard.com/report/code.cloudfoundry.org/route-registrar)
[![Go
Reference](https://pkg.go.dev/badge/code.cloudfoundry.org/route-registrar.svg)](https://pkg.go.dev/code.cloudfoundry.org/route-registrar)

A standalone executable written in golang that continuously broadcasts a
routes to the [gorouter](https://github.com/cloudfoundry/gorouter). This
is designed to be a general purpose solution, packaged as a BOSH job to
be colocated with components that need to broadcast their routes to the
gorouter, so that those components don't need to maintain logic for
route registration.

> \[!NOTE\]
>
> This repository should be imported as
> `code.cloudfoundry.org/route-registrar`.

# Docs

-   [Usage](./docs/01-usage.md)

# Contributing

See the [Contributing.md](./.github/CONTRIBUTING.md) for more
information on how to contribute.

# Working Group Charter

This repository is maintained by [App Runtime
Platform](https://github.com/cloudfoundry/community/blob/main/toc/working-groups/app-runtime-platform.md)
under `Networking` area.

> \[!IMPORTANT\]
>
> Content in this file is managed by the [CI task
> `sync-readme`](https://github.com/cloudfoundry/wg-app-platform-runtime-ci/blob/main/shared/tasks/sync-readme/metadata.yml)
> and is generated by CI following a convention.
