---
layout: post
title: How to use HTML5 Boilerplate with Zend Framework
description: It is described how H5BP and its build script can be integrated with Zend Framework with the minimum efforts.
author: Kim
tags:
  - zf
  - h5bp
---

<img itemprop="image" src="{{ site.url }}/images/h5bp-zf-integration.png" alt="H5BP star and ZF logo integrated" style="float: left; margin: 10px 10px 0 0;" />
You may have noticed that when ever you start a new project for a website there is quite a bit boilerplate HTML and CSS to write. Fortunately there is bunch of projects, such as [HTML5 Boilerplate](https://github.com/h5bp) and [Bootstrap](https://github.com/twitter/bootstrap), which provide default HTML5, CSS and JavaScript where to start. Another interesting project is [Initializr](http://www.initializr.com) that combines the upper two. Well good so far but, if used with a web application framework still there are tedious tasks to complite, for example, updating the framework's templates. In addition, H5BP comes with a build script that takes care of CSS and JS minification to mention but a few. Especially the integration of this with a framework most likely needs improvisation. Here it is shown how to integrate HTML5 Boilerplate and its build script with [Zend Framework](http://framework.zend.com) (version 1).

## Step 1: Let's create a new project

Here it is assumed that you have ZF tool set in PATH and that you have wget installed. Create! Start by changing the directory to your workspace or where ever you keep your projects and cast the spells given above:

	zf create project my-zfproject
	cd my-zfproject && zf enable layout
	wget --no-check-certificate https://github.com/muiku/h5bp-zendframework/tarball/v3.0.2-zfint -O - | tar -xvz --strip 1

What just happend? -- Well you created a new ZF project, have layouts enabled and the ZF's default layout template has all the HTML5 Boilerplate goodies added. In addition, you got new a .htaccess file and error page. In short, you are completely ready to start producing the application. Though you may like to remove a few unnecessary files, which are:

- .gitattributes
- .gitignore
- readme.md

These files were copied with others from our [H5BP Zend Framework integration](https://github.com/muiku/h5bp-zendframework) GitHub repository (the third line of the given commands).

## Step 2: Put build script into use

HTML5 build script is an optional tool and kept in the separate repository. Thus it was not included yet in step 1 and there is really no hurry for it because it's used in deployment for the most part. However, now it's time to put it into use. Under your project directory say:

	wget --no-check-certificate https://github.com/muiku/h5bp-antbs-zendframework/tarball/zfint -O - | tar -xvz --strip 1

What did we get? -- A new directory public/build/. Cd into it and command ant. After that H5BP build script will begin to run and comrepss your files, which are css/js/images etc. under public/ directory. If the build ends successfully you should have three new directories:

- public/publish/ - this is your new public/
- public/intermediate/ - you can ignore this or remove it if you like
- application/layouts/scripts/publish/ - the builded layout templates locate here

If you now check your development site in the browser you should not see anything new. All the CSS and JS are still as written. To make magic happen open the application.ini and tell ZF to look layouts from application/layouts/scripts/publish/ folder. Your application.ini should look something like this

[production]
phpSettings.display_startup_errors = 0
phpSettings.display_errors = 0
includePaths.library = APPLICATION_PATH "/../library"
bootstrap.path = APPLICATION_PATH "/Bootstrap.php"
bootstrap.class = "Bootstrap"
appnamespace = "Application"
resources.frontController.controllerDirectory = APPLICATION_PATH "/controllers"
resources.frontController.params.displayExceptions = 0

; Published (H5BP build script generated) layouts
resources.layout.layoutPath = APPLICATION_PATH "/layouts/scripts/publish"

; Ensure that view encoding is UTF-8 and that view helpers use HTML5
resources.view.encoding = "UTF-8"
resources.view.doctype = "HTML5"

[staging : production]

[testing : production]
phpSettings.display_startup_errors = 1
phpSettings.display_errors = 1
resources.layout.layoutPath = APPLICATION_PATH "/layouts/scripts"

[development : production]
phpSettings.display_startup_errors = 1
phpSettings.display_errors = 1
resources.frontController.params.displayExceptions = 1
resources.layout.layoutPath = APPLICATION_PATH "/layouts/scripts"

Ouh! And don't forget to point site docroot to public/publish/. In Apache2 you can do this in this way

<VirtualHost *:80>
    # ...

    SetEnv APPLICATION_ENV production

    DocumentRoot /home/me/workspace/myproject/public/publish
    DirectoryIndex index.php

    <Directory /home/me/workspace/myproject/public/publish>
        AllowOverride All
        Order allow,deny
        Allow from all
    </Directory>

    # ...
</VirtualHost>

## That's it

Hope this tutorial helps you to use H5BP and adapt its build script with Zend Framework. You can follow the integration process in GitHub

- [HTML5 Boilerplate Zend Framework Integration](HTML5 Boilerplate Zend Framework Integration)
- [The H5BP ant build script Zend Framework integration](https://github.com/muiku/h5bp-antbs-zendframework)

[Zend Framework 2](http://packages.zendframework.com/) is, in the time of writing, in Beta3 and has now rewritten view layer so maybe we see ZF2 integration soon ;)

