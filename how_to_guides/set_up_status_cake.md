# How to set up status cake with your service

This guide will include setting up notifications to email addresses and Slack channels, and the team may opt to include mobile phone alerts as well.

## Request account access

The first step is to request access using your .education.gov.uk email address on service now. You can use [this link](https://dfe.service-now.com.mcas.ms/serviceportal?id=sc_cat_item&sys_id=e7a004df1b399c502fe864606e4bcb21) to request access. Make 2 separate requests, 1 for `Request Account Access` and another for `Request API key`. The API key may not be needed to set up a basic uptime test but may be required for more advanced scenarios.

## Set up a health check endpoint in your project

Status cake needs a URL endpoint to make requests to. You may need to create a suitable page it can request that won't interfere with your running site. For example in `ASP.NET CORE` you can add some middleware to the startup class to create an endpoint such as below.

In the `ConfigureServices` method:

    services.AddHealthChecks();

In the `Configure` method:

    app.UseEndpoints(endpoints =>
            {
                . . .
                endpoints.MapHealthChecks("/health").WithMetadata(new AllowAnonymousAttribute());
            });

For example this creates an endpoint for `/health` that allows anonymous access but your situation may be different.

You can test this endpoint for example with `curl https://<yourdomain>/health` and confirm that it returns a result of `Healthy`


## Create the uptime test in status cake

When your account has been set up and you've had a confirmation email, follow the steps below to set up the test in status cake.

### Log in to status cake

Go to [statuscake.com](https://app.statuscake.com/Login/) and create a new account using your education.gov.uk email address.

Hover the mouse over your email address in the top right of the page and you will get the option to select a Sub Account, select DfeStatusCake.

This should take you to the page with all DfE tests.

### Create a slack integration

- Ensure you have a slack channel you want to use for alerts or create a new one (if you do not have permissions to create one, ask in the digital-tools-support slack channel)
- On the `ALERTING` menu on the left click `Integrations`
- Assuming an integration doesn't already exist, at the top of the page select Type = Slack
- Click Add to Slack
- Select the channel to send alerts to and click Allow
- This should set up a new integration that will be added to the bottom of the list. Send a test alert by clicking the relevant button and confirm it works ok

### Create a new Contact Group

- On the `ALERTING` menu on the left click `New Contact Group`
- Give the name a suitable name using the naming convention `team-subteam` e.g. SDD-Manage an academy transfer
- Add emails to be notified as required. Note that whilst testing you should use a suitable personal email address such as your education.gov.uk email address, but for production scenarios a team-wide email should be used so that the notification isn't tied to an account and multiple people will receive the notification
- In slack integrations select the integration you created in the step above
- Click Save Now

### Create new uptime test

- On the `MONITORING` menu on the left click `New Uptime Test`
- Enter the url of the healthcheck endpoint
- Give the test a suitable name using the naming convention `team-subteam` e.g. SDD-Manage an academy transfer
- Select the contact group listed above
- Select the desired check rate, a common inrterval is 5 minutes
- Click Save now
- Confirm works ok and you can view the test and results


## Troubleshooting

### Cannot save test - get page with `CAN NOT SAVE`

This is a permissions issue that should now have been resolved for any new accounts, however if you get this it most likely means your account only has View only access. To resolve request Edit/Save access by emailing the master account email address at DfEStatusCake@education.com and they will be able to grant you relevant access.





