# Tenant Creation Helm Chart

This is a Helm chart for initialization and maintenance of creating a tenant.

## Contents

- [Tenant Porvision Helm Chart](#tenant-creation-helm-chart)
  - [Contents](#contents)
  - [Prerequisites](#prerequisites)
    - [Container Registry Credentials](#container-registry-credentials)
  - [Installing the Charts](#installing-the-charts)
  - [Configurable Variables](#configurable-variables)
    - [Tenant RBAC](#tenant-rbac)
    - [Tenant Secrets](#tenant-secrets)
    - [Tenant Quotas Counts](#tenant-quotas-counts)
    - [Tenant Resource Quotas](#tenant-resource-quotas)
    - [Tenant Kyverno Policy](#tenant-kyverno-policy)
    - [Tenant Deployment Service Account](#tenant-deployment-service-account)
    - [Flux Config](#flux-config)
  - [Issues and feedback](#issues-and-feedback)

## Prerequisites
Group Created in Azure Active Directory

### Container Registry Credentials

## Installing the Chart
helm add repo
helm install 

## Installing the Charts using Flux
```yaml
apiVersion: source.toolkit.fluxcd.io/v1beta2
kind: HelmRepository
metadata:
  name: tenant1
  namespace: flux-system
spec:
  interval: 5m
  url: https://benchmarkconsulting.github.io/helm-bootstrap
```

```yaml
apiVersion: helm.toolkit.fluxcd.io/v2beta1
kind: HelmRelease
metadata:
  name: tenant1
  namespace: flux-system
spec:
  releaseName: tenant1
  chart:
    spec:
      chart: tenant-provision
      sourceRef:
        kind: HelmRepository
        name: tenant1
        namespace: flux-system
      version: 0.1.0
  interval: 1m0s
  values:
    tenants:
      tenant1a:
        rbac:
          tenantAdminRole:
            groupObjectId: "8783d9bf-3730-4371-aeb7-817e10dfc794"
          tenantAuditRole:
            groupObjectId: "f7ee8319-03f2-4656-9eb7-7196df622509"
          tenantDevRole:
            groupObjectId: "79757788-82b9-4442-a076-858cee86d1e8"
          tenantReaderRole:
            groupObjectId: "2bfabfba-8efb-4e6e-9f7c-9253fcc7a22e"
```
## Configurable Variables

### Tenant RBAC

| Parameter                         | Description                          | Default                                                                      |
| --------------------------------- | ------------------------------------ | ---------------------------------------------------------------------------- |
| `tenantAdminRole.enable`               | Whether to create the Role and Role binding for tenantAdminRole.    | `true`                                                                 |
| `tenantAdminRole.groupObjectId`               | The group object ID from AZAD for the tenantAdminRole group.  | `N/A`                                                                   |
| `tenantAuditRole.enable`               | Whether to create the Role and Role binding for tenantAuditRole.    | `true`                                                                   |
| `tenantAuditRole.groupObjectId`               | The group object ID from AZAD for the tenantAuditRole group.    | `N/A`                                                                   |
| `tenantReaderRole.enable`                          | Whether to create the Role and Role binding for tenantReaderRole.   | `true`                                                     |
| `tenantReaderRole.groupObjectId`                          |  The group object ID from AZAD for the tenantReaderRole group.    | `N/A`                                                  |
| `tenantDevRole.enable`                            | Whether to create the Role and Role binding for tenantDevRole.    | `true`                                                     |
| `tenantDevRole.groupObjectId`                              |  The group object ID from AZAD for the tenantDevRole group.  | `N/A`                                                  |

```yaml
tenants:
  team-test1:
    rbac:
      tenantAdminRole: 
        groupObjectId: "ObjectId"
      tenantAuditRole: 
        groupObjectId: "ObjectId"
      tenantDevRole: 
        groupObjectId: "ObjectId"
      tenantReaderRole: 
        groupObjectId: "ObjectId"
```
### Tenant Secrets

| Parameter                         | Description                          | Default                                                                      |
| --------------------------------- | ------------------------------------ | ---------------------------------------------------------------------------- |
| `akv2k8sSecret.enabled`               | Whether to create the akv2k8sSecret.    | `false`                                                                 |


### Tenant Quotas Counts

| Parameter                         | Description                          | Default                                                                      |
| --------------------------------- | ------------------------------------ | ---------------------------------------------------------------------------- |
| `quotaCounts.enabled`               | Set if to create new pull image secret    | `false`                                                                 |
| `quotaCounts.configmaps`               | Your Docker pull image secret name    | `100`                                                                   |
| `quotaCounts.persistentvolumeclaims`               | Your Docker registry (DockerHub, etc.) username    | 100`                                                                   |
| `quotaCounts.pods`               | Your Docker registry (DockerHub, etc.) password    | `100`                                                                   |
| `quotaCounts.replicationcontrollers`                           | token    | `100`                                                     |
| `quotaCounts.secrets`                          | Gateway host name    | `100`                                                     |
| `quotaCounts.services`   

### Tenant Resource Quotas

| Parameter                         | Description                          | Default                                                                      |
| --------------------------------- | ------------------------------------ | ---------------------------------------------------------------------------- |
| `resourceQuota.enabled`               | Set if to create new pull image secret    | `false`                                                                 |
| `resourceQuota.requests.cpu`               | Your Docker pull image secret name    | `10`                                                                   |
| `resourceQuota.requests.memory`               | Your Docker registry (DockerHub, etc.) username    | `10Gi`                                                                   |
| `resourceQuota.limits.cpu`               | Your Docker registry (DockerHub, etc.) password    | `20`                                                                   |
| `resourceQuota.limits.memory`                           | token    | `20Gi`                                                     |

```yaml
tenants:
  team-test1:
    quotas:
      quotaCounts:
        enabled: true
      resourceQuota:
        enabled: true
        requests:
          cpu: 1
          memory: 1Gi
        limits:
          cpu: 2
          memory: 4Gi
```

### Tenant Kyverno Policy

| Parameter                         | Description                          | Default                                                                      |
| --------------------------------- | ------------------------------------ | ---------------------------------------------------------------------------- |
| `labelPolicy.enabled`               | Whether to enforce a kyverno policy label policy in the tenant    | `false`                                                                 |

```yaml
tenants:
  team-test1:
    kyverno:
      labelPolicy:
        enabled: true
```

### Tenant Deployment Service Account

| Parameter                         | Description                          | Default                                                                      |
| --------------------------------- | ------------------------------------ | ---------------------------------------------------------------------------- |
| `serviceaccount.enabled`               | Whether to create a flux config in the tenant    | `false`                                                                 |
| `serviceaccount.name`               | The GitRepository URL    | `deploy-sa`                                                                   |

```yaml
tenants:
  team-test1:
    serviceaccount:
      enabled: true
```
### Tenant Flux Config

| Parameter                         | Description                          | Default                                                                      |
| --------------------------------- | ------------------------------------ | ---------------------------------------------------------------------------- |
| `fluxConfig.enabled`               | Whether to create a flux config in the tenant    | `false`                                                                 |
| `fluxConfig.url`               | The GitRepository URL    | `"."`                                                                   |
| `fluxConfig.path`               | The Kustomization path for the GitRepository | `"./$name"`                                                                   |  

```yaml
tenants:
  team-test1:
  flux:
    enabled: true
    path: ./team-test1
```
## Issues and feedback

If you encounter any problems or would like to give us feedback on deployments, raise issues here on GitHub.