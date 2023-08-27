# Upbound reference architecture for Spaces

This reference implementation demonstrates the recommended starting (baseline) infrastructure architecture for a multi-control plane architecture built with [Upbound Spaces](https://docs-git-spaces-staging-upboundio.vercel.app/spaces/overview/). This repository's ReadMe walks through the architecture verbosely to help you understand each component of this cluster. The goal is to teach you about each layer and provide you with the knowledge necessary to apply it to your workload.

## Core architecture components

- This repository contains three manifests for control planes to be deployed in a single Space. Each control plane resource manifest is found in the [infrastructure](infrastructure) folder.
- Each control plane is configured to have a single source of truth, contained in the [controlplanes](controlplanes) folder.
- A Crossplane configuration package, containing a custom API for a `VirtualMachine` abstraction, is found in the [apis](apis) folder.

## Set up your own environment

To mimic the setup of this repository, you should:

- Fork this repository into your own account.
- Deploy a Kubernetes cluster to install Spaces v1.0.0 into. Follow the [Spaces quickstart](https://docs-git-spaces-staging-upboundio.vercel.app/spaces/quickstart/aws-deploy/) to get set up.
- Configure the Kubernetes cluster hosting your Spaces install with Flux or Argo.

## Configure your CI pipeline

This repository is configured to publish to an Upbound-owned demo account, `Acme Co`. Every time a new commit is pushed in the `apis` folder, this repository is configured with a GitHub Action to build and push a new Configuration package.

After you clone this repository, you need to update the [CI job](.github/workflows/ci.yml) to point at your own repository on the [Upbound Marketplace](https://marketplace.upbound.io). Follow these steps:

1. If you don't have an Upbound account, [create one first](https://accounts.upbound.io/register?targetProperty=marketplace&returnTo=https%3A%2F%2Fmarketplace.upbound.io%2F).
2. Go to your organization account settings. You can travel there via the Upbound Console or Upbound Marketplace by using the account dropdown.