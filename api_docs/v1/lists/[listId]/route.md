# Individual List Endpoint

## Overview
This endpoint manages operations on individual bookmark lists, providing functionality to retrieve, update, and delete specific lists by their ID.

## Technical Details

### Endpoint Information
- **URL**: `/api/v1/lists/:listId`
- **Methods**: GET, PATCH, DELETE
- **Authentication**: Required
- **Dynamic**: Yes (force-dynamic enabled)

## Operations

### 1. Get List (GET)
Retrieves a specific list by its ID.

#### Parameters
- `listId` (path parameter): The unique identifier of the list

#### Response
- **Status**: 200 OK
- **Body**: List object
```typescript
{
  id: string;
  // Other list properties
}
```

### 2. Update List (PATCH)
Updates an existing list with new data.

#### Parameters
- `listId` (path parameter): The unique identifier of the list

#### Request Body
Uses `zEditBookmarkListSchema` from `@hoarder/shared/types/lists` with `listId` omitted.
```typescript
{
  // Optional list update fields
  name?: string;
  // Other updateable properties
}
```

#### Response
- **Status**: 200 OK
- **Body**: Updated list object

### 3. Delete List (DELETE)
Permanently removes a list.

#### Parameters
- `listId` (path parameter): The unique identifier of the list

#### Response
- **Status**: 204 No Content
- **Body**: Empty

### Implementation
```typescript
import { NextRequest } from "next/server";
import { buildHandler } from "@/app/api/v1/utils/handler";
import { zEditBookmarkListSchema } from "@hoarder/shared/types/lists";

export const dynamic = "force-dynamic";

export const GET = (
  req: NextRequest,
  { params }: { params: { listId: string } },
) =>
  buildHandler({
    req,
    handler: async ({ api }) => {
      const list = await api.lists.get({
        listId: params.listId,
      });
      return {
        status: 200,
        resp: list,
      };
    },
  });

export const PATCH = (
  req: NextRequest,
  { params }: { params: { listId: string } },
) =>
  buildHandler({
    req,
    bodySchema: zEditBookmarkListSchema.omit({ listId: true }),
    handler: async ({ api, body }) => {
      const list = await api.lists.edit({
        ...body!,
        listId: params.listId,
      });
      return { status: 200, resp: list };
    },
  });

export const DELETE = (
  req: NextRequest,
  { params }: { params: { listId: string } },
) =>
  buildHandler({
    req,
    handler: async ({ api }) => {
      await api.lists.delete({
        listId: params.listId,
      });
      return {
        status: 204,
      };
    },
  });
```

## Usage Examples

### Get List
```bash
curl http://localhost:3000/api/v1/lists/123 \
  -H "Authorization: Bearer YOUR_TOKEN"
```

### Update List
```bash
curl -X PATCH http://localhost:3000/api/v1/lists/123 \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Updated List Name"
  }'
```

### Delete List
```bash
curl -X DELETE http://localhost:3000/api/v1/lists/123 \
  -H "Authorization: Bearer YOUR_TOKEN"
```

## Notes
- Uses the common buildHandler utility for consistent error handling
- Validates list updates using shared schema
- Returns appropriate status codes (200, 204) for different operations
- Force-dynamic ensures fresh data on each request
- List operations are user-specific through authentication
- Delete operation is permanent and cannot be undone
- Update operation allows partial updates through PATCH method