---
title: Installation
description: How to install Newsletter Studio into your Umbraco website.
---
# Installation
When installing the package on Umbraco 9, you need to use the NuGet package.

## Install via NuGet
The package can be found on [NuGet](https://www.nuget.org/packages/NewsletterStudio/).

Just install the package using the Package Manager inside Visual Studio or via the console.

```xml
dotnet add package NewsletterStudio
```

We host several packages on NuGet:

* NewsletterStudio - Contains only UI-file (App_Data)
* NewsletterStudio.Web - Contains Umbraco-dependencies, most of the time any custom class library you might have should depend on this package.
* NewsletterStudio.Core - Core files for the package with minimum external dependencies. Reference this package when you want to extend the functionality.
* NewsletterStudio.CssInline - Package that deals with inlining css into html when rendering the email.

## Install via the backoffice
Since Umbraco 9 don't support package installation from the backoffice this not supported by v4+ of Newsletter Studio.

