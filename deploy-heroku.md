---
id: deploy-heroku
---

# Deploy Manifest on Heroku

[Heroku](https://heroku.com/) is a popular cloud app platform provider that supports many languages and provides a nice interface to deploy apps.

### Create a "start" script for production

The `npm run manifest` script should only be used for **development** as it watches file changes.

Go back to your codebase and open the `package.json` file and add a new **start** script on the scripts list with the value `node node_modules/manifest/dist/manifest/src/main.js` as following:

```json title="package.json"
"scripts": {
    "start": "node node_modules/manifest/dist/manifest/src/main.js"
    [...]
}
```

## Link the source provider

Sign in to your [Heroku](https://heroku.com/) account or create a new one.

You can click on the "New" button and then "Create a new app" to get started. Choose the app name and the region that is closest to your users and click on "Create app".

In our example we are deploying a Manifest backend hosted in [GitHub](https://github.com) but you can get it from other sources too like Heroku Git or Container Registry.

Select "GitHub" and then connect the repository of your manifest backend. Click on "Automatic deploys" if you want to enable push-to-deploy on the main branch. Make sure that you are on the correct branch that corresponds to your production version (usually `main` or `master`).

![Heroku setup source and automatic deploys](./assets/images/deploy/heroku1.png)

Click on "Deploy branch" to build your app.

:::warning

A message will indicate that your app is ready, and a link to view it will appear. However, we still have some configuration to make it work.

:::

## Add environment variables

Go to the "Settings" tab and click on "Reveal Config Vars" in the "Config Vars" section. Then add the 2 environment variables: `TOKEN_SECRET_KEY` (which you can generate at https://jwtsecret.com/generate) and `NODE_ENV=production`.

## Create a start script for your Dyno

![Heroku dynos script](./assets/images/deploy/heroku2.png)

Heroku's task runners are called Dynos and the default one will run on `npm run start`.

To make this script launch our Manifest backend, go back to your codebase and open the `package.json` file and add a new "start" script on the scripts list with the value `node node_modules/manifest/dist/manifest/src/main.js`:

```json title="package.json"
"scripts": {
	"start": "node node_modules/manifest/dist/manifest/src/main.js",
	"manifest": "node node_modules/manifest/scripts/watch/watch.js",
	"manifest:seed": "node node_modules/manifest/dist/manifest/src/seed/scripts/seed.js"
},
```

Commit, push, and deploy on Heroku if you haven't enabled the automatic deploys. In your Heroku panel, go to the "Resources" tab and activate Dynos to start the script.

🎉 That's it! Now you can click on "Open app" to see it!
