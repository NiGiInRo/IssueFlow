# Domain

## Flujo de negocio principal

1. El solicitante crea un ticket.
2. El ticket inicia en OPEN.
3. El coordinador o una regla solicita asignacion.
4. Assignment Service devuelve el agente asignado.
5. Ticket Service actualiza el ticket a ASSIGNED.
6. El agente inicia atencion y el ticket pasa a IN_PROGRESS.
7. El agente resuelve el ticket y pasa a RESOLVED.
8. Finalmente el ticket puede pasar a CLOSED.
9. Cada cambio relevante se registra en auditoria.

## Estados recomendados

- OPEN
- ASSIGNED
- IN_PROGRESS
- RESOLVED
- CLOSED

Transiciones recomendadas:

- OPEN -> ASSIGNED
- ASSIGNED -> IN_PROGRESS
- IN_PROGRESS -> RESOLVED
- RESOLVED -> CLOSED

## Entidades de dominio

### Ticket Service

- Ticket
- TicketComment (opcional en una version posterior)

Atributos sugeridos para Ticket:

- id
- title
- description
- status
- priority
- category
- requesterId
- assigneeId
- createdAt
- updatedAt
- resolvedAt
- closedAt

Reglas basicas:

- inicia en OPEN
- no debe pasar de OPEN a CLOSED directamente
- no se puede cerrar si no esta en RESOLVED
- no se puede asignar sin assigneeId

### Assignment Service

- Agent
- Assignment

### Notification Audit Service

- AuditEvent
- NotificationLog (opcional en V1)

## Objetos de valor sugeridos

### TicketStatus

- OPEN
- ASSIGNED
- IN_PROGRESS
- RESOLVED
- CLOSED

### TicketPriority

- LOW
- MEDIUM
- HIGH
- CRITICAL

### TicketCategory

- SOFTWARE
- HARDWARE
- ACCESS
- NETWORK
- OTHER

### AssignmentStatus

- ACTIVE
- REASSIGNED
- REMOVED

### AuditEventType

- TICKET_CREATED
- TICKET_ASSIGNED
- STATUS_CHANGED
- TICKET_RESOLVED
- TICKET_CLOSED

## Casos de uso principales

### Ticket Service

- CreateTicket
- GetTicketById
- ListTickets
- UpdateTicketStatus
- AssignTicketToAgent

### Assignment Service

- AssignAgentToTicket
- GetAgentById
- ListAvailableAgents

### Notification Audit Service

- RegisterAuditEvent
- ListAuditEventsByTicket
