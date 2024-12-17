---
id: introduction
title: Introduction
slug: /
---

# Welcome to Manifest Documentation 👋

Manifest is the simplest **Micro-backend** you will find: It provides a complete backend to your client app without the hassle that comes with it.

It actually fits into **a single YAML file** that generates a complete backend. Here is an example of a complete Manifest app:

```yaml title="manifest/backend.yml"
name: Healthcare application

entities:
  Doctor 👩🏾‍⚕️:
    properties:
      - fullName
      - avatar
      - { name: price, type: money, options: { currency: EUR } }
    belongsTo:
      - City

  Patient 🤒:
    properties:
      - fullName
      - { name: birthdate, type: date }
    belongsTo:
      - Doctor

  City 🌍:
    properties:
      - name
```

## Key features

- ⚡ Develop 10x faster comparing to traditional approaches
- 😎 Super-easy syntax easy to read and version control
- 🕊️ Self-hosted free open source software
