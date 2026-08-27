# Statespace documentation

Documentation for Statespace, built with Mintlify.

## Preview

Install the current Mintlify CLI.

```bash
npm install --global mint
```

Start the local documentation server.

```bash
mint dev
```

Open `http://localhost:3000`.

## Validate

Check the configuration and content before publication.

```bash
mint validate
mint broken-links
```

Pushes and pull requests run both checks in GitHub Actions.

## Structure

The documentation follows five sections.

- Getting started introduces the interfaces.
- Concepts explains experiment theory.
- Experiment workflow follows the production lifecycle.
- Examples provides complete recipes.
- Reference documents exact interfaces and schemas.

## Content status

Pages describe either current behavior or proposed UX. Every page for an unimplemented feature must start with a visible planned-feature warning.

## License

MIT
