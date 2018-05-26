# front-end-projects

## Starting the app

Since this is AngularJS based application you need a server (Apache, IIS, xampp, etc)to serve the html files and perform http request to load all views.
 
Important! Opening the index.html with a double click (i.e. using file:// protocol) will show you only a blank page because there’s no server that response to the requests made for each view in order to display the app interface.
Basic server setup (requires nodejs)

This is simple solution for a basic server setup using nodejs that might help you. If don't have installed nodejs take a look at the Build section to learn about it
 
Install the http-server  (-g installs globally)
  npm install http-server -g  
Once it is installed move to the root folder of the theme (where index.html is located) and run
  http-server . -a 127.0.0.1 -p 8080
 
If everything goes fine you can now access to the app at http://127.0.0.1:8080/

==============================================================================

## Structure
Before starting to customize the template, here are the project files organization structure:
 
+-- app/
|   +-- css/
|   +-- documentation/
|   +-- img/
|   +-- js/
|   +-- i18n/
|   +-- pages/
|   +-- vendor/
|   +-- views/
+-- master/
|   +-- jade/
|   |   +-- pages/
|   |   +-- views/
|   +-- js/
|   |   +-- modules/
|   |   |   +-- controllers/
|   |   |   +-- directives/
|   |   |   +-- services/
|   |   +-- custom/
|   +-- less/
|   |   +-- app/
|   |   +-- bootstrap/
|   |   +-- themes/
|   +-- gulpfile.js
|   +-- package.json
|   +-- bower.json
+-- server/
|   +-- *.json
+-- vendor/
+-- index.html
 

## Main folders explanation
 
app/ folder
 
This folder contains the compiled files. This files are ready to deploy on your server.
 
pages/ This folder contains the compiled html files for the single pages (out of the app).
views/ This folder contains the compiled html files for the views and partials used for the app.
i18n/ This folder contains the json files use for translation.
vendor/ This folder contains vendor files not managed via bower
 
master/ folder
 
This folder contains the source files. You will find the following folders inside
 
jade/ This folder contains JADE files. This files need to be compiled into html files to be displayed by a browser
less/ This folder contains the LESS files for the core styles and bootstrap styles.
js/ Here you will find pure JS files. All this files are concatenated into the file app.js.
 
vendor/ folder
 
This folder contains the vendor files used to include plugins and other components. This folder is handled via bower so optionally you can remove or upgrade the vendor components using such tool.
 
server/ folder
 
This folder contains server side files used for demonstration and guide for the flot chart and file upload components.
 
sidebar-menu.json This file is important and required because it contains the sidebar menu definition.
 
Custom code
To add your own code you can follow this instructions:
 
Working with css and js
 
Create a file app/css/custom.css and add your own styles
Create a file app/js/custom.js and add your own javascript
Edit the file index.html and include custom.css after all other css files and custom.js after all other js files.
 
Working with source files
 
For JS, go to folder master/js/custom and start editing the file custom.js. After compile the source again with gulp, your own code will included at th bottom of file app/js/app.js.
For LESS, go to folder master/less and create a folder called custom and add your won less files. Then edit file app.less and @import all your stylesheets at the bottom (overrides all app default styles)
A note on updating
 
The premise is, the less you change the downloaded code, the easier will be to apply any updates. Try always to keep your own code the most separated as possible from the package code to easily apply new updates when necessary.
 

 

## Build
Important! You only need to follow this instructions in case you want to work with JADE, LESS and concatenate all JS modules.
 
Node.js is a platform built on Chrome’s JavaScript runtime for easily building fast, scalable network applications.
 
Gulp is a task manager, you can define different tasks to run certain commands. Those commands does the compilation job for JADE and LESS, and concatenates the JS files.
 
Bower is a dependency manager, it works by fetching and installing packages from all over, taking care of hunting, finding, downloading, and saving the stuff you’re looking for. Bower keeps track of these packages in a manifest file, bower.json.
 
The package includes under the master/ folder the file gulpfile.js, package.json and bower.json to install the required components to compile the source files.
 
## Installing tools
 
The following steps are intended to be an orientation guide, if you are not experienced with this you will need to learn more about it from Google :)
 
