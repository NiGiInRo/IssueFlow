# Deployment ECS

## Proposito

Segunda etapa de despliegue usando contenedores administrados en AWS.
Permite separar la aplicacion de la administracion directa de servidores.

## Enfoque

- imagenes publicadas en ECR
- servicios definidos en ECS
- uso recomendado de Fargate
- gateway expuesto mediante balanceo o servicio publico

## Objetivo de aprendizaje

- ECR
- task definitions
- services
- networking basico en ECS
- logs centralizados

## Alcance minimo

- construir y subir imagenes a ECR
- desplegar servicios principales
- exponer gateway
- validar comunicacion entre contenedores
