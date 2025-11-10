# Table Data Gateway pattern

The Table Data Gateway pattern represents a way to encapsulate all SQL interactions related to a single database table within a dedicated object. This object acts as a "gateway" to the table, handling operations like selecting, inserting, updating, and deleting rows, without containing any business logic.
While the concept of a Table Data Gateway is a design pattern applicable across various programming languages, there isn't a single, universally recognized "TableGateway" library or module specifically for Node.js in the same way there might be in frameworks like Zend Framework for PHP (where zend-db/table-gateway is a core component).

## Table of Contents

- [Table Data Gateway pattern](#table-data-gateway-pattern)
  - [Table of Contents](#table-of-contents)
  - [To implement a Table Data Gateway in Node.js, one would typically:](#to-implement-a-table-data-gateway-in-nodejs-one-would-typically)
  - [Example (Conceptual):](#example-conceptual)
  - [Considerations:](#considerations)
  - [References](#references)

## To implement a Table Data Gateway in Node.js, one would typically:

- Define a class or object for each table: This class would represent the gateway for a specific database table (e.g., UserGateway, ProductGateway).
- Encapsulate database interactions: Methods within this class would handle all SQL queries for that table, such as findById(id), insert(data), update(id, data), delete(id).
- Use a database driver: The methods would utilize a Node.js database driver (e.g., mysql, pg, mongodb) to connect and interact with the database.
- Return raw data: The gateway methods would typically return raw data (e.g., JSON objects or arrays of objects) directly from the database, without transforming it into complex domain objects.

## Example (Conceptual):

```JavaScript
class UserGateway {
    constructor(dbConnection) {
        this.db = dbConnection; // Assume dbConnection is an active database connection
    }

    async findById(id) {
        const [rows] = await this.db.query('SELECT * FROM users WHERE id = ?', [id]);
        return rows[0];
    }

    async insert(userData) {
        const [result] = await this.db.query('INSERT INTO users SET ?', [userData]);
        return result.insertId;
    }

    async update(id, userData) {
        await this.db.query('UPDATE users SET ? WHERE id = ?', [userData, id]);
        return true;
    }

    async delete(id) {
        await this.db.query('DELETE FROM users WHERE id = ?', [id]);
        return true;
    }
}

// Usage example
// const userGateway = new UserGateway(myDbConnection);
// const user = await userGateway.findById(1);
// await userGateway.insert({ name: 'John Doe', email: 'john@example.com' });
```

## Considerations:

- Simplicity: The Table Data Gateway pattern is best suited for simpler data models where complex relationships and business logic are minimal.
- Alternatives: For more complex applications, patterns like Data Mapper or Object-Relational Mappers (ORMs) might be more appropriate.
- No specific library: While the pattern is applicable, there isn't a dedicated "TableGateway" package to install in Node.js; it's a design choice for structuring your database interactions.

## References

1. https://medium.com/caquicoders/criando-um-simples-api-gateway-com-nodejs-e-express-2ec5369e975d
2. https://www.martinfowler.com/eaaCatalog/tableDataGateway.html

