# Firely Helm Repository

<img src="https://fire.ly/wp-content/themes/firely/images/logo.svg" alt="Firely" width="200"/>

This is a [Helm](https://helm.sh/) charts repository for Kubernetes made by [Firely](https://fire.ly).

## Add the Firely Helm repository

```bash
helm repo add firely https://firelyteam.github.io/Helm.Charts
```

## Install Firely Server

```bash
helm upgrade --install firely-server firely/firely-server
```
For more details on installing Firely Server please see the chart's README.

## Install Firely Auth

```bash
helm upgrade --install firely-auth firely/firely-auth
```
For more details on installing Firely Auth please see the chart's README.
