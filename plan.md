# Evaluation Dataset Research: Wasp (wasp-lang)

Wasp is a declarative, full-stack web framework that uses a domain-specific language (DSL) to orchestrate React, Node.js, and Prisma. It aims to reduce boilerplate by handling the "glue" between the frontend, backend, and database automatically.

### 1. Library Overview

*   **Description**: Wasp is a "batteries-included" framework where you describe your app's high-level structure (auth, routes, entities, jobs) in a `.wasp` file, and write the business logic in standard TypeScript/JavaScript.
*   **Ecosystem Role**: It sits between a meta-framework like Next.js and a low-level stack. It is particularly popular for SaaS development (via the "Open SaaS" template) because it provides built-in auth, cron jobs, and email integration.
*   **Project Setup**:
    1.  **Install CLI**: `npm i -g @wasp.sh/wasp-cli@latest`
    2.  **Initialize**: `wasp new <project-name>` (choose a template like `todo-ts` or `saas`)
    3.  **Start Dev Server**: `cd <project-name> && wasp start`
    4.  **Database Migration**: `wasp db migrate-dev` (syncs `schema.prisma` with the DB)

### 2. Core Primitives & APIs

*   **The `.wasp` Config File**: The central manifest for the app.
    ```wasp
    app MySaaS {
      wasp: { version: "^0.23.0" },
      title: "My App",
      auth: {
        userEntity: User,
        methods: { usernameAndPassword: {} },
        onAuthFailedRedirectTo: "/login"
      }
    }

    route RootRoute { path: "/", to: MainPage }
    page MainPage { component: import { Main } from "@src/MainPage" }

    query getTasks {
      fn: import { getTasks } from "@src/queries",
      entities: [Task]
    }
    ```
*   **Operations (Queries & Actions)**: RPC-style communication.
    *   **Server Implementation**:
        ```ts
        import { type GetTasks } from 'wasp/server/operations'
        export const getTasks: GetTasks<void, Task[]> = async (args, context) => {
          return context.entities.Task.findMany()
        }
        ```
    *   **Client Usage**:
        ```tsx
        import { useQuery, getTasks } from 'wasp/client/operations'
        const { data: tasks } = useQuery(getTasks)
        ```
*   **Entities (Prisma)**: Defined in `schema.prisma` and automatically injected into operation contexts.
*   **Auth Hooks**: Custom logic for post-signup or post-login actions.
*   **Jobs**: Background tasks using `PgBoss` (PostgreSQL-based queue).

### 3. Real-World Use Cases & Templates

*   **Open SaaS**: The flagship template ([opensaas.sh](https://opensaas.sh)) including Stripe, AWS S3, and OpenAI integrations.
*   **Dashboards**: Leveraging `useQuery` for real-time data fetching and built-in auth for protected routes.
*   **AI Wrappers**: Often used for GPT-based apps because of the easy background job handling for long-running AI tasks.

### 4. Developer Friction Points

*   **DSL Learning Curve**: Developers must learn the `.wasp` syntax for configuration, which can feel like a "black box" compared to pure TS frameworks. [Source](https://wasp.sh/blog/2026/05/13/new-language-for-web-dev-was-a-mistake)
*   **Deployment Constraints**: Wasp requires a persistent Node.js server; it does not natively support Vercel's serverless model. [Source](https://starterpick.com/guides/wasp-review-2026)
*   **Middleware Customization**: Adding custom Express middleware requires specific Wasp configuration and doesn't follow the standard `app.use()` pattern found in raw Express apps.
*   **File Uploads**: Requires manual setup of Multer middleware or S3 presigned URLs, as Wasp doesn't have a "primitive" for file handling yet. [Source](https://docs.opensaas.sh/guides/file-uploading/)

### 5. Evaluation Ideas

*   **Auth Enhancement**: Add a "role" field to the `User` entity and implement a server-side check in a Wasp Action to restrict access.
*   **Custom Signup Logic**: Implement `userSignupFields` to capture and validate a user's "Invite Code" during registration.
*   **Background Jobs**: Set up a recurring cron job that sends a summary email to all users every Monday at 9 AM.
*   **File Upload Integration**: Configure a custom Express middleware in Wasp to handle multi-part form data via Multer.
*   **Complex Relationships**: Create a "Project" and "Task" relationship in `schema.prisma` and implement a Query that fetches Projects with their nested Tasks.
*   **TS Migration**: Convert an existing `.wasp` configuration file to the newer `main.wasp.ts` specification.
*   **External API Integration**: Implement an Action that calls an external API (e.g., Stripe) and updates a Wasp Entity based on the response.

### 6. Sources

1.  [Wasp Official Documentation](https://wasp.sh/docs) - Core framework documentation.
2.  [Wasp llms.txt](https://wasp.sh/llms.txt) - Structured overview for AI agents.
3.  [Open SaaS Documentation](https://docs.opensaas.sh/llms.txt) - Detailed guides for the SaaS starter kit.
4.  [Wasp Blog: 5 Years and $5M Later](https://wasp.sh/blog/2026/05/13/new-language-for-web-dev-was-a-mistake) - Insights into the transition from DSL to TS.
5.  [StarterPick Wasp Review](https://starterpick.com/guides/wasp-review-2026) - Independent review of limitations and friction points.
6.  [Wasp GitHub Repository](https://github.com/wasp-lang/wasp) - Source code and issue tracker.