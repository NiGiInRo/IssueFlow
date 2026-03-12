# Notification Audit Service

## Proposito

Registrar eventos relevantes del sistema para dejar trazabilidad del flujo.
En el MVP se enfoca en auditoria y deja notificaciones como extension futura.

## Responsabilidades iniciales

- registrar eventos de auditoria
- listar eventos por ticket
- simular una base para futuras notificaciones

## Stack minimo

- Ruby
- Sinatra o Rails API
- persistencia simple para eventos

## Alcance MVP

- endpoint de health
- endpoint para registrar evento
- endpoint para consultar eventos por ticket
- modelo basico de AuditEvent
