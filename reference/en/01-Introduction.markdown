Sympal is a Content Management System built on top of the popular MVC
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

## Symfony Plugins

Sympal is intended to be extremely flexible and modular. Due to this, sympal
is made up of multiple symfony plugins. Below you can find a brief list of
the plugins which make up the core of sympal.

**Core Sympal Plugins**

The core of Sympal is made up of several plugins:

* **sfSympalPlugin** - The core of sympal containing basic functionality needed by all plugins
* **sfSympalAdminPlugin** - Backend admin functionality
* **sfSympalAssetsPlugin** - Assets management functionality
* **sfSympalCMFPlugin** - Contains the content models and core of the CMf
* **sfSympalContentListPlugin** - ContentList content type for easily creating lists of content
* **sfSympalContentSyntaxPlugin** - Interface for transforming content
* **sfSympalDataGridPlugin** - OO library for creating data grid lists powered by Doctrine
* **sfSympalEditorPlugin** - Frontend inline editor functionality
* **sfSympalInstallPlugin** - Web and CLI installation functionality
* **sfSympalMenuPlugin** - Sympal menu functionality
* **sfSympalMinifyPlugin** - Handles the minification of css and js
* **sfSympalPagesPlugin** - Default Page content type
* **sfSympalPluginManagerPlugin** - Sympal Plugin Web and CLI installation and management functionality
* **sfSympalRenderingPlugin** - Render frontend content
* **sfSympalSearchPlugin** - Search functionality
* **sfSympalThemePlugin** - Functionality for the creation and switching of themes
* **sfSympalUpgradePlugin** - Sympal upgrade management
* **sfSympalUserPlugin** - Additional functionality for sfDoctrineGuardPlugin and use related functionality

**Core Addon Plugins**

In addition to the core sympal plugins, we also bundle some other useful
and well-known symfony plugins:

* **sfDoctrineGuardPlugin** - User management
* **sfFeed2Plugin** - Feed parsing and generation
* **sfFormExtraPlugin** - Additional form goodies, widgets, validators, etc.
* **sfFormImageTransformPlugin** - Powerful image transformation plugin
* **sfJqueryReloadedPlugin** - jQuery helpers, etc.
* **sfTaskExtraPlugin** - Additional useful symfony tasks
* **sfWebBrowserPlugin** - Required by sfFeed2Plugin

> **NOTE**
> The above plugins are normal symfony plugins but are included in 
> sympal because the functionality they provide is very useful both while
> working with Sympal and in general. We'll discuss these plugins later in
> more detail.

All of these plugins together implement the basic functionality of a content 
management system as well as an extensible infrastructure so developers
can add their own content types and plugins easily. Each plugin can also
easily interact and manipulate other sympal plugins by taking advantage
of implemented hooks and events which will be discussed later.

## Features

**Inline Editing**

The content of each page can be modified inline on the frontend of the
site. Both the output and the editors for the content can be easily configured.

**Multiple Sites**

Easily manage multiple sites from within one sympal project.

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

**Layouts**

The same goes for layouts as templates. You can customize what layout to
use at multiple levels. For example you can set a global layout, site
layout, content type layout, or even customize the layout for an individual
content record. This is the idea of theming.