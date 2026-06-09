To improve user engagement, the application needs to automatically send out a weekly summary email to all registered users.

You need to configure a recurring background job in the Wasp configuration file that runs every Monday at 9:00 AM. Implement the job's worker function using Wasp's PgBoss queue system to query all users from the database and mock an email sending process by logging a message to the console for each user.

**Constraints:**
- Use standard cron syntax (`0 9 * * 1`) for the job schedule in the Wasp config.
- The background job must be properly registered within the `app` declaration block.
- Do NOT integrate a real email provider; strictly use `console.log`.