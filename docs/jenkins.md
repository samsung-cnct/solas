# Overview

This document details the steps to implement our CI/CD pipeline with Jenkins. Each GitHub repo contains one Jenkins job that builds, tests and then deploys an artifact to Quay, such as a container or a chart.

The following steps assume you have already duplicated a repo according to the
[README](../README.md) instructions and in conformance with [GitHub](./github.md)
and [Quay](./quay.md) guidelines.

## Edit the [Jenkinsfile](../Jenkinsfile)

* Edit the `container_name` or `image_name` and `robot_secret` at the top of
the [Jenkinsfile](../Jenkinsfile).

  * For container repositories:

```
def image_name            = "zabra-container";
def robot_secret          = "quay-robot-zabra-container-rw"
```

  The resulting container image will be deployed to the `quay.io` container
  repository at https://quay.io/application/samsung_cnct/zabra-container?namespace=samsung_cnct .

  Push these changes to the GitHub repository before proceeding.

  * For chart repositories:

```
def chart_name            = "zabra";
def robot_secret          = "quay-robot-zabra-rw"
```

  The resulting Helm chart will be deployed to the `quay.io` app
  repository at https://quay.io/application/samsung_cnct/zabra?namespace=samsung_cnct.

The secrets defined here were created during the [Quay](./quay.md) configuration.

## Chart Repositories Only: Edit the [Chart.yaml.in](../Chart.yaml.in)

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

Push these changes to the GitHub repository before proceeding.

## Configure Jenkins

### Log in and create the project

* Log in to your Jenkins server
* Create a new project by selecting `New Item` on the Jenkins homepage
  * Enter the repo name in the field at the top (for example, `container-zabra` or `chart-zabra`)
  * Select `Multibranch Pipeline` 
  
  <p align="center">
  <img src="https://github.com/NancyHarvey/solas/blob/ea5c38b5f5210c9635b7850d20b0a346aecdbb5c/docs/images/jenkins/Multibranch_cropped.png" width="700" title="Github Logo">
</p>

### Configure GitHub

* Under `Branch Sources`, add GitHub as a source:
<p align="left">
  <img src="https://github.com/NancyHarvey/solas/blob/master/docs/images/Jenkins%20Branch%20Sources%20Config.png" width="200" title="Github Logo">
</p>

* Configure `Branch Sources` as shown below.
<p align="center">
  <img src="https://github.com/NancyHarvey/solas/blob/master/docs/images/jenkins/Jenkins%20Branch%20Resources%20steps.png" width="700" title="Jenkins Branch Sources steps">
</p>

* Confirm the configuration appears as below, then click `Save`.
<p align="center">
  <img src="https://github.com/NancyHarvey/solas/blob/master/docs/images/jenkins/Jenkins%20Branch%20Sources_completed.png" width="800" title="Jenkins Branch Sources steps Completed">
</p>

### Did you`save`?
