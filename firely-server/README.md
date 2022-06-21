# Firely Server Helm Chart

## Prerequisites Details

* Kubernetes 1.19+
* PV support on the underlying infrastructure.
* Firely Server license

## Chart Details

This chart implements a [Firely Server](https://fire.ly/products/firely-server/) deployment on a [Kubernetes](https://kubernetes.io) cluster using the [Helm](https://helm.sh/) package manager.

## Installing the Chart

To install the chart with the release name `my-release`:

``` console
$ helm repo add firely https://raw.githubusercontent.com/FirelyTeam/Helm.Charts/main/firely-server/
$ helm install my-release firely/firely-server
```
The command deploys Firely Server on the Kubernetes cluster in the default configuration. The configuration section lists the parameters that can be configured during installation.

## Uninstalling the Chart
To uninstall/delete the `my-release` deployment:

``` console 
$ helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Configuration

The following table lists the configurable parameters of the firely-server chart and their default values.

### Global parameters

 Name                                                      | Description                                                               | Default                                             |
| -------------------------------------------------------- | ------------------------------------------------------------------------- | --------------------------------------------------- |
| `image.repository`                                       | Firely Server image name                                                  | `firely/server`                                   |
| `image.tag`                                              | Firely Server image tag                                                   | `4.7.0`                                             |
| `image.pullPolicy`                                       | Firely Server image pull policy                                           |`IfNotPresent`                                       |

### Common parameters
 Name                                                      | Description                                                               | Default                                             |
| -------------------------------------------------------- | ------------------------------------------------------------------------- | --------------------------------------------------- |
| `nameOverride`                                           | String to partially override firely-server.fullname template (will maintain the release name) | `""`
| `fullnameOverride`                                       | String to fully override firely-server.fullname template | `""` |

### Firely Server parameters
 Name                                                      | Description                                                               | Default                                             |
| -------------------------------------------------------- | ------------------------------------------------------------------------- | --------------------------------------------------- |
| `vonksettings`                                           | The override of the appsettings of Firely Server. This must be a Json format. Do not forget to indent this with 2 white spaces. See also more information below.  | `{ }` |
| `logsettings`                                            | The override of the logsettings of Firely Server. This must be a Json format.  Do not forget to indent this with 2 white spaces. See also more information below.| `{ }` |
| `license.secretName`                                     | You can  save the Firely Server license key yourself as a Kubernetes secret. Use then the name of that secret here. This setting overrides `license.value`| `nil` |
| `license.fileName`                                       | The name of the license file used internally. No reason to change that. | `firelyserver-license.json` |
| `license.value`                                          | The content of the license. See also more information below. | `nil` |
| `license.requestInfoFile`                                | Sets the location of the file with request information. This file will be used in future releases.| `nil` |
| `license.writeRequestInfoFileInterval`                   | Sets the time interval (in minutes) to write aggregate information about processed requests to the RequestInfoFile.| `nil` |
| `plugins[].url`                                          | An absolute url where a Firely Server plugin can be downloaded| `nil` |
| `plugins[].checksum`                                     | The checksum of the plugin | `nil` |

### Firely Server deployment parameters 
 Name                                                      | Description                                                               | Default                                             |
| -------------------------------------------------------- | ------------------------------------------------------------------------- | --------------------------------------------------- |
| `replicaCount`                                           | Number of replicas in the replica set                                     | `1`                                                 |
| `readinessProbe.initialDelaySeconds`                     | Number of seconds after the container has started before readiness probes are initiated | `15` |
| `readinessProbe.timeoutSeconds`                          | Number of seconds after which the probe times out  | `5` |
| `readinessProbe.failureThreshold`                        | When a Pod starts and the probe fails, Kubernetes will try `failureThreshold` times before giving up. The Pod will be marked Unready. | `3`| 
| `readinessProbe.periodSeconds`                           | How often (in seconds) to perform the probe | `10` |
| `readinessProbe.successThreshold`                        | Minimum consecutive successes for the probe to be considered successful after having failed | `1` |
| `livenessProbe.initialDelaySeconds`                      | Number of seconds after the container has started before liveness probes are initiated | `10` |
| `livenessProbe.timeoutSeconds`                           | Number of seconds after which the probe times out | `5` |
| `livenessProbe.failureThreshold`                         | When a Pod starts and the probe fails, Kubernetes will try `failureThreshold` times before giving up. The Pod will restart | `3` |
| `livenessProbe.periodSeconds`                            | How often (in seconds) to perform the probe | `120` |
| `livenessProbe.successThreshold`                         | inimum consecutive successes for the probe to be considered successful after having failed | `1` |
| `resources.limits`                                       | The resources limits for the Firely Server container | `{}`|
| `resources.requests`                                     | The requested resources for the Firely Server container | `{}` |



### Traffic Exposure Parameters
 Name                                                      | Description                                                               | Default                                             |
| -------------------------------------------------------- | ------------------------------------------------------------------------- | --------------------------------------------------- |
| `service.type`                                           | Kubernetes Service type                                                   | `LoadBalancer` |
| `service.port`                                           | The port which the Kubernetes service will expose the Firely Server pod   | `80` |
| `ingress.enabled`                                        | Enable ingress record generation for Firely Server                        | `false` |
| `ingress.annotations`                                    | Annotations for this host's ingress record                                | `{}` |
| `ingress.ingressClass`                                   | IngressClass that will be be used to implement the Ingress                | `nginx` |
| `ingress.certIssuer`                                     | The name of the cert-manager                                              | `letsencrypt-production` |
| `ingress.path`                                           | Path within the url structure                                             | `/` |
| `ingress.pathType`                                       | Ingress path type                                                         | `Prefix` |
| `ingress.hosts[0]`                                       | Hostname to your Firely Server installation                               | `nil` |
| `ingress.tls[0].secretName`                              | The secretname used in the cert-manager                                   | `nil` |

### Persistence Parameters
 Name                                                      | Description                                                               | Default                                             |
| -------------------------------------------------------- | ------------------------------------------------------------------------- | --------------------------------------------------- |
| `persistence.enabled`                                    | When set to `true` a mount would be available in the Firely Server pod on path `/var/run/vonk` | `false` |
| `persistence.existingPVClaim`                            | The name of an existing PVC to use for persistence                        | `nil` |

### Firely Server Validation service Parameters

This validation service is experimental and is not supported yet. 

 Name                                                      | Description                                                               | Default                                             |
| -------------------------------------------------------- | ------------------------------------------------------------------------- | --------------------------------------------------- |
| `validation.enabled`                            | Enable the Firely Server Validation service. When `false`, Firely Server uses its internal validator, otherwise it will use this validation service| `false` |
| `validation.replicaCount`                       | Number of replicas in the replica set | `1`   |
| `validation.image.registry`                     | Firely Server Validation image name | `firely.azurecr.io`   |
| `validation.image.repository`                   | Firely Server Validation image respository | `firely/firely-cloud-components`   |
| `validation.image.tag`                          | Firely Server Validation image tag | `latest`   |
| `validation.image.pullPolicy`                   | Firely Server Validation image pull policy | `IfNotPresent`   |
| `validation.port`                               | The port which the Kubernetes service will expose the Firely Server Validation pod | `8080`   |
| `validation.persistence.carfilePVClaim`         | The PersistenceVolumeClaim where the carfiles are located  | `nil`   |
| `validation.resources`                          | The resources limits for the Firely Server Validation container | `nil`   |
| `validation.nodeSelector`                       | Allows you to constrain which nodes your pod is eligible to be scheduled on, based on labels on the node  | `{ }`   |
| `validation.affinity`                           | A more elaborate way to constrain which nodes your pod is eligible to be scheduled on| `{ }`   |
| `validation.tolerations`                        | Allow (but do not require) the pods to schedule onto nodes with matching taints | `{ }`   |
| `validationsettings`                            | The override of the appsettings of Firely Server Validation service. This must be a Json format. Do not forget to indent this with 2 white spaces | `{ }` |

## Obtaining and using a license

Download the the license file from [Simplifier.net](https://simplifier.net/firely-server).

Transform the downloaded license in the following format and save it as `firelyserver-license.yaml' 

```yaml
license:
  value: '{"LicenseOptions": {"Kind": "Evaluation","ValidUntil": "2018-11-13","Licensee": "marco@fire.ly"}, "Signature": "+hLwZrrTL4tcW+l0r5yDHYSASM6EWiaVcRBN1..etc"}'
```
> **Note**: the option value should be on one line. No line feeds.

Install the firely-server chart like this:
```console
$ helm install my-release -f .\firelyserver-license.yaml firely/firely-server 
```

## Override Firely Server settings

You can override the default configuration settings of Firely Server by override the appsettings elements, like described in https://docs.simplifier.net/projects/Firely-Server/en/latest/configuration/appsettings.html. 

In the following example the `InformationModel` has been overriden. The default is now `Fhir4.0` instead of `Fhir3.0`, which is the default. Furthermore the minimal loglevel is set to `Information`.

```yaml
vonksettings: 
  {
    "InformationModel":{
      "Default":"Fhir4.0"
    }
  }

logsettings:
  {
    "Serilog": {
      "MinimumLevel": {
        "Default": "Information"
      }
    }
  }
```

> **Note**: be carefull to indent the json snippet correctly! The first curly bracket should have been indented with 2 white spaces. The next element (in our example `"InformationModel"`) should be prefixed with 4 white spaces, etc.

> **Note**: don't use double forward slashes (`//`) to comment out sections. JSON formally has no notion of comments.

Install the firely-server Chart like this:
```console
$ helm install my-release -f .\settings.yaml firely/firely-server 
```