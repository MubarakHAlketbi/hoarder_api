# Hoarder API Documentation

## Overview

This document provides a comprehensive overview of the Hoarder API, its architecture, endpoints, and usage. The Hoarder API is designed to manage bookmarks, highlights, lists, and tags, providing functionalities for creating, retrieving, updating, and deleting these resources.

## Documentation

The following documentation files are available in the root directory:

-   `README.md`: Overview of the project and API documentation.
-   `utils.md`: Utility functions used across the API.
-   `bookmarks.md`: Detailed documentation for the `/v1/bookmarks` endpoints.
-   `highlights.md`: Detailed documentation for the `/v1/highlights` endpoints.
-   `lists.md`: Detailed documentation for the `/v1/lists` endpoints.
-   `tags.md`: Detailed documentation for the `/v1/tags` endpoints.
-   `assets.md`: Documentation for asset management.
-   `health.md`: Documentation for the health check endpoint.
-   `trpc.md`: Documentation for the tRPC setup.
-   `auth.md`: Documentation for the authentication setup.

## API Endpoints (v1)

### Bookmarks

-   `GET /v1/bookmarks`: Retrieve all bookmarks.
-   `POST /v1/bookmarks`: Create a new bookmark.
-   `GET /v1/bookmarks/:bookmarkId`: Retrieve a specific bookmark.
-   `PUT /v1/bookmarks/:bookmarkId`: Update a bookmark.
-   `DELETE /v1/bookmarks/:bookmarkId`: Delete a bookmark.
-   `GET /v1/bookmarks/:bookmarkId/assets`: Retrieve assets for a bookmark.
-   `POST /v1/bookmarks/:bookmarkId/assets`: Add an asset to a bookmark.
-   `GET /v1/bookmarks/:bookmarkId/assets/:assetId`: Retrieve a specific asset.
-   `DELETE /v1/bookmarks/:bookmarkId/assets/:assetId`: Delete an asset.
-   `GET /v1/bookmarks/:bookmarkId/highlights`: Retrieve highlights for a bookmark.
-   `POST /v1/bookmarks/:bookmarkId/highlights`: Add a highlight to a bookmark.
-   `GET /v1/bookmarks/:bookmarkId/lists`: Retrieve lists associated with a bookmark.
-   `POST /v1/bookmarks/:bookmarkId/lists`: Add a bookmark to a list.
-   `GET /v1/bookmarks/:bookmarkId/tags`: Retrieve tags for a bookmark.
-   `POST /v1/bookmarks/:bookmarkId/tags`: Add a tag to a bookmark.
-   `GET /v1/bookmarks/search`: Search bookmarks.
-   `POST /v1/bookmarks/singlefile`: Import bookmarks from SingleFile.

### Highlights

-   `GET /v1/highlights`: Retrieve all highlights.
-   `POST /v1/highlights`: Create a new highlight.
-   `GET /v1/highlights/:highlightId`: Retrieve a specific highlight.
-   `PUT /v1/highlights/:highlightId`: Update a highlight.
-   `DELETE /v1/highlights/:highlightId`: Delete a highlight.

### Lists

-   `GET /v1/lists`: Retrieve all lists.
-   `POST /v1/lists`: Create a new list.
-   `GET /v1/lists/:listId`: Retrieve a specific list.
-   `PUT /v1/lists/:listId`: Update a list.
-   `DELETE /v1/lists/:listId`: Delete a list.
-   `GET /v1/lists/:listId/bookmarks`: Retrieve bookmarks in a list.
-   `POST /v1/lists/:listId/bookmarks`: Add a bookmark to a list.
-   `DELETE /v1/lists/:listId/bookmarks/:bookmarkId`: Remove a bookmark from a list.

### Tags

-   `GET /v1/tags`: Retrieve all tags.
-   `POST /v1/tags`: Create a new tag.
-   `GET /v1/tags/:tagId`: Retrieve a specific tag.
-   `PUT /v1/tags/:tagId`: Update a tag.
-   `DELETE /v1/tags/:tagId`: Delete a tag.
-   `GET /v1/tags/:tagId/bookmarks`: Retrieve bookmarks associated with a tag.

## Further Development

This documentation is located in the root directory and will be continuously updated as the API evolves. Future updates will include more detailed information on each endpoint, request/response formats, error handling, and other relevant aspects of the Hoarder API.