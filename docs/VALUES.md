# Values
A continuación se listan todas los posibles valores que acepta el chart.

## global

| Clave | Tipo | Valor por defecto | Descripción|
| -- | -- | -- | --|
|`global.storageClassName`|`string`|"csi-cephfs-sc"|Nombre del proveedor de PV, es necesario adaptarlo si se despliega fuera de la infraestructura de GNOSS|
|`global.hosts.services`|`string`||Dominio (sin esquema) en el que se desea que respondan los servicios del proyecto, debe definirse en cada proyecto y entorno.|
|`global.hosts.web`|`string`||Dominio (sin esquema) en el que se desea que respondan la web del proyecto, debe definirse en cada proyecto y entorno.|
|`global.hosts.basicAuth`|`boolean`||Indica si esta activa la autenticación básica en el dominio de la web, por defecto esta desactivada|

## secrets
| Clave | Tipo | Valor por defecto | Descripción|
| -- | -- | -- | --|
|`secrets.sql.name`|`string`|sql-secret|Nombre del secreto de K8s donde se encuentran las distintas conexiones de base de datos SQL|
|`secrets.sql.acid.secretKey`|`string`|acidConnectionString|Clave del secreto cuyo valor es la cadena de conexión a la base de datos ACID|
|`secrets.sql.base.secretKey`|`string`|baseConnectionString|Clave del secreto cuyo valor es la cadena de conexión a la base de datos BASE|
|`secrets.sql.oauth.secretKey`|`string`|oauthConnectionString|Clave del secreto cuyo valor es la cadena de conexión a la base de datos OAUTH|
|`secrets.virtuoso.name`|`string`|virtuoso-secret|Nombre del secreto de K8s donde se encuentran las distintas conexiones a Virtuoso|
|`secrets.virtuoso.read.secretKey`|`string`|virtuosoRead|Clave del secreto cuyo valor es la cadena de conexión al balanceador de Virtuoso para lectura|
|`secrets.virtuoso.readHome.secretKey`|`string`|virtuosoReadHome|Clave del secreto cuyo valor es la cadena de conexión de lectura a Virtuoso, en el caso de usar un Virtuoso específico para la home|
|`secrets.virtuoso.writeHome.secretKey`|`string`|virtuosoWriteHome|Clave del secreto cuyo valor es la cadena de conexión de escritura a Virtuoso, en el caso de usar un Virtuoso específico para la home|
|`secrets.virtuoso.write1.secretKey`|`string`|virtuosoWrite1|Clave del secreto cuyo valor es la cadena de conexión de escritura al Virtuoso 1|
|`secrets.virtuoso.write2.secretKey`|`string`|virtuosoWrite2|Clave del secreto cuyo valor es la cadena de conexión de escritura al Virtuoso 2|
|`secrets.rabbitmq.name`|`string`|rabbitmq-secret|Nombre del secreto de K8s donde se encuentra la conexión a RabbitMQ|
|`secrets.rabbitmq.connectionString.secretKey`|`string`|rabbitMQConnectionString|Clave del secreto cuyo valor es la cadena de conexión a RabbitMQ|
|`secrets.identity.name`|`string`|identity-secret|Nombre del secreto de K8s donde se encuentran los valores de configuración de Identity|
|`secrets.identity.scope.secretKey`|`string`|scope|Clave del secreto cuyo valor es el scope del servicio Identity|
|`secrets.identity.clientID.secretKey`|`string`|clientID|Clave del secreto cuyo valor es el clientId del servicio Identity|
|`secrets.identity.clientSecret.secretKey`|`string`|clientSecret|Clave del secreto cuyo valor es la secretKey del servicio Identity|
|`secrets.tls.secretName`|`string`|tls-secret|Nombre del secreto de K8s donde se encuentran los datos del certificado TLS|

## ingress
| Clave | Tipo | Valor por defecto | Descripción|
| -- | -- | -- | --|
|`ingress.https`|`bool`|true|Indica si el ingress debe usar HTTPS|
|`ingress.schema_servicios`|`string`|"https"|Esquema utilizado para las URLs de los servicios internos|

## general
| Clave | Tipo | Valor por defecto | Descripción|
| -- | -- | -- | --|
|`general.tag`|`string`|"6.1.19"|Versión general de la aplicación|
|`general.extraTolerations`|`list`|null|Lista de toleraciones adicionales que se añadirán a las toleraciones específicas de cada servicio|
|`general.extraAffinity`|`object`|null|Reglas de afinidad adicionales que se combinarán con las reglas de afinidad específicas de cada servicio|
|`general.imagePullSecrets`|`list`|null|Lista de secretos de K8s usados para autenticarse en registros de imágenes privados|
|`general.user`|`int`| ID del usuario con el que se ejecuta el contenedor, debe coincidir con el mismo que usa la imagen. No debería cambiarse a no ser que cambie en la imagen|
|`general.group`|`int`| ID del grupo con el que se ejecuta el contenedor, debe coincidir con el mismo que usa la imagen. No debería cambiarse a no ser que cambie en la imagen|
|`general.language`|`string`| Idiomas disponibles en la plataforma con el formato `clave_idioma1|nombre_idioma1,clave_idioma2|nombre_idioma2`los idiomas deben ser de entre los 10 soportados por la plataforma|
|`general.apiPort`|`int`|8080|Puerto que recibe el tráfico API. Publicado en ingress / reverse proxy|
|`general.managementPort`|`int`|8081|Solo red interna del clúster. Se utiliza para las comprobaciones liveness y readiness de las aplicaciones|
|`general.healthChecksEnabled`|`bool`|true|Indica si se configuran los `livenessProbe`/`readinessProbe` en los Deployments/StatefulSets|

