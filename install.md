---
id: install
---

# Install

## Prerequisites

- [NodeJS](https://nodejs.org/en/) (**18.x**).

## Install Manifest

Run the following on your terminal from the **root of your project**:

```bash
npx add-manifest@latest
```

This will create a `manifest/backend.yml` file and add the required dependencies.

Then, serve the backend locally:

```
npm run manifest
```

You can now:
<br/> - See your **Admin panel** at http://localhost:1111 using the email `admin@manifest.build` and the password `admin`
<br/> - Use your **REST API** at http://localhost:1111/api

:::tip

If you already have a frontend app, you can run the `npx add-manifest` command from your **project root** to include it in your repo.

:::
