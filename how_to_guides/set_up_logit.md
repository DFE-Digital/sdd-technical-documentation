# How to set up Logit

[Logit](https://logit.io/) is a SaaS product providing log aggregation functionality.

## Logit organisation

We have currently have one Logit account for SDD, with multiple stacks created within it. There is currently one stack per application or API, which will contain all logs for all environments.

Logit will be used in preference to the Azure Log Analytics for Azure-hosted components, so that there exists a single tool for log aggregation (single pane of glass) across SDD - enabling logs spanning multiple services to be viewed together (e.g. the common setup of API in Azure, web app in GOV.UK PaaS).

# Setup

Before setting up Logit, it is recommended to use Serilog so that structured (JSON) log output can be easily enabled for all output leaving your service.

The [logging guide](../development_guidance/logging.md) details steps for enabling Serilog.

## 1. Gaining access to your stack(s)

Access is granted via the `#digital-tools-support` Slack channel in the DfE workspace. 

* If you are in a digital service delivery team (e.g. Make A Change, Apply to Become, Concern Casework) then request access to the stack for your team AND the stack for the Academies API.
* If you are in the Service Support team then request access to the Academies API and any digital services which are supported by the Service Support team (none at the time of writing, A2B External is planned for soon).

## 2. Setting up on Logit

Follow the [GOV.UK PaaS guide](https://docs.cloud.service.gov.uk/monitoring_apps.html#set-up-the-logit-log-management-service) to setup Logit. **DO NOT** follow the section utilizing the `cf` CLI tool under heading '[Configure app](https://docs.cloud.service.gov.uk/monitoring_apps.html#configure-app)', as we will utilize Terraform instead.

## 3. Adding Logit app configuration to your Terraform code

Finally we need to tell GOV.UK PaaS to ship logs to the Logit LogStash endpoint. To do this we will amend our Terraform scripts to setup a CloudFormation [user-provided service](https://docs.cloudfoundry.org/devguide/services/user-provided.html#syslog).

*The following approach is a guide which can be adapted to your specific Terraform setup.*

### a) Terraform variables

The first change is the [introduction of a Terraform input variable](https://github.com/DFE-Digital/academy-transfers-api/commit/eb25fe36b7c7664215bd569d3dc2764604962a5d#diff-b25e8cb262b29f5005a96d0c4a06a3deb77698fa6e11e4f43b21f6f4ad0f45e4) `logit_sink_url` to accept the Logit LogStash URL.

```
variable logit_sink_url {
	type = string
	description = "Target URL (HTTPS) for logs to be streamed to"
}
```

We also define a [local variable](https://github.com/DFE-Digital/academy-transfers-api/blob/eb25fe36b7c7664215bd569d3dc2764604962a5d/terraform/variables.tf#L80) to store the name for the user-provided service.

```
locals {
  app_name_suffix      = var.app_environment
  // other locals elided
  logit_service_name   = "academy-transfers-logit-sink-${local.app_name_suffix}"
}
```

### b) GitHub - Secret to store variable

A simple method to supply this variable value is by utilizing GitHub secrets, so we will define one as such:

Name: `LOGIT_SINK_URL`  
Value: `syslog-tls://ENDPOINT:PORT` 

The values are copied from the Logit LogStash stack configuration page. PORT is the port for the protocol where type is `Syslog-SSL`, ENDPOINT is the LogStash endpoint for your application's stack.

We bind the GitHub secret to our `tf apply` commands as appropriate ([example](https://github.com/DFE-Digital/academy-transfers-api/blob/f947ed9aea6936cdc02f174fdcdb3af20bc80901/.github/workflows/tf-apply.yml#L78)).

### c) Terraform - user provided service

The last step is to create the user provided service and [bind it to the application](https://github.com/DFE-Digital/academy-transfers-api/blob/f947ed9aea6936cdc02f174fdcdb3af20bc80901/terraform/main.tf):

```terraform
resource cloudfoundry_app worker_app {

    // add this
    service_binding {
		service_instance = cloudfoundry_user_provided_service.logit.id
	}
}

// add this
resource cloudfoundry_user_provided_service logit {
	name             = local.logit_service_name
	space            = data.cloudfoundry_space.space.id
	syslog_drain_url = var.logit_sink_url
}
```

After deployment you should see log entries appear in the stack within Logit.