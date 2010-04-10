After you have generated your project with the Sympal installer you will
want to start customizing your site. You can even begin to customize the
installer so you can easily re-install your site to test changes, similar
to how you would use the doctrine task `doctrine:build --all` when developing.

If you want to re-install Sympal in your newly generated project you can
do so by using the `sympal:install` task:

    $ ./symfony sympal:install

The task accepts many optional arguments and options, if they are left
blank the default installation information is used.

> **CAUTION**
> The above install task will blow away your database completely and
> re-install Sympal and all your plugins. It is common to use this command
> to re-install your site when testing during development mode.


## Core Data Fixtures

Sympal comes with a set of data fixtures that are _always_ installed and
used to populate your initial database when installing. These fixtures are
responsible for setting up your first site, content type records, etc.,
but no site content. 

* sfSympalPlugin/data/fixtures/install.yml

## Extra Data Fixtures

Additional fixtures may also be supplied via plugins. For example,
`sfSympalPagesPlugin`, which is a core sympal plugin, includes some data
fixtures that populate the default content for your site, including menu
items and content:

* sfSympalPlugin/lib/plugins/sfSympalPagesPlugin/data/fixtures/install.yml

By default, any yaml file that exists inside the the `data/fixtures` directory
of a plugin will be processed when installing sympal.

These data fixtures can be overridden completely to create your own default
content. To do this, create a directory in your Sympal project named `install`
in the `data/fixtures` directory and add some fixture files, for example:

    data/fixtures/install/install.yml

By creating this directory, sympal will no longer install the fixtures from
the `data/fixtures` directory of your project or any plugin. Instead, sympal
will load only the core fixtures and those located in the `data/fixtures/install`
directory. This is a great way to replace the default fixtures with real
content related to your site.


## Post Install SQL

You can optionally execute custom SQL files at the end of the installation
process. This nice little feature allows you to do things at the end of the
installation that may be DBMS specific and can't be done through the ORM.
Or you could use it to populate your newly created database with the initial
data instead of using data fixtures.

You can execute SQL for the current connect or per connection by placing the 
files in specific locations in the `project/data/sql` directory. For example 
to have some globally executed SQL for each connect you can place it in the 
`data/sql/sympal_install` directory.

    [sql]
    // data/sql/sympal_install/install.sql
    CREATE TABLE some_table (id BIGINT AUTO_INCREMENT, title VARCHAR(255) NOT NULL, PRIMARY KEY(id)) ENGINE = INNODB;

Or you can execute custom SQL statements for each individual connection
by placing the SQL files in a directory named after the connection name.

    [sql]
    // data/sql/sympal_install/connection_name/install.sql
    CREATE TABLE some_table (id BIGINT AUTO_INCREMENT, title VARCHAR(255) NOT NULL, PRIMARY KEY(id)) ENGINE = INNODB;

Now when you run the installation process, at the very end the contents
of these SQL files will be executed.

## Post Install Hook

A simple post install hook is called at the end of the installation process
to allow you to execute additional code when the install is done. The function
is looked  for on your `ProjectConfiguration` class. Since the core sympal
fixtures located in `sfSympalPlugin/data/fixtures/install.yml` are always
loaded, this is a good place to perform any customization you may need.

    [php]
    class ProjectConfiguration extends sfProjectConfiguration
    {
      public function install()
      {
        // Do some extra work
        // Create default users
        // $user = new User();
        // ...

        // Customize the sfSympalSite object
        $site = Doctrine_Core::getTable('sfSympalSite')->findOneBySlug('sympal');
        $site->title = 'My sympal application';
        $site->save();
      }
    }

## Pre & Post Install Events

When the installation process is invoked, a Symfony event is fired before
and after to allow you to execute custom code. The event names are
`sympal.pre_install` and `sympal.post_install`.

    [php]
    $dispatcher->connect('sympal.post_install', array('MyInstall', 'postInstall'));
    
    class MyInstall
    {
      public static function postInstall(sfEvent $event)
      {
        
      }
    }