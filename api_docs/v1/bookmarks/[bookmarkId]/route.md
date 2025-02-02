# Individual Bookmark Operations

## Overview
This endpoint handles operations on individual bookmarks, providing functionality to retrieve, update, and delete specific bookmarks by their ID.

## Technical Details

### Endpoint Information
- **URL**: `/api/v1/bookmarks/:bookmarkId`
- **Methods**: GET, PATCH, DELETE
- **Authentication**: Required
- **Dynamic**: Yes (force-dynamic enabled)

## Operations

### 1. Get Bookmark (GET)
Retrieves a specific bookmark by its ID.

#### Parameters
- `bookmarkId` (path parameter): The unique identifier of the bookmark

#### Response
- **Status**: 200 OK
- **Body**: Bookmark object
```typescript
{
  // Bookmark details
  id: string;
  // Other bookmark properties
}
```

### 2. Update Bookmark (PATCH)
Updates an existing bookmark with new data.

#### Parameters
- `bookmarkId` (path parameter): The unique identifier of the bookmark

#### Request Body
Uses `zUpdateBookmarksRequestSchema` from `@hoarder/shared/types/bookmarks` with `bookmarkId` omitted.

```typescript
{
  // Optional bookmark update fields
  title?: string;
  url?: string;
  // Other updateable properties
}
```

#### Response
- **Status**: 200 OK
- **Body**: Updated bookmark object

### 3. Delete Bookmark (DELETE)
Permanently removes a bookmark.

#### Parameters
- `bookmarkId` (path parameter): The unique identifier of the bookmark

#### Response
- **Status**: 204 No Content
- **Body**: Empty

### Implementation
```typescript
import { NextRequest } from "next/server";
import { buildHandler } from "@/app/api/v1/utils/handler";
import { zUpdateBookmarksRequestSchema } from "@hoarder/shared/types/bookmarks";

export const dynamic = "force-dynamic";

export const GET = (
  req: NextRequest,
  { params }: { params: { bookmarkId: string } },
) =>
  buildHandler({
    req,
    handler: async ({ api }) => {
      const bookmark = await api.bookmarks.getBookmark({
        bookmarkId: params.bookmarkId,
      });
      return { status: 200, resp: bookmark };
    },
  });

export const PATCH = (
  req: NextRequest,
  { params }: { params: { bookmarkId: string } },
) =>
  buildHandler({
    req,
    bodySchema: zUpdateBookmarksRequestSchema.omit({ bookmarkId: true }),
    handler: async ({ api, body }) => {
      const bookmark = await api.bookmarks.updateBookmark({
        bookmarkId: params.bookmarkId,
        ...body!,
      });
      return { status: 200, resp: bookmark };
    },
  });

export const DELETE = (
  req: NextRequest,
  { params }: { params: { bookmarkId: string } },
) =>
  buildHandler({
    req,
    handler: async ({ api }) => {
      await api.bookmarks.deleteBookmark({
        bookmarkId: params.bookmarkId,
      });
      return { status: 204 };
    },
  });
```

## Usage Examples

### Get Bookmark
```bash
curl http://localhost:3000/api/v1/bookmarks/123
```

### Update Bookmark
```bash
curl -X PATCH http://localhost:3000/api/v1/bookmarks/123 \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Updated Title",
    "url": "https://updated-url.com"
  }'
```

### Delete Bookmark
```bash
curl -X DELETE http://localhost:3000/api/v1/bookmarks/123
```

## Notes
- All operations require authentication
- Uses standard HTTP status codes (200, 204)
- Update operation validates request body using shared schema
- Delete operation is permanent and cannot be undone
- Force dynamic ensures fresh data on each request
- Uses the common `buildHandler` utility for consistent error handling