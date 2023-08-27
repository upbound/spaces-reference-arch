# Upbound reference architecture for Spaces

This reference implementation demonstrates the recommended starting (baseline) infrastructure architecture for a multi-control plane architecture built with [Upbound Spaces](https://docs-git-spaces-staging-upboundio.vercel.app/spaces/overview/). This repository's ReadMe walks through the architecture verbosely to help you understand each component of this cluster. The goal is to teach you about each layer and provide you with the knowledge necessary to apply it to your workload.

## Core architecture components

- This repository contains three manifests for control planes to be deployed in a single Space. Each control plane resource manifest is found in the [infrastructure](infrastructure) folder.
- Each control plane is configured to have a single source of truth, contained in the [controlplanes](controlplanes) folder.
- A Crossplane configuration package, containing a custom API for a `VirtualMachine` abstraction, is found in the [apis](apis) folder.

## Set up your own environment

To mimic the setup of this repository, you should:

1. Fork this repository into your own account.
2. Deploy a Kubernetes cluster to install Spaces v1.0.0 into. Follow the [Spaces quickstart](https://docs-git-spaces-staging-upboundio.vercel.app/spaces/quickstart/aws-deploy/) to get set up.
3. Configure the Kubernetes cluster hosting your Spaces install with Flux or Argo.
4. Configure Flux/Argo to deploy the control plane manifests found in the [infrastructure](infrastructure) folder. Once the control planes are created, Spaces will do the rest of the job syncing and keeping your control planes' desired states up to date.

## Configure your CI pipeline

This repository is configured to publish to an Upbound-owned demo account, `Acme Co`. Every time a new commit is pushed in the `apis` folder, this repository is configured with a GitHub Action to build and push a new Configuration package.

After you clone this repository, you need to update the [CI job](.github/workflows/ci.yml) to point at your own repository on the [Upbound Marketplace](https://marketplace.upbound.io). 

### Create a robot on Upbound (Service Account)

Follow these steps:

1. If you don't have an Upbound account, [create one first](https://accounts.upbound.io/register?targetProperty=marketplace&returnTo=https%3A%2F%2Fmarketplace.upbound.io%2F).
2. Create a new repository in the Upbound Marketplace and give it a name.
3. Go to your organization account settings. You can travel there via the Upbound Console or Upbound Marketplace by using the account dropdown.
4. Once in your organization settings, click on `Robots`.
5. Create a new `Robot`, called "CI", and give it a short description. Make sure to copy the `access ID` and `access token` for use later.
6. Click on the newly created robot, click on `Team Assignments` tab, and add it to a new team.
7. If you don't have any teams in your organization, create one.
8. Add the robot to the team. In the team where you assigned the robot, click `Create Repository Permission`, giving the team `Admin` privileges on the repository.

### Update the CI job

Next, you need to update the CI job to publish to your own repository using the robot created in the previous section:

1. In your forked GitHub repo's settings page, click `Secrets and variables -> Actions`.
2. Update the repository secrets named `XPKG_ACCESS_ID` and `XPKG_ACCESS_TOKEN` with the values you saved earlier.
3. Update the repository environment variable named `PACKAGE_NAME` to the repository name you created earlier in the Marketplace. It should look like `acmeco/configuration-acme-api`

Each time you push a commit to the `apis/` folder of this repository, it will now build a new package containing your custom APIs. Once the package builds, you can update the version referenced by each of your control plane's source of truths as you like.