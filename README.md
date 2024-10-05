# ForgeAPI
ForgeAPI is a powerful and automated C++ tool designed to streamline the process of building RESTful APIs from structured JSON inputs.Acting as a "forge" for API development, ForgeAPI takes JSON schema definitions and transforms them into fully functional (Laravel-based) APIs, complete with migrations, models, controllers, routes, and middleware.

## With ForgeAPI, developers can:
  - Automate the entire API creation process from JSON.
  - Handle complex database relationships, including foreign keys, with dependency resolution.
  - Generate fully working CRUD operations for your APIs, including advanced features like pagination and custom SQL queries.
  - Reduce development time by focusing on business logic rather than manual setup.

## Key Features:

- JSON-Driven API Creation: Simply define your database structure and relationships in JSON, and ForgeAPI does the rest.
- Automated Migration and Model Generation: Create database migrations and Eloquent models in seconds.
- Controller and Route Creation: Auto-generate API controllers and routes with support for all RESTful operations.
- Dependency Management: Handle complex table relationships (foreign keys) with built-in topological sorting.
- Custom SQL Support: Define custom SQL queries for specific requests directly in your JSON schema.
- Seamless Laravel Integration: ForgeAPI outputs clean, maintainable Laravel code, ready to be extended or used as-is.

## Ideal For 
Developers who need to quickly spin up APIs for web and mobile applications, database architects looking for an easy way to implement their schema in Restful, and teams seeking to automate repetitive coding tasks.


## Tagline: "Forge Your APIs in Seconds"


## Requirements
- C++ 20
- CMAKE 3.27
- [GTEST](https://github.com/google/googletest)
- [santizers](https://github.com/google/sanitizers)
- clang-format



