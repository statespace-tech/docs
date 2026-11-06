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

The site has three tabs. `AGENTS.md` has the rules for each page.

- Docs: Get started (overview, quickstart, installation), Concepts, and Guides.
- Runtimes: Docker, Kubernetes, Cloud Run, ECS Fargate, Fly.io, S3, and Cloud Storage.
- Reference: `statespace.toml`, CEL, commit statuses, and the CLI.

The pages describe the current release only.

## License

Apache 2.0. See `LICENSE`.
