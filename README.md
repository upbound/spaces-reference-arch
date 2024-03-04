# Deprecation notice

Go to [Spaces reference architecture](https://github.com/upbound/spaces-reference-architecture) for a new repository containing a reference architecture describing best practices for deploying a self-hosted Spaces

# Upbound reference architecture for Spaces

This reference implementation demonstrates the recommended starting (baseline) infrastructure architecture for a multi-control plane architecture built with [Upbound Spaces](https://docs.upbound.io/spaces/overview/). This repository's ReadMe walks through the architecture verbosely to help you understand each component of this cluster. The goal is to teach you about each layer and provide you with the knowledge necessary to apply it to your workload.

## Core architecture components

- This repository contains three manifests for control planes to be deployed in a single Space. Each control plane resource manifest is found in the [infrastructure](infrastructure) folder.
- Each control plane is configured to have a single source of truth, contained in the [controlplanes](controlplanes) folder.
- A Crossplane configuration package, containing a custom API for a `VirtualMachine` abstraction for AWS, is found in the [apis](apis) folder.

## Deploy the reference architecture

### Fork this repository

This repository provides baseline manifests for three control planes and a custom set of Crossplane APIs. You should fork this repository so you can experiment and tweak values.

### Set up a Spaces cluster

Spaces are hosted in Kubernetes clusters. Deploy a Kubernetes cluster in your own cloud environment and install a Space into it. Upbound offers quick start tutorials for:

- [Deploying a Space into AKS](https://docs.upbound.io/spaces/quickstart/azure-deploy/)
- [Deploying a Space into GKE](https://docs.upbound.io/spaces/quickstart/gcp-deploy/)
- [Deploying a Space into EKS](https://docs.upbound.io/spaces/quickstart/aws-deploy/) 

### Configure the CI pipeline

Every time a new commit is pushed in the `apis` folder, this repository is configured with a GitHub Action to build and push a new Configuration package. This repository is configured to publish the defined Configuration package to an Upbound-owned demo account, `Acme Co`.

After you clone this repository, you need to update the [CI job](.github/workflows/ci.yml) to point at your own repository on the [Upbound Marketplace](https://marketplace.upbound.io). 

#### Create a robot on Upbound (Service Account)

Follow these steps:

1. If you don't have an Upbound account, [create one first](https://accounts.upbound.io/register?targetProperty=marketplace&returnTo=https%3A%2F%2Fmarketplace.upbound.io%2F).
2. Create a new repository in the Upbound Marketplace and give it a name.
3. Go to your organization account settings. You can travel there via the Upbound Console or Upbound Marketplace by using the account dropdown.
4. Once in your organization settings, select `Robots`.
5. Create a new `Robot`, called "CI," and give it a short description. Make sure to copy the `access ID` and `access token` for use later.
6. Select the newly created robot, select `Team Assignments` tab, and add it to a new team.
7. If you don't have any teams in your organization, create one.
8. Add the robot to the team. In the team where you assigned the robot, click `Create Repository Permission`, giving the team `Admin` privileges on the repository.

#### Update the CI job

Next, you need to update the CI job to publish to your own repository using the robot created in the previous section:

1. In your forked GitHub repo's settings page, click `Secrets and variables -> Actions`.
2. Update the repository secrets named `XPKG_ACCESS_ID` and `XPKG_ACCESS_TOKEN` with the values you saved earlier.
3. Update the repository environment variable named `PACKAGE_NAME` to the repository name you created earlier in the Marketplace. It should look like `acmeco/configuration-acme-api`

Each time you push a commit to the `apis/` folder of this repository, it builds a new package containing your custom APIs. Once the package builds, you can update the version referenced by each of your control plane's source of truths as you like.

### Deploy the managed control planes

This repository defines manifests for three managed control planes in the [infrastructure](infrastructure) folder. To deploy these control planes to your Space, you can either:

1. Install Argo or Flux into the cluster where you installed Spaces, pointing to this folder as a source. 
2. Directly apply the control plane manifests to your Space cluster.

Once the control planes are created, Spaces will do the rest of the work syncing and keeping your control planes' desired states up to date, with one caveat: you need to create secrets in each control plane to provider credentials for the provider packages that get installed on each control plane. Each control plane source defines a `ProviderConfig` that references a secret with the name `upbound-provider-creds` with a key called `creds`. You need to create these.

## What you can do next

If you've made it to this point, you should have your own Space installed in your own cloud environment. You should also have three control planes deployed to manage resources in `dev`, `staging`, and `production` environments. Here are some ways you can experiment with this setup:

### Create and customize control planes

You can create more managed control planes by deploying new manifests in the [infrastructure](infrastructure) folder. You can experiment with having control planes for different purposes: a control plane per tenant or a control plane per cloud account.

### Customize the APIs

This repository defines a custom API tailored to AWS. You can push new commits into the [apis](apis) folder, defining new compositions and XRDs. Your repository will automatically increment and build the version of your Configuration package. 

When you have a new package version, if you want to install it on some of your control planes, you can update the dependency in your control plane source (for example, updating the `spec.package` version at `controlplanes/ctp-dev/configurations/configuration-acme-api`).
