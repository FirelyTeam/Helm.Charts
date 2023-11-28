# Firely Auth Helm Chart

## Prerequisites Details

* Kubernetes 1.19+
* PV support on the underlying infrastructure.
* Firely Auth license

## Chart Details

This chart implements a [Firely Auth](https://fire.ly/products/firely-auth/) deployment on a [Kubernetes](https://kubernetes.io) cluster using the [Helm](https://helm.sh/) package manager.

## Installing the Chart

To install the chart with the release name `my-release`:

``` console
$ helm repo add firely-auth https://raw.githubusercontent.com/FirelyTeam/Helm.Charts/main/firely-auth/
$ helm install my-release firely-auth/firely-auth
```
The command deploys Firely Auth on the Kubernetes cluster in the default configuration. The configuration section lists the parameters that can be configured during installation.

## Uninstalling the Chart
To uninstall/delete the `my-release` deployment:

``` console 
$ helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Configuration

The following table lists the configurable parameters of the firely-auth chart and their default values.

### Global parameters

 Name                                                      | Description                                                               | Default                                             |
| -------------------------------------------------------- | ------------------------------------------------------------------------- | --------------------------------------------------- |
| `image.repository`                                       | Firely Auth image name                                                  | `firely/auth`                                   |
| `image.tag`                                              | Firely Auth image tag                                                   | `3.0.0`                                             |
| `image.pullPolicy`                                       | Firely Auth image pull policy                                           |`IfNotPresent`                                       |

### Common parameters
 Name                                                      | Description                                                               | Default                                             |
| -------------------------------------------------------- | ------------------------------------------------------------------------- | --------------------------------------------------- |
| `nameOverride`                                           | String to partially override firely-auth.fullname template (will maintain the release name) | `""`
| `fullnameOverride`                                       | String to fully override firely--auth.fullname template | `""` |

### Security parameters

 Name                                                      | Description                                                               | Default                                             |
| -------------------------------------------------------- | ------------------------------------------------------------------------- | --------------------------------------------------- |
| `podSecurityContext`                                      | Security settings for the Pod, like `faGroup`.                           | `{}`                                   |
| `securityContext`                                         | Security settings for the container, like `allowPrivilegeEscalation` and `capabilities`.                                                   | `{}`                                             |

### Firely Auth parameters
 Name                                                      | Description                                                               | Default                                             |
| -------------------------------------------------------- | ------------------------------------------------------------------------- | --------------------------------------------------- |
| `appsettings`                                           | The override of the appsettings of Firely Auth. This must be a Json format. Do not forget to indent this with 2 white spaces. See also more information below.  | `{ }` |
| `logsettings`                                            | The override of the logsettings of Firely Auth. This must be a Json format.  Do not forget to indent this with 2 white spaces. See also more information below.| `{ }` |
| `license.secretName`                                     | You can  save the Firely Auth license key yourself as a Kubernetes secret. Use then the name of that secret here. This setting overrides `license.value`| `nil` |
| `license.fileName`                                       | The name of the license file used internally. No reason to change that. | `firely-auth-license.json` |
| `license.value`                                          | The content of the license. See also more information below. | `nil` |


### Firely Auth deployment parameters 
 Name                                                      | Description                                                               | Default                                             |
| -------------------------------------------------------- | ------------------------------------------------------------------------- | --------------------------------------------------- |
| `replicaCount`                                           | Number of replicas in the replica set                                     | `1`                                                 |
| `resources.limits`                                       | The resources limits for the Firely Auth container | `{}`|
| `resources.requests`                                     | The requested resources for the Firely Auth container | `{}` |



### Traffic Exposure Parameters
 Name                                                      | Description                                                               | Default                                             |
| -------------------------------------------------------- | ------------------------------------------------------------------------- | --------------------------------------------------- |
| `service.type`                                           | Kubernetes Service type                                                   | `ClusterIP` |
| `service.port`                                           | The port which the Kubernetes service will expose the Firely Auth pod     | `80` |
| `ingress.enabled`                                        | Enable ingress record generation for Firely Auth                          | `false` |
| `ingress.annotations`                                    | Annotations for this host's ingress record                                | `{}` |
| `ingress.ingressClass`                                   | IngressClass that will be be used to implement the Ingress                | `nginx` |
| `ingress.certIssuer`                                     | The name of the cert-manager                                              | `letsencrypt-production` |
| `ingress.hosts[0]`                                       | Hostname to your FIrelyAuth installation                                  | `chart-example.local` |
| `ingress.hosts[0].path`                                  | Path within the url structure                                             | `/` |
| `ingress.hosts[0].pathType`                              | Ingress path type                                                         | `ImplementationSpecific` |
| `ingress.tls[0].secretName`                              | The secretname used in the cert-manager                                   | `nil` |

### Service Account parameters
 Name                                                      | Description                                                               | Default                                             |
| -------------------------------------------------------- | ------------------------------------------------------------------------- | --------------------------------------------------- |
| `serviceAccount.create`                                  | Specifies whether a service account should be created                     | `true` |
| `serviceAccount.annotations`                             | Annotations to add to the service account                                 | `{}` |
| `serviceAccount.name`                                    | The name of the service account to use. If not set and create is true, a name is generated using the fullname template.                    | `""` |



## Obtaining and using a license

Download the the license file from [Simplifier.net](https://simplifier.net/firely-auth).

Transform the downloaded license in the following format and save it as `firely-auth-license.yaml' 

```yaml
license:
  value: '{"LicenseOptions": {"Kind": "Evaluation","ValidUntil": "2018-11-13","Licensee": "examplefire.ly"}, "Signature": "+hLwZrrTL4tcW+l0r5yDHYSASM6EWiaVcRBN1..etc"}'
```
> **Note**: the option value should be on one line. No line feeds.

Install the firely-auth chart like this:
```console
$ helm install my-release -f .\firely-auth-license.yaml firely-auth/firely-auth
```