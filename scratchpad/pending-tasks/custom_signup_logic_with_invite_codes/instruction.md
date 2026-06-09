You are building an exclusive SaaS application where new registrations must be gated by a specific invitation code during the authentication flow.

You need to implement the `userSignupFields` configuration in the Wasp authentication setup to capture an "Invite Code" from the user. Write the corresponding validation logic in the server-side auth hook that rejects the signup if the code provided is not exactly "WASP2026".

**Constraints:**
- The custom field captured from the frontend must be named `inviteCode`.
- You must throw a properly formatted Wasp `HttpError` with a 400 status if the validation fails.
- Do NOT modify the client-side login form component.