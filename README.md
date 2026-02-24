# GNOSS Open Semantic AI Platform - Helm Chart

Este chart de Helm permite desplegar todas las aplicaciones que forman GNOSS Semantic AI Platform en un namespace de Kubernetes en su versión Open.

## Introducción

El chart se encarga de desplegar todos los objetos de Kubernetes necesarios para el funcionamiento de la plataforma, a excepción de las bases de datos y los secretos.

Los componentes que forman parte del chart son:

-  **Gateway:** permite gestionar la comunicación del exterior con el interior del clúster, definiendo diferentes dominios y reglas para enrutar las peticiones a los servicios correspondientes.
-  **HTTPRoute:** uno de los tipos de enrutamiento que permite configurar un Gateway para dirigir el trafico de las peticiones desde el Gateway al objeto de API correspondiente, por ejemplo a un Service.
-  **Service:** permite exponer como un servicio a la red aquellas aplicaciones que se ejecutan en uno o mas pods
-  **Deployment:** las configuraciones de cada una de las aplicaciones que se ejecutan dentro del clúster en pods
-  **StatefulSet:** igual que un Deployment gestiona las aplicaciones que se ejecutan en pods, pero es especifico para aplicaciones que necesitan estado, como puede ser acceso a almacenamiento persistente en disco.
-  **ConfigMap:** usado para gestionar las variables de entorno no secretas en forma de clave-valor
-  **Role:** los roles de los controles de permisos necesarios para un control RBAC
-  **RoleBinding:** la asignación de los roles en RBAC
-  **Values:** valores estándar de configuración no secretos.

## Prerrequisitos

- Kubernetes 1.34+
- Helm 3.18+
- NGINX Gateway Fabric 1.5+ instalado
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
| `init.namespace` | Namespace donde se despliega | `produccion` |
| `ingress.https` | Habilitar HTTPS | `true` |
| `ingress.hostWeb` | Hostname para web | `gnoss.com` |
| `ingress.hostServicios` | Hostname para servicios | `servicios.gnoss.com` |

Ver [VALUES.md](./docs/VALUES.md) para la lista completa.

### Secrets necesarios