To install node and npm, go to http://nodejs.org/
Run npm install -g bower to install bower to manage dependencies
Download and install GIT for your platform http://git-scm.com/downloads
 
Once you have all tools installed
 
Open a terminal, go the package master/ folder, then run the command npm install. This command will install gulp and all project dependencies.
Then, to install vendor dependencies, run bower install
Finally run gulp to start the task manager
If everything goes fine, you should see the messages in the terminal telling you that most the task are done ok. The task will watch for files to compile them automatically all files when change.
 
To enable the automatic page reload there is also included a LiveReload task that requires the Chrome plugin Livereload
 
You can also run npm start inside master folder to automatically install npm packages, bower dependencies and finally gulp, all with just one command.
Main Gulp tasks
 
This command will run the default task without minify assets
$ gulp
This task will generate the assest wihout mininfy and with sourcemaps
$ gulp sourcemaps
This taks will generate the asset minified
$ gulp build
You can also mix task build and sourcemaps to get minified version with sourcemaps
$ gulp build sourcemaps
All tasks will end up watching source files for LiveReload
Javascript
The Javascript source is divided in two main files that controls the app
 
base.js: contains the scripts to start the application
 
app.js: contains the modules used in the application (controllers, directive, etc)
 
Note The edit the scripts included in base.js open the file vendor.base.json located under the master folder
 
The app.js script build (concatenation) order is
 
'js/app.init.js'
'js/modules/*.js'
'js/modules/controllers/*.js'
'js/modules/directives/*.js'
'js/modules/services/*.js'
'js/modules/filters/*.js'
 
## LESS
The LESS files compiles into the file app.css. This file contains the bootstrap styles and the application custom styles.
 
Also the app-rtl.css is automatically generated with the same styles but inverted for RTL layout. To convert styles the node script css-flip is used.
 
## Vendor
Vendor script dependencies are managed by bower. Just run bower install in folder master/ and all dependencies will be installed.
 
After installing all dependencies is necessary to install them in the locations the app expects them. To do that, run gulp task with the command gulp scripts:vendor in the master folder.
This task will run automatically the tasks scripts:vendor:app and scripts:vendor:base
 
## Vendor folder
 
To avoid not necessary files that bower downloads there’s a task scripts:vendor:app that will copy all files required by the app from the bower_components folder to the main /vendor folder.
This files are listed in vendor.json file which contains the path of all necessary files required by the app components. Those files are usually required via the lazy load module and you can include fonts, svg, etc.
 
## Vendor Base
 
The base.js file is generated with the task scripts:vendor:base. This task will read the list of files in the vendor.base.json file and will concat and compress all files and move them to create the mentioned base.js file.
All files in base.js are loaded when the app is required for first time and contains all vendor scripts necessary to start the app (jquery, angular, etc).
 
## Vendor Updates
 
To update vendor files via bower you can edit the bower.json file by adding the last version you want to download.
 
Note The folder app/vendor contains vendor files that currently aren’t possible to be managed via bower
 

Usage
Layout
Layout can be changed via the following classed applied to the body tag
 
.layout-fixed: Makes navbars become fixed while the user can scroll only content
 
.layout-boxed: Limits the width of the main wrapper element
 
.aside-collapsed: Condenses the sidebar showing only icons
 
.aside-toggle: used internally for mobiles to hide the sidebar off screen
 
.offsidebar-open: used internally to display the offsidebar component (formally the right sidebar)
 
The following markup representation is in fact divided into views but this code will give a good perspective of the final organization after the app is rendered:
 
<html>
  <head>
    #metas and css
  </head>  
  <body>
    <section class="wrapper" data-ui-view>
 
        #start include from app.html
          <nav class="navbar topnavbar">
            #top navbar content
          </nav>
 
          <aside class="aside">
            #sidebar content (left)
          </aside>
 
          <aside class="offsidebar">
            #offsidebar content (right)
          </aside>
 
         <section>
 
            <div class="content-wrapper" data-ui-view>
              #page content
            </div>  
 
          </section>
        #end include from app.html
     </section>
     #scripts
  </body>
