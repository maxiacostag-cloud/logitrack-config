# logitrack-config

Repositorio de **configuración centralizada** de LogiTrack (Sprint 2).
El `config-server` del proyecto [daas-proyecto-final](https://github.com/maxiacostag-cloud/daas-proyecto-final)
lee estos archivos y se los entrega a cada microservicio cuando arranca.

## Archivos

| Archivo | Para qué servicio | Cuándo se usa |
|---|---|---|
| `application.yml` | Todos | Siempre (actuator, logging y el JWT como variable de entorno) |
| `shipment-service.yml` | shipment-service | Siempre |
| `shipment-service-dev.yml` / `-prod.yml` | shipment-service | Según el perfil activo |
| `courier-service.yml` | courier-service | Siempre |
| `courier-service-dev.yml` / `-prod.yml` | courier-service | Según el perfil activo |
| `security-service.yml` | security-service | Siempre |
| `security-service-dev.yml` / `-prod.yml` | security-service | Según el perfil activo |

Spring junta los archivos en este orden (el último pisa al anterior):
`application.yml` → `<servicio>.yml` → `<servicio>-<perfil>.yml`.

## Perfiles

| Perfil | Dónde corre la app | Cómo llega a MySQL y a los otros servicios |
|---|---|---|
| `dev` | En el IDE | `localhost` con los puertos publicados por Docker (3307, 3308 y 3309) |
| `prod` | En Docker Compose | Por nombre de servicio (`mysql-shipment`, `courier-service`, etc.) |

## Secretos

Este repositorio **no tiene contraseñas ni claves**. Los valores sensibles se escriben como
`${VARIABLE}` y cada microservicio los toma de sus variables de entorno:

| Variable | Qué es |
|---|---|
| `DB_PASSWORD` | Contraseña de MySQL |
| `JWT_SECRET` | Clave con la que se firman y validan los JWT |

## Cómo probarlo

Con el `config-server` levantado (puerto 8888):

```
http://localhost:8888/shipment-service/dev
http://localhost:8888/courier-service/prod
http://localhost:8888/security-service/dev
```

Cada URL devuelve las propiedades que recibe ese servicio en ese perfil.
