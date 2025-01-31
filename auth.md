# Hoarder API Authentication

This document describes the authentication setup for the Hoarder API.

## Authentication Handler

The authentication logic is handled by the `authHandler` function, which is defined in `apps/web/server/auth.ts`. This function is used as both the `GET` and `POST` handler for the `/api/auth/[...nextauth]` route, as configured in `code/auth/[...nextauth]/route.tsx`.

## NextAuth.js

The API uses NextAuth.js for authentication. The `authOptions` object configures NextAuth.js with the following:

-   **Adapter:** `DrizzleAdapter` is used to connect to the database.
-   **Providers:**
    -   `CredentialsProvider`: Allows users to sign in with an email and password.
    -   `OAuth Provider`: Allows users to sign in with an OAuth provider (if configured in `serverConfig.auth.oauth`).
-   **Session Strategy:** `jwt` is used for session management.
-   **Pages:** Custom sign-in, sign-out, error, and new user pages are configured.
-   **Callbacks:**
    -   `signIn`: Checks if the user is allowed to sign in. If it's a new user and signups are disabled, the sign-in is rejected.
    -   `jwt`: Adds user information (ID, name, email, image, role) to the JWT.
    -   `session`: Adds user information to the session object.

## Credentials Provider

The `CredentialsProvider` is configured to:

-   Accept an email and password.
-   Use the `validatePassword` function to authenticate the user.
-   Log authentication errors using `logAuthenticationError`.

## OAuth Provider

If configured in `serverConfig.auth.oauth`, an OAuth provider is added. It uses the provided `wellKnownUrl`, `clientId`, `clientSecret`, and other settings. The `profile` function maps the OAuth profile to a user object, setting the role to "admin" if the user is the first user or if the user's email is configured as an admin.

## User Roles

The API supports two user roles:

-   `admin`: Users with administrative privileges.
-   `user`: Regular users.

The first user to sign up is automatically assigned the "admin" role. Additionally, users with emails configured as admins in the database will also have the "admin" role.

## Database

The API uses a database to store user information, accounts, sessions, and verification tokens. The `DrizzleAdapter` connects NextAuth.js to the database using the `db` object from `@hoarder/db`.

## Server Config

The `serverConfig` object from `@hoarder/shared/config` is used to configure various aspects of authentication, such as demo mode, signups, and OAuth settings.