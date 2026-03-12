# Project Roadmap

Roadmap general de IssueFlow pensado como referencia de ejecucion por fases.
Este documento sirve como backlog base para abrir chats separados por microservicio o por etapa sin perder consistencia.

## Como usar este roadmap

- usar cada historia como unidad de conversacion o trabajo
- derivar subtareas tecnicas por microservicio a partir de cada historia
- no implementar todo al mismo tiempo; cerrar historias en orden
- marcar una historia como cerrada solo cuando cumpla su DoD

## Convenciones

- `HU`: historia de usuario o historia tecnica
- `DoD`: Definition of Done
- el enfoque es MVP primero, complejidad despues

## Fase 0 - Fundacion del proyecto

### HU-00.1 Vision y alcance

**Como** desarrollador del proyecto  
**Quiero** definir una version minima coherente de IssueFlow  
**Para** construir un MVP realista sin mezclar objetivos de aprendizaje con sobrearquitectura

**DoD**

- alcance V1 documentado
- exclusiones de V1 documentadas
- roles principales definidos
- flujo principal documentado
- orden de desarrollo de microservicios definido

### HU-00.2 Estructura documental

**Como** desarrollador del proyecto  
**Quiero** organizar la documentacion por temas y por etapas  
**Para** poder trabajar el proyecto en sesiones separadas sin perder trazabilidad tecnica

**DoD**

- `README.md` raiz reducido a contexto minimo
- carpeta `DOCS/` creada como fuente principal de documentacion
- vision, arquitectura, dominio, roadmap y deployments documentados por separado
- diagramas versionados dentro de `DOCS/diagrams`

### HU-00.3 Estructura de monorepo

**Como** desarrollador del proyecto  
**Quiero** definir un monorepo con un espacio aislado por microservicio  
**Para** desarrollar cada componente en orden sin mezclar configuraciones ni dependencias

**DoD**

- carpetas de los cuatro microservicios creadas
- cada carpeta tiene un `README.md` con proposito y stack minimo
- documentacion general enlaza la estructura del monorepo

## Fase 1 - Diseno funcional y tecnico

### HU-01.1 Dominio principal del ticket

**Como** desarrollador del proyecto  
**Quiero** modelar la entidad `Ticket`, sus estados y reglas de transicion  
**Para** establecer el dominio central sobre el que se apoyaran los demas microservicios

**DoD**

- entidad `Ticket` definida a nivel documental
- enums de estado, prioridad y categoria definidos
- transiciones validas documentadas
- reglas invalidas documentadas
- casos de uso principales del ticket listados

### HU-01.2 Contratos API iniciales

**Como** desarrollador del proyecto  
**Quiero** definir endpoints minimos por servicio  
**Para** construir cada microservicio contra contratos claros y reducir reprocesos

**DoD**

- endpoints MVP del `ticket-service` definidos
- endpoints MVP del `assignment-service` definidos
- endpoints MVP del `notification-audit-service` definidos
- responsabilidades del `gateway-service` delimitadas

### HU-01.3 Arquitectura hexagonal del ticket-service

**Como** desarrollador del proyecto  
**Quiero** separar dominio, casos de uso, puertos y adaptadores en `ticket-service`  
**Para** proteger la logica principal del framework y de detalles de infraestructura

**DoD**

- capas esperadas documentadas
- puertos de entrada y salida listados
- adaptadores esperados documentados
- estructura base sugerida del servicio definida

### HU-01.4 Diagramas base del sistema

**Como** desarrollador del proyecto  
**Quiero** documentar la vista de componentes y el flujo principal del sistema  
**Para** tener una referencia comun antes de comenzar a programar

**DoD**

- diagrama de componentes creado
- diagrama de secuencia creado
- ambos reflejan el flujo MVP actual
- ambos estan versionados dentro de `DOCS/diagrams`

## Fase 2 - Ticket Service

### HU-02.1 Crear tickets

**Como** solicitante  
**Quiero** registrar un ticket con datos basicos  
**Para** reportar una incidencia o solicitud en el sistema

**DoD**

- existe endpoint para crear ticket
- el ticket se persiste con estado `OPEN`
- se validan campos obligatorios
- se devuelve identificador del ticket creado
- el caso de uso esta separado del controlador HTTP

### HU-02.2 Consultar tickets

**Como** coordinador o agente  
**Quiero** listar tickets y consultar su detalle  
**Para** dar seguimiento al trabajo pendiente y al estado de cada caso

**DoD**

- existe endpoint para listar tickets
- existe endpoint para consultar ticket por id
- la consulta devuelve datos minimos del dominio
- el acceso a persistencia esta desacoplado mediante puerto o repositorio definido

### HU-02.3 Cambiar estado del ticket

**Como** agente o coordinador  
**Quiero** actualizar el estado de un ticket bajo reglas controladas  
**Para** reflejar el avance real del caso sin romper el flujo del negocio

**DoD**

- existe endpoint para cambio de estado
- se validan transiciones permitidas
- las transiciones invalidas retornan error controlado
- se actualizan timestamps relevantes cuando aplique

### HU-02.4 Preparar integraciones externas

**Como** `ticket-service`  
**Quiero** exponer puertos de salida para asignacion y auditoria  
**Para** integrarme despues con otros microservicios sin acoplar la logica del dominio

**DoD**

- existe puerto de salida para `AssignmentClient`
- existe puerto de salida para `AuditClient`
- los puertos estan definidos a nivel de aplicacion o dominio segun el diseno adoptado
- la implementacion concreta puede quedar pendiente sin romper el servicio

## Fase 3 - Assignment Service

