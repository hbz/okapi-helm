# Okapi-helm

This helm chart deploys Okapi to a kubernetes cluster.

The helm chart creates the following Kubernetes Objects:

* Namespace - The namespace where Okapi would be deployed.
* Deployment - A deployment running the Okapi image. (For standalone deployments).
* Stateful set - A stateful set running the Okapi image. (For cluster deployments).
* Config maps - Stores configuration for Hazelcast.
* Secret - Stores sensitive environment variables e.g database details.
* Service account - Attached to the pods to assign certain kubernetes Cluster roles to them.
* Service - Exposes the pods.
* Ingress - Exposes the pods to allow Ingress traffic.

To use this helm chart, first add the repository locally.  

`helm repo add okapi https://indexdata.github.io/okapi-helm/`

  
Okapi can either be deployed standalone or as a cluster, the default mode is standalone.

For standalone deployments, follow the following steps:

* Create a secrets.json file to hold your environment variables. See example format below:

`secret: {"storage": "postgres","postgres_host": "<your_database_host>","postgres_username": "<your_database_username>","postgres_password": "<your_database_password>","postgres_database": "<your_database_name>","okapiurl": "http://okapi:9130"}`


* Run the command `helm install okapi/okapi --values=secrets.json`

For clustered deployments, follow the following steps:

* Create a secrets.json file to hold your environment variables. See example format below:

`secret: {"storage": "postgres","postgres_host": "<your_database_host>","postgres_username": "<your_database_username>","postgres_password": "<your_database_password>","postgres_database": "<your_database_name>","okapiurl": "http://okapi:9130"}`


* Run the command `helm install okapi/okapi --set clustered.enable=yes --values=secrets.json`

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| affinity | object | `{}` |  |
| affinity | object | `{}` |  |
| autoscaling.enabled | bool | `false` |  |
| autoscaling.maxReplicas | int | `100` |  |
| autoscaling.minReplicas | int | `1` |  |
| autoscaling.targetCPUUtilizationPercentage | int | `80` |  |
| clustered | object | `{"enabled":"no"}` | Specifies whether Okapi is deployed in cluster or standalone mode |
| config | object | `{}` |  |
| eks_iam.enabled | bool | `false` |  |
| eks_iam.existing.enabled | bool | `false` |  |
| eks_iam.existing.name | string | `"dummy"` |  |
| fullnameOverride | string | `"okapi"` |  |
| fuse.enabled | bool | `false` |  |
| image | object | `{"pullPolicy":"Always","repository":"folioorg/okapi","tag":"4.14.10"}` | Okapi docker image  |
| image.tag | string | `"4.14.10"` | Okapi docker image tag |
| imagePullSecrets | list | `[]` |  |
| imageSecretName | string | `""` | Image secret for accessing private private docker registries |
| ingress | object | `{"annotations":{"external-dns.alpha.kubernetes.io/target":"3739406b-kubesystem-foliod-57fa-596039908.us-east-1.elb.amazonaws.com","kubernetes.io/ingress.class":"nginx","nginx.ingress.kubernetes.io/proxy-body-size":"10000m","nginx.ingress.kubernetes.io/proxy-read-timeout":"300","nginx.ingress.kubernetes.io/proxy-request-buffering":"off"},"className":"","enabled":true,"hosts":[{"host":"okapi-cluster-okapi2.folio-dev-us-east-1-1.folio-dev.indexdata.com","paths":[{"path":"/","pathType":"ImplementationSpecific"}]}],"tls":[]}` | Specifies whether an ingress should be created |
| labels.env | string | `"default"` |  |
| labels.type | string | `"default"` |  |
| lifecycle | object | `{}` |  |
| nameOverride | string | `"okapi"` |  |
| namespace.create | bool | `true` | Specifies whether a namespace should be created |
| namespace.name | string | `"okapi-cluster-helm"` | The name of the namespace to deploy resources to. If create namespace is set to false, ensure the namespace exists. |
| nodeSelector | object | `{}` |  |
| nodeSelector | object | `{}` |  |
| persistentVolume.accessModes[0] | string | `"ReadWriteOnce"` |  |
| persistentVolume.annotations | object | `{}` |  |
| persistentVolume.enabled | bool | `false` |  |
| persistentVolume.labels | object | `{}` |  |
| persistentVolume.name | string | `"data"` |  |
| persistentVolume.size | string | `"8Gi"` |  |
| podAnnotations | object | `{}` |  |
| podDisruptionBudget.maxUnavailable | int | `1` |  |
| podDisruptionBudgetEnabled | bool | `true` |  |
| podManagementPolicy | string | `"OrderedReady"` | Pod management policy for pods |
| podSecurityContext | object | `{}` |  |
| probes | object | `{"liveness":{"enabled":true,"failureThreshold":2,"initialDelaySeconds":120,"path":"/_/proxy/health","periodSeconds":10,"timeoutSeconds":10},"readiness":{"enabled":true,"failureThreshold":2,"initialDelaySeconds":120,"path":"/_/proxy/health","periodSeconds":10,"timeoutSeconds":10},"startup":{"enabled":false,"failureThreshold":30,"path":"/","periodSeconds":10}}` | Specifies whether a liveness probe should be created |
| probes.startup | object | `{"enabled":false,"failureThreshold":30,"path":"/","periodSeconds":10}` | Specifies whether a startup probe should be created |
| replicaCount | int | `3` | Number of pods to create (applies to clusteres deployments alone) |
| resources | object | `{}` |  |
| secret | object | `{}` |  |
| secrets | list | `[]` |  |
| securityContext | object | `{}` |  |
| service.enabled | bool | `true` |  |
| service.external.enabled | bool | `false` |  |
| service.http.enabled | bool | `false` |  |
| service.internal.enabled | bool | `false` |  |
| service.name | string | `"okapi"` |  |
| service.port | int | `9130` |  |
| service.second_port.enabled | bool | `true` |  |
| service.second_port.port | int | `5701` |  |
| service.third_port.enabled | bool | `false` |  |
| service.third_port.port | int | `9090` |  |
| service.type | string | `"ClusterIP"` |  |
| serviceAccount.annotations | object | `{}` | Annotations to add to the service account |
| serviceAccount.create | bool | `true` | Specifies whether a service account should be created |
| serviceAccount.name | string | `""` | The name of the service account to use. If not set and create is true, a name is generated using the fullname template |
| terminationGracePeriodSeconds | int | `300` | Termination grace period for pods |
| tolerations | list | `[]` |  |
| tolerations | list | `[]` |  |
| updateStrategy | string | `"RollingUpdate"` | Update strategy for pods |

----------------------------------------------
