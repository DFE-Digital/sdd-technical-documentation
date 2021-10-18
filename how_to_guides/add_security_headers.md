# How to add security headers to an asp.net core project

This guide shows a base approach to increasing security within an asp.net core web application by adding security headers using the NetEscapades framework.

## Install the NetEscapades.AspNetCore.SecurityHeaders nuget package

Install using the [NetEscapades.AspNetCore.SecurityHeaders NuGet package](https://www.nuget.org/packages/NetEscapades.AspNetCore.SecurityHeaders) from the Visual Studio Package Manager Console:

```
PM> Install-Package NetEscapades.AspNetCore.SecurityHeaders
```

Or using the `dotnet` CLI

```bash
dotnet package add Install-Package NetEscapades.AspNetCore.SecurityHeaders
```

Further information can be found at https://github.com/andrewlock/NetEscapades.AspNetCore.SecurityHeaders

## Create a class to define the security headers

Add a class in a suitable location that creates a new HeaderPolicyCollection for example as defined below:

```csharp
public static class SecureHeadersDefinitions
    {
        public static class SecurityHeadersDefinitions
        {
            public static HeaderPolicyCollection GetHeaderPolicyCollection(bool isDev)
            {
                var policy = new HeaderPolicyCollection()
                    .AddFrameOptionsDeny()
                    .AddXssProtectionBlock()
                     .AddContentTypeOptionsNoSniff()
                     .AddReferrerPolicyStrictOriginWhenCrossOrigin()
                     .RemoveServerHeader()
                     .AddCrossOriginOpenerPolicy(builder =>
                     {
                         builder.SameOrigin();
                     })
                     .AddCrossOriginEmbedderPolicy(builder =>
                     {
                         builder.RequireCorp();
                     })
                     .AddCrossOriginResourcePolicy(builder =>
                     {
                         builder.SameOrigin();
                     })
                     .AddContentSecurityPolicy(builder =>
                     {
                         builder.AddObjectSrc().None();
                         builder.AddBlockAllMixedContent();
                         builder.AddImgSrc().Self().From("data:");
                         builder.AddFormAction().Self();
                         builder.AddFontSrc().Self();
                         builder.AddStyleSrc().Self();
                         builder.AddBaseUri().Self();
                         builder.AddScriptSrc().From("https://www.googletagmanager.com").UnsafeInline().WithNonce();
                         builder.AddFrameAncestors().None();
                     })
                     .RemoveServerHeader()
                     .AddPermissionsPolicy(builder =>
                     {
                         builder.AddAccelerometer().None();
                         builder.AddAutoplay().None();
                         builder.AddCamera().None();
                         builder.AddEncryptedMedia().None();
                         builder.AddFullscreen().All();
                         builder.AddGeolocation().None();
                         builder.AddGyroscope().None();
                         builder.AddMagnetometer().None();
                         builder.AddMicrophone().None();
                         builder.AddMidi().None();
                         builder.AddPayment().None();
                         builder.AddPictureInPicture().None();
                         builder.AddSyncXHR().None();
                         builder.AddUsb().None();
                     });
                    
                if (!isDev)
                {
                    // max age = one year in seconds
                    policy.AddStrictTransportSecurityMaxAgeIncludeSubDomains(maxAgeInSeconds: 60 * 60 * 24 * 365);
                }

                return policy;
            }
        }
    }
```

The above allows scripts from `wwww.googletagmanager.com` which is only necessary if the site is using google analytics.

## Add the security header definitions to the startup class middleware

In the `Configure` method add the following at the start of the pipeline after the exception page condition:

```csharp

app.UseSecurityHeaders(
                SecureHeadersDefinitions.SecurityHeadersDefinitions.GetHeaderPolicyCollection(env.IsDevelopment()));

```

## Add the nonce to any scripts

The `AddScriptsSrc` `UnsafeInline().WithNonce()` method ensures that scripts will only be run if they have the correct nonce value. To set this add the nonce to **all** script elements e.g.

```html
<body class="govuk-template__body ">
@if (_environment.IsProduction())
{
     <!-- Google Tag Manager (noscript) -->
     <noscript nonce="@nonce">
        <iframe src=https://www.googletagmanager.com/ns.html?id=XXXXX
                class="not-visible" height="0" width="0">
        </iframe>
    </noscript>
    <!-- End Google Tag Manager (noscript) -->
}
<script nonce="@nonce">
    document.body.className = ((document.body.className) ? document.body.className + ' js-enabled' : 'js-enabled');
</script>
```

Note that inline styles will cause an error because of the CSP definition, so in the example google analytics snippet above, the inline style has been replaced with a css class (`not-visible`) defined in an external css file e.g. `Site.css`