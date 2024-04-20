# Kubectyl Panel And Kuber Helm Chart

## Configuration

### Global Values

| Key  | Type | Default | Description |
| :--: | :-----------------: | :-----: | ----------- |
| `global.timezone` | `string` | `UTC` | Timezone for the panel. |

---

### Ingress

| Key  | Type | Default | Description |
| :--: | :-----------------: | :-----: | ----------- |
| `ingress.class` | `string` | `nginx` | The Ingress class for routing external traffic to services. |
| `ingress.panel` | `string` | `panel.example.com` | The full FQDN that your panel will be accessible at. |
| `ingress.kuber` | `string` | `kuber.example.com` | The full FQDN that the kuber daemon will be accessible at. |
| `ingress.tls.create` | `bool` | `true` | Boolean to control if the chart should manage the creation of the Certificate resources. This is particularly useful if you have automation around Ingress resources that creates Certificates already. |
| `ingress.tls.clusterIssuer` | `string` | `letsencrypt-prod` | Name of the ClusterIssuer that should be specified on the Ingress resources to create your certificate. Required for most configurations even if not managing the certificate in this chart. |
| `ingress.annotations` | `map(string\|int\|bool)` | `{}` | Map of additional annotations to add to the Ingress resources. |

---

### Panel

| Key  | Type | Default | Description |
| :--: | :-----------------: | :-----: | ----------- |
| `panel.image` | `string` | `quay.io/kubectyl/panel:develop` | The image for the Panel application container. |
| `panel.storageClass` | `string` | `""` | The storage class to use for panel's persistent volume. To use default K8s storage class set this value to "". **This is mutually exclusive with `existingVolumeClaim` and should not be used with it.** |
| `panel.existingVolumeClaim` | `string` | `""` | Name of existing volume claim resource to use for the pod volumes. **This is mutually exclusive with `storageClass` and should not be used with it.** |
| `panel.email` | `string` | `abc@example.com` | The email address for Letsencrypt. Used for panel only as a reference to enable cert-manager. |
| `panel.serviceAnnotations` | `map(string\|int\|bool)` | `{}` | Map of additional annotations to add to the panel's Service resource. |
| `panel.statefulSetAnnotations` | `map(string\|int\|bool)` | `{}` | Map of additional annotations to add to the panel's StatefulSet resource. |

---

### Kuber

| Key  | Type | Default | Description |
| :--: | :-----------------: | :-----: | ----------- |
| `kuber.image` | `string` | `quay.io/kubectyl/kuber:deveop` | The image for the Kuber application container. |
| `kuber.replicaCount` | `int` | `0` | Set to 0. Will be automatically set to 1 by panel after installation. |
| `kuber.serviceAnnotations` | `map(string\|int\|bool)` | `{}` | Map of additional annotations to add to kuber's Service resource. |
| `kuber.deploymentAnnotations` | `map(string\|int\|bool)` | `{}` | Map of additional annotations to add to kuber's Deployment resource. |

---

### MariaDB

| Key  | Type | Default | Description |
| :--: | :-----------------: | :-----: | ----------- |
| `mariadb.create` | `bool` | `true` | Boolean to control creation of mariadb chart resources. Useful if you plan on using an external mariadb instance. |
| `mariadb.global.storageClass` | `string` | `""` | The storage class to use for mariadb's persistent volume. To use default K8s storage class set this value to "". |
| `mariadb.externalHost` | `string` | `""` | Hostname of external mariadb instance if you intend to use one. If using built-in mariadb chart, leave this blank or don't include it at all. |
| `mariadb.volumePermissions.enabled` | `bool` | `true` | Enable init container that changes the owner and group of the persistent volume(s) mountpoint to `runAsUser:fsGroup`. |
| `mariadb.image.debug` | `bool` | `true` | Boolean to control if debug logs should be enabled. |
| `mariadb.auth.database` | `string` | `panel` | Name of mariadb database to use for panel installation. |
| `mariadb.auth.username` | `string` | `kubectyl` | User to authenticate to mariadb with. |
| `mariadb.auth.password` | `string` | `SecretPassword` | Password for user `mariadb.auth.username`. |
| `mariadb.auth.rootPassword` | `string` | `SuperSecretPassword` | If creating host with chart, password to use for `root` user upon creation. |
| `mariadb.primary.persistence.size` | `(int)Gi` | `1Gi` | The size of the primary mariadb pod's persistent volume. |
| `mariadb.secondary.replicaCount` | `int` | `0` | The number of mariadb replicas to create. |

For more in-depth explanation of the configuration and additional options you can specify to the `mariadb` chart, please see [Bitnami's documentation](https://github.com/bitnami/charts/tree/main/bitnami/mariadb).

---

### Redis

| Key  | Type | Default | Description |
| :--: | :-----------------: | :-----: | ----------- |
| `redis.create` | `bool` | `true` | Boolean to control creation of redis chart resources. Useful if you plan on using an external redis instance. |
| `redis.global.storageClass` | `string` | `""` | The storage class to use for the redis persistent volume. To use default K8s storage class set this value to "". |
| `redis.externalHost` | `string` | `""` | Hostname of external redis instance if you intend to use one. If using built-in redis chart, leave this blank or don't include it at all. |
| `redis.auth.enabled` | `bool` | `false` | Boolean to control whether we should try to authenticate when connecting to redis. |
| `redis.auth.password` | `string` | `""` | Password to use for redis authentication. |
| `redis.master.persistence.size` | `(int)Gi` | `1Gi` | The size of the master redis pod's persistent volume. |
| `redis.secondary.replicaCount` | `int` | `0` | The number of redis replicas to create. |
| `redis.sentinel.enabled` | `bool` | `false` | Boolean to enable redis sentinel for high availability. |

For more in-depth explanation of the configuration and additional options you can specify to the `redis` chart, please see [Bitnami's documentation](https://github.com/bitnami/charts/tree/main/bitnami/redis).

---

### Service Account

| Key  | Type | Default | Description |
| :--: | :-----------------: | :-----: | ----------- |
| `serviceAccount.create` | `bool` | `true` | Boolean to enable the creation of a service account for our services. |
| `serviceAccount.name` | `string` | `""` | Name of service account to create. If not set, a name is generated. |