## web
| Clave | Tipo | Valor por defecto | Descripción|
| -- | -- | -- | --|
|`web.enabled`|`bool`|true|Indica si el servicio está habilitado. Si es `false`, no se crean sus recursos en Kubernetes (Deployment/StatefulSet, ConfigMap, Service, Ingress)|
|`web.replicas`|`int`|1|Número de réplicas del servicio|
|`web.image`|`string`|"gnoss/gnoss.web.enterprise"|Imagen Docker del servicio|
|`web.tag`|`string`|""|Tag de la imagen Docker|
|`web.pullPolicy`|`string`|IfNotPresent|Política de descarga de la imagen|
|`web.limitsEnabled`|`bool`|true|Indica si se aplican límites de recursos|
|`web.limitMemory`|`string`|6000Mi|Límite de memoria del contenedor|
|`web.limitCpu`|`string`|4|Límite de CPU del contenedor|
|`web.requestMemory`|`string`|300Mi|Memoria solicitada por el contenedor|
|`web.requestCpu`|`string`|200m|CPU solicitada por el contenedor|
|`web.restartPolicy`|`string`|Always|Política de reinicio del pod|
|`web.tolerations`|`list`|null|Toleraciones específicas del servicio|
|`web.affinity`|`object`|null|Reglas de afinidad específicas del servicio|
|`web.logs.enabled`|`bool`|true|Indica si se habilita el volumen de logs|
|`web.logs.size`|`string`|"1Gi"|Tamaño del volumen de logs|
|`web.logs.path`|`string`|"/app/logs"|Ruta del volumen de logs dentro del contenedor|
|`web.robotsTxt`|`string`|`"User-agent: *`<br>`Disallow: /"`|Contenido del archivo `robots.txt`|

## login
| Clave | Tipo | Valor por defecto | Descripción|
| -- | -- | -- | --|
|`login.enabled`|`bool`|true|Indica si el servicio está habilitado. Si es `false`, no se crean sus recursos en Kubernetes (Deployment/StatefulSet, ConfigMap, Service, Ingress)|
|`login.replicas`|`int`|1|Número de réplicas del servicio|
|`login.image`|`string`|"gnoss/gnoss.login.enterprise"|Imagen Docker del servicio|
|`login.tag`|`string`|""|Tag de la imagen Docker|
|`login.pullPolicy`|`string`|IfNotPresent|Política de descarga de la imagen|
|`login.limitsEnabled`|`bool`|true|Indica si se aplican límites de recursos|
|`login.limitMemory`|`string`|600Mi|Límite de memoria del contenedor|
|`login.limitCpu`|`string`|2|Límite de CPU del contenedor|
|`login.requestMemory`|`string`|200Mi|Memoria solicitada por el contenedor|
|`login.requestCpu`|`string`|100m|CPU solicitada por el contenedor|
|`login.restartPolicy`|`string`|Always|Política de reinicio del pod|
|`login.tolerations`|`list`|null|Toleraciones específicas del servicio|
|`login.affinity`|`object`|null|Reglas de afinidad específicas del servicio|

## facetas
| Clave | Tipo | Valor por defecto | Descripción|
| -- | -- | -- | --|
|`facetas.enabled`|`bool`|true|Indica si el servicio está habilitado. Si es `false`, no se crean sus recursos en Kubernetes (Deployment/StatefulSet, ConfigMap, Service, Ingress)|
|`facetas.replicas`|`int`|1|Número de réplicas del servicio|
|`facetas.image`|`string`|"gnoss/gnoss.facets.enterprise"|Imagen Docker del servicio|
|`facetas.tag`|`string`|""|Tag de la imagen Docker|
|`facetas.pullPolicy`|`string`|IfNotPresent|Política de descarga de la imagen|
|`facetas.limitsEnabled`|`bool`|true|Indica si se aplican límites de recursos|
|`facetas.limitMemory`|`string`|800Mi|Límite de memoria del contenedor|
|`facetas.limitCpu`|`string`|2|Límite de CPU del contenedor|
|`facetas.requestMemory`|`string`|200Mi|Memoria solicitada por el contenedor|
|`facetas.requestCpu`|`string`|100m|CPU solicitada por el contenedor|
|`facetas.restartPolicy`|`string`|Always|Política de reinicio del pod|
|`facetas.tolerations`|`list`|null|Toleraciones específicas del servicio|
|`facetas.affinity`|`object`|null|Reglas de afinidad específicas del servicio|

## results
| Clave | Tipo | Valor por defecto | Descripción|
| -- | -- | -- | --|
|`results.enabled`|`bool`|true|Indica si el servicio está habilitado. Si es `false`, no se crean sus recursos en Kubernetes (Deployment/StatefulSet, ConfigMap, Service, Ingress)|
|`results.replicas`|`int`|1|Número de réplicas del servicio|
|`results.image`|`string`|"gnoss/gnoss.results.enterprise"|Imagen Docker del servicio|
|`results.tag`|`string`|""|Tag de la imagen Docker|
|`results.pullPolicy`|`string`|IfNotPresent|Política de descarga de la imagen|
|`results.limitsEnabled`|`bool`|true|Indica si se aplican límites de recursos|
|`results.limitMemory`|`string`|800Mi|Límite de memoria del contenedor|
|`results.limitCpu`|`string`|2|Límite de CPU del contenedor|
|`results.requestMemory`|`string`|200Mi|Memoria solicitada por el contenedor|
|`results.requestCpu`|`string`|100m|CPU solicitada por el contenedor|
|`results.restartPolicy`|`string`|Always|Política de reinicio del pod|
|`results.tolerations`|`list`|null|Toleraciones específicas del servicio|
|`results.affinity`|`object`|null|Reglas de afinidad específicas del servicio|

## autocompletar
| Clave | Tipo | Valor por defecto | Descripción|
| -- | -- | -- | --|
|`autocompletar.enabled`|`bool`|true|Indica si el servicio está habilitado. Si es `false`, no se crean sus recursos en Kubernetes (Deployment/StatefulSet, ConfigMap, Service, Ingress)|
|`autocompletar.replicas`|`int`|1|Número de réplicas del servicio|
|`autocompletar.image`|`string`|"gnoss/gnoss.autocomplete.enterprise"|Imagen Docker del servicio|
|`autocompletar.tag`|`string`|""|Tag de la imagen Docker|
|`autocompletar.pullPolicy`|`string`|IfNotPresent|Política de descarga de la imagen|
|`autocompletar.limitsEnabled`|`bool`|true|Indica si se aplican límites de recursos|
|`autocompletar.limitMemory`|`string`|500Mi|Límite de memoria del contenedor|
|`autocompletar.limitCpu`|`string`|2|Límite de CPU del contenedor|
|`autocompletar.requestMemory`|`string`|100Mi|Memoria solicitada por el contenedor|
|`autocompletar.requestCpu`|`string`|100m|CPU solicitada por el contenedor|
|`autocompletar.restartPolicy`|`string`|Always|Política de reinicio del pod|
|`autocompletar.tolerations`|`list`|null|Toleraciones específicas del servicio|
|`autocompletar.affinity`|`object`|null|Reglas de afinidad específicas del servicio|

