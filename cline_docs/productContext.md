# Product Context

## Why this project exists

The Hoarder API exists to provide a backend service for managing personal knowledge and information organization. It allows users to store, organize, and retrieve digital content efficiently.

## What problems it solves

-   **Information overload**: Helps users manage the vast amount of information they encounter daily.
-   **Disorganization**: Provides a structured way to store and categorize digital content.
-   **Accessibility**: Enables users to access their saved information from anywhere.
-   **Searchability**: Allows users to quickly find the information they need.

## How it should work

The Hoarder API is a RESTful API that provides endpoints for managing bookmarks, highlights, lists, and tags. It allows users to:

-   Create, retrieve, update, and delete bookmarks.
-   Create, retrieve, update, and delete highlights associated with bookmarks.
-   Create, retrieve, update, and delete lists of bookmarks.
-   Create, retrieve, update, and delete tags and associate them with bookmarks.
-   Manage assets associated with bookmarks.
-   Perform health checks on the API.

The API uses tRPC for type-safe API endpoints and handles asset uploads. It is designed to be scalable and maintainable, with a clear separation of concerns and a well-defined architecture.