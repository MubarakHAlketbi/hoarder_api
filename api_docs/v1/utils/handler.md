# Request Handler Utility

## Overview
The `buildHandler` utility provides a standardized way to handle HTTP requests in the Hoarder API. It implements consistent error handling, request validation, and response formatting while integrating with tRPC for type-safe API operations.

## Core Features

### 1. Request Processing
- Validates request parameters and body against Zod schemas
- Handles JSON parsing and content-type validation
- Creates tRPC context and client for each request
- Provides type-safe access to request data

### 2. Error Handling
- Handles different types of errors with appropriate HTTP status codes:
  - Zod validation errors (400 Bad Request)
  - tRPC errors (mapped to appropriate HTTP codes)
  - JSON parsing errors (400 Bad Request)
  - Unexpected errors (500 Internal Server Error)

### 3. Type Safety
- Fully typed request handling with TypeScript
- Generic type parameters for search params and body schemas
- Type inference for request handlers

## Technical Details

### Type Definitions

```typescript
interface ErrorMessage {
  path: (string | number)[];
  message: string;
}

interface TrpcAPIRequest<SearchParamsT, BodyType> {
  ctx: Context;
  api: ReturnType<typeof createTrcpClientFromCtx>;
  searchParams: SearchParamsT extends z.ZodTypeAny
    ? z.infer<SearchParamsT>
    : undefined;
  body: BodyType extends z.ZodTypeAny
    ? z.infer<BodyType> | undefined
    : undefined;
}
```

### TRPC Error Code Mapping
```typescript
function trpcCodeToHttpCode(code: TRPCError["code"]) {
  switch (code) {
    case "BAD_REQUEST":
    case "PARSE_ERROR":
      return 400;
    case "UNAUTHORIZED":
      return 401;
    case "FORBIDDEN":
      return 403;
    case "NOT_FOUND":
      return 404;
    case "METHOD_NOT_SUPPORTED":
      return 405;
    case "TIMEOUT":
      return 408;
    case "PAYLOAD_TOO_LARGE":
      return 413;
    case "INTERNAL_SERVER_ERROR":
      return 500;
    default:
      return 500;
  }
}
```

### Implementation Details

#### Handler Configuration
```typescript
buildHandler({
  req: NextRequest;
  handler: (req: InputT) => Promise<{ status: number; resp?: object }>;
  searchParamsSchema?: SearchParamsT;
  bodySchema?: BodyT;
})
```

#### Error Response Format
- Zod Validation Errors:
  ```json
  {
    "code": "ParseError",
    "message": "field.path: error message"
  }
  ```
- TRPC Errors:
  ```json
  {
    "code": "ERROR_CODE",
    "error": "Error message"
  }
  ```
- Unexpected Errors:
  ```json
  {
    "code": "UnknownError"
  }
  ```

## Usage Example

```typescript
export const POST = (req: NextRequest) =>
  buildHandler({
    req,
    bodySchema: z.object({
      title: z.string(),
      content: z.string()
    }),
    searchParamsSchema: z.object({
      draft: z.boolean().optional()
    }),
    handler: async ({ api, body, searchParams }) => {
      const result = await api.someEndpoint.create({
        ...body,
        isDraft: searchParams?.draft
      });
      return { status: 201, resp: result };
    },
  });
```

## Error Handling Flow
1. Request received
2. Content-type checked for POST/PUT requests
3. JSON body parsed if present
4. Search params and body validated against schemas
5. Handler executed within try-catch
6. Errors caught and formatted:
   - ZodError → 400 with formatted validation errors
   - TRPCError → Mapped HTTP status with error message
   - Other errors → 500 with UnknownError code

## Notes
- Always use Zod schemas for request validation
- Handler responses should include status code and optional response object
- All responses are automatically formatted as JSON
- Unexpected errors are logged but not exposed to clients
- TRPC errors are automatically mapped to appropriate HTTP status codes