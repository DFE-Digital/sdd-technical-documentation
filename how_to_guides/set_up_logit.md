# How to set up Logit

[Logit](https://logit.io/) is a SaaS product providing log aggregation functionality.

## Logit organisation

We have currently have one Logit license, with multiple stacks created within it. There is currently one stack per application or API, which will contain all logs for all environments.

Logit.io will be used in preference to the Azure Log Analytics for Azure-hosted components, so that there exists a single tool for log aggregation (single pane of glass) across SDD - enabling logs spanning multiple services to be viewed together (e.g. API in Azure, web app in GOV.UK PaaS).

# Setup

Before setting up Logit, it is recommended to use Serilog so that structured (JSON) log output can be easily enabled for all output leaving your service.

The [logging guide](../development_guidance/logging.md) details steps for enabling Serilog.

## 1. Gaining access to your stack(s)

## 2. Setting up on Logit

## 3. Adding Logit log shipping to your Terraform code

GitHub variables:

`syslog-tls://ENDPOINT:PORT` where PORT is the value where type = 'Syslog-SSL' in their configuration page
