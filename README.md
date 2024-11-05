# Postiz Helm Chart

This Helm chart deploys the Postiz application on a Kubernetes cluster using the Helm package manager.

## Prerequisites

- Kubernetes 1.12+
- Helm 3.0+
- PV provisioner support in the underlying infrastructure (if persistence is required)

## Installing the Chart

The Postiz helm chart registry uses the OCI format, not HTTP, which means you do not need to do a `helm repo add` to install the chart. You can install the chart directly from the GitHub repository.

To install the chart with the release name `postiz-app`:

```bash
$ helm install postiz oci://ghcr.io/gitroomhq/postiz-helmchart/charts/postiz-app
```

The command deploys Postiz on the Kubernetes cluster in the default configuration. The [Parameters](#parameters) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `postiz` deployment:

```bash
$ helm delete postiz-app
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Parameters

The following table lists the configurable parameters of the Postiz chart and their default values.

| Parameter                | Description             | Default        |
| ------------------------ | ----------------------- | -------------- |
| `replicaCount`           | Number of replicas      | `1`            |
| `image.repository`       | Image repository        | `ghcr.io/gitroomhq/postiz-app` |
| `image.pullPolicy`       | Image pull policy       | `IfNotPresent` |
| `image.tag`              | Image tag               | `latest`       |
| `service.type`           | Kubernetes service type | `ClusterIP`    |
| `service.port`           | Kubernetes service port | `80`           |
| `postgresql.enabled`     | Deploy PostgreSQL       | `true`         |
| `postgresql.auth.username` | PostgreSQL username   | `postiz`       |
| `postgresql.auth.password` | PostgreSQL password   | `postiz-password` |
| `postgresql.auth.database` | PostgreSQL database   | `postiz`       |
| `redis.enabled`          | Deploy Redis            | `true`         |
| `redis.auth.password`    | Redis password          | `postiz-redis-password` |
| `ingress.enabled`        | Enable ingress controller resource    | `false`                  |
| `ingress.className`      | IngressClass that will be be used     | `""`                     |
| `ingress.annotations`    | Ingress annotations                   | `{}`                     |
| `ingress.hosts`          | Ingress hostnames                     | `[]`                     |
| `ingress.tls`            | Ingress TLS configuration             | `[]`                     |
| `ingress.path`           | Path within the host                  | `/`                      |
| `ingress.pathType`       | Ingress path type                     | `ImplementationSpecific` |
| `extraVolumes`           | Additional volumes to mount           | `[{"name": "uploads-volume", "emptyDir": {}}]` |
| `extraVolumeMounts`      | Additional volume mounts to use       | `[{"name": "uploads-volume", "mountPath": "/uploads"}]` |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example,

```bash
$ helm install postiz-app \
  --set postgresql.auth.password=secretpassword \
    postiz/postiz
```

The above command sets the PostgreSQL password to `secretpassword`.

Alternatively, you can use a YAML file to specify the values while installing the chart. Create a file called `custom-values.yaml` (or any name you prefer) and specify your values:

```yaml
postgresql:
  auth:
    password: secretpassword
ingress:
  enabled: true
  hosts:
    - host: postiz.example.com
```

Then, you can install the chart using the `-f`  flag:

```bash
$ helm install postiz-app -f custom-values.yaml postiz/postiz
```

> **Tip**: You can use the default [values.yaml](values.yaml) as a starting point for your custom configuration.


## Persistence

The chart mounts a [Persistent Volume](http://kubernetes.io/docs/user-guide/persistent-volumes/) for the PostgreSQL and Redis data. The volume is created using dynamic volume provisioning. If you want to disable this functionality you can change the values.yaml to disable persistence and use an emptyDir instead.

## Configuration and installation details

### External database support

You may want to have Postiz connect to an external database rather than installing one inside your cluster. Typical reasons for this are to use a managed database service, or to share a common database server for all your applications. To achieve this, set the `postgresql.enabled` parameter to `false` and specify the credentials for the external database using the `postgresql.auth.username`, `postgresql.auth.password`, and `postgresql.auth.database` parameters.

### External Redis support

Similar to the database, you can use an external Redis instance by setting `redis.enabled` to `false` and specifying the external Redis URL using the `REDIS_URL` environment variable in the `env` section of your values.yaml.

## Upgrading

### To 1.0.0

This is the first major release of the Postiz Helm chart.

## Contributing

We welcome contributions to this chart. Please read our [Contributing Guide](CONTRIBUTING.md) before submitting a pull request.

## License

This chart is licensed under the Apache License 2.0. See the [LICENSE](LICENSE) file for details.