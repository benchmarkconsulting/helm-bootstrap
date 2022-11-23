# Platform Tenant Provisioning - Helm Charts

This repository contains packaged Helm charts provided by NGINX:

- Cluster RBAC
- Tenant Provisioning 

## Cluster RBAC

### Documentation

https://github.com/manulife-innersource/aks-multitenant-pattern/blob/develop/charts/cluster-rbac/README.md

### Add Repository (stable)

```sh
$ helm repo add <place holder> <place holder>
$ helm repo update
```

### Install Packages (stable)

```sh
$ helm install my-release <place holder>
```

## Tenant Provisioning

### Documentation

https://github.com/manulife-innersource/aks-multitenant-pattern/blob/develop/charts/tenant-provision/README.md

### Add repository
```sh
$ helm repo add <place holder> <place holder>
$ helm repo update
```

### Install packages
```sh
$ helm install my-release <place holder>
```

# Install via Flux

## Setup Repos
```yaml
clusterrbac-repo.yaml
apiVersion: source.toolkit.fluxcd.io/v1beta2
kind: HelmRepository
metadata:
  name: cluster-rbac
  namespace: flux-system
spec:
  interval: 5m
  url: <place holder>
```

```yaml
tenant-repo.yaml
apiVersion: source.toolkit.fluxcd.io/v1beta2
kind: HelmRepository
metadata:
  name: tenant-provision
  namespace: flux-system
spec:
  interval: 5m
  url: <place holder>
```
## Setup Releases

```yaml
apiVersion: helm.toolkit.fluxcd.io/v2beta1
kind: HelmRelease
metadata:
  name: tenant-provision
  namespace: flux-system
spec:
  releaseName: tenant-provision
  chart:
    spec:
      chart: tenant-provision
      sourceRef:
        kind: HelmRepository
        name: tenant-provision
        namespace: flux-system
      version: 0.1.20
  interval: 5m0s
```

```yaml
apiVersion: helm.toolkit.fluxcd.io/v2beta1
kind: HelmRelease
metadata:
  name: cluster-rbac
  namespace: flux-system
spec:
  releaseName: cluster-rbac
  chart:
    spec:
      chart: cluster-rbac
      sourceRef:
        kind: HelmRepository
        name: cluster-rbac
        namespace: flux-system
      version: 0.1.5
  interval: 1h0m0s
  install:
    remediation:
      retries: 3
```
