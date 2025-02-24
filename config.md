---
id: config
---

# Configuration

Manifest embraces the **convention over configuration** concept: it assumes several logical situations by default without showing you the setting to keep things as simple as possible.

Nevertheless there is still the possibility to adapt your Manifest app to your needs, especially through the `.env` file. Here are the list of available environment variables:

## General variables

General environment variables.

| Variable      | Default                             | Description                                                                                                                                  |
| ------------- | ----------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| NODE_ENV      | `development`                       | The app environment. For production and staging instances, it should be set to `production`, mostly to turn off live reload on file change.  |
| PORT          | `1111`                              | The port of your app. You can either adapt your server settings to listen to `1111` or change here to your server's default (usually `3000`) |
| BASE_URL      | `http://localhost:$PORT`            | The base url of your backend. Change it when deploying if you use [file or image upload](./upload.md)                                        |
| OPEN_API_DOCS | `true` unless `NODE_ENV=production` | Shows or not the OpenAPI doc (formerly Swagger) for your REST API at `/api`. Make sure to set to `true` if you want to display on production |

## Paths

Environment variables related to paths.

| Variable                 | Default                 | Description                                                                                  |
| ------------------------ | ----------------------- | -------------------------------------------------------------------------------------------- |
| PUBLIC_FOLDER            | `/public`               | The public folder to show [static files](https://expressjs.com/en/starter/static-files.html) |
| MANIFEST_HANDLERS_FOLDER | `/manifest/handlers`    | The folder to put your handlers functions for [custom endpoints](./endpoints.md)             |
| MANIFEST_FILE_PATH       | `/manifest/backend.yml` | The relative or absolute path of your Manifest YAML file                                     |

## Database

By default Manifest runs with [SQLite](https://www.sqlite.org/) to enable instant launch with the `npx add-manifest@latest` command. We recommend to switch to [PostgreSQL](https://www.postgresql.org/) on production for more robustness and to choose from a large number of cloud/DBaaS providers.

| Variable      | Default                | Description                                                               | Applies To |
| ------------- | ---------------------- | ------------------------------------------------------------------------- | ---------- |
| DB_CONNECTION | `sqlite`               | Choose `postgres` to switch to PostgreSQL                                 | Both       |
| DB_PATH       | `/manifest/backend.db` | Path of the database. Your server should have access to this path locally | SQLite     |
| DB_HOST       | `localhost`            | Database host                                                             | PostgreSQL |
| DB_PORT       | `5432`                 | Database port                                                             | PostgreSQL |
| DB_USERNAME   | `postgres`             | Database username                                                         | PostgreSQL |
| DB_PASSWORD   | `postgres`             | Database password                                                         | PostgreSQL |
| DB_DATABASE   | `manifest`             | Database name                                                             | PostgreSQL |
| DB_SSL        | `false`                | Require SSL for DB connection. Set to true if using remote DB.            | PostgreSQL |
