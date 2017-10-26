# Overview

Webhooks allow GitHub to notify our Jenkins CI/CD service when 
a change must be built, tested and deployed to our Quay registry, such as
whenever a PR is submitted or merged. By configuring "Collaborators &
teams" you can ensure your project can take advantage of peer review from the
start.

The following assumes a repository that's been duplicated according to
the [README](../README.md) instructions.

## Configure Collaborators & Teams

From the 'Settings' section, go to the "Collaborators & teams" tab, then
add "commontools" as a team with admin privileges (required for
[Slack](./docs/slack.md) notifications) and "kraken-reviewers" as a team
with write privileges (required for [CODEOWNERS](./CODEOWNERS)).

<p align="center">
  <img src="https://github.com/NancyHarvey/solas/blob/master/docs/images/github/GitHub%20Teams_edited.png" width="900" title="GitHub teams">
</p>

## Jenkins Webhooks

Within your GitHub repository, go to Settings and add the following two webhooks. 

### Normal Webhook

* URL: https://common-jenkins.kubeme.io/github-webhook/
* Select "Send me everything"

### Pull-Request-Builder Webhook

* URL: https://common-jenkins.kubeme.io/ghprbhook/
* Select "Let me select individual events" and choose:
  * Issue comment
  * Pull Request

![screenshot](images/github/github-selective-webhook.png)
