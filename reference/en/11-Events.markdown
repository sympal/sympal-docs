# Introduction

One of the greatest features of Sympal is that it takes advantage of the 
wonderful event system that Symfony provides. Sympal implements dozens of events 
that are strategically placed in the core code so that you can connect to them 
and manipulate functionality without altering any core code.

In this chapter we will describe all the different events in Sympal and how you 
can utilize them.

# Installation

The installation of Sympal offers two events that you can connect to. These are 
simple events and allow you to execute your own code before and after the 
installation runs. Below you will find examples for both.

## Pre Install

You can easily connect to the `sympal.pre_install` event and execute some custom 
code before Sympal is installed.

    [php]
    class sfSympalTestPluginConfiguration extends sfPluginConfiguration
    {
      public function initialize()
      {
        $this->dispatcher->connect('sympal.pre_install', 
            array($this, 'sympalPreInstall'))
      }

      public function sympalPreInstall(sfEvent $event)
      {
        // do some custom code
      }
    }

## Post Install

You can also just as easily connect to the `sympal.post_install` event and 
execute some custom code after Sympal is installed.

    [php]
    class sfSympalTestPluginConfiguration extends sfPluginConfiguration
    {
      public function initialize()
      {
        $this->dispatcher->connect('sympal.post_install', 
             array($this, 'sympalPostInstall'))
      }

      public function sympalPostInstall(sfEvent $event)
      {
        // do some custmo code
      }
    }

# Models

Sympal implements a few events in Doctrine models so that you can easily 
customize models, add methods, etc. from your own code.

## Setup

When using Sympal you can alter any aspect of the a model during its setup and 
initialization. This allows Sympal plugins to add columns to models, remove 
columns, add relationships, etc. Below you will find some examples of how to 
utilize these events.

Imagine you have a Sympal plugin name `sfSympalUserProfilePlugin` which 
introduces a new model named `Profile` and adds a foreign key `profile_id` to 
the `User` model and adds the relationship between the two. First we need to 
hook in to the set table definition for the `User` model to add the `profile_id` 
column.

> **TIP**
> The `model_name` is what to use if the model was named `ModelName`. The event 
> names are always lower case and underscored. Even if your model name is 
> capitalized.

Now we can add the column and our relationship to the `User` model with the 
following code.

    [php]
    class sfSympalUserProfilePluginConfiguration extends sfPluginConfiguration
    {
      public function initialize()
      {
        $this->dispatcher->connect('sympal.user.set_table_definition', 
            array($this, 'userSetTableDefinition'));

        $this->dispatcher->connect('sympal.user.set_up',
            array($this, 'userSetUp'));
      }

      public function userSetTableDefinition(sfEvent $event)
      {
        $subject = $event->getSubject();
        $subject->hasColumn('profile_id', 'integer');
      }

      public function userSetUp(sfEvent $event)
      {
        $subject = $event->getSubject();
        $subject->hasOne('Profile', array(
          'local' => 'profile_id',
          'foreign' => 'id'
        ));
      }
    }

Now our `User` model has a relationship to the `Profile` object and was all done 
without actually modifying any core code.

    [php]
    $user = new User();
    $profile = $user->Profile;

# Content Format

Sympal offers your content automatically in several formats. If you have a new 
format you wish to add you can easily do so by connecting to the 
`sympal.content_renderer.unknown_format` event.

    [php]
    class sfSympalTestPluginConfiguration extends sfPluginConfiguration
    {
      public function initialize()
      {
        $this->dispatcher->connect('sympal.content_renderer.unknown_format', 
            array($this, 'listenForUnknownFormat'));
      }

      public function listenForUnknownFormat(sfEvent $event)
      {
        // Set the extension name of your format
        $event['format'] = 'encryptedJson';

        // Build the output for your format
        $content = $event['content'];
        $array = $content->toArray(true);
        $json = json_encode($array);
        $encrypted = $this->someEncryptFunction($json);

        $event->setProcessed();
        $event->setReturnValue($encrypted);

        return true;
      }

      public function someEncryptFunction($json)
      {
        // encrypt the json with some key/salt
        // so the retrieving end can decrypt it

        return $json;
      }
    }

You can also take a look at the `sfSympalContentListPlugin` which is a core 
Sympal plugin. It adds feeds functionality to your content lists. It can be 
found in `sfSympalPlugin/lib/plugins/sfSympalContentListPlugin`.

# Admin Menu

The admin menu in Sympal is a central administrative navigation menu that you can 
connect to and add new menus, or alter existing menus. You can add to this admin menu by connecting to the `sympal.load_admin_menu` event.

In this example we'll add a new child to the Administration menu.

    [php]
    class sfSympalTestPluginConfiguration extends sfPluginConfiguration
    {
      public function initialize()
      {
        $this->dispatcher->connect('sympal.load_admin_menu',
            array($this, 'loadAdminMenu'));
      }

      public function loadAdminMenu(sfEvent $event)
      {
        $menu = $event->getSubject();
        $menu->getChild('Administration')
          ->addChild('New Item', '@new_item_route');
      }
    }

This is useful if you write a Sympal plugin that offers some new modules for 
administering the functionality of your plugin. You can connect to the above 
event and add links to these modules.

# Editor Menu

The editor menu is the dropdown menu in the inline edit car that shows when you are editing some content. All the contents of this panel are populated via events. So even the items you see by 
default are added via these events.

[asset:127 title=Editor menu]

