# 23/09/26   01:30 - 03:00(90min)

## Campus Edge Software Development Team

## Session 1 Notes: Birth of the Control Plane

Duration: ~1 hour

Phase: Backend Foundation

Objective: Design the Control Plane architecture before writing production code.

Outcome: We intentionally made six architectural decisions that will govern every future backend feature.

## Session Goal

Today's objective was not to build a Spring Boot application.

Today's objective was to answer one question:

> What kind of backend are we building, and why?

We treated this like an architecture review rather than a coding tutorial.

## Project Context

Campus Edge is a distributed edge-computing platform where campus laptops contribute:

- CPU
    
- RAM
    
- Storage
    
- GPU (future)
    

This chat belongs to the Software Development Team, whose responsibility is building the backend platform that manages the network.

Other teams provide:

- OS internals
    
- Networking
    
- DBMS theory
    
- Distributed systems
    

Our responsibility is turning those concepts into production software.

## Today's Engineering Decisions

## Decision 1: Control Plane

### What is a Control Plane?

The Control Plane is the coordinator of the entire network.

It does not execute workloads.

Instead, it decides:

- Who joins
    
- Who receives jobs
    
- Who is online
    
- Where information is stored
    

![](data:image/svg+xml;charset=utf-8,%3Csvg%20font-family%3D%22-apple-system-body%2C%20ui-sans-serif%2C%20-apple-system%2C%20system-ui%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Helvetica%2C%20%26quot%3BApple%20Color%20Emoji%26quot%3B%2C%20Arial%2C%20sans-serif%2C%20%26quot%3BSegoe%20UI%20Emoji%26quot%3B%2C%20%26quot%3BSegoe%20UI%20Symbol%26quot%3B%22%20font-weight%3D%22400%22%20data-d-component%3D%22svg%22%20fill%3D%22currentColor%22%20style%3D%22color%3Argb\(255%2C%20255%2C%20255\)%22%20viewBox%3D%220%200%20620%20260%22%20width%3D%22100%25%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%3Cg%20fill%3D%22none%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%3E%3Crect%20x%3D%22210%22%20y%3D%2220%22%20width%3D%22200%22%20height%3D%2250%22%20rx%3D%2210%22%2F%3E%3Crect%20x%3D%2220%22%20y%3D%22140%22%20width%3D%22120%22%20height%3D%2250%22%20rx%3D%2210%22%2F%3E%3Crect%20x%3D%22170%22%20y%3D%22140%22%20width%3D%22120%22%20height%3D%2250%22%20rx%3D%2210%22%2F%3E%3Crect%20x%3D%22320%22%20y%3D%22140%22%20width%3D%22120%22%20height%3D%2250%22%20rx%3D%2210%22%2F%3E%3Crect%20x%3D%22470%22%20y%3D%22140%22%20width%3D%22120%22%20height%3D%2250%22%20rx%3D%2210%22%2F%3E%3Cpath%20d%3D%22M270%2070%20L80%20140%22%2F%3E%3Cpath%20d%3D%22M310%2070%20L230%20140%22%2F%3E%3Cpath%20d%3D%22M310%2070%20L380%20140%22%2F%3E%3Cpath%20d%3D%22M350%2070%20L530%20140%22%2F%3E%3C%2Fg%3E%3Cg%20font-size%3D%2214%22%20text-anchor%3D%22middle%22%3E%3Ctext%20x%3D%22310%22%20y%3D%2250%22%3EControl%20Plane%3C%2Ftext%3E%3Ctext%20x%3D%2280%22%20y%3D%22170%22%3ENode%20A%3C%2Ftext%3E%3Ctext%20x%3D%22230%22%20y%3D%22170%22%3ENode%20B%3C%2Ftext%3E%3Ctext%20x%3D%22380%22%20y%3D%22170%22%3ENode%20C%3C%2Ftext%3E%3Ctext%20x%3D%22530%22%20y%3D%22170%22%3ENode%20D%3C%2Ftext%3E%3C%2Fg%3E%3C%2Fsvg%3E)

Industry examples:

- Kubernetes API Server
    
