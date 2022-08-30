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
    - [Tenant Quotas](#tenant-quotas)
  - [Issues and feedback](#issues-and-feedback)

## Prerequisites

### Container Registry Credentials

## Installing the Charts

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

### Tenant Secrets

| Parameter                         | Description                          | Default                                                                      |
| --------------------------------- | ------------------------------------ | ---------------------------------------------------------------------------- |
| `akv2k8sSecret.enabled`               | Whether to create the akv2k8sSecret.    | `false`                                                                 |
| `name`               | Your Docker pull image secret name    | `N/A`                                                                   |
| `username`               | Your Docker registry (DockerHub, etc.) username    | `N/A`                                                                   |
| `password`               | Your Docker registry (DockerHub, etc.) password    | `N/A`                                                                   |


### Tenant Quotas

| Parameter                         | Description                          | Default                                                                      |
| --------------------------------- | ------------------------------------ | ---------------------------------------------------------------------------- |
| `create`               | Set if to create new pull image secret    | `false`                                                                 |
| `name`               | Your Docker pull image secret name    | `N/A`                                                                   |
| `username`               | Your Docker registry (DockerHub, etc.) username    | `N/A`                                                                   |
| `password`               | Your Docker registry (DockerHub, etc.) password    | `N/A`                                                                   |
| `placeholder`                           | Aqua Enforcer token    | `N/A`                                                     |
| `server`                          | Gateway host name    | `N/A`                                                     |
| `port`   

## Issues and feedback

If you encounter any problems or would like to give us feedback on deployments, raise issues here on GitHub.