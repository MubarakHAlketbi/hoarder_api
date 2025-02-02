# Hoarder API Documentation

## Overview

This document provides a comprehensive overview of the Hoarder API, its architecture, endpoints, and usage. The Hoarder API is designed to manage bookmarks, highlights, lists, and tags, providing functionalities for creating, retrieving, updating, and deleting these resources.

## Documentation Structure

The API documentation is organized in a mirrored structure to the codebase, making it easy to find corresponding documentation for any code file:

```
api_docs/
├── health/
│   └── route.md              # Health check endpoint
├── assets/
│   ├── route.md             # Asset upload
│   └── [assetId]/
│       └── route.md         # Individual asset operations
├── v1/
│   ├── utils/
│   │   ├── handler.md       # Request handler utility
│   │   ├── pagination.md    # Pagination utility
│   │   └── types.md        # Type definitions
│   ├── bookmarks/
│   │   ├── route.md        # Bookmark collection
│   │   ├── search/
│   │   │   └── route.md    # Bookmark search
│   │   └── [bookmarkId]/
│   │       ├── route.md    # Individual bookmark
│   │       └── assets/     # Bookmark assets
│   ├── lists/
│   │   ├── route.md        # List collection
│   │   └── [listId]/
│   │       ├── route.md    # Individual list
│   │       └── bookmarks/  # List bookmarks
│   └── tags/
│       ├── route.md        # Tag collection
│       └── [tagId]/
│           ├── route.md    # Individual tag
│           └── bookmarks/  # Tagged bookmarks
```

Each documentation file includes:
- Endpoint/utility overview
- Technical details
- Implementation examples
- Usage examples with curl commands
- Notes and best practices

## API Endpoints (v1)

### Bookmarks

- `GET /v1/bookmarks`: Retrieve all bookmarks.
- `POST /v1/bookmarks`: Create a new bookmark.
- `GET /v1/bookmarks/:bookmarkId`: Retrieve a specific bookmark.
- `PUT /v1/bookmarks/:bookmarkId`: Update a bookmark.
- `DELETE /v1/bookmarks/:bookmarkId`: Delete a bookmark.
- `GET /v1/bookmarks/:bookmarkId/assets`: Retrieve assets for a bookmark.
- `POST /v1/bookmarks/:bookmarkId/assets`: Add an asset to a bookmark.
- `GET /v1/bookmarks/:bookmarkId/assets/:assetId`: Retrieve a specific asset.
- `DELETE /v1/bookmarks/:bookmarkId/assets/:assetId`: Delete an asset.
- `GET /v1/bookmarks/:bookmarkId/highlights`: Retrieve highlights for a bookmark.
- `POST /v1/bookmarks/:bookmarkId/highlights`: Add a highlight to a bookmark.
- `GET /v1/bookmarks/:bookmarkId/lists`: Retrieve lists associated with a bookmark.
- `POST /v1/bookmarks/:bookmarkId/lists`: Add a bookmark to a list.
- `GET /v1/bookmarks/:bookmarkId/tags`: Retrieve tags for a bookmark.
- `POST /v1/bookmarks/:bookmarkId/tags`: Add a tag to a bookmark.
- `GET /v1/bookmarks/search`: Search bookmarks.
- `POST /v1/bookmarks/singlefile`: Import bookmarks from SingleFile.

### Highlights

- `GET /v1/highlights`: Retrieve all highlights.
- `POST /v1/highlights`: Create a new highlight.
- `GET /v1/highlights/:highlightId`: Retrieve a specific highlight.
- `PUT /v1/highlights/:highlightId`: Update a highlight.
- `DELETE /v1/highlights/:highlightId`: Delete a highlight.

### Lists

- `GET /v1/lists`: Retrieve all lists.
- `POST /v1/lists`: Create a new list.
- `GET /v1/lists/:listId`: Retrieve a specific list.
- `PUT /v1/lists/:listId`: Update a list.
- `DELETE /v1/lists/:listId`: Delete a list.
- `GET /v1/lists/:listId/bookmarks`: Retrieve bookmarks in a list.
- `POST /v1/lists/:listId/bookmarks`: Add a bookmark to a list.
- `DELETE /v1/lists/:listId/bookmarks/:bookmarkId`: Remove a bookmark from a list.

### Tags

- `GET /v1/tags`: Retrieve all tags.
- `POST /v1/tags`: Create a new tag.
- `GET /v1/tags/:tagId`: Retrieve a specific tag.
- `PUT /v1/tags/:tagId`: Update a tag.
- `DELETE /v1/tags/:tagId`: Delete a tag.
- `GET /v1/tags/:tagId/bookmarks`: Retrieve bookmarks associated with a tag.

## Documentation Features

- **Mirrored Structure**: Documentation follows the same structure as the codebase
- **Comprehensive Coverage**: Each endpoint and utility has detailed documentation
- **Code Examples**: Implementation details with TypeScript code
- **Usage Examples**: Practical curl commands for API interaction
- **Type Safety**: Detailed type information for request/response schemas
- **Best Practices**: Notes on usage patterns and considerations

## Further Development

The documentation is maintained alongside the codebase in the api_docs directory. Each code file in api_code has a corresponding markdown file in api_docs with detailed documentation. This structure ensures documentation stays current with code changes and makes it easy to find relevant documentation for any part of the API.