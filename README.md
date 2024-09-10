# Postiz Helm Chart

This Helm chart deploys the Postiz application on a Kubernetes cluster using the Helm package manager.

## Prerequisites

- Kubernetes 1.12+
- Helm 3.0+
- PV provisioner support in the underlying infrastructure (if persistence is required)

## Installing the Chart

To install the chart with the release name `postiz-app`:

```bash
$ helm repo add postiz https://github.com/gitroomhq/postiz-helmchart
$ helm install postiz-app postiz/postiz
```

The command deploys Postiz on the Kubernetes cluster in the default configuration. The [Parameters](#parameters) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `postiz-app` deployment:

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

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example,

```bash
$ helm install postiz-app \
  --set postgresql.auth.password=secretpassword \
    postiz/postiz
```

The above command sets the PostgreSQL password to `secretpassword`.

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example,

```bash
$ helm install postiz-app -f values.yaml postiz/postiz
```

> **Tip**: You can use the default [values.yaml](values.yaml)

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