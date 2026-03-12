# Deployment EKS

## Proposito

Tercera etapa de despliegue para ejecutar la misma solucion sobre Kubernetes.
Permite entender orquestacion mas flexible y portable.

## Enfoque

- reutilizar imagenes desde ECR
- un Deployment por microservicio
- un Service por microservicio
- exposicion del gateway mediante Ingress o LoadBalancer

## Objetivo de aprendizaje

- Pods
- Deployments
- Services
- Ingress
- configuracion distribuida

## Alcance minimo

- manifiestos base por servicio
- variables de entorno y configuracion minima
- conectividad entre pods
- validacion del flujo principal desde el gateway