To add to menu you can simply connect to the `sympal.load_editor` 
event.

    [php]
    class sfSympalTestPluginConfiguration extends sfPluginConfiguration
    {
      public function initialize()
      {
        $this->dispatcher->connect('sympal.load_editor',
            array($this, 'loadEditor'));
      }

      public function loadEditor(sfEvent $event)
      {
        $menu = $event->getSubject();
        $menu->addChild('New Item', '@new_item_route');
      }
    }

This is useful if you add some new functionality to content and want to show 
controls for the functionality in the editor menu.

# Actions

You can easily add functionality to all your actions class by connecting to the 
`component.method_not_found` event. This is a event provided by the Symfony 
core.

    [php]
    class sfSympalTestPluginConfiguration extends sfPluginConfiguration
    {
      public function initialize()
      {
        $this->dispatcher->connect('component.method_not_found',
            array(new myActionsExtension(), 'extend'));
      }
    }

    class myActionsExtension extends sfSympalExtendClass
    {
      public function goBack()
      {
        $this->redirect($this->getRequest()->getReferer());
      }
    }

The above code would make available a `goBack()` method in all your actions 
class that goes back to the referer.

> **NOTE**
> Sympal uses this event internally and makes available several new functions to 
> your actions class. `goBack()` is in fact one of them. For demonstration 
> purposes we used this feature as an example above.

    [php]
    class my_moduleActions extends sfActions
    {
      public function executeSave_comment(sfWebRequest $request)
      {
        // save comment

        // go back to where we came from
        $this->goBack();
      }
    }

# Configuration Form

Sympal provides a means to edit all the configurations from a web interface. The 
default way to edit configuration values is by manually overriding the `app.yml` 
file and changing values. If you want your configurations to be editable in the 
browser then you can connect to the `sympal.load_config_form` event and add the 
settings to the form.

## Adding Configuration

Imagine you had a `sfSympalTestPlugin` and you have a `config/app.yml` that 
looks like the following:

    [yml]
    all:
      sympal_config:
        test:
          my_setting: true

Now if you want that setting to be editable from the browser you can do the 
following.

    [php]
    class sfSympalTestPluginConfiguration extends sfPluginConfiguration
    {
      public function initialize()
      {
        $this->dispatcher->connect('sympal.load_config_form',
            array($this, 'loadConfig'));
      }

      public function loadConfig(sfEvent $event)
      {
        $form = $event->getSubject();
        $form->addSetting('test', 'my_setting',
            'My Setting', 'InputCheckbox', 'Boolean');
      }
    }

## Pre Save

You can hook in to the saving process of the configuration form and execute some 
custom code before it is saved.

    [php]
    class sfSympalTestPluginConfiguration extends sfPluginConfiguration
    {
      public function initialize()
      {
        $this->dispatcher->connect('sympal.config_form.pre_save',
            array($this, 'configFormPreSave'));
      }

      public function configFormPreSave(sfEvent $event)
      {
        $form = $event->getSubject();

        // execute some custom code
      }
    }

## Post Save

You can hook in to the saving process of the configuration form and execute some 
custom code after it is saved.

    [php]
    class sfSympalTestPluginConfiguration extends sfPluginConfiguration
    {
      public function initialize()
      {
        $this->dispatcher->connect('sympal.config_form.post_save',
            array($this, 'configFormPostSave'));
      }

      public function configFormPostSave(sfEvent $event)
      {
        $form = $event->getSubject();

        // execute some custom code
      }
    }

# Dashboard

The contents of the dashboard you see when you login are populated by events as 
well. So if you want to add a new box or change something on the dashboard then 
you can do by connecting to some events.

## Boxes

First if you want to add some new boxes to the dashboard you can do so by 
connecting to the `sympal.load_dashboard_boxes` event.

    [php]
    class sfSympalTestPluginConfiguration extends sfPluginConfiguration
    {
      public function initialize()
      {
        $this->dispatcher->connect('sympal.load_dashboard_boxes',
            array($this, 'loadDashboardBoxes'));
      }

      public function loadDashboardBoxes(sfEvent $event)
      {
        $menu = $event->getSubject();
        $menu->addChild('New Box', '@new_box_route');
      }
    }

# Content Rendering

When a Sympal content record is rendered we notify a few events to give you an 
opportunity to change something about the rendering.

## Filter Slot Content

You can filter the rendered slot value with the `sympal.content_renderer.filter_slot_content` event. 

    [php]
    class sfSympalTestPluginConfiguration extends sfPluginConfiguration
    {
      public function initialize()
      {
        $this->dispatcher->connect('sympal.content_renderer.filter_slot_content',
            array($this, 'filterSympalSlotContent'));
      }

      public function filterSympalContent(sfEvent $event, $content)
      {
        // Manipulate the $content string
        // Add to it
        // Change it, etc..

        return $content;
      }
    }

## Filter Content

After some content has been rendered you can use a filter event to filter the 
html of the rendered content. This is a similar concept to the core Symfony 
event named `response.filter_content` which allows you to alter the response 
HTML before it is sent to the browser.

    [php]
    class sfSympalTestPluginConfiguration extends sfPluginConfiguration
    {
      public function initialize()
      {
        $this->dispatcher->connect('sympal.content_renderer.filter_content',
            array($this, 'filterSympalContent'));
      }

      public function filterSympalContent(sfEvent $event, $content)
      {
        // Manipulate the $content string
        // Add to it
        // Change it, etc..

        return $content;
      }
    }

> **TIP**
> The web debug toolbar is added to the HTML automatically by Symfony by 
> connecting to the `response.filter_content` event.