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
  name: tenant1
  namespace: flux-system
spec:
  releaseName: tenant1
  chart:
    spec:
      chart: tenant-provision
      sourceRef:
        kind: HelmRepository
        name: global-artifactory
        namespace: flux-system
      version: 0.1.22
  interval: 1m0s
  values:
    tenants:
      team-test1:
        flux:
          path: ./team-test1
        kyverno:
          labelPolicy:
            enabled: true
        quotas:
          quotaCounts:
            configmaps: 150
            enabled: true
            persistentvolumeclaims: 150
            pods: 150
            secrets: 150
            services: 150
          resourceQuota:
            enabled: true
            limits:
              cpu: 4
              memory: 4Gi
            requests:
              cpu: 2
              memory: 2Gi
        rbac:
          tenantAdminRole:
            groupObjectId: ObjectId
          tenantAuditRole:
            groupObjectId: ObjectId
          tenantDevRole:
            groupObjectId: ObjectId
          tenantReaderRole:
            groupObjectId: ObjectId
        secret:
          eso:
            enabled: true
            azure_secret_store_name: azure-secret-store
            azure_managed_identity_client_id: 6e0adb10-1c95-4321-a6da-3f6d248901d1
            azure_tenant_id: 65b6be73-2104-4ff4-899f-5bff3196f3d1
            azure_vaultUrl: https://example.vault.azure.net/
            hc_vault_jwt_auth_path: clustere1
            hc_vault_jwt_role: team-test1
            hc_vault_kv_version: v2
            hc_vault_secret_path: team-test1
            hc_vault_secret_store_name: vault-secret-store
            hc_vault_server: http://127.0.0.1:8200/
        serviceaccount:
          enabled: true
          name: deploy-sa
          roleBinding:
            name: deploy-rb
      team-test2:
        flux:
          path: ./team-test2
        quotas:
          quotaCounts:
            configmaps: 150
            enabled: true
            persistentvolumeclaims: 150
            pods: 150
            secrets: 150
            services: 150
          resourceQuota:
            enabled: true
            limits:
              cpu: 4
              memory: 4Gi
            requests:
              cpu: 2
              memory: 2Gi
        rbac:
          tenantAdminRole:
            groupObjectId: ObjectId
          tenantAuditRole:
            groupObjectId: ObjectId
          tenantDevRole:
            groupObjectId: ObjectId
          tenantReaderRole:
            groupObjectId: ObjectId
```
## Configurable Variables

### Tenant Namespace

| Parameter                         | Description                          | Default                                                                      |
| --------------------------------- | ------------------------------------ | ---------------------------------------------------------------------------- |
| `namespace.create`               | Whether to create the corresponding namespace. If the Namespace is pre-existing then the default will be sufficient   | `false`   

#### Creating a Namespace
```yaml
tenants:
  team-test1:
    namespace:
      create: true
```

#### Pre-existing namespace

```yaml
tenants:
  team-test1:
```

### Tenant RBAC

| Parameter                         | Description                          | Default                                                                      |
| --------------------------------- | ------------------------------------ | ---------------------------------------------------------------------------- |
| `tenantAdminRole.enabled`               | Whether to create the Role and Role binding for tenantAdminRole.    | `false`                                                                 |
| `tenantAdminRole.groupObjectId`               | The group object ID from AZAD for the tenantAdminRole group.  | `N/A`                                                                   |
| `tenantAuditRole.enabled`               | Whether to create the Role and Role binding for tenantAuditRole.    | `false`                                                                   |
| `tenantAuditRole.groupObjectId`               | The group object ID from AZAD for the tenantAuditRole group.    | `N/A`                                                                   |
| `tenantReaderRole.enabled`                          | Whether to create the Role and Role binding for tenantReaderRole.   | `false`                                                     |
| `tenantReaderRole.groupObjectId`                          |  The group object ID from AZAD for the tenantReaderRole group.    | `N/A`                                                  |
| `tenantDevRole.enabled`                            | Whether to create the Role and Role binding for tenantDevRole.    | `false`                                                     |
| `tenantDevRole.groupObjectId`                              |  The group object ID from AZAD for the tenantDevRole group.  | `N/A`                                                  |

```yaml
tenants:
  team-test1:
    rbac:
      tenantAdminRole:
        enabled: true
        groupObjectId: "ObjectId"
      tenantAuditRole:
        enabled: true
        groupObjectId: "ObjectId"
      tenantDevRole:
        enabled: true
        groupObjectId: "ObjectId"
      tenantReaderRole:
        enabled: true
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
| `serviceaccount.name`               | The name of the service account being created    | `deploy-sa`                                                                   |

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

## Emerging Technology Section

### Tenant ESO AKV
| Parameter                         | Description                          | Default                                                                      |
| --------------------------------- | ------------------------------------ | ---------------------------------------------------------------------------- |
| `akv_eso.enabled`               | Whether to create a akv eso secret store in the tenant    | `false`                                                                 |
| `akv_eso.`               | The    | `"."`                                                                   |
| `akv_eso.`               | The  | `"./$name"`   

```yaml
tenants:
  team-test1:
    secret:
      akv_eso:
        enable: true
```

### Tenant ESO Vault
| Parameter                         | Description                          | Default                                                                      |
| --------------------------------- | ------------------------------------ | ---------------------------------------------------------------------------- |
| `vault_eso.enabled`               | Whether to create a vault eso secret store in the tenant    | `false`                                                                 |
| `vault_eso.`               | The     | `"."`                                                                   |
| `vault_eso.`               | The  | `""`   

```yaml
tenants:
  team-test1:
    secret:
      vault_eso:
        enable: true
```

## Issues and feedback

If you encounter any problems or would like to give us feedback on deployments, raise issues here on GitHub.
