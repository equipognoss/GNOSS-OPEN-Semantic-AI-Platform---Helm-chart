# GNOSS Open Semantic AI Platform - Helm Chart

Este chart de Helm permite desplegar todas las aplicaciones que forman GNOSS Semantic AI Platform en un namespace de Kubernetes en su versión Open.

## Introducción

El chart se encarga de desplegar todos los objetos de Kubernetes necesarios para el funcionamiento de la plataforma, a excepción de las bases de datos y los secretos.

Los principales tipos de objetos que forman parte del chart son:

-  **Ingress:** permite gestionar la comunicación del exterior con el interior del clúster, definiendo diferentes dominios y reglas para enrutar las peticiones a los servicios correspondientes.
-  **Service:** permite exponer como un servicio a la red aquellas aplicaciones que se ejecutan en uno o mas pods
-  **Deployment:** las configuraciones de cada una de las aplicaciones que se ejecutan dentro del clúster en pods
-  **StatefulSet:** igual que un Deployment gestiona las aplicaciones que se ejecutan en pods, pero es especifico para aplicaciones que necesitan estado, como puede ser acceso a almacenamiento persistente en disco.
-  **ConfigMap:** usado para gestionar las variables de entorno no secretas en forma de clave-valor
-  **Values:** valores estándar de configuración no secretos.

## Prerrequisitos

- Kubernetes 1.34+
- Helm 3.18+
- PV provisioner
- Certificados TLS (si usa HTTPS)
- Bases de datos:
    - SQL: SQL Server/Oracle SQL/PostgreSQL
    - Cache: Redis
    - Eventos: RabbitMQ
    - Grafo: Virtuoso
- Secretos creados en el namespace en el que se va a desplegar

## Configuración

### Values principales

| Valor | Descripción | Valor por defecto |
|-----------|-------------|-------------------|
| `Release.namespace` | Namespace donde se despliega | Valor fijado por el sistema de despliegue |
| `ingress.https` | Habilitar HTTPS |  |
| `ingress.forwardedheaders` | Valor que se le da a la variable ASPNETCORE_FORWARDEDHEADERS_ENABLED para que llegue el schema a la app |  |
| `global.hosts.web` | Hostname para web |  |
| `global.hosts.services` | Hostname para servicios |  |

Ver [VALUES.md](./docs/VALUES.md) para la lista completa.

### Secrets necesarios

| Nombre | Clave | Descripción | Ejemplo |
|-----------|-------------|-------------------|-------------------|
| sql-secret | `acid` | Cadena de conexión para la base de datos ACID | `Server=SERVER_IP;Database=DB_NAME;User Id=DB_USER;Password=DB_USER_PASSWORD;Persist Security Info=true;TrustServerCertificate=True` |
| virtuoso-secret | `virtuosoRead` | Cadena de conexión para Virtuoso | `HOST=SERVER_IP;UID=USER;PWD=PASSWORD;Pooling=true;Max Pool Size=10;Connection Lifetime=15000` |
| rabbitmq-secret | `connectionString` | Cadena de conexión para RabbitMQ | `amqp://USER:PASSWORD@SERVER_IP:5672/VIRTUAL_HOST` |
| identity-secret | `scope` | Ámbito necesario para crear un token para el Identity y poder realizar peticiones a servicios internos | `apiidentity` |

Ver [SECRETS.md](./docs/SECRETS.md) para la lista completa.

### Ejemplos de configuración

## Instalación

### Añadir el repositorio

```bash
helm repo add gnoss https://charts.gnoss.com
helm repo update
```

### Instalar el chart

```bash
helm install gnoss gnoss/gnoss \
  --namespace produccion \
  --create-namespace \
  -f values-produccion.yaml
```

### Instalar desde código fuente

```bash
git clone https://github.com/gnoss/helm-chart.git
cd helm-chart
helm install gnoss . --namespace produccion
```

## Desinstalación

```bash
helm uninstall gnoss --namespace produccion
```

## Actualización

### Actualizar a nueva versión

```bash
helm upgrade gnoss gnoss/gnoss \
  --namespace produccion \
  -f values-produccion.yaml
```

