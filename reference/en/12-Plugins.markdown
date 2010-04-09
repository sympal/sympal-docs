# Introduction

Sympal plugins are much the same as normal Symfony plugins. The only real difference is that if they follow some additional rules and standardizations they can be easily one-click installed and integrate in to your existing Sympal project without having to do the actual implementation.

This is possible because of the Sympal events that are implemented. We talked about all the events available in a previous chapter.

# Generation

Generating a new Sympal plugin is easy. It can be done from the command line with the following command. In the following example we're going to create a new Sympal plugin that provides `Article` functionality.

    $ php symfony sympal:plugin-generate Article --content-type=Article

The above command will generate a new plugin named `sfSympalArticlePlugin`.

> **NOTE**
> All Sympal plugins are forced to have the same name format of sfSympal**[NAME]**Plugin

Now lets take a quick look at the file structure that was generated for us in `project/plugins/sfSympalArticlePlugin`.



    sfSympalArticlePlugin
      config/
        doctrine/
          schema.yml
        routing.yml
        sfSympalArticlePluginConfiguration.class.php
      data/
        fixtures/
          install.yml
      lib/
      LICENSE
      package.xml.tmpl
      README

# Configuration

The configuration of a Sympal plugin is controlled through the normal configuration class and the `app` yaml configuration file.

## Dependencies

You can control what a Sympal plugin depends on by specifying the array of dependencies in the plugin configuration class.

    [php]
    class sfSympalArticlePluginConfiguration extends sfPluginConfiguration
    {
      public static
        $dependencies = array(
          'sfSympalPlugin'
        );

      // ...
    }

## App Yaml Configuration

All the configurations of a Sympal plugin are stored in the `app.yml` under the key named `sympal_config`. Below is what it looks like for the `sfSympalCommentsPlugin` which is an available addon Sympal plugin.

    [yml]
    all:
      sympal_config:
        sfSympalCommentsPlugin:
          default_status: Approved
          enabled: true
          enable_recaptcha: true
          requires_auth: true

## Adding to Configuration Form

Now if we wanted some of the above configurations to be editable via the configuration form we could easily do so by doing the following.

    [php]
    class sfSympalCommentsPluginConfiguration extends sfPluginConfiguration
    {
      // ...

      public function initialize()
      {
        $this->dispatcher->connect('sympal.load_config_form',
            array($this, 'loadConfig'));
      }

      public function loadConfig()
      {
        $form = $event->getSubject();

        $form->addSetting('sfSympalCommentsPlugin', 'enabled',
            'Enabled', 'InputCheckbox', 'Boolean');
      }
    }

Now when you try and view the Sympal Configuration form you will see that you can enable and disable the comments on your site easily.

> **TIP**
> The 4th and 5th arguments to the addSetting() method can be either the name of 
> a widget/validator or can be an instance of the widget/validator. If you just pass 
> the string name then it will be instantiated for you internally.
>
>     [php]
>     $widget = new sfWidgetFormInput();
>     $validator = new sfValidatorPass();
>     $form->addSetting('group', 'name', 'Label', $widget, $validator);

# Admin Menu

Sympal implements a global administrative menu that is populated by a Symfony event named `sympal.load_admin_bar`. You can connect to this event in your plugins to add new items to it.

Here is an example where `sfSympalCommentsPlugin` adds a new child to the `Administration` menu to allow you to go to the comments moderation.


    [php]
    class sfSympalCommentsPluginConfiguration extends sfPluginConfiguration
    {
      public function initialize()
      {
        $this->dispatcher->connect('sympal.load_admin_bar',
            array($this, 'loadAdminBar'));
      }

      public function loadAdminBar(sfEvent $event)
      {
        $menu = $event['menu'];

        if (sfSympalConfig::get('sfSympalCommentsPlugin', 'installed', false) 
            && sfSympalConfig::get('sfSympalCommentsPlugin', 'enabled'))
        {
          $commentTable = Doctrine::getTable('Comment')
              ->getNumPending();
          $menu->getChild('Administration')
              ->addChild('Comments ('.$commentTable.')', '@sympal_comments');
        }
      }
    }

Now when you look at the Administration menu you'll see a new menu item for the comments.

# Packaging

Packaging a Sympal plugin is as easy as ever due to the fact that Sympal bundles the sfTaskExtraPlugin which provides tasks that help with creating and packing Symfony plugins.

    $ php symfony plugin:package sfSympalArticlePlugin

It will ask you a series of questions related to your plugin and then it will generate a new package file in your `project/plugins/sfSympalArticlePlugin` with a name of `sfSympalArticlePlugin-1.0.0.tgz`

# Uploading

Now that we have the package generated we can upload it to the Symfony PEAR package repository.

> **NOTE**
> You will need an account on http://www.symfony-project.com in order to execute 
> the next steps. You can create an account [here](http://www.symfony-project.org/user/new).

Once you have your account and you're logged in you can create a new plugin by clicking the [Create a new plugin](http://www.symfony-project.org/plugins/new) link.

After creating your new plugin you can then upload your package file by clicking the `Admin` tab and scrolling down to the `New Release` section where it gives you a file upload form for uploading your package file.

# Plugin Sources

When you download a Sympal plugin, the plugin is either installed via the normal `plugin:install` task or it will be retrieved from one of the `plugin_sources` that can be configured with `sfSympalConfig` and the `app.yml` configuration file.

A plugin source can be a location on your disk or a path to a HTTP svn server with public access. If you look at `project/plugins/sfSympalPlugin/config/app.yml` you will see the following.

    [yml]
    all:
      sympal_config:
        # ...
        plugin_sources: ["svn.symfony-project.com/plugins"]
        # ...

Here is an example where we add a path on our system to the list of source as well as another svn source.

    [yml]
    all:
      sympal_config:
        plugin_sources: ["svn.symfony-project.com/plugins", "/path/to/plugins", "svn.mydomain.com/plugins"]

Now clear your cache and the new plugins will be able to download and install in to your sympal application.

    $ php symfony sympal:plugin-list

# Downloading

Downloading of Sympal plugins can be done through the browser or through the command line.

# Installing

Installation of Sympal plugins can be done through the browser, or through the command line. Below you will find directions for both.

## Browser

To install a plugin from your browser simply login as a user with access to the plugin manager and navigate to Administration > Plugin Manager. You will see a list of available plugins and you can click to download then install the desired plugins.

## Command Line

To install a plugin from the command line interface you can use the `sympal:plugin-download` command.

    $ php symfony sympal:plugin-download Blog

# Uninstalling

Uninstalling can be done from the browser or command line just like everything else.

## Browser

To uninstall a plugin from your browser simply login as a user with access to the plugin manager and navigate to Administration > Plugin Manager. You will see a list of available plugins and you can click to un-install any of the installed plugins.

## Command Line

    $ php symfony sympal:plugin-uninstall Blog

If you want to delete the plugin after uninstalling it from your system you can pass the `--delete` option.

    $ php symfony sympal:plugin-uninstall Blog --delete