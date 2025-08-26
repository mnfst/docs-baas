---
id: get-started
title: Get Started
slug: /
description: Manifest is an Open Source backend that fits in a single YAML file. Easy to edit, validate and version for humans and LLMs.
---

# Manifest Documentation 👋

## Introduction

Manifest is a **1-file backend** for prototypes and MVPs.

Most backend tools feel too heavy for simple apps. They force you to use bloated configuration UIs. Even with AI tools that generate frontend code, the backend remains a pain to set up and validate. It slows you down when you just want to test an idea or build something simple..
The solution

Manifest is an open source backend that fits in only 1 file. You define it in a simple yaml language to get data, auth, storage, logic and an admin panel.

**Key advantages:**

- 🧠 Zero friction setup
- 🚀 Ship your backend fast and stay focused on building your app
- 🤖 Easy for LLMs to generate
- 💻 Can run everywhere

## Install Manifest

Follow the steps below to install Manifest in your local machine.

### Prerequisites

- [NodeJS](https://nodejs.org/en/) (**20.x** or superior).

### Installation steps

Run this command to create a Manifest project ready to use with Cursor IDE.

```bash
# NPX
npx create-manifest@latest my-project --cursor

# Yarn
yarn create manifest my-project --cursor
```

This will create a `my-project` folder with a Manifest backend configured for Cursor.

You can replace `--cursor` with another option if you're using a different AI tool;

- `--copilot` if you're using **GitHub Copilot**
- `--windsurf` for **Windsurf**
- or remove it entirely if you're not using any AI coding tool

To start the Manifest backend, run the following command in the new project folder:

```
cd my-project
npm run start
```

You can now:
<br/> - See your **Admin panel** at http://localhost:1111 using the email `admin@manifest.build` and the password `admin`
<br/> - Use your **REST API** at http://localhost:1111/api

:::tip

If you already have a frontend app, we recommend that you use a **monorepo** structure with one folder for the backend and one folder for the frontend. For example you can run `npx create-manifest@latest server` at root level to add your Manifest to a `server` folder that you put next to the `client` folder that will contain your frontend.

:::

#### Note with PNMP

As [PNPM](https://pnpm.io/fr/) blocks postinstall scripts, we have to adapt the `package.json`. Add this to your `package.json` file before doing `pnpm install`:

```json
  "pnpm": {
    "onlyBuiltDependencies": [
      "@nestjs/core",
      "sharp",
      "sqlite3"
    ]
  }
```