</html>
 
## Lazy Load
This app requires only the necessary scripts according to the view that is loaded saving tons unnecessary request to the server.
 
The lazy load is handled by a custom core function based on the plugin ocLazyLoad
 
To configure the lazy scripts, you need to edit the constants APP_REQUIRES (constants.js)
Then edit the app configuration (config.js) where you will find the routes configuration and add or edit the params using the special function resolveFor which handles the scripts request order for the current route.
 
Adding new AngularJS dependencies
You can add a new AngularJS dependencies following two alternatives.
 
1- If you want to inject a module in the app initialization (i.e. angular.module[] definition), which also means it will be available across all the application, you can:
  a. Append the module content to the file base.js or add it to the file /master/vendor.base.js to generate it using gulp
  b. Or, edit the index.html (or jade) and append the new assets before the app.js 
 
2- If you want to use the new module only when it is required, you can use the lazy load system
 
a. Bower only: Download the new component via bower, add the main component files to the file vendor.json in order to copy them to the root vendor folder.
 
b. Edit the App.constant block ( constants.js ) and add the new module into the APP_REQUIRES constant, by using the "modules" option
 
For example: 
  modules: { name: 'moduleName', files : ['path/file1', 'path/fileetc']) }
 
Make sure moduleName it is exactly the new module name because ocLazyLoad will use it to inject dinamically into your app to make it available in your view controller
 
c. After that, edit the App.config block (config.js) and edit or add your new route setting the new depency in the resolve option
 
    .state('app.my-view ', {
        url: '/my-view',
        templateUrl: helper.basepath('my-view.html'),
        resolve: helper.resolveFor( ' moduleName' )
    })
 
Note that here we don't inject the new module in the angular.module definition.
 
 
3- If everything goes fine, you can now use the new module in your view/controller
 
 
Adding new JavaScript (non AngularJS) dependencies
Apply the same step as defined for the AngularJS modules but in ste 2-b, use the "scripts" option instead.

The "scripts" option is used to require js scripts without trying to inject a new modules into the app (necessary for JS and jQuery plugins)

 

RTL
RTL support uses the a tool called css-flip which inverts most the css properties to change the page orientation.
It’s also a property $rootScope.isRTL to detect when the site is in RTL mode.
 
Routes
This app uses for routing the AngularUI Router with nested states making more simple to manage the routing system and load resource in cascade.
 
To add custom routes you need to
 
Create the view for the route in folder app/views/myroute.html
Add a new entry in the file server/sidebar-menu.json to create the menu entry (see format below)
Add your route config in the App.confg part (file app.js or config.js) using the following example code
Note that helper corresponds to the RouteHelperProvider introduced to share functions basepath and resolveFor
.state('app.someroute', {
  url: '/some_url',
  templateUrl: helper.basepath('someroute_view.html'),
  controller: 'someController',
  resolve: angular.extend(
    helper.resolveFor(), {
    // YOUR RESOLVES GO HERE
    }
  )
})
Translation
The translation system uses the AngularUI Translate module.
This modules simplifies the translation system by loading translate references from a JSON file and replacing the content where the reference has been used.
 
Examples
 
<h3 ui-translate="reference.NAME">Text that will be replaced</h3>
 
<h3>{{ 'reference.NAME' | translate }}</h3>
 
<a href="#" title="{{ 'reference.NAME' | translate }}">Link</a>
 
The JSON files with translation references are located in the folder app/i18n
 
Dynamic Sidebar
The sidebar is created dynamically from a JSON file.
 
JSON properties format:
 
[
  {
    "text":      "Item text",          // replaced by translate reference
    "sref":      "app.dashboard",      // the state name of the target route
    "icon":      "icon-speedometer",   // the icon full classname
    "translate": "sidebar.ITEM",       // the translation reference
    "heading":   "true"                // only when item is used as heading 
  },
  ...
]
 