## despliegues
| Clave | Tipo | Valor por defecto | Descripción|
| -- | -- | -- | --|
|`despliegues.enabled`|`bool`|true|Indica si el servicio está habilitado. Si es `false`, no se crean sus recursos en Kubernetes (Deployment/StatefulSet, ConfigMap, Service, Ingress)|
|`despliegues.replicas`|`int`|1|Número de réplicas del servicio|
|`despliegues.image`|`string`|"gnoss/gnoss.deploy.enterprise"|Imagen Docker del servicio|
|`despliegues.tag`|`string`|""|Tag de la imagen Docker|
|`despliegues.pullPolicy`|`string`|IfNotPresent|Política de descarga de la imagen|
|`despliegues.limitsEnabled`|`bool`|true|Indica si se aplican límites de recursos|
|`despliegues.limitMemory`|`string`|500Mi|Límite de memoria del contenedor|
|`despliegues.limitCpu`|`string`|2|Límite de CPU del contenedor|
|`despliegues.requestMemory`|`string`|100Mi|Memoria solicitada por el contenedor|
|`despliegues.requestCpu`|`string`|100m|CPU solicitada por el contenedor|
|`despliegues.restartPolicy`|`string`|Always|Política de reinicio del pod|
|`despliegues.tolerations`|`list`|null|Toleraciones específicas del servicio|
|`despliegues.affinity`|`object`|null|Reglas de afinidad específicas del servicio|

## api
| Clave | Tipo | Valor por defecto | Descripción|
| -- | -- | -- | --|
|`api.enabled`|`bool`|true|Indica si el servicio está habilitado. Si es `false`, no se crean sus recursos en Kubernetes (Deployment/StatefulSet, ConfigMap, Service, Ingress)|
|`api.replicas`|`int`|1|Número de réplicas del servicio|
|`api.image`|`string`|"gnoss/gnoss.api.enterprise"|Imagen Docker del servicio|
|`api.tag`|`string`|""|Tag de la imagen Docker|
|`api.pullPolicy`|`string`|IfNotPresent|Política de descarga de la imagen|
|`api.limitsEnabled`|`bool`|true|Indica si se aplican límites de recursos|
|`api.limitMemory`|`string`|1Gi|Límite de memoria del contenedor|
|`api.limitCpu`|`string`|4|Límite de CPU del contenedor|
|`api.requestMemory`|`string`|125Mi|Memoria solicitada por el contenedor|
|`api.requestCpu`|`string`|100m|CPU solicitada por el contenedor|
|`api.restartPolicy`|`string`|Always|Política de reinicio del pod|
|`api.tolerations`|`list`|null|Toleraciones específicas del servicio|
|`api.affinity`|`object`|null|Reglas de afinidad específicas del servicio|

## oauth
| Clave | Tipo | Valor por defecto | Descripción|
| -- | -- | -- | --|
|`oauth.enabled`|`bool`|true|Indica si el servicio está habilitado. Si es `false`, no se crean sus recursos en Kubernetes (Deployment/StatefulSet, ConfigMap, Service, Ingress)|
|`oauth.replicas`|`int`|1|Número de réplicas del servicio|
|`oauth.image`|`string`|"gnoss/gnoss.oauth.enterprise"|Imagen Docker del servicio|
|`oauth.tag`|`string`|""|Tag de la imagen Docker|
|`oauth.debugNodePort`|`int`|30011|NodePort expuesto para depuración remota|
|`oauth.pullPolicy`|`string`|IfNotPresent|Política de descarga de la imagen|
|`oauth.rateLimitPermitLimit`|`string`|"2000"|Número máximo de peticiones permitidas por ventana de rate limiting en el servicio OAuth|
|`oauth.limitsEnabled`|`bool`|true|Indica si se aplican límites de recursos|
|`oauth.limitMemory`|`string`|800Mi|Límite de memoria del contenedor|
|`oauth.limitCpu`|`string`|2|Límite de CPU del contenedor|
|`oauth.requestMemory`|`string`|125Mi|Memoria solicitada por el contenedor|
|`oauth.requestCpu`|`string`|100m|CPU solicitada por el contenedor|
|`oauth.restartPolicy`|`string`|Always|Política de reinicio del pod|
|`oauth.tolerations`|`list`|null|Toleraciones específicas del servicio|
|`oauth.affinity`|`object`|null|Reglas de afinidad específicas del servicio|

## identityserver
| Clave | Tipo | Valor por defecto | Descripción|
| -- | -- | -- | --|
|`identityserver.enabled`|`bool`|true|Indica si el servicio está habilitado. Si es `false`, no se crean sus recursos en Kubernetes (Deployment/StatefulSet, ConfigMap, Service, Ingress)|
|`identityserver.replicas`|`int`|1|Número de réplicas del servicio|
|`identityserver.image`|`string`|"gnoss/gnoss.identityserver.enterprise"|Imagen Docker del servicio|
|`identityserver.tag`|`string`|""|Tag de la imagen Docker|
|`identityserver.debugNodePort`|`int`|30013|NodePort expuesto para depuración remota|
|`identityserver.pullPolicy`|`string`|IfNotPresent|Política de descarga de la imagen|
|`identityserver.limitsEnabled`|`bool`|true|Indica si se aplican límites de recursos|
|`identityserver.limitMemory`|`string`|500Mi|Límite de memoria del contenedor|
|`identityserver.limitCpu`|`string`|500m|Límite de CPU del contenedor|
|`identityserver.requestMemory`|`string`|100Mi|Memoria solicitada por el contenedor|
|`identityserver.requestCpu`|`string`|100m|CPU solicitada por el contenedor|
|`identityserver.restartPolicy`|`string`|Always|Política de reinicio del pod|
|`identityserver.tolerations`|`list`|null|Toleraciones específicas del servicio|
|`identityserver.affinity`|`object`|null|Reglas de afinidad específicas del servicio|

