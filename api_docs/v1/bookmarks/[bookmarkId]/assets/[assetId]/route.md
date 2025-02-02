# Individual Bookmark Asset Endpoint

## Overview
This endpoint manages operations on individual assets associated with a bookmark, providing functionality to replace existing assets and remove asset associations.

## Technical Details

### Endpoint Information
- **URL**: `/api/v1/bookmarks/:bookmarkId/assets/:assetId`
- **Methods**: PUT, DELETE
- **Authentication**: Required
- **Dynamic**: Yes (force-dynamic enabled)

## Operations

### 1. Replace Asset (PUT)
Replaces an existing asset with a new one while maintaining the bookmark association.

#### Parameters
- `bookmarkId` (path parameter): The unique identifier of the bookmark
- `assetId` (path parameter): The current asset's unique identifier

#### Request Body Schema
```typescript
{
  assetId: string  // ID of the new asset to replace with
}
```

#### Response
- **Status**: 204 No Content
- **Body**: Empty

### 2. Detach Asset (DELETE)
Removes the association between an asset and a bookmark.

#### Parameters
- `bookmarkId` (path parameter): The unique identifier of the bookmark
- `assetId` (path parameter): The asset's unique identifier to detach

#### Response
- **Status**: 204 No Content
- **Body**: Empty

### Implementation
```typescript
import { NextRequest } from "next/server";
import { buildHandler } from "@/app/api/v1/utils/handler";
import { z } from "zod";

export const dynamic = "force-dynamic";

export const PUT = (
  req: NextRequest,
  params: { params: { bookmarkId: string; assetId: string } },
) =>
  buildHandler({
    req,
    bodySchema: z.object({ assetId: z.string() }),
    handler: async ({ api, body }) => {
      await api.bookmarks.replaceAsset({
        bookmarkId: params.params.bookmarkId,
        oldAssetId: params.params.assetId,
        newAssetId: body!.assetId,
      });
      return { status: 204 };
    },
  });

export const DELETE = (
  req: NextRequest,
  params: { params: { bookmarkId: string; assetId: string } },
) =>
  buildHandler({
    req,
    handler: async ({ api }) => {
      await api.bookmarks.detachAsset({
        bookmarkId: params.params.bookmarkId,
        assetId: params.params.assetId,
      });
      return { status: 204 };
    },
  });
```

## Usage Examples

### Replace Asset
```bash
curl -X PUT http://localhost:3000/api/v1/bookmarks/123/assets/456 \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "assetId": "789"
  }'
```

### Detach Asset
```bash
curl -X DELETE http://localhost:3000/api/v1/bookmarks/123/assets/456 \
  -H "Authorization: Bearer YOUR_TOKEN"
```

## Notes
- Uses the common buildHandler utility for consistent error handling
- Both operations return 204 No Content on success
- Asset replacement requires the new asset to exist
- Asset detachment only removes the association, doesn't delete the asset
- Force-dynamic ensures fresh data on each request
- Request body validation uses Zod schema
- Path parameters are used to identify both bookmark and asset