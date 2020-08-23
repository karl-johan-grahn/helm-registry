# Helm Registry
This repository is a Helm chart repository to host charts,
and host and share Helm packages.

Everything needed to host a Helm registry is the ability to host the `index.yaml`
file and the `.tgz` package files.

For this assignment, `GitHub` will be used as a charts repository to
avoid the need to manage a separate web server. Files will be accessed
as if GitHub was a simple HTTP server hosting raw files. Github provides
this feature via `raw.githubusercontent.com`. In order for Helm to be able
to pull files from such repository we need to provide it a personal access token.

Add a [robots.txt](robots.txt) file to stop bots from visiting the site and
stop search engines from ranking it.

## Helm Charts
The charts are located in the [charts](charts) folder.

## Build
The charts are linted and tested automatically via [GitHub workflow](.github/workflows/lint_test.yml)
as part of changes to charts via pull requests.

## Release
When a chart is updated on `master`, the `index.yaml` file
will automatically be updated via [GitHub workflow](.github/workflows/release.yml)
on the [gh-pages branch](https://github.com/karl-johan-grahn/helm-registry/blob/gh-pages/index.yaml).

## Deploy
When a cluster is provisioned, store the cluster authentication information for
kubectl in the `KUBECONFIG` secret and deploy the charts with the
[Deploy](.github/workflows/deploy.yml) workflow.

## Future Work
For a production environment, consider the following:
* Can an existing Helm registry be used?
* Use `Ingress` instead of `NodePort` to expose service
* Validate the output from Helm against schemas generated from the
Kubernetes OpenAPI specification using [kubeval](https://github.com/instrumenta/kubeval)
* Which Kubernetes versions should be supported and regression tested against?
* Customize liveness and readiness endpoints:
    * When creating a chart, Helm points the `livenessProbe` and `readinessProbe`
    to `/` by default in `deployment.yaml`
    * Implement a proper `/live` and `/ready` endpoint in the service and
    parametrize the readiness probes to use those custom endpoints instead
* Helm does not seem to support installing charts from private GitHub repositores,
so the repository has to be public for now:
https://github.com/helm/helm/issues/8392