## deploy
| Clave | Tipo | Valor por defecto | Descripción|
| -- | -- | -- | --|

## etiquetadoautomatico
| Clave | Tipo | Valor por defecto | Descripción|
| -- | -- | -- | --|
|`etiquetadoautomatico.enabled`|`bool`|true|Indica si el servicio está habilitado. Si es `false`, no se crean sus recursos en Kubernetes (Deployment/StatefulSet, ConfigMap, Service, Ingress)|
|`etiquetadoautomatico.replicas`|`int`|1|Número de réplicas del servicio|
|`etiquetadoautomatico.image`|`string`|"gnoss/gnoss.etiquetadoautomatico.enterprise"|Imagen Docker del servicio|
|`etiquetadoautomatico.tag`|`string`|""|Tag de la imagen Docker|
|`etiquetadoautomatico.pullPolicy`|`string`|IfNotPresent|Política de descarga de la imagen|
|`etiquetadoautomatico.limitsEnabled`|`bool`|true|Indica si se aplican límites de recursos|
|`etiquetadoautomatico.limitMemory`|`string`|200Mi|Límite de memoria del contenedor|
|`etiquetadoautomatico.limitCpu`|`string`|2|Límite de CPU del contenedor|
|`etiquetadoautomatico.requestMemory`|`string`|100Mi|Memoria solicitada por el contenedor|
|`etiquetadoautomatico.requestCpu`|`string`|100m|CPU solicitada por el contenedor|
|`etiquetadoautomatico.restartPolicy`|`string`|Always|Política de reinicio del pod|
|`etiquetadoautomatico.tolerations`|`list`|null|Toleraciones específicas del servicio|
|`etiquetadoautomatico.affinity`|`object`|null|Reglas de afinidad específicas del servicio|

## generalstatefulset
| Clave | Tipo | Valor por defecto | Descripción|
| -- | -- | -- | --|
|`generalstatefulset.pullPolicy`|`string`|IfNotPresent|Política de descarga de imagen por defecto para los servicios StatefulSet|

## documents
| Clave | Tipo | Valor por defecto | Descripción|
| -- | -- | -- | --|
|`documents.enabled`|`bool`|true|Indica si el servicio está habilitado. Si es `false`, no se crean sus recursos en Kubernetes (Deployment/StatefulSet, ConfigMap, Service, Ingress)|
|`documents.replicas`|`int`|1|Número de réplicas del servicio|
|`documents.image`|`string`|"gnoss/gnoss.documents.enterprise"|Imagen Docker del servicio|
|`documents.tag`|`string`|""|Tag de la imagen Docker|
|`documents.debugNodePort`|`int`|30009|NodePort expuesto para depuración remota|
|`documents.limitsEnabled`|`bool`|true|Indica si se aplican límites de recursos|
|`documents.limitMemory`|`string`|800Mi|Límite de memoria del contenedor|
|`documents.limitCpu`|`string`|2|Límite de CPU del contenedor|
|`documents.requestMemory`|`string`|125Mi|Memoria solicitada por el contenedor|
|`documents.requestCpu`|`string`|100m|CPU solicitada por el contenedor|
|`documents.restartPolicy`|`string`|Always|Política de reinicio del pod|
|`documents.espacioVol`|`string`|"5Gi"|Tamaño del volumen persistente del StatefulSet|
|`documents.tolerations`|`list`|null|Toleraciones específicas del servicio|
|`documents.affinity`|`object`|null|Reglas de afinidad específicas del servicio|

## interno
| Clave | Tipo | Valor por defecto | Descripción|
| -- | -- | -- | --|
|`interno.enabled`|`bool`|true|Indica si el servicio está habilitado. Si es `false`, no se crean sus recursos en Kubernetes (Deployment/StatefulSet, ConfigMap, Service, Ingress)|
|`interno.replicas`|`int`|1|Número de réplicas del servicio|
|`interno.image`|`string`|"gnoss/gnoss.intern.enterprise"|Imagen Docker del servicio|
|`interno.tag`|`string`|""|Tag de la imagen Docker|
|`interno.debugNodePort`|`int`|30007|NodePort expuesto para depuración remota|
|`interno.limitsEnabled`|`bool`|true|Indica si se aplican límites de recursos|
|`interno.limitMemory`|`string`|800Mi|Límite de memoria del contenedor|
|`interno.limitCpu`|`string`|500m|Límite de CPU del contenedor|
|`interno.requestMemory`|`string`|125Mi|Memoria solicitada por el contenedor|
|`interno.requestCpu`|`string`|100m|CPU solicitada por el contenedor|
|`interno.restartPolicy`|`string`|Always|Política de reinicio del pod|
|`interno.espacioVol`|`string`|"5Gi"|Tamaño del volumen persistente del StatefulSet|
|`interno.tolerations`|`list`|null|Toleraciones específicas del servicio|
|`interno.affinity`|`object`|null|Reglas de afinidad específicas del servicio|

