---
id: authentication
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Authentication

## Introduction

Authentication is the process of proving that people are who they say they are.

Manifest uses **JSON Web Tokens (JWT)** to do that. When you log in, you basically create a new **token** that you use in your next requests to prove your identity. This allows us to use [Policies](./auth#access-policies) to grant or deny the access to some resources based on the user characteristics.

:::info

Notice the `TOKEN_SECRET_KEY` variable in your `.env` file ? This is the key that will encrypt your tokens, you can [generate one here](https://jwtsecret.com/generate).

:::

## Admins

Admins are a built-in entity that are **the only ones with access to the admin panel** (located at http://localhost:1111 by default). The admins are usually the people who manage the application on a day-to-day basis. Only admins can see and manage other admins.

Even though they are the most powerful users of your application, you can still create some [policies](./policies.md#access-types) that will restrict the access even for them.

The [seed command](./entities.md#seed) will create an admin with the email `admin@manifest.build` and the password `admin`. You can create more admins from the admin panel.

:::tip

In Manifest, the admin panel is **non-technical** 😺.

It means that you can give credentials to the administrators of your app without worrying that they will end up breaking the system!

:::

## Authenticable entities

You can convert any entity into an **authenticatable entity**, allowing users to log in with it.

```yaml
entities:
  Patient 🤒:
    authenticable: true # Makes entity authenticable.
    properties:
      - name
```

Authenticable entities have 2 extra properties that are used as credentials to log in: `email` and `password`. You do not need to specify them.

The passwords are automatically hashed using _SHA-3 algorithm_.

## Syntax

### Login

Log in your credentials as an **admin** or an **authenticable entity**.

<Tabs>

  <TabItem value="rest" label="REST API" default>
    ```http title="Example HTTP Request"
    POST /api/auth/admins/login
    Content-Type: application/json
    {
      "email": "admin@manifest.build",
      "password": "password"
    }
    ```
    ```json title="Example HTTP Response"
    {
      "token": "12345"
    }
    ```

    Then you can add your token to your requests `Authorization` header using the **Bearer** prefix:

    ```http
    GET /api/dynamic/cats
    Content-Type: application/json
    Authorization: Bearer your-token-here
    ```

  </TabItem>
    <TabItem value="sdk" label="JS SDK" default>
    ```js
    // Login as Admin.
    await manifest.login('admins', 'admin@manifest.build', 'password')

    // Login as User entity.
    await manifest.login('users', 'user@example.com', 'password')

    // Then all following requests will have the authorization token in their header until logout.
    const example = await manifest.from('restricted-resource').find()
    ```

  </TabItem>
</Tabs>

### Sign up

Any authenticable entity allows new users to sign up if the [policies](./policies.md) permit it.

<Tabs>

  <TabItem value="rest" label="REST API" default>
    ```http title="Example HTTP Request"
    POST /api/auth/users/signup
    Content-Type: application/json
    {
        "email": "user@example.com",
        "password": "password"
    }
    ```
    ```json title="Example HTTP Response"
    {
        "token": "12345"
    }
    ```

    Same as [login](#login), you can add your token to your requests `Authorization` header using the **Bearer** prefix:

    ```http
    GET /api/dynamic/cats
    Content-Type: application/json
    Authorization: Bearer your-token-here
    ```

  </TabItem>
    <TabItem value="sdk" label="JS SDK" default>
    ```js
    // Sign up as a new user.
    await manifest.signup('users', 'user@example.com', 'password')

    // Then all following requests will have the authorization token in its header until logout.
    const example = await manifest.from('restricted-resource').find()
    ```

  </TabItem>
</Tabs>

:::info

It is not possible to sign up as an **admin**. If you want to create more admins, do it from the admin panel.

:::

### Get current user

Get the current logged-in user.

<Tabs>

  <TabItem value="rest" label="REST API" default>
    ```http title="Example HTTP Request"
    GET /api/auth/contributors/me
    Content-Type: application/json
    Authorization: Bearer your-token-here
    ```
    
    ```json title="Example HTTP Response"
    {
      id: 1,
      email: 'contributor@test.com'
    }
    ```

  </TabItem>
    <TabItem value="sdk" label="JS SDK" default>
    ```js
    // Get the current user (logged as Contributor entity).
    const me = await manifest.from('contributors').me()
    ```

  </TabItem>
</Tabs>

### Logout

Logout removes the token from future request headers.

<Tabs>

  <TabItem value="rest" label="REST API" default>
    Reset the `Authorization` header as you usually do, and you are good to go!
  </TabItem>
    <TabItem value="sdk" label="JS SDK" default>
    ```js
    // All future calls will lose the "Authorization" header.
    await manifest.logout()
    ```

  </TabItem>
</Tabs>

## API Policies

API Policies ensure that we provide specific access to resources for users. They are a way to implement **Authorization** following the RBAC (Role-Based Access Control) method. Indeed it is possible to create different entities (ex: User, Manager...) with different access to resources.

Policies can be added to [entities](./entities.md) or [endpoints](./endpoints.md).

### Entity rules

Each entity has **5 rules** where one or several access policies can be applied:

- **create**: create a new item
- **read**: see the detail and the list of items
- **update**: update an existing item
- **delete**: delete an existing item
- **signup**: sign up as a new user (only for [authenticable entities](./auth.md#authenticable-entities))

### Access policies

The policies for each rule can be added to each [entity description](./entities.md) as shown below:

```yaml
entities:
  Invoice 🧾:
    properties:
      - number
      - { name: issueDate, type: date }
    policies:
      create:
        - { access: restricted, allow: User }
      update:
        - access: admin
      delete:
        - access: forbidden
```

In this case, everyone can see the **Invoice** items, only logged-in **Users** can create new ones. Updating an Invoice is restricted to [Admins](./auth.md#admins) only and no one can delete them (not even Admins).

:::warning
If **no policy** is specified for a rule, **the access is public** for the related action, thus anyone can manage records.

The only exception is the **update** of [single entities](./entities.md#singles) that is set to `admin` by default for convenience.
:::

| Prop       | Description                                                               | Type               |
| ---------- | ------------------------------------------------------------------------- | ------------------ |
| **access** | The type of access: **public**, **restricted**, **admin**, **forbidden**  | AccessType         |
| **allow**  | Only for **restricted** access: the entity (or entities) that have access | string \| string[] |

#### Access types

There are 4 possible access types:

| Access         | Description                                                                                                                                                                                     | Short version (emoji) |
| -------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------- |
| **public**     | Everyone has access                                                                                                                                                                             | 🌐                    |
| **restricted** | Only logged-in users have access to it. If _allow_ key specifies one or several entities, users logged in as other entities will not have access. Admins always have access to restricted rules | 🔒                    |
| **admin**      | Only [admins](./auth.md#admins) have access                                                                                                                                                     | 👨🏻‍💻                    |
| **forbidden**  | No one has access, not even admins                                                                                                                                                              | 🚫                    |

#### Additional examples

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
```
