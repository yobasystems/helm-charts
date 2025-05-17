# Nginx Helm Chart

This Helm chart deploys Nginx on Kubernetes, using an Nginx container image (e.g., `nginx:latest`).

## Chart Details

- **Chart Name:** nginx
- **Chart Version:** {{ .Chart.Version }}
- **Application Version:** {{ .Chart.AppVersion }}

## Prerequisites

- Kubernetes 1.19+
- Helm 3.2.0+
- PV provisioner support in the underlying infrastructure (if using persistence)

## Installing the Chart

To install the chart with the release name `my-nginx`:

```bash
helm install my-nginx .
```

Or, if you have packaged the chart:

```bash
helm install my-nginx nginx-{{ .Chart.Version }}.tgz
```

## Configuration

The configurable parameters for the Nginx chart and their default values are listed in `values.yaml`. You can customize these parameters to suit your deployment needs.

| Parameter                       | Description                                     | Default                               |
|---------------------------------|-------------------------------------------------|---------------------------------------|
| `replicaCount`                  | Number of Nginx replicas                         | `1`                                   |
| `image.repository`              | Nginx image repository                          | `nginx`                               |
| `image.pullPolicy`              | Image pull policy                               | `IfNotPresent`                        |
| `image.tag`                     | Nginx image tag (defaults to chart's appVersion) | `{{ .Chart.AppVersion }}`             |
| `serviceAccount.create`         | Whether to create a service account             | `true`                                |
| `serviceAccount.name`           | Name of the service account to use              | `""` (generated)                    |
| `podSecurityContext`            | Pod security context                            | `{}`                                  |
| `securityContext`               | Container security context                      | `{}`                                  |
| `service.type`                  | Kubernetes service type                         | `ClusterIP`                           |
| `service.port`                  | Nginx service port                              | `80`                                  |
| `livenessProbe.*`               | Liveness probe configuration                    | (see `values.yaml`)                   |
| `readinessProbe.*`              | Readiness probe configuration                   | (see `values.yaml`)                   |
| `resources`                     | CPU/Memory resource requests/limits             | `{}`                                  |
| `autoscaling.*`                 | HPA configuration                               | (disabled by default)                 |
| `persistence.content.enabled`   | Enable persistence for website content          | `false`                               |
| `persistence.content.annotations`| Annotations for the content PVC                 | `{}`                                  |
| `persistence.content.storageClass`| Storage class for content PVC                   | `""` (default provisioner)          |
| `persistence.content.accessMode`| Access mode for content PVC                     | `ReadWriteOnce`                       |
| `persistence.content.size`      | Size of the content PVC                         | `10Gi`                                |
| `persistence.content.existingClaim`| Use an existing PVC for content                | `nil`                                 |
| `persistence.content.mountPath`| Mount path for content volume                   | `/usr/share/nginx/html`               |
| `nodeSelector`                  | Node selector for pod assignment                | `{}`                                  |
| `tolerations`                   | Tolerations for pod assignment                  | `[]`                                  |
| `affinity`                      | Affinity for pod assignment                     | `{}`                                  |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`.
For example:

```bash
helm install my-nginx . \
  --set replicaCount=3 \
  --set service.type=LoadBalancer
```

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example:

```bash
helm install my-nginx . -f myvalues.yaml
```

> **Tip**: You can use `helm show values .` to see all configurable options.

## Persistence

This chart can be configured to use `PersistentVolumeClaim`s for storing Nginx data, such as website content or logs. You can configure persistence in the `values.yaml` file.

To enable persistence (example for website content):

```bash
--set persistence.content.enabled=true
--set persistence.content.size=10Gi
```

## Uninstalling the Chart

To uninstall/delete the `my-nginx` deployment:

```bash
helm uninstall my-nginx
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

**Note:** PersistentVolumeClaims (PVCs) are not automatically deleted when uninstalling the chart. You will need to manually delete them if you no longer need the data:

```bash
# Example: Replace with actual PVC names based on your release and values.yaml
kubectl delete pvc my-nginx-content
```
(Replace `my-nginx` with your release name if different, and `my-nginx-content` with the actual PVC name(s).)

## Contributing

Feel free to contribute to this chart by submitting issues or pull requests.
