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