# Statespace documentation

Documentation site for Statespace.

Mintlify builds and serves the site.

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

The documentation has four sections.

- Get started: the introduction, the quickstart, and installation.
- Concepts: how it works, apps and live, experiments, subjects and assignment, events and metrics,
  evidence, overrides, and permissions.
- Guides: CI on GitHub Actions, the proxy on Docker, Cloud Run, and Kubernetes, events and metrics,
  emergency stops, and agents.
- Reference: the CLI, `statespace.toml` with a starter file, the API, CEL variables, commit statuses,
  and limits.

## Content status

Pages describe either current behavior or proposed UX. Every page for an unimplemented feature must start with a visible planned-feature warning.

## License

MIT
