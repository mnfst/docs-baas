---
id: rest-api
---

# REST API

An alternative to the [JS SDK](javascript-sdk.md) to connect to your backend is through the **REST API**.

The slug of the entity is by default the plural dasherized name of the entity, but you can change it in the [params](entities.md#entity-params)

All entities start with the `api` prefix followed by `collections` for [collections](#collections) and `singles` for [single entities](#singles).

Examples:

- `http://localhost:1111/api/collections/cats` gets the list of the cats
- `http://localhost:1111/api/singles/home-content` gets the home content

:::tip

An **OpenAPI** documentation for the REST API is automatically generated on http://localhost:1111/api. Have a look!

:::

## Collections

### GET /slug

Gets a list of items from an entity.

#### Filters

You can filter by [property](properties.md) to refine the list of items. Use suffix to pass logic to it:

| Suffix     | Description           | Example                    |
| ---------- | --------------------- | -------------------------- |
| **\_eq**   | equals                | `isActive_eq=true`         |
| **\_neq**  | not equals            | `name_neq=alice`           |
| **\_gt**   | greater than          | `birthdate_gt=2020-01-01 ` |
| **\_gte**  | greater than or equal | `age_gte=4`                |
| **\_lt**   | less than             | `amount_lt=400`            |
| **\_lte**  | less than or equal    | `amount_lte=400`           |
| **\_like** | like                  | `name_like=%bi%`           |
| **\_in**   | included in           | `customer_in=1,2,3`        |

By default the results are ordered by `id` in a `DESC` order and thus shows the new ones first.

#### Relations

##### Load relations

You can specify the relations you want to load. [Eager relationships](relations.md#relation-params) are loaded by default.

```http
// Loads cats and their owners.
GET http://localhost:1111/api/collections/cats?relations=owner

// Coma-separated relations.
GET http://localhost:1111/api/collections/invoices?relation=project,customer

// Nested relations.
GET http://localhost:111/api/collections/city?relations=region,region.country
```

##### Filter by relations

Once the relation is loaded, you can also filter by its properties using the same filters suffixes:

```http
GET http://localhost:1111/api/collections/cats?relations=owner&owner.id_eq=1
GET http://localhost:1111/api/collections/cats?relations=owner&owner.name_eq=Jorge
```

#### Pagination

All list requests are paginated by default. Just use the `page` parameter to chose your page and the `perPage` param if you want to change the number of items per page.

```json title="Response format"
{
  "data": [{...}, {...}],
  "currentPage": 1,
  "lastPage": 10,
  "from": 1,
  "to": 10,
  "total": 100,
  "perPage": 10
}
```

| Param       | Description                      | Example      |
| ----------- | -------------------------------- | ------------ |
| **page**    | The number of the page requested | `page=3`     |
| **perPage** | The number of items of each page | `perPage=40` |

#### Order

Order your list by a defined property.

| Param       | Description                                | Example       |
| ----------- | ------------------------------------------ | ------------- |
| **orderBy** | The name of property you want to order by. | `orderBy=age` |
| **order**   | Ascending 'ASC' or Descending 'DESC'       | `order=DESC`  |

### GET /slug/\:id

Get a single item.

### POST /slug

Create a new item.

Provide a Request Payload in JSON:

```json title="Request body"
{
  "age": 2
  "name": "Milo"
}
```

### PUT /slug/\:id

Update an item.

Provide a Request Payload in JSON:

```json title="Request body"
{
  "age": 2
  "name": "Milo"
}
```

### DELETE /slug/\:id

Delete an item.

## Singles

On single entities, there is one item only so we do not need to specify the id.

### GET /slug

Gets the single record.

```json title="Response format"
{
  "title": "New home title",
  "showCarousel": true
}
```

### PUT /slug

Update the single record

Provide a Request Payload in JSON:

```json title="Request body"
{
  "title": "New home title",
  "showCarousel": true
}
```
