Your Wasp application needs administrative features, requiring you to enforce role-based access control (RBAC) securely on the backend.

You need to add a `role` field (type String, default "user") to the `User` entity in Prisma. Then, implement a server-side Wasp Action called `deleteUser` that verifies if the calling user's role is "admin" before proceeding with the database deletion.

**Constraints:**
- You must throw an `HttpError(403)` if the requesting user is unauthenticated or does not have the "admin" role.
- Ensure the user ID to be deleted is passed as an argument to the Action.
- The action must return a success boolean upon successful deletion.