Building a project management application in Wasp requires setting up related database models and fetching them efficiently through Wasp's RPC operations.

You need to define a `Project` and `Task` entity in `schema.prisma` with a one-to-many relationship (a Project has many Tasks). Then, register a Query in the Wasp configuration and implement the server-side TypeScript function to fetch all Projects along with their nested Tasks. 

**Constraints:**
- Do NOT modify the existing `User` entity or authentication settings.
- You must use the `wasp/server/operations` import for the query types in your TypeScript implementation.
- The Prisma query must explicitly `include` the nested tasks.