# ZONE

## Core Concepts

### Blocks

Blocks are pieces of code designed to add more functionality to a larger system. They take a configuration object to construct an Endpoint, Middleware, Model or a Util. Each block is a node module and has its own `package.json`.

### Blocks may communicate with each other

A linker is what connects two blocks together, for example:

> An authentication middleware may conenct to a User model.

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