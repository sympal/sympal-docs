Everything in sympal was designed to be as configurable as possible. This
is why  the `sfSympalConfig` class was introduced. It is an extension of
`sfConfig` made specifically for sympal.

All of sympal's configuration is ultimately housed in an `app.yml` file,
which is a standard symfony practice. Each configuration value lives
somewhere beneath the `app_sympal_config` configuration key.

The `sfSympalConfig` class was introduced to ease the retrieval and manipulation
of all of the values beneath `app_sympal_config`. It works through symfony's
native configuration but makes life easier when working with sympal configuration.

## Groups and Values

Beneath the `app_sympal_config` config key, many configuration values are
organized into groups. For example, consider the following piece
of sympal's main configuration file, found inside `config/app.yml`:

    [yml]
    all:
      sympal_config:
        rows_per_page: 20
        
        inline_editing:
          enabled:            true
          default_edit_mode:  popup

In this example, `rows_per_page` does _not_ appear in a group. However,
the `enabled` and `default_edit_mode` config keys live inside the
`inline_editing` group.

Most of sympal's configuration exists inside a group, which helps the configuration
stay organized both in the code and in the admin area. As we'll show next,
configuration values are retrieved and set differently depending on whether
or not they live inside a group.

## Manipulating and Retriving configuration

You can easily manipulate and modify the sympal configuration by using
the `sfSympalConfig` class.

### Setting

Setting sympal configuration values is very similar to sfConfig. As mentioned
above, the exact syntax depends on whether or not the configuration is
organized into a group.

    [php]
    // For top-level configuration
    sfSympalConfig::set('rows_per_page', '20');

    // For configuration inside the group "inline_editing"
    sfSympalConfig::set('inline_editing', 'enabled', true);

### Getting

Getting Sympal configuration values is also similar to `sfConfig`:

    [php]
    // For top-level configuration
    echo sfSympalConfig::get('rows_per_page', null, 20);

    // For configuration inside the group "inline_editing"
    echo sfSympalConfig::get('inline_editing', 'enabled', true);

In the above example, `20` is the default value returned if the `rows_per_page`
configuration value does not exist. Similarly, `true` is the default value
returned if the `enabled` configuration value of the `inline_editing`
group does not exist.

### Writing

You can write new settings to disk by using the `writeSetting` method.
The arguments for this method are the same as the `set` method. The only
difference is that the value is physically written to disk as well as
saved in memory.

    [php]
    // For top-level configuration
    sfSympalConfig::writeSetting('rows_per_page', 20);

    // For configuration inside the group "inline_editing"
    sfSympalConfig::writeSetting('inline_editing', 'enabled', true);

## Configuration Files

Sympal comes packaged with sensible defaults for all of the configuration
values. These defaults can be easily customized by creating an `app.yml`
file in either the `config` directory of your project. If you're running
multiple sites from your sympal project, the `app.yml` file can be placed
in the `config` directory of an application for site-specific defaults.

Below is an example where we change some of the default values set by
sympal in our `config/app.yml` file:

    [yml]
    all:
      sympal_config:
        # Disable markdown editor
        enable_markdown_editor: false

        # Disable elastic textareas
        elastic_textareas: false

        # Enable page caching with the layout
        page_cache:
          enabled: true
          with_layout: true
          lifetime: 3600

## Web Editable Configuration

Many of these configuration values are things that can be changed and
modified after sympal has been installed. For many of these values, you
may want to allow a CMS user to be able to edit them from within sympal
in their browser.

If you login to sympal as an administrator and browse to
**Global Setup > System Settings**, you will see a form that lets you edit
certain configuration values.

[asset:48]

This form is dynamically built through the firing of an event named
`sympal.load_config_form`. You can connect to this event from anywhere and
add new editable configurations to the form. Below is an example of doing
this from inside a plugin configuration file:

    [php]
    public function initialize()
    {
      $dispatcher->connect('sympal.load_config_form', array($this, 'loadConfig'));
    }

    public function loadConfig(sfEvent $event)
    {
      $form = $event->getSubject();

      $form->addSetting('some_group', 'enabled',
          'Some Enabled Config', 'InputCheckbox', 'Boolean');
      $form->addSetting(null, 'no_group', 'No Group Config');
    }

The arguments for `addSetting` are as follows:

* **$groupName** - The group name the configuration is under. If none, specify null.
* **$configName** - The name of the configuration value.
* **$configLabel** - The label of the configuration value for the form.
* **$widget** - The name or instance of the widget to edit the value.
* **$validator** - The name or instance of the validator to validate the value.

> **NOTE**
> If you do not specify a widget or validator it defaults to `sfWidgetFormInput` 
> and `sfValidatorString`.