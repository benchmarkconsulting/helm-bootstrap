# Cluster RBAC Helm Chart

This is a Helm chart for initialization and maintenance of creating RBAC for Cluster Roles.

## Contents

- [Cluster RBAC Porvision Helm Chart](#cluster-rbac-helm-chart)
  - [Contents](#contents)
  - [Prerequisites](#prerequisites)
    - [Container Registry Credentials](#container-registry-credentials)
  - [Installing the Charts](#installing-the-charts)
  - [Configurable Variables](#configurable-variables)
    - [Cluster RBAC](#cluster-rbac)
  - [Issues and feedback](#issues-and-feedback)

## Prerequisites

### Container Registry Credentials

## Installing the Charts

### Flux

```yaml
apiVersion: source.toolkit.fluxcd.io/v1beta2
kind: HelmRepository
metadata:
  name: global-artifactory
  namespace: flux-system
spec:
  interval: 5m
  url: https://artifactory.platform.manulife.io/artifactory/helm
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
        name: global-artifactory
        namespace: flux-system
      version: 0.1.5
  interval: 1h0m0s
  install:
    remediation:
      retries: 3
  values:
    clusterAuditRole:
      enable: true
      annotations: {}
      name: "cluster-audit"
      groupObjectId: "5f20f209-7ae5-455f-bbd4-a3b8520771cd"
    clusterReaderRole:
      enable: true
      annotations: {}
      name: "cluster-reader"
      groupObjectId: "bfc98300-bc8c-4df5-803e-ef0d67f2a3bc"
```

## Configurable Variables

### ClusterAudit RBAC Role

| Parameter                         | Description                          | Default                                                                      |
| --------------------------------- | ------------------------------------ | ---------------------------------------------------------------------------- |
| `clusterAuditRole.enable`               | Whether to create the Role and Role binding for tenantAdminRole.    | `false`                                                                 |
| `clusterAuditRole.name`               | The group object ID from AZAD for the tenantAdminRole group.  | `N/A`                                                                   |
| `clusterAuditRole.groupObjectId`               | Whether to create the Role and Role binding for tenantAuditRole.    | `false`                                                                   |



### ClusterReader RBAC Role

| Parameter                         | Description                          | Default                                                                      |
| --------------------------------- | ------------------------------------ | ---------------------------------------------------------------------------- |
| `clusterReaderRole.enable`               | Whether to create the Role and Role binding for tenantAdminRole.    | `false`                                                                 |
| `clusterReaderRole.name`               | The group object ID from AZAD for the tenantAdminRole group.  | `N/A`                                                                   |
| `clusterReaderRole.groupObjectId`               | Whether to create the Role and Role binding for tenantAuditRole.    | `false`                                                                   |                                                                  |


## Issues and feedback

If you encounter any problems or would like to give us feedback on deployments, raise issues here on GitHub.
