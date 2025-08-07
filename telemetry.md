---
id: telemetry
title: Telemetry
description: Manifest collects a tiny amount of anonymous data to help us improve the product. See how and what data, and eventually opt out.
---

# Telemetry

Telemetry is the process of collecting data from remote sources to improve a software.

At Manifest, we believe that we should only collect the minimum amount of data to help us improving our product. Participation is optional.

## What is being collected ?

When checking for a new version available in the admin panel, we log the following information:

- Current version of Manifest
- Anonymous IP
- Origin (URL)
- Environment (development/production)

Information is not logged more than once every 24 hours.

## Opting out

That tiny amount of data we collect helps us a lot to make Manifest a better product. Nevertheless you can opt-out anytime adding this to your `.env` file.

```
MANIFEST_TELEMETRY_DISABLED=true
```