## ontologias
| Clave | Tipo | Valor por defecto | Descripción|
| -- | -- | -- | --|
|`ontologias.enabled`|`bool`|true|Indica si el servicio está habilitado. Si es `false`, no se crean sus recursos en Kubernetes (Deployment/StatefulSet, ConfigMap, Service, Ingress)|
|`ontologias.replicas`|`int`|1|Número de réplicas del servicio|
|`ontologias.image`|`string`|"gnoss/gnoss.ontologies.enterprise"|Imagen Docker del servicio|
|`ontologias.tag`|`string`|""|Tag de la imagen Docker|
|`ontologias.debugNodePort`|`int`|30008|NodePort expuesto para depuración remota|
|`ontologias.limitsEnabled`|`bool`|true|Indica si se aplican límites de recursos|
|`ontologias.limitMemory`|`string`|500Mi|Límite de memoria del contenedor|
|`ontologias.limitCpu`|`string`|500m|Límite de CPU del contenedor|
|`ontologias.requestMemory`|`string`|125Mi|Memoria solicitada por el contenedor|
|`ontologias.requestCpu`|`string`|100m|CPU solicitada por el contenedor|
|`ontologias.restartPolicy`|`string`|Always|Política de reinicio del pod|
|`ontologias.espacioVol`|`string`|"5Gi"|Tamaño del volumen persistente del StatefulSet|
|`ontologias.tolerations`|`list`|null|Toleraciones específicas del servicio|
|`ontologias.affinity`|`object`|null|Reglas de afinidad específicas del servicio|

## generalBack
| Clave | Tipo | Valor por defecto | Descripción|
| -- | -- | -- | --|
|`generalBack.restartPolicy`|`string`|Always|Política de reinicio por defecto para los servicios back|
|`generalBack.pullPolicy`|`string`|IfNotPresent|Política de descarga de imagen por defecto para los servicios back|

## mailservice
| Clave | Tipo | Valor por defecto | Descripción|
| -- | -- | -- | --|
|`mailservice.enabled`|`bool`|true|Indica si el servicio está habilitado. Si es `false`, no se crean sus recursos en Kubernetes (Deployment/StatefulSet, ConfigMap, Service, Ingress)|
|`mailservice.replicas`|`int`|1|Número de réplicas del servicio|
|`mailservice.image`|`string`|"gnoss/gnoss.mail.enterprise"|Imagen Docker del servicio|
|`mailservice.tag`|`string`|""|Tag de la imagen Docker|
|`mailservice.pullPolicy`|`string`|IfNotPresent|Política de descarga de la imagen|
|`mailservice.limitsEnabled`|`bool`|true|Indica si se aplican límites de recursos|
|`mailservice.limitMemory`|`string`|600Mi|Límite de memoria del contenedor|
|`mailservice.limitCpu`|`string`|500m|Límite de CPU del contenedor|
|`mailservice.requestMemory`|`string`|125Mi|Memoria solicitada por el contenedor|
|`mailservice.requestCpu`|`string`|50m|CPU solicitada por el contenedor|
|`mailservice.tolerations`|`list`|null|Toleraciones específicas del servicio|
|`mailservice.affinity`|`object`|null|Reglas de afinidad específicas del servicio|

## cacherefresh
| Clave | Tipo | Valor por defecto | Descripción|
| -- | -- | -- | --|
|`cacherefresh.enabled`|`bool`|true|Indica si el servicio está habilitado. Si es `false`, no se crean sus recursos en Kubernetes (Deployment/StatefulSet, ConfigMap, Service, Ingress)|
|`cacherefresh.replicas`|`int`|1|Número de réplicas del servicio|
|`cacherefresh.image`|`string`|"gnoss/gnoss.cacherefresh.enterprise"|Imagen Docker del servicio|
|`cacherefresh.tag`|`string`|""|Tag de la imagen Docker|
|`cacherefresh.pullPolicy`|`string`|IfNotPresent|Política de descarga de la imagen|
|`cacherefresh.limitsEnabled`|`bool`|true|Indica si se aplican límites de recursos|
|`cacherefresh.limitMemory`|`string`|800Mi|Límite de memoria del contenedor|
|`cacherefresh.limitCpu`|`string`|500m|Límite de CPU del contenedor|
|`cacherefresh.requestMemory`|`string`|125Mi|Memoria solicitada por el contenedor|
|`cacherefresh.requestCpu`|`string`|50m|CPU solicitada por el contenedor|
|`cacherefresh.tolerations`|`list`|null|Toleraciones específicas del servicio|
|`cacherefresh.affinity`|`object`|null|Reglas de afinidad específicas del servicio|

## distributor
| Clave | Tipo | Valor por defecto | Descripción|
| -- | -- | -- | --|
|`distributor.enabled`|`bool`|true|Indica si el servicio está habilitado. Si es `false`, no se crean sus recursos en Kubernetes (Deployment/StatefulSet, ConfigMap, Service, Ingress)|
|`distributor.replicas`|`int`|1|Número de réplicas del servicio|
|`distributor.image`|`string`|"gnoss/gnoss.distributor.enterprise"|Imagen Docker del servicio|
|`distributor.tag`|`string`|""|Tag de la imagen Docker|
|`distributor.pullPolicy`|`string`|IfNotPresent|Política de descarga de la imagen|
|`distributor.limitsEnabled`|`bool`|true|Indica si se aplican límites de recursos|
|`distributor.limitMemory`|`string`|600Mi|Límite de memoria del contenedor|
|`distributor.limitCpu`|`string`|500m|Límite de CPU del contenedor|
|`distributor.requestMemory`|`string`|125Mi|Memoria solicitada por el contenedor|
|`distributor.requestCpu`|`string`|50m|CPU solicitada por el contenedor|
|`distributor.tolerations`|`list`|null|Toleraciones específicas del servicio|
|`distributor.affinity`|`object`|null|Reglas de afinidad específicas del servicio|

## searchgraphgeneration
| Clave | Tipo | Valor por defecto | Descripción|
| -- | -- | -- | --|
|`searchgraphgeneration.enabled`|`bool`|true|Indica si el servicio está habilitado. Si es `false`, no se crean sus recursos en Kubernetes (Deployment/StatefulSet, ConfigMap, Service, Ingress)|
|`searchgraphgeneration.replicas`|`int`|1|Número de réplicas del servicio|
|`searchgraphgeneration.image`|`string`|"gnoss/gnoss.searchgraphgeneration.enterprise"|Imagen Docker del servicio|
|`searchgraphgeneration.tag`|`string`|""|Tag de la imagen Docker|
|`searchgraphgeneration.pullPolicy`|`string`|IfNotPresent|Política de descarga de la imagen|
|`searchgraphgeneration.limitsEnabled`|`bool`|true|Indica si se aplican límites de recursos|
|`searchgraphgeneration.limitMemory`|`string`|800Mi|Límite de memoria del contenedor|
|`searchgraphgeneration.limitCpu`|`string`|500m|Límite de CPU del contenedor|
|`searchgraphgeneration.requestMemory`|`string`|125Mi|Memoria solicitada por el contenedor|
|`searchgraphgeneration.requestCpu`|`string`|50m|CPU solicitada por el contenedor|
|`searchgraphgeneration.tolerations`|`list`|null|Toleraciones específicas del servicio|
|`searchgraphgeneration.affinity`|`object`|null|Reglas de afinidad específicas del servicio|

