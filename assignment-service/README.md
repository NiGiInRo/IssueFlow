# Assignment Service

## Proposito

Resolver la asignacion de tickets a agentes.
Mantiene la logica basica para seleccionar, validar y registrar asignaciones.

## Responsabilidades iniciales

- listar agentes disponibles
- asignar un agente a un ticket
- validar si un agente existe o esta habilitado
- guardar historial basico de asignaciones

## Stack minimo

- Python
- FastAPI
- Pydantic
- SQLAlchemy o persistencia simple inicial

## Alcance MVP

- endpoint de health
- endpoint para asignar ticket
- endpoint para consultar agentes
- logica simple de asignacion
