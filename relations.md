---
id: relations
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Relations

You can create **one-to-many** relationships or **many-to-many** relationships.

## Syntax

The following example showcases the possibilities of **Manifest relations**:

```yaml title="manifest/backend.yml"
name: Basketball League 🏀

entities:
  Player 🤾:
    properties:
      - name
    belongsTo:
      - Team
    belongsToMany:
      - Skill

  Team 👥:
    properties:
      - name

  Skill 💪:
    properties:
      - name

  Fixture 🏟️:
    properties:
      - { name: homeScore, type: number }
      - { name: awayScore, type: number }
    belongsTo:
      - { name: homeTeam, entity: Team }
      - { name: awayTeam, entity: Team }
```

In other words:

- The **belongsTo** keyword creates a **many-to-one** relationship (and its opposite _one-to-many_)
- The **belongsToMany** keyword creates a **many-to-many** bi-directional relationship
- The **long syntax** (as in _Fixture.belongsTo_) is useful when we want to edit the name of the relationships

:::tip

All relationships are **bi-directional**, which means that you just have to specify them **in one side only** to work with them in both directions.

When you define a **belongsTo** relationship, it implicitly set the opposite _hasMany_ relationship and allow [querying relations](#querying-relations) in both directions.
:::

## Relation params

You can pass arguments using the long syntax:

| Option     | Default     | Type    | Description                                                                                                                      |
| ---------- | ----------- | ------- | -------------------------------------------------------------------------------------------------------------------------------- |
| **name**   | Entity name | string  | The name of the relation                                                                                                         |
| **entity** | -           | string  | The class name of the entity that the relationship is with                                                                       |
| **eager**  | `false`     | boolean | Whether the relationship should be eager loaded. Otherwise, you need to explicitly request the relation in the client SDK or API |

## Work with relations

### Load relations

You can specify which relations you want to load with your entities in your query. **Eager relations** are loaded automatically.

<Tabs>
  <TabItem value="sdk" label="JS SDK" default>
    ```js
      // Fetch entities with 2 relations.
      const cities = await manifest
        .from('cities')
        .with(['region', 'mayor'])
        .find()

      // Fetch nested relations.
      const cities = await manifest
        .from('cities')
        .with(['region', 'region.country', 'region.country.planet'])
        .find()
    ```

  </TabItem>
  <TabItem value="rest" label="REST API" default>
    ```http

    // Fetch entities with 2 relations.
    GET http://localhost:111/api/dynamic/city?relations=region,mayor

    // Fetch nested relations.
    GET http://localhost:111/api/dynamic/city?relations=region,region.country,region.country.planet
    ```

  </TabItem>
</Tabs>

### Filter by relation

Once the relation is loaded, you can also filter items by their relation id or properties.

<Tabs>
  <TabItem value="sdk" label="JS SDK" default>
    ```js
      // Get all cats that belong to owner with id 1.
      const cats = await manifest
        .from('cats')
        .with(['owner'])
        .where('owner.id = 1')
        .find()

      // Get all cats that have an owner with name "Jorge".
      const cats = await manifest
        .from('cats')
        .with(['owner'])
        .where('owner.name = Jorge')
        .find()
    ```

  </TabItem>
  <TabItem value="rest" label="REST API" default>
    ```http
    // Get all cats that belong to owner with id 1.
    GET http://localhost:1111/api/dynamic/cats?relations=owner&owner.id_eq=1

    // Get all cats that have an owner with name "Jorge".
    GET http://localhost:1111/api/dynamic/cats?relations=owner&owner.name_eq=Jorge
    ```

  </TabItem>
</Tabs>

### Store relations

To store or update an item with its relations, you have to pass the relation id(s) as a property that end with **Id** for many-to-one and **Ids** for many-to-many like `companyId` or `tagIds`

<Tabs>
  <TabItem value="sdk" label="JS SDK" default>
    ```js
      // Store a new player with relations Team and Skill.
      const newPlayer = await manifest.from('players').create({
        name: 'Mike',
        teamId: 10,
        skillIds: [1,2,3,4,5]
      })
    ```

  </TabItem>
  <TabItem value="rest" label="REST API" default>
    ```http
    // Store a new player with relations Team and Skill.
    POST http://localhost:1111/api/dynamic/players
    Content-Type: application/json
    {
        "name": "Mike",
        "teamId": 10,
        "skillIds": [1,2,3,4,5]
    }
    ```

  </TabItem>
</Tabs>

:::note

When storing **many-to-many** relations, you always need to **pass an array**, even if you just have one single value.

:::

### Update relations

As for updating properties, you can either do a **full replacement** using the update function (PUT) or a **partial replacement** using the patch function (PATCH).

<Tabs>
  <TabItem value="sdk" label="JS SDK" default>
    ```js
      // Replaces the whole skill relations by the new skillIds array.
      await manifest.from('players').update(1, {
        name: 'Mike',
        teamId: 10,
        skillIds: [10, 11]
      })

      // Updates the team without changing the skills or the name.
      await manifest.from('players').patch(1, {teamId: 5})
    ```

  </TabItem>
  <TabItem value="rest" label="REST API" default>
    ```http
    // Replaces the whole skill relations by the new skillIds array.
    PUT http://localhost:1111/api/dynamic/players/1
    Content-Type: application/json
    {
      name: 'Mike',
      teamId: 10,
      skillIds: [10, 11]
    }

    // Updates the team without changing the skills or the name.
    PATCH http://localhost:1111/api/dynamic/players/1
    Content-Type: application/json
    {
      teamId: 5,
    }
    ```

  </TabItem>
</Tabs>
