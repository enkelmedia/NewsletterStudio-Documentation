# Themes

Emails in Newsletter Studio is all based on a theme. The theme contains

* HTML- templates (Razor) for the email and the controls.
* Default settings for colors, font size and more.

The default theme that is shipped with the package is based on best practices and provides a solid foundation for the rendering.

You can easily create your own themes to override the default behaviour. Themes only need contain the overrides as we'll fall back on values from the default theme if any setting is not found.

You should never make changes to the default theme as these changes might be overwritten when updating the package.

## How to create a Theme
Themes lives inside the "App Plugins" folder, the default theme is located here:

`App_Plugins/NewsletterStudio/Themes/Default/`

To create a new theme you first need a folder inside App_Plugins, let's use "MySite" as an example, the folder can be named what ever suites your project. We need to create folders with the following structure:

`App_Plugins/MySite/NewsletterStudio/Themes/MyTheme`

Inside "MyTheme" or what every you choose to call your folder we place files to override the settings in the default theme.


### 

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


Inside this folder you can place a themes.json file to override default settings.

Have a look at the default theme.json file to se the available settings. One can also override the email rendering by putting razor temples inside this folder. If no override files is found in the theme the files from the default template will be used.

**TODO:**
* [ ] Rendering
* [ ] Email Controls
* [ ] Fonts (custom fonts?)
