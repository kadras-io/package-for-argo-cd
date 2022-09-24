# Argo CD

This project provides a [Carvel package](https://carvel.dev/kapp-controller/docs/latest/packaging) for [Argo CD](https://argoproj.github.io/cd), a declarative and GitOps continuous delivery tool for Kubernetes.

## Components

* argo-cd

## Configuration

The Argo CD package has the following configurable properties.

| Value | Required/Optional | Description |
|-------|-------------------|-------------|
| `namespace` | Optional | The namespace where to install Argo CD. By default, it's `argocd`. |
| `service.type` | Optional | The type of Kubernetes service to provision for the Argo CD Server. Valid values are `LoadBalancer`, `NodePort`, and `ClusterIP`. By default, it's `ClusterIP`. |

```yaml
namespace: argocd
service:
  type: ClusterIP
```

## Installation

TBD

## Documentation

For documentation specific to Argo CD, check out [https://argoproj.github.io/cd](https://argoproj.github.io/cd).

## Supply Chain Security

This project is compliant with level 2 of the [SLSA Framework](https://slsa.dev).

<img src="https://slsa.dev/images/SLSA-Badge-full-level2.svg" alt="The SLSA Level 2 badge" width=200>
