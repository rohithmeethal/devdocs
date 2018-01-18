---
layout: default
title: Using Composer to install Shopware
github_link: developers-guide/shopware-composer/index.md
indexed: true
group: Developer Guides
subgroup: General Resources
menu_title: Using composer with Shopware
menu_order: 2
---

<div class="toc-list"></div>

## Introduction

Starting with v5.4 Shopware supports installing a shop as a [Composer dependency](https://github.com/shopware/composer-project)
out of the box. This helps you to professionalize development and deployments of Shopware shops by providing a reliable
versioning of Shopware itself and all the plugins required by your project.

## What is Composer

Composer defines itself as "[...] a tool for dependency management in PHP. It allows you to declare the libraries
your project depends on and it will manage (install/update) them for you" ([Composer website](https://getcomposer.org/)).

Composer is the de-facto-standard for dependency management in the PHP community. You can think of it as the
apt/yum/npm/brew of the PHP world.

## How to start

To create a new Shopware project with Shopware as a dependency, all you need to do is install Composer
([download it here](https://getcomposer.org/download/) if you haven't yet) and run

```bash
composer create-project shopware/composer-project my_project_name --no-interaction --stability=dev
```

This will clone the project repository with all necessary dependencies (including the latest released Shopware version)
into a new directory `my_project_name`. Shopware itself will be deployed under `vendor/shopware/shopware` like any other
dependency you might require later on.

## Configuring Shopware

The Composer Shopware project template relies on environment variables to configure your Shopware project. You can
either set those directly (this is recommended on production environments e.g. to not store credentials on disk) or
using a `.env` file in the project root. To see which variables are supported, have a look at the `.env.example` file.

You can also have a `.env` file created for you! Simply run `app/install.sh` inside your new project directory to have a
little installer-script ask you all necessary information.

If you need to configure other values in the `config.php` (e.g. the error or session handler), you can find the file in 
the `app/config/` directory.

## Requiring plugins

Given you want to require the SwagMediaSftp-plugin, all you need to do is run

```bash
composer require shopwarelabs/swag-media-sftp:1.0.0
```

This will add the plugin to your composer.json, download it and install it into the appropriate folder `custom/plugins`.
All you need to do afterwards is install and enable it in the Shopware backend.

Composer knows where to install the plugin to because of the `type` defined in the `composer.json` of this plugin. For
a complete list of available types, see [Composer Installers](https://github.com/composer/installers).

## Project specific plugins

The path `custom/plugins` and all the old plugin directories were added to the `.gitignore` file to prevent plugins
required via Composer from being version controlled in the new project repository as well. We recommend to use Github
or some on-premise Git hosting solution to share plugins you use in more than one Shopware project between those shops.
You can then require those plugins in any of your shops using a Composer command like the one above.

In case you want to create a project specific plugin which doesn't need to be shared, you can install it into the
`custom/project` directory. This directory is equivalent to `custom/plugins`, the only difference is that plugins added
here are version controlled together with the project. There is no equivalent alternative to the old-style plugin
directories, only 5.2 project specific plugins are supported out of the box. Of course you can modify your `.gitignore`-
file, but in that case you should add all plugins you might require via Composer into it.

## Upgrading Shopware

Update the version number of `shopware/shopware` in the `composer.json`. Then run `composer update shopware/shopware`
to install the new version. Do not forget to commit the new `composer.lock` file to your project afterwards.

Currently every `composer update` triggers a `bin/console sw:migration:migrate` since it would be possible that
Shopware itself got updated and need a new schema version to run properly. If you want to disable this behaviour you can
remove the `post-update-cmd` hook in the `composer.json` or modify the `app/post-update.sh` according to your needs.
