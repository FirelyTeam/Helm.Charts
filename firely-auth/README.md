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
| `license.mountPath`                                      | The folder where the license is mounted. | `/var/run/license` |
| `license.mountedFromExtraVolumes`                        | If `true`, the license file is mounted from an extra volume, ignoring `license.Value` and `license.secretName` in this case. See more information below. | `false` |
| `license.value`                                          | The content of the license. See also more information below. | `nil` |
| `envFromSecret`                                          | The name of an existing secret in the same kubernetes namespace which contains key-value pairs which are converted into environment variables of the Firely Auth container. This is the recommended approach for specifying all confidential settings, like connection strings, etc. For example, run the following to create a secret containing a connection string for MSSQL: `kubectl create secret generic -n ${FA_NAMESPACE} fa-env-secret  --from-literal=FIRELY_AUTH_UserStore__SqlServer__ConnectionString='User Id=fauser;...'`| `nil` |
| `extraVolumes`                                           | Array of additional volumes to be mounted in the Firely Auth container | `[]` |
| `extraMountPoints`                                       | Array of additional mount points for the Firely Auth container | `[]` |


### Firely Auth deployment parameters 
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
| `resources.limits`                                       | The resources limits for the Firely Auth container | `{}`|
| `resources.requests`                                     | The requested resources for the Firely Auth container | `{}` |



### Traffic Exposure Parameters
 Name                                                      | Description                                                               | Default                                             |
| -------------------------------------------------------- | ------------------------------------------------------------------------- | --------------------------------------------------- |
| `service.type`                                           | Kubernetes Service type                                                   | `ClusterIP` |
| `service.port`                                           | The port which the Kubernetes service will expose the Firely Auth pod     | `80` |
| `ingress.enabled`                                        | Enable ingress record generation for Firely Auth                          | `false` |
| `ingress.annotations`                                    | Annotations for this host's ingress record                                | `{}` |
| `ingress.className`                                      | Ingress Class Name that will be be used to implement the Ingress          | `nginx` |
| `ingress.certIssuer`                                     | The name of the cert-manager                                              | `letsencrypt-production` |
| `ingress.hosts[0]`                                       | Hostname to your FIrelyAuth installation                                  | `chart-example.local` |
| `ingress.hosts[0].path`                                  | Path within the url structure                                             | `/` |
| `ingress.hosts[0].pathType`                              | Ingress path type                                                         | `ImplementationSpecific` |
| `ingress.tls[0].secretName`                              | The secretname used in the cert-manager                                   | `nil` |

### Persistence Parameters
 Name                                                      | Description                                                               | Default                                             |
| -------------------------------------------------------- | ------------------------------------------------------------------------- | --------------------------------------------------- |
| `persistence.enabled`                                    | When set to `true` a mount would be available in the Firely Auth pod on path `/var/run/fa` | `false` |
| `persistence.existingPVClaim`                            | The name of an existing PVC to use for persistence                        | `nil` |


### Service Account parameters
 Name                                                      | Description                                                               | Default                                             |
| -------------------------------------------------------- | ------------------------------------------------------------------------- | --------------------------------------------------- |
| `serviceAccount.create`                                  | Specifies whether a service account should be created                     | `true` |
| `serviceAccount.annotations`                             | Annotations to add to the service account                                 | `{}` |
| `serviceAccount.name`                                    | The name of the service account to use. If not set and create is true, a name is generated using the fullname template.                    | `""` |



## Obtaining and using a license

