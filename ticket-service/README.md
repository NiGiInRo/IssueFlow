# Ticket Service

Microservicio central de IssueFlow. Aqui vive el dominio principal del sistema: tickets, estados, reglas de transicion y coordinacion inicial con asignacion y auditoria.

## Estado actual

Proyecto base generado con Spring Initializr.

- `Spring Boot 3.5.11`
- `Java 21`
- Maven
- dependencias actuales: `Spring Web`, `Spring Data JPA`, `Validation`, `Actuator`, `H2`, `PostgreSQL Driver`

## Objetivo del MVP

En esta primera iteracion el servicio debe:

- crear tickets
- listar tickets
- consultar ticket por id
- asignar ticket a un agente
- cambiar estado
- exponer `GET /health`
- dejar stubs para auditoria y asignacion

## Reglas minimas del dominio

- un ticket inicia en `OPEN`
- no puede pasar de `OPEN` a `CLOSED` directamente
- solo puede cerrarse si esta en `RESOLVED`
- solo puede asignarse si existe `assigneeId`

## Estados iniciales

- `OPEN`
- `ASSIGNED`
- `IN_PROGRESS`
- `RESOLVED`
- `CLOSED`

## Estructura arquitectonica buscada

- `domain`
- `application`
- `port.in`
- `port.out`
- `infrastructure`

## Endpoints MVP

- `GET /health`
- `POST /tickets`
- `GET /tickets`
- `GET /tickets/{id}`
- `POST /tickets/{id}/assign`
- `PATCH /tickets/{id}/status`

## Documentacion especifica

- [docs/README.md](C:/Users/Nicolas/Documents/projects/flow-tickets-admin/ticket-service/docs/README.md)
- [docs/01-roadmap.md](C:/Users/Nicolas/Documents/projects/flow-tickets-admin/ticket-service/docs/01-roadmap.md)
- [docs/02-api-contract.md](C:/Users/Nicolas/Documents/projects/flow-tickets-admin/ticket-service/docs/02-api-contract.md)
- [docs/03-project-structure.md](C:/Users/Nicolas/Documents/projects/flow-tickets-admin/ticket-service/docs/03-project-structure.md)

## Orden recomendado de trabajo

1. cerrar contrato API inicial
2. crear estructura hexagonal base
3. modelar `Ticket` y reglas del dominio
4. implementar persistencia desacoplada
5. implementar casos de uso MVP
6. exponer controladores HTTP y manejo de errores
7. dejar stubs para `AssignmentClient` y `AuditClient`
