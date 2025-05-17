# Yoba Systems PostgreSQL Helm Chart

This Helm chart deploys PostgreSQL on Kubernetes, using the `ghcr.io/yobasystems/alpine-postgres` Container image.

## Chart Details

- **Chart Name:** postgresql
- **Chart Version:** {{ .Chart.Version }}
- **Application Version:** {{ .Chart.AppVersion }}

## Prerequisites

- Kubernetes 1.19+
- Helm 3.2.0+
- PV provisioner support in the underlying infrastructure (if using persistence)

## Installing the Chart

To install the chart with the release name `my-postgresql` (assuming you have added the `yobasystems` repository):

```bash
helm install my-postgresql yobasystems/postgresql
```

## Configuration

The following table lists the configurable parameters of the PostgreSQL chart and their default values.

| Parameter                                  | Description                                                                                                 | Default                                                     |
|--------------------------------------------|-------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------|
| `replicaCount`                             | Number of PostgreSQL replicas                                                                                  | `1`                                                         |
| `image.repository`                         | PostgreSQL image repository                                                                                    | `ghcr.io/yobasystems/alpine-postgresql`                         |
| `image.tag`                                | PostgreSQL image tag (immutable tags are recommended)                                                        | `{{ .Chart.AppVersion }}`                                   |
| `image.pullPolicy`                         | PostgreSQL image pull policy                                                                                 | `IfNotPresent`                                              |
| `serviceAccount.create`                    | Specifies whether a service account should be created                                                       | `true`                                                      |
| `serviceAccount.name`                      | The name of the service account to use. If not set and create is true, a name is generated using the fullname template | `""`                                                        |
| `podSecurityContext`                       | Pod security context                                                                                         | `{}`                                                        |
| `securityContext`                          | Container security context                                                                                   | `{}`                                                        |
| `service.type`                             | Kubernetes service type                                                                                     | `ClusterIP`                                                 |
| `service.port`                             | Kubernetes service port                                                                                     | `5432`                                                      |
| `postgresqlConfiguration.rootPassword`     | Root password for PostgreSQL (use secrets in prod)                                                         | `"changeme-root"`                                           |
| `postgresqlConfiguration.database`         | Default database to create                                                                                  | `"mydatabase"`                                              |
| `postgresqlConfiguration.user`             | Default user to create                                                                                      | `"myuser"`                                                  |
| `postgresqlConfiguration.password`         | Password for the default user (use secrets in prod)                                                        | `"changeme-user"`                                           |
| `livenessProbe.*`                          | Liveness probe configuration                                                                                | (see `values.yaml`)                                         |
| `readinessProbe.*`                         | Readiness probe configuration                                                                               | (see `values.yaml`)                                         |
| `resources`                                | CPU/Memory resource requests/limits                                                                         | `{}`                                                        |
| `autoscaling.*`                            | HPA configuration                                                                                           | (disabled by default)                                       |
| `persistence.data.enabled`                 | Enable persistence for PostgreSQL data                                                                       | `true`                                                      |
| `persistence.data.annotations`             | Annotations for the data PVC                                                                                | `{}`                                                        |
| `persistence.data.storageClass`            | Storage class for data PVC                                                                                  | `""` (default provisioner)                                   |
| `persistence.data.accessMode`              | Access mode for data PVC                                                                                    | `ReadWriteOnce`                                             |
| `persistence.data.size`                    | Size of the data PVC                                                                                        | `8Gi`                                                       |
| `persistence.data.existingClaim`           | Use an existing PVC for data                                                                                | `nil`                                                       |
| `persistence.data.mountPath`               | Mount path for PostgreSQL data                                                                              | `/var/lib/postgresql/data`                                  |
| `persistence.logs.enabled`                 | Enable persistence for log files                                                                            | `true`                                                      |
| `persistence.logs.annotations`             | Annotations for the logs PVC                                                                                | `{}`                                                        |
| `persistence.logs.storageClass`            | Storage class for logs PVC                                                                                  | `""` (default provisioner)                                   |
| `persistence.logs.accessMode`              | Access mode for logs PVC                                                                                    | `ReadWriteOnce`                                             |
| `persistence.logs.size`                    | Size of the logs PVC                                                                                        | `1Gi`                                                       |
| `persistence.logs.existingClaim`           | Use an existing PVC for logs                                                                                | `nil`                                                       |
| `persistence.logs.mountPath`               | Mount path for logs volume                                                                                  | `/var/lib/postgresql/logs`                                  |
| `nodeSelector`                             | Node selector for pod assignment                                                                            | `{}`                                                        |
| `tolerations`                              | Tolerations for pod assignment                                                                              | `[]`                                                        |
| `affinity`                                 | Affinity for pod assignment                                                                                 | `{}`                                                        |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`.
For example:

```bash
helm install my-postgresql yobasystems/postgresql \
  --set postgresqlConfiguration.rootPassword=mysecretpassword \
  --set persistence.data.size=20Gi
```

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example:

```bash
helm install my-postgresql yobasystems/postgresql -f myvalues.yaml
```

> **Tip**: You can use `helm show values yobasystems/postgresql` to see all configurable options.

## Persistence

This chart provisions `PersistentVolumeClaim`s for PostgreSQL data and logs by default. You can configure the `storageClass`, size, and other parameters in the `values.yaml` file under the `persistence` section.

To disable persistence for data:

```bash
--set persistence.data.enabled=false
```

To disable persistence for logs:

```bash
--set persistence.logs.enabled=false
```

## Uninstalling the Chart

To uninstall/delete the `my-postgresql` deployment:

```bash
helm uninstall my-postgresql
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

**Note:** PersistentVolumeClaims (PVCs) are not automatically deleted when uninstalling the chart. You will need to manually delete them if you no longer need the data:

```bash
kubectl delete pvc my-postgresql-data
kubectl delete pvc my-postgresql-logs
```
(Replace `my-postgresql` with your release name if different).

## Contributing

Feel free to contribute to this chart by submitting issues or pull requests.
