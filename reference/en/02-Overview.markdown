In this chapter we will give an overview of some of the main concepts implemented in Sympal to help you understand how everything works together better.

## Content Structure

Every request or page in a Sympal application is represented by a `sfSympalContent` record instance. 

### Content Types

Every content record has a relationship to a `sfSympalContentType` instance. To create a new `sfSympalContent` you should use the `createNew()` method:

    [php]
    $content = sfSympalContent::createNew('sfSympalPage');

Now you can access the type object with the following:

    [php]
    echo $content->getType()->getName(); // The model name. For example sfSympalPage

The type model is automatically instantiated and added as a relationship. With some magic the properties of the content type model can be access from the content record. For example the `sfSympalPage` content type has a property named `title` and it can be manipulated from the content record:

    [php]
    $content->setTitle('Sample title');

### Content Slots

In addition to the content type record, each content record has a many relationship to `sfSympalContentSlot`. This allows arbitrary areas of data to be defined and added to a template without having to modify the schema in your database.

You can define these slots by using the `get_sympal_content_slot()` helper:

    [php]
    <h1><?php echo get_sympal_content_slot($content, 'title') ?></h1>

    <?php echo get_sympal_content_slot($content, 'body') ?>

In the above example the first slot renders the title property of the content type record and the second is a `sfSympalContentSlot` record with a name of `body`. If the slot does not exist it will automatically be added.

Here is a screenshot of the rendered content template for the default `sfSympalPage` content type which is very similar to the above example content template.

[asset:36]

## Menu Structure

The menu system in Sympal consists of one model named `sfSympalMenuItem` and is included in the `sfSympalMenuPlugin`. It is a normal Doctrine model with the `NestedSet` behavior enabled. This behavior gives us the functionality needed to describe the hierarchy of our sites menus. Each `sfSympalMenuItem` instance can map to a content record which allows each menu item to easily link to the URL of the content. A menu item link can also be customized and pointed to any URL on the web.