This is method is also useful if you pretend to generate a per user menu dynamically in the server.
 
You can also create an object and edit all properties to generate and update the sidebar directly from Javascript instead of load from the json file.
Markdown Docs
This documentation is loaded from a Markdown source using Flatdoc plugin. The menu and the content is generated automatically from the .md file and styled directly from custom css.
 
Via the flatdoc directive you can use it like this
 
<flatdoc src="path/to/readme.md"></flatdoc>
 
 
 
Themes
To change the color scheme you have 2 options:

From LESS files
 
Edit the LESS files in folder master/less/app and the file master/less/bootstrap/variables.less to use the color you want.
Like Bootstrap, most of the colors are based on @brand-* variables.
 
You can also edit the files in master/less/theme folder to create your own set of color schemes. This files must be included after the app.css in order to override the default color set.
 
Changing the theme colors from LESS files helps you to avoid bloating your css by not double declaring your color rules.
 
From CSS files
 
This template support color schemes including a css file. You can find the color options in the folder app/css/ files are named theme-*.css
 
If you want to change or add a new component color, just inspect the color using your favorite browser devtool and then replace the value in the file.
 
This files are prepared to change the basic color scheme (both sidebars and top navbar) but if you want to make a more deep change I suggest you to check the LESS way which is more simple for multiple component changes.
 
Default Theme
 
To set a default just include in file index.html the theme css file of your choice right after the the main stylesheet (app.css)
Directives
This item include all directives from Angular BootstrapUI. plus the following custom directives (see demo for examples)
 
[href]
File: anchor.js
Disables null anchor behavior
 
[animate-enabled]
File: animate-enabled.js
Enable or disables ngAnimate for element with directive
 
[chosen]
File: chosen-select.js
Initializes the chose select plugin
 
[classyloader]
File: classy-loader.js
Enable use of classyLoader directly from data attributes
 
[reset-key]
File: clear-storage.js
Removes a key from the browser storage via element click
 
[filestyle]
File: filestyle.js
Initializes the fielstyle plugin
 
[flatdoc]
File: flatdoc.js
Creates the flatdoc markup and initializes the plugin
 
[form-wizard]
File: form-wizard.js
Handles form wizard plugin and validation
 
[toggle-fullscreen]
File: fullscreen.js
Toggle the fullscreen mode on/off
 
[gmap]
File: gmap.js
Init Google Map plugin
 
[load-css]
File: load-css.js
Request and load into the current page a css file
 
[markdownarea]
File: markdownarea.js
Markdown Editor from UIKit adapted for Bootstrap Layout.
 
[masked]
File: masked.js
Initializes the masked inputs
 
[search-open]
File: navbar-search.js
Open the search in the top navbar. Use [search-dismiss__] in buttons at close it.
 
