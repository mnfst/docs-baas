---
id: deploy
---

# Deploy Manifest

Manifest is made to be **self-hosted**: backends can be deployed with ease wherever you want using different methods.

We recommend using [Docker](#docker) to simplify deployments but you also can install Manifest manually on a VM or on a bare-metal server.

## System requirements

The minimum system requirements to run a small Manifest backend are **1vCPU** and **512 MB RAM**. It usually corresponds to one of the cheapest option on cloud providers.

The server needs at least [Node.js v18 or more](https://nodejs.org/fr) and a process manager like [pm2](https://github.com/Unitech/pm2/).

### Database

Manifest works by default in local with SQLite but we recommend to [switch to PostgreSQL](./config.md#database) for production deployments.

All popular hosting providers have their managed PostgreSQL solutions and there is many DB-as-a-Service providers like [Neon](https://neon.tech/) that offer generous free-tier to get started. Here is a list of popular services:

| Provider            | Service Name                                                                                          |
| ------------------- | ----------------------------------------------------------------------------------------------------- |
| **Amazon**          | [Amazon Aurora PostgreSQL](https://aws.amazon.com/rds/aurora/postgresql-features/)                    |
| **Google Cloud**    | [Cloud SQL for PostgreSQL](https://cloud.google.com/sql/docs/postgres)                                |
| **Microsoft Azure** | [Azure Database for PostgreSQL](https://azure.microsoft.com/en-us/products/postgresql/)               |
| **Neon**            | [Neon Database](https://neon.tech/)                                                                   |
| **Crunchy Data**    | [Crunchy Bridge](https://www.crunchydata.com/products/crunchy-bridge)                                 |
| **Aiven**           | [Aiven for PostgreSQL](https://aiven.io/postgresql)                                                   |
| **DigitalOcean**    | [DigitalOcean Managed PostgreSQL](https://www.digitalocean.com/products/managed-databases-postgresql) |
| **Heroku**          | [Heroku Postgres](https://www.heroku.com/postgres)                                                    |
| **StackGres**       | [StackGres](https://stackgres.io/)                                                                    |
| **Render.com**      | [Render PostgreSQL Database](https://render.com/docs/postgresql-creating-connecting)                  |

:::tip

While you could technically create a [Docker volume](https://docs.docker.com/engine/storage/volumes/) to ensure data persistence for your SQLite database, we found it easier to use any managed PostgreSQL service **even if you never used PostgreSQL**.

:::

### Storage

As for the database, uploaded files and images will be flushed when the container restarts if using [Docker](#docker).

:::danger

We are currently working on integrating **S3 storage services** to prevent those data losses. In the meantime we **do not recommend** to deploy your app in prod if you rely on [file](./upload.md#upload-a-file) or [image](./upload#upload-an-image) uploads.

:::

### Environment variables

You need to create a `.env` file at app root level with at least the following variables:

```env title=".env"
TOKEN_SECRET_KEY=%ReplaceByYourKey%
NODE_ENV=production
BASE_URL=https://my-backend.com
```

See more [environment variables](./config.md#general-variables) you may need.

### Start script for production

The `npm run manifest` script should only be used for **development** as it watches file changes.

Go back to your codebase and open the `package.json` file and add a new **start** script on the scripts list with the value `node node_modules/manifest/dist/manifest/src/main.js` as following:

```json title="package.json"
"scripts": {
    "start": "node node_modules/manifest/dist/manifest/src/main.js"
    [...]
}
```

After that you will be able to run Manifest for production with `npm run start`.

## Docker

[Docker](https://www.docker.com/) is a popular choice among developers. It uses containerization to ensure that the app will work well whatever the environment.

```dockerfile title="Dockerfile"
# Use the official Node.js image as a base
FROM node:18-slim

# Copy package.json and package-lock.json (if available)
COPY package*.json ./

# Install dependencies
RUN npm install && npm cache clean --force && rm -rf /root/.npm && rm -rf /tmp/*

# Copy the rest of your application code
COPY . .

# Set the NODE_ENV environment variable
ENV NODE_ENV=production

# Expose the port the app runs on (adjust as needed)
EXPOSE 1111

# Start the application
CMD ["npm", "run", "start"]
```

## Guides for popular app platform services

Here are some quick guides to launch your app in a few minutes:

<div class="card-container">
  <a href="./deploy-digital-ocean" class="card">
    <p>Deploy Manifest on DigitalOcean App Platform</p>
    <span>➡️</span>
  </a>
  
  <a href="./deploy-fly-io" class="card">
    <p>Deploy Manifest on Fly.io</p>
    <span>➡️</span>
  </a>
  
  <a href="./deploy-render-com" class="card">
    <p>Deploy Manifest on Render.com</p>
    <span>➡️</span>
  </a>
  
  <a href="./deploy-heroku" class="card">
    <p>Deploy Manifest on Heroku</p>
    <span>➡️</span>
  </a>
</div>
