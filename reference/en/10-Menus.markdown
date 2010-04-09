# Introduction

When working with most web applications, how to manage the menus is a 
reoccurring problem. Not only is simply doing it difficult, but doing it 
efficiently is difficult.

Thanks to the [Doctrine](http://www.doctrine-project.org) ORM we are able to
make use of the NestedSet behavior so that we can manage our menus and retrieve 
them efficiently.

No matter how many menus you have, or how big they are, or how many times you 
retrieve and render them. It will only ever required one database query!

# Menu Class

Before we explain how to render your site menus I want to explain to you the 
base classes that are used to do all the menu rendering.

Everything in Sympal is based around the `sfSympalMenu` class which can be found 
in the `sfSympalMenuPlugin` that is bundled with the Sympal core.

The menu class is a totally database agnostic implementation of a menu system. 
You can use the menu object standalone to build and render your own menus. Below 
is an example where we build our own menu instance and render it.

    [php]
    $menu = new sfSympalMenu('My Menu');

    $awesomeCompanies = $menu->addChild('Awesome Companies');
    $awesomeCompanies->addChild('Sensio Labs', 'http://www.sensiolabs.com');
    $awesomeCompanies->addChild('centre{source}', 'http://www.centresource.com');

    $searchEngines = $menu->addChild('Search Engines');
    $searchEngines->addChild('Google', 'http://www.google.com');
    $searchEngines->addChild('Yahoo!', 'http://www.yahoo.com');

> **NOTE**
> When you add a new child to a menu, the method `addChild` will return the
> instance of the new menu item.

Now to render a menu is as simple as echoing the object. This is possible 
because we implement a `__toString()` method.

    [php]
    echo $menu;

The above example would output the following html.

    <ul id="my-menu">
      <li id="awesome-companies">Awesome Companies
        <ul id="awesome-companies">
          <li id="sensio-labs"><a href="http://www.sensiolabs.com">Sensio Labs</a></li>
          <li id="centre-source"><a href="http://www.centresource.com">centre{source}</a></li>
        </ul>
      </li>
      <li id="search-engines">Search Engines
        <ul id="search-engines">
          <li id="google"><a href="http://www.google.com">Google</a>
          </li><li id="yahoo"><a href="http://www.yahoo.com">Yahoo!</a></li>
        </ul>
      </li>
    </ul>

> **NOTE**
> The output of the menu is not formatted like you see above. This HTML was 
> formatted for this documentation so it is more readable for you.

## Securing Menus

The menu class has features built in to it so you can restrict menu items to 
require authentication, no authentication or certain credentials.

You can secure a menu item by using the `requiresAuth()`, `requiresNoAuth()` and
`setCredentials()` methods.

    [php]
    $menu = new sfSympalMenu('My Menu');
    $child = $menu->addChild('New Child', '@new_child_route')
      ->requiresAuth(true);

Now the above child menu item won't show to the viewing user unless he is 
authenticated.

    [php]
    $child = $menu->addChild('New Child', '@new_child_route')
      ->requiresNoAuth(true);

We can do the same for non-authenticated users. The menu item will only show if 
the user is not authenticated.

    [php]
    $child = $menu->addChild('New Child', '@new_child_route')
      ->setCredentials(array('ViewChildren'));

Now the above child will not display unless the viewing user has a credential 
named `ViewChildren`.

> **TIP**
> If you want to check if a user has access to view a menu item you can use the 
> `checkUserAccess()` method.
>
>     [php]
>     if ($menu->checkUserAccess())
>     {
>       // do something
>     }


## Parents and Children

When working with menu items you can easily retrieve the parent and children 
items associated with it by calling the `getParent()` and `getChildren()` 
methods.

    [php]
    $child = $menu->addChild('Child');
    $parent = $child->getParent();
    // $menu === $parent

Now if we wanted to get the children of `$menu` we could just call the 
`getChildren()` method.

    [php]
    $children = $menu->getChildren();
    // $children is an array containing the one child we added above

## Level

Sometimes you may need to know how deep a menu item is in an existing tree. You 
can grab this value with the `getLevel()` method. The value is automatically 
determined for you and set automatically when you build your menu item trees 
using the `addChild()` method.

    [php]
    $menu = new sfSympalMenu('My Menu');
    $child1 = $menu->addChild('Child 1');
    $child2 = $child2->addChild('Child 2');
    
    echo $child2->getLevel(); // Would output 1

> **NOTE**
> The first level of children in a menu starts at 0 and increments from there.

## Hiding Children

Sometimes you may have a menu but want to hide the children from rendering. This 
is possible with the `showChildren()` method.

    [php]
    $menu = new sfSympalMenu('My Menu');
    $child = $menu->addChild('Child');
    $child->addChild('Child 1');
    $child->addChild('Child 2');
    
    $child->showChildren(false);
    
    echo $menu;

The above code would only output the following.

    <ul id="my-menu"><li id="child">Child</li></ul>

This is due to the fact that we specified to not show the children of the first 
child we added.


## Rendering

The rendering of a `sfSympalMenu` instance is handled by five methods: 
`render()`, `renderChild()`, `renderChildBody()`, `renderLink()` and 
`renderLabel()`.

The rendering of menus are split in to multiple methods to make it easier for 
developers to extend the `sfSympalMenu` class and customize the way a particular 
menu renders.

Here is an example where we override the `renderLabel()` method to wrap it in a
additional HTML tag.

    [php]
    class MyMenu extends sfSympalMenu
    {
      public function renderLabel()
      {
        return '<span class="my_menu_label">'.parent::renderLabel().'</span>';
      }
    }

## Call Method Recursively

Sometimes you may want to call a method on a menu and have it be called 
recursively on all children in the entire tree. This is possible with the 
`callRecursively()` method.

    [php]
    $menu->callRecursively('requiresAuth', true);

Now every single menu item in that menu tree will require authentication.

# Rendering Site Menus

Finally! Now that we have explained the `sfSympalMenu` class and how it actually 
works. We can get in to the details on how to render our menus for our Sympal 
sites.

A menu can easily be rendered by using the `get_sympal_menu()` helper function. 
Below are some examples where we make use of it.

    [php]
    echo get_sympal_menu('primary');

The `get_sympal_menu()` method accepts an argument for the slug of the menu you 
want to render. It returns an instance of `sfSympalMenu`. It also accepts some 
additional optional arguments, `$showChildren` and `$class`.

In this example we'll display the primary menu but hide the children.

    [php]
    echo get_sympal_menu('primary', false);

Now in this example we'll get the primary menu but we'll specify we want to use 
our own class instead of the default `sfSympalMenu`.

    [php]
    class PrimaryMenu extends sfSympalMenuSite
    {
      
    }

    $menu = get_sympal_menu('primary', false, 'PrimaryMenu');
    echo get_class($menu); // PrimaryMenu

Now with the above code you can change the way a menu renders as much as you 
desire.

> **NOTE**
> The base of the menu system is the `sfSympalMenu` class but for Sympal site menus you
> must extend the `sfSympalMenuSite` class which adds some specific functionality that
> ties a `sfSympalMenuSite` instance to a Doctrine `sfSympalMenuItem` instance.

## Sub-Menus

Rendering sub-menus in Sympal could not be any easier. All you need to do is 
simply pass the `MenuItem` instance that you want to render the sub-menu for.

    [php]
    echo get_sympal_menu($menuItem);

In the above example we would be rendering the children menu items of the 
`$menuItem` we passed to the function.

You can also render the sub-menu for the current menu item by doing the 
following.

    [php]
    echo get_sympal_menu(sfSympalContext::getInstance()->getCurrentMenuItem());

This is useful if you want to implement some kind of roll-over sub-menu loading 
via ajax or to display sub-menus for the current menu item in a sidebar on your 
site.