# Argo CD

This project provides a [Carvel package](https://carvel.dev/kapp-controller/docs/latest/packaging) for [Argo CD](https://argoproj.github.io/cd), a declarative and GitOps continuous delivery tool for Kubernetes.

## Components

* Argo CD

## Prerequisites

* Kubernetes 1.24+
* Carvel [`kctrl`](https://carvel.dev/kapp-controller/docs/latest/install/#installing-kapp-controller-cli-kctrl) CLI.
* Carvel [kapp-controller](https://carvel.dev/kapp-controller) deployed in your Kubernetes cluster. You can install it with Carvel [`kapp`](https://carvel.dev/kapp/docs/latest/install) (recommended choice) or `kubectl`.

  ```shell
  kapp deploy -a kapp-controller -y \
    -f https://github.com/vmware-tanzu/carvel-kapp-controller/releases/latest/download/release.yml
  ```

## Installation

First, add the [Kadras package repository](https://github.com/arktonix/kadras-packages) to your Kubernetes cluster.

  ```shell
  kubectl create namespace kadras-packages
  kctrl package repository add -r kadras-repo \
    --url ghcr.io/arktonix/kadras-packages \
    -n kadras-packages
  ```

Then, install the Argo CD package.

  ```shell
  kctrl package install -i argo-cd \
    -p argo-cd.packages.kadras.io \
    -v 2.5.2 \
    -n kadras-packages
  ```

You can verify the list of installed packages and their status.

  ```shell
  kctrl package installed list -n kadras-packages
  ```

You can also get the list of versions available in the Kadras package repository for Argo CD.

  ```shell
  kctrl package available list -p argo-cd.packages.kadras.io -n kadras-packages
  ```

## Configuration

The Argo CD package has the following configurable properties.

| Config | Default | Description |
|-------|-------------------|-------------|
| `namespace` | `argocd` | The namespace where to install Argo CD. |
| `service.type` | `ClusterIP` | The type of Kubernetes Service to use for the Argo CD Server. Valid values are `LoadBalancer`, `NodePort`, and `ClusterIP`. |

You can define your configuration in a `values.yml` file.

  ```yaml
  namespace: argocd
  service:
    type: ClusterIP
  ```

Then, pass the file when installing the package.

  ```shell
  kctrl package install -i argo-cd \
    -p argo-cd.packages.kadras.io \
    -v 2.5.2 \
    -n kadras-packages \
    --values-file values.yml
  ```

## Upgrading

You can upgrade an existing package to a newer version using `kctrl`.

  ```shell
  kctrl package installed update -i argo-cd \
    -v <new-version> \
    -n kadras-packages
  ```

You can also update an existing package with a newer `values.yml` file.

  ```shell
  kctrl package installed update -i argo-cd \
    -n kadras-packages \
    --values-file values.yml
  ```

## Other

The recommended way of installing the Argo CD package is via the [Kadras package repository](https://github.com/arktonix/kadras-packages). If you prefer not using the repository, you can install the package by creating the necessary Carvel `PackageMetadata` and `Package` resources directly
using [`kapp`](https://carvel.dev/kapp/docs/latest/install) or `kubectl`.

  ```shell
  kubectl create namespace kadras-packages
  kapp deploy -a argo-cd-package -n kadras-packages -y \
    -f https://github.com/arktonix/package-for-argo-cd/releases/latest/download/metadata.yml \
    -f https://github.com/arktonix/package-for-argo-cd/releases/latest/download/package.yml
  ```

## Support and Documentation

For support and documentation specific to Argo CD, check out [argoproj.github.io/cd](https://argoproj.github.io/cd).

## Supply Chain Security

This project is compliant with level 2 of the [SLSA Framework](https://slsa.dev).

<img src="https://slsa.dev/images/SLSA-Badge-full-level2.svg" alt="The SLSA Level 2 badge" width=200>
