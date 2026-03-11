@startuml
!define RECTANGLE class

package "Cliente" {
  actor Usuario
}

package "IssueFlow Platform" {
  [Gateway Service\nNode.js] as Gateway
  [Ticket Service\nSpring Boot] as Ticket
  [Assignment Service\nFastAPI] as Assignment
  [Notification Audit Service\nRails/Sinatra] as Audit

  database "Ticket DB" as TicketDB
  database "Assignment DB" as AssignmentDB
  database "Audit DB" as AuditDB
}

Usuario --> Gateway : HTTP
Gateway --> Ticket : HTTP
Ticket --> Assignment : HTTP
Ticket --> Audit : HTTP

Ticket --> TicketDB
Assignment --> AssignmentDB
Audit --> AuditDB
@enduml