
| ID      | Decision                                           | Reason                         |
| ------- | -------------------------------------------------- | ------------------------------ |
| ADR-001 | Node Agent is a background process                 | Continuous monitoring          |
| ADR-002 | Separate static and dynamic node data              | Reduce overhead                |
| ADR-003 | Linux becomes the primary development OS           | Direct kernel access           |
| ADR-004 | Learn low-level features in C before Java wrappers | Understand the mechanism first |
# ADR-001: Adopt a Modular Monolith Architecture

**Status:** Accepted

**Date:** Session 1

## Context

Campus Edge is expected to grow into a distributed edge-computing platform, but the initial goal is a working 3 to 5 node MVP. We need an architecture that allows rapid development while preserving clean boundaries for future extraction.

## Decision

The Control Plane will be built as a **Modular Monolith**.

All backend functionality will run inside a single Spring Boot application while being separated into well-defined modules.

Initial modules will be introduced only when they become necessary.

## Alternatives Considered

### Single-file application

- Fastest to start.
    
- Becomes difficult to maintain.
    
- Encourages tight coupling.
    

### Microservices from Day One

- Provides independent deployment.
    
- Introduces unnecessary complexity.
    
- Requires service discovery, distributed debugging, and multiple deployments before they're needed.
    

## Consequences

### Positive

- Faster MVP development.
    
- Clear module boundaries.
    
- Future migration to microservices remains possible.
    

### Negative

- Entire backend shares one deployment initially.
    
- Module discipline must be maintained.
# ADR-002: Build the Node Module First

**Status:** Accepted

**Date:** Session 1

## Context

Every major Campus Edge feature depends on machines existing inside the network.

Without registered nodes there can be:

- no scheduler
    
- no jobs
    
- no monitoring
    
- no distributed execution
    

## Decision

The first functional module will be `nodes`.

Future modules will be introduced in response to project needs rather than creating empty folders in advance.

## Initial Module Order

1. `common`
    
2. `nodes`
    
3. `jobs`
    
4. `scheduler`
    
5. `logs`
    
6. `auth`
    

Authentication may temporarily be simplified during the MVP to prioritize proving distributed communication.

## Consequences

The Node Registry becomes the foundation that every other subsystem depends upon.

# ADR-003: Use Client-Generated Persistent Node IDs

**Status:** Accepted

**Date:** Session 1

## Context

The Control Plane must recognize the same machine across restarts.

Possible identities considered:

- hostname
    
- MAC address
    
- hardware UUID
    
- generated node ID
    

## Decision

The Node Agent will generate a UUID during its first startup.

The UUID will be stored locally and included in every future request.

The Control Plane registers and validates this identifier.

## Why Not Hostname?

Hostnames can change and multiple machines may share identical names.

## Why Not MAC Address?

MAC addresses can change due to hardware replacement, virtualization, or randomized MAC features.

## Flow

First startup:

- Generate UUID
    
- Store locally
    
- Register
    

Future startups:

- Read stored UUID
    
- Send heartbeats
    
- Continue using the same identity
    

## Security Note

The UUID is an identifier, not proof of identity.

Future authentication will pair this identifier with credentials or cryptographic keys.

# ADR-004: Adopt a Layered Backend Architecture

**Status:** Accepted

**Date:** Session 1

## Context

The Control Plane will receive HTTP requests, apply business rules, and persist data.

Placing all logic inside controllers would tightly couple transport, business logic, and persistence.

## Decision

The backend will use four layers.

Controller → Service → Repository → Database

## Layer Responsibilities

### Controller

- Receive HTTP requests.
    
- Validate incoming DTOs.
    
- Return responses.
    

### Service

- Own business logic.
    
- Decide node state.
    
- Handle registration behavior.
    

### Repository

- Perform database operations.
    

### Database

- Persist data.
    

## Example

When a node registers:

- Controller receives the request.
    
- Service assigns `ONLINE`.
    
- Repository saves the node.
    

Business meaning belongs inside the Service layer.

# ADR-005: Separate DTOs from Database Entities

**Status:** Accepted

**Date:** Session 1

## Context

The Node Agent should not control internal database fields.

The database model contains server-owned information that should never appear as writable API input.

Examples:

- status
    
- IP
    
- registration timestamp
    
- last heartbeat
    

## Decision

Controllers will receive dedicated request DTOs instead of JPA entities.

The Service layer converts DTOs into entities.

## Registration Flow

Node Agent

↓

RegistrationDTO

↓

NodeService

↓

Node Entity

↓

Database

## Benefits

- Protects internal models.
    
- Prevents clients from modifying server-owned fields.
    
- Allows API evolution without changing database schemas.

# ADR-006: Separate Registration from Heartbeat Data

**Status:** Accepted

**Date:** Session 1

## Context

Some node information rarely changes while other information changes continuously.

Sending everything during every heartbeat would waste bandwidth and blur responsibilities.

## Decision

Registration will contain primarily static machine information.

Heartbeats will contain dynamic runtime information.

## Registration Data

- nodeId
    
- hostname
    
- cpuCores
    
- totalRamMB
    
- totalStorageGB
    

Server derives:

- IP
    
- status
    
- registration time
    
- last heartbeat
    

## Heartbeat Data

Planned examples:

- CPU usage
    
- free RAM
    
- free storage
    
- running jobs
    
- battery (future)
    

## Consequences

The Node Registry stores capabilities once.

Heartbeats become lightweight runtime updates rather than repeated hardware inventories.