---
id: authentication
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Authentication

Authentication is the process of proving that people are who they say they are.

Manifest uses **JSON Web Tokens (JWT)** to do that. When you log in, you basically create a new **token** that you use in your next requests to prove your identity. This allows us to use [Policies](./policies.md) to grant or deny the access to some resources based on the user characteristics.

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
  🤒 Patient:
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
  <TabItem value="rest" label="REST API" default>
    ```http
    // Login as Admin.
    POST /api/auth/admins/login
    Content-Type: application/json
    {
        "email": "admin@manifest.build",
        "password": "password"
    }

    // Response.
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
</Tabs>

### Sign up

Any authenticable entity allows new users to sign up if the [policies](./policies.md) permit it.

<Tabs>
  <TabItem value="sdk" label="JS SDK" default>
    ```js
    // Sign up as a new user.
    await manifest.signup('users', 'user@example.com', 'password')

    // Then all following requests will have the authorization token in its header until logout.
    const example = await manifest.from('restricted-resource').find()
    ```

  </TabItem>
  <TabItem value="rest" label="REST API" default>
    ```http
    // Sign up as a new user.
    POST /api/auth/users/signup
    Content-Type: application/json
    {
        "email": "user@example.com",
        "password": "password"
    }

    // Response.
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
</Tabs>

:::info

It is not possible to sign up as an **admin**. If you want to create more admins, do it from the admin panel.

:::

### Get current user

Get the current logged-in user.

<Tabs>
  <TabItem value="sdk" label="JS SDK" default>
    ```js
    // Get the current user (logged as Contributor entity).
    const me = await manifest.from('contributors').me()
    ```

  </TabItem>
  <TabItem value="rest" label="REST API" default>
    ```http
    GET /api/auth/contributors/me
    Content-Type: application/json
    Authorization: Bearer your-token-here

    // Response.
    {
      id: 1,
      email: 'contributor@test.com'
    }
    ```

  </TabItem>
</Tabs>

### Logout

Logout removes the token from future request headers.

<Tabs>
  <TabItem value="sdk" label="JS SDK" default>
    ```js
    // All future calls will lose the "Authorization" header.
    await manifest.logout()
    ```

  </TabItem>
  <TabItem value="rest" label="REST API" default>
    Reset the `Authorization` header as you usually do, and you are good to go!
  </TabItem>
</Tabs>
