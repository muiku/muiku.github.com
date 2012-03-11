---
layout: post
title: How to use HTML5 Boilerplate with Zend Framework
description: It is described how H5BP and its build script can be integrated with Zend Framework with the minimum efforts.
author: Kim
tags:
- zf
- h5bp
---

<img itemprop="image" src="{{ site.url }}/images/h5bp-zf-integration.png" alt="H5BP star and ZF logo integrated" />

You may have noticed that when ever you start a new project for a website there is quite a bit boilerplate HTML and CSS to write. Fortunately there is bunch of projects, such as [HTML5 Boilerplate](http://html5boilerplate.com/) and [Bootstrap](http://twitter.github.com/bootstrap/), which provide default HTML5, CSS and JavaScript where to start. Another interesting project is [Initializr](http://www.initializr.com) that combines the upper two. Good so far but, if used with a web application framework there are still tedious tasks to complete, for example, updating the framework's template files. In addition, H5BP comes with a build script that takes care of CSS and JS minification and especially the integration of this tool with the chosen framework might get problematic. Here it is shown how to use HTML5 Boilerplate and its build script with [Zend Framework](http://framework.zend.com) (version 1). The detailed explanation how the integration was constructed is left for another post.

## Step 1: Let's create a new project

It is assumed that you have ZF tool set in PATH and that you have wget installed. Create! Start by changing the directory to your workspace or where ever you keep your projects and cast the spells given below:

    zf create project my-zfproject
    cd my-zfproject && zf enable layout
    wget --no-check-certificate https://github.com/muiku/h5bp-zendframework/tarball/v3.0.2-zfint -O - | tar -xvz --strip 1

What just happend? &ndash; Well you created a new ZF project, have layouts enabled and the ZF's default layout template has all the HTML5 Boilerplate goodies added. In addition, you got a new .htaccess file and error page. In short, you are completely ready to start producing the application. Though you may like to remove a few unnecessary files, which are:

- `.gitattributes`
- `.gitignore`
- `readme.md`

These files were copied with others from our [H5BP Zend Framework integration](https://github.com/muiku/h5bp-zendframework) GitHub repository.

## Step 2: Put build script into use

HTML5 build script is an optional tool and kept in the separate repository. Thus it was not included yet in step 1 and there is really no hurry for it because it's used in deployment for the most part. However, now it's time to put it into use. Under your project directory say:

    wget --no-check-certificate https://github.com/muiku/h5bp-antbs-zendframework/tarball/zfint -O - | tar -xvz --strip 1

What did we get? &ndash; A new directory `public/build/`. Cd into it and command `ant` &ndash; yes, the build script needs [Apache Ant](http://ant.apache.org/) to be installed. After that H5BP build script will begin to run and compress your static assets, which are CSS, JavaScript, images etc. &ndash; all the things kept under `public/`. If the build ends successfully, you should have three new directories:

- `public/publish/` &ndash; this is your new `public/`
- `public/intermediate/` &ndash; this is for staging, ignore it so far
- `application/layouts/scripts/publish/` &ndash; the builded layout templates locate here

If you now check your development site in the browser, you shouldn't see anything new. All the CSS and JS are still as before and not in minified form. To enable the minified assets change the docroot of your web server to point to the `public/publish/` and tell ZF to look layouts from `application/layouts/scripts/publish/` folder when in production. The latter is done by editing `application.ini`. Make sure that you have the following lines in it:

    [production]

    ; Published (H5BP build script generated) layouts
    resources.layout.layoutPath = APPLICATION_PATH "/layouts/scripts/publish"

    ; Ensure that view encoding is UTF-8 and that view helpers use HTML5
    resources.view.encoding = "UTF-8"
    resources.view.doctype = "HTML5"

    [staging : production]

    [testing : production]
    resources.layout.layoutPath = APPLICATION_PATH "/layouts/scripts"

    [development : production]
    resources.layout.layoutPath = APPLICATION_PATH "/layouts/scripts"

Here is an example virtualhost config to set docroot in Apache2:

    <VirtualHost *:80>
        ServerName my-zfproject.localhost

        SetEnv APPLICATION_ENV production

        DocumentRoot /home/your/workspace/my-zfproject/public/publish
        DirectoryIndex index.php

        <Directory /home/your/workspace/my-zfproject/public/publish>
            AllowOverride All
            Order allow,deny
            Allow from all
        </Directory>
    </VirtualHost>

## That's it

Hope this tutorial helps you to use H5BP and adapt its build script with Zend Framework. You can follow the integration process in GitHub

- [HTML5 Boilerplate Zend Framework Integration](https://github.com/muiku/h5bp-zendframework)
- [The H5BP ant build script Zend Framework integration](https://github.com/muiku/h5bp-antbs-zendframework)

[Zend Framework 2](http://packages.zendframework.com/) is, in the time of writing, in Beta3 and has now rewritten view layer so maybe we will see ZF2 integration soon ;)

