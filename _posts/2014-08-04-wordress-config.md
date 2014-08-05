---
layout: post-no-feature
title: "Move your WordPress Config out of the Web Root!"
description: "Your website should be in source control. Your database credentials should not be in source control."
category: articles
tags: [composer, php, wordpress, package manager, software development]
comments: true
---

As anybody who's worked with WordPress, or even anyone who's looked at the file structure of a WordPress site knows, your site is controlled by a `wp-config.php` file typically found in the top level of a site.

There are a multitude of reasons why this isn't a great way to do it. Let's talk about some of them!

* **Storing passwords in code inside the web root is dangerous.** It's not the most dangerous thing you could do. It's not everyday that a server configuration is suddenly broken, exposing code to the outside world. But it happens. And when it does, your secret keys are suddenly not secret. Your database credentials are suddenly public. A quick Google search returns a [StackOverflow question](http://stackoverflow.com/questions/97984/how-to-secure-database-passwords-in-php) from 2008, suggesting moving your configuration out of the web root. That's what we're going to do.
* **Storing passwords in a spot that should be in source control is dangerous.** It's 2014. If you're not using source control (SVN, Git, Mercurial, whatever), you're doing it wrong. When your passwords are stored in a spot that is usually in source control, it's far too easy to accidentally commit that file. It's also far too hard to completely remove that sensitive information from the repository history.
* **If you're using any automated deployment, it's easy to accidentally overwrite your production configuration**
* **If you've got test or beta environments, it's hard to manage multiple versions of your config file.**

Hopefully I've convinced you. Now where do we go from here?

I believe the best way to manage your WordPress configuration is in a .ini file stored one level above the web root. Let's take a look at how it would work.

`/var/www/wp-config.ini`:

````INI
[wp]
ABSPATH           = '/var/www/wp-composer/public'
WP_CONTENT_DIR    = '/var/www/wp-composer/public/wp-content'
WP_CONTENT_URL    = 'http://wpcomposer.dev/wp-content'
WPLANG            = ''
UPLOADS           = 'uploads'
WP_DEBUG          = false
WP_HOME           = 'http://wpcomposer.dev'
WP_SITEURL        = 'http://wpcomposer.dev'
FORCE_SSL_ADMIN   = false
FORCE_SSL_LOGIN   = false
DISALLOW_FILE_MODS= true

[db]
DB_NAME           = 'wordpress'
DB_USER           = 'root'
DB_PASSWORD       = 'root'
DB_HOST           = 'localhost'
DB_CHARSET        = 'utf8'
DB_COLLATE        = ''
TABLE_PREFIX      = 'wp_'
SAVEQUERIES       = false

[keys]
AUTH_KEY          = ''
SECURE_AUTH_KEY'  = ''
LOGGED_IN_KEY'    = ''
NONCE_KEY'        = ''
AUTH_SALT'        = ''
SECURE_AUTH_SALT' = ''
LOGGED_IN_SALT'   = ''
NONCE_SALT'       = ''

[site]
ENV               = 'test'
````

`/var/www/public/wp-config.php`:

````PHP
<?php

$config = parse_ini_file(__DIR__ .'/../wp-config.ini', true);

define('DB_NAME',            $config['db']['DB_NAME']);
define('DB_USER',            $config['db']['DB_USER']);
define('DB_PASSWORD',        $config['db']['DB_PASSWORD']);
define('DB_HOST',            $config['db']['DB_HOST']);
define('DB_CHARSET',         $config['db']['DB_CHARSET']);
define('DB_COLLATE',         $config['db']['DB_COLLATE']);
define('SAVEQUERIES',        $config['db']['SAVEQUERIES']);

define('WP_CONTENT_DIR',     $config['wp']['WP_CONTENT_DIR']);
define('WP_CONTENT_URL',     $config['wp']['WP_CONTENT_URL']);
define('UPLOADS',            $config['wp']['UPLOADS']);
define('WPLANG',             $config['wp']['WPLANG']);
define('WP_DEBUG',           $config['wp']['WP_DEBUG']);
define('WP_HOME',            $config['wp']['WP_HOME']);
define('WP_SITEURL',         $config['wp']['WP_SITEURL']);
define('FORCE_SSL_ADMIN',    $config['wp']['FORCE_SSL_ADMIN']);
define('FORCE_SSL_LOGIN',    $config['wp']['FORCE_SSL_LOGIN']);
define('DISALLOW_FILE_MODS', $config['wp']['DISALLOW_FILE_MODS']);
define('ABSPATH',            $config['wp']['ABSPATH']);

define('AUTH_KEY',           $config['keys']['AUTH_KEY']);
define('SECURE_AUTH_KEY',    $config['keys']['SECURE_AUTH_KEY']);
define('LOGGED_IN_KEY',      $config['keys']['LOGGED_IN_KEY']);
define('NONCE_KEY',          $config['keys']['NONCE_KEY']);
define('AUTH_SALT',          $config['keys']['AUTH_SALT']);
define('SECURE_AUTH_SALT',   $config['keys']['SECURE_AUTH_SALT']);
define('LOGGED_IN_SALT',     $config['keys']['LOGGED_IN_SALT']);
define('NONCE_SALT',         $config['keys']['NONCE_SALT']);

define('SITE_ENV',           $config['site']['ENV']);

if ($config['wp']['FORCE_SSL_LOGIN']) {
  $_SERVER['HTTPS']='on';
}

$table_prefix  = $config['db']['TABLE_PREFIX'];

require_once( ABSPATH . 'wp-settings.php' );
````

Since WordPress requires that Site URLs are explicitly set, I think it's better to specify them in code rather than in the database.

Have you tried any alternate methods of managing WordPress configuration? I think it's clear that there's a problem. I like my solution, but I'd love to hear your ideas in the comments!