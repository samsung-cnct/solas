# Overview

This document details the steps to implement our CI/CD pipeline with GitLab. Each GitHub repo is linked to one GitLab project that builds, tests and then deploys an artifact to Quay, such as a container or a chart.

The following steps assume you have already duplicated a repo according to the
[README](../README.md) instructions and in conformance with [GitHub](./github.md)
and [Quay](./quay.md) guidelines.

## Edit gitlab-ci.yml file

* Edit the `chart_name` or `image_name` as well as the `robot_account` in the `variables` section of the gitlab-ci.yml file.

  * For container repositories:

`image_name: "zabra-container"`

`robot_account: "zabra_robot"` (the name of the robot created during the [Quay](./quay.md) configuration)

  The resulting container image will be deployed to the `quay.io` container
  repository at https://quay.io/application/samsung_cnct/zabra-container?namespace=samsung_cnct .

  * For chart repositories:

`chart_name: "zabra"`

`robot_account: "zabra_robot_rw"` (the name of the robot created during the [Quay](./quay.md) configuration)


  The resulting Helm chart will be deployed to the `quay.io` app
  repository at https://quay.io/application/samsung_cnct/zabra?namespace=samsung_cnct.

## Chart Repositories Only: Edit the Chart.yaml.in file

* This file is located in the repository's `build` folder

* Edit the `name`, `description`, `home` and `sources`. Do not edit the `version`:

```
name: zabra
version: ${CHART_VER}-${CHART_REL}
description: Sample chart template for registry
keywords:
- kraken
home: https://github.com/samsung-cnct/chart-zabra
sources:
- https://github.com/samsung-cnct/chart-zabra
```

Also add any relevant keywords and look at other
related charts for inspiration. For example, if you're creating a logging chart, you might
look at the [Fluent Bit Chart](https://github.com/samsung-cnct/chart-fluent-bit).

## Configure GitLab

Get set up with GitLab:
[Samsung GitLab workflow best practices](missing link)

TODO: How to integrate Github repo with GitLab on a new repo

### GitLab environment variables
GitLab comes with a list of handy built-in environment variables, some of which are used in the CI file. 
[Reference here](http://docs.gitlab.com/ce/ci/variables/README.html#predefined-variables-environment-variables)

### Define GitLab secrets

  * Head to your solas repo on Gitlab. Go to `Settings` --> `CI/CD` and expand `Secret Variables`.
  ![screenshot](images/gitlab/gitlab-settings.png)
  * Create a `QUAY_PASSWORD` Secret Key and assign it the docker login password of the robot created during the [Quay](./quay.md) configuration. 
  ![screenshot](images/gitlab/gitlab-secrets.png)

### Optional Cleanup stage

Some repositories build very large images, and we may want to clean up any unused branch builds on the GitLab docker image repository. This is currently an upstream work in progress; in the meantime if necessary you can implement it yourself.

First, for a sample cleanup stage, please see the cleanup stage of [container-fluent-bit](https://github.com/samsung-cnct/container-fluent-bit/blob/master/.gitlab-ci.yml). You should be able to copy this verbatim, and then provide some GitLab configurations to make it work:

1. Go to User -> Settings and request a [personal access token](https://docs.gitlab.com/ce/user/profile/personal_access_tokens.html), with API access (read and write)
2. In your Gitlab project repository, follow the steps for [GitLab secrets](#define-gitlab-secrets) to create the following additional secrets:

`DOCKER_USERNAME` - the value should be the name of your personal access token
`DOCKER_PASSWORD` - the value should be the value of your personal access token

These two env variables will be passed into the `docker-registry-curl` tool's `entrypoint` script and allow to remove entries from the registry.