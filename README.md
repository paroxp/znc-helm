# ZNC Helm Chart

[ZNC](http://znc.in/) is an IRC network bouncer or BNC.


## TL;DR;

```console
$ helm install stable/znc
```


## Introduction

This chart bootstraps a [ZNC](http://znc.in/) deployment on a [Kubernetes](https://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.


## Prerequisites Details

* [`PersistentVolume`](https://kubernetes.io/docs/concepts/storage/persistent-volumes/) support on underlying infrastructure


## Installing the Chart

To install the chart with the release name `my-release`:

```console
$ helm install --name my-release stable/znc
```


## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```console
$ helm delete my-release
```

The command removes nearly all the Kubernetes components associated with the chart and deletes the release.

> ps: By default, a [namespace](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/) is created for the `main` team named after `${RELEASE}-main` and is kept untouched after a `helm delete`.
> See the [Configuration section](#configuration) for how to control the behavior.


### Cleanup orphaned Persistent Volumes

This chart uses [`StatefulSets`](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/) for ZNC. Deleting a `StatefulSet` does not delete associated `PersistentVolume`s.

Do the following after deleting the chart release to clean up orphaned Persistent Volumes.

```console
$ kubectl delete pvc -l app=${RELEASE-NAME}-worker
```


## Configuration

The following table lists the configurable parameters of the ZNC chart and their default values.

| Parameter               | Description                           | Default                                                    |
| ----------------------- | ----------------------------------    | ---------------------------------------------------------- |
| `fullnameOverride` | Provide a name to substitute for the full names of resources | `nil` |
| `replicaCount` | Number of ZNC replicas running | `1` |
| `image.repository` | ZNC image | `znc` |
| `image.tag` | ZNC image tag | `1.7.4-slim` |
| `image.pullPolicy` | ZNC image pull policy | `IfNotPresent` |
| `imagePullSecrets` | Array of `imagePullSecrets` in the namespace for pulling images	 | `[]` |
| `nameOverride` | Provide a name in place of `znc` for `app:` labels | `nil` |
| `service.type` | ZNC service type | `ClusterIP` |
| `service.port` | ZNC service port | `6697` |
| `ingress.enabled` | Enable ZNC web ingress	 | `false` |
| `ingress.annotations` | ZNC web ingress annotations | `{}` |
| `ingress.hosts` | ZNC web ingress hostnames | `[]` |
| `ingress.tls` | ZNC web ingress tls configuration | `[]` |
| `resources` | Kubernetes resource definition for ZNC | `{}` |
| `nodeSelector` | Node selector for ZNC | `{}` |
| `tolerations` | Tolerations for ZNC | `[]` |
| `affinity` | Affinity rules for ZNC | `{}` |
| `persistent.enabled` | Enable perstistent storage | `true` |
| `persistent.size` | Size of the persistent storage | `128Mi` |
| `config.ssl` | Use the SSL connection | `false` |
| `config.ipv4` | Enable IPv4 connections | `true` |
| `config.ipv6` | Enable IPv6 connections | `false` |
| `config.modules` | Modules to be loaded globally | `[webadmin]` |
| `config.users.[].name` | User name | `admin` |
| `config.users.[].admin` | Declare user as admin | `true` |
| `config.users.[].modules` | List of modules to be loaded for specific user | `[chansaver, controlpanel]` |

For those sub-sections that have `enabled`, one needs to set `enabled` to be `true` to use the following params within the section.

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`.

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example,

```console
$ helm install --name my-release -f values.yaml stable/znc
```

> **Tip**: You can use the default [values.yaml](values.yaml)
