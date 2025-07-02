---
id: security
title: Security
description: Implement Security in your Manifest backend and make sure that your app is protected.
---

# Security

Implement security measures in your Manifest backend.

## Rate limiting

Rate-limiting can protect your backend from **brute-force attacks** by blocking requests after reaching a limit.

You can implement one or several throttler definitions to limit API calls in the `manifest.yml` file. The following example allow no more than 2 calls per second, and 50 calls per minute:

```yaml title="manifest.yml"
name: my app

settings:
  rateLimits:
    - { name: 'short', limit: 2, ttl: 1000 } # 2 requests per second
    - { name: 'medium', limit: 50, ttl: 60000 } # 50 requests per minute.
```
