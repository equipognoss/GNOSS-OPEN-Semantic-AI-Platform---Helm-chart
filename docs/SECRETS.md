# Secrets
A continuación se listan todas los posibles secrets que acepta el chart.
## SQL
| Nombre secret | Clave secret | Ejemplo | Descripción |
| -- | -- | -- | -- |
| sql-secret | acidConnectionString | Server=SERVER_IP;Database=DB_NAME;User Id=DB_USER;Password=DB_USER_PASSWORD;Persist Security Info=true;TrustServerCertificate=True | Cadena de conexión que utilizará la aplicación para conectarse a la base de datos ACID. Esta base de datos almacena casi toda la información que va a manejar la Web. |
| sql-secret | baseConnectionString | Server=SERVER_IP;Database=BASE_DB_NAME;User Id=DB_USER;Password=DB_USER_PASSWORD;Persist Security Info=true;TrustServerCertificate=True | Cadena de conexión que utilizará la aplicación para conectarse a la base de datos BASE. Esta base de datos almacena información de los RDF de los recursos y las etiquetas que se utilizarán para el autocompletar |
| sql-secret | oauthConnectionString | Server=SERVER_IP;Database=OAUTH_DB_NAME;User Id=DB_USER;Password=DB_USER_PASSWORD;Persist Security Info=true;TrustServerCertificate=True | Cadena de conexión que utilizará la aplicación para conectarse a la base de datos OAuth. Esta base de datos almacena información para validar peticiones OAuth y debe estar presente en las configuraciones del servicio OAuth, API y Web. |
> [!note]
> En el caso de que se quiera tener todo junto en la misma base de datos, es posible tener una sola cadena de conexión para las tres configuraciones.

## Virtuoso
| Nombre secret | Clave secret | Ejemplo | Descripción |
| -- | -- | -- | -- |
| virtuoso-secret | virtuosoRead | HOST=SERVER_IP;UID=USER;PWD=PASSWORD;Pooling=true;Max Pool Size=10;Connection Lifetime=15000 | Cadena de conexión que utilizará la aplicación para conectarse a Virtuoso |
| virtuoso-secret | virtuosoReadHome | HOST=SERVER_IP;UID=USER;PWD=PASSWORD;Pooling=true;Max Pool Size=10;Connection Lifetime=15000 | Cadena de conexión que utilizará la aplicación para conectarse al Virtuoso de la Home, donde se guarda información acerca de la actividad reciente |
| virtuoso-secret | virtuosoWriteHome | HOST=SERVER_IP;UID=USER;PWD=PASSWORD;Pooling=true;Max Pool Size=10;Connection Lifetime=15000 | Cadena de conexión que utilizará la aplicación para escribir en el Virtuoso de la Home, donde se guarda información acerca de la actividad reciente |
| virtuoso-secret| virtuosoWrite1 | HOST=SERVER_IP;UID=USER;PWD=PASSWORD;Pooling=true;Max Pool Size=10;Connection Lifetime=15000 | Cadena de conexión que utilizará la aplicación para escribir en el Virtuoso 1 cuando la replicación esté activada |
| virtuoso-secret | virtuosoWrite2| HOST=SERVER_IP;UID=USER;PWD=PASSWORD;Pooling=true;Max Pool Size=10;Connection Lifetime=15000 | Cadena de conexión que utilizará la aplicación para escribir en el Virtuoso 2 cuando la replicación esté activada |

## RabbitMQ
| Nombre secret | Clave secret | Ejemplo | Descripción |
| -- | -- | -- | -- |
| rabbitmq-secret | rabbitMQConnectionString | amqp://USER:PASSWORD@SERVER_IP:5672/VIRTUAL_HOST | Cadena de conexión que utilizará la aplicación para conectarse a RabbitMQ y envíar o leer mensajes de las colas |

## Identity
| Nombre secret | Clave secret | Ejemplo | Descripción |
| -- | -- | -- | -- |
| identity-secret | scope | apiidentity | Ámbito necesario para crear un token para el Identity que posteriormente se utilizará para realizar peticiones a servicios internos |
| identity-secret | clientID | 00000000-0000-0000-0000-000000000000 | ClientID necesario para crear un token para el Identity que posteriormente se utilizará para realizar peticiones a servicios internos |
| identity-secret | clientSecret | 00000000-0000-0000-0000-000000000000 | ClientSecret necesario para crear un token para el Identity que posteriormente se utilizará para realizar peticiones a servicios internos |
> [!note]
> El `scope` por compatibilidad debe tener siempre el valor `apiidentity`


## Ejemplo de implementación
### Fichero secrets.yml

    kind: Secret
    metadata:
	    name: virtuoso-secret
	    namespace: {{ .Release.Namespace | quote }}
	type: Opaque
	stringData:
		virtuosoConnectionString: "HOST=SERVER_IP;UID=USER;PWD=PASSWORD;Pooling=true;Max Pool Size=10;Connection Lifetime=15000"
		Virtuoso__Escritura__Virtuoso1: "HOST=SERVER_IP_1;UID=USER;PWD=PASSWORD;Pooling=true;Max Pool Size=10;Connection Lifetime=15000"