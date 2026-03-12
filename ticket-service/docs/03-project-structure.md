# Ticket Service Project Structure

Estructura propuesta para la primera iteracion de `ticket-service` usando Spring Boot y arquitectura hexagonal simple.

## Base tecnica actual

- `Spring Boot 3.5.11`
- `Java 21`
- Maven
- dependencias actuales: Web, Validation, Data JPA, Actuator, H2 y PostgreSQL driver

## Estructura de paquetes propuesta

```text
src/main/java/com/issueflow/ticket
  TicketServiceApplication.java
  domain/
    model/
      Ticket.java
      TicketStatus.java
      TicketPriority.java
      TicketCategory.java
    exception/
      TicketNotFoundException.java
      InvalidTicketStatusTransitionException.java
      InvalidTicketAssignmentException.java
  application/
    service/
      CreateTicketService.java
      GetTicketByIdService.java
      ListTicketsService.java
      UpdateTicketStatusService.java
      AssignTicketToAgentService.java
  port/
    in/
      CreateTicketUseCase.java
      GetTicketByIdUseCase.java
      ListTicketsUseCase.java
      UpdateTicketStatusUseCase.java
      AssignTicketToAgentUseCase.java
    out/
      TicketRepository.java
      AssignmentClient.java
      AuditClient.java
  infrastructure/
    config/
      RestClientConfig.java
    web/
      HealthController.java
      TicketController.java
      GlobalExceptionHandler.java
      dto/
        CreateTicketRequest.java
        AssignTicketRequest.java
        UpdateTicketStatusRequest.java
        TicketResponse.java
        TicketSummaryResponse.java
        ErrorResponse.java
    persistence/
      jpa/
        TicketJpaEntity.java
        SpringDataTicketRepository.java
      adapter/
        JpaTicketRepositoryAdapter.java
      mapper/
        TicketPersistenceMapper.java
    client/
      stub/
        StubAssignmentClient.java
        StubAuditClient.java
```

## Responsabilidad por capa

### `domain`

- contiene las reglas del negocio
- no depende de Spring ni de JPA
- define el comportamiento del `Ticket`

### `application`

- implementa casos de uso
- orquesta el dominio
- usa puertos de entrada y salida

### `port.in`

- define contratos de entrada para cada caso de uso
- sirve como frontera entre web y aplicacion

### `port.out`

- define contratos hacia persistencia y servicios externos

### `infrastructure`

- adapta HTTP, JPA y clientes externos al modelo interno
- concentra configuracion de framework

## Entidades y enums iniciales

### `Ticket`

Campos iniciales:

- `id`
- `title`
- `description`
- `status`
- `priority`
- `category`
- `requesterId`
- `assigneeId`
- `createdAt`
- `updatedAt`
- `resolvedAt`
- `closedAt`

### `TicketStatus`

- `OPEN`
- `ASSIGNED`
- `IN_PROGRESS`
- `RESOLVED`
- `CLOSED`

### `TicketPriority`

- `LOW`
- `MEDIUM`
- `HIGH`
- `CRITICAL`

### `TicketCategory`

- `SOFTWARE`
- `HARDWARE`
- `ACCESS`
- `NETWORK`
- `OTHER`

## Casos de uso iniciales

- `CreateTicketUseCase`
- `GetTicketByIdUseCase`
- `ListTicketsUseCase`
- `UpdateTicketStatusUseCase`
- `AssignTicketToAgentUseCase`

## Decisiones para mantener el MVP limpio

- sin seguridad en esta iteracion
- sin paginacion ni filtros avanzados
- sin eventos asincronos todavia
- sin validacion real contra microservicios externos
- con stubs para asignacion y auditoria
- con H2 para desarrollo local
