---
id: access
---

# Access policies

Access Policies ensure that we provide specific access to resources for users. You may want to open publicly the **read** access for an entity while limiting **creation** to logged in users for example.

Acces policies are a way to implement **Authorization** following the RBAC (Role-Based Access Control) method. Indeed it is possible to create different entities (ex: User, Manager...) with different access to resources.

Policies can be added to [entities](./entities.md) or [endpoints](./endpoints.md).

## Syntax

The policies for each rule can be added to each [entity description](./entities.md) as shown below:

```yaml
entities:
  Invoice 🧾:
    properties:
      - number
      - { name: issueDate, type: date }
    policies:
      create:
        - { access: restricted, allow: User } # Only logged in users can create.
      read:
        - access: public # All read endpoints are public.
      update:
        - access: admin # Only logged in admins can update.
      delete:
        - access: forbidden # No one can delete (even admins!)
```

In this case, everyone can see the **Invoice** items, only logged-in **Users** can create new ones. Updating an Invoice is restricted to [Admins](./auth.md#admins) only and no one can delete them (not even Admins).

By default, all rules access are set to **admin** and thus only visible by logged-in **Admins**.

| Prop       | Description                                                               | Type               |
| ---------- | ------------------------------------------------------------------------- | ------------------ |
| **access** | The type of access: **public**, **restricted**, **admin**, **forbidden**  | AccessType         |
| **allow**  | Only for **restricted** access: the entity (or entities) that have access | string \| string[] |

### Access types

There are 4 possible access types:

| Access         | Description                                                                                                                                                                                     | Short version (emoji) |
| -------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------- |
| **public**     | Everyone has access                                                                                                                                                                             | 🌐                    |
| **restricted** | Only logged-in users have access to it. If _allow_ key specifies one or several entities, users logged in as other entities will not have access. Admins always have access to restricted rules | 🔒                    |
| **admin**      | Only [admins](./auth.md#admins) have access                                                                                                                                                     | 👨🏻‍💻                    |
| **forbidden**  | No one has access, not even admins                                                                                                                                                              | 🚫                    |

### Entity rules

Each entity has **5 rules** where one or several access policies can be applied:

- **create**: create a new item
- **read**: see the detail and the list of items
- **update**: update an existing item
- **delete**: delete an existing item
- **signup**: sign up as a new user (only for [authenticable entities](./auth.md#authenticable-entities))

By default, all rules have the [admin access type](#access-types)

## Additional examples

```yaml
entities:
  Project 🗂️:
    properties:
      - name
    policies:
      read:
        - { access: restricted, allow: [Contributor, Manager] } # Only some entities (and admins).
      create:
        - { access: restricted, allow: Manager } # Only managers (and admins).
      update:
        - access: 👨🏻‍💻 # Only admin.
      delete:
        - access: 🚫 # Forbidden (no one).

  Contributor 👨‍💼:
    authenticable: true
    properties:
      - name
    policies:
      signup:
        - access: 🚫 # Forbidden (no one).
      create:
        - { access: 🔒, allow: Manager } # Managers can create contributors.
      update:
        - { access: 🔒, allow: Manager }
      delete:
        - { access: 🔒, allow: Manager } # Managers can create contributors.

endpoints:
  basicEndpoint:
    path: /basic
    description: A basic endpoint that returns a simple message.
    method: GET
    handler: basicEndpoint
    policies:
      - access: public # This endpoint is public.
```
