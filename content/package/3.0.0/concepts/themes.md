# Themes

Emails in Newsletter Studio are all based on a theme which contains:

* Default settings for colors, font size, and more.
* HTML templates (Razor) for rendering the email and controls.

The default theme that is shipped with the package is based on best practices and provides a solid foundation for the rendering a HTML-email. Our [contrib-project](https://github.com/enkelmedia/NewsletterStudioContrib/tree/master) contains all the files from the default theme for you to use as a starting point.

You can easily create your own themes to override the default behavior. Themes only need to contain the overrides you need to make since the package will fallback on values from the default theme if any setting or views are not present in a custom theme.

## How to create a Theme
To create a new theme you first need your own folder inside App_Plugins, let's use "MySitePlugins" as an example, and let's call the theme "MyTheme", the folder can be named whatever suits your project. We need to create folders with the following structure:

`App_Plugins/MySitePlugins/NewsletterStudio/Themes/MyTheme`

Notice here that the extentions for Newsletter Studio should go in a folder called "NewsletterStudio" inside your custom folder in `App_Plugins`. Never try to put your custom code inside `App_Plugins/NewsletterStudio` as this folder might be removed/cleaned/re-populated during build.

Newsletter Studio will scan each subfolder in "App_Plugins" and look for a folder called "NewsletterStudio". So basically a wildcard search like this:

`App_Plugins/*/NewsletterStudio/Themes/*`

Inside the "MyTheme"-folder we need to create the file `theme.json` to declare our theme. Custom themes only override settings from the default theme, so the minimum content of this file would be:

```json
{
  "name": "My New Theme"
}
```

A more useful version would be something like this to override the selectable colors:

```json
{
  "name": "My New Theme",
  "selectableColors": "#ff00ff,#0000ff,#ffff00",
}
```

All other properties would be fetched from the default theme.json file that looks like this:

```json
{
  "name": "Default",
  "backgroundColor": "#F3F3F3",

  "selectableColors": "#991BAD,#31D843,#8447FF,#0F2947,#0F2947,#ffffff,#000000",

  "rowBackgroundColor": "#ffffff",

  "controlButtonBackgroundColor": "#31D843",
  "controlButtonTextColor": "#FFFFFF",
  "controlButtonFontFamily": "Arial, 'Helvetica Neue', Helvetica, sans-serif",
  "controlButtonSmallPadding" : "12px 25px",
  "controlButtonSmallFontSize" : "16px",
  "controlButtonMediumPadding" : "16px 35px",
  "controlButtonMediumFontSize" : "18px",
  "controlButtonLargePadding" : "20px 50px",
  "controlButtonLargeFontSize" : "20px",

  "controlTextTextColor": "#000000",
  "controlTextLinkColor": "#8447FF",
  "controlTextFontFamily": "Arial, 'Helvetica Neue', Helvetica, sans-serif",
  "controlTextFontSize": "16px",

  "controlTextHeading1FontFamily": "Arial, 'Helvetica Neue', Helvetica, sans-serif",
  "controlTextHeading1FontSize": "32px",

  "controlTextHeading2FontFamily": "Arial, 'Helvetica Neue', Helvetica, sans-serif",
  "controlTextHeading2FontSize": "28px",

  "controlTextHeading3FontFamily": "Arial, 'Helvetica Neue', Helvetica, sans-serif",
  "controlTextHeading3FontSize": "24px"

}
```

## Overriding rendering views (cshtml)
There are two ways to override the rendering of the email. A good starting point is to look at the [views from the default theme](https://github.com/enkelmedia/NewsletterStudioContrib/tree/master/Newsletter%20Studio%20V12/Default-Theme). 

It's possible to override:
* The main "email.csthml" file which renders the "framework" for the email
* Individual view for each [Email Control](../develop/email-control.md).

If you need to override one or more of them just mimic the folder structure inside the global `NewsletterStudioExtensions`-folder or inside the NewsletterStudio-folder or inside your custom theme folder.

### Global override
A global override would be used for all themes and basically override the default view. Create a new folder called `NewsletterStudioExtensions` in the `App_Plugin`-folder and create a view-folder inside this: 

`App_Plugins/NewsletterStudioExtensions/Views/`

Inside here, place the cshtml-files that you want to override, for example:

`App_Plugins/NewsletterStudioExtensions/Views/email.cshtml`
`App_Plugins/NewsletterStudioExtensions/Views/unsubscribe.cshtml`
`App_Plugins/NewsletterStudioExtensions/Views/Controls/button.cshtml`

### Theme-specific overrides
For a theme-specific override just create the view folder inside your theme, ie:

`App_Plugins/MySitePlugins/NewsletterStudio/Themes/MyTheme/Views/`

Inside here, place the cshtml-files that you want to override.

### Example 1: Global override for button-view
Create the file:
`App_Plugins/NewsletterStudioExtensions/Views/Controls/button.cshtml`

Update the content to reflect what you need, this file will now be used for all rendering except for when a given theme has an override for this file.

### Example 2: Theme-specific override for button-view
Create the file:
`App_Plugins/MySitePlugins/NewsletterStudio/Themes/MyTheme/Views/Controls/button.cshtml`

This view will now be used for all email rendering when "My Theme" is used.

## Custom Fonts
It's possible to manipulate the list of fonts that we show in the drop downs and use in your emails. Be aware that while it's possible to render the needed CSS for a custom font not all email client will support them. One example is Microsoft Outlook that does not support custom fonts and it will not respect any fallback fonts. For example when setting the style `font-family: CustomFont, Arial;` Outlook will ignore the CustomFont but it will not fallback to Arial but to the default font configured in Outlook.

If you still want to use a custom font, this is how to configure the dropdown to include your custom font:
```csharp
 public  sealed class SiteComposer : IUserComposer
{
    public void Compose(Composition composition)
    {
        NewsletterStudio.Core.Editor.FontsCollection.All.Add(new FontDefinitionGroup()
        {
            Title = "Custom Fonts",
            Definitions = new List<FontDefinition>()
            {
                new FontDefinition() {
                    DisplayName = "SFProDisplay",
                    FontFamilyValue = "SFProDisplay",
                    CustomFontDeclarationCss = "@font-face { font-family: 'SFProDisplay'; src: url('https://www.mysite.com/fonts/SFProDisplay-Bold.woff2') format('woff'); font-weight: normal; font-style: normal; }"
                }
            }
        });
    }
}

```