Download the the license file from [Simplifier.net](https://simplifier.net/firely-auth).

Once you have a valid license file (for example `license.json`), you have multiple options for deploying it.

### Using helm value `license.value`

Transform the downloaded license in the following format and save it as `firely-auth-license.yaml' 

```yaml
license:
  value: '{"LicenseOptions": {"Kind": "Evaluation","ValidUntil": "2018-11-13","Licensee": "marco@fire.ly"}, "Signature": "+hLwZrrTL4tcW+l0r5yDHYSASM6EWiaVcRBN1..etc"}'
```
> **Note**: the option value should be on one line. No line feeds.

Install the firely-auth chart like this:
```console
$ helm install my-release -f .\firely-auth-license.yaml firely-auth/firely-auth 
```

### Using a secret 
Create a secret in the Firely Auth namespace with the following command:
```SH
kubectl create secret generic firely-auth-license --namespace ${FA_NAMESPACE} --from-file=firely-auth-license.json=./license.json`
```
Then, modify the `values.yaml` to add the following settings:
```YAML
...
license:
  secretName: firely-auth-license
...
```

### Using a keyvault
See the dedicated keyvault section below.

## Override Firely Auth settings
There are multiple ways to specify Firely Server settings.
### Use `appsettings`
You can override the default configuration settings of Firely Auth by overriding the appsettings elements, like described in https://docs.fire.ly/projects/Firely-Server/en/latest/security/firely-auth/firely-auth-settings.html. 

Similarly, you can overwrite the log settings by specifying the `logsettings` entry.

In the following, the Firely Server endpoint has been overwritten.

```yaml
appsettings: 
  {
    "FhirServer": {
      "Name": "Firely Server", 
      "FHIR_BASE_URL": "https://secure.server.playground.fire.ly",
      "IntrospectionSecret": "mysecret"
    },
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

> **Note**: be carefull to indent the json snippet correctly! The first curly bracket should have been indented with 2 white spaces. The next element (in our example `"FhirServer"`) should be prefixed with 4 white spaces, etc.

> **Note**: don't use double forward slashes (`//`) to comment out sections. JSON formally has no notion of comments.

Install the firely-auth Chart like this:
```console
$ helm install my-release -f .\settings.yaml firely-auth/firely-auth 
```

### Use a secret
For confidential settings, you mmight want to use a secret managed differently that the other settings. For this, you need to create a secret
in the Firely Auth namespace using a command similar to 
```SH
kubectl create secret generic -n ${FA_NAMESPACE} firely-auth-env-secret --from-literal=FIRELY_AUTH_SqlDbOptions__ConnectionString='User Id=...'`
```
And add the following settings in you `values.yaml`:
```YAML
envFromSecret: "firely-auth-env-secret"
```
Then, the environment variable `FIRELY_AUTH_SqlDbOptions__ConnectionString` will be set in the Firely Server container, thereby overriding the default value from the `appsettings.json`.

Note that the secret could also contain additional settings.

### Use an Azure Keyvault
See below.


## Using Azure keyvault
By using an Azure keyvault, you can retrieve artifacts directly from a Keyvault and either mount it inside the Firely Server container or access them through using environment variables.
In order to achieve that, some pre-requisites must be fulfilled. You can find more details in [Azure doc](https://learn.microsoft.com/en-us/azure/aks/csi-secrets-store-identity-access).
Below, we will describe how the Azure keyvault could be used to retrieve the Firely license as well as the  connection string for accessing the MS SQL database. This can be extended to access additional confidential settings used in Firely Server.

First, you need to enable the keyvault add-in in the AKS cluster:
```SH
az aks enable-addons --resource-group $RESOURCE_GROUP --name $CLUSTER_NAME --addons azure-keyvault-secrets-provider
```
Then, you need to allow the client identity associated to the keyvault add-in to access your keyvault.
This can be done using a script similar to the following:
```SH
$kvProviderClientId = az aks show --resource-group $RESOURCE_GROUP  --name $CLUSTER_NAME --query addonProfiles.azureKeyvaultSecretsProvider.identity.clientId -o tsv
Write-Host "Identity Client Id of the user identity associated to keyvault add-on: $kvProviderClientId"
Write-Host "Adding the roles to access the Keyvault to the user client identity associated to keyvault ($kvProviderClientId)..."

foreach ($role in @("Key Vault Secrets User", "Key Vault Certificate User", "Key Vault Crypto User")) {
   $roleExists = az role assignment list --role $role --assignee $kvProviderClientId --scope $keyvaultId --query "[].id | [0]"
   if ($roleExists) {
      Write-Host "Role '$role' already assigned to '$kvProviderClientId'"
   }
   else {
      Write-Host "Role '$role' is not yet assigned to '$kvProviderClientId'. Assigning it..."
      az role assignment create --role "$role" --assignee $kvProviderClientId --scope $keyvaultId
      if ($? -eq $false) {
         Write-Host "Error assigning role '$role'."
      }
      else {
         Write-Host "Role '$role' assigned to '$kvProviderClientId'"
      }
   }
}
```

Once the keyvault add-on is enabled and the associated client has access to your keyvault, you need to 
deploy a `SescretProviderClass` in the Firely Server namespace, using the following command:
```SH
kubectl apply -n $FS_NAMESPACE ./azure_kv_spc.yaml
```
with `./azure_kv_spc.yaml` being similar to the following:
```YAML
apiVersion: secrets-store.csi.x-k8s.io/v1
kind: SecretProviderClass
metadata:
  name: $name
spec:
  provider: azure
  parameters:
    usePodIdentity: "false"
    useVMManagedIdentity: "true"
    userAssignedIdentityID: "<KEYVAULT_PROVIDER_CLIENTID>"
    keyvaultName: "<KEYVAULT_NAME>"
    cloudName: "AzurePublicCloud"   
    objects:  |
      array:
        - |
          objectName: <FA_MSSQL_CONNECTION_STRING_KEYVAULT_OBJECTNAME>
          objectType: secret
          objectVersion: "" 
        - |
          objectName: <FA_LICENSE_KEYVAULT_OBJECTNAME>
          objectType: secret
          objectVersion: "" 
    tenantId: "$tenantId"
  secretObjects:
      - secretName: firely-keyvault-secrets
        type: Opaque
        data:
          - key: <FA_MSSQL_CONNECTION_STRING_SECRET_DATAKEY>
            objectName: <FA_MSSQL_CONNECTION_STRING_KEYVAULT_OBJECTNAME>
"@
```
with:
- `<KEYVAULT_NAME>` is the name of the Azure keyvault
- `<KEYVAULT_PROVIDER_CLIENTID>` is the id of the client associated to the Keyvault add-in
- `<FA_LICENSE_KEYVAULT_OBJECTNAME>` is the name of the secret where the license content is stored in the Azure Keyvault
- `<FA_MSSQL_CONNECTION_STRING_KEYVAULT_OBJECTNAME>` is the name of the secret where the MSSQL connection string is stored in the Azure Keyvault
- `<FA_MSSQL_CONNECTION_STRING_SECRET_DATAKEY>` is the name of the entry in the Kubernetes secret created by the `SecretProviderClass`


In order to access the license stored in the keyvault from the Firely Auth container, you need to specify that the license file should be read from a custom mount point corresponding to a custom volume by adding the following entries in your `values.yaml` used in the helm deployment:
```YAML
...
license:
  filename: <FA_LICENSE_KEYVAULT_OBJECTNAME>
  mountPath: /mnt/keyvault_secrets_store
  mountedFromExtraVolumes: true
...
extraVolumes:
  - name: keyvault-secrets-store
    csi:
      driver: secrets-store.csi.k8s.io
      readOnly: true
      volumeAttributes:
        secretProviderClass: "firely-keyvault"
extraMountPoints:
  - name: keyvault-secrets-store
    mountPath: /mnt/keyvault_secrets_store
    readOnly: true
...
```

In order to load the MS SQL connection string from Firely Server, add the following entries in your `values.yaml`:
```YAML
...
extraEnvs:
...
  - name: FIRELY_AUTH_SqlDbOptions__ConnectionString
    valueFrom:
      secretKeyRef:
        key: <FA_MSSQL_CONNECTION_STRING_SECRET_DATAKEY>
        name: firely-keyvault-secrets
...
```