## visitcluster
| Clave | Tipo | Valor por defecto | Descripción|
| -- | -- | -- | --|
|`visitcluster.enabled`|`bool`|true|Indica si el servicio está habilitado. Si es `false`, no se crean sus recursos en Kubernetes (Deployment/StatefulSet, ConfigMap, Service, Ingress)|
|`visitcluster.replicas`|`int`|1|Número de réplicas del servicio|
|`visitcluster.image`|`string`|"gnoss/gnoss.visitcluster.enterprise"|Imagen Docker del servicio|
|`visitcluster.tag`|`string`|""|Tag de la imagen Docker|
|`visitcluster.pullPolicy`|`string`|IfNotPresent|Política de descarga de la imagen|
|`visitcluster.limitsEnabled`|`bool`|true|Indica si se aplican límites de recursos|
|`visitcluster.limitMemory`|`string`|600Mi|Límite de memoria del contenedor|
|`visitcluster.limitCpu`|`string`|500m|Límite de CPU del contenedor|
|`visitcluster.requestMemory`|`string`|125Mi|Memoria solicitada por el contenedor|
|`visitcluster.requestCpu`|`string`|50m|CPU solicitada por el contenedor|
|`visitcluster.espacioVol`|`string`|"5Gi"|Tamaño del volumen persistente del StatefulSet|
|`visitcluster.tolerations`|`list`|null|Toleraciones específicas del servicio|
|`visitcluster.affinity`|`object`|null|Reglas de afinidad específicas del servicio|

## visitregistry
| Clave | Tipo | Valor por defecto | Descripción|
| -- | -- | -- | --|
|`visitregistry.enabled`|`bool`|true|Indica si el servicio está habilitado. Si es `false`, no se crean sus recursos en Kubernetes (Deployment/StatefulSet, ConfigMap, Service, Ingress)|
|`visitregistry.replicas`|`int`|1|Número de réplicas del servicio|
|`visitregistry.image`|`string`|"gnoss/gnoss.visitregistry.enterprise"|Imagen Docker del servicio|
|`visitregistry.tag`|`string`|""|Tag de la imagen Docker|
|`visitregistry.pullPolicy`|`string`|IfNotPresent|Política de descarga de la imagen|
|`visitregistry.limitsEnabled`|`bool`|true|Indica si se aplican límites de recursos|
|`visitregistry.limitMemory`|`string`|600Mi|Límite de memoria del contenedor|
|`visitregistry.limitCpu`|`string`|500m|Límite de CPU del contenedor|
|`visitregistry.requestMemory`|`string`|125Mi|Memoria solicitada por el contenedor|
|`visitregistry.requestCpu`|`string`|50m|CPU solicitada por el contenedor|
|`visitregistry.espacioVol`|`string`|"5Gi"|Tamaño del volumen persistente del StatefulSet|
|`visitregistry.tolerations`|`list`|null|Toleraciones específicas del servicio|
|`visitregistry.affinity`|`object`|null|Reglas de afinidad específicas del servicio|

## thumbnailgenerator
| Clave | Tipo | Valor por defecto | Descripción|
| -- | -- | -- | --|
|`thumbnailgenerator.enabled`|`bool`|true|Indica si el servicio está habilitado. Si es `false`, no se crean sus recursos en Kubernetes (Deployment/StatefulSet, ConfigMap, Service, Ingress)|
|`thumbnailgenerator.image`|`string`|"gnoss/gnoss.thumbnail.enterprise"|Imagen Docker del servicio|
|`thumbnailgenerator.tag`|`string`|""|Tag de la imagen Docker|
|`thumbnailgenerator.pullPolicy`|`string`|IfNotPresent|Política de descarga de la imagen|
|`thumbnailgenerator.limitsEnabled`|`bool`|true|Indica si se aplican límites de recursos|
|`thumbnailgenerator.limitMemory`|`string`|600Mi|Límite de memoria del contenedor|
|`thumbnailgenerator.limitCpu`|`string`|500m|Límite de CPU del contenedor|
|`thumbnailgenerator.requestMemory`|`string`|125Mi|Memoria solicitada por el contenedor|
|`thumbnailgenerator.requestCpu`|`string`|50m|CPU solicitada por el contenedor|
|`thumbnailgenerator.tolerations`|`list`|null|Toleraciones específicas del servicio|
|`thumbnailgenerator.affinity`|`object`|null|Reglas de afinidad específicas del servicio|

## communitywall
| Clave | Tipo | Valor por defecto | Descripción|
| -- | -- | -- | --|
|`communitywall.enabled`|`bool`|true|Indica si el servicio está habilitado. Si es `false`, no se crean sus recursos en Kubernetes (Deployment/StatefulSet, ConfigMap, Service, Ingress)|
|`communitywall.image`|`string`|"gnoss/gnoss.communitywall.enterprise"|Imagen Docker del servicio|
|`communitywall.tag`|`string`|""|Tag de la imagen Docker|
|`communitywall.pullPolicy`|`string`|IfNotPresent|Política de descarga de la imagen|
|`communitywall.limitsEnabled`|`bool`|true|Indica si se aplican límites de recursos|
|`communitywall.limitMemory`|`string`|500Mi|Límite de memoria del contenedor|
|`communitywall.limitCpu`|`string`|500m|Límite de CPU del contenedor|
|`communitywall.requestMemory`|`string`|125Mi|Memoria solicitada por el contenedor|
|`communitywall.requestCpu`|`string`|50m|CPU solicitada por el contenedor|
|`communitywall.tolerations`|`list`|null|Toleraciones específicas del servicio|
|`communitywall.affinity`|`object`|null|Reglas de afinidad específicas del servicio|

