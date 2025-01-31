# Hoarder API Lists Endpoints

This document describes the API endpoints for managing lists in the Hoarder API, located in the `code/v1/lists` directory.

## `/v1/lists`

### `GET /v1/lists`

Retrieves a list of all lists.

**Response:**

-   `status`: 200
-   `resp`: An array of list objects.

**Example:**

```
GET /v1/lists
```

```json
[
  {
    "listId": "l1",
    "name": "My List",
    "createdAt": "2023-10-27T16:00:00Z"
  }
]
```

### `POST /v1/lists`

Creates a new list.

**Request Body:**

Conforms to the `zNewBookmarkListSchema` schema.

**Response:**

-   `status`: 201
-   `resp`: The created list object.

**Example:**

```
POST /v1/lists
```

```json
{
  "name": "My New List"
}
```

```json
{
  "listId": "l2",
  "name": "My New List",
  "createdAt": "2023-10-27T17:00:00Z"
}
```

## `/v1/lists/[listId]`

### `GET /v1/lists/[listId]`

Retrieves a specific list by its ID.

**Parameters:**

-   `listId`: The ID of the list.

**Response:**

-   `status`: 200
-   `resp`: The list object.

**Example:**

```
GET /v1/lists/l1
```

```json
{
  "listId": "l1",
  "name": "My List",
  "createdAt": "2023-10-27T16:00:00Z"
}
```

### `PATCH /v1/lists/[listId]`

Updates a specific list.

**Parameters:**

-   `listId`: The ID of the list.

**Request Body:**

Conforms to the `zEditBookmarkListSchema` schema, excluding the `listId` field.

**Response:**

-   `status`: 200
-   `resp`: The updated list object.

**Example:**

```
PATCH /v1/lists/l1
```

```json
{
  "name": "My Updated List"
}
```

```json
{
  "listId": "l1",
  "name": "My Updated List",
  "createdAt": "2023-10-27T16:00:00Z"
}
```

### `DELETE /v1/lists/[listId]`

Deletes a specific list.

**Parameters:**

-   `listId`: The ID of the list.

**Response:**

-   `status`: 204

**Example:**

```
DELETE /v1/lists/l1
```

## `/v1/lists/[listId]/bookmarks`

### `GET /v1/lists/[listId]/bookmarks`

Retrieves the bookmarks that belong to a specific list.

**Parameters:**

-   `listId`: The ID of the list.
-   `limit` (optional): A number representing the maximum number of bookmarks to return.
-   `cursor` (optional): A string representing the cursor for pagination.

**Response:**

-   `status`: 200
-   `resp`: An object containing an array of bookmarks and a `nextCursor` for pagination.

**Example:**

```
GET /v1/lists/l1/bookmarks?limit=10
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

## `/v1/lists/[listId]/bookmarks/[bookmarkId]`

### `PUT /v1/lists/[listId]/bookmarks/[bookmarkId]`

Adds a specific bookmark to a specific list.

**Parameters:**

-   `listId`: The ID of the list.
-   `bookmarkId`: The ID of the bookmark.

**Response:**

-   `status`: 204

**Example:**

```
PUT /v1/lists/l1/bookmarks/1
```

### `DELETE /v1/lists/[listId]/bookmarks/[bookmarkId]`

Removes a specific bookmark from a specific list.

**Parameters:**

-   `listId`: The ID of the list.
-   `bookmarkId`: The ID of the bookmark.

**Response:**

-   `status`: 204

**Example:**

```
DELETE /v1/lists/l1/bookmarks/1