- Docker Swarm Manager
    
- AWS Control APIs
    

## Decision 2: Modular Monolith

Instead of choosing between:

- Single file
    
- Microservices
    

we selected a Modular Monolith.

![](data:image/svg+xml;charset=utf-8,%3Csvg%20font-family%3D%22-apple-system-body%2C%20ui-sans-serif%2C%20-apple-system%2C%20system-ui%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Helvetica%2C%20%26quot%3BApple%20Color%20Emoji%26quot%3B%2C%20Arial%2C%20sans-serif%2C%20%26quot%3BSegoe%20UI%20Emoji%26quot%3B%2C%20%26quot%3BSegoe%20UI%20Symbol%26quot%3B%22%20font-weight%3D%22400%22%20data-d-component%3D%22svg%22%20fill%3D%22currentColor%22%20style%3D%22color%3Argb\(255%2C%20255%2C%20255\)%22%20viewBox%3D%220%200%20760%20140%22%20width%3D%22100%25%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%3Cg%20fill%3D%22none%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%3E%3Crect%20x%3D%2230%22%20y%3D%2230%22%20width%3D%22180%22%20height%3D%2270%22%20rx%3D%2212%22%2F%3E%3Crect%20x%3D%22290%22%20y%3D%2230%22%20width%3D%22180%22%20height%3D%2270%22%20rx%3D%2212%22%2F%3E%3Crect%20x%3D%22550%22%20y%3D%2230%22%20width%3D%22180%22%20height%3D%2270%22%20rx%3D%2212%22%2F%3E%3C%2Fg%3E%3Cg%20font-size%3D%2214%22%20text-anchor%3D%22middle%22%3E%3Ctext%20x%3D%22120%22%20y%3D%2250%22%3ESingle%20File%3C%2Ftext%3E%3Ctext%20x%3D%22120%22%20y%3D%2268%22%3EFast%20start%3C%2Ftext%3E%3Ctext%20x%3D%22120%22%20y%3D%2286%22%3EPoor%20growth%3C%2Ftext%3E%3Ctext%20x%3D%22380%22%20y%3D%2250%22%3EModular%20Monolith%3C%2Ftext%3E%3Ctext%20x%3D%22380%22%20y%3D%2268%22%3EChosen%3C%2Ftext%3E%3Ctext%20x%3D%22380%22%20y%3D%2286%22%3EClean%20boundaries%3C%2Ftext%3E%3Ctext%20x%3D%22640%22%20y%3D%2250%22%3EMicroservices%3C%2Ftext%3E%3Ctext%20x%3D%22640%22%20y%3D%2268%22%3EFuture%20option%3C%2Ftext%3E%3Ctext%20x%3D%22640%22%20y%3D%2286%22%3EToo%20early%3C%2Ftext%3E%3C%2Fg%3E%3C%2Fsvg%3E)

Why?

- Fast development
    
- One deployment
    
- Clear module boundaries
    
- Future extraction into microservices remains possible
    

Important lesson:

> We optimize for today's complexity, not tomorrow's assumptions.

## Decision 3: Node Module Comes First

We resisted creating empty folders.

Instead, modules appear when the project actually needs them.

Current order:

```
common
↓
nodes
↓
jobs
↓
scheduler
↓
logs
↓
auth
```

Why `nodes`?

Without registered machines:

- no scheduler
    
- no jobs
    
- no monitoring
    
- no distributed execution
    

The Node Registry becomes the foundation.

## UPCF Applied: Node Registration

## What is it?

A contract between a Node Agent and the Control Plane.

The node says:

> "Here's who I am."

The server decides:

> "You're now part of the network."

## How it works

