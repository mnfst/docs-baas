# Manifest AI rules

Rules are predefined guidelines and constraints that steer an AI assistant’s behavior.

They specify the style, conventions, best practices, or security policies to follow.

Rules ensure consistency, quality, and compliance of the generated suggestions.

## Usage

Running the install command above will add the rules to the Manifest rules in your project.

```bash
# Example: install for Cursor AI Code Editor
npm create manifest --cursor
```

You can replace `--cursor` with another option if you're using a different AI tool;

- `--copilot` if you're using GitHub Copilot
- `--windsurf` for Windsurf

If you need to install rules manually, download the file below and add it to your AI code editor:

<a href="https://raw.githubusercontent.com/mnfst/rules/refs/heads/main/src/rules.md" download>
  ⬇️ Download Manifest Rules
</a></p>

## Types and OpenAPI Generation

When you run Manifest, it generates key context files:

- OpenAPI: A .yml file with the available endpoints, schemas, and the API base URL of your backend.
- Types: A `.ts` file with types for each entity.

The AI editor rules are designed to pick up these files automatically.

By including these context files, Manifest ensures your AI code editor has full visibility into your API, enabling smarter and more reliable code generation.
