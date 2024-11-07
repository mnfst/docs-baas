---
id: deploy
---

# Deploy Manifest

Manifest is made to be self-hosted: backends can be deployed with ease wherever you want using different methods.

## Guides for popular cloud services

Unless you have motives to do otherwise, **we recommend using app platform services** as deploying your backend can be done **in a few clicks without hassle**.

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

## General guidelines

The following guidelines concern every deployment of Manifest. Unlike the previous section, which is provider-specific, it applies to all Manifest deployments.

### System requirements

The minimum system requirements to run a Manifest backend are **1vCPU** and **512 MB RAM**. It usually corresponds to one of the cheapest option on cloud providers.

The server needs at least [Node.js v18 or more](https://nodejs.org/fr) and a process manager like [pm2](https://github.com/Unitech/pm2/).

### Environment variables

You need to create a `.env` file at app root level with at least the 2 following variables:

```env title=".env"
TOKEN_SECRET_KEY=%ReplaceByYourKey%
NODE_ENV=production
```

#### Available variables

| Variable         | Default       | Description                                                                                                                                  |
| ---------------- | ------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| TOKEN_SECRET_KEY | -             | The [JSON Web Token](https://jwt.io/) secret key to encode your authentication tokens. [Generate one here](https://jwtsecret.com/generate)   |
| NODE_ENV         | `development` | The environment of the app. For Production/Staging deployements, it should be set to `production`                                            |
| PORT             | `1111`        | The port of your app. You can either adapt your server settings to listen to `1111` or change here to your server's default (usually `3000`) |

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

### Docker

[Docker](https://www.docker.com/) is a popular choice among developers. It uses containerization to ensure that the app will work well whatever the environment.

As of today, Manifest has no official Docker image but you can use the following steps to build your own Docker configuration.

```dockerfile title="dockerfile"
# Use the official Node.js image as a base
FROM node:18

# Copy package.json and package-lock.json (if available)
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of your application code
COPY . .

# Set the NODE_ENV environment variable
ENV NODE_ENV=production

# Expose the port the app runs on (adjust as needed)
EXPOSE 1111

# Start the application
CMD ["npm", "run", "start"]
```

And we need also to use [Docker Compose](https://docs.docker.com/compose/) file in order to persist the data for the database and the storage in a single command. Adding this `docker-compose.yml` file will create 2 volumes among the image.

```yaml title="docker-compose.yml"
version: '3.8'

services:
  app:
    image: manifest
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - '1111:1111'
    volumes:
      - ./manifest/backend.db:/app/manifest/backend.db
      - ./public:/app/public
```