| Nombre | Clave | Descripción | Ejemplo |
|-----------|-------------|-------------------|-------------------|
| sql | `acid` | Cadena de conexión para la base de datos ACID | `Server=SERVER_IP;Database=DB_NAME;User Id=DB_USER;Password=DB_USER_PASSWORD;Persist Security Info=true;TrustServerCertificate=True` |
| virtuoso | `virtuosoRead` | Cadena de conexión para Virtuoso | `HOST=SERVER_IP;UID=USER;PWD=PASSWORD;Pooling=true;Max Pool Size=10;Connection Lifetime=15000` |
| rabbitmq | `connectionString` | Cadena de conexión para RabbitMQ | `amqp://USER:PASSWORD@SERVER_IP:5672/VIRTUAL_HOST` |

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
| Nombre | Objetos Kubernetes | Descripción
| -- | -- | -- |
|Gnoss API | Deployment + Service | Aplicación Web que ofrece un interfaz de programación para que otras aplicaciones puedan realizar consultas o modificaciones en los datos almacenados en la plataforma de manera automatizada. |
|Servicio de archivos	| StatefulSet | Aplicación Web que se encarga de almacenar y servir las ontologías de la plataforma. |
|	Servicio autocompletar | Deployment + Service | Aplicación Web que se encarga de generar las sugerencias de búsqueda de una faceta concreta.
|	Servicio autocompletar etiquetas | Deployment + Service |
|	Servicio refresco cache | Deployment | Aplicación de segundo plano que se encarga de invalidar las cachés que necesitan ser actualizadas. | 
|	Servicio muro comunidad | Deployment | Aplicación de segundo plano que se encarga de generar la actividad reciente de cada comunidad. |
| API despliegues	| Deployment |
| Servicio distribuidor de eventos internos	|Deployment| Aplicación de segundo plano que recibe un evento de creación o edición de un recurso y notifica al resto de servicios que tienen que realizar alguna acción
|Servicio de documentos	|StatefulSet| Aplicación de segundo plano que recibe un evento de creación o edición de un recurso y notifica al resto de servicios que tienen que realizar alguna acción
|Servicio facetas	| Deployment + Service | Aplicación Web que se encarga de mostrar los filtros de búsqueda (facetas) disponibles en una página de búsqueda.|
|Identity Server	| Deployment | Aplicación Web que se encarga de proveer y validar los tokens de acceso para acceder a las aplicaciones de uso interno (intern, ontologies y documents).
| Servicio interno 	| StatefulSet + Service | Aplicación Web que se encarga de almacenar el contenido estático (imágenes, vídeos y pdfs principalmente) que suben los usuarios desde la Web
|Servicio login	|Deployment + Service | Aplicación Web que se encarga de autenticar al usuario, validar su contraseña y enviar las credenciales a la Web.
|Servicio envio correos	| Deployment | Aplicación de segundo plano que se encarga de enviar todos los emails que se mandan a través de la plataforma. |
|Servicio envio newsletters	| Deployment | Aplicación de segundo plano que se encarga de enviar a todos los usuarios de una comunidad los emails de una newsletter. |
|Servicio Oauth	| Deployment | Aplicación Web que se encarga de validar las firmas Oauth que le llegan a la Web o el API.
|Servicio replicación	| Deployment | plicación de segundo plano que permite la alta disponibilidad de lectura. | 
|Servicio resultados	| Deployment + Service | Aplicación Web que se encarga de mostrar los resultados en una página de búsqueda.|
| Servicio base 	| Deployment | Aplicación de segundo plano que se encarga de insertar en el grafo de búsqueda los triples de cada elemento que se cree en la comunidad (recurso, persona, etc). 
|	Servicio refresco cache sociales | Deployment | Aplicación de segundo plano que se encarga de invalidar las cachés de la bandeja de mensajes de un usuario cada vez que recibe un mensaje nuevo, para que las bandejas de mensajes estén siempre actualizadas. |
| Servicio generación grafo búsqueda usuario	| Deployment | Aplicación de segundo plano que se encarga de insertar en el grafo de búsqueda de cada usuario los triples de los mensajes que envía y recibe dentro de la plataforma. |
| Contenido estático	| Deployment | Contenido estático de la plataforma JS,CSS ... |
| Servicio suscripciones | Deployment | Aplicación de segundo plano que se encarga de generar los boletines de suscripciones de los usuarios que tienen alguna suscripción activa. |
| Servicio generador de miniaturas | Deployment | Aplicación de segundo plano que se encarga de generar las miniaturas de las imágenes que suben los usuarios a los recursos. |
| Servicio muro usuario	| Deployment | Aplicación de segundo plano que se encarga de generar la actividad reciente relativa a todas las comunidades que pertenece un usuario en su muro de la plataforma, habitualmente en la home de la plataforma. |
|	Servicio recuento visitas | Deployment | Aplicación de segundo plano que se encarga de insertar en base de datos las visitas que ha contabilizado el servicio Visit Registry. |
|	Servicio registro visitas | Deployment | Aplicación de segundo plano que expone un puerto UDP, al que la Web le envía las visitas a cada recurso.
|Gnoss Web | Deployment + Service | Es la aplicación principal de la plataforma GNOSS. Se encarga de gestionar la autorización de los usuarios a las páginas de la plataforma, la navegación por la web, mantener la sesión del usuario, la carga del menú de la aplicación web con las opciones que el usuario tiene disponibles, etc. |

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

**P: ¿Puedo usar mi propio Gateway existente?**
R: Sí, puedes desactivar la creación del Gateway...

**P: ¿Cómo expongo solo ciertos endpoints?**
R: Modifica los HTTPRoutes en `templates/httproutes.yaml`...

## Licencia

[Tipo de licencia]