## Base Deployment

This helm chart is designed to help deploy basic services.  
It has really limited functionality, but is useful for getting up and running quickly.  
Included services:

* Configmap
* Deployment
* Service
* Ingress

The deployment allows you to provide a command and arguments for the container.

The resource limits are capped at 1 CPU and 1512MB of memory. but requests are configurable

## Usage

```yaml 
helm repo add myrepo https://lordrahl90.github.io/base-deployment
```

## Configuration

See the [values file](values.yaml) for configuration options

## Installation

```yaml 
helm repo add myrepo https://lordrahl90.github.io/base-deployment
```

## Usage

Create your own chart and add this repository as a dependency

```yaml Chart.yml
apiVersion: v2
name: chart-name
description: A Helm chart for Kubernetes
type: application
version: 0.1.0
appVersion: "1.16.0"
dependencies:
  - name: base-deployment
    version: 0.1.0
    repository: https://lordrahl90.github.io/base-deployment
```

## Note
The label section is really important. It is used to identify the resources.  
The label section in the values file can be defined as follows:

```yaml
labels:
  tier: frontend
```

## Importing Templates

In the templates package, you add the necessary ones that you need.

### Deployment

If you want to add a deployment, you can use the following template:
`deployment` -> you create a `deployment.yaml` file and insert the following:
```yaml
{{ - include "base-deployment.deployment" . - }}
```
The deployment section in the `values.yaml` file can be defined as follows:

```yaml
deployment:
  replicas: 10
  image: "lordrahl/shipments"
  resources:
    memory: "64Mi"
    cpu: "250m"
```

### Service

If you want to add a service, you can use the following template:   
You create a `service.yaml` file and insert the following:
```yaml
{{ - include "base-deployment.service" . - }}
```

The service section in the `values.yaml` file can be defined as follows:

```yaml
service:
  targetPort: 8080
```

2 ports are exposed for service by default and are mapped to the target port
* 80 -> http
* 443 -> https

### Ingress

If you want to add an ingress, you create a `ingress.yaml` file and insert the following:

```yaml
{{ - include "base-deployment.ingress" . - }}
```

The values in the section will be populated as:

```yaml
ingress:
  host: "ing-shipments.lordrahl.ng"
  annotations:
    kubernetes.io/ingress.class: "alb"
```

The ingress can take as many annotations as you want.  
The ingress also uses the service that was defined/created earlier.

### Configmap
The configmap allows merging of configuration files.
You create a `configmap.yaml` file and insert the following:

```yaml
{{- include "base-deployment.configmap" . -}}
{{- define "shipments.configmap" -}}
data:
  MESSAGE: "hello-world"`
{{- end -}}
```
This allows you to add as many values as you'd like to the configmap

### Secrets
The secret that is attached to the deployment is configurable.  
If you do not want any secret on the pod, set the `secrets.enabled` value to `false` in the `values.yaml` file.  
The name of the secret is also configurable, however if the secret is enabled and no name is given, the deployment defaults to 
`.ReleaseName-secrets` as the secret name.   
This could prevent the pod from starting up if the secret is not available.

## TODO:

* Improve the readme and add more examples