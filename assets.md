# Hoarder API Assets Management

This document describes the API endpoints for managing assets in the Hoarder API, located in the `code/assets` directory.

## Asset Upload

### `uploadFromPostData(user, db, formData)`

Handles the upload of an asset from form data.

**Parameters:**

-   `user`: The authenticated user object.
-   `db`: The database connection object.
-   `formData`: The form data containing the asset file.

**Returns:**

-   An object containing an error message and status code if the upload fails.
-   An object containing the `assetId`, `contentType`, `fileName`, and `size` if the upload succeeds.

**Functionality:**

1. Checks if the uploaded data is a `File` instance.
2. Validates the content type against a list of supported types.
3. Checks if the file size exceeds the maximum allowed size.
4. Reads the file content into a buffer.
5. Inserts a new record into the `assets` table in the database with initial values (e.g., `assetType` set to `UNKNOWN`, `bookmarkId` set to `null`).
6. Saves the asset file using the `saveAsset` function.
7. Returns the asset ID, content type, size, and file name.

### `POST /api/assets`

Handles asset upload requests.

**Request Body:**

-   Form data containing the asset file (either as `file` or `image`).

**Response:**

-   `status`: 401 if the user is not authenticated.
-   `status`: 403 if the application is in demo mode.
-   `status`: 400 if the request is invalid or the asset type is unsupported.
-   `status`: 413 if the asset size exceeds the maximum allowed size.
-   `status`: 200 if the upload is successful.
-   `resp`: An object containing the `assetId`, `contentType`, `size`, and `fileName` if the upload is successful.

**Example:**

```
POST /api/assets
```

```
------WebKitFormBoundaryExample
Content-Disposition: form-data; name="file"; filename="image.jpg"
Content-Type: image/jpeg

[...image data...]
------WebKitFormBoundaryExample--
```

```json
{
  "assetId": "a1",
  "contentType": "image/jpeg",
  "size": 12345,
  "fileName": "image.jpg"
}
```

## Asset Retrieval

### `GET /api/assets/[assetId]`

Retrieves a specific asset by its ID.

**Parameters:**

-   `assetId`: The ID of the asset.

**Request Headers:**

-   `Range` (optional): Specifies the byte range of the asset to retrieve (e.g., `bytes=0-999`).

**Response:**

-   `status`: 401 if the user is not authenticated.
-   `status`: 404 if the asset is not found.
-   `status`: 200 if the request is successful and the `Range` header is not specified.
-   `status`: 206 if the request is successful and the `Range` header is specified (partial content).
-   `resp`: The asset data as a stream.

**Headers (200 OK):**

-   `Content-Length`: The size of the asset in bytes.
-   `Content-type`: The content type of the asset.

**Headers (206 Partial Content):**

-   `Content-Range`: Specifies the byte range of the asset being returned (e.g., `bytes 0-999/12345`).
-   `Accept-Ranges`: Indicates that the server supports range requests (`bytes`).
-   `Content-Length`: The size of the partial content being returned.
-   `Content-type`: The content type of the asset.

**Example (Full Asset):**

```
GET /api/assets/a1
```

**Example (Partial Content):**

```
GET /api/assets/a1
Range: bytes=0-999