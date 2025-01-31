# Hoarder API Health Check

This document describes the health check endpoint for the Hoarder API, located in the `code/health` directory.

## `/api/health`

### `GET /api/health`

Checks the health status of the API.

**Response:**

-   `status`: 200
-   `resp`: A JSON object with a `status` field set to "ok" and a `message` field indicating that the web app is working.

**Example:**

```
GET /api/health
```

```json
{
  "status": "ok",
  "message": "Web app is working"
}