# Control planes

This folder contains the configurations of the three managed control planes defined in the [infrastructure](../infrastructure/) folder. After deploying the control planes under [infrastructure](../infrastructure/), each control plane reads its source configuration from associated directories under this folder. Think of these folders as the source of truth for their respective control plane.

Each control plane source defines a set of Providers, ProviderConfigs, and Configuration package installed on each control plane:

- [ctp-dev](ctp-dev) contains the configuration for the managed control plane managing the dev environment.
- [ctp-staging](ctp-staging) contains the configuration for the managed control plane managing the staging environment.
- [ctp-prod](ctp-prod) contains the configuration for the managed control plane managing the prod environment.
