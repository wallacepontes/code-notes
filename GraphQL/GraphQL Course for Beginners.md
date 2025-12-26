# GraphQL Course for Beginners

---

## Table of Contents

- [GraphQL Course for Beginners](#graphql-course-for-beginners)
  - [Table of Contents](#table-of-contents)
  - [GraphQL Crash Course](#graphql-crash-course)
    - [#1 - What is GraphQL?](#1---what-is-graphql)
    - [#2 - Query Basics](#2---query-basics)
    - [#3 - Making a GraphQL Server (with Apollo)](#3---making-a-graphql-server-with-apollo)
    - [#4 - Schema \& Types](#4---schema--types)
    - [#5 - Resolver Functions](#5---resolver-functions)
    - [#6 - Query Variables](#6---query-variables)
    - [#7 - Related Data](#7---related-data)
    - [#8 - Mutations (Adding \& Deleting Data)](#8---mutations-adding--deleting-data)
    - [#9 - Update Mutation](#9---update-mutation)
    - [#10 - Operations Apollo Federation](#10---operations-apollo-federation)
  - [Course Presentation: GraphQL for Beginners](#course-presentation-graphql-for-beginners)
    - [Slide 1: Welcome to GraphQL: The Cutting Edge Query Language](#slide-1-welcome-to-graphql-the-cutting-edge-query-language)
    - [Slide 2: Understanding GraphQL Fundamentals](#slide-2-understanding-graphql-fundamentals)
    - [Slide 3: GraphQL vs. Traditional REST APIs](#slide-3-graphql-vs-traditional-rest-apis)
    - [Slide 4: Designing Queries: Precision and Traversal](#slide-4-designing-queries-precision-and-traversal)
    - [Slide 5: Setting Up the GraphQL Server](#slide-5-setting-up-the-graphql-server)
    - [Slide 6: Defining the Schema: Type Definitions (`typeDefs`)](#slide-6-defining-the-schema-type-definitions-typedefs)
    - [Slide 7: Defining Query Entry Points](#slide-7-defining-query-entry-points)
    - [Slide 8: Resolvers: Handling Incoming Requests](#slide-8-resolvers-handling-incoming-requests)
    - [Slide 9: Resolving Nested and Related Data](#slide-9-resolving-nested-and-related-data)
    - [Slide 10: Mutations: Data Modification](#slide-10-mutations-data-modification)
    - [Slide 11: Summary and Next Steps](#slide-11-summary-and-next-steps)
  - [Video](#video)
  - [References](#references)

---

## GraphQL Crash Course

This course provides a comprehensive introduction to **GraphQL**, a cutting-edge query language developed by the popular instructor, The Net Ninja. The goal is to rapidly equip students with the necessary skills to understand the basic syntax and build a GraphQL server using Node.js and the Apollo server package. A basic understanding of **Node.js** is expected before starting this course.

Here is the detailed course breakdown:

---

### #1 - What is GraphQL?

GraphQL is a **query language** (QL) that uses a specific syntax to query a server to request or mutate data. It serves as an alternative to the traditional approach of sending standard requests to a REST API using endpoints. While GraphQL still uses HTTP requests, the query language layer provides more flexibility and control over what data is fetched or mutated.

GraphQL addresses two major drawbacks often encountered with traditional REST APIs:
1.  **Overfetching:** When a client requests data from an endpoint and the server sends back **too much data** (more properties than needed). GraphQL resolves this because clients specify exactly what data and fields they need back from the server using the GraphQL syntax.
2.  **Underfetching:** When a client does not get back all the required data from a single request, necessitating multiple requests to different endpoints. GraphQL allows users to **fetch nested related data** within a single query, avoiding the need for subsequent requests.

Unlike REST APIs, where resources typically have their own set of endpoints, a GraphQL request is typically sent to a **single endpoint**, often `/graphql`.

---

### #2 - Query Basics

Before building the server, it is necessary to understand the basic GraphQL query syntax. We will use **Apollo Explorer** (the GraphQL equivalent of Postman) to send test queries to the server.

A query is structured as follows:

1. Use the `query` keyword, followed by an optional name (e.g., `reviewsQuery`).
2. Specify the desired data resource (the entry point to the graph, e.g., `reviews`).
3. Inside curly braces, specify the **exact fields** (properties) required from that resource (e.g., `rating`, `content`, `ID`).

```js
//schema.js
export const typeDefs = `#graphql
  type Game {
    id: ID!
    title: String!
    platform: [String!]!
    reviews: [Review!]
  }
`  
```


The underlying principle is that a GraphQL server forms a **graph**, a collection of connected data types (e.g., reviews, authors, games). This structure allows users to **traverse or walk through the graph** to fetch any related data from their starting point. This means related resources (like an `author` associated with a `review`) can be nested inside the initial query, and all this data is returned in a single request.

---

### #3 - Making a GraphQL Server (with Apollo)

To create a GraphQL API, we use **Node.js** and the **Apollo server** library. Apollo Server is beneficial because it automatically spins up an instance of the Apollo Explorer for testing the API.

The setup involves the following steps:

1. Initialize a new Node project and set the type to `module` (to allow ES6 `import` syntax).
2. Install two dependencies: `graphql` (the core) and `@apollo/server`.
3. In the `index.js` file, import `ApolloServer` and `startStandaloneServer`.
4. Instantiate the server: `const server = new ApolloServer({})`.
5. Start the server, typically listening on port `4000`.

The `ApolloServer` constructor requires an object with two main properties:

1. `typeDefs` (Type Definitions): Descriptions of data types and their relationships (the **schema**).
2. `resolvers`: Functions that determine how the server responds to incoming queries.

```js
// index.js
import { ApolloServer } from '@apollo/server'
import { startStandaloneServer } from '@apollo/server/standalone'

// server setup
const server = new ApolloServer({
  typeDefs,
  resolvers
})

const { url } = await startStandaloneServer(server, {
  listen: { port: 4000 }
})
```

---

### #4 - Schema & Types

The **schema** describes the shape of the graph and the data available on it. The schema is defined using **type definitions (`typeDefs`)**.

GraphQL has five basic **scalar types**:

- `Int` (whole numbers).
- `Float` (decimal numbers).
- `String` (text).
- `Boolean` (True or False).
- `ID` (a special type used as a key for data objects).

Custom data types (like `Game`, `Review`, `Author`) are defined using the `type` keyword.

- Fields are defined with their associated scalar type (e.g., `title: String`).
- An **exclamation mark (`!`)** signifies that a field is required and cannot be null.
- **Square brackets (`[]`)** denote a list or array of a certain type (e.g., `platform: [String!]!`).

```js
//schema.js
export const typeDefs = `#graphql
  type Review {
    id: ID!
    rating: Int!
    content: String!
    author: Author!
    game: Game!
  }
  type Author {
    id: ID!
    name: String!
    verified: Boolean!
    reviews: [Review!]
  }
`
```

Every GraphQL schema **must** include a special type called **Query**. The Query type defines the initial **entry points** to the graph and specifies the return types (e.g., `reviews: [Review!]` for a list of reviews).

```js
//schema.js
export const typeDefs = `#graphql
  type Query {
    games: [Game]
    game(id: ID!): Game
    reviews: [Review]
    review(id: ID!): Review
    authors: [Author]
    author(id: ID!): Author
  }
`  
```

---

### #5 - Resolver Functions

**Resolvers** are functions that tell the server how to handle incoming requests and return the requested data to the client. The schema acts as the map for the graph structure, while the resolvers provide the action to handle the queries.

- For this course, we use local data stored in a variable in a file called `_db.js` instead of a traditional database.

```js
//_db.js
let games = [
  {id: '1', title: 'Zelda, Tears of the Kingdom', platform: ['Switch']},
  {id: '2', title: 'Final Fantasy 7 Remake', platform: ['PS5', 'Xbox']},
  {id: '3', title: 'Elden Ring', platform: ['PS5', 'Xbox', 'PC']},
  {id: '4', title: 'Mario Kart', platform: ['Switch']},
  {id: '5', title: 'Pokemon Scarlet', platform: ['PS5', 'Xbox', 'PC']},
]
```

- Resolver functions for the entry points defined in the Query type are placed inside a **`Query` property** within the main `resolvers` object.

```js
// resolvers
const resolvers = {
  Query: {
    games() {
      return db.games
    }
  }
}
```

- The function name must match the field name defined in the `Query` type (e.g., a `games` resolver for the `games` field).

```js
// resolvers
const resolvers = {
  Game: {
    reviews(parent) {
      return db.reviews.filter((r) => r.game_id === parent.id)
    }
  }
}
```

- These functions simply return the appropriate data array (e.g., `DB.games`). Apollo Server then automatically filters the returned data to match only the fields the user requested in the query.

---

### #6 - Query Variables

To fetch a single item (e.g., one review instead of a list), we must add new entry points to the root `Query` type.

For example, to fetch a single review, the schema entry point is defined as:
`type Query { review(id: ID!): Review }`.

- The parentheses contain the **query variable** (`ID`), its type (`ID`), and the requirement status (`!`).

In the resolver function, the variables are accessed using the **`args`** argument, which is automatically available.

`const resolvers = { Query: { review(_, args) { return db.reviews.find((review) => review.id === args.id) } }`

- To retrieve the ID, we use `args.id`.
- The resolver then uses a method like `find` on the local data array (e.g., `db.reviews.find(...)`) to locate and return the item where the stored ID matches `args.id`.

On the client side (using Apollo Explorer), variables must be declared after the query name (e.g., `query reviewQuery($ID: ID!)`) and then passed into the query body using the dollar sign (e.g., `review(id: $ID)`).

---

### #7 - Related Data

To enable nested queries, relationships must be defined in the schema. For example, the `Review` type is updated to include its related `game: Game!` and `author: Author!`.

Nested queries require **type-specific resolvers**, which are defined outside of the root `Query` property in the `resolvers` object. For example, the resolver for `reviews` nested under a `Game` query is placed inside a `Game` object property.

These nested resolvers use the first automatic argument, **`parent`**.

- The `parent` argument references the value returned by the previous resolver in the query chain (the parent object).
- If a user queries a game and then asks for its reviews, the `parent` will be the `Game` object, providing access to `parent.id`.
- The resolver then uses the `filter` method on the related data array (e.g., `DB.reviews.filter(...)`) to return only the subset of data where the foreign key (e.g., `r.game_ID`) matches `parent.id`.

```js
//schema.js
export const typeDefs = `#graphql
type Review {
    id: ID!
    rating: Int!
    content: String!
    author: Author!
    game: Game!
  }
  type Author {
    id: ID!
    name: String!
    verified: Boolean!
    reviews: [Review!]
  }
`
```

```js
// resolvers
const resolvers = {
  Review: {
    author(parent) {
      return db.authors.find((a) => a.id === parent.author_id)
    },
    game(parent) {
      return db.games.find((g) => g.id === parent.game_id)
    }
  },
  Author: {
    reviews(parent) {
      return db.reviews.filter((r) => r.author_id === parent.id)
    }
  }
}
```

This mechanism creates a **resolver chain**, allowing clients to nest queries as deeply as needed to traverse the graph and fetch related data in a single request.

---

### #8 - Mutations (Adding & Deleting Data)

A **mutation** is the generic term in GraphQL for any change made to the data (adding, deleting, or updating). Mutations are defined in the schema using the special **`Mutation`** type.

**Deletion Mutation:**

1. Define the mutation in the schema: e.g., `deleteGame(id: ID!): [Game!]` (specifying the required ID argument and the return type, which is often the updated list of games).
2. Define the resolver inside the `Mutation` property of the `resolvers` object.
3. The resolver accesses the ID via `args.id` and uses the `filter` method on the data array (`DB.games.filter()`) to remove the matching record before returning the updated array.

```js
//schema.js
export const typeDefs = `#graphql
  type Mutation {
    addGame(game: AddGameInput!): Game
    deleteGame(id: ID!): [Game]
    updateGame(id: ID!, edits: EditGameInput): Game
  }
  input AddGameInput {
    title: String!,
    platform: [String!]!
  }
  input EditGameInput {
    title: String,
    platform: [String!]
  }
`
```

**Adding Mutation:**

1. For mutations requiring multiple fields (like a new game's title and platform), it is best practice to define an **input type**.
2. Input types are defined using the `input` keyword (e.g., `input AddGameInput`). This groups several fields into a single, structured argument for the mutation.
3. The `addGame` mutation uses this input type: `addGame(game: AddGameInput!): Game`.
4. The resolver then accesses the new data via `args.game`, generates a unique ID (e.g., using `Math.random()`), pushes the new object onto the array, and returns the newly created game.

```js
// resolvers
const resolvers = {
  Mutation: {
    addGame(_, args) {
      let game = {
        ...args.game, 
        id: Math.floor(Math.random() * 10000).toString()
      }
      db.games.push(game)

      return game
    },
    deleteGame(_, args) {
      db.games = db.games.filter((g) => g.id !== args.id)

      return db.games
    }
}
```

---

### #9 - Update Mutation

The update mutation typically requires the ID of the item to be edited and the new data to apply.

1. We define a separate **input type** for editing (e.g., `EditGameInput`) where the fields (like `title` or `platform`) are *not* required (`title: String`), providing flexibility for updating only specific fields.
2. The schema mutation is defined: `updateGame(id: ID!, edits: EditGameInput!): Game`.
3. The resolver uses the `map` method to iterate through the data array.
4. If the game's ID matches `args.id`, the resolver returns a new object by spreading the existing game properties and then spreading `args.edits`, which **overrides** any existing fields with the new values provided by the client.
5. Finally, the resolver uses the `find` method to return the single updated game object to the client.


```js
// resolvers
const resolvers = {
  Mutation: {
    updateGame(_, args) {
      db.games = db.games.map((g) => {
        if (g.id === args.id) {
          return {...g, ...args.edits}
        }

        return g
      })

      return db.games.find((g) => g.id === args.id)
    }
  }
}
```

---

### #10 - Operations Apollo Federation

- Games Query

```js
//Operation
query GamesQuery {
  games{
    id,
    title,
    platform
  }
}
```

- Review Query

```js
//Operation
query ReviewQuery($id:ID!) {
  review(id: $id) {
    rating,
    game {
      title,
      platform,
      reviews {
        rating,
        content
      }
    }
    author {
      name,
      verified
    }
  }
}

//Variables
{
  "id": "1"
}
```

- DeleteMutation

```js
//Operation
mutation DeleteMutation($id: ID!) {
  deleteGame(id: $id) {
    id,
    title,
    platform
  }
}

//Variables
{
  "id": "2"
}
```

- AddMutation

```js
//Operation
mutation AddMutation($game: AddGameInput!) {
  addGame(game: $game) {
    id,
    title,
    platform
  }
}

//Variables
{
  "game": {
    "title": "a new game",
    "platform": ["Switch", "ps5"]
  }
}
```

- EditMutation

```js
//Operation
mutation EditMutation($edits: EditGameInput!, $id: ID!) {
  updateGame(edits: $edits, id: $id) {
    title,
    platform
  }
}

// Variables
{
  "edits": {
    "platform": ["dark too"]
  },
  "id": "3"
}
``` 

---

## Course Presentation: GraphQL for Beginners

Learning GraphQL is like moving from giving a waiter an entire, rigid menu (REST) and hoping they bring exactly what you want, to explicitly ordering exactly the dish you want, specifying every ingredient, and even asking for related side dishes from that same order (GraphQL).

### Slide 1: Welcome to GraphQL: The Cutting Edge Query Language

**Course Overview**
*   This course provides a comprehensive introduction to **GraphQL**, a cutting-edge query language designed for modern web application data retrieval needs.
*   We will explore its core principles and advantages over traditional REST APIs.
*   The goal is to gain practical skills to design and implement robust data-driven applications.

**Instructor & Prerequisites**
*   This material was developed by a popular GraphQL instructor, **The Net Ninja**.
*   **Prerequisite:** A basic understanding of **Node.js** is expected, as we will use it to build our GraphQL server.
*   **Tools:** We will build a GraphQL server using **Node.js** and the **Apollo server** package.

---

### Slide 2: Understanding GraphQL Fundamentals

**What is GraphQL?**
*   The "QL" in GraphQL stands for **Query Language**.
*   It is a specific syntax used to query a server to request or mutate data.
*   GraphQL is an alternative to the more traditional approach of sending standard requests to a **REST API** using endpoints.
*   While REST is an architectural style, GraphQL is an actual query language with its own syntax and rules.

**Core Mechanism**
*   GraphQL still uses **HTTP requests** under the hood, but the query language layer provides more flexibility and control over what data is fetched or mutated.
*   GraphQL servers handle requests differently than typical REST APIs. Requests are typically sent to a **single endpoint**, often `/graphql`.

---

### Slide 3: GraphQL vs. Traditional REST APIs

| REST API Drawback | Definition | How GraphQL Solves It | Source |
| :--- | :--- | :--- | :--- |
| **Overfetching** | Requesting data from an endpoint and the server sending back *too much* data (more than needed). | Users specify **exactly what fields** they need back from the server using the GraphQL syntax, resulting in no overfetching. | |
| **Underfetching** | Not getting back all the required data from a single request, often leading to making multiple requests to different endpoints. | Allows fetching **nested related data** within a single query. All related data can be brought back in one request. | |

---

### Slide 4: Designing Queries: Precision and Traversal

**Query Structure**
1.  Start with the `query` keyword and optionally give the query a name (e.g., `reviewsQuery`).
2.  Inside curly braces, specify the desired data resource (the entry point to the graph, e.g., `reviews`).
3.  Specify the exact fields (properties) required from that resource (e.g., `rating`, `content`, `ID`).

**The Graph Concept**
*   When a GraphQL server is created, it forms a **graph**—a collection of connected data types (e.g., reviews, authors, games).
*   The GraphQL design allows users to **traverse or walk through this graph** to fetch any related data from a starting point.

**Fetching Related Data**
*   Related data can be nested inside a query. For example, when querying for reviews, you can nest a request for the associated `author` and specify fields like `name` and `ID`.
*   This ability to nest related data into a single query is a key benefit, avoiding multiple queries required in REST.

---

### Slide 5: Setting Up the GraphQL Server

**Required Packages**
*   We use **Node.js** and the **Apollo Server** library to spin up a GraphQL server.
*   Apollo Server is beneficial because it automatically starts an instance of the **Apollo Explorer** for testing the API.
*   We install two dependencies via npm: `graphql` (the core) and `@apollo/server`.

**Server Initialization (in `index.js`)**
The server is set up using the imported components:
1.  Import `ApolloServer` and `startStandaloneServer`.
2.  Instantiate the server: `const server = new ApolloServer({})`.
3.  Start the server: `startStandaloneServer(server, { listen: { port: 4000 } })`.

**Apollo Server Configuration**
The `ApolloServer` constructor requires an object with two main properties:
1.  `typeDefs` (Type Definitions): Descriptions of data types and their relationships (the **schema**).
2.  `resolvers`: Functions that determine how the server responds to incoming queries.

---

### Slide 6: Defining the Schema: Type Definitions (`typeDefs`)

**Purpose of Type Definitions**
*   `typeDefs` define the different types of data exposed on the graph (e.g., `Author`, `Game`, `Review`).
*   The combination of all types and their relationships forms the **schema**, which describes the shape of the graph and the available data.

**GraphQL Scalar Types**
GraphQL has five basic scalar types:
*   **Int:** Whole numbers.
*   **Float:** Decimal numbers.
*   **String:** Text.
*   **Boolean:** True or False.
*   **ID:** A special type used as a key for data objects, serialized as strings.

**Defining Custom Types**
*   Types are defined using the keyword `type` (e.g., `type Game`).
*   You specify properties and their corresponding scalar types (e.g., `title: String`).
*   **Required Fields:** An exclamation mark (`!`) at the end means the field is required and cannot be null.
*   **Arrays/Lists:** Square brackets indicate a list of a certain type (e.g., `platform: [String!]!`).

---

### Slide 7: Defining Query Entry Points

**The Query Type**
*   Every GraphQL schema **must** include a special type called **Query**.
*   The Query type defines the initial **entry points** to the graph and specifies the return types of those entries.
*   If an entry point is for a list (e.g., retrieving all reviews), the return type is wrapped in square brackets (e.g., `reviews: [Review!]`).

**Query Variables (Arguments)**
*   To allow querying for a single item (e.g., one review), an entry point needs an argument.
*   Arguments are defined in parentheses, including a variable name, type, and required status (e.g., `review(id: ID!): Review`).
*   This defines that the user must pass in an `ID` to fetch a single review.

---

### Slide 8: Resolvers: Handling Incoming Requests

**Resolver Functions**
*   The `resolvers` object contains functions that determine **how** the server responds to incoming queries.
*   The schema provides the *map* (structure), and the resolvers handle the *action* (fetching or manipulating data).

**Root Query Resolvers**
*   Resolvers for entry points defined in the Query type are placed within a `Query` object property.
*   The function name must match the field name defined in the `Query` type (e.g., `games` resolver for the `games` field).
*   These functions return the requested data (e.g., `DB.games`). Apollo Server automatically filters the returned data to match only the fields the user requested in the query, preventing overfetching.

**Accessing Query Variables in Resolvers**
*   Resolver functions automatically receive several arguments, including `args`.
*   The **`args`** object contains any query variables sent with the request (e.g., `args.id`). This ID is used to search the data (e.g., `db.reviews.find(...)`) and return the matching single item.

---

### Slide 9: Resolving Nested and Related Data

**Defining Relationships in Schema**
*   To enable nested queries, relationships must be defined in the schema.
*   Example: Inside the `Review` type, you can define related resources: `game: Game!` and `author: Author!`.
*   Example: Inside the `Game` type, you can define related reviews: `reviews: [Review!]`.

**Type-Specific Resolvers**
*   Nested queries are handled by resolvers placed on the specific data type they belong to (e.g., the `Game` property within the `resolvers` object).
*   This approach ensures Apollo knows how to resolve subsets of data (e.g., finding only reviews related to a specific game).

**The Parent Argument**
*   These resolvers utilize the first automatic argument, **`parent`**.
*   The `parent` argument references the value returned by the previous (or parent) resolver in the query chain.
*   Example: When resolving `reviews` nested under a `Game` query, the `parent` argument will be the `Game` object, allowing access to `parent.id` to filter the reviews.

---

### Slide 10: Mutations: Data Modification

**Purpose of Mutations**
*   A **mutation** is the generic term in GraphQL for any change made to the data (adding, deleting, or updating).
*   Mutations are defined in the schema using the special **`Mutation`** type.

**Defining Mutations**
*   Mutations specify arguments needed (e.g., ID for deletion) and define a return type (e.g., returning the updated list of games).
*   Mutation resolvers are placed in a `mutation` property within the main `resolvers` object.

**Using Input Types for Flexibility**
*   For mutations that require many fields (like adding a new game), it is best practice to define a special **input type**.
*   Input types are defined using the `input` keyword (e.g., `input AddGameInput`).
*   Input types group several fields into a single argument for the mutation (e.g., `addGame(game: AddGameInput!): Game`).
*   When updating data, creating a separate input type (e.g., `EditGameInput`) where fields are *not* required enhances flexibility, allowing a user to update only one field (like `title`) without having to provide all fields.

---

### Slide 11: Summary and Next Steps

**Key Concepts Reviewed**
*   Schema definition using `typeDefs` (including scalar types and required fields).
*   Defining entry points via the mandatory **Query** type.
*   Implementing **Resolvers** to handle root queries.
*   Handling single items using **Query Variables**.
*   Traversing the graph and resolving related data using the **Parent Argument**.
*   Modifying data using **Mutations** and **Input Types**.

**Testing Tools**
*   We used **Apollo Explorer** (accessible on localhost:4000 after server setup) to send and test queries and mutations. Apollo Explorer acts as the GraphQL equivalent of Postman for REST APIs.

**Future Learning**
*   This course provides the basics necessary to move on to more advanced applications of GraphQL, such as integrating it into frameworks like Next.js.

---

## Video

 * [GraphQL Course for Beginners](https://www.youtube.com/watch?v=5199E50O7SI)
	> [<img src="https://img.youtube.com/vi/5199E50O7SI/0.jpg" width="200">](https://www.youtube.com/watch?v=5199E50O7SI "GraphQL Course for Beginners by freeCodeCamp.org 142,762 views 1 hour, 29 minutes")

## References

1. https://www.youtube.com/watch?v=xMCnDesBggM&list=PL4cUxeGkcC9gUxtblNUahcsg0WLxmrK_y
2. https://github.com/iamshaunjp/graphql-crash-course
