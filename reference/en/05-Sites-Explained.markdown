Sympal offers the ability to have multiple sites within one project. The benefit
of having multiples sites in one project is that the sites can share information 
and code. It is typical for a companies web presence to have multiple faces and 
each one has its own sitemap, content, layouts, etc. With Sympal this is made 
easy as all the data is organized per site in the database. This allows us to 
manage data for the current site or other sites.

For each sympal site that exists, a matching symfony application must exist as 
well. When installing sympal, an sfSympalSite record is automatically added
for you. The name of the site, which must match the name of your symfony
application, will be automatically set to the first application found in
the `apps` directory.

## Queries

When you are viewing the content of a site, the query for viewing content is 
automatically altered to include the conditions to limit it to show only the data
from that site.

## Creating New Site

Creating a new site is as easy as executing a few tasks from the command line. First we need to generate a new application to create our site for:

    $ php symfony generate:app my_new_site

Now we can create a site for it:

    $ php symfony sympal:create-site my_new_site

Now you can navigate to `my_new_site_dev.php` from your browser and you will see a blank Sympal site ready for you to work with.

## Getting Current Site

When you are developing in Sympal you'll often want to know what the current site is. This information can be easily retrieved.

Since the Sympal sites are bound to Symfony applications, we can know the slug of the site by using the `sf_app` config value.

    [php]
    $siteSlug = sfConfig::get('sf_app');

We can also retrieve this same value from the Sympal context.

    [php]
    $siteSlug = sfSympalContext::getInstance()->getSite();

You may want to just go ahead and get the record for the site too.

    [php]
    $site = sfSympalContext::getInstance()->getSiteRecord();
    echo $site->name;
    print_r($site->toArray());

## Site Layout

Changing the layout a site should use is very easy. First, you can set the `default_layout` setting with `sfSympalConfig`:

    [php]
    sfSympalConfig::set('default_layout', 'my_layout');

It would be ideal to just change this value in your `app.yml` for the application. 
Edit `apps/site_name/config/app.yml` and place the following YAML inside.

    [yml]
    all:
      sympal_config:
        default_layout: layout

> **TIP**
> You can see what options are available to go under `sympal_config` by looking 
> at the default `app.yml` which comes with `sfSympalPlugin`. Take a look in
> `plugins/sfSympalPlugin/config/app.yml` to learn what is available.

You can also change the layout the site uses by editing the site record in the 
database. There you will find a property named `layout` and the value of this property is used whenever you are in that site. This value can be overridden at
a higher level by content types and content records.