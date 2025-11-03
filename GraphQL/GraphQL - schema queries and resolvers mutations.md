# GraphQL - schema queries and resolvers mutations

## Table of Contents
- [GraphQL - schema queries and resolvers mutations](#graphql---schema-queries-and-resolvers-mutations)
  - [Table of Contents](#table-of-contents)
  - [GraphQL](#graphql)
    - [GraphQL Schema:](#graphql-schema)
    - [Resolvers:](#resolvers)
  - [Video](#video)
  - [References](#references)

## GraphQL
GraphQL utilizes a type system to define the capabilities of an API through its schema, which includes definitions for queries and mutations. Resolvers provide the implementation details for these operations.

### GraphQL Schema:

The schema defines the structure of the data and the operations that can be performed.
Queries: These are used to fetch data from the server. The `Query` type in the schema defines the entry points for reading data.

Code:
```js
    type Query {
      books: [Book!]!
      book(id: ID!): Book
    }

    type Book {
      id: ID!
      title: String!
      author: String!
    }
```
In this example, `books` is a query that returns a list of Book objects, and `book(id: ID!)` is a query that returns a single `Book` object based on its ID.

- **Mutations**: These are used to modify data on the server (create, update, delete). The `Mutation` type defines the entry points for writing data.

Code:
```js
    type Mutation {
      addBook(title: String!, author: String!): Book!
      updateBook(id: ID!, title: String, author: String): Book
      deleteBook(id: ID!): Boolean!
    }
```

Here, `addBook` creates a new book, `updateBook` modifies an existing one, and `deleteBook` removes a book.

### Resolvers:
Resolvers are functions that correspond to each field in the schema and define how to retrieve or modify the data for that field. They act as the bridge between the GraphQL schema and the actual data sources (e.g., databases, REST APIs).
- **Query Resolvers**: These functions handle the logic for fetching data requested by queries.
```JavaScript
    const resolvers = {
      Query: {
        books: () => {
          // Logic to fetch all books from a database or API
          return [{ id: '1', title: 'The Hitchhiker\'s Guide to the Galaxy', author: 'Douglas Adams' }];
        },
        book: (parent, args) => {
          // Logic to fetch a specific book based on args.id
          return { id: args.id, title: 'Specific Book', author: 'Specific Author' };
        },
      },
      // ... other resolvers for types like Book
    };
```

- **Mutation Resolvers**: These functions handle the logic for modifying data requested by mutations.
```JavaScript
    const resolvers = {
      // ... Query resolvers
      Mutation: {
        addBook: (parent, args) => {
          // Logic to add a new book to the database
          const newBook = { id: '2', title: args.title, author: args.author };
          return newBook;
        },
        updateBook: (parent, args) => {
          // Logic to update an existing book in the database
          return { id: args.id, title: args.title || 'Updated Title', author: args.author || 'Updated Author' };
        },
        deleteBook: (parent, args) => {
          // Logic to delete a book from the database
          return true; // Indicate success
        },
      },
    };
```

In essence, the schema defines what operations are available and the structure of the data, while the resolvers define how those operations are executed and how the data is retrieved or manipulated.

## Video
 * [Generating TypeScript Types for GraphQL Schema, Queries and Mutations Using GraphQL Code Generator](https://www.youtube.com/watch?v=DFTVPZvgnaQ)
	> [<img src="https://img.youtube.com/vi/DFTVPZvgnaQ/0.jpg" width="200">](https://www.youtube.com/watch?v=DFTVPZvgnaQ "In this video I'll show how to generate TypeScript types for GraphQL schema, resolvers, queries and mutations using GraphQL Code Generator. by Dmytro Danylov 19 k views 2 minutes, 23 seconds")

 * [GraphQL Crash Course #8 - Mutations (Adding & Deleting Data)](https://www.youtube.com/watch?v=MnDbZK6B8uE)
	> [<img src="https://img.youtube.com/vi/MnDbZK6B8uE/0.jpg" width="200">](https://www.youtube.com/watch?v=MnDbZK6B8uE "In this video I'll show how to generate TypeScript types for GraphQL schema, resolvers, queries and mutations using GraphQL Code Generator. by Net Ninja 19 k views 2 minutes, 3 seconds")

## References
1. https://graphql.org/learn/mutations/
2. https://graphql.org/learn/queries/
3. https://graphql.org/learn/schema/
4. 