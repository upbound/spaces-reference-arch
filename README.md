# Upbound reference architecture for Spaces

This reference implementation demonstrates the recommended starting (baseline) infrastructure architecture for a multi-control plane architecture built with [Upbound Spaces](https://docs-git-spaces-staging-upboundio.vercel.app/spaces/overview/). This repository's ReadMe walks through the architecture verbosely to help you understand each component of this cluster. The goal is to teach you about each layer and provide you with the knowledge necessary to apply it to your workload.

## Core architecture components

- A Kubernetes cluster with Spaces v1.0.0 deployed.
- The cluster is configured with Flux.