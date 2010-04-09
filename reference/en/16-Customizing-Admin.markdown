The admin area of Sympal mostly just a collection of modules tied together by a common dashboard and menu. If you wish to add some functionality to the Sympal admin area you can do so by connecting to some events provided to you by Sympal. These events allow you to easily connect to and add items to the dashboard or menu. Below you will find some examples of how you can add items to the admin dashboard and menu!

## Dashboard

If you want to add an item to the admin dashboard you can do so by connecting to the `sympal.load_dashboard` event. Here is an example where we add a new item to the dashboard from our `ProjectConfiguration::setup()`:

    [php]
    // ...
    class ProjectConfiguration extends sfProjectConfiguration
    {
      // ...
      public function setup()
      {
        // ...

        $this->dispatcher->connect('sympal.load_dashboard', array($this, 'loadDashboard'));
      }

      public function loadDashboard(sfEvent $event)
      {
        $dashboard = $event->getSubject();
        $dashboard->addChild('My New Item', '@my_new_item');
      }
    }

Now when you view the dashboard you will see the new item on the dashboard:

[asset:97]

## Menu

If you want to add a new item to the menu in the admin area you can do so just in the same way you did the dashboard. Just connect to the event named `sympal.load_admin_menu`. Here is an example where we add a new item to the administration section of the menu.


    [php]
    // ...
    class ProjectConfiguration extends sfProjectConfiguration
    {
      public function setup()
      {
        // ...

        $this->dispatcher->connect('sympal.load_admin_menu', array($this, 'loadAdminMenu'));
      }

      public function loadAdminMenu(sfEvent $event)
      {
        $menu = $event->getSubject();
        $menu->
          getChild('Administration')->
            addChild('My New Item', '@homepage');
      }
    }

Now when you have a look at the admin menu you will see the new menu item:

[asset:98 title=picture-3.png]