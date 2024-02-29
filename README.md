# Okapi-helm

Copyright (C) 2024 Index Data

This software is distributed under the terms of the Apache License,
Version 2.0. See the file "[LICENSE](LICENSE)" for more information.

This helm chart deploys Okapi to a kubernetes cluster and is offered by Index Data only as is. There is no support implied unless under contract.

The helm chart creates the following Kubernetes Objects:

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

* Create a secrets.yaml file to hold your environment variables. See example format below:

    
    secret:
    
      storage: postgres
    
      postgres_host: <your_database_host>
    
      postgres_username: <your_database_username>
    
      postgres_password: <your_database_password>
    
      postgres_database: <your_database_name>
    
      okapiurl: http://okapi:9130


* Run the command `helm install okapi okapi/okapi --namespace <your-namespace> --values=secrets.yaml`

For clustered deployments, follow the following steps:

* Create a secrets.yaml file to hold your environment variables. See example format below:

    secret:
    
      storage: postgres
    
      postgres_host: <your_database_host>
    
      postgres_username: <your_database_username>
    
      postgres_password: <your_database_password>
    
      postgres_database: <your_database_name>
    
      okapiurl: http://okapi:9130


* Run the command `helm install okapi okapi/okapi --namespace <your-namespace> --values=secrets.yaml --set clustered.enabled=true`

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| affinity | object | `{}` |  |
| autoscaling.enabled | bool | `false` |  |
| autoscaling.maxReplicas | int | `100` |  |
| autoscaling.minReplicas | int | `1` |  |
| autoscaling.targetCPUUtilizationPercentage | int | `80` |  |
| clustered | object | `{"enabled":"false"}` | Specifies whether Okapi is deployed in cluster or standalone mode |
| config | object | `{}` |  |
| fullnameOverride | string | `okapi` |  |
| image.repository | string | `folioorg/okapi` | Okapi docker image tag |
| image.pullPolicy | string | `Always` | Okapi docker image tag |
| image.tag | string | `5.1.2` | Okapi docker image tag |
| imagePullSecrets | list | `[]` |  |
| imageSecretName | string | `""` | Image secret for accessing private private docker registries |
| ingress.enabled | bool | `true` | Specifies whether an ingress should be created |
| ingress.className | object | `nginx` | Specifies the class for the Ingress
| ingress.annotations | object | `external-dns.alpha.kubernetes.io/target:xxxxxxx.your-dns-target.com` <br/> `nginx.ingress.kubernetes.io/proxy-body-size:"10000m"`<br/> `nginx.ingress.kubernetes.io/proxy-read-timeout:"300"` <br/> `nginx.ingress.kubernetes.io/proxy-request-buffering:"off"` | Ingress annotations |
| ingress.hosts | list | `{"host":"xyz.hostname-of-your-service.com","paths":[{"path":"/","pathType":"ImplementationSpecific"}]}` | Ingress host |
| ingress.tls | list | `[]` | Enable TLS for ingress
| labels.env | string | `"default"` |  |
| labels.type | string | `"default"` |  |
| lifecycle | object | `{}` |  |
| nameOverride | string | `"okapi"` |  |
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
| probes.liveness.enabled | bool | `true` | Specifies whether a liveness probe should be created |
| probes.liveness.path | string | `/_/proxy/health` | Specifies path for liveness probe |
| probes.liveness.initialDelaySeconds | int | `120` | Specifies initial delay before liveness probe starts |
| probes.liveness.failureThreshold | int| `2` | Specifies failure threshold for liveness probe |
| probes.liveness.timeoutSeconds | int | `10` | Specifies timeout in seconds for liveness probe |
| probes.liveness.periodSecond | int | `10` | Specifies period in seconds for liveness probe |
| probes.readiness.enabled | bool | `true` | Specifies whether a readiness probe should be created |
| probes.readiness.failureThreshold | int | `2` | Specifies initial delay before readiness probe starts|
| probes.readiness.initialDelaySeconds | int | `120` | Specifies initial delay before readiness probe starts |
| probes.readiness.path | string | `/_/proxy/health` | Specifies path for readiness probe  |
| probes.readiness.periodSeconds | int | `10` | Specifies period in seconds for readiness probe |
| probes.readiness.timeoutSeconds | int | `10` | Specifies timeout in seconds for readiness probe |
| probes.startup.enabled | object | `false`| Specifies whether a startup probe should be created |
| probes.startup.failureThreshold | int | `30`| Specifies failure threshold for startup probe |
| probes.startup.path | string | `/`| Specifies path for readiness probe |
| probes.startup.periodSeconds | int | `10`| Specifies period in seconds for startup probe |
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
| updateStrategy | string | `RollingUpdate` | Update strategy for pods |
| topologySpreadConstraints.maxSkew | int | `1` | Max Skew for spread topology constraint |
| topologySpreadConstraints.topologyKey | string | `kubernetes.io/hostname` | Topology Key for spread topology constraint |
| topologySpreadConstraints.whenUnsatisfiable | string | `ScheduleAnyway` | Action when spread topology constraint is unsatisfiable  |

----------------------------------------------
