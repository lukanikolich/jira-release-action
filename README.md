# Jira Release Action (GitHub Action)

## Description

The **Jira Release Action** GitHub Action automates the process of creating a new release in Jira by scanning your repository's commit history for Jira issue keys. These issues are then associated with the new release, streamlining your release management process in Jira.

This action was inspired by the [create-jira-release](https://github.com/GeoWerkstatt/create-jira-release) action but extends its functionality by adding the ability to specify a configurable `base-ref`. This allows you to choose a specific branch or tag as the base reference for comparing commits, enhancing flexibility in different workflow scenarios.

## Inputs

| key                             | description                   | required |
|---------------------------------|-------------------------------|----------|
| `jira-project-key`              | The key of the Jira project used to identify related issues (e.g., `PROJ`).   | true     |
| `jira-automation-webhook`       | The URL of the Jira automation webhook that will be triggered<br> to create the release.             | true     |
| `build-version`                 | The version identifier for the Jira release (e.g., `1.0.0`).                     | true     |
| `base-ref`                      | The base reference (e.g., a branch or tag) to compare against<br> for determining which commits are relevant for the release.<br> If this input is not provided The latest tag in the repository<br> will be used as the base reference. If there are no tags,<br> the comparison will default to the very first commit in the repository | false |

## Jira setup

![image](https://github.com/user-attachments/assets/382d1e41-b40f-4bff-8db5-d5da3490c367)
This image demonstrates a Jira automation rule triggered by an incoming webhook. The webhook is configured to:

- **Create a version:** Using the `{{webhookData.version}}` field, the rule creates a new version in Jira.
- **Filter issues with JQL:** The rule uses the `{{webhookData.issueKeys}}` field to filter issues by their keys.
- **No issues from the webhook:** The webhook configuration is set to not rely on issues provided by the webhook directly but uses the incoming data to perform actions.

## Usage

Below is an example of how to use this action in your GitHub workflow:

```yaml
steps:
  - name: Checkout repository
    uses: actions/checkout@v2

  - name: Create Jira Release
    uses: lukanikolich/jira-release-action@v1
    with:
      jira-project-key: "PROJ"
      jira-automation-webhook: ${{ secrets.JIRA_AUTOMATION_WEBHOOK }}
      build-version: ${{ github.ref_name }}
      base-ref: "main"  # Optional
