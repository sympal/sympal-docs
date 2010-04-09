Sympal is really just multiple Sympal plugins all bundled together and designed to work with each other to give you a fully featured CMS. Because of this, it means you can easily disable a plugin provided by Sympal and replace it with your own. Sympal is already extremely configurable, extensible and flexible. An example might be that you don't like the default inline editing and you want to replace it with something custom. You can do so by disabling the core `sfSympalEditorPlugin` and replace it with your own!

## Plugin Loader

In Sympal you can manage what all plugins get loaded and from where in your `ProjectConfiguration::setup()` method.

A typical `ProjectConfiguration` for Sympal would look something like the following:

    [php]
    public function setup()
    {
      require_once(dirname(__FILE__).'/../../../../config/sfSympalPluginConfiguration.class.php');
      sfSympalPluginConfiguration::enableSympalPlugins($this);

      $this->enableAllPluginsExcept('sfPropelPlugin');
    }

The above could would simply enable all Sympal plugins. Sometimes you may want to enable only specific plugins or override a plugin. You can do so by using the `sfSympalPluginEnabler`, an instance of this class is returned by the `enableSympalPlugins()` method.

    [php]
    public function setup()
    {
      require_once(dirname(__FILE__).'/../../../../config/sfSympalPluginConfiguration.class.php');
      $enabler = sfSympalPluginConfiguration::enableSympalPlugins($this);

      $this->enableAllPluginsExcept('sfPropelPlugin');
    }

Now you have an instance of `sfSympalPluginEnabler` stored in `$enabler`. This class has several methods to make it easy for you to work with Sympal plugins:

* **isSympalEnabled()** - Check if Sympal is enabled for the current application.
* **enableSympalPlugins()** - Enables all Sympal plugins.
* **enableSympalCorePlugins($plugins)** - Enable a Sympal core plugin.
* **disableSympalCorePlugins($plugins)** - Disable a Sympal core plugin.
* **overrideSympalPlugin($plugin, $newPlugin, $newPluginPath)** - Override a Sympal plugin with your own.

Here is an example where you  might override the `sfSympalEditorPlugin` with your own:

    [php]
    public function setup()
    {
      require_once(dirname(__FILE__).'/../../../../config/sfSympalPluginConfiguration.class.php');
      $enabler = sfSympalPluginConfiguration::enableSympalPlugins($this);
      $enabler->overrideSympalPlugin('sfSympalEditorPlugin', 'sfSympalEditorPlugin', '/path/to/my/sfSympalEditorPlugin');

      $this->enableAllPluginsExcept('sfPropelPlugin');
    }
##