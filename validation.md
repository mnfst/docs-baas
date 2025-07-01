---
id: validation
title: Validation
description: Server-side validation for your user-generated content. Use many validators like "required", "minLength", "contains" or "matches" for every property.
---

# Validation

## Introduction

Implementing **server-side validation** is very easy with Manifest.

You can use the built-in **custom validators** to ensure that the data you are receiving is correctly formatted.

## Syntax

In your **manifest.yml**, you can add a _validation_ object that lists the properties and their validators:

```yaml
entities:
  Dog:
    properties:
      - name
      - { name: age, type: number }
    validation:
      name: { minLength: 3 } # The name should have at least 3 characters.
      age: { min: 1, max: 30 } # Age should be a number between 1 and 30.
```

:::tip Tip
**Type validation** is natively implemented with [property types](./entities.md#property-types). You do not need to set it.

Ex: sending a _string_ value for a [boolean](./entities.md#boolean) property type will throw an error.
:::

## Inline syntax

If you want to keep it short you can simply pass the **validation** object to the **property object**:

```yaml
entities:
  Author 🧑🏽‍🦱:
    properties:
      - name
      - { name: email, type: email, validation: { isNotEmpty: true } }
      - { name: bio, type: textarea, validation: { maxLength: 300 } }
```

Both syntaxes (block and inline) can be used simultaneously. In case of declaration conflict, the **inline** declaration will prevail.

## Validate optional properties

If you have some values that are optional **but** that need to be validated **if present**, you can add the `isOptional` validator. The value will be compared against validators only if not _undefined_ or _null_. Example:

```yaml
entities:
  Employee:
    properties:
      - name
      - {
          name: email,
          type: email,
          validation: { contains: '@company.com', isOptional: true }
        } #  If provided, the email should contain "@company.com"
```

## Validation response

If the validation fails, the response will list the validation error(s) in its body:

```http
// Create a new Employee
POST /api/dynamic/employees
Content-Type: application/json
{
    "name": "John Doe",
    "email": "john@manifest.build"
}

// Response.
[
    {
        "property": "email",
        "constraints": {
            "contains": "The value must contain @company.com"
        }
    }
]
```

## Available validators

| Validator        | Type    | Description                                                                          |
| ---------------- | ------- | ------------------------------------------------------------------------------------ |
| `required`       | boolean | Indicates whether the property must not be empty.                                    |
| `isDefined`      | boolean | Checks if value is defined (!== undefined, !== null).                                |
| `isOptional`     | boolean | Checks if given value is empty (=== null, === undefined) and ignores all validators. |
| `equals`         | any     | Checks if value equals ("===") comparison.                                           |
| `notEquals`      | any     | Checks if value not equal ("!==") comparison.                                        |
| `isEmpty`        | boolean | Indicates whether the property can be empty.                                         |
| `isNotEmpty`     | boolean | Indicates whether the property must not be empty. Alias for `required`               |
| `isIn`           | array   | Checks if value is in an array of allowed values.                                    |
| `isNotIn`        | array   | Checks if value not in an array of disallowed values.                                |
| `min`            | number  | The minimum value or length allowed for the property.                                |
| `max`            | number  | The maximum value or length allowed for the property.                                |
| `contains`       | string  | Checks if string contains the seed.                                                  |
| `notContains`    | string  | Checks if string does not contain the seed.                                          |
| `isAlpha`        | boolean | Checks if the string contains only letters (a-zA-Z).                                 |
| `isAlphanumeric` | boolean | Checks if the string contains only letters and numbers.                              |
| `isAscii`        | boolean | Checks if the string contains ASCII chars only.                                      |
| `isEmail`        | boolean | Checks if the string is an email.                                                    |
| `isJSON`         | boolean | Checks if the string is valid JSON.                                                  |
| `minLength`      | number  | Checks if the string's length is not less than given number.                         |
| `maxLength`      | number  | Checks if the string's length is not more than given number.                         |
| `matches`        | string  | Checks if string matches the pattern.                                                |
| `isMimeType`     | boolean | Checks if the string matches to a valid MIME type format.                            |
