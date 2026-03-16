# Rancher UI Extension for Evroc

This repository is a scaffold for the Evroc Rancher UI extension.

Purpose:

- provide a `cloud-credential` component for the `evroc` node driver
- provide a `machine-config` component for the `evroc` node driver
- improve the Rancher UX over the raw machine-driver flags exposed by `docker-machine-driver-evroc`

Current status:

- scaffolded extension package layout
- minimal `cloud-credential/evroc.vue`
- minimal `machine-config/evroc.vue`
- extension entrypoint with `importTypes`
- reusable Rancher GitHub workflows for chart and catalog publication
- repo-shaped structure for a separate GitHub repository

What is not done yet:

- build tooling validation against a live Rancher extension dev environment
- packaging and publishing flow
- API-backed dropdowns for regions, zones, profiles, and images
- credential test flow through Rancher's proxy APIs

## Structure

- `pkg/evroc/package.json`: extension package metadata
- `pkg/evroc/index.ts`: extension entrypoint
- `pkg/evroc/cloud-credential/evroc.vue`: Evroc auth fields
- `pkg/evroc/machine-config/evroc.vue`: Evroc machine configuration fields
- `package.json`: root workspace metadata for local development
- `tsconfig.json`: minimal TypeScript config

## Intended Rancher Integration

The extension ID must match the node driver name:

- `evroc`

According to Rancher extension docs, placing the components in:

- `cloud-credential/evroc.vue`
- `machine-config/evroc.vue`

allows them to be automatically registered for the `evroc` driver, as long as the package entrypoint calls `importTypes`.

Sources:

- https://extensions.rancher.io/extensions/next/usecases/node-driver/cloud-credential
- https://extensions.rancher.io/extensions/v2/api/components/node-drivers
- https://extensions.rancher.io/extensions/v2/usecases/node-driver/machine-config

## Next Steps

1. Enable GitHub Pages and create a `gh-pages` branch in the repo.
2. Push the repo and create a tagged GitHub release using the extension package version tag format: `evroc-0.1.5`.
3. Validate the UI against a live Rancher instance.
4. Add proxy-backed credential testing and dynamic option loading.
