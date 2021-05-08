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

## Install via the backoffice
To the package via the Umbraco Backoffice, navigate to the "Packages"-section and search for "Newsletter Studio" or download the installation package from the package repository on [our.umbraco.com](https://our.umbraco.com/packages/backoffice-extensions/newsletter-studio-the-email-studio/).

