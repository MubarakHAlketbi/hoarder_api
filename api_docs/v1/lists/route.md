# Lists Endpoint

## Overview
This endpoint manages bookmark lists, providing functionality to retrieve all lists and create new ones. Lists allow users to organize their bookmarks into collections.

## Technical Details

### Endpoint Information
- **URL**: `/api/v1/lists`
- **Methods**: GET, POST
- **Authentication**: Required
- **Dynamic**: Yes (force-dynamic enabled)

## Operations

### 1. List All Lists (GET)
Retrieves all bookmark lists for the authenticated user.

#### Response
- **Status**: 200 OK
- **Body**: Array of list objects
```typescript
{
  // Array of bookmark lists
  lists: {
    id: string;
    // Other list properties
  }[]
}
```

### 2. Create List (POST)
Creates a new bookmark list.

#### Request Body
Uses `zNewBookmarkListSchema` from `@hoarder/shared/types/lists` for validation.
```typescript
{
  // List properties as defined in zNewBookmarkListSchema
  name: string;
  // Other required properties
}
```

#### Response
- **Status**: 201 Created
- **Body**: The created list object

### Implementation
```typescript
import { NextRequest } from "next/server";
import { zNewBookmarkListSchema } from "@hoarder/shared/types/lists";
import { buildHandler } from "../utils/handler";

export const dynamic = "force-dynamic";

export const GET = (req: NextRequest) =>
  buildHandler({
    req,
    handler: async ({ api }) => {
      const lists = await api.lists.list();
      return { status: 200, resp: lists };
    },
  });

export const POST = (req: NextRequest) =>
  buildHandler({
    req,
    bodySchema: zNewBookmarkListSchema,
    handler: async ({ api, body }) => {
      const list = await api.lists.create(body!);
      return { status: 201, resp: list };
    },
  });
```

## Usage Examples

### Get All Lists
```bash
curl http://localhost:3000/api/v1/lists \
  -H "Authorization: Bearer YOUR_TOKEN"
```

### Create New List
```bash
curl -X POST http://localhost:3000/api/v1/lists \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Reading List",
    // Other required properties according to zNewBookmarkListSchema
  }'
```

## Notes
- Uses the common buildHandler utility for consistent error handling
- Validates new list creation using shared schema
- Returns 201 status for successful list creation
- Force-dynamic ensures fresh data on each request
- Lists are user-specific through authentication
- No pagination implemented for list retrieval
- List creation requires all fields specified in zNewBookmarkListSchema