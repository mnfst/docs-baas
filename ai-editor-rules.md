# AI Editor Rules

This guide explains how to obtain and install the Manifest rules file for your AI code editor.

## Rules Integration

Manifest already provides one rule file per supported editor. It’s installed automatically during the Manifest CLI setup when you specify your editor:

```bash
# Example: install for Cursor AI Code Editor
yarn create manifest server --cursor
```

Check the [Get Started](/docs/#install-manifest) guide for full documentation on built‑in integration.

<p>

## Rules for AI code editors

If you need to install rules yourself, download the file for your editor and add it to your AI code editor:

<a href="https://raw.githubusercontent.com/mnfst/rules/refs/heads/main/src/rules.md" download>
  Download Manifest rules
</a></p>

## Generated API & Type Definitions

When you run Manifest (e.g., via npm run start or the CLI), it generates key context files:

- OpenAPI Specification (`./manifest/openapi.yml`): Contains all available endpoints, parameters, schemas, and the API base URL.
- TypeScript Definitions (`./manifest/types.ts`): Provides typed interfaces for your API, which you can copy into your client folder for accurate code completion and type safety.

The AI editor rules are designed to pick up these files automatically, guiding the AI agent to:

1. Read the OpenAPI spec to understand your backend’s capabilities.
2. Use the generated types to produce valid, type-safe client code.

By including these context files, Manifest ensures your AI code editor has full visibility into your API, enabling smarter and more reliable code generation.

This mechanism streamlines the creation of cohesive full-stack applications by ensuring your frontend and backend share a unified understanding of endpoints and types, reducing communication errors and speeding up development.