![](data:image/svg+xml;charset=utf-8,%3Csvg%20font-family%3D%22-apple-system-body%2C%20ui-sans-serif%2C%20-apple-system%2C%20system-ui%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Helvetica%2C%20%26quot%3BApple%20Color%20Emoji%26quot%3B%2C%20Arial%2C%20sans-serif%2C%20%26quot%3BSegoe%20UI%20Emoji%26quot%3B%2C%20%26quot%3BSegoe%20UI%20Symbol%26quot%3B%22%20font-weight%3D%22400%22%20data-d-component%3D%22svg%22%20fill%3D%22currentColor%22%20style%3D%22color%3Argb\(255%2C%20255%2C%20255\)%22%20viewBox%3D%220%200%20640%20140%22%20width%3D%22100%25%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%3Cg%20fill%3D%22none%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%3E%3Crect%20x%3D%2230%22%20y%3D%2240%22%20width%3D%22140%22%20height%3D%2250%22%20rx%3D%2210%22%2F%3E%3Crect%20x%3D%22250%22%20y%3D%2240%22%20width%3D%22140%22%20height%3D%2250%22%20rx%3D%2210%22%2F%3E%3Crect%20x%3D%22470%22%20y%3D%2240%22%20width%3D%22140%22%20height%3D%2250%22%20rx%3D%2210%22%2F%3E%3Cpath%20d%3D%22M170%2065%20H250%22%2F%3E%3Cpath%20d%3D%22M390%2065%20H470%22%2F%3E%3C%2Fg%3E%3Cg%20font-size%3D%2214%22%20text-anchor%3D%22middle%22%3E%3Ctext%20x%3D%22100%22%20y%3D%2270%22%3ENode%20Agent%3C%2Ftext%3E%3Ctext%20x%3D%22320%22%20y%3D%2270%22%3EControl%20Plane%3C%2Ftext%3E%3Ctext%20x%3D%22540%22%20y%3D%2270%22%3ENode%20Registry%3C%2Ftext%3E%3C%2Fg%3E%3C%2Fsvg%3E)

## Registration Pipeline

Before writing code, we designed the processing pipeline.

![](data:image/svg+xml;charset=utf-8,%3Csvg%20font-family%3D%22-apple-system-body%2C%20ui-sans-serif%2C%20-apple-system%2C%20system-ui%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Helvetica%2C%20%26quot%3BApple%20Color%20Emoji%26quot%3B%2C%20Arial%2C%20sans-serif%2C%20%26quot%3BSegoe%20UI%20Emoji%26quot%3B%2C%20%26quot%3BSegoe%20UI%20Symbol%26quot%3B%22%20font-weight%3D%22400%22%20data-d-component%3D%22svg%22%20fill%3D%22currentColor%22%20style%3D%22color%3Argb\(255%2C%20255%2C%20255\)%22%20viewBox%3D%220%200%20760%20140%22%20width%3D%22100%25%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%3Cg%20fill%3D%22none%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%3E%3Crect%20x%3D%2220%22%20y%3D%2240%22%20width%3D%22110%22%20height%3D%2250%22%20rx%3D%2210%22%2F%3E%3Crect%20x%3D%22150%22%20y%3D%2240%22%20width%3D%22110%22%20height%3D%2250%22%20rx%3D%2210%22%2F%3E%3Crect%20x%3D%22280%22%20y%3D%2240%22%20width%3D%22110%22%20height%3D%2250%22%20rx%3D%2210%22%2F%3E%3Crect%20x%3D%22410%22%20y%3D%2240%22%20width%3D%22110%22%20height%3D%2250%22%20rx%3D%2210%22%2F%3E%3Crect%20x%3D%22540%22%20y%3D%2240%22%20width%3D%22110%22%20height%3D%2250%22%20rx%3D%2210%22%2F%3E%3Cpath%20d%3D%22M130%2065%20H150%22%2F%3E%3Cpath%20d%3D%22M260%2065%20H280%22%2F%3E%3Cpath%20d%3D%22M390%2065%20H410%22%2F%3E%3Cpath%20d%3D%22M520%2065%20H540%22%2F%3E%3C%2Fg%3E%3Cg%20font-size%3D%2213%22%20text-anchor%3D%22middle%22%3E%3Ctext%20x%3D%2275%22%20y%3D%2268%22%3EHTTP%20Request%3C%2Ftext%3E%3Ctext%20x%3D%22205%22%20y%3D%2268%22%3EValidate%3C%2Ftext%3E%3Ctext%20x%3D%22335%22%20y%3D%2268%22%3EEnrich%3C%2Ftext%3E%3Ctext%20x%3D%22465%22%20y%3D%2268%22%3EStore%3C%2Ftext%3E%3Ctext%20x%3D%22595%22%20y%3D%2268%22%3ERespond%3C%2Ftext%3E%3C%2Fg%3E%3C%2Fsvg%3E)

