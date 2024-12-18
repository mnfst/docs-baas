---
id: entities
title: Entities
---

# Create an entity

An entity is a model of objects linked to real-world concepts. Creating an entity in manifest generates **CRUD endpoints** that can be used by the [REST API](rest-api.md) or the [SDK](javascript-sdk.md).

All entities are located in the `backend.yml` file under the **entities** property.

There are 2 types of entities in Manifest: [Collections](#collections) and [Singles](#singles). **Collections** are multiple instances of similar data, stored as a list. E.g., users, customers, videos, etc. Singles are unique, standalone data that are singular in nature. E.g., home content, about content, settings, logo...

## Collections

Let's see a simple example:

```yaml title="manifest/backend.yml"
name: A pet app

entities:
  Cat 😺:
    properties:
      - name
  Dog 🐶:
    properties:
      - name
```

This file will generate the **Cat** and **Dog** entity both with a `name` property. You can now add your own pets through the admin panel!

### Seed

Dummy data is crucial for app development and testing. You can generate dummy data for all your entities with the simple command:

```
npm run manifest:seed
```

:::warning

The seed replaces the previous data by the new one and thus should never be used in production.

:::

### Collection entity params

You can pass different arguments to configure your entities. Example:

```yaml
entities:
  Member 👤:
    seedCount: 200
    mainProp: lastName
    properties:
      - firstName
      - lastName
      - email
```

| Option            | Default                    | Type     | Description                                                                                              |
| ----------------- | -------------------------- | -------- | -------------------------------------------------------------------------------------------------------- |
| **authenticable** | false                      | boolean  | Whether the entity is [authenticable](auth.md#authenticable-entities) or not                             |
| **mainProp**      | _first string field_       | string   | Identifier prop. Used widely on the admin panel                                                          |
| **nameSingular**  | _singular lower case name_ | string   | The singular lowercase name of your entity. Used widely on the admin panel.                              |
| **namePlural**    | _plural lower case name_   | string   | The plural lowercase name of your entity. Used widely on the admin panel Default: plural lowercase name. |
| **policies**      | -                          | Policies | The [access control policies](./policies.md) of your entity                                              |
| **properties**    | `[]`                       | Array    | The [properties](./properties.md) of your entity                                                         |
| **seedCount**     | `50`                       | number   | the number of entities to seed when running the seed command.                                            |
| **slug**          | _plural dasherized name_   | string   | The kebab-case slug of the entity that will define API endpoints.                                        |

## Singles

Single entities differ a bit from [collections](#collections). A single entity is **singular in nature**, and there can be only one record of them. Examples are website dynamic elements, pages, standalone content or settings. They do not have relations.

On single entities **Create** and **Delete** CRUD actions are disabled. Thus, the [REST API endpoints for single entities](./rest-api.md#singles) are different.

```yaml
ContactPage:
  single: true
  slug: contact
  properties:
    - { name: title, type: string }
    - { name: content, type: text }
    - { name: image, type: image }
  validation:
    title: { required: true }
```

### Single entity params

You can pass different arguments to configure your single entities. Example:

```yaml
entities:
  Member 👤:
    seedCount: 200
    mainProp: lastName
    properties:
      - firstName
      - lastName
      - email
```

| Option           | Default                    | Type     | Description                                                                 |
| ---------------- | -------------------------- | -------- | --------------------------------------------------------------------------- |
| **nameSingular** | _singular lower case name_ | string   | The singular lowercase name of your entity. Used widely on the admin panel. |
| **policies**     | -                          | Policies | The [access control policies](./policies.md) of your entity                 |
| **properties**   | `[]`                       | Array    | The [properties](./properties.md) of your entity                            |
| **slug**         | _plural dasherized name_   | string   | The kebab-case slug of the entity that will define API endpoints.           |