## socialcacherefresh
| Clave | Tipo | Valor por defecto | Descripción|
| -- | -- | -- | --|
|`socialcacherefresh.enabled`|`bool`|true|Indica si el servicio está habilitado. Si es `false`, no se crean sus recursos en Kubernetes (Deployment/StatefulSet, ConfigMap, Service, Ingress)|
|`socialcacherefresh.image`|`string`|"gnoss/gnoss.socialcacherefresh.enterprise"|Imagen Docker del servicio|
|`socialcacherefresh.tag`|`string`|""|Tag de la imagen Docker|
|`socialcacherefresh.pullPolicy`|`string`|IfNotPresent|Política de descarga de la imagen|
|`socialcacherefresh.limitsEnabled`|`bool`|true|Indica si se aplican límites de recursos|
|`socialcacherefresh.limitMemory`|`string`|500Mi|Límite de memoria del contenedor|
|`socialcacherefresh.limitCpu`|`string`|500m|Límite de CPU del contenedor|
|`socialcacherefresh.requestMemory`|`string`|125Mi|Memoria solicitada por el contenedor|
|`socialcacherefresh.requestCpu`|`string`|50m|CPU solicitada por el contenedor|
|`socialcacherefresh.tolerations`|`list`|null|Toleraciones específicas del servicio|
|`socialcacherefresh.affinity`|`object`|null|Reglas de afinidad específicas del servicio|

## socialsearchgraphgeneration
| Clave | Tipo | Valor por defecto | Descripción|
| -- | -- | -- | --|
|`socialsearchgraphgeneration.enabled`|`bool`|true|Indica si el servicio está habilitado. Si es `false`, no se crean sus recursos en Kubernetes (Deployment/StatefulSet, ConfigMap, Service, Ingress)|
|`socialsearchgraphgeneration.image`|`string`|"gnoss/gnoss.socialsearchgraphgeneration.enterprise"|Imagen Docker del servicio|
|`socialsearchgraphgeneration.tag`|`string`|""|Tag de la imagen Docker|
|`socialsearchgraphgeneration.pullPolicy`|`string`|IfNotPresent|Política de descarga de la imagen|
|`socialsearchgraphgeneration.limitsEnabled`|`bool`|true|Indica si se aplican límites de recursos|
|`socialsearchgraphgeneration.limitMemory`|`string`|500Mi|Límite de memoria del contenedor|
|`socialsearchgraphgeneration.limitCpu`|`string`|500m|Límite de CPU del contenedor|
|`socialsearchgraphgeneration.requestMemory`|`string`|125Mi|Memoria solicitada por el contenedor|
|`socialsearchgraphgeneration.requestCpu`|`string`|50m|CPU solicitada por el contenedor|
|`socialsearchgraphgeneration.tolerations`|`list`|null|Toleraciones específicas del servicio|
|`socialsearchgraphgeneration.affinity`|`object`|null|Reglas de afinidad específicas del servicio|

## subscriptionsmail
| Clave | Tipo | Valor por defecto | Descripción|
| -- | -- | -- | --|
|`subscriptionsmail.enabled`|`bool`|true|Indica si el servicio está habilitado. Si es `false`, no se crean sus recursos en Kubernetes (Deployment/StatefulSet, ConfigMap, Service, Ingress)|
|`subscriptionsmail.image`|`string`|"gnoss/gnoss.subscriptionsmail.enterprise"|Imagen Docker del servicio|
|`subscriptionsmail.tag`|`string`|""|Tag de la imagen Docker|
|`subscriptionsmail.pullPolicy`|`string`|IfNotPresent|Política de descarga de la imagen|
|`subscriptionsmail.limitsEnabled`|`bool`|true|Indica si se aplican límites de recursos|
|`subscriptionsmail.limitMemory`|`string`|500Mi|Límite de memoria del contenedor|
|`subscriptionsmail.limitCpu`|`string`|500m|Límite de CPU del contenedor|
|`subscriptionsmail.requestMemory`|`string`|125Mi|Memoria solicitada por el contenedor|
|`subscriptionsmail.requestCpu`|`string`|50m|CPU solicitada por el contenedor|
|`subscriptionsmail.tolerations`|`list`|null|Toleraciones específicas del servicio|
|`subscriptionsmail.affinity`|`object`|null|Reglas de afinidad específicas del servicio|

## replication
| Clave | Tipo | Valor por defecto | Descripción|
| -- | -- | -- | --|
|`replication.enabled`|`bool`|true|Indica si el servicio está habilitado. Si es `false`, no se crean sus recursos en Kubernetes (Deployment/StatefulSet, ConfigMap, Service, Ingress)|
|`replication.image`|`string`|"gnoss/gnoss.backgroundtask.replication.opencore"|Imagen Docker del servicio|
|`replication.tag`|`string`|""|Tag de la imagen Docker|
|`replication.pullPolicy`|`string`|IfNotPresent|Política de descarga de la imagen|
|`replication.limitsEnabled`|`bool`|true|Indica si se aplican límites de recursos|
|`replication.limitMemory`|`string`|500Mi|Límite de memoria del contenedor|
|`replication.limitCpu`|`string`|500m|Límite de CPU del contenedor|
|`replication.requestMemory`|`string`|125Mi|Memoria solicitada por el contenedor|
|`replication.requestCpu`|`string`|50m|CPU solicitada por el contenedor|
|`replication.tolerations`|`list`|null|Toleraciones específicas del servicio|
|`replication.affinity`|`object`|null|Reglas de afinidad específicas del servicio|
|`replication.colasReplicacionMaster`|`list`|`[]`|Lista de colas de replicación master. Cada elemento define el nombre de la cola RabbitMQ y la key del Secret de Virtuoso con su cadena de conexión|
|`replication.colasReplicacionMaster[].nombre`|`string`|-|Nombre de la cola RabbitMQ. Se usa como sufijo en la variable de entorno `ColaReplicacionMaster_{nombre}`|
|`replication.colasReplicacionMaster[].secretKey`|`string`|-|Key dentro del Secret de Virtuoso que contiene la cadena de conexión|
|`replication.colasReplicacionMasterHome`|`list`|`[]`|Lista de colas de replicación master home. Cada elemento define el nombre de la cola RabbitMQ y la key del Secret de Virtuoso con su cadena de conexión|
|`replication.colasReplicacionMasterHome[].nombre`|`string`|-|Nombre de la cola RabbitMQ. Se usa como sufijo en la variable de entorno `ColaReplicacionMasterHome__{nombre}`|
|`replication.colasReplicacionMasterHome[].secretKey`|`string`|-|Key dentro del Secret de Virtuoso que contiene la cadena de conexión|

