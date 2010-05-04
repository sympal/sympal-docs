# Introduction

A theme in Sympal is nothing more than a configured Symfony layout and
set of stylesheets and javascripts. You can create your own themes or
extend and customize existing ones easily.

## Configuration

Here is the configuration for the default theme bundled with Sympal:

    [yml]
    all:
      sympal_config:
        themes:
          default:
            layout: default
            stylesheets: [/sfSympalPlugin/css/default.css]

The above configuration defines a theme named `default` with a configured
layout and stylesheet. You can use this theme throughout Sympal many different
ways. Continue reading to learn how to use Sympal themes and how to create
your own.

## Usage

You can use Sympal themes lots of different ways. Since you may want to
use certain themes in all sorts of different situations, we need to provide
many different ways for you to configure the theme to use at many different
levels. Here are the ways you can configure the theme Sympal should use
when issuing a request:

### Global Default Theme

The global default theme can be configured in your `config/app.yml`:

    [yml]
    all:
       sympal_config:
         default_theme: wordpress_default

This is the theme that is loaded if no other theme is specified at any
other level during the execution of a Sympal request. The default theme
is the `default` theme but in the above example we changed the default
to `wordpress_default` which is a port of the default Wordpress theme
for Sympal.

### Site Theme

You can configure the theme a site should use by editing the site record
in the Sympal admin area by going to Administration > Sites. When you
edit your site you will see a dropdown of the available themes:

[asset:99 title=Change site theme]

If you don't want to change the theme in the database, you can also
configure it from the `app.yml` for your sites Symfony application. For
example a site named `sympal` could have it's default theme customized
by editing `apps/sympal/config/app.yml` and adding the following:

    [yml]
    all:
       sympal_config:
         default_theme: sympal

Now the `sympal` site will use `sympal` by default and override any setting
that exists in the project `config/app.yml`.

>**NOTE**
>The Sympal theme is the theme used to build the website for the Sympal
>project itself. You can use this theme if you like, use another or create
>your own!

### Content Type Theme

In addition to sites you can configure what theme should be used for
every content type this can also be done in both the database from the
interface or from the Symfony configuration.

From the Sympal admin you just need to go to Administration > Content Types
and edit your content type. You will see a dropdown of available themes
just like you did for the site:

[asset:99 title=Change content type theme]

To change the default theme for a content type from the `app.yml` you can
just add some YAML like the following:

    [yml]
    all:
      sympal_config:
        page:
          default_theme: wordpress_default

Now the `page` content type that comes with Sympal will use the `wordpress_default` 
theme by default.

### Content Record Theme

Lastly you can customize the theme a specific piece of content should use
by selecting the theme from the dropdown when editing your content:

[asset:100 title=Change content theme]

## Creating a Theme

You can easily create your own theme by just editing your `config/app.yml`,
adding a theme configuration and adding the defined files.

    [yml]
    all:
      sympal_config:
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
>This file can be located in your application templates directory or a
>plugins templates directory.
>Sympal will find the location of the file so all you need to do is specify
>the name of the layout.

Now for the CSS file we just need to create a file in `project/web/css/test_theme.css`.

### Generating

To make things simpler, we can easily generate a new theme in a plugin
and automate the above steps a bit! Just run the following command and
you will have a new plugin with a copy of the default Sympal theme ready
for you to customize!

    $ php symfony sympal:plugin-generate TestTheme --theme=test_theme

If you have a look now you will see a new plugin named `sfSympalTestThemePlugin`
in your plugins folder:

[asset:105 title=Generated theme]

After generating the theme plugin you just need to install it with the the
following command to make everything available:

    $ php symfony sympal:plugin-install TestTheme