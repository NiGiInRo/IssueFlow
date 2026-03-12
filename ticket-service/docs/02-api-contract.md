# Ticket Service API Contract

Contrato HTTP inicial del MVP de `ticket-service`.

## Principios

- API REST simple
- JSON como formato de intercambio
- errores funcionales controlados
- sin paginacion ni filtros avanzados en esta primera iteracion

## Endpoints MVP

### GET /health

Verifica que el servicio este activo.

**Respuesta 200**

```json
{
  "status": "UP",
  "service": "ticket-service"
}
```

### POST /tickets

Crea un nuevo ticket.

**Request**

```json
{
  "title": "No puedo acceder al correo",
  "description": "El usuario no puede iniciar sesion en Outlook",
  "priority": "HIGH",
  "category": "ACCESS",
  "requesterId": "user-123"
}
```

**Reglas**

- `title` es obligatorio
- `description` es obligatoria
- `priority` es obligatoria
- `category` es obligatoria
- `requesterId` es obligatorio
- `status` no llega desde el cliente; siempre inicia en `OPEN`

**Respuesta 201**

```json
{
  "id": "7d2964b1-3ef7-4e08-81f7-b2c3b11f8ad7",
  "title": "No puedo acceder al correo",
  "description": "El usuario no puede iniciar sesion en Outlook",
  "status": "OPEN",
  "priority": "HIGH",
  "category": "ACCESS",
  "requesterId": "user-123",
  "assigneeId": null,
  "createdAt": "2026-03-11T21:30:00Z",
  "updatedAt": "2026-03-11T21:30:00Z",
  "resolvedAt": null,
  "closedAt": null
}
```

### GET /tickets

Lista tickets registrados.

**Respuesta 200**

```json
[
  {
    "id": "7d2964b1-3ef7-4e08-81f7-b2c3b11f8ad7",
    "title": "No puedo acceder al correo",
    "status": "OPEN",
    "priority": "HIGH",
    "category": "ACCESS",
    "requesterId": "user-123",
    "assigneeId": null,
    "createdAt": "2026-03-11T21:30:00Z",
    "updatedAt": "2026-03-11T21:30:00Z"
  }
]
```

### GET /tickets/{id}

Consulta un ticket por identificador.

**Respuesta 200**

```json
{
  "id": "7d2964b1-3ef7-4e08-81f7-b2c3b11f8ad7",
  "title": "No puedo acceder al correo",
  "description": "El usuario no puede iniciar sesion en Outlook",
  "status": "OPEN",
  "priority": "HIGH",
  "category": "ACCESS",
  "requesterId": "user-123",
  "assigneeId": null,
  "createdAt": "2026-03-11T21:30:00Z",
  "updatedAt": "2026-03-11T21:30:00Z",
  "resolvedAt": null,
  "closedAt": null
}
```

**Respuesta 404**

```json
{
  "message": "Ticket not found",
  "code": "TICKET_NOT_FOUND"
}
```

### POST /tickets/{id}/assign

Asigna el ticket a un agente.

**Request**

```json
{
  "assigneeId": "agent-001"
}
```

**Reglas**

- `assigneeId` es obligatorio
- si el ticket esta en `OPEN`, pasa a `ASSIGNED`
- esta iteracion no valida aun al agente contra `assignment-service`

**Respuesta 200**

```json
{
  "id": "7d2964b1-3ef7-4e08-81f7-b2c3b11f8ad7",
  "status": "ASSIGNED",
  "assigneeId": "agent-001",
  "updatedAt": "2026-03-11T21:35:00Z"
}
```

### PATCH /tickets/{id}/status

Actualiza el estado del ticket.

**Request**

```json
{
  "status": "IN_PROGRESS"
}
```

**Transiciones permitidas**

- `OPEN -> ASSIGNED`
- `ASSIGNED -> IN_PROGRESS`
- `IN_PROGRESS -> RESOLVED`
- `RESOLVED -> CLOSED`

**Transiciones invalidas destacadas**

- `OPEN -> CLOSED`
- `OPEN -> RESOLVED`
- `ASSIGNED -> CLOSED`
- `IN_PROGRESS -> CLOSED`

**Respuesta 200**

```json
{
  "id": "7d2964b1-3ef7-4e08-81f7-b2c3b11f8ad7",
  "status": "IN_PROGRESS",
  "updatedAt": "2026-03-11T21:40:00Z",
  "resolvedAt": null,
  "closedAt": null
}
```

**Respuesta 400**

```json
{
  "message": "Invalid ticket status transition",
  "code": "INVALID_TICKET_STATUS_TRANSITION"
}
```

## DTOs iniciales

### CreateTicketRequest

- `title: String`
- `description: String`
- `priority: TicketPriority`
- `category: TicketCategory`
- `requesterId: String`

### AssignTicketRequest

- `assigneeId: String`

### UpdateTicketStatusRequest

- `status: TicketStatus`

### TicketResponse

- `id: UUID`
- `title: String`
- `description: String`
- `status: TicketStatus`
- `priority: TicketPriority`
- `category: TicketCategory`
- `requesterId: String`
- `assigneeId: String | null`
- `createdAt: Instant`
- `updatedAt: Instant`
- `resolvedAt: Instant | null`
- `closedAt: Instant | null`

## Errores iniciales

- `TICKET_NOT_FOUND`
- `INVALID_TICKET_STATUS_TRANSITION`
- `INVALID_TICKET_ASSIGNMENT`
- `VALIDATION_ERROR`

## Contratos externos que quedan como stubs

### AssignmentClient

Contrato de salida para futura validacion o resolucion de agente.

Posible operacion inicial:

- `assignAgent(ticketId, category, priority): AssignmentResult`

### AuditClient

Contrato de salida para futura auditoria de eventos del ticket.

Posibles eventos iniciales:

- `TICKET_CREATED`
- `TICKET_ASSIGNED`
- `STATUS_CHANGED`
- `TICKET_RESOLVED`
- `TICKET_CLOSED`
