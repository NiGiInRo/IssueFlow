# Architecture

## Stack objetivo

- Gateway Service: Node.js
- Ticket Service: Spring Boot
- Assignment Service: FastAPI
- Notification Audit Service: Rails API o Sinatra

## Ruta tecnica

- construccion local
- dockerizacion con Docker Compose
- despliegue en EC2
- migracion a ECS con ECR y Fargate
- migracion posterior a EKS

## Microservicios

### Gateway Service

Punto unico de entrada al sistema.
Recibe peticiones del cliente y las enruta hacia los servicios internos.

Responsabilidades iniciales:

- exponer endpoints HTTP
- enrutar solicitudes
- unificar acceso para cliente externo
- servir como base futura para autenticacion y agregacion

### Ticket Service

Corazon del dominio.
Aqui conviene aplicar arquitectura hexagonal.

Responsabilidades iniciales:

- crear tickets
- listar tickets
- consultar tickets por id
- cambiar estado
- coordinar asignacion
- coordinar auditoria

### Assignment Service

Gestiona asignaciones y validaciones basicas de agentes.

Responsabilidades iniciales:

- listar agentes disponibles
- asignar ticket a agente
- validar disponibilidad o existencia de agente
- mantener historial basico de asignaciones

### Notification Audit Service

Registra eventos relevantes del sistema y mantiene el historial auditable.

Responsabilidades iniciales:

- registrar eventos de auditoria
- listar eventos por ticket
- servir como base para notificaciones futuras

## Comunicacion entre servicios

Version inicial recomendada:

- Gateway -> Ticket Service: HTTP
- Ticket Service -> Assignment Service: HTTP
- Ticket Service -> Notification Audit Service: HTTP

Evolucion futura:

- mover auditoria y notificaciones a mensajeria asincrona
- usar RabbitMQ o SQS
- desacoplar eventos del flujo principal

## Arquitectura base sugerida

Se recomienda aplicar arquitectura hexagonal principalmente en `ticket-service`.
Los otros servicios deben mantenerse simples y bien delimitados para no sobrecargar la primera iteracion.
