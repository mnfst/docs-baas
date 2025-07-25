---
id: groups
title: Groups (nested entities)
slug: groups
description: TODO
---

# Groups (nested entities)

A group is a **reusable set of properties** that you can attach to your entities: whether they are [single](./entities.md#singles) or [collections](./entities.md#collections). They help you structure your nested data without having to repeat yourself. Some common use cases for groups are:

- Testimonials
- FAQs
- Widgets (CTA, UI elements like cards, blocks etc.)

Groups are automatically loaded when you fetch a list or a detail version of the parent entity (using both REST API and SDK).

## Syntax

Define your groups in the `manifest.yml` file to use it in entities as a property.

```yaml title="manifest.yml"
entities:
  Service:
    properties:
      - name
      - { name: description, helpText: 'Description of the service' }
      - {
          name: testimonials,
          type: group, # Group type indicates that it is a group.
          helpText: 'Add some testimonials for the service',
          options: { group: Testimonial } # Pass the name of the group in the "group" option
        }

groups:
  Testimonial: # We create a testimonial group with 3 properties.
    properties:
      - { name: author, type: text }
      - { name: content, type: text }
      - { name: rating, type: number, helpText: '1 to 5 stars' }
    validation:
      rating: { isNotEmpty: true }
```

The code above will add testimonials to service. Once the `Testimonial` group is created, you can add it in as many entities as you want.

## Non-multiple groups

In the example above, each service can have several testimonials, but you can also have non-multiple groups.

This example shows a backend for a website where each page has a single "call to action" element that can be used as widget UI.

```yaml title="manifest.yml"
  Homepage:
    single: true
    properties:
      - { name: title, type: text }
      - {
          name: callToAction,
          type: group,
          options: { group: CallToAction, multiple: false }
        }

  AboutPage:
    single: true
    properties:
      - { name: title, type: text }
      - { name: description, type: text }
      - {
          name: callToAction,
          type: group,
          options: { group: CallToAction, multiple: false }
        }

groups:
  CallToAction:
    properties:
      - { name: title, type: string }
      - { name: description, type: text }
      - { name: buttonText, type: string, helpText: "Text for the button" }
      - { name: buttonLink, type: link }
    validation:
      title: { isNotEmpty: true }
      description: { isNotEmpty: true }
```
