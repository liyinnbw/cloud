# Cloud
Notes on things that needs to be considered when designing cloud application.

## Service
- Different services may be written in different languages & technologies that are most suitable/efficient for the task.
- Availability (auto scale, duplicates)
- Serverless (stateless function) vs Microservices (container)
- Services should be self-contained and decoupled, including database. Do not share database between services. Trade off is you have to manage inter-database consistency (polyglot persistence).
- How big should a service be? (Domain Driven Design, or practically, if two services are chatty communicating, they should be merged)
- Multi-tenant (single app that supports many concurrent users)
- Gateway / Ambassador patterns (might become bottleneck & single source of failure)
- [Gateway](https://docs.microsoft.com/en-us/azure/architecture/microservices/design/gateway) sits between client and service are called (reverse proxy, such as Nginx)
- Micro service [design patterns](https://docs.microsoft.com/en-us/azure/architecture/microservices/design/patterns)

## Inter-service Communication
- Event driven deisgn (be aware that event messaging queue can be the bottle neck, use aggregation & batch processing to metigate)
- [API](https://docs.microsoft.com/en-us/azure/architecture/microservices/design/api-design) (Interface definition language (IDL) such as Swagger)
  - API should model the domain rather than DB (avoid leaking DB schema, API should avoid changes even if DB schema changed).
  - For API call with side effects, use PUT
  - For asynchronous request, first return HTTP 202 response to indicate the request has been accepted and processing
- Synchronous (HTTP requests) vs Asynchronous (Messaging Queue)
- Indempotency
  - A request method is considered "idempotent" if the intended effect on the server of multiple identical requests with that method is the same as the effect for a single such request.
  - The HTTP specification states that GET, PUT, and DELETE methods must be idempotent. POST methods are not guaranteed to be idempotent.
- Versioning
  - Avoid removing/editing a field, adding new field is ok (but be aware of trade off between backward compatibility and payload)
  - Have a field called "version"

## Data Store (Persistence Technology)
- An app may use multiple persistence technologies ([polyglot persistence](https://martinfowler.com/bliki/PolyglotPersistence.html))
- Trade off between consistency and availability (strong consistency vs eventual consistency)
- Long-term vs short-term storage (short-term are those only valid/useful for the duration of a task, such as delivery tracking)
- Distributed data store are no longer consistent by default, update needs to be propagated across data stores (for each service). Eventual consistency might be used most of the time.
- Relational vs Non-relational
- CRUD vs CQRS (write: append events, read: reconstruct final state in a meterialized view)

## Logging

## Deployment
- Infrastructure as Code (templates that specify resources instead of manual configure every time)

## Continuous Development
- [Strangler Pattern](https://docs.microsoft.com/en-us/azure/architecture/patterns/strangler) (deprecate old service gradually)

