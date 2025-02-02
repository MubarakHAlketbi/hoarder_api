# Assets Upload Endpoint

## Overview
This endpoint handles file uploads for the Hoarder API, allowing users to upload assets that can be associated with bookmarks. It includes validation for file types and sizes, and handles both database and file storage operations.

## Technical Details

### Endpoint Information
- **URL**: `/api/assets`
- **Method**: POST
- **Authentication**: Required
- **Content-Type**: multipart/form-data
- **Dynamic**: Yes (force-dynamic enabled)

### Configuration
```typescript
const MAX_UPLOAD_SIZE_BYTES = serverConfig.maxAssetSizeMb * 1024 * 1024;
```

### Request Format
Multipart form data with either:
- `file` field containing the file to upload
- `image` field containing the image to upload

### Response Format
#### Success (200 OK)
```typescript
interface UploadResponse {
  assetId: string;      // Unique identifier for the uploaded asset
  contentType: string;  // MIME type of the uploaded file
  size: number;        // Size in bytes
  fileName: string;    // Original file name
}
```

#### Error Responses
- 400 Bad Request
  ```json
  { "error": "Unsupported asset type" }
  // or
  { "error": "Bad request" }
  ```
- 401 Unauthorized
  ```json
  { "error": "Unauthorized" }
  ```
- 413 Payload Too Large
  ```json
  { "error": "Asset is too big" }
  ```
- 403 Forbidden (Demo Mode)
  ```json
  { "error": "Mutations are not allowed in demo mode" }
  ```

## Implementation Details

### Upload Process
1. Authentication check
2. Demo mode check
3. Form data extraction
4. File validation
   - Type checking against SUPPORTED_UPLOAD_ASSET_TYPES
   - Size checking against MAX_UPLOAD_SIZE_BYTES
5. Database entry creation
6. File storage
7. Response formatting

### Database Schema
```typescript
{
  id: string;           // Generated asset ID
  assetType: string;    // Initially set to UNKNOWN
  bookmarkId: null;     // Initially null until associated
  userId: string;       // Owner of the asset
  contentType: string;  // MIME type
  size: number;        // File size in bytes
  fileName: string;    // Original file name
}
```

### Implementation
```typescript
export async function uploadFromPostData(
  user: AuthedContext["user"],
  db: AuthedContext["db"],
  formData: FormData,
): Promise<
  | { error: string; status: number }
  | {
      assetId: string;
      contentType: string;
      fileName: string;
      size: number;
    }
> {
  const data = formData.get("file") ?? formData.get("image");
  // ... validation and processing
  // ... database and storage operations
  return {
    assetId: assetDb.id,
    contentType,
    size: buffer.byteLength,
    fileName,
  };
}

export async function POST(request: Request) {
  const ctx = await createContextFromRequest(request);
  // ... authentication and demo mode checks
  const formData = await request.formData();
  const resp = await uploadFromPostData(ctx.user, ctx.db, formData);
  // ... response handling
}
```

## Usage Example

```bash
curl -X POST http://localhost:3000/api/assets \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -F "file=@/path/to/file.pdf"
```

## Notes
- Assets are initially uploaded with type UNKNOWN
- Assets are not immediately associated with bookmarks
- File size limit is configured in serverConfig.maxAssetSizeMb
- Only supported file types can be uploaded
- Demo mode prevents any uploads
- Files are stored using the saveAsset utility
- Database entry is created before file storage
- Supports both 'file' and 'image' form field names
- Response type is validated against ZUploadResponse