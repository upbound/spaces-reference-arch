# Infrastructure

This folder contains the instantiations of managed control planes in a Space deployment. After you've deployed a Space, you can sync this folder and setup GitOps flows around it. This allows you to declaratively drive the lifecycle and configuration of control planes in your Space.

Spaces offers a local API for creating and managing control planes in its environment. The manifests for `ctp-dev`, `ctp-staging`, and `ctp-prod` represent control planes for 3 environments in kind (`dev`, `staging`, and `prod`). Each control plane is configured to:

- propagate it's connection details (kubeconfig) to a secret in the Spaces cluster.
- named according to the environment it's designated to manage.
- configured to sync its source configuration from folders defined in this repository under `/controlplanes/`.