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
helm install base-deployment stable/base-deployment
```

## Configuration

See the [values file](values.yaml) for configuration options

## Installation

The chart is available on [Helm Hub](https://hub.helm.sh/chart/stable/base-deployment)

## Upgrading

The chart is available on [Helm Hub](https://hub.helm.sh/chart/stable/base-deployment)

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md)

## TODO:

* Improve the readme  