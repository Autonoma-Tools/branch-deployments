# Branch Deployments: Every PR Gets Its Own Live URL (Here's How)

Companion code for the Autonoma blog post 'Branch Deployments: Every PR Gets Its Own Live URL (Here's How)'.

> Companion code for the Autonoma blog post: **[Branch Deployments: Every PR Gets Its Own Live URL (Here's How)](https://getautonoma.com/blog/branch-deployments)**

## Requirements

A GitHub repository and an account with the corresponding provider (Vercel, Netlify, Railway, Render) or a VPS with SSH access (Docker). GitHub Actions workflows require repository secrets as documented in each file header.

## Quickstart

```bash
git clone https://github.com/Autonoma-Tools/branch-deployments.git
cd branch-deployments
Copy the config or workflow file that matches your hosting provider into your project. Each file is a standalone, production-ready reference. Set the secrets listed in the file header and push a PR to verify the deployment fires.
```

## Project structure

```
branch-deployments/
├── .github/
│   └── workflows/
│       ├── docker-preview.yml      # Per-PR Docker builds pushed to GHCR + deployed to a VPS via Traefik
│       └── railway-preview.yml     # Per-PR Railway environments created and torn down automatically
├── netlify/
│   └── netlify.toml                # [context.production], [context.deploy-preview], [context.branch-deploy] blocks
├── render/
│   └── render.yaml                 # Top-level `previews` block with per-PR environment overrides
├── vercel/
│   └── vercel.json                 # Preview-scoped env refs, headers, rewrites, and redirects
├── LICENSE
└── README.md
```

Each provider directory / workflow file is a standalone reference — pick the one that matches your stack and copy it into the root of your project.

## What's inside

| File | Provider | What it does |
|------|----------|--------------|
| `vercel/vercel.json` | Vercel | Declares build, env references (including preview-scoped secrets), response headers, rewrites, and redirects. |
| `netlify/netlify.toml` | Netlify | Defines `[context.production]`, `[context.deploy-preview]`, and `[context.branch-deploy]` blocks with per-context env vars, build commands, and noindex headers for previews. |
| `.github/workflows/railway-preview.yml` | Railway | Creates a per-PR Railway environment on open/synchronize, deploys the service, posts the URL to the PR, and deletes the environment on close. |
| `render/render.yaml` | Render | Blueprint with `previews.generation: automatic`, `expireAfterDays`, and `previewValue` / `previewValueFrom` overrides for every env var. |
| `.github/workflows/docker-preview.yml` | Docker + any VPS | Builds an image tagged per PR, pushes to GHCR, and deploys via SSH with Traefik labels for automatic wildcard subdomain + TLS. Tears down on close. |

## About

This repository is maintained by [Autonoma](https://getautonoma.com) as reference material for the linked blog post. Autonoma builds autonomous AI agents that plan, execute, and maintain end-to-end tests directly from your codebase.

If something here is wrong, out of date, or unclear, please [open an issue](https://github.com/Autonoma-Tools/branch-deployments/issues/new).

## License

Released under the [MIT License](./LICENSE) © 2026 Autonoma Labs.