### Rollback

```bash
helm rollback gnoss 1 --namespace produccion
```

## Arquitectura

Gnoss Semantic AI Platform esta formado por una gran cantidad de servicios que trabajan de forma conjunta, esto servicios se ven reflejados en distintos objetos de Kubernetes como Services, Deployment o StatefulSet

![Diagrama de objetos desplegados](docs/images/block-deploy-overview.png  "Diagrama de objetos desplegados")
Los servicios que se despliegan son:
| Nombre | Servicio | Plantillas | Objetos Kubernetes | Descripción
| -- | -- | -- | -- | -- |
|Gnoss API | api | templates/api | Deployment + Service | Aplicación Web que ofrece un interfaz de programación para que otras aplicaciones puedan realizar consultas o modificaciones en los datos almacenados en la plataforma de manera automatizada. |
|Servicio de archivos | ontologias | templates/ontologies | StatefulSet | Aplicación Web que se encarga de almacenar y servir las ontologías de la plataforma. |
|	Servicio autocompletar | autocompletar | templates/autocomplete | Deployment + Service | Aplicación Web que se encarga de generar las sugerencias de búsqueda de una faceta concreta.
|	Servicio autocompletar etiquetas | etiquetadoautomatico | templates/automatic-labeling | Deployment + Service | Aplicación Web que se encarga de proponer las etiquetas de los recursos. |
|	Servicio refresco cache | cacherefresh | templates/cache-refresh | Deployment | Aplicación de segundo plano que se encarga de invalidar las cachés que necesitan ser actualizadas. |
|	Servicio muro comunidad | communitywall | templates/community-wall | Deployment | Aplicación de segundo plano que se encarga de generar la actividad reciente de cada comunidad. |
| API despliegues	| despliegues | templates/deploy-api | Deployment + Service | Aplicación Web que gestiona la subida de los distintos paquetes de configuración en los despliegues. |
| Servicio distribuidor de eventos internos	| distributor | templates/distributor | Deployment | Aplicación de segundo plano que recibe un evento de creación o edición de un recurso y notifica al resto de servicios que tienen que realizar alguna acción
|Servicio de documentos	| documents | templates/documents | StatefulSet | Aplicación de segundo plano que recibe un evento de creación o edición de un recurso y notifica al resto de servicios que tienen que realizar alguna acción
|Servicio facetas	| facetas | templates/facets | Deployment + Service | Aplicación Web que se encarga de mostrar los filtros de búsqueda (facetas) disponibles en una página de búsqueda.|
|Identity Server	| identityserver | templates/identity-server | Deployment | Aplicación Web que se encarga de proveer y validar los tokens de acceso para acceder a las aplicaciones de uso interno (intern, ontologies y documents).
| Servicio interno 	| interno | templates/intern | StatefulSet + Service | Aplicación Web que se encarga de almacenar el contenido estático (imágenes, vídeos y pdfs principalmente) que suben los usuarios desde la Web
|Servicio login	| login | templates/login | Deployment + Service | Aplicación Web que se encarga de autenticar al usuario, validar su contraseña y enviar las credenciales a la Web.
|Servicio envio correos	| mailservice | templates/mail | Deployment | Aplicación de segundo plano que se encarga de enviar todos los emails que se mandan a través de la plataforma. |
|Servicio envio newsletters	| newsletters | templates/newsletters | Deployment | Aplicación de segundo plano que se encarga de enviar a todos los usuarios de una comunidad los emails de una newsletter. |
|Servicio Oauth	| oauth | templates/oauth | Deployment | Aplicación Web que se encarga de validar las firmas Oauth que le llegan a la Web o el API.
|Servicio replicación	| replication | templates/replication | Deployment | Aplicación de segundo plano que permite la alta disponibilidad de lectura. | 
|Servicio resultados	| results | templates/results | Deployment + Service | Aplicación Web que se encarga de mostrar los resultados en una página de búsqueda.|
| Servicio base 	| searchgraphgeneration | templates/search-graph-generation | Deployment | Aplicación de segundo plano que se encarga de insertar en el grafo de búsqueda los triples de cada elemento que se cree en la comunidad (recurso, persona, etc).
|	Servicio refresco cache sociales | socialcacherefresh | templates/social-cache-refresh | Deployment | Aplicación de segundo plano que se encarga de invalidar las cachés de la bandeja de mensajes de un usuario cada vez que recibe un mensaje nuevo, para que las bandejas de mensajes estén siempre actualizadas. |
| Servicio generación grafo búsqueda usuario	| socialsearchgraphgeneration | templates/social-search-graph-generation | Deployment | Aplicación de segundo plano que se encarga de insertar en el grafo de búsqueda de cada usuario los triples de los mensajes que envía y recibe dentro de la plataforma. |
| Contenido estático	| static | templates/static | Deployment | Contenido estático de la plataforma JS,CSS ... |
| Servicio suscripciones | subscriptionsmail | templates/subscription-mail | Deployment | Aplicación de segundo plano que se encarga de generar los boletines de suscripciones de los usuarios que tienen alguna suscripción activa. |
| Servicio generador de miniaturas | thumbnailgenerator | templates/thumbnail | Deployment | Aplicación de segundo plano que se encarga de generar las miniaturas de las imágenes que suben los usuarios a los recursos. |
| Servicio muro usuario	| userwall | templates/user-wall | Deployment | Aplicación de segundo plano que se encarga de generar la actividad reciente relativa a todas las comunidades que pertenece un usuario en su muro de la plataforma, habitualmente en la home de la plataforma. |
|	Servicio recuento visitas | visitcluster | templates/visit | Deployment | Aplicación de segundo plano que se encarga de insertar en base de datos las visitas que ha contabilizado el servicio Visit Registry. |
|	Servicio registro visitas | visitregistry | templates/visit | Deployment | Aplicación de segundo plano que expone un puerto UDP, al que la Web le envía las visitas a cada recurso.
|Gnoss Web | web | templates/web | Deployment + Service | Es la aplicación principal de la plataforma GNOSS. Se encarga de gestionar la autorización de los usuarios a las páginas de la plataforma, la navegación por la web, mantener la sesión del usuario, la carga del menú de la aplicación web con las opciones que el usuario tiene disponibles, etc. |

