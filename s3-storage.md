---
id: s3-storage
title: S3 Storage
description: Use any S3-compatible storage to store your user assets in your Manifest backend. Images, videos, documents...
---

# S3 Storage

## Introduction

Manifest supports **S3 storage** to manage assets. To use it, ensure you have:

- A **storage bucket** (or equivalent).
- Access credentials with read and write permissions.

## Configuration

Define these variables in your `.env` file:

```
S3_BUCKET=my-bucket-name
S3_ENDPOINT=https://your-s3-provider.com
S3_REGION=XXX
S3_ACCESS_KEY_ID=XXX
S3_SECRET_ACCESS_KEY=XXX
```

Optionally, you can set a folder prefix to prepend your object path:

```
S3_FOLDER_PREFIX=development/manifest
```

:::note  
S3 storage in Manifest has been **validated** with **AWS S3** and **Digital Ocean Spaces**. Other S3-compatible providers may work, but they have not been officially tested.  
:::

## Manifest guides

<a href="https://manifest.build/integrations/s3-storage" download>
AWS S3 Storage guide
</a>
<a href="https://manifest.build/integrations/digital-ocean-spaces" download>
Digital Ocean Spaces guide
</a>
