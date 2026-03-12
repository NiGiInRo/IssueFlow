# Ticket Service

## Proposito

`ticket-service` es el primer microservicio a desarrollar y el nucleo del dominio de IssueFlow.
Aqui vive la logica principal del sistema: tickets, estados, reglas de transicion y coordinacion del flujo con otros servicios.

## Por que va primero

- define el dominio principal
- fija las reglas del flujo del ticket
- establece contratos que luego consumen otros microservicios
- es el mejor punto para aplicar arquitectura hexagonal desde el inicio

## Responsabilidades iniciales

- crear tickets
- listar tickets
- consultar ticket por id
- asignar ticket a un agente
- cambiar estado del ticket
- coordinar registro de eventos de auditoria

## Stack minimo

- Java
- Spring Boot
- Spring Web
- Spring Data JPA
- base de datos relacional simple
- cliente HTTP para comunicacion con otros servicios

## Enfoque arquitectonico

Este servicio debe implementar arquitectura hexagonal.
La idea es proteger el dominio del framework, de la base de datos y de clientes HTTP externos.

### Capas esperadas

- `domain`: entidades, enums y reglas del negocio
- `application`: casos de uso
- `port.in`: contratos de entrada
- `port.out`: contratos de salida
- `infrastructure`: controladores, persistencia y clientes externos

## Dominio minimo esperado

### Entidad principal

- `Ticket`

### Enums iniciales

- `TicketStatus`
- `TicketPriority`
- `TicketCategory`

### Estados recomendados

- `OPEN`
- `ASSIGNED`
- `IN_PROGRESS`
- `RESOLVED`
- `CLOSED`

### Reglas basicas

- un ticket inicia en `OPEN`
- no debe pasar de `OPEN` a `CLOSED` directamente
- solo puede cerrarse si esta en `RESOLVED`
- solo puede asignarse si existe `assigneeId`

## Casos de uso MVP

- `CreateTicket`
- `GetTicketById`
- `ListTickets`
- `UpdateTicketStatus`
- `AssignTicketToAgent`

## Integraciones esperadas

### Assignment Service

Se usa para resolver la asignacion del ticket a un agente.

### Notification Audit Service

Se usa para registrar eventos relevantes del flujo del ticket.

## Estructura base sugerida

```text
ticket-service/
  src/main/java/com/issueflow/ticket/
    domain/
      model/
        Ticket.java
        TicketStatus.java
        TicketPriority.java
        TicketCategory.java
      port/
        in/
          CreateTicketUseCase.java
          GetTicketUseCase.java
          ListTicketsUseCase.java
          UpdateTicketStatusUseCase.java
          AssignTicketUseCase.java
        out/
          TicketRepository.java
          AssignmentClient.java
          AuditClient.java
    application/
      service/
        CreateTicketService.java
        GetTicketService.java
        ListTicketsService.java
        UpdateTicketStatusService.java
        AssignTicketService.java
    infrastructure/
      persistence/
      web/
      client/
```

## Endpoints minimos esperados

- `GET /health`
- `POST /tickets`
- `GET /tickets`
- `GET /tickets/{id}`
- `POST /tickets/{id}/assign`
- `PATCH /tickets/{id}/status`

## Objetivo de esta primera iteracion

Tener un servicio local funcional que permita:

- persistir tickets
- validar transiciones de estado
- exponer endpoints base
- preparar los contratos para integrarse luego con `assignment-service` y `notification-audit-service`
