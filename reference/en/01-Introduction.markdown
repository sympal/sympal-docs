Sympal is a Content Management System built on top of the popular MVC
framework for PHP, [Symfony](http://www.symfony-project.org "Symfony").

> **NOTE**
> This documentation is written with the assumption that you already have
> some knowledge of Symfony. The
> [2009 Advent calendar](http://www.symfony-project.org/advent_calendar)
> is a great way to get started learning Symfony quickly! It is a set of
> 24 tutorials about advanced symfony topics, published day-by-day between
> December 1st and Christmas 2009. All tutorials are available in five languages: 
> English, French, Spanish, Italian, and Japanese.
> 
> These tutorials have also been published by 
> [Sensio Labs Books](http://books.sensiolabs.com/), under the title of 
> "[More with symfony 1.3 & 1.4](http://books.sensiolabs.com/book/9782918390176)"

## Symfony Plugins

Sympal is intended to be extremely flexible and modular. Due to this Sympal
is made up of multiple Symfony plugins. Below you can find a brief list of
the plugins which make up the core of Sympal.

**Core Sympal Plugins**

Sympal core is made up of a few Sympal based plugins:

* **sfSympalPlugin** - The core of Sympal
* **sfSympalAdminPlugin** - Backend admin functionality
* **sfSympalAssetsPlugin** - Assets management functionality
* **sfSympalContentListPlugin** - ContentList content type for easily creating lists of content
* **sfSympalDataGridPlugin** - OO library for creating data grid lists powered by Doctrine
* **sfSympalFrontendEditorPlugin** - Frontend inline editor functionality
* **sfSympalInstallPlugin** - Web and CLI installation functionality
* **sfSympalMenuPlugin** - Sympal menu functionality
* **sfSympalPagesPlugin** - Default Page content type
* **sfSympalPluginManagerPlugin** - Sympal Plugin Web and CLI installation and management functionality
* **sfSympalRenderingPlugin** - Render frontend content
* **sfSympalUpgradePlugin** - Sympal upgrade management
* **sfSympalUserPlugin** - Additional functionality for sfDoctrineGuardPlugin and use related functionality

**Core Addon Plugins**

In addition to the core Sympal plugins we also bundle some other useful Symfony plugins:

* **sfDoctrineGuardPlugin** - Use management
* **sfJqueryReloadedPlugin** - jQuery helpers, etc.
* **sfFeed2Plugin** - Feed parsing and generation
* **sfWebBrowserPlugin** - Required by sfFeed2Plugin
* **sfFormExtraPlugin** - Additional form goodies, widgets, validators, etc.
* **sfTaskExtraPlugin** - Additional useful Symfony tasks

> **NOTE**
> The above plugins are normal Symfony plugins but are included in 
> Sympal because the functionality they provide is very useful while working 
> with Sympal and in general when working in Symfony. We'll discuss later in
> more detail the functionality they provide.

All of these plugins together implement the basic functionality of a content 
management system as well as an extensible infrastructure so developers
can add their own content types and Sympal plugins easily. They can also
easily interact and manipulate other Sympal plugins by taking advantage
of implemented hooks and events which will be discussed later.

## Features

**Multiple Sites**

Easily manage multiple sites from within one Sympal project.

**Page Caching**

Easily turn on and off page caching for Sympal. You can easily cache your
pages with or without the layout.

**Security**

You can control who can edit/view content with the users, groups and 
permissions system.

**Internationalization**

Easily enable internationalization for any of your content. Any property
in your database can be internationalized by simply changing some
configuration values.

**Site Menus**

Manage your site menus from a nice web interface and easily render them
in your site layouts with very little work.

**Breadcrumbs Generation**

Easily generate breadcrumbs from your current location in the sitemap or
generate breadcrumbs manually from an array of input.

**Custom Content Types**

Expand Sympal content by adding your own content types (i.e. BlogPost,
Article, etc.)

**Custom Slot Types**

Even more specifically your content can have multiple defined "slots" that
contain user input. You can add your own types that allow you to customize
how the contents are edited and rendered.

**CLI & Web Based Installer**

Easily install Sympal from a command line or web based installer.

**CLI & Web Based Plugin Installer**

You can easily browse and install available Sympal plugins from the web
based or command line installer.

**Templates**

The templates used to render content can be configured and overridden at
multiple levels of Sympal. Because of this rendering certain content with
different templates is possible.

**Layouts**

The same goes for layouts as templates. You can customize what layout to
use at multiple levels. For example you can set a global layout, site
layout, content type layout, or even customize the layout for an individual
content record.