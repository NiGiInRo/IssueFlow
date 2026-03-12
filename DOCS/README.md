# DOCS

Indice de documentacion general de IssueFlow.

## Orden recomendado de desarrollo

1. `ticket-service`
   Razon: concentra el dominio principal, los estados del ticket y las reglas del flujo.
2. `assignment-service`
   Razon: depende del flujo del ticket y resuelve la asignacion sin bloquear el modelado principal.
3. `notification-audit-service`
   Razon: es una dependencia saliente del dominio y puede implementarse simple una vez estabilizados los eventos.
4. `gateway-service`
   Razon: es un adaptador de entrada; conviene montarlo al final cuando los contratos internos ya esten claros.

## Documentos

- [01-vision-and-scope.md](01-vision-and-scope.md)
- [02-architecture.md](02-architecture.md)
- [03-domain.md](03-domain.md)
- [04-roadmap.md](04-roadmap.md)

## Deployments

- [deployments/01-ec2.md](deployments/01-ec2.md)
- [deployments/02-ecs.md](deployments/02-ecs.md)
- [deployments/03-eks.md](deployments/03-eks.md)

## Diagramas

- [diagrams/issueflow-components.puml](diagrams/issueflow-components.puml)
- [diagrams/issueflow-sequence.puml](diagrams/issueflow-sequence.puml)
