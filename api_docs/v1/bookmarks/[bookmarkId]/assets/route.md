# Bookmark Assets Endpoint

## Overview
This endpoint manages the relationship between bookmarks and their associated assets, providing functionality to list existing assets and attach new assets to a bookmark.

## Technical Details

### Endpoint Information
- **URL**: `/api/v1/bookmarks/:bookmarkId/assets`
- **Methods**: GET, POST
- **Authentication**: Required
- **Dynamic**: Yes (force-dynamic enabled)

## Operations

### 1. List Bookmark Assets (GET)
Retrieves all assets associated with a specific bookmark.

#### Parameters
- `bookmarkId` (path parameter): The unique identifier of the bookmark

#### Response
- **Status**: 200 OK
- **Body**:
```typescript
{
  assets: {
    // Array of assets associated with the bookmark
    id: string;
    // Other asset properties
  }[]
}
```

### 2. Attach Asset (POST)
Attaches an existing asset to a bookmark.

#### Parameters
- `bookmarkId` (path parameter): The unique identifier of the bookmark

#### Request Body
Uses `zAssetSchema` from `@hoarder/shared/types/bookmarks` for validation.
```typescript
{
  // Asset properties as defined in zAssetSchema
}
```

#### Response
- **Status**: 201 Created
- **Body**: The attached asset object

### Implementation
```typescript
import { NextRequest } from "next/server";
import { buildHandler } from "@/app/api/v1/utils/handler";
import { zAssetSchema } from "@hoarder/shared/types/bookmarks";

export const dynamic = "force-dynamic";

export const GET = (
  req: NextRequest,
  params: { params: { bookmarkId: string } },
) =>
  buildHandler({
    req,
    handler: async ({ api }) => {
      const resp = await api.bookmarks.getBookmark({
        bookmarkId: params.params.bookmarkId,
      });
      return { status: 200, resp: { assets: resp.assets } };
    },
  });

export const POST = (
  req: NextRequest,
  params: { params: { bookmarkId: string } },
) =>
  buildHandler({
    req,
    bodySchema: zAssetSchema,
    handler: async ({ api, body }) => {
      const asset = await api.bookmarks.attachAsset({
        bookmarkId: params.params.bookmarkId,
        asset: body!,
      });
      return { status: 201, resp: asset };
    },
  });
```

## Usage Examples

### List Bookmark Assets
```bash
curl http://localhost:3000/api/v1/bookmarks/123/assets \
  -H "Authorization: Bearer YOUR_TOKEN"
```

### Attach Asset to Bookmark
```bash
curl -X POST http://localhost:3000/api/v1/bookmarks/123/assets \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    // Asset properties according to zAssetSchema
  }'
```

## Notes
- Uses the common buildHandler utility for consistent error handling
- Validates asset attachment using zAssetSchema
- Returns 201 status for successful asset attachment
- Force-dynamic ensures fresh data on each request
- Asset must exist before it can be attached to a bookmark
- Integrates with both bookmark and asset management systems
- Provides a way to organize assets within the context of bookmarks