## newsletters
| Clave | Tipo | Valor por defecto | Descripción|
| -- | -- | -- | --|
|`newsletters.enabled`|`bool`|true|Indica si el servicio está habilitado. Si es `false`, no se crean sus recursos en Kubernetes (Deployment/StatefulSet, ConfigMap, Service, Ingress)|
|`newsletters.image`|`string`|"gnoss/gnoss.newsletters.enterprise"|Imagen Docker del servicio|
|`newsletters.tag`|`string`|""|Tag de la imagen Docker|
|`newsletters.pullPolicy`|`string`|IfNotPresent|Política de descarga de la imagen|
|`newsletters.limitsEnabled`|`bool`|true|Indica si se aplican límites de recursos|
|`newsletters.limitMemory`|`string`|500Mi|Límite de memoria del contenedor|
|`newsletters.limitCpu`|`string`|500m|Límite de CPU del contenedor|
|`newsletters.requestMemory`|`string`|125Mi|Memoria solicitada por el contenedor|
|`newsletters.requestCpu`|`string`|50m|CPU solicitada por el contenedor|
|`newsletters.tolerations`|`list`|null|Toleraciones específicas del servicio|
|`newsletters.affinity`|`object`|null|Reglas de afinidad específicas del servicio|

## userwall
| Clave | Tipo | Valor por defecto | Descripción|
| -- | -- | -- | --|
|`userwall.enabled`|`bool`|true|Indica si el servicio está habilitado. Si es `false`, no se crean sus recursos en Kubernetes (Deployment/StatefulSet, ConfigMap, Service, Ingress)|
|`userwall.image`|`string`|"gnoss/gnoss.userwall.enterprise"|Imagen Docker del servicio|
|`userwall.tag`|`string`|""|Tag de la imagen Docker|
|`userwall.pullPolicy`|`string`|IfNotPresent|Política de descarga de la imagen|
|`userwall.limitsEnabled`|`bool`|true|Indica si se aplican límites de recursos|
|`userwall.limitMemory`|`string`|500Mi|Límite de memoria del contenedor|
|`userwall.limitCpu`|`string`|500m|Límite de CPU del contenedor|
|`userwall.requestMemory`|`string`|125Mi|Memoria solicitada por el contenedor|
|`userwall.requestCpu`|`string`|50m|CPU solicitada por el contenedor|
|`userwall.tolerations`|`list`|null|Toleraciones específicas del servicio|
|`userwall.affinity`|`object`|null|Reglas de afinidad específicas del servicio|

## workflows
| Clave | Tipo | Valor por defecto | Descripción|
| -- | -- | -- | --|
|`workflows.enabled`|`bool`|true|Indica si el servicio está habilitado. Si es `false`, no se crean sus recursos en Kubernetes (Deployment/StatefulSet, ConfigMap, Service, Ingress)|
|`workflows.replicas`|`int`|0|Número de réplicas del servicio (0 = deshabilitado por defecto)|
|`workflows.image`|`string`|"gnoss/gnoss.workflows.enterprise"|Imagen Docker del servicio|
|`workflows.tag`|`string`|""|Tag de la imagen Docker|
|`workflows.pullPolicy`|`string`|IfNotPresent|Política de descarga de la imagen|
|`workflows.limitsEnabled`|`bool`|true|Indica si se aplican límites de recursos|
|`workflows.limitMemory`|`string`|500Mi|Límite de memoria del contenedor|
|`workflows.limitCpu`|`string`|500m|Límite de CPU del contenedor|
|`workflows.requestMemory`|`string`|125Mi|Memoria solicitada por el contenedor|
|`workflows.requestCpu`|`string`|50m|CPU solicitada por el contenedor|
|`workflows.tolerations`|`list`|null|Toleraciones específicas del servicio|
|`workflows.affinity`|`object`|null|Reglas de afinidad específicas del servicio|

## redis
| Clave | Tipo | Valor por defecto | Descripción|
| -- | -- | -- | --|
|`redis.read`|`string`|"redis"|Hostname del servidor Redis de lectura|
|`redis.master`|`string`|"redis"|Hostname del servidor Redis maestro (escritura)|
|`redis.redisBd`|`string`|"1"|Índice de la base de datos Redis para uso general|
|`redis.redisTimeout`|`string`|"60"|Timeout en segundos para la base de datos Redis general|
|`redis.recursosBd`|`string`|"2"|Índice de la base de datos Redis para recursos|
|`redis.recursosTimeout`|`string`|"60"|Timeout en segundos para la base de datos Redis de recursos|
|`redis.liveusuariosBd`|`string`|"3"|Índice de la base de datos Redis para usuarios en línea|
|`redis.liveusuariosTimeout`|`string`|"60"|Timeout en segundos para la base de datos Redis de usuarios en línea|
|`redis.bandejaBd`|`string`|"4"|Índice de la base de datos Redis para la bandeja de entrada|
|`redis.bandejaTimeout`|`string`|"60"|Timeout en segundos para la base de datos Redis de bandeja|
|`redis.sparqlBd`|`string`|"5"|Índice de la base de datos Redis para caché de consultas SPARQL|
|`redis.sparqlTimeout`|`string`|"60"|Timeout en segundos para la base de datos Redis de SPARQL|

## postgres
| Clave | Tipo | Valor por defecto | Descripción|
| -- | -- | -- | --|
|`postgres.enabled`|`bool`|true|Indica si se habilita el uso de PostgreSQL como base de datos SQL|
