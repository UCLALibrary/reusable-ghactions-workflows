# Reusable GitHub Actions Workflows for UCLA Library Repositories

This repository contains UCLA-specific [reusable GitHub Actions workflows](https://docs.github.com/en/actions/using-workflows/reusing-workflows) that can be used within any UCLALibrary org repository.

## Why have Reusable Workflows?

Instead of copy/pasting an identical GitHub Actions workflow across multiple repositories, you can create a "reusable workflow" to call from as many repository GitHub Actions configurations as necessary. 

This is useful when there are parameters in a workflow that periodically change, and using environment variables is not supported for that parameter. 

For example, in the `uses` parameter of a jobs step, you specify the name of an action that you want use for that step. If that action name requires a version number, e.g. `actions/checkout@v3` this cannot be referenced via an environment variable. The `uses` parameter does not support variables. Assuming you had the same actions workflow in multiple repositories, you'd have to edit each repository to change the values in the `uses` parameter. 

Instead you can convert this into a reusable workflow, that is called from multiple repositories, and you'd only have to make the change once. 

## Available Workflows

* [`cm_helm_chart_push.yml`](./.github/workflows/cm_helm_chart_push.yml)

This workflow pushes a Helm chart into the UCLA Library's ChartMusuem repository. 

It installs a specified version of the Helm client, installs the ChartMuseum helm plugin, and pushes the repository's chart up to ChartMuseum. 

Requires the following environment variables:

* `CM_PUSH_HELM_VERSION` - version of Helm client to install
* `CM_BASICAUTH_USERNAME` - (secret) HTTP Basic Auth username for UCLA Library ChartMuseum push access
* `CM_BASICAUTH_PASSWORD` - (secret) HTTP Basic Auth password for UCLA Library ChartMuseum push access

**Usage**
```yaml
jobs:
  push_to_chart_museum:
    uses: UCLALibrary/reusable-ghactions-workflows/.github/workflows/cm_helm_chart_push.yml@main
    with: 
      CM_PUSH_HELM_VERSION: ${{ vars.CM_PUSH_HELM_VERSION }}
    secrets: inherit
```