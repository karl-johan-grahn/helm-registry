# Private Helm Registry
This repository is a private Helm chart repository to host charts,
and host and share Helm packages.

## Helm Charts
The charts are located in the [charts](charts) folder.

## Build
The charts are linted and tested automatically via [GitHub workflow](.github/workflows/lint_test.yml)
as part of changes to charts via pull requests.

## Release
When a chart is updated on `master`, the `index.yaml` file
will automatically be updated via [GitHub workflow](.github/workflows/release.yml)
on the `gh-pages` branch.

## Registry
Everything needed to host a Helm registry is the ability to host the `index.yaml`
file and the `.tgz` package files.

For this assignment, `GitHub` will be used as a private charts repository to
avoid the need to manage a separate web server. Files will be accessed
as if GitHub was a simple HTTP server hosting raw files. Github provides
this feature via `raw.githubusercontent.com`. In order for Helm to be able
to pull files from such repository we need to provide it a personal access token.

Add a [robots.txt](robots.txt) file to stop bots from visiting the site and
stop search engines from ranking it, in case the registry will be made public.

## Future Work
For a production environment, consider the following:
* Can an existing Helm registry be used?
* Which Kubernetes versions should be supported?
    * To ensure backwards compatibility:
        * Run `kubeval` chart across all versions
        * Install chart on `kind` cluster across all versions
* Customize liveness and readiness endpoints:
    * When creating a chart, Helm points the `livenessProbe` and `readinessProbe`
    to `/` by default in `deployment.yaml`
    * Implement a proper `/live` and `/ready` endpoint in the service and
    parametrize the readiness probes to use those custom endpoints instead