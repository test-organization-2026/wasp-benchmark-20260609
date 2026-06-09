Wasp does not currently provide a native primitive for multi-part file handling, so raw Express middleware must be leveraged to accept user uploads.

You need to configure a custom Express middleware in Wasp using the `multer` package to intercept and save uploaded files. Register this middleware in your Wasp configuration to apply exclusively to a custom API route path `/api/upload`.

**Constraints:**
- Do NOT configure external cloud storage (e.g., AWS S3); configure Multer to store files locally in an `/uploads` directory.
- The middleware injection must strictly follow Wasp's custom middleware array configuration syntax.
- The API route handler must return the saved file's path in a JSON response.