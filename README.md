# RAPID.IO

Making Back End development faster, much more scalable, easier and with the least amount of boilerplate code.

## Core Concepts

### The Chain

Set of Blocks connected together horizontally and vertically to form a bigger structure which is the API.

For example, the diagram image below is a basic chain constructing an API with the following features:

1. Increase a counter by +1 by inserting a new document into the database containing user's `name` and `date` of insertion.
2. Count the documents inserted.
3. Log each count into the console or logfile using a `Logger` util which can be a wrapper for `winston` or native `console`.
4. Handle errors

![diagram](https://raw.githubusercontent.com/ziadalzarka/rapid.io/master/diagram.jpg)

> Each reactangle is a Block.

### Blocks

Rapid.io is more similar to React that Angular and NestJS in that it does not provide any built-in Blocks or implement any helpers like the HTTP client in Angular or Pipes in NestJS. You can create a Block to wrap an external library and inject it into another Block.

Features:

- Blocks are pieces of code designed to be chained together to form a bigger structure called the **Chain**.
- Each block is a node module and has its own `package.json`.
- Each block is instantiated one or more times with a different Injection Token each time.
- Each block can be injected to any other block and has its own unique Injection Token which can be manually set for simplicity.

### Blocks may communicate with each other

Blocks can be injected into other Blocks classes, for example:

> An authentication middleware may inject a User Model to lookup the user and validate privilages.

Another example:

> A middleware that creates a Book may need to connect to the Book model and connect to a Logger util to print out logs.

Another example:

> Book model may come with a swagger definition.
```
{
  "definitions": {
    "Book: {
      "type": "object",
      "properties": {
        "name": {
          type: "string"
        },
        "author": {
          "$ref": "#/definitions/User"
        }
      }
    }
  }
}
```

### Blocks may be used multiple times with different variables

Blocks are the most fundmental building block of a ZONE API and one block can be used multiple times with different configurations, for example:

> A validation middleware block may be used to build different endpoints and configured with different models.

### All block linkers are optional

If a linker was not able to find the target block, the main block must implement a way to gracefully handle the missing link.

## Four Principles

### Endpoints

An API main building block is an endpoint. An endpoint can be designed to do many things.

### Middlewares

Each endpoint consists of a set of middlewares. Example middlewares:
- Body validation
- Error handling
- Authentication

### Models

Models are data schemas and can:

- Control the data stored in the database.
- Generate schemas to be used for request body validation.
- Have special properties like `@Exclude` and `@Transform`.

### Utils

Basically, utils are wrappers for external libraries like: `winston`, `express`, `swagger` or `cors`.
