---
title: Installation
description: How to install Newsletter Studio into your Umbraco website.
---
# Installation
There is two ways to install the package, either use the backoffice of Umbraco to install/upload the package or using the recommended way which is to install the package via NuGet. 

## Install via NuGet
The package can be found on [NuGet](https://www.nuget.org/packages/NewsletterStudio/).

Just install the package using the Package Manager inside Visual Studio or via the console.

```xml
Install-Package NewsletterStudio
```

We host several packages on NuGet:

* NewsletterStudio - Contains only UI-file (App_Data)
* NewsletterStudio.Web - Contains Umbraco-dependencies, most of the time any custom class library you might have should depend on this package.
* NewsletterStudio.Core - Core files for the package with minimum external dependencies. Reference this package when you want to extend the functionality.
* NewsletterStudio.CssInline - Package that deals with inlining css into html when rendering the email.

## Install via the backoffice
Since Umbraco 9 don't support package installation from the backoffice this not supported by v4+ of Newsletter Studio.

