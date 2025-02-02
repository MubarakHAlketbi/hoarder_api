# Individual Asset Endpoint

## Overview
This endpoint handles retrieval of individual assets, supporting both full downloads and partial content requests through range headers. It includes authentication checks and ensures users can only access their own assets.

## Technical Details

### Endpoint Information
- **URL**: `/api/assets/:assetId`
- **Method**: GET
- **Authentication**: Required
- **Dynamic**: Yes (force-dynamic enabled)
- **Supports**: Range requests for streaming

### Authentication & Authorization
- Requires authenticated user
- Users can only access their own assets
- Validates asset ownership through database query

### Response Formats

#### Full Download (200 OK)
```typescript
Headers:
{
  "Content-Length": string,    // Total file size
  "Content-type": string      // MIME type of the asset
}
Body: Asset stream
```

#### Partial Content (206 Partial Content)
```typescript
Headers:
{
  "Content-Range": "bytes start-end/total",
  "Accept-Ranges": "bytes",
  "Content-Length": string,    // Size of the chunk
  "Content-type": string      // MIME type of the asset
}
Body: Asset stream (partial)
```

#### Error Responses
- 401 Unauthorized
  ```json
  { "error": "Unauthorized" }
  ```
- 404 Not Found
  ```json
  { "error": "Asset not found" }
  ```

### Implementation
```typescript
export const dynamic = "force-dynamic";

export async function GET(
  request: Request,
  { params }: { params: { assetId: string } },
) {
  const ctx = await createContextFromRequest(request);
  if (!ctx.user) {
    return Response.json({ error: "Unauthorized" }, { status: 401 });
  }

  // Check asset ownership
  const assetDb = await ctx.db.query.assets.findFirst({
    where: and(eq(assets.id, params.assetId), eq(assets.userId, ctx.user.id)),
  });

  if (!assetDb) {
    return Response.json({ error: "Asset not found" }, { status: 404 });
  }

  // Get asset metadata and size
  const [metadata, size] = await Promise.all([
    readAssetMetadata({
      userId: ctx.user.id,
      assetId: params.assetId,
    }),
    getAssetSize({
      userId: ctx.user.id,
      assetId: params.assetId,
    }),
  ]);

  // Handle range requests
  const range = request.headers.get("Range");
  if (range) {
    const parts = range.replace(/bytes=/, "").split("-");
    const start = parseInt(parts[0], 10);
    const end = parts[1] ? parseInt(parts[1], 10) : size - 1;

    return new Response(
      createAssetReadStream({
        userId: ctx.user.id,
        assetId: params.assetId,
        start,
        end,
      }),
      {
        status: 206,
        headers: {
          "Content-Range": `bytes ${start}-${end}/${size}`,
          "Accept-Ranges": "bytes",
          "Content-Length": (end - start + 1).toString(),
          "Content-type": metadata.contentType,
        },
      },
    );
  }

  // Handle full downloads
  return new Response(
    createAssetReadStream({
      userId: ctx.user.id,
      assetId: params.assetId,
    }),
    {
      status: 200,
      headers: {
        "Content-Length": size.toString(),
        "Content-type": metadata.contentType,
      },
    },
  );
}
```

## Usage Examples

### Full Download
```bash
curl http://localhost:3000/api/assets/123 \
  -H "Authorization: Bearer YOUR_TOKEN"
```

### Partial Content (Range Request)
```bash
curl http://localhost:3000/api/assets/123 \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Range: bytes=0-1023"  # First 1KB
```

## Notes
- Supports streaming through range requests
- Uses database to validate asset ownership
- Reads metadata and size concurrently for efficiency
- Returns appropriate status codes for different scenarios
- Handles both full downloads and partial content requests
- Asset streams are created using createAssetReadStream utility
- Content-Type is determined from stored metadata
- Force-dynamic ensures fresh data on each request