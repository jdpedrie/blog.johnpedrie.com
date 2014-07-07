---
layout: post-no-feature
title: "WordPress and Composer"
description: "How to manage updates, plugins, themes and PHP packages entirely through the Composer package manager"
category: articles
tags: [composer, php, wordpress, package manager]
---

Over the last several years, there has been a revolution happening in the PHP world. This revolution has been driven by projects like PHP-FIG and the Composer Package Manager. The WordPress community has largely been bypassed by this change.

Today we'll look at how we can manage an entire WordPress site through Composer and git. We'll install our themes and plugins via Composer. We'll even install WordPress itself through Composer.

## Managing WordPress configuration and database

The example we're using, found [here](https://github.com/jdpedrie/wp-composer), is configured to use ini-based configuration. The main config file, [`wp-config.ini`](https://github.com/jdpedrie/wp-composer/blob/master/wp-config.ini) is stored one level above the web root.

Our `wp-config.php` file, stored in the web root, includes the .ini file and uses the values set there to configure WordPress for database access and manage other settings.

Most settings in `wp-config.ini` will be familiar to anyone who has used WordPress in the past. The only things I want to mention are `DISALLOW_FILE_MODS` and `ENV`. The first of these prevents any plugin or theme updates or modifications from the WordPress admin panel. The reason this is important will become apparent soon, but suffice it to say that the source of truth MUST be our `composer.json` file. Should plugins installed or the versions installed become out-of-sync with our composer configuration, problems will arise when we attempt to update things later. The `ENV` setting helps with development and allows us to determine whether we are in a production environment or a test environment.


## Plugins and Themes in Composer

Composer includes several "installers" for managing WordPress. An Installer essentially allows packages to specify custom paths for packages. We're using a few installers, `wordpress-plugin`, `wordpress-theme` and `wordpress-mu-plugin`.

An installer is specified in a package's `composer.json`:

````
{
  "name": "author/package-name",
  "type": "wordpress-plugin"
}
````

To learn more about Composer installers, check out the [Github Repository](https://github.com/composer/installers). For our purposes, though, the important thing to remember is the types, and to see how we set the paths in which to install each package type.

By default, the WordPress Composer Installers will install to `wp-content/<type>`. In our custom configuration (found [here](https://github.com/jdpedrie/wp-composer/blob/master/composer.json)), we'll modify those slightly.

````
"extra": {
    "wordpress-install-dir": "public/wordpress",
    "installer-paths": {
        "public/wp-content/plugins/{$name}/": ["type:wordpress-plugin"],
        "public/wp-content/themes/{$name}/": ["type:wordpress-theme"],
        "public/wp-content/mu-plugins/{$name}/": ["type:wordpress-muplugin"]
    }
}
````

[wpackagist](http://wpackagist.org) is a Composer repository that mirrors the WordPress.org Plugin and Theme repository and makes them installable via Composer. We're using wpackagist to manage themes and plugins in this project.

The first step is to add wpackagist as a source in our `composer.json` file:

````
"repositories":[
    {
        "type": "composer",
        "url": "http://wpackagist.org"
    }
]
````

Now, we can add plugins as required packages in the `composer.json` file:

````
"require": {
    ....
    "wpackagist-plugin/akismet": "dev-trunk"
}
````

Each version available in the WordPress plugin repository is also available in wpackagist.