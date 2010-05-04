# Introduction

A theme in sympal is nothing more than a symfony layout and set of
stylesheets and javascripts. You can create your own themes or extend
and customize existing ones easily.

>**NOTE**
>Sympal relies on the standalone symfony plugin [sfThemePlugin](http://github.com/weaverryan/sfThemePlugin)
>for theming and adds a few additional features. The above link is the best
>reference for learning all about theming.

## Configuration

Here is the configuration for the default theme bundled with sympal:

    [yml]
    all:
      theme:
        themes:
          default:
            layout: default
            stylesheets: [/sfSympalPlugin/css/default.css]

The above configuration defines a theme named `default` with a configured
layout and stylesheet. You can use this theme throughout sympal in many
different ways. Continue reading to learn how to use sympal themes and
how to create your own.

## Usage

You can use sympal themes in lots of different ways. Since you may want to
use certain themes in all sorts of different situations, we need to provide
many different ways for you to configure the theme to use at many different
levels. Here are the ways you can configure which theme sympal should use
when issuing a request:

### Global default theme

The global default theme can be configured in your `config/app.yml`:

    [yml]
    all:
      theme:
        controller_options:
          default_theme: wordpress_default

This is the theme that is loaded if no other theme is specified at any
other level during the execution of a sympal request. Nn the above example
we've changed the default theme to `wordpress_default`, which is a port
of the default Wordpress theme made for sympal.

### Site Theme

You can configure the theme a specific site should use by editing the
site record in the sympal admin area by going to Administration > Sites.
When you edit your site you will see a dropdown of the available themes:

[asset:99 title=Change site theme]

If you don't want to change the theme in the database, you can also
configure it from the `app.yml` for your site's symfony application. For
example, a site named `sympal` could have its default theme customized
by editing `apps/sympal/config/app.yml` and adding the following:

    [yml]
    all:
       theme:
         controller_options:
           default_theme: sympal

Now, the `sympal` site will use `sympal` theme by default and override
any setting that exists in the project's `config/app.yml` file.

>**NOTE**
>The sympal theme is the theme used to build the website for the sympal
>project itself. You can use this theme if you like, use another or create
>your own!

### Content type theme

In addition to sites, you can configure what theme should be used for
every content type. This can also be done in both the database, through
the admin area, or via the symfony configuration.

From the sympal admin, go to `Administration > Content Types` and edit
your content type. You will see a dropdown of available themes just like
you did for the site:

[asset:99 title=Change content type theme]

To change the default theme for a content type via `app.yml`, use the
following YAML code:

    [yml]
    all:
      sympal_config:
        sfSympalPage:
          default_theme: wordpress_default

Now, the `sfSympalPage` content type that comes with sympal will use the
`wordpress_default` theme by default.

### Content record theme

Lastly, you can also customize the theme for a specific piece of content.
To do this, select the theme from the dropdown when editing your content:

[asset:100 title=Change content theme]

### Other methods

There are various other methods for setting the correct theme for a request.
See the [sfThemePlugin](http://github.com/weaverryan/sfThemePlugin) for the
full documentation.

## Creating a theme

You can easily create your own theme by just editing your `config/app.yml`,
adding a theme configuration and adding the defined files.

    [yml]
    all:
      theme:
        themes:
          test_theme:
            layout: test_theme
            stylesheets: [/css/test_theme.css]

Now we just need to define the `test_theme.php` file:

    [php]
    <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN"
     "http://www.w3.org/TR/html4/strict.dtd">
    <html>
    <head>
      <?php include_http_metas() ?>
      <?php include_metas() ?>
      <?php include_title() ?>
      <?php sympal_minify() ?>
      <?php include_stylesheets() ?>
      <?php include_javascripts() ?>
    </head>
    <body>
      <?php echo $sf_content ?>
    </body>
    </html>

>**NOTE**
>This file can be located in your application's templates directory or a
>plugin's templates directory. Sympal will find the location of the file
>so all you need to do is specify the name of the layout.

Now for the CSS file we just need to create a file in `web/css/test_theme.css`.

### Generating

To make things simpler, we can easily generate a new theme in a plugin
and automate the above steps a bit! Just run the following command and
you will have a new plugin with a copy of the default sympal theme ready
for you to customize!

    $ php symfony sympal:plugin-generate TestTheme --theme=test_theme

If you have a look now, you will see a new plugin named `sfSympalTestThemePlugin`
in your plugins directory:

[asset:105 title=Generated theme]

After generating the theme plugin you just need to install it with the the
following command to make everything available:

    $ php symfony sympal:plugin-install TestTheme

>***NOTE***
>The above command creates the necessary symbolic link in your web directory
>to the web directory in the plugin and ensures that the permissions inside
>the plugin are properly set.