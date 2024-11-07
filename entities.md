---
id: entities
title: Entities
---

# Create an entity

An entity is an object often linked to a real world concept like users, customers, videos etc. Creating an entity in manifest generates **CRUD endpoints** that can be used by the [REST API](rest-api.md) or the [SDK](javascript-sdk.md).

All entities are located in the `backend.yml` file under the **entities** property.

## Syntax

Let's see a simple example:

```yaml title="manifest/backend.yml"
# manifest/backend.yml
name: A pet app

entities:
  😺 Cat:
    properties:
      - name
  🐶 Dog:
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

## Entity params

You can pass different arguments to configure your entities. Example:

```yaml
entities:
  👤 Member:
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
