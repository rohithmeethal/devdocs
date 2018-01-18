---
layout: default
title: Professional Deployments
github_link: sysadmins-guide/professional-deployments/index.md
indexed: true
group: System Guides
menu_title: Professional Deployments
menu_order: 40
---

## Professional Deployments

This guide is intended as a list of best practices to follow when professionally deploying Shopware shops.

### Development best practices

Professional deployment begins with professional development. The time when everybody edited files on a central
FTP server are long gone.

You should be using a version control system (VCS), preferably [git](https://git-scm.com/) to allow parallel development
on local machines while integrating these parts in a central point. See the [developers guide]({{ site.url }}/developers-guide/plugin-quick-start)

If you find yourself reusing plugins, themes or configuration, think about using [Composer](https://getcomposer.org/) and
the [Shopware Composer Project](https://github.com/shopware/composer-project) for your new projects. It will help you
to require plugins or libraries you repeatedly use.  

### Development, staging and production systems

Even if some feature works perfectly on the developers own machine, there is always a chance that some setting or the
integration with some feature of another developer breaks the shop. So it is a good idea to have one or more stages of
integrations; a common pattern is a three level system of development, staging and production systems. Of course all
these are just a recommendation - all of this can and should be modified to your situation, use cases
and workflows.

- The development system is where a first check is possible or where internal stakeholders can get a preview or glimpse
  of how a feature is about to be developed.

- The staging system is where a thoroughly tested feature is being shown to an external customer and features are combined
  into releases.

- The production system is what the end customer get's to see. If a staging system contains all relevant features
  for e.g. a certain milestone, these can be deployed together to this system.

### Deployments

Deployments should be as automatic as possible to prevent any chance of human error. It is generally a good idea to
not simply replace or update your webroot on the production system. Rather create a fresh clone of the latest production
release from your VCS, maybe run some automated tests to make sure no basic errors occured and just switch the webroot
of your webserver to point to your new release. Shared resources (like images, documents etc.) can be offloaded to a
CDN or themselves be included in your webroot by file system links.

Should anything be wrong with this release, you can still switch back to the old version (given the database wasn't
modified in a destructive way). This way you can minimize the downtime, or even eliminate it all together if you're
working in a cluster setup and are able to update machine by machine.

Try to not store credentials on disk to minimize the information an attacker might gain in case of file system access.
Rather use environment variable set e.g. in the webserver. The [Shopware Composer Project](https://github.com/shopware/composer-project)
supports environment variable for the setup of e.g. the database out of the box. 
