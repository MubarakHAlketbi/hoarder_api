# Tags Endpoint

## Overview
This endpoint provides functionality to retrieve all tags in the system. Tags are used to categorize and organize bookmarks.

## Technical Details

### Endpoint Information
- **URL**: `/api/v1/tags`
- **Method**: GET
- **Authentication**: Required
- **Dynamic**: Yes (force-dynamic enabled)

## Operation

### List All Tags (GET)
Retrieves all tags for the authenticated user.

#### Response
- **Status**: 200 OK
- **Body**: Array of tag objects
```typescript
{
  // Array of tags
  tags: {
    id: string;
    name: string;
    // Other tag properties
  }[]
}
```

### Implementation
```typescript
import { NextRequest } from "next/server";
import { buildHandler } from "../utils/handler";

export const dynamic = "force-dynamic";

export const GET = (req: NextRequest) =>
  buildHandler({
    req,
    handler: async ({ api }) => {
      const tags = await api.tags.list();
      return { status: 200, resp: tags };
    },
  });
```

## Usage Example

### Get All Tags
```bash
curl http://localhost:3000/api/v1/tags \
  -H "Authorization: Bearer YOUR_TOKEN"
```

## Notes
- Uses the common buildHandler utility for consistent error handling
- No pagination implemented for tag listing
- Force-dynamic ensures fresh data on each request
- Tags are user-specific through authentication
- Simple endpoint focused solely on tag retrieval
- Tags can be used for bookmark organization
- Returns all tags without filtering options