# Hoarder API Bookmarks Endpoints

This document describes the API endpoints for managing bookmarks in the Hoarder API, located in the `code/v1/bookmarks` directory.

## `/v1/bookmarks`

### `GET /v1/bookmarks`

Retrieves a list of bookmarks.

**Request Parameters:**

-   `favourited` (optional): A boolean string (`"true"` or `"false"`) indicating whether to filter by favourited bookmarks.
-   `archived` (optional): A boolean string (`"true"` or `"false"`) indicating whether to filter by archived bookmarks.
-   `limit` (optional): A number representing the maximum number of bookmarks to return.
-   `cursor` (optional): A string representing the cursor for pagination.

**Response:**

-   `status`: 200
-   `resp`: An object containing an array of bookmarks and a `nextCursor` for pagination.

**Example:**

```
GET /v1/bookmarks?favourited=true&limit=10
```

```json
{
  "bookmarks": [
    {
      "bookmarkId": "1",
      "url": "https://example.com",
      "title": "Example Domain",
      "favourited": true,
      "archived": false,
      "createdAt": "2023-10-27T12:00:00Z"
    }
  ],
  "nextCursor": "2_2023-10-27T11:00:00Z"
}
```

### `POST /v1/bookmarks`

Creates a new bookmark.

**Request Body:**

Conforms to the `zNewBookmarkRequestSchema` schema.

**Response:**

-   `status`: 201
-   `resp`: The created bookmark object.

**Example:**

```
POST /v1/bookmarks
```

```json
{
  "url": "https://example.com",
  "title": "Example Domain"
}
```

```json
{
  "bookmarkId": "2",
  "url": "https://example.com",
  "title": "Example Domain",
  "favourited": false,
  "archived": false,
  "createdAt": "2023-10-27T13:00:00Z"
}
```

## `/v1/bookmarks/[bookmarkId]`

### `GET /v1/bookmarks/[bookmarkId]`

Retrieves a specific bookmark by its ID.

**Parameters:**

-   `bookmarkId`: The ID of the bookmark.

**Response:**

-   `status`: 200
-   `resp`: The bookmark object.

**Example:**

```
GET /v1/bookmarks/1
```

```json
{
  "bookmarkId": "1",
  "url": "https://example.com",
  "title": "Example Domain",
  "favourited": true,
  "archived": false,
  "createdAt": "2023-10-27T12:00:00Z"
}
```

### `PATCH /v1/bookmarks/[bookmarkId]`

Updates a specific bookmark.

**Parameters:**

-   `bookmarkId`: The ID of the bookmark.

**Request Body:**

Conforms to the `zUpdateBookmarksRequestSchema` schema, excluding the `bookmarkId` field.

**Response:**

-   `status`: 200
-   `resp`: The updated bookmark object.

**Example:**

```
PATCH /v1/bookmarks/1
```

```json
{
  "title": "New Title",
  "favourited": false
}
```

```json
{
  "bookmarkId": "1",
  "url": "https://example.com",
  "title": "New Title",
  "favourited": false,
  "archived": false,
  "createdAt": "2023-10-27T12:00:00Z"
}
```

### `DELETE /v1/bookmarks/[bookmarkId]`

Deletes a specific bookmark.

**Parameters:**

-   `bookmarkId`: The ID of the bookmark.

**Response:**

-   `status`: 204

**Example:**

```
DELETE /v1/bookmarks/1
```

## `/v1/bookmarks/[bookmarkId]/assets`

### `GET /v1/bookmarks/[bookmarkId]/assets`

Retrieves the assets associated with a specific bookmark.

**Parameters:**

-   `bookmarkId`: The ID of the bookmark.

**Response:**

-   `status`: 200
-   `resp`: An object containing an array of assets.

**Example:**

```
GET /v1/bookmarks/1/assets
```

```json
{
  "assets": [
    {
      "assetId": "a1",
      "url": "https://example.com/asset1.jpg"
    }
  ]
}
```

### `POST /v1/bookmarks/[bookmarkId]/assets`

Attaches an asset to a specific bookmark.

**Parameters:**

-   `bookmarkId`: The ID of the bookmark.

**Request Body:**

Conforms to the `zAssetSchema` schema.

**Response:**

-   `status`: 201
-   `resp`: The attached asset object.

**Example:**

```
POST /v1/bookmarks/1/assets
```

```json
{
  "assetId": "a2",
  "url": "https://example.com/asset2.jpg"
}
```

```json
{
  "assetId": "a2",
  "url": "https://example.com/asset2.jpg"
}
```

## `/v1/bookmarks/[bookmarkId]/assets/[assetId]`

### `PUT /v1/bookmarks/[bookmarkId]/assets/[assetId]`

Replaces an asset associated with a bookmark.

**Parameters:**

-   `bookmarkId`: The ID of the bookmark.
-   `assetId`: The ID of the asset to be replaced.

**Request Body:**

-   `assetId`: The ID of the new asset.

**Response:**

-   `status`: 204

**Example:**

