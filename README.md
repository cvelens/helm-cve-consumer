# helm-webapp-cve-consumer

This Helm chart deploys the CVE Consumer application, which consumes CVE (Common Vulnerabilities and Exposures) data from a Kafka topic and stores it in a PostgreSQL database.

## Prerequisites

Before deploying this Helm chart, ensure you have the following:

- Kubernetes cluster (version 1.19+)
- Helm (version 3.0+)
- Access to a Kafka cluster
- Namespace created for the deployment

## Installation

To install the chart with the release name `my-release`:

```bash
helm install my-release ./helm-webapp-cve-consumer
```

## Configuration

The following table lists the configurable parameters of the `helm-webapp-cve-consumer` chart and their default values.

| Parameter                    | Description                                 | Default                                      |
|------------------------------|---------------------------------------------|----------------------------------------------|
| `replicaCount`               | Number of consumer replicas                 | 1                                            |
| `namespace`                  | Namespace to deploy the consumer            | ns1                                          |
| `app.image`                  | Consumer application image                  | your-image-repository/cve-consumer           |
| `app.tag`                    | Consumer application image tag              | latest                                       |
| `app.pullPolicy`             | Image pull policy                           | Always                                       |
| `app.env`                    | Environment variables for the consumer app  | See `values.yaml` for defaults               |
| `postgres.image`             | PostgreSQL image                            | postgres                                     |
| `postgres.tag`               | PostgreSQL image tag                        | 15.3                                         |
| `postgres.pullPolicy`        | PostgreSQL image pull policy                | IfNotPresent                                 |
| `postgres.env`               | Environment variables for PostgreSQL        | See `values.yaml` for defaults               |
| `storage.size`               | Size of persistent volume claim             | 1Gi                                          |
| `imagePullSecrets.name`      | Name of the image pull secret               | dockerhub                                    |
| `imagePullSecrets.dockerconfigjson` | Base64 encoded Docker config JSON    | See `values.yaml` for defaults               |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example:

```bash
helm install my-release ./helm-webapp-cve-consumer --set replicaCount=3
```

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example:

```bash
helm install my-release ./helm-webapp-cve-consumer -f values.yaml
```

## Architecture

This Helm chart deploys the following components:

- CVE Consumer application (Deployment)
- PostgreSQL database (StatefulSet)
- Services for the consumer and PostgreSQL
- Persistent Volume Claim for PostgreSQL data

The consumer application reads CVE data from a Kafka topic and stores it in the PostgreSQL database. The application is configured to handle potential duplicates and updates to existing CVE records.

## Persistence

The chart mounts a Persistent Volume for PostgreSQL. The volume is created using dynamic volume provisioning.

## Upgrading

To upgrade the chart:

```bash
helm upgrade my-release ./helm-webapp-cve-consumer
```

## Uninstalling

To uninstall/delete the my-release deployment:

```bash
helm delete my-release
```
This command removes all the Kubernetes components associated with the chart and deletes the release.

## Troubleshooting

If you encounter issues:

Check the status of the pods:

```bash
kubectl get pods -n <namespace>
```

View the logs of the consumer application:

```bash
kubectl logs -f <consumer-pod-name> -n <namespace>
```

Check the PostgreSQL logs:

```bash
kubectl logs -f <postgres-pod-name> -n <namespace>
```

## Contributing

Please follow the standard GitHub workflow:

1. Fork the repository.
2. Create a new branch for your feature or bug fix.
3. Make your changes and commit them with descriptive messages.
4. Push your changes to your forked repository.
5. Submit a pull request to the main repository.

Please ensure that your code follows the existing style and conventions used in the project.