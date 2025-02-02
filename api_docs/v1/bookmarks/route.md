# Bookmarks Endpoint

## Overview
This endpoint handles the core bookmark operations in the Hoarder API, allowing users to retrieve and create bookmarks. It supports pagination and filtering options for bookmark retrieval.

## Technical Details

### Endpoint Information
- **URL**: `/api/v1/bookmarks`
- **Methods**: GET, POST
- **Authentication**: Required

### GET Request
Retrieves a paginated list of bookmarks with optional filters.

#### Query Parameters
- `favourited` (optional): Boolean as string to filter favourited bookmarks
- `archived` (optional): Boolean as string to filter archived bookmarks
- Pagination parameters:
  - `page`: Current page number
  - `limit`: Number of items per page

#### Response Format
```typescript
{
  items: Bookmark[];
  meta: {
    currentPage: number;
    totalPages: number;
    totalItems: number;
  }
}
```

### POST Request
Creates a new bookmark.

#### Request Body Schema
Uses `zNewBookmarkRequestSchema` from `@hoarder/shared/types/bookmarks` which defines the required fields for creating a new bookmark.

#### Response
- Status: 201 Created
- Body: The created bookmark object

### Implementation
```typescript
import { NextRequest } from "next/server";
import { z } from "zod";
import { zNewBookmarkRequestSchema } from "@hoarder/shared/types/bookmarks";
import { buildHandler } from "../utils/handler";
import { adaptPagination, zPagination } from "../utils/pagination";
import { zStringBool } from "../utils/types";

export const dynamic = "force-dynamic";

export const GET = (req: NextRequest) =>
  buildHandler({
    req,
    searchParamsSchema: z
      .object({
        favourited: zStringBool.optional(),
        archived: zStringBool.optional(),
      })
      .and(zPagination),
    handler: async ({ api, searchParams }) => {
      const bookmarks = await api.bookmarks.getBookmarks({
        ...searchParams,
      });
      return { status: 200, resp: adaptPagination(bookmarks) };
    },
  });

export const POST = (req: NextRequest) =>
  buildHandler({
    req,
    bodySchema: zNewBookmarkRequestSchema,
    handler: async ({ api, body }) => {
      const bookmark = await api.bookmarks.createBookmark(body!);
      return { status: 201, resp: bookmark };
    },
  });
```

### Code Breakdown
- Uses Next.js server components with route handlers
- Implements request validation using Zod schemas
- Uses `buildHandler` utility for consistent error handling and response formatting
- Supports dynamic content with `force-dynamic` export
- Implements pagination through `adaptPagination` utility
- Handles both query parameters and request body validation

## Usage Examples

### Fetch Bookmarks
```bash
# Get first page of bookmarks
curl http://localhost:3000/api/v1/bookmarks?page=1&limit=10

# Get favourited bookmarks
curl http://localhost:3000/api/v1/bookmarks?favourited=true&page=1&limit=10
```

### Create Bookmark
```bash
curl -X POST http://localhost:3000/api/v1/bookmarks \
  -H "Content-Type: application/json" \
  -d '{
    "url": "https://example.com",
    "title": "Example Website"
  }'
```

## Notes
- The endpoint uses the `buildHandler` utility for consistent error handling and response formatting
- Pagination is handled automatically through the `zPagination` schema and `adaptPagination` utility
- All requests are validated using Zod schemas before processing
- The endpoint is marked as dynamic to ensure fresh data on each request