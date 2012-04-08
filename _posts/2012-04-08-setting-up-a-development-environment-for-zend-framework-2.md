---
layout: post
title: "Setting up a development environment for Zend Framework 2"
description: "In this blog post the development environment for Zend Framework 2 has been set up along with the PHP version 5.4.0"
tweet-text: "Setting up a development environment for #ZF2 along with #PHP5.4"
author: "Kim"
tags:
- zf2
- php5.4
categories:
- "Web development"
- "Zend Framework"
---

<img itemprop="image" src="{{ site.url }}/images/php54-plus-zf2-small.png" alt="ZF2+PHP5.4" />

I have followed the development process of [Zend Framework 2](http://framework.zend.com/zf2) quite a while, but haven't taken a closer look at it yet. At last, today I decided to jump in and set up the development enviroment to get started. This blog post documents what I did to see the _"Welcome to Zend Framework 2"_ message in my web browser.

## Install PHP 5.4.0 locally

Zend Framework 2 requires PHP 5.3, but I wanted to use PHP 5.4 because of its built-in web server. As I use Debian 6 as my primary Linux distro, I hadn't other choices than built PHP 5.4 from sources.

First fetch PHP 5.4.0 source code:

<pre class="prettyprint linenums lang-sh">
cd ~/Downloads
wget http://fi2.php.net/get/php-5.4.0.tar.gz/from/fi2.php.net/mirror -O - | tar -xvz
</pre>

Before going any further, make sure that all required supportive software are around:

<pre class="prettyprint linenums lang-sh">
sudo aptitude install \
libxml2-dev \
libxml2 libssl-dev \
libevent-dev \
libbz2-dev \
libcurl4-openssl-dev \
libjpeg62-dev \
libpng12-dev \
libxpm-dev \
libfreetype6-dev \
libt1-dev \
libmcrypt-dev \
libmysql++-dev \
libmysqld-dev \
libmysqlclient-dev \
libxslt1-dev \
autoconf
</pre>

Now run configure script packed with PHP 5.4.0 sources. I decided to add almost everything whether I need those or not. In addition, I wanted PHP work locally thus `--prefix=$HOME/local/php5`. To browse through for other configuration params call `./configure --help`.

<pre class="prettyprint linenums lang-sh">
cd php-5.4.0
./configure \
--prefix=$HOME/local/php5 \
--with-config-file-path=$HOME/local/php5/etc \
--with-curl \
--with-pear \
--with-gd \
--with-jpeg-dir \
--with-png-dir \
--with-zlib \
--with-xpm-dir \
--with-freetype-dir \
--with-t1lib \
--with-mcrypt \
--with-mhash \
--with-mysql \
--with-mysqli \
--with-pdo-mysql \
--with-openssl \
--with-xmlrpc \
--with-xsl \
--with-bz2 \
--with-gettext \
--with-fpm-user=www-data \
--with-fpm-group=www-data \
--enable-fpm \
--enable-exif \
--enable-wddx \
--enable-zip \
--enable-bcmath \
--enable-calendar \
--enable-ftp \
--enable-mbstring \
--enable-soap \
--enable-sockets \
--enable-sysvmsg \
--enable-sysvsem \
--enable-sysvshm
</pre>

After successful configuration, compile and install:

<pre class="prettyprint linenums lang-sh">
make
make install
</pre>

Do we have a correct version? &ndash; Yes we have :)

<pre class="prettyprint linenums lang-sh">
kblomqvist@debian:~$ export PATH=$HOME/local/php5/bin:$PATH
kblomqvist@debian:~$ php -v
PHP 5.4.0 (cli) (built: Apr  7 2012 20:24:10) 
Copyright (c) 1997-2012 The PHP Group
Zend Engine v2.4.0, Copyright (c) 1998-2012 Zend Technologies
</pre>


## Checkout ZF2 Skeleton Application

Yay, this was far too easy:

<pre class="prettyprint linenums lang-sh">
cd ~/workspace
git clone --recursive git://github.com/zendframework/ZendSkeletonApplication.git
cd ZendSkeletonApplication/public
php -S localhost:8000
</pre>

<img src="/images/zf2-welcome-screen.png" alt="Zend Framework 2 welcome screen" />