[notify]
File: notify.js
Create a notifications that fade out automatically. Based on Notify addon from UIKit (http://getuikit.com/docs/addons_notify.html)
 
[now]
File: now.js
Provides a simple way to display the current time formatted
 
[paneltool]
Module panel-tools.js
Directive tools to control panels. Allows collapse, refresh and dismiss (remove) Saves panel state in browser storage.
Supports attributes [panel-dismiss] [panel-collapse] [panel-refresh]
 
[animate]
File: play-animation.js
Provides a simple way to run animation with a trigger. Requires animo.js
 
[scrollable]
File: scroll.js
Make a content box scrollable
 
[sidebar]
File: sidebar.js
Wraps the sidebar and handles collapsed state
 
[skycon]
File: skycons.js
Include any animated weather icon from Skycons
 
[sparkline]
File: sparkline.js
SparkLines Mini Charts
 
[check-all]
File: table-checkall.js
Tables check all checkbox
 
[tagsinput]
File: tags-input.js
Initializes the tag inputs plugin
 
[toggle-state]
File: toggle-state.js
Toggle a classname from the body tag. Useful to change a state that affects globally the entire layout or more than one item.
Elements must have [toggle-state=”CLASS-NAME-TO-TOGGLE”]. Use [no-persist] to avoid saving the sate in browser storage.
 
[ui-slider]
File: ui-slider.js
Initializes the jQuery UI slider controls
 
[validate-form]
File: validate-form.js
Initializes the validation plugin Parsley
 
[vector-map]
File: vector-map.js.js
Initializes jQuery Vector Map plugin
 
 
Seed Project
This project is an application skeleton. You can use it to quickly bootstrap your angular webapp projects and dev environment for these projects. 
The seed app doesn't do much and has most of the feature removed so you can add theme as per your needs just following the demo app examples.
 
The angular seed project includes the following features
 
- Modernizr
- Icons (FontAwesome and SimpleLine)
- Translation
- RTL mode
- View loading bar
- Top navbar search form
- Themes
- Two routes with its own sidebar entries (one single item and a menu)
- Empty offsidebar (left sidebar)
- Full base.js (like demo app)
 
 
 
Credits
Plugins
 
[Angular](https://angularjs.org/)  
[Angular Docs](https://docs.angularjs.org/guide/)  
[ocLazyLoad](https://github.com/ocombe/ocLazyLoad)  
[uiRouter](https://github.com/angular-ui/ui-router)  
[uiTranslate](https://github.com/angular-translate/angular-translate)  
[uiBootstrap](http://angular-ui.github.io/bootstrap/)  
[Toaster](https://github.com/jirikavi/AngularJS-Toaster)  
[Angular Loading Bar](http://chieffancypants.github.io/angular-loading-bar/)  
[Bootstrap](http://getbootstrap.com/)  
[jQuery]( http://jquery.com/)  
[Fastclick](https://github.com/ftlabs/fastclick)  
[Animo](http://labs.bigroomstudios.com/libraries/animo-js)  
[Animate.css](http://daneden.github.io/animate.css/)  
[Chosen](http://harvesthq.github.io/chosen/)  
[Codemirror](http://codemirror.net/)  
[BS Filestyle](http://markusslima.github.io/bootstrap-filestyle/)  
[FlotCharts](http://www.flotcharts.org/)  
[gMap](http://github.com/marioestrada/jQuery-gMap)  
[Marked](https://github.com/chjj/marked)  
[ClassyLoader](http://www.class.pm/projects/jquery/classyloader/)  
[CSSRadialBar](http://codepen.io/geedmo/pen/InFfd)  
[Modernizr](http://modernizr.com/)  
[MomentJs](http://momentjs.com/)  
[Parsley](http://parsleyjs.org/)  
[Bootstrap Slider](http://www.eyecon.ro/bootstrap-slider)  
[Sparkline](http://omnipotent.net/jquery.sparkline/#s-about)  
[BS Tags Input](http://timschlechter.github.io/bootstrap-tagsinput/examples/bootstrap3/)  
[slimSCroll](http://rocha.la/jQuery-slimScroll)  
[DataTables](https://datatables.net/.)  
[FullCalendar](http://arshaw.com/fullcalendar/docs/)  
[CsSpinner](http://jh3y.github.io/-cs-spinner/)  
[InputMask](https://github.com/RobinHerbots/jquery.inputmask)  
[jVectorMap](http://jvectormap.com/)  
[FlatDoc](https://github.com/rstacruz/flatdoc)  
[jQueryUI](http://jqueryui.com/sortable/)  
[UiKit Upload](http://www.getuikit.com/docs/addons_upload.html)  
[UiKit Notify](http://www.getuikit.com/docs/addons_notify.html)  
[UiKit MarkdownArea](http://www.getuikit.com/docs/addons_markdownarea.html)  
 
Icons  
 
[Font Awesome](http://fortawesome.github.io/Font-Awesome/)  
[Skycons](http://darkskyapp.github.io/skycons/)  
[Weather Icons](http://erikflowers.github.io/weather-icons/)  
 
Demo images  
 
[uiFaces](http://uifaces.com/)  
[Raumrot](http://www.raumrot.com/10/)  
[Unsplash](http://unsplash.com)  
 
