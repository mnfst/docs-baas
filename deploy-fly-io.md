---
id: deploy-fly-io
---

# Deploy Manifest on Fly.io

![Fly.io homepage](./assets/images/deploy/flyhome.png)

[Fly.io](https://fly.io) is a cloud provider that focuses on user experience and zero-config deployments.

First create a Fly.io account or sign in to access your dashboard. Click on "Launch an app" to start the process.

In our example we are deploying a Manifest backend hosted in [GitHub](https://github.com/). You can also connect to your codebase using the [Fly.io CLI](https://fly.io/docs/flyctl/install/).After configuring your GitHub integration with Fly.io, select the repository and click on "Deploy repository" to launch the deployment.

![Fly.io new app](./assets/images/deploy/fly1.png)

## Configuring your app

The next screen will allow you to configure the app to make it work as you want. Here are the settings that you can or must update (in UI order):

- **Basics (optional)**: Choose your app's name
- **Region (optional):** Choose the region closest to where your audience is
- **Network**: Set the internal port to `1111` to match the default Manifest port.
- **CPU & Memory (optional)**: Manifest can run on the **1 CPU** and **512 MB** VM Memory on small/medium projects. The default size is a bit bigger, you can replace it by a smaller one if you want.
- **Secrets**: Add the 2 secrets: `TOKEN_SECRET_KEY` (which you can generate at [JWTSecret.com](https://jwtsecret.com/generate)) and `NODE_ENV=production`.

Validate the config and Fly.io will build the image and deploy.

🎉 That's it! Click on "View app" to see it.
