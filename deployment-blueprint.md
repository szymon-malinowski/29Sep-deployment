# Deployment Blueprint: Render + MongoDB Atlas

This blueprint follows the assignment and uses one application host and one database host from the research section: Render for the app and MongoDB Atlas for the database.

## 1. Create the MongoDB Atlas database

1. Sign up for MongoDB Atlas.
2. Create a new project.
3. Build a free shared cluster in a region close to your app.
4. Create a database user with a strong username and password.
5. Add the network access rule needed for your app.
6. In the Atlas dashboard, choose Connect, then Drivers, and copy the connection string.
7. Replace the username, password, and database name in the URI.

Example format:

```text
mongodb+srv://USERNAME:PASSWORD@cluster.example.mongodb.net/DATABASE_NAME
```

Store this connection string in a secure environment variable and never commit it to GitHub.

## 2. Prepare the app for deployment

Before deploying, make sure your project includes:

- a valid `package.json`
- a working `npm start` script
- a `.gitignore` file that excludes `.env`
- a server that listens on `process.env.PORT`
- a health route such as `/health` if available

Example:

```js
const port = process.env.PORT || 3000;
app.listen(port, () => console.log(`Server listening on ${port}`));
```

Example `.gitignore`:

```text
.env
.env.*
node_modules
```

## 3. Set environment variables safely

Create a local `.env` file for development only:

```text
MONGODB_URI=mongodb+srv://USERNAME:PASSWORD@cluster.example.mongodb.net/DATABASE_NAME
NODE_ENV=development
PORT=3000
```

Do not commit this file to version control. On Render, add the same variables in the project environment settings instead of storing them in code.

## 4. Push the project to GitHub

1. Create a GitHub repository.
2. Commit the project files.
3. Make sure `.env` is ignored.
4. Push the repo to GitHub.

When the GitHub repository is ready, the hosting platform can connect to it and deploy automatically.

## 5. Deploy the app on Render

1. Sign in to Render.
2. Click New and choose Web Service.
3. Connect the GitHub repository.
4. Select the branch to deploy.
5. Choose the Node.js environment.
6. Set the build command:

```bash
npm install
```

7. Set the start command:

```bash
npm start
```

8. Add the environment variables in Render:

- `MONGODB_URI` = the Atlas connection string
- `NODE_ENV` = `production`
- `PORT` = automatic or a Render-supported value

9. Click Create Web Service.

Render will install the dependencies, start the app, and give the project a public URL.

## 6. Automatic deployments

Once the project is connected to GitHub, every push to the selected branch can trigger a new deployment automatically. This makes updates easier because the platform rebuilds and restarts the app with no manual upload step.

## 7. Test and verify the live app

After deployment, do the following:

1. Check the Render logs for startup errors.
2. Visit the public URL and test a health route or root route.
3. Test a GET endpoint to read data.
4. Test a POST endpoint to write new data.
5. Check the same data again to confirm it was saved.
6. Open MongoDB Atlas and confirm that the record exists in the database.

This verifies that the public app can both read and write to the remote database.

## 8. Final deployment checklist

- [ ] Remote database is created and connection string is stored safely.
- [ ] `.env` is not committed to GitHub.
- [ ] Render environment variables are configured.
- [ ] Build and start commands work correctly.
- [ ] GitHub repository is connected to Render.
- [ ] Public URL responds successfully.
- [ ] Read and write operations work against the remote database.

This is the basic deployment flow for a student project using Render and MongoDB Atlas.