Ver [ARCHITECTURE.md](./docs/ARCHITECTURE.md) para más detalles.

## Troubleshooting

### El Gateway no recibe tráfico

```bash
# Verificar Gateway
kubectl get gateway -n produccion

# Ver logs del controlador
kubectl logs -n nginx-gateway -l app=nginx-gateway
```

### Los HTTPRoutes no funcionan

```bash
# Verificar HTTPRoutes
kubectl get httproute -n produccion
kubectl describe httproute gnoss-routes -n produccion

# Ver eventos
kubectl get events -n produccion --sort-by='.lastTimestamp'
```

### Certificados TLS no funcionan

```bash
# Verificar el Secret
kubectl get secret gnoss-tls-cert -n produccion
kubectl describe secret gnoss-tls-cert -n produccion
```

## FAQ

**P: ¿Como actualizo la versión de la aplicación?**

R: La versión se controla mediante el value `.Values.general.tag` que define la versión para todos los servicios, excepto si tiene valor el value de cada servicio, por ejemplo, `.Values.web.tag`, en ese caso se usara la versión concreta para ese servicio.

Esto permite actualizar la versión globalmente y actualizar servicios concretos.

**P: ¿Como desactivo un servicio?**

R: Para desactivar un servicio que no se necesite en el proyecto se pueden seguir dos caminos:
- Usar el value `replicas`: si se establece a `0` se crearán todos los objetos asociados al servicio pero la cantidad de pods en ejecución será 0. Es útil para desactivarlo temporalmente.
- Usar el value `enabled`: si se establece a `false` no se creará ninguna de los objetos asociados al servicio. Útil para servicios que no se necesiten usar nunca.

En ambos casos los values se definen a nivel de cada uno de los servicios, por ejemplo, `.Values.cacherefresh.enabled`

## Licencia

[Tipo de licencia]