```
PUT /v1/bookmarks/1/assets/a1
```

```json
{
  "assetId": "a3"
}
```

### `DELETE /v1/bookmarks/[bookmarkId]/assets/[assetId]`

Detaches an asset from a bookmark.

**Parameters:**

-   `bookmarkId`: The ID of the bookmark.
-   `assetId`: The ID of the asset to be detached.

**Response:**

-   `status`: 204

**Example:**

```
DELETE /v1/bookmarks/1/assets/a1
```

## `/v1/bookmarks/[bookmarkId]/highlights`

### `GET /v1/bookmarks/[bookmarkId]/highlights`

Retrieves the highlights for a specific bookmark.

**Parameters:**

-   `bookmarkId`: The ID of the bookmark.

**Response:**

-   `status`: 200
-   `resp`: An array of highlight objects.

**Example:**

```
GET /v1/bookmarks/1/highlights
```

```json
[
  {
    "highlightId": "h1",
    "text": "This is a highlight",
    "createdAt": "2023-10-27T14:00:00Z"
  }
]
```

## `/v1/bookmarks/[bookmarkId]/lists`

### `GET /v1/bookmarks/[bookmarkId]/lists`

Retrieves the lists that a specific bookmark belongs to.

**Parameters:**

-   `bookmarkId`: The ID of the bookmark.

**Response:**

-   `status`: 200
-   `resp`: An array of list objects.

**Example:**

```
GET /v1/bookmarks/1/lists
```

```json
[
  {
    "listId": "l1",
    "name": "My List"
  }
]
```

## `/v1/bookmarks/[bookmarkId]/tags`

### `POST /v1/bookmarks/[bookmarkId]/tags`

Attaches tags to a specific bookmark.

**Parameters:**

-   `bookmarkId`: The ID of the bookmark.

**Request Body:**

-   `tags`: An array of tags conforming to the `zManipulatedTagSchema` schema.

**Response:**

-   `status`: 200
-   `resp`: An object containing an array of attached tags.

**Example:**

```
POST /v1/bookmarks/1/tags
```

```json
{
  "tags": [
    {
      "tagId": "t1",
      "name": "tag1"
    },
    {
      "tagId": "t2",
      "name": "tag2"
    }
  ]
}
```

```json
{
  "attached": [
    {
      "tagId": "t1",
      "name": "tag1"
    },
    {
      "tagId": "t2",
      "name": "tag2"
    }
  ]
}
```

### `DELETE /v1/bookmarks/[bookmarkId]/tags`

Detaches tags from a specific bookmark.

**Parameters:**

-   `bookmarkId`: The ID of the bookmark.

**Request Body:**

-   `tags`: An array of tags conforming to the `zManipulatedTagSchema` schema.

**Response:**

-   `status`: 200
-   `resp`: An object containing an array of detached tags.

**Example:**

```
DELETE /v1/bookmarks/1/tags
```

```json
{
  "tags": [
    {
      "tagId": "t1",
      "name": "tag1"
    }
  ]
}
```

```json
{
  "detached": [
    {
      "tagId": "t1",
      "name": "tag1"
    }
  ]
}
```

## `/v1/bookmarks/search`

### `GET /v1/bookmarks/search`

Searches for bookmarks based on a query string.

**Request Parameters:**

-   `q`: The search query string.
-   `limit` (optional): The maximum number of results to return.
-   `cursor` (optional): A cursor for pagination.

**Response:**

-   `status`: 200
-   `resp`: An object containing an array of bookmarks and a `nextCursor` for pagination.

**Example:**

```
GET /v1/bookmarks/search?q=example&limit=5
```

```json
{
  "bookmarks": [
    {
      "bookmarkId": "1",
      "url": "https://example.com",
      "title": "Example Domain",
      "favourited": true,
      "archived": false,
      "createdAt": "2023-10-27T12:00:00Z"
    }
  ],
  "nextCursor": "5"
}
```

## `/v1/bookmarks/singlefile`

### `POST /v1/bookmarks/singlefile`

Creates a bookmark from SingleFile data.

**Request Body:**

-   Form data containing the SingleFile data and the URL of the bookmark.

**Response:**

-   `status`: 201
-   `resp`: The created bookmark object.

**Example:**

```
POST /v1/bookmarks/singlefile
```

```
------WebKitFormBoundaryExample
Content-Disposition: form-data; name="file"; filename="singlefile.html"
Content-Type: text/html

<!DOCTYPE html>
<html>
<head>
  <title>SingleFile Data</title>
</head>
<body>
  <h1>SingleFile Content</h1>
</body>
</html>
------WebKitFormBoundaryExample
Content-Disposition: form-data; name="url"

https://example.com
------WebKitFormBoundaryExample--
```

```json
{
  "bookmarkId": "3",
  "url": "https://example.com",
  "title": "Example Domain",
  "favourited": false,
  "archived": false,
  "createdAt": "2023-10-27T15:00:00Z",
  "type": "LINK",
  "precrawledArchiveId": "a4"
}