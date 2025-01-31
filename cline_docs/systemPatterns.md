# System Patterns

## How the system is built

The Hoarder API is built as a RESTful API using a modular design. It is organized into several key modules, each responsible for a specific part of the system's functionality. The API is versioned (v1) and uses tRPC for type-safe endpoints.

## Key technical decisions

-   **RESTful API**: The API follows REST principles for managing resources, providing a standardized way to interact with the system.
-   **tRPC**: Using tRPC ensures type safety and improves the developer experience by providing autocompletion and compile-time error checking.
-   **Modular Design**: Organizing the code into modules improves maintainability, readability, and reusability.
-   **API Versioning**: Versioning the API allows for future updates and changes without breaking existing clients.
-   **Schema Validation**: Using `zod` for schema validation ensures that the data received by the API conforms to the expected format, reducing errors and improving data integrity.
-   **Pagination**: Implementing pagination allows the API to handle large datasets efficiently by returning data in smaller chunks.
-   **Error Handling**: Using a standardized error handling mechanism (e.g., `buildHandler`) ensures that errors are handled consistently and communicated effectively to the client.

## Architecture patterns

-   **Modular Architecture**: The system is divided into modules, each with a specific responsibility (e.g., `bookmarks`, `highlights`, `lists`, `tags`, `utils`, `assets`, `health`, `trpc`).
-   **Layered Architecture**: The API can be seen as having different layers:
    -   **Presentation Layer**: Handles incoming requests and outgoing responses (e.g., route handlers).
    -   **Service Layer**: Contains the business logic (e.g., tRPC procedures).
    -   **Data Access Layer**: Interacts with the database (e.g., database queries).
-   **Dependency Injection**: The `buildHandler` function can be seen as a form of dependency injection, where the `api` object (tRPC client) is injected into the handler function.
-   **Singleton**: The tRPC router (`appRouter`) can be considered a singleton, as it is a single instance that is used throughout the application.