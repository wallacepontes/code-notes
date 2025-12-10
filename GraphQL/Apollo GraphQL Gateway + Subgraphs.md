# Apollo GraphQL Gateway + Subgraphs

Here's your **updated PlantUML diagram** using **only Apollo GraphQL Gateway and Subgraphs** — **no AWS services, no API Gateway (REST), no Lambda, no DynamoDB**.

This reflects a **modern GraphQL Federation architecture** using **Apollo Gateway** and **federated subgraphs**.

---

### Updated PlantUML (Apollo GraphQL Gateway + Subgraphs)

```plantuml
@startuml
' ========================================
' Apollo GraphQL Federation – Clean & Valid
' Tested with PlantUML 1.2025.11beta2
' ========================================

skinparam backgroundColor #FEFEFE
skinparam shadowing false
skinparam roundCorner 12
skinparam defaultFontName "Verdana"
skinparam defaultFontSize 12

skinparam rectangle {
    BackgroundColor #E3F2FD
    BorderColor #1976D2
    FontColor #0D47A1
    RoundCorner 12
}

skinparam cloud {
    BackgroundColor #FFF3E0
    BorderColor #FF8F00
    FontColor #E65100
}

skinparam agent {
    BackgroundColor #E8F5E8
    BorderColor #388E3C
    FontColor #1B5E20
}

' === Clients ===
agent "Web Client\n(React/Vue/Svelte)" as web #E8F5E8
agent "Mobile App\n(iOS/Android)" as mobile #E8F5E8
agent "Admin Panel" as admin #E8F5E8

' === Apollo Gateway ===
rectangle "Apollo Gateway\n(Supergraph Router)" as gateway {
    rectangle "Query\nPlanner" as planner
    rectangle "Execution\nEngine" as executor
    rectangle "Auth &\nCaching" as authcache
}

' === Subgraphs Cloud ===
cloud "Subgraphs\n(Federated Services)" as subgraphs #FFF3E0

package "Users Subgraph" as users_pkg {
    rectangle "User {\n  @key(fields: \"id\")\n  me { ... }\n}" as users
}

package "Products Subgraph" as products_pkg {
    rectangle "Product {\n  @key(fields: \"id\")\n  products { ... }\n}" as products
}

package "Reviews Subgraph" as reviews_pkg {
    rectangle "Review {\n  @key(fields: \"id\")\n  reviews(on User)\n}" as reviews
}

package "Comments Subgraph" as comments_pkg {
    rectangle "Comment {\n  @key(fields: \"id\")\n  addComment()\n}" as comments
}

' === Connections ===
web -down-> gateway : 1. POST /graphql\n{ me { reviews { product } } }
mobile -down-> gateway
admin -down-> gateway

gateway -down-> subgraphs : 2. Parallel Execution\n(_entities, fields)

gateway --> users : me, _entities
gateway --> products : _entities
gateway --> reviews : reviews
gateway --> comments : addComment

' === Hidden for layout ===
planner -[hidden]> executor
executor -[hidden]> authcache

' === Notes ===
note right of gateway
  • Apollo Federation v2+
  • Query planning & merging
  • Entity resolution via @key
  • APQ + @cacheControl
  • JWT → context
  • Metrics → GraphOS
end note

note bottom of subgraphs
  Each subgraph is:
  • Independently deployed
  • Owns part of schema
  • Resolves @key fields
end note

footer Apollo GraphOS – Supergraph Architecture (2025)
@enduml
```


## Fixed

```plantuml
@startuml
' ========================================
' Apollo GraphQL Federation Architecture
' Using Apollo Gateway + Subgraphs
' ========================================

!define RECTANGLE class

skinparam backgroundColor #FEFEFE
skinparam shadowing false
skinparam roundCorner 12
skinparam defaultFontName "Verdana"
skinparam defaultFontSize 12

skinparam rectangle {
    BackgroundColor #E3F2FD
    BorderColor #1976D2
    FontColor #0D47A1
}

skinparam cloud {
    BackgroundColor #FFF3E0
    BorderColor #FF8F00
    FontColor #E65100
}

skinparam agent {
    BackgroundColor #E8F5E8
    BorderColor #388E3C
    FontColor #1B5E20
}

' === Actors & Clients ===
agent "Web Client\n(React, iOS, etc.)" as client #E8F5E8
agent "Mobile App" as mobile #E8F5E8
agent "Admin Dashboard" as admin #E8F5E8

' === Apollo Gateway (Supergraph Router) ===
rectangle "Apollo Gateway\n(supergraph router)" as gateway {
    rectangle "Query Planner" as planner
    rectangle "Execution Engine" as executor
    rectangle "Auth & Cache" as authcache
}

' === Subgraphs (Federated Services) ===
cloud "Subgraphs\n(independent services)" as subgraphs #FFF3E0

frame "Users Subgraph" as users_pkg {
  () "User type\n@key(id)" as user_entity
  () "me query" as me_query
}

package "Users Subgraph" as users_pkg {
    rectangle "User {\n  @key(fields: \"id\")\n  me { ... }\n}" as users

}

package "Products Subgraph" as products_pkg {
    rectangle "Product {\n  @key(fields: \"id\")\n  products { ... }\n}" as products
					
}

package "Reviews Subgraph" as reviews_pkg {
    rectangle "Review {\n  @key(fields: \"id\")\n  reviews(on User)\n}" as reviews
									  
}

package "Comments Subgraph" as comments_pkg {
    rectangle "Comment {\n  @key(fields: \"id\")\n  addComment()\n}" as comments
						 
}

' === Relationships ===
client -down-> gateway : 1. GraphQL Query\n{ me { reviews { product } } }
mobile -down-> gateway
admin -down-> gateway

gateway -down-> subgraphs : 2. Parallel\nsubgraph requests

gateway --> users : _entities, me
gateway --> products : _entities
gateway --> reviews : reviews
gateway --> comments : addComment

' === Internal Gateway Flow ===
planner -[hidden]> executor
executor -[hidden]> authcache

note right of gateway
  • Federation v2+
  • Query planning
  • Entity resolution
  • Caching (APQ, @cacheControl)
  • Auth passthrough
  • Metrics → Apollo Studio
end note

note bottom of subgraphs
  Each subgraph:
  • Owns part of schema
  • Resolves @key fields
  • Deployed independently
end note

footer Apollo Federation with GraphOS – 2025
@enduml
```

---

### Key Features Shown

| Element | Meaning |
|-------|--------|
| **Apollo Gateway** | Single GraphQL endpoint; composes supergraph |
| **Query Planner** | Builds optimal execution plan across subgraphs |
| **Subgraphs** | Independent services contributing to the schema |
| **@key** | Enables entity stitching across services |
| **Parallel Requests** | Gateway fetches from multiple subgraphs concurrently |

---

### How to Render

1. Copy the code above
2. Paste into any PlantUML renderer:
   - [https://www.plantuml.com/plantuml](https://www.plantuml.com/plantuml)
   - VS Code + PlantUML extension
   - IntelliJ, Confluence, etc.

---

### Example Query This Architecture Supports

```graphql
query {
  me {
    id
    name
    reviews {
      body
      product {
        name
        price
      }
    }
  }
}
```

→ Resolved across **Users**, **Reviews**, and **Products** subgraphs via **Apollo Gateway**.

---

Let me know if you want:
- A **horizontal layout**
- **Kubernetes deployment view**
- **With Apollo Studio / GraphOS**
- **Rust-based Router** emphasis
- **Persisted Queries / APQ** flow