# Ticket Service Roadmap

Roadmap especifico del microservicio `ticket-service` para la primera iteracion MVP.

## Alcance de esta iteracion

- crear tickets
- consultar ticket por id
- listar tickets
- asignar ticket a un agente
- cambiar estado
- exponer health check
- dejar preparados contratos de salida para asignacion y auditoria

## Convenciones

- `HU`: historia de usuario o historia tecnica
- `DoD`: Definition of Done
- el orden propuesto va de base tecnica a flujo funcional

## Fase 1 - Base tecnica y estructura

### HU-TS-01 Estructura base hexagonal

**Como** desarrollador de `ticket-service`  
**Quiero** organizar el proyecto en dominio, aplicacion, puertos y adaptadores  
**Para** mantener la logica del ticket desacoplada de Spring, JPA y HTTP

**DoD**

- existe estructura de paquetes alineada al enfoque hexagonal
- `domain`, `application` e `infrastructure` quedan claramente separados
- los casos de uso entran por `port.in`
- las dependencias externas salen por `port.out`
- el `README` y la documentacion reflejan la estructura real

### HU-TS-02 Configuracion minima local

**Como** desarrollador de `ticket-service`  
**Quiero** configurar el servicio para correr localmente con una base simple  
**Para** poder iterar rapido sobre el MVP sin depender aun de infraestructura externa

**DoD**

- `Spring Boot 3.5.11` y `Java 21` quedan como base actual del proyecto
- la aplicacion arranca localmente
- existe configuracion minima para H2
- existe endpoint `GET /health`
- el proyecto compila con Maven

## Fase 2 - Dominio del ticket

### HU-TS-03 Modelo de dominio inicial

**Como** desarrollador de `ticket-service`  
**Quiero** definir la entidad `Ticket` y sus enums principales  
**Para** representar el flujo central de IssueFlow con reglas claras

**DoD**

- existe la entidad de dominio `Ticket`
- existen `TicketStatus`, `TicketPriority` y `TicketCategory`
- `Ticket` inicia en `OPEN`
- se modelan `createdAt`, `updatedAt`, `resolvedAt` y `closedAt`
- el dominio contiene las reglas basicas de transicion

### HU-TS-04 Reglas de negocio del ciclo de vida

**Como** desarrollador de `ticket-service`  
**Quiero** encapsular las reglas de asignacion y cambio de estado en el dominio  
**Para** evitar que los controladores o adaptadores rompan el flujo del negocio

**DoD**

- no se permite `OPEN -> CLOSED`
- solo se puede cerrar desde `RESOLVED`
- no se puede asignar sin `assigneeId`
- las reglas invalidas generan errores de dominio claros
- los timestamps se actualizan cuando corresponde

## Fase 3 - Casos de uso MVP

### HU-TS-05 Crear ticket

**Como** solicitante  
**Quiero** registrar un ticket con los datos basicos  
**Para** reportar una incidencia o solicitud en el sistema

**DoD**

- existe caso de uso `CreateTicket`
- existe endpoint `POST /tickets`
- se validan campos obligatorios de entrada
- el ticket se persiste con estado `OPEN`
- la respuesta devuelve el ticket creado o su informacion minima necesaria

### HU-TS-06 Consultar ticket por id

**Como** coordinador o agente  
**Quiero** consultar un ticket puntual  
**Para** revisar su estado actual y sus datos principales

**DoD**

- existe caso de uso `GetTicketById`
- existe endpoint `GET /tickets/{id}`
- si el ticket no existe se devuelve error controlado
- la respuesta representa el estado actual del dominio

### HU-TS-07 Listar tickets

**Como** coordinador o agente  
**Quiero** listar tickets registrados  
**Para** tener visibilidad del trabajo pendiente y del estado del sistema

**DoD**

- existe caso de uso `ListTickets`
- existe endpoint `GET /tickets`
- la respuesta devuelve una coleccion consistente
- el caso de uso no depende del framework web

### HU-TS-08 Asignar ticket a agente

**Como** coordinador del sistema  
**Quiero** asignar un ticket a un agente  
**Para** dejar trazabilidad de quien atendera el caso

**DoD**

- existe caso de uso `AssignTicketToAgent`
- existe endpoint `POST /tickets/{id}/assign`
- se valida que exista `assigneeId`
- el ticket pasa a `ASSIGNED` cuando aplica
- la operacion deja listo el punto de integracion con `assignment-service`

### HU-TS-09 Cambiar estado del ticket

**Como** agente o coordinador  
**Quiero** cambiar el estado de un ticket bajo reglas controladas  
**Para** reflejar el avance real del caso sin romper el flujo del negocio

**DoD**

- existe caso de uso `UpdateTicketStatus`
- existe endpoint `PATCH /tickets/{id}/status`
- las transiciones invalidas retornan error controlado
- `resolvedAt` y `closedAt` se actualizan cuando corresponde

## Fase 4 - Persistencia y adaptadores

### HU-TS-10 Persistencia desacoplada

**Como** desarrollador de `ticket-service`  
**Quiero** persistir tickets a traves de un puerto de salida  
**Para** mantener el dominio desacoplado de JPA

**DoD**

- existe `TicketRepository` como puerto de salida
- existe adaptador JPA que implementa ese puerto
- el mapeo entre dominio y persistencia es explicito
- el resto de la aplicacion no depende de entidades JPA

### HU-TS-11 API HTTP inicial

**Como** consumidor interno del sistema  
**Quiero** interactuar con `ticket-service` por HTTP  
**Para** ejecutar el flujo MVP y preparar la futura integracion con el gateway

**DoD**

- existe controlador para `/tickets`
- existen DTOs de request y response
- las validaciones de entrada funcionan
- los errores del dominio se traducen a respuestas HTTP consistentes

## Fase 5 - Contratos externos preparados

### HU-TS-12 Preparar contrato de asignacion

**Como** desarrollador de `ticket-service`  
**Quiero** dejar un puerto de salida para resolver asignaciones  
**Para** integrar luego `assignment-service` sin rehacer la logica central

**DoD**

- existe `AssignmentClient` como puerto de salida
- existe stub o interfaz clara para respuesta esperada
- el servicio puede seguir funcionando aunque el adaptador real no este implementado

### HU-TS-13 Preparar contrato de auditoria

**Como** desarrollador de `ticket-service`  
**Quiero** dejar un puerto de salida para registrar eventos de auditoria  
**Para** integrar luego `notification-audit-service` sin acoplar el dominio

**DoD**

- existe `AuditClient` como puerto de salida
- existe contrato minimo para eventos relevantes
- el flujo local no depende aun de la integracion real

## Orden recomendado de ejecucion

1. HU-TS-01
2. HU-TS-02
3. HU-TS-03
4. HU-TS-04
5. HU-TS-10
6. HU-TS-05
7. HU-TS-06
8. HU-TS-07
9. HU-TS-08
10. HU-TS-09
11. HU-TS-11
12. HU-TS-12
13. HU-TS-13
