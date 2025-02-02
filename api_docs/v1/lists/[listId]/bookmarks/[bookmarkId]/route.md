# List Bookmark Management Endpoint

## Overview
This endpoint manages the relationship between individual bookmarks and lists, providing functionality to add and remove bookmarks from specific lists.

## Technical Details

### Endpoint Information
- **URL**: `/api/v1/lists/:listId/bookmarks/:bookmarkId`
- **Methods**: PUT, DELETE
- **Authentication**: Required
- **Dynamic**: Yes (force-dynamic enabled)

## Operations

### 1. Add Bookmark to List (PUT)
Adds a specific bookmark to a list.

#### Known Limitation
Currently, the endpoint is not fully idempotent as required by PUT semantics. It will fail if the bookmark is already in the list. This behavior is planned to be fixed in a future update.

#### Parameters
- `listId` (path parameter): The unique identifier of the list
- `bookmarkId` (path parameter): The unique identifier of the bookmark to add

#### Response
- **Status**: 204 No Content
- **Body**: Empty

### 2. Remove Bookmark from List (DELETE)
Removes a specific bookmark from a list.

#### Parameters
- `listId` (path parameter): The unique identifier of the list
- `bookmarkId` (path parameter): The unique identifier of the bookmark to remove

#### Response
- **Status**: 204 No Content
- **Body**: Empty

### Implementation
```typescript
import { NextRequest } from "next/server";
import { buildHandler } from "@/app/api/v1/utils/handler";

export const dynamic = "force-dynamic";

export const PUT = (
  req: NextRequest,
  { params }: { params: { listId: string; bookmarkId: string } },
) =>
  buildHandler({
    req,
    handler: async ({ api }) => {
      await api.lists.addToList({
        listId: params.listId,
        bookmarkId: params.bookmarkId,
      });
      return { status: 204 };
    },
  });

export const DELETE = (
  req: NextRequest,
  { params }: { params: { listId: string; bookmarkId: string } },
) =>
  buildHandler({
    req,
    handler: async ({ api }) => {
      await api.lists.removeFromList({
        listId: params.listId,
        bookmarkId: params.bookmarkId,
      });
      return { status: 204 };
    },
  });
```

## Usage Examples

### Add Bookmark to List
```bash
curl -X PUT http://localhost:3000/api/v1/lists/123/bookmarks/456 \
  -H "Authorization: Bearer YOUR_TOKEN"
```

### Remove Bookmark from List
```bash
curl -X DELETE http://localhost:3000/api/v1/lists/123/bookmarks/456 \
  -H "Authorization: Bearer YOUR_TOKEN"
```

## Notes
- Uses the common buildHandler utility for consistent error handling
- Both operations return 204 No Content on success
- No request body required for either operation
- Force-dynamic ensures fresh data on each request
- Known issue with PUT idempotency to be addressed in future update
- Operations are atomic - they either completely succeed or fail
- No validation of bookmark existence before attempting operations