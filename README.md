# IssueFlow — Plan del Proyecto (PlantUML)

## 1. Descripción general

**IssueFlow** es un sistema de gestión de tickets e incidencias orientado a pequeñas empresas o equipos que necesitan registrar, asignar, atender y dar seguimiento a solicitudes internas o de soporte.

Este proyecto busca cumplir dos objetivos al mismo tiempo:

- servir como ejercicio técnico serio para practicar microservicios, cloud y despliegue progresivo;
- evolucionar en el futuro hacia un producto u oferta real para gestión operativa.

---

## 2. Visión de negocio

### Problema que resuelve

Muchas organizaciones pequeñas manejan sus solicitudes de soporte o incidentes de forma desordenada:

- WhatsApp
- correos sueltos
- llamadas
- chats dispersos
- hojas de cálculo

Eso genera problemas como:

- falta de trazabilidad
- tickets perdidos
- dificultad para saber quién atiende cada caso
- ausencia de historial
- imposibilidad de medir tiempos de respuesta

### Propuesta de valor

IssueFlow centraliza la operación y permite:

- crear tickets
- asignarlos a agentes
- cambiar su estado
- registrar auditoría e historial
- consultar seguimiento del caso

### Roles principales

- **Solicitante**: crea tickets
- **Agente**: atiende tickets
- **Administrador / Coordinador**: asigna y supervisa tickets

---

## 3. Alcance funcional V1

### Incluye

- crear ticket
- listar tickets
- consultar detalle de ticket
- asignar ticket a un agente
- cambiar estado del ticket
- registrar eventos de auditoría

### No incluye en V1

- autenticación compleja
- dashboard avanzado
- archivos adjuntos
- SLA
- reglas complejas de asignación
- observabilidad avanzada
- CI/CD completo desde el día 1

---

## 4. Microservicios propuestos

### 4.1 Gateway Service — Node.js

**Responsabilidad:**
Punto único de entrada al sistema.

**Funciones:**
- recibir peticiones del cliente
- enrutar hacia otros servicios
- unificar respuestas
- más adelante centralizar autenticación

---

### 4.2 Ticket Service — Spring Boot

**Responsabilidad:**
Corazón del dominio.

**Funciones:**
- crear tickets
- listar tickets
- consultar tickets
- cambiar estado
- aplicar reglas del flujo del ticket
- coordinar asignación y auditoría

**En este servicio se aplicará:**
- arquitectura hexagonal
- casos de uso
- puertos y adaptadores
- separación dominio / aplicación / infraestructura

---

### 4.3 Assignment Service — FastAPI

**Responsabilidad:**
Gestionar asignaciones.

**Funciones:**
- asignar ticket a agente
- validar si agente existe o está habilitado
- devolver resultado de asignación

---

### 4.4 Notification Audit Service — Rails API o Sinatra

**Responsabilidad:**
Registrar eventos y notificaciones simples.

**Funciones:**
- guardar historial
- registrar eventos del sistema
- simular notificaciones futuras

---

## 5. Flujo de negocio

1. El solicitante crea un ticket.
2. El ticket se registra inicialmente como `OPEN`.
3. Un coordinador o regla de negocio solicita asignación.
4. Assignment Service devuelve el agente asignado.
5. Ticket Service actualiza el ticket a `ASSIGNED`.
6. El agente inicia atención y el ticket pasa a `IN_PROGRESS`.
7. El agente resuelve el ticket y pasa a `RESOLVED`.
8. Finalmente el ticket puede pasar a `CLOSED`.
9. Cada cambio importante se registra en auditoría.

### Estados iniciales recomendados

- `OPEN`
- `ASSIGNED`
- `IN_PROGRESS`
- `RESOLVED`
- `CLOSED`

---

## 6. Comunicación entre servicios

### V1 recomendada
- **Gateway → Ticket Service**: HTTP
- **Ticket Service → Assignment Service**: HTTP
- **Ticket Service → Notification Audit Service**: HTTP

### Evolución futura
- mover auditoría y notificaciones a mensajería asíncrona
- usar RabbitMQ o SQS
- desacoplar eventos del flujo principal

---

## 7. Fases del proyecto

## Fase 0 — Definición y alcance

### Objetivo
Definir una versión mínima coherente y realizable.

### Entregables
- visión de negocio
- responsabilidades por servicio
- flujo principal
- límites de la V1

