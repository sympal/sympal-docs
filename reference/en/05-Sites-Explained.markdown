Sympal offers the ability to have multiple sites within one project. The
benefit of having multiples sites in one project is that the sites can
share information  and code. It is typical for a companies web presence to
have multiple faces and  each one has its own sitemap, content, layouts,
etc. With Sympal this is made  easy as all the data is organized per site
in the database. This allows us to  manage data for the current site or
other sites.

For each sympal site that exists, a matching symfony application must exist as 
well. When installing sympal, an sfSympalSite record is automatically added
for you. The name of the site, which must match the name of your symfony
application, will be automatically set to the first application found in
the `apps` directory.

## Queries

When you are viewing the content of a site, the query for viewing content is 
automatically altered to include the conditions to limit it to show only
the data from that site. In other words, while the `sfSympalContent` model
may contain records from multiple sites, the query is automatically altered
so that only the current site's content records are returned.

## Creating New Site

Creating a new site is as easy as executing a few tasks from the command line.
First we need to generate a new application for our site:. This is done
using the following stock symfony task:

    $ php symfony generate:app my_new_site

Now we can create a site for it:

    $ php symfony sympal:create-site my_new_site

Now you can navigate to `my_new_site_dev.php` from your browser and you will
see a blank Sympal site ready for you to work with.

> **NOTE**
> In reality, very little needs to be done to "prepare" an application to
> be a sympal site. The `sympal:create-site` task merely creates the necessary
> `sfSympalSite` record, changes the `myUser` class to extend `sfSympalUser`
> and removes the `homepage`, `default` and `default_index` routes from
> routing.yml.

## Retrieving the Current Site

When you are developing in sympal you'll often need to know what the current
site is. This information can be easily retrieved.

Since each sympal site is bound to a symfony application, we can determine the
`slug` of the site by using the `sf_app` config value.

    [php]
    $siteSlug = sfConfig::get('sf_app');

We can also retrieve this same value from sympal's service container.

    [php]
    $siteRecord = sfSympalContext::getInstance()->getService('site_manager')->getSite();

Additionally, the site record is always available in the template:

    [php]
    echo $sf_sympal_site->description;

## Themes

Each site will likely need its own layout and set of stylesheets and javascripts.
In symfony, this is determined largely in the view.yml file of an application.
Sympal, on the other hand, introduces the idea of a theme.

A theme consists of a layout, stylsheets, javascripts and a few other minor
options. A project can have multiple themes and each site (and even page)
can use a different theme.

To set the default theme that should be used for an entire sympal site,
modify the `app.yml` file in the `config` directory of your application:

    [yml]
    all:
      sympal_config:
        theme:
          default_theme:  wordpress

Themes will be explained in greater detail in another chapter.