Five responsibilities.

This pipeline will be reused for future APIs.

## Identity Design

We evaluated:

|Candidate|Problem|
|---|---|
|Hostname|Can change|
|MAC Address|Hardware changes|
|Hardware UUID|Platform dependent|
|Generated `nodeId`|Chosen|

## Final decision

The Node Agent generates a UUID once.

![](data:image/svg+xml;charset=utf-8,%3Csvg%20font-family%3D%22-apple-system-body%2C%20ui-sans-serif%2C%20-apple-system%2C%20system-ui%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Helvetica%2C%20%26quot%3BApple%20Color%20Emoji%26quot%3B%2C%20Arial%2C%20sans-serif%2C%20%26quot%3BSegoe%20UI%20Emoji%26quot%3B%2C%20%26quot%3BSegoe%20UI%20Symbol%26quot%3B%22%20font-weight%3D%22400%22%20data-d-component%3D%22svg%22%20fill%3D%22currentColor%22%20style%3D%22color%3Argb\(255%2C%20255%2C%20255\)%22%20viewBox%3D%220%200%20760%20120%22%20width%3D%22100%25%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%3Cg%20fill%3D%22none%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%3E%3Crect%20x%3D%2220%22%20y%3D%2230%22%20width%3D%22120%22%20height%3D%2250%22%20rx%3D%2210%22%2F%3E%3Crect%20x%3D%22170%22%20y%3D%2230%22%20width%3D%22120%22%20height%3D%2250%22%20rx%3D%2210%22%2F%3E%3Crect%20x%3D%22320%22%20y%3D%2230%22%20width%3D%22120%22%20height%3D%2250%22%20rx%3D%2210%22%2F%3E%3Crect%20x%3D%22470%22%20y%3D%2230%22%20width%3D%22120%22%20height%3D%2250%22%20rx%3D%2210%22%2F%3E%3Crect%20x%3D%22620%22%20y%3D%2230%22%20width%3D%22120%22%20height%3D%2250%22%20rx%3D%2210%22%2F%3E%3Cpath%20d%3D%22M140%2055%20H170%22%2F%3E%3Cpath%20d%3D%22M290%2055%20H320%22%2F%3E%3Cpath%20d%3D%22M440%2055%20H470%22%2F%3E%3Cpath%20d%3D%22M590%2055%20H620%22%2F%3E%3C%2Fg%3E%3Cg%20font-size%3D%2213%22%20text-anchor%3D%22middle%22%3E%3Ctext%20x%3D%2280%22%20y%3D%2260%22%3ENode%20Starts%3C%2Ftext%3E%3Ctext%20x%3D%22230%22%20y%3D%2260%22%3EGenerate%20UUID%3C%2Ftext%3E%3Ctext%20x%3D%22380%22%20y%3D%2260%22%3ESave%20Locally%3C%2Ftext%3E%3Ctext%20x%3D%22530%22%20y%3D%2260%22%3ERegister%3C%2Ftext%3E%3Ctext%20x%3D%22680%22%20y%3D%2260%22%3EReuse%20Forever%3C%2Ftext%3E%3C%2Fg%3E%3C%2Fsvg%3E)

The server registers it.

Future requests reuse it.

Important distinction:

`nodeId` identifies.

Authentication will later prove ownership.

## API Ownership

We discovered an important API principle.

The client should not control server-owned information.

### Client provides

```
{
  "nodeId":"...",
  "hostname":"...",
  "cpuCores":8,
  "totalRamMB":16384,
  "totalStorageGB":256
}
```

### Server determines

- IP
    
- Status
    
- Registration time
    
- Last heartbeat
    

Principle:

> Clients describe themselves. Servers establish reality.

