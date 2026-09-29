# Pre-Deployment Checklist

Before pushing a backend to production, a developer should check security, database integrity, logs, and environment settings. This checklist helps reduce the risk of broken deployments, exposed secrets, and avoidable downtime.

---

## 1. Security

### API keys and secrets

- [ ] Store all secrets in the hosting platform's environment variables.
- [ ] Add `.env` and `.env.*` to `.gitignore`.
- [ ] Never hard-code database URLs, API keys, JWT secrets, or payment credentials.
- [ ] Rotate any secret if it was accidentally committed or shared.
- [ ] Use different secrets for local development and production.

### CORS configuration

- [ ] Restrict CORS to the intended frontend origin instead of allowing all domains.
- [ ] Do not allow `*` in production unless the app is truly public and safe.
- [ ] Verify that the frontend domain is exactly the expected URL.
- [ ] Test that requests from unauthorized origins are rejected.

### Security headers and rate limiting

- [ ] Use `helmet` to set recommended HTTP security headers.
- [ ] Add rate limiting to public or login routes.
- [ ] Validate request data, headers, params, and query strings.
- [ ] Reject malformed or unexpected input before it reaches business logic.
- [ ] Use HTTPS only. Managed hosting providers usually provide this automatically.

### Authentication and authorization

- [ ] Hash passwords before storing them.
- [ ] Never store plain-text passwords or secrets in the database.
- [ ] Use proper authorization checks for protected endpoints.
- [ ] Enforce least-privilege access for database users.

---

## 2. Database management

### Production-safe schema changes

- [ ] Review schema changes before deployment.
- [ ] For Prisma, prefer `prisma migrate deploy` for production migrations.
- [ ] Do not use `prisma db push` as the main production migration strategy when a controlled migration history is needed.
- [ ] Run migrations in a safe environment and check rollback options.
- [ ] Ensure app and database versions remain compatible during rollout.

### Indexes

- [ ] Add indexes for frequently queried fields.
- [ ] Add indexes to fields used in sorting, filtering, and joins.
- [ ] Verify that index creation does not create unnecessary storage overhead.
- [ ] Check slow queries and optimize them before the app gets busy.

### Backups and recovery

- [ ] Back up production data regularly.
- [ ] Make sure the team knows how to restore a database if data is lost.
- [ ] Avoid testing destructive commands on the production database.
- [ ] Use a separate production database instead of a local dev database.

---

## 3. Error handling & logs

### Prevent sensitive leakage

- [ ] Turn off verbose stack traces in production.
- [ ] Return safe user-facing error messages instead of server internals.
- [ ] Do not expose environment secrets or database config in error output.
- [ ] Use structured logging without dumping sensitive values.
- [ ] Catch unexpected errors in central middleware.

### Log monitoring

- [ ] Check deployment logs after every push.
- [ ] Use a log viewer or provider dashboard to inspect runtime errors.
- [ ] Make sure the app logs startup, shutdown, and database connection issues.
- [ ] Set up alerts for failed deployments or repeated errors.
- [ ] Review logs after testing the public URL.

### Health checks and testing

- [ ] Add a `/health` or equivalent endpoint for quick verification.
- [ ] Test valid and invalid requests before announcing the app public.
- [ ] Confirm the app can read and write to the remote database.
- [ ] Test the app after a cold start or idle sleep cycle if the hosting plan supports it.

---

## 4. Environment setup and production optimization

### Clean production build

- [ ] Remove unnecessary development dependencies before deployment.
- [ ] Ensure the app only installs what is required for production.
- [ ] Avoid keeping local test scripts, debugging code, or console spam in production.
- [ ] Use a minimal and stable production environment.

### Node and package configuration

- [ ] Check the Node.js version required by the project.
- [ ] Ensure `npm install` is enough for the platform to build correctly.
- [ ] Add explicit production scripts such as `npm start` and `npm run build` if needed.
- [ ] Verify that Prisma generation runs correctly in the hosting environment.

### Performance and stability

- [ ] Confirm the server listens on `process.env.PORT` and not a hard-coded local port.
- [ ] Keep database pool sizes small and controlled.
- [ ] Avoid creating a new database connection for every request.
- [ ] Reduce large payloads and unnecessary network calls.
- [ ] Test the app under light load before sharing it with users.

---

## 5. Final pre-launch review

Before declaring the app ready:

- [ ] Project is pushed to GitHub
- [ ] `.env` is not visible in version control
- [ ] Hosting platform environment variables are set correctly
- [ ] Public URL responds successfully
- [ ] Database connection works in production
- [ ] Security headers and CORS are configured
- [ ] App logs are readable and useful
- [ ] No secrets are exposed in code or output
- [ ] App has been checked for obvious runtime issues

This checklist should be completed before a backend is considered production-ready.
