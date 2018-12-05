# Customizing core services and demo front-end

When you have created your "default" CX Cloud, as a next step you might want to customize it a bit for example to create a demo for a specific use case. Here are some pointers for that.

## Customize core services

TBA.

## Customize front-end demo \(Angular\)

There are several aspects of the frontend demo that can be easily customized to match specific needs. To create for example a customer specific demo, this might be more feasible way compared to [creating full custom](create-custom-frontend.md) frontend. Here are some guides for small changes. Naturally you can freely change it any way you like.  

### Main colors

Using SASS instead of CSS gives the possibility to declare variables for colors and reuse them in multiples stylesheets in the different components of the application. In the case of our frontend demo, these variables are defined in the following path: `src/assets/scss/modules/_colors.scss`. Changing a color in this file will have an affect applicationwide.

If you want to change the color of an element in only one specific component, it is advised to add that color to the `_colors.scss` file and then replace with the new color in that component.

### Font

The font definition can be found in `src/assets/scss/partials/_typography.scss`.

### Icons

The frontend demo uses [icomoon](https://icomoon.io/) to generate the font files. These files can be found in `src/assets/fonts`. Below are the steps required to modify/add an icon:

* Go to the [icomoon app](https://icomoon.io/app/#/select)
* Click on import icons, and navigate to the fonts directory in the frontend demo project
* Select `icomoon.svg`. You should then see a list of icons in the icomoon app.
* To add a new svg icon, click on the ☰ icon next to icons set from the previously imported svg.
* Chose import to set; then, navigate the icon and select/open it. You should see that icon added to the icons set.
* Click again on the ☰ icon next to icons set, then click _select all_.
* On the bottom of the page, click _Generate SVG & more_. 
* Click _Generate font_.
* At this point you can remove icons you dont need, or change icons names/codes. 
* Click _Download_. 
* Unzip the generated zip file, and nagivate to the _fonts_ folder in the unzipped folder. 
* Copy/replace the content of the _fonts_ folder to the fonts folder of the frontend demo applicaiton \(`src/assets/fonts`\)
* If you are using a premium account in icomoon, you should have a _woff2_ file in the downloaded fonts folder. 
* If you are not a premium user you should use an online tool to convert the downloaded _woff_ file to a _woff2_ file, and then add it the the frontend demo fonts folder.

The icons styling can be manually added to the the _icons.scss_ file in `src/assets/scss/partials/_icons.scss`. But the recommended way is to copy the content of the _styles.css_ file in the downloaded icomoon folder, and copy it in the _\_icons.scss_ file.

In the HTML files, you can add an icon by using the following format `<span class="icon icon-name "></span>` where `icon_name` matches an icon name in `_icons.scs`

#### favicon

To replace the icon displayed in the address bar of the browser, simply modify the _favicon.ico_ file located inside the _src_ folder of the demo application.

## Customize front-end demo \(React\)

TBA