## Layered Backend Architecture

Instead of letting controllers directly talk to the database,

we adopted a four-layer architecture.

![](data:image/svg+xml;charset=utf-8,%3Csvg%20font-family%3D%22-apple-system-body%2C%20ui-sans-serif%2C%20-apple-system%2C%20system-ui%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Helvetica%2C%20%26quot%3BApple%20Color%20Emoji%26quot%3B%2C%20Arial%2C%20sans-serif%2C%20%26quot%3BSegoe%20UI%20Emoji%26quot%3B%2C%20%26quot%3BSegoe%20UI%20Symbol%26quot%3B%22%20font-weight%3D%22400%22%20data-d-component%3D%22svg%22%20fill%3D%22currentColor%22%20style%3D%22color%3Argb\(255%2C%20255%2C%20255\)%22%20viewBox%3D%220%200%20760%20120%22%20width%3D%22100%25%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%3Cg%20fill%3D%22none%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%3E%3Crect%20x%3D%2220%22%20y%3D%2230%22%20width%3D%22120%22%20height%3D%2250%22%20rx%3D%2210%22%2F%3E%3Crect%20x%3D%22180%22%20y%3D%2230%22%20width%3D%22120%22%20height%3D%2250%22%20rx%3D%2210%22%2F%3E%3Crect%20x%3D%22340%22%20y%3D%2230%22%20width%3D%22120%22%20height%3D%2250%22%20rx%3D%2210%22%2F%3E%3Crect%20x%3D%22500%22%20y%3D%2230%22%20width%3D%22120%22%20height%3D%2250%22%20rx%3D%2210%22%2F%3E%3Cpath%20d%3D%22M140%2055%20H180%22%2F%3E%3Cpath%20d%3D%22M300%2055%20H340%22%2F%3E%3Cpath%20d%3D%22M460%2055%20H500%22%2F%3E%3C%2Fg%3E%3Cg%20font-size%3D%2213%22%20text-anchor%3D%22middle%22%3E%3Ctext%20x%3D%2280%22%20y%3D%2260%22%3EController%3C%2Ftext%3E%3Ctext%20x%3D%22240%22%20y%3D%2260%22%3EService%3C%2Ftext%3E%3Ctext%20x%3D%22400%22%20y%3D%2260%22%3ERepository%3C%2Ftext%3E%3Ctext%20x%3D%22560%22%20y%3D%2260%22%3EDatabase%3C%2Ftext%3E%3C%2Fg%3E%3C%2Fsvg%3E)

Responsibilities:

|Layer|Responsibility|
|---|---|
|Controller|HTTP|
|Service|Business decisions|
|Repository|Persistence|
|Database|Storage|

Example:

The Service decides:

```
status = ONLINE
```

The Controller does not.

## Why DTOs Exist

One of today's biggest lessons.

Question:

> Why shouldn't the Controller receive the `Node` database object?

Because the outside world should never control internal data.

Instead:

