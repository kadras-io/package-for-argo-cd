# Argo CD

![Test Workflow](https://github.com/kadras-io/package-for-argo-cd/actions/workflows/test.yml/badge.svg)
![Release Workflow](https://github.com/kadras-io/package-for-argo-cd/actions/workflows/release.yml/badge.svg)
[![The SLSA Level 3 badge](https://slsa.dev/images/gh-badge-level3.svg)](https://slsa.dev/spec/v1.0/levels)
[![The Apache 2.0 license badge](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Follow us on Twitter](https://img.shields.io/static/v1?label=Twitter&message=Follow&color=1DA1F2)](https://twitter.com/kadrasIO)

> **Warning**
> This package has been archived. For GitOps features, you can choose between Carvel and [Flux](https://github.com/kadras-io/package-for-flux), supported out-of-the-box by Kadras, or bring your own Argo CD package.

A Carvel package for [Argo CD](https://argoproj.github.io/cd), a declarative and GitOps continuous delivery tool for Kubernetes.

## 🚀&nbsp; Getting Started

### Prerequisites

* Kubernetes 1.27+
* Carvel [`kctrl`](https://carvel.dev/kapp-controller/docs/latest/install/#installing-kapp-controller-cli-kctrl) CLI.
* Carvel [kapp-controller](https://carvel.dev/kapp-controller) deployed in your Kubernetes cluster. You can install it with Carvel [`kapp`](https://carvel.dev/kapp/docs/latest/install) (recommended choice) or `kubectl`.

  ```shell
  kapp deploy -a kapp-controller -y \
    -f https://github.com/carvel-dev/kapp-controller/releases/latest/download/release.yml
  ```

### Installation

Add the Kadras [package repository](https://github.com/kadras-io/kadras-packages) to your Kubernetes cluster:

  ```shell
  kctrl package repository add -r kadras-packages \
    --url ghcr.io/kadras-io/kadras-packages \
    -n kadras-packages --create-namespace
  ```

<details><summary>Installation without package repository</summary>
The recommended way of installing the Argo CD package is via the Kadras <a href="https://github.com/kadras-io/kadras-packages">package repository</a>. If you prefer not using the repository, you can add the package definition directly using <a href="https://carvel.dev/kapp/docs/latest/install"><code>kapp</code></a> or <code>kubectl</code>.

  ```shell
  kubectl create namespace kadras-packages
  kapp deploy -a argo-cd-package -n kadras-packages -y \
    -f https://github.com/kadras-io/package-for-argo-cd/releases/latest/download/metadata.yml \
    -f https://github.com/kadras-io/package-for-argo-cd/releases/latest/download/package.yml
  ```
</details>

Install the Argo CD package:

  ```shell
  kctrl package install -i argo-cd \
    -p argo-cd.packages.kadras.io \
    -v ${VERSION} \
    -n kadras-packages
  ```

> **Note**
> You can find the `${VERSION}` value by retrieving the list of package versions available in the Kadras package repository installed on your cluster.
> 
>   ```shell
>   kctrl package available list -p argo-cd.packages.kadras.io -n kadras-packages
>   ```

Verify the installed packages and their status:

  ```shell
  kctrl package installed list -n kadras-packages
  ```

## 📙&nbsp; Documentation

Documentation, tutorials and examples for this package are available in the [docs](docs) folder.
For documentation specific to Argo CD, check out [argoproj.github.io/cd](https://argoproj.github.io/cd).

## 🎯&nbsp; Configuration

The Argo CD package can be customized via a `values.yml` file.

  ```yaml
  service:
    type: LoadBalancer
  ```

Reference the `values.yml` file from the `kctrl` command when installing or upgrading the package.

  ```shell
  kctrl package install -i argo-cd \
    -p argo-cd.packages.kadras.io \
    -v ${VERSION} \
    -n kadras-packages \
    --values-file values.yml
  ```

### Values

The Argo CD package has the following configurable properties.

<details><summary>Configurable properties</summary>

| Config | Default | Description |
|-------|-------------------|-------------|
| `namespace` | `argocd` | The namespace where to install Argo CD. |
| `service.type` | `ClusterIP` | The type of Kubernetes Service to use for the Argo CD Server. Valid values are `LoadBalancer`, `NodePort`, and `ClusterIP`. |

</details>

## 🛡️&nbsp; Security

The security process for reporting vulnerabilities is described in [SECURITY.md](SECURITY.md).

## 🖊️&nbsp; License

This project is licensed under the **Apache License 2.0**. See [LICENSE](LICENSE) for more information.
