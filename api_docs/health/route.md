# Health Check Endpoint

## Overview
This endpoint provides a simple health check mechanism to verify if the API is operational. It's a lightweight endpoint that returns a basic status response indicating the web application is functioning correctly.

## Technical Details

### Endpoint Information
- **URL**: `/api/health`
- **Method**: GET
- **Authentication**: Not required

### Implementation
```typescript
import { NextRequest, NextResponse } from "next/server";

export const GET = async (_req: NextRequest) => {
  return NextResponse.json({
    status: "ok",
    message: "Web app is working",
  });
};
```

### Code Breakdown
- Uses Next.js server components with `NextRequest` and `NextResponse`
- Implements a GET handler that returns a JSON response
- The underscore prefix in `_req` indicates the request parameter is not used
- Returns a simple JSON object with status and message

### Response Format
```json
{
  "status": "ok",
  "message": "Web app is working"
}
```

## Usage Example
```bash
curl http://localhost:3000/api/health
```

## Notes
- This endpoint is useful for:
  - Load balancer health checks
  - Monitoring systems
  - Verifying API availability
- Returns HTTP 200 status code when successful