# Jira Release Action (GitHub Action)

## Description

The **Jira Release Action** GitHub Action automates the process of creating a new release in Jira by scanning your repository's commit history for Jira issue keys. These issues are then associated with the new release, streamlining your release management process in Jira.

This action was inspired by the [create-jira-release](https://github.com/GeoWerkstatt/create-jira-release) action but extends its functionality by adding the ability to specify a configurable `base-ref`. This allows you to choose a specific branch or tag as the base reference for comparing commits, enhancing flexibility in different workflow scenarios.

## Inputs

- **jira-project-key** (required):  
  The key of the Jira project used to identify related issues (e.g., `PROJ`).

- **jira-automation-webhook** (required):  
  The URL of the Jira automation webhook that will be triggered to create the release.

- **build-version** (required):  
  The version identifier for the Jira release (e.g., `1.0.0`).

- **base-ref** (optional):  
  The base reference (e.g., a branch or tag) to compare against for determining which commits are relevant for the release. If this input is not provided:
  - The latest tag in the repository will be used as the base reference.
  - If there are no tags, the comparison will default to the very first commit in the repository.

## Usage

Below is an example of how to use this action in your GitHub workflow:

```yaml
name: "Create Jira Release"
on:
  push:
    tags:
      - '*'

jobs:
  create-jira-release:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v2

      - name: Create Jira Release
        uses: your-repo/jira-release-action@v1
        with:
          jira-project-key: "PROJ"
          jira-automation-webhook: ${{ secrets.JIRA_AUTOMATION_WEBHOOK }}
          build-version: ${{ github.ref_name }}
          base-ref: "main"  # Optional