---

## Fase 1 — Diseño funcional y técnico

### Objetivo
Diseñar antes de programar.

### Actividades
- definir entidades
- definir estados
- definir endpoints
- definir contratos entre servicios
- definir estructura de carpetas
- definir bases de datos por servicio

### Entregables
- endpoints
- diagramas
- estructura base del repositorio

---

## Fase 2 — Construcción local

### Objetivo
Tener el sistema funcionando localmente.

### Servicios
- gateway-service
- ticket-service
- assignment-service
- notification-audit-service

### Actividades
- crear proyectos base
- implementar `/health`
- construir endpoints mínimos
- persistencia mínima
- probar comunicación entre servicios

---

## Fase 3 — Dockerización

### Objetivo
Empaquetar correctamente cada servicio.

### Actividades
- Dockerfile por servicio
- variables de entorno
- Docker Compose
- redes internas
- validación local de integración

---

## Fase 4 — Despliegue en EC2

### Objetivo
Aprender el modelo convencional.

### Actividades
- crear instancia EC2
- instalar Docker y Docker Compose
- desplegar todo en una sola VM
- exponer entrada pública
- probar flujo end-to-end

### Aprendizaje
- SSH
- Security Groups
- puertos
- Linux
- despliegue clásico

---

## Fase 5 — Despliegue en ECS

### Objetivo
Llevar la solución a contenedores administrados.

### Actividades
- crear imágenes
- subir imágenes a ECR
- definir tasks
- crear servicios ECS
- exponer gateway
- configurar logs

### Recomendación
Usar **Fargate**

---

## Fase 6 — Despliegue en EKS

### Objetivo
Entender Kubernetes con la misma aplicación.

### Actividades
- reutilizar imágenes desde ECR
- crear Deployment y Service por microservicio
- exponer gateway con Ingress o LoadBalancer
- validar service discovery interno

### Aprendizaje
- Pods
- Services
- Ingress
- Configuración distribuida
- diferencia real frente a ECS

---

## Fase 7 — Evolución posterior

### Posibles mejoras
- mensajería asíncrona
- reglas automáticas de asignación
- métricas
- observabilidad
- CI/CD
- autenticación
- frontend real

---

## 8. Diagrama de secuencia (PlantUML)

```plantuml
@startuml
actor Solicitante
participant "Gateway Service\n(Node.js)" as Gateway
participant "Ticket Service\n(Spring Boot)" as Ticket
participant "Assignment Service\n(FastAPI)" as Assignment
participant "Notification Audit Service\n(Rails/Sinatra)" as Audit

Solicitante -> Gateway : POST /tickets
Gateway -> Ticket : createTicket(request)
Ticket -> Ticket : validar datos
Ticket -> Ticket : guardar ticket OPEN
Ticket -> Audit : registrar evento TICKET_CREATED
Audit --> Ticket : ok
Ticket --> Gateway : ticket creado
Gateway --> Solicitante : 201 Created

Solicitante -> Gateway : POST /tickets/{id}/assign
Gateway -> Ticket : assignTicket(id)
Ticket -> Assignment : assign(ticketId)
Assignment -> Assignment : seleccionar / validar agente
Assignment --> Ticket : agente asignado
Ticket -> Ticket : actualizar estado ASSIGNED
Ticket -> Audit : registrar evento TICKET_ASSIGNED
Audit --> Ticket : ok
Ticket --> Gateway : asignación exitosa
Gateway --> Solicitante : 200 OK

Solicitante -> Gateway : PATCH /tickets/{id}/status IN_PROGRESS
Gateway -> Ticket : updateStatus(id, IN_PROGRESS)
Ticket -> Ticket : validar transición
Ticket -> Ticket : guardar cambio
Ticket -> Audit : registrar evento STATUS_CHANGED
Audit --> Ticket : ok
Ticket --> Gateway : estado actualizado
Gateway --> Solicitante : 200 OK

Solicitante -> Gateway : PATCH /tickets/{id}/status RESOLVED
Gateway -> Ticket : updateStatus(id, RESOLVED)
Ticket -> Ticket : validar transición
Ticket -> Ticket : guardar cambio
Ticket -> Audit : registrar evento TICKET_RESOLVED
Audit --> Ticket : ok
Ticket --> Gateway : ticket resuelto
Gateway --> Solicitante : 200 OK
@enduml