![](data:image/svg+xml;charset=utf-8,%3Csvg%20font-family%3D%22-apple-system-body%2C%20ui-sans-serif%2C%20-apple-system%2C%20system-ui%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Helvetica%2C%20%26quot%3BApple%20Color%20Emoji%26quot%3B%2C%20Arial%2C%20sans-serif%2C%20%26quot%3BSegoe%20UI%20Emoji%26quot%3B%2C%20%26quot%3BSegoe%20UI%20Symbol%26quot%3B%22%20font-weight%3D%22400%22%20data-d-component%3D%22svg%22%20fill%3D%22currentColor%22%20style%3D%22color%3Argb\(255%2C%20255%2C%20255\)%22%20viewBox%3D%220%200%20760%20180%22%20width%3D%22100%25%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%3Cg%20fill%3D%22none%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%3E%3Crect%20x%3D%2220%22%20y%3D%2260%22%20width%3D%22120%22%20height%3D%2250%22%20rx%3D%2210%22%2F%3E%3Crect%20x%3D%22170%22%20y%3D%2260%22%20width%3D%22120%22%20height%3D%2250%22%20rx%3D%2210%22%2F%3E%3Crect%20x%3D%22320%22%20y%3D%2260%22%20width%3D%22120%22%20height%3D%2250%22%20rx%3D%2210%22%2F%3E%3Crect%20x%3D%22470%22%20y%3D%2260%22%20width%3D%22120%22%20height%3D%2250%22%20rx%3D%2210%22%2F%3E%3Crect%20x%3D%22620%22%20y%3D%2260%22%20width%3D%22120%22%20height%3D%2250%22%20rx%3D%2210%22%2F%3E%3Cpath%20d%3D%22M140%2085%20H170%22%2F%3E%3Cpath%20d%3D%22M290%2085%20H320%22%2F%3E%3Cpath%20d%3D%22M440%2085%20H470%22%2F%3E%3Cpath%20d%3D%22M590%2085%20H620%22%2F%3E%3C%2Fg%3E%3Cg%20font-size%3D%2213%22%20text-anchor%3D%22middle%22%3E%3Ctext%20x%3D%2280%22%20y%3D%2252%22%3ENode%20Agent%3C%2Ftext%3E%3Ctext%20x%3D%2280%22%20y%3D%2290%22%3EJSON%3C%2Ftext%3E%3Ctext%20x%3D%22230%22%20y%3D%2252%22%3EController%3C%2Ftext%3E%3Ctext%20x%3D%22230%22%20y%3D%2290%22%3ERegistration%20DTO%3C%2Ftext%3E%3Ctext%20x%3D%22380%22%20y%3D%2252%22%3EService%3C%2Ftext%3E%3Ctext%20x%3D%22380%22%20y%3D%2290%22%3ERules%3C%2Ftext%3E%3Ctext%20x%3D%22530%22%20y%3D%2252%22%3ERepository%3C%2Ftext%3E%3Ctext%20x%3D%22530%22%20y%3D%2290%22%3ENode%20Entity%3C%2Ftext%3E%3Ctext%20x%3D%22680%22%20y%3D%2252%22%3EDatabase%3C%2Ftext%3E%3Ctext%20x%3D%22680%22%20y%3D%2290%22%3EPersist%3C%2Ftext%3E%3C%2Fg%3E%3C%2Fsvg%3E)

A DTO becomes a safe boundary between external requests and internal models.

## Registration vs Heartbeats

We separated static and dynamic information.

### Registration

Sent once.

|Field|
|---|
|nodeId|
|hostname|
|cpuCores|
|totalRamMB|
|totalStorageGB|

### Heartbeats

Sent repeatedly.

Future fields:

- CPU usage
    
- Free RAM
    
- Free storage
    
- Running jobs
    
- Battery (future)
    

Why?

Static information should not be retransmitted every few seconds.

## Meta-Learning Tags

|Concept|Primitive|Pattern|Project Role|
|---|---|---|---|
|Control Plane|Service|Coordination|Backend|
|Modular Monolith|Module|Modular Architecture|Control Plane|
|`nodeId`|Identifier|Persistent Identity|Node Agent|
|DTO|Data Object|Boundary Protection|Node Registration|
|Service Layer|Object|Business Logic|Nodes|

## Key Mental Models Learned

Instead of memorizing Spring terminology, we built these mental models.

### Model 1

> The Control Plane is a decision engine, not a workload executor.

### Model 2

> Clients describe themselves. Servers establish reality.

### Model 3

> Business logic belongs in Services.

### Model 4

> API contracts should not expose database models.

### Model 5

> Identity belongs to the platform, not the hardware.

## Next Session Preview

Session 2 begins implementation.

Instead of blindly using Spring Initializr, we'll reverse-engineer every file it creates.

We'll build:

- Maven project
    
- `control-plane`
    
- `nodes` module
    
- `GET /health`
    

Most importantly, we'll understand why every generated file exists before we keep it.

## Session 1 Git Artifact

Although we intentionally delayed writing production code, this session still produced permanent engineering assets.

Git Commit

```
docs(architecture): establish control plane foundation and node registration design
```

This marks the first permanent design milestone of the Campus Edge backend.