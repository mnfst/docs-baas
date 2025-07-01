---
id: astro
title: Create a Full-Stack app with Astro and Manifest
description: Quick start guide to create a full-stack app using Astro as a frontend and Manifest as a backend.
---

# Quick start with Astro

Connect your Manifest backend to your Astro app.

:::warning

This quick start guide focuses exclusively on the **frontend**. To ensure the functionality of this code, your Manifest backend must be [up and running](./introduction.md#install-manifest) at `http://localhost:1111`.

:::

## 1. Create a Astro app

If you already have your Astro app up and running, skip this step. Replace `my-astro-app` by the name of the Astro app created.

```
npm create astro@latest
cd my-astro-app
npm run dev
```

## 2. Install Manifest SDK

Install the JS SDK from the root of your Astro app.

```
npm i @mnfst/sdk
```

## 3. Use it in your app

In that example we are using a Cat entity [created previously](entities.md). Replace it by your own entity.

```js
---

import Manifest from '@mnfst/sdk'

// Init SDK.
const manifest = new Manifest()

// Fetch the cat list.
const result = await manifest.from('cats').find()
const cats = result.data

---

<html lang="en">
	<head>
		<meta charset="utf-8" />
		<link rel="icon" type="image/svg+xml" href="/favicon.svg" />
		<meta name="viewport" content="width=device-width" />
		<meta name="generator" content={Astro.generator} />
		<title>Astro</title>
	</head>
	<body>
		<ul>
			{
				cats.map((cat) => (
					<li>{cat.name}</li>
				))
			}
		</ul>

	</body>
</html>
```

Checkout the [SDK doc](./crud.md#using-the-javascript-sdk) to see more usages of the SDK: CRUD operations, file upload, authentication,
