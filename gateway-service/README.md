# Gateway Service

## Proposito

Punto unico de entrada para el MVP de IssueFlow.
Recibe las peticiones del cliente y las enruta hacia los servicios internos.

## Responsabilidades iniciales

- exponer endpoints HTTP para el cliente
- redirigir solicitudes al ticket-service
- redirigir solicitudes al assignment-service cuando aplique
- servir como base futura para autenticacion, rate limiting y agregacion

## Stack minimo

- Node.js
- Express o Fastify
- cliente HTTP simple para comunicacion entre servicios

## Alcance MVP

- endpoint de health
- proxy o rutas minimas para tickets
- configuracion por variables de entorno