### HU-03.1 Mantener agentes disponibles

**Como** coordinador del sistema  
**Quiero** consultar agentes disponibles para asignacion  
**Para** elegir o resolver a quien debe atender un ticket

**DoD**

- existe endpoint para consultar agentes
- el modelo `Agent` esta definido
- el servicio responde con estructura consistente para integracion

### HU-03.2 Asignar ticket a agente

**Como** coordinador del sistema  
**Quiero** asignar un ticket a un agente valido  
**Para** dejar trazabilidad de quien atendera el caso

**DoD**

- existe endpoint de asignacion
- el servicio valida existencia o disponibilidad del agente
- se registra la asignacion con estado inicial
- el contrato de respuesta sirve para actualizar el ticket en `ticket-service`

## Fase 4 - Notification Audit Service

### HU-04.1 Registrar auditoria

**Como** sistema IssueFlow  
**Quiero** registrar eventos relevantes del ciclo de vida del ticket  
**Para** mantener trazabilidad auditable del flujo de negocio

**DoD**

- existe endpoint para registrar eventos
- el modelo `AuditEvent` esta definido
- se persisten eventos con tipo, ticketId, actor y fecha
- el contrato de entrada es estable para integracion con `ticket-service`

### HU-04.2 Consultar historial de auditoria

**Como** coordinador o auditor  
**Quiero** consultar los eventos de un ticket  
**Para** revisar su historial operativo y sus cambios de estado

**DoD**

- existe endpoint para consultar eventos por ticket
- los eventos se devuelven ordenados de forma consistente
- la respuesta permite reconstruir el historial principal del ticket

## Fase 5 - Gateway Service

### HU-05.1 Centralizar entrada HTTP

**Como** consumidor del sistema  
**Quiero** interactuar con un unico punto de entrada  
**Para** no depender de conocer la ubicacion interna de cada microservicio

**DoD**

- existe `gateway-service` con endpoint de health
- el gateway expone rutas minimas del MVP
- las solicitudes se enrutan al microservicio correcto
- los errores de backend se traducen a respuestas HTTP consistentes

### HU-05.2 Exponer flujo MVP completo

**Como** consumidor del sistema  
**Quiero** ejecutar el flujo principal desde el gateway  
**Para** probar el sistema como una sola aplicacion aunque internamente use varios microservicios

**DoD**

- crear ticket funciona desde el gateway
- asignar ticket funciona desde el gateway
- cambiar estado funciona desde el gateway
- consultar ticket funciona desde el gateway
- consultar auditoria funciona desde el gateway o queda claramente delimitado si no entra en esta iteracion

## Fase 6 - Integracion local y contenedorizacion

### HU-06.1 Integracion local entre microservicios

**Como** desarrollador del proyecto  
**Quiero** ejecutar e integrar los microservicios localmente  
**Para** validar el flujo funcional antes de desplegar en AWS

**DoD**

- cada microservicio tiene endpoint `/health`
- los contratos HTTP entre servicios funcionan localmente
- el flujo principal puede ejecutarse end-to-end
- variables de entorno minimas estan definidas por servicio

### HU-06.2 Dockerizacion del sistema

**Como** desarrollador del proyecto  
**Quiero** contenerizar cada microservicio y orquestarlos con Docker Compose  
**Para** tener un entorno reproducible y listo para la primera fase de despliegue

**DoD**

- existe un `Dockerfile` por servicio
- existe `docker-compose` para el entorno local integrado
- los servicios levantan con configuracion minima
- el flujo MVP funciona dentro del entorno orquestado

## Fase 7 - Despliegue

### HU-07.1 Desplegar en EC2

**Como** desarrollador del proyecto  
**Quiero** desplegar el MVP en una instancia EC2  
**Para** aprender el modelo clasico de despliegue en AWS con una aplicacion real

**DoD**

- instancia EC2 provisionada
- Docker y Compose instalados en la VM
- servicios desplegados y accesibles segun el alcance definido
- flujo principal validado en entorno remoto

### HU-07.2 Desplegar en ECS

**Como** desarrollador del proyecto  
**Quiero** mover el MVP a ECS con imagenes en ECR  
**Para** aprender el modelo de contenedores administrados en AWS

**DoD**

- imagenes publicadas en ECR
- tasks y services definidos en ECS
- gateway expuesto segun el diseno de red adoptado
- flujo principal validado en ECS

### HU-07.3 Desplegar en EKS

**Como** desarrollador del proyecto  
**Quiero** desplegar la misma solucion sobre Kubernetes en EKS  
**Para** comparar el enfoque de ECS con una orquestacion mas flexible y portable

**DoD**

- manifiestos base creados por microservicio
- imagenes reutilizadas desde ECR
- gateway expuesto mediante Service o Ingress
- flujo principal validado en EKS

## Fase 8 - Cierre de MVP y evolucion

### HU-08.1 Consolidar MVP demostrable

**Como** desarrollador del proyecto  
**Quiero** cerrar una version demostrable del sistema  
**Para** usarla como evidencia de aprendizaje, portafolio y base de evolucion futura

**DoD**

- README raiz actualizado y coherente
- documentacion principal alineada con el estado real del proyecto
- flujo MVP demostrable de punta a punta
- backlog de mejoras posteriores identificado

### HU-08.2 Preparar evolucion posterior

**Como** desarrollador del proyecto  
**Quiero** identificar mejoras no incluidas en V1  
**Para** evolucionar el sistema sin contaminar el alcance del MVP actual

**DoD**

- mejoras futuras listadas por categoria
- dependencias tecnicas para cada mejora identificadas a alto nivel
- backlog posterior separado del backlog MVP
