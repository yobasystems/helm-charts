# MariaDB Helm Chart

This Helm chart deploys MariaDB on Kubernetes, using the `ghcr.io/yobasystems/alpine-mariadb` Container image.

## Chart Details

- **Chart Name:** mariadb
- **Chart Version:** {{ .Chart.Version }}
- **Application Version:** {{ .Chart.AppVersion }}

## Prerequisites

- Kubernetes 1.19+
- Helm 3.2.0+
- PV provisioner support in the underlying infrastructure (if using persistence)

## Installing the Chart

To install the chart with the release name `my-mariadb`:

```bash
helm install my-mariadb .
```

Or, if you have packaged the chart:

```bash
helm install my-mariadb mariadb-{{ .Chart.Version }}.tgz
```

## Configuration

The following table lists the configurable parameters of the MariaDB chart and their default values.

| Parameter                       | Description                                     | Default                               |
|---------------------------------|-------------------------------------------------|---------------------------------------|
| `replicaCount`                  | Number of MariaDB replicas                      | `1`                                   |
| `image.repository`              | MariaDB image repository                        | `ghcr.io/yobasystems/alpine-mariadb`    |
| `image.pullPolicy`              | Image pull policy                               | `IfNotPresent`                        |
| `image.tag`                     | MariaDB image tag (defaults to chart's appVersion) | `{{ .Chart.AppVersion }}`             |
| `serviceAccount.create`         | Whether to create a service account             | `true`                                |
| `serviceAccount.name`           | Name of the service account to use              | `""` (generated)                    |
| `podSecurityContext`            | Pod security context                            | `{}`                                  |
| `securityContext`               | Container security context                      | `{}`                                  |
| `service.type`                  | Kubernetes service type                         | `ClusterIP`                           |
| `service.port`                  | MariaDB service port                            | `3306`                                |
| `mariadbConfiguration.rootPassword` | Root password for MariaDB (use secrets in prod) | `"changeme-root"`                   |
| `mariadbConfiguration.database` | Default database to create                      | `"mydatabase"`                        |
| `mariadbConfiguration.user`     | Default user to create                          | `"myuser"`                            |
| `mariadbConfiguration.password` | Password for the default user (use secrets in prod) | `"changeme-user"`                   |
| `mariadbConfiguration.charset`  | Default charset for MariaDB                     | `"utf8mb4"`                           |
| `mariadbConfiguration.collation`| Default collation for MariaDB                   | `"utf8mb4_unicode_ci"`                |
| `livenessProbe.*`               | Liveness probe configuration                    | (see `values.yaml`)                   |
| `readinessProbe.*`              | Readiness probe configuration                   | (see `values.yaml`)                   |
| `resources`                     | CPU/Memory resource requests/limits             | `{}`                                  |
| `autoscaling.*`                 | HPA configuration                               | (disabled by default)                 |
| `persistence.data.enabled`      | Enable persistence for database files           | `true`                                |
| `persistence.data.annotations`  | Annotations for the data PVC                    | `{}`                                  |
| `persistence.data.storageClass` | Storage class for data PVC                      | `""` (default provisioner)          |
| `persistence.data.accessMode`   | Access mode for data PVC                        | `ReadWriteOnce`                       |
| `persistence.data.size`         | Size of the data PVC                            | `8Gi`                                 |
| `persistence.data.existingClaim`| Use an existing PVC for data                    | `nil`                                 |
| `persistence.data.mountPath`    | Mount path for data volume                      | `/var/lib/mysql`                      |
| `persistence.logs.enabled`      | Enable persistence for log files                | `true`                                |
| `persistence.logs.annotations`  | Annotations for the logs PVC                    | `{}`                                  |
| `persistence.logs.storageClass` | Storage class for logs PVC                      | `""` (default provisioner)          |
| `persistence.logs.accessMode`   | Access mode for logs PVC                        | `ReadWriteOnce`                       |
| `persistence.logs.size`         | Size of the logs PVC                            | `1Gi`                                 |
| `persistence.logs.existingClaim`| Use an existing PVC for logs                    | `nil`                                 |
| `persistence.logs.mountPath`    | Mount path for logs volume                      | `/var/lib/mysql/mysql-bin`            |
| `nodeSelector`                  | Node selector for pod assignment                | `{}`                                  |
| `tolerations`                   | Tolerations for pod assignment                  | `[]`                                  |
| `affinity`                      | Affinity for pod assignment                     | `{}`                                  |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`.
For example:

```bash
helm install my-mariadb . \
  --set mariadbConfiguration.rootPassword=mysecretpassword \
  --set persistence.data.size=20Gi
```

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example:

```bash
helm install my-mariadb . -f myvalues.yaml
```

> **Tip**: You can use `helm show values .` to see all configurable options.

## Persistence

This chart provisions `PersistentVolumeClaim`s for MariaDB data and logs by default. You can configure the `storageClass`, size, and other parameters in the `values.yaml` file under the `persistence` section.

To disable persistence for data:

```bash
--set persistence.data.enabled=false
```

To disable persistence for logs:

```bash
--set persistence.logs.enabled=false
```

## Uninstalling the Chart

To uninstall/delete the `my-mariadb` deployment:

```bash
helm uninstall my-mariadb
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

**Note:** PersistentVolumeClaims (PVCs) are not automatically deleted when uninstalling the chart. You will need to manually delete them if you no longer need the data:

```bash
kubectl delete pvc my-mariadb-data
kubectl delete pvc my-mariadb-logs
```
(Replace `my-mariadb` with your release name if different).

## Contributing

Feel free to contribute to this chart by submitting issues or pull requests.
