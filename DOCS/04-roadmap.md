# Roadmap

## Fase 0 - Definicion y alcance

Definir una version minima coherente y realizable.

Entregables:

- vision de negocio
- responsabilidades por servicio
- flujo principal
- limites de la V1

## Fase 1 - Diseno funcional y tecnico

Definir antes de programar.

Actividades:

- definir entidades
- definir estados
- definir endpoints
- definir contratos entre servicios
- definir estructura del repositorio
- definir persistencia por servicio

## Fase 2 - Construccion local

Levantar los cuatro servicios localmente.

Actividades:

- crear proyectos base
- implementar health checks
- construir endpoints minimos
- agregar persistencia minima
- probar comunicacion entre servicios

## Fase 3 - Dockerizacion

Empaquetar cada servicio y orquestar el entorno local.

Actividades:

- Dockerfile por servicio
- variables de entorno
- Docker Compose
- redes internas
- validacion de integracion local

## Fase 4 - Despliegue en EC2

Desplegar toda la solucion en una sola VM.

Aprendizaje esperado:

- SSH
- Security Groups
- puertos
- Linux
- despliegue clasico con Docker Compose

## Fase 5 - Despliegue en ECS

Subir imagenes a ECR y desplegar servicios administrados, idealmente con Fargate.

## Fase 6 - Despliegue en EKS

Reutilizar imagenes desde ECR y desplegar con Kubernetes.

## Fase 7 - Evolucion posterior

Mejoras futuras:

- mensajeria asincrona
- reglas automaticas de asignacion
- observabilidad
- autenticacion
- CI/CD
- frontend real

## Objetivo inmediato

Cerrar un MVP funcional que permita demostrar:

- modelado de dominio
- diseno de APIs
- integracion entre servicios
- contenedorizacion
- despliegue progresivo en AWS
