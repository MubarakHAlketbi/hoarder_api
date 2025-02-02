# Individual Tag Endpoint

## Overview
This endpoint manages operations on individual tags, providing functionality to retrieve, update, and delete specific tags by their ID.

## Technical Details

### Endpoint Information
- **URL**: `/api/v1/tags/:tagId`
- **Methods**: GET, PATCH, DELETE
- **Authentication**: Required
- **Dynamic**: Yes (force-dynamic enabled)

## Operations

### 1. Get Tag (GET)
Retrieves a specific tag by its ID.

#### Parameters
- `tagId` (path parameter): The unique identifier of the tag

#### Response
- **Status**: 200 OK
- **Body**: Tag object
```typescript
{
  id: string;
  name: string;
  // Other tag properties
}
```

### 2. Update Tag (PATCH)
Updates an existing tag with new data.

#### Parameters
- `tagId` (path parameter): The unique identifier of the tag

#### Request Body
Uses `zUpdateTagRequestSchema` from `@hoarder/shared/types/tags` with `tagId` omitted.
```typescript
{
  // Optional tag update fields
  name?: string;
  // Other updateable properties
}
```

#### Response
- **Status**: 200 OK
- **Body**: Updated tag object

### 3. Delete Tag (DELETE)
Permanently removes a tag.

#### Parameters
- `tagId` (path parameter): The unique identifier of the tag

#### Response
- **Status**: 204 No Content
- **Body**: Empty

### Implementation
```typescript
import { NextRequest } from "next/server";
import { buildHandler } from "@/app/api/v1/utils/handler";
import { zUpdateTagRequestSchema } from "@hoarder/shared/types/tags";

export const dynamic = "force-dynamic";

export const GET = (
  req: NextRequest,
  { params }: { params: { tagId: string } },
) =>
  buildHandler({
    req,
    handler: async ({ api }) => {
      const tag = await api.tags.get({
        tagId: params.tagId,
      });
      return {
        status: 200,
        resp: tag,
      };
    },
  });

export const PATCH = (
  req: NextRequest,
  { params }: { params: { tagId: string } },
) =>
  buildHandler({
    req,
    bodySchema: zUpdateTagRequestSchema.omit({ tagId: true }),
    handler: async ({ api, body }) => {
      const tag = await api.tags.update({
        tagId: params.tagId,
        ...body!,
      });
      return { status: 200, resp: tag };
    },
  });

export const DELETE = (
  req: NextRequest,
  { params }: { params: { tagId: string } },
) =>
  buildHandler({
    req,
    handler: async ({ api }) => {
      await api.tags.delete({
        tagId: params.tagId,
      });
      return {
        status: 204,
      };
    },
  });
```

## Usage Examples

### Get Tag
```bash
curl http://localhost:3000/api/v1/tags/123 \
  -H "Authorization: Bearer YOUR_TOKEN"
```

### Update Tag
```bash
curl -X PATCH http://localhost:3000/api/v1/tags/123 \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Updated Tag Name"
  }'
```

### Delete Tag
```bash
curl -X DELETE http://localhost:3000/api/v1/tags/123 \
  -H "Authorization: Bearer YOUR_TOKEN"
```

## Notes
- Uses the common buildHandler utility for consistent error handling
- Validates tag updates using shared schema
- Returns appropriate status codes (200, 204) for different operations
- Force-dynamic ensures fresh data on each request
- Tag operations are user-specific through authentication
- Delete operation is permanent and cannot be undone
- Update operation allows partial updates through PATCH method
- Tag deletion may affect bookmark categorization