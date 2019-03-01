# RAPID.IO

Making Back End development faster, much more scalable, easier and with the least amount of boilerplate code.

## Contents

- [Core Concepts](#core-concepts)
  - [The Chain](#the-chain)
  - [Blocks](#blocks)
- [Four Principles](#four-principles)
  - [Endpoints](#endpoints)
  - [Middlewares](#middlewares)
  - [Models](#models)
  - [Utils](#utils)

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

Rapid.io is more similar to React than Angular and NestJS in that it does not provide any built-in Blocks or implement any helpers like the HTTP client in Angular or Pipes in NestJS. You can create a Block to wrap an external library and inject it into another Block.

Features:

- Blocks are pieces of code designed to be chained together to form a bigger structure called **The Chain**.
- Each block is a node module and has its own `package.json`.
- Each block is instantiated one or more times with a different Injection Token each time.
- Each block can be injected to any other block and has its own unique Injection Token which can be manually set for simplicity.

### Blocks may communicate with each other

Blocks can be injected into other Blocks classes, for example:

> An authentication middleware may inject a User Model to lookup the user and validate privilages.

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

Blocks are the most fundmental building block of The Chain and one block can be used multiple times with different configurations, for example:

> A validation middleware block may be used to build different endpoints and configured with different models.

### All block injections are optional

If the injector was not able to find the target block, the main block must implement a way to gracefully handle the missing dependency.

## Four Principles

### Endpoints

Chain's last building block is an endpoint. An endpoint can be designed to do many things. Example endpoints:

- Create
- Read
- Update
- Delete
- Paginate

### Middlewares

Endpoints can have a parent of type Middleware back in the chain. Example middlewares:

- Body validation
- Error handling
- Authentication

### Models

Models are data schemas:

- Control the data stored in the database.
- Generate schemas to be used for request body validation.
- Serialize data for response
  > e.g. Removing password from user object


### Utils

Basically, utils are wrappers for external libraries like: `winston`, `express`, `swagger` or `cors`.
