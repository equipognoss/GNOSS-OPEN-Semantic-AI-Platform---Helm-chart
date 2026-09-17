# Changelog

Todos los cambios notables de este proyecto se documentarán en este fichero.

El formato está basado en [Keep a Changelog](https://keepachangelog.com/es-ES/1.1.0/),
y este proyecto se adhiere al [Versionado Semántico](https://semver.org/lang/es/).

## [Unreleased]

## [2.0.0] - 2026-09-17

### ⚠️ Breaking changes

- **Se elimina el servicio `api-intern`** (API interna). Cualquier consumidor interno que
  siguiera llamando a `http://api-interna:8080` dejará de poder hacerlo. Solo es seguro
  actualizar si la versión de la aplicación desplegada ya no depende de esa instancia
  interna (mejora ligada a la optimización del protocolo OAuth).
- **Los health checks se activan por defecto** (`general.healthChecksEnabled: true`).
  Se añaden `livenessProbe`/`readinessProbe` contra `/health/live` y `/health/ready` en el
  puerto `general.managementPort` (8081) para prácticamente todos los servicios. Si la
  imagen desplegada no expone aún esos endpoints, los pods pueden empezar a fallar el
  liveness/readiness tras la actualización. Puede desactivarse con
  `general.healthChecksEnabled: false`.
- **Se reducen los límites de RAM** de varios servicios (ver sección _Changed_). Si el
  consumo real de memoria de la imagen desplegada es mayor al esperado, puede provocar
  `OOMKilled`. Revisar y ajustar `limitMemory` si es necesario tras la actualización.
- **Nuevos `Service` de tipo `NodePort`** en `documents`, `identityserver`, `interno`,
  `oauth` y `ontologias`, con puertos fijos (30007-30013). No afecta a la comunicación
  interna del clúster, pero:
  - Esos puertos deben estar libres en los nodos del clúster.
  - Si se despliegan varios releases del chart en el mismo clúster (multi-tenant),
    pueden colisionar, ya que el NodePort es a nivel de clúster y no de namespace.
  - Pueden desactivarse/cambiarse mediante `<servicio>.debugNodePort`.

### Added

- NodePorts fijos para depuración remota de `documents`, `identityserver`, `interno`,
  `oauth` y `ontologias`, configurables mediante `<servicio>.debugNodePort`.
- `web.robotsTxt` para configurar el contenido del fichero `robots.txt` desde los values.
- Health checks configurables mediante `general.healthChecksEnabled` (por defecto `true`),
  `general.apiPort` (8080) y `general.managementPort` (8081).
- `oauth.rateLimitPermitLimit` para configurar el límite de peticiones (rate limit) del
  servicio OAuth (variable `OAuthSettings__RateLimitPermitLimit`).
- Value `<servicio>.enabled` (por defecto `true`) en cada servicio, que permite omitir la
  creación de todos sus recursos de Kubernetes (Deployment/StatefulSet, ConfigMap,
  Service, Ingress), incluidas las reglas correspondientes en el Ingress compartido.
- Documentación de los servicios desplegados en el `README.md` (columnas `Servicio` y
  `Plantillas`) y nueva entrada en la FAQ sobre cómo desactivar un servicio
  (`replicas: 0` frente a `enabled: false`).
- Este fichero `CHANGELOG.md`.

### Changed

- Eliminada la variable de entorno `Servicios__urlBase` duplicada en los
  Deployments/StatefulSets que ya cargan el ConfigMap global `config-gnoss-core`, que la
  provee con el mismo valor.
- Reducidos los límites de RAM tras la optimización de la nueva versión de la
  aplicación:
  - `web.limitMemory`: 6000Mi → 5000Mi
  - `web.requestCpu`: 200m → 500m
  - `api.limitMemory`: 1Gi → 768Mi
  - `oauth.limitMemory`: 800Mi → 256Mi
  - `identityserver.limitMemory`: 500Mi → 384Mi

### Removed

- Servicio `api-intern` (ver _Breaking changes_).
