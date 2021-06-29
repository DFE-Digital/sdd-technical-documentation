# Logging

How do we expect log output to be made available to assist in production monitoring and debugging?

## Tooling

We should follow guidance available from the [GovPaaS Technical Documentation](https://docs.cloud.service.gov.uk/monitoring_apps.html#logs) in terms of which log aggregation framework to utilize. Currently this is [Logit](https://logit.io) when the service is running on GovPaaS, and Application Insights when on Azure. However we place high value in consistency, so will investigate the use of Logit on Azure as well.

### Logit-specific recommendation 

It is recommended to write the log output as JSON for easier parsing and thus searching from Logit. To accomplish this a reference to Serilog can be added and then configured as per the changes made in [this PR](https://github.com/DFE-Digital/academy-transfers-api/pull/97/files).

To enable Logit in GovPaaS, follow the GovPaaS instructions [here](https://docs.cloud.service.gov.uk/monitoring_apps.html#set-up-the-logit-log-management-service).

## Writing helpful log output

Using the `ILogger` abstraction provided as part of the .NET MVC libraries, we should look to write log output with 'future us' in mind. Recommendations which help us with this include:

### Provide detailed log messages

Output what would you like to see when something goes wrong. Then additionally think what output someone else would need to see in order to debug an issue - they lack the context you do.

You may opt to take the logging output for a test drive - exercise the feature and send the log output to a member of your team - could they tell what has happened?

Inclusion of stack traces - use the overload for `LogError` which takes an `Exception` object to include nicely formatted exception information including the stack trace. Don't format your own exception details string unless you have a good reason to.

### Log to appropriate logging levels

1. **Trace** - used to trace flow through code. Consider this is a code smell if used in production. An alternative to stepping through code line-by-line, it is typically not required if following TDD. It is disabled by default when not in a development environment.

   **Example:** stating line 123 is hit, used for debugging why a particular branch in an 'if' statement was executed.

2. **Debug** - the lowest-severity and most-granular level which could be enabled during production if diagnostic information was required to help solve an especially tricky production issue.
   
   **Example:** logging HTTP response body.

3. **Information** - logs that track the normal flow of the application, or system-initiated actions commencing. Useful in a diagnostic context to provide context of what the system was doing just before the error / warning occurred.
   
   **Example:** logging that a scheduled batch job has started, along with pertinent job parameters.

4. **Warning** - logs that something an sub-optimal or unexpected has happened, which should not cause the action to fail. Good warning messages are often under-utilized - but they will allow proper automated alerting, and during troubleshooting allow us to better understand if/how the system was misbehaving before the failure.
   
   **Example:** recording that a request took a long amount of time to complete, or a certain number of retries.
   
5. **Error** - log every error condition at this level. That can be API calls that return errors or internal error conditions. These should indicate a failure in the current activity, not an application-wide failure.
   
   **Example:** failure to invoke a HTTP API after a number of retries.

6. **Critical** - logs that describe an unrecoverable application or system crash. May require manual intervention to fix.
   
   **Example:** application shutdown due to out-of-memory; host machine reboot; inability to bind to inbound network address.

#### Utilize structured logging

This is an easy way to attach different metadata to a logging event beyond just supplying a message string. This extra metadata is then searchable in the log aggregator via a separate field.

It is accomplished by supplying a message template and values to the logging call, for example the below will output the `date` field in Kibana in addition to the usual message.

```csharp
_logger.LogInformation("HomeController Index executed at {date}", DateTime.UtcNow);
```

## Timing

We should aim for log aggregation to be in place prior to going Live or when expecting traffic from more than a small handful of users.