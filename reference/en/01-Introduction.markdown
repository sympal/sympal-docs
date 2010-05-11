Sympal is a content management framework built on top of the popular MVC
framework for PHP, [symfony](http://www.symfony-project.org "symfony").

> **NOTE**
> This documentation is written with the assumption that you already have
> some knowledge of symfony. The
> [2009 Advent calendar](http://www.symfony-project.org/more-with-symfony/1_4/en/)
> is a great way to get started learning symfony quickly! It is a set of
> 24 tutorials about advanced symfony topics, published day-by-day between
> December 1st and Christmas 2009. All tutorials are available in five languages: 
> English, French, Spanish, Italian, and Japanese.
> 
> These tutorials have also been published by 
> [Sensio Labs Books](http://books.sensiolabs.com/), under the title of 
> "[More with symfony 1.3 & 1.4](http://books.sensiolabs.com/book/9782918390176)"

## What is sympal really?

Sympal is a toolset for creating a content-based website or portion of a
website. It's a collection of plugins that add to your project, but do not
take it over. It can be easily added to an existing, symfony project in order
to add content management functionality.

## Symfony Plugins

Sympal is intended to be extremely flexible and modular. As mentioned above,
sympal is actually a plugin, `sfSympalPlugin`, which, for organizational
purposes, comes packaged with a set of core plugins inside of it:

**Core Sympal Plugins**

The core of Sympal is made up of several plugins:

* **sfSympalPlugin** - The core of sympal containing basic functionality needed by all plugins
* **sfSympalAdminPlugin** - Backend admin functionality
* **sfSympalAssetsPlugin** - Assets management functionality
* **sfSympalContentListPlugin** - ContentList content type for easily creating lists of content
* **sfSympalDataGridPlugin** - OO library for creating data grid lists powered by Doctrine
* **sfSympalEditorPlugin** - Frontend inline editor functionality
* **sfSympalInstallPlugin** - Web and CLI installation functionality
* **sfSympalMenuPlugin** - Sympal menu functionality
* **sfSympalMinifyPlugin** - Handles the minification of css and js
* **sfSympalPagesPlugin** - Default Page content type
* **sfSympalPluginManagerPlugin** - Sympal Plugin Web and CLI installation and management functionality
* **sfSympalRenderingPlugin** - Render frontend content
* **sfSympalSearchPlugin** - Search functionality
* **sfSympalUpgradePlugin** - Sympal upgrade management
* **sfSympalUserPlugin** - Additional functionality for sfDoctrineGuardPlugin and use related functionality

**Core Addon Plugins**

In an effort to reinvent as little as possible, sympal also packages a few
other, trusted, plugins in its core:

* **sfDoctrineGuardPlugin** - User management
* **sfFeed2Plugin** - Feed parsing and generation
* **sfFormExtraPlugin** - Additional form goodies, widgets, validators, etc.
* **sfFormImageTransformPlugin** - Powerful image transformation plugin
* **sfJqueryReloadedPlugin** - jQuery helpers, etc.
* **sfTaskExtraPlugin** - Additional useful symfony tasks
* **sfWebBrowserPlugin** - Required by sfFeed2Plugin
* **sfThemePlugin** - symfony theming plugin
* **sfContentFilterPlugin** - Allows for cached content transformation
* **sfInlineObjectPlugin** - An inline object syntax

All of these plugins together implement the basic functionality of a content 
management system as well as an extensible infrastructure so developers
can add their own content types and plugins easily. Each plugin can also
easily interact and manipulate other sympal plugins by taking advantage
of hooks and events which will be discussed later.

## Features

**Multiple Sites**

Easily manage multiple sites from within one sympal project.

**Inline Editing**

The content of each page can be modified inline on the frontend of the
site. Both the output and the editors for the content can be configured.

**Page Caching**

Easily turn on and off page caching for sympal. You can easily cache your
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

**Breadcrumb Generation**

Easily generate breadcrumbs from your current location in the sitemap or
generate breadcrumbs manually from an array of input.

**Custom Content Types**

Expand sympal content by adding your own content types (i.e. BlogPost,
Article, etc.)

**CLI & Web Based Installer**

Easily install Sympal from a command line or web based installer.

**CLI & Web Based Plugin Installer**

You can easily browse and install available sympal plugins from the web
based or command line installer.

**Templates**

The templates used to render content can be configured and overridden at
multiple levels of sympal. Because of this, the same content can be rendered
using multiple different templates.

**Themes**

The same goes for themes. You can customize what theme to use at multiple
levels. For example you can set a global theme, site theme, content type
theme, or even customize the theme for an individual content record.