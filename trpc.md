# Hoarder API tRPC Setup

This document describes the tRPC setup for the Hoarder API, located in the `code/trpc` directory.

## `/api/trpc/[trpc]/route.ts`

This file sets up the tRPC handler using `fetchRequestHandler` from `@trpc/server/adapters/fetch`.

### `handler(req: Request)`

Handles tRPC requests.

**Parameters:**

-   `req`: The incoming request object.

**Functionality:**

1. Sets the tRPC endpoint to `/api/trpc`.
2. Uses the `appRouter` as the tRPC router.
3. Logs errors to the console, including more detailed logging in development mode.
4. Creates the context for each request using `createContextFromRequest`.

**Exports:**

-   `GET`: The `handler` function, used to handle `GET` requests.
-   `POST`: The `handler` function, used to handle `POST` requests.

**Example:**

```typescript
import { fetchRequestHandler } from "@trpc/server/adapters/fetch";
import { appRouter } from "@hoarder/trpc/routers/_app";
import { createContextFromRequest } from "@/server/api/client";

const handler = (req: Request) =>
  fetchRequestHandler({
    endpoint: "/api/trpc",
    req,
    router: appRouter,
    onError: ({ path, error }) => {
      if (process.env.NODE_ENV === "development") {
        console.error(`❌ tRPC failed on ${path}`);
      }
      console.error(error);
    },
    createContext: async (opts) => {
      return await createContextFromRequest(opts.req);
    },
  });

export { handler as GET, handler as POST };