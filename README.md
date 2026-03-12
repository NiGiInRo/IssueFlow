# IssueFlow

Monorepo para un sistema de gestion de tickets e incidencias orientado a aprendizaje practico de microservicios y despliegue progresivo en AWS.

## Servicios

- Gateway Service: Node.js
- Ticket Service: Spring Boot
- Assignment Service: FastAPI
- Notification Audit Service: Ruby con Sinatra o Rails API

## Orden recomendado de desarrollo

1. ticket-service
2. assignment-service
3. notification-audit-service
4. gateway-service

## Estructura

```text
flow-tickets-admin/
  DOCS/
  gateway-service/
  ticket-service/
  assignment-service/
  notification-audit-service/
```

## Documentacion

La documentacion general del proyecto esta en [DOCS/README.md](DOCS/README.md).
Cada servicio mantiene su README propio dentro de su carpeta.
