The Wasp framework is transitioning away from its proprietary `.wasp` declarative language toward a pure TypeScript configuration file (`main.wasp.ts`).

You need to migrate a legacy `main.wasp` configuration—containing an `app` block with standard username/password auth, a `RootRoute` pointing to `/`, and a `MainPage` component—into the equivalent, modern `main.wasp.ts` specification. 

**Constraints:**
- The output must be valid TypeScript using standard objects and Wasp's new configuration exports.
- Do NOT change the names of the existing routes, pages, or the app's title during the migration.
- Ensure all component imports use the standard ES module syntax instead of the legacy DSL import syntax.