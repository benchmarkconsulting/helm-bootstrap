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

## Configurable Variables

### ClusterAdmin RBAC Role

| Parameter                         | Description                          | Default                                                                      |
| --------------------------------- | ------------------------------------ | ---------------------------------------------------------------------------- |
| `clusterAdminRole.enable`               | Whether to create the Role and Role binding for tenantAdminRole.    | `false`                                                                 |
| `clusterAdminRole.name`               | The group object ID from AZAD for the tenantAdminRole group.  | `N/A`       |
| `clusterAdminRole.groupObjectId`              | Whether to create the Role and Role binding for tenantAuditRole. |   `false`                                                                                                                              

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