In this chapter we will give an overview of some of the main concepts
implemented in sympal to help you understand how everything works together
better.

## Content Structure

Every request or page in a sympal application is represented by a
`sfSympalContent` record instance. This is a Doctrine record and so each
page represents one row in the database.

### Content Types

Every content record has a relationship to exactly one `sfSympalContentType`
instance. This content type relationship specifies what "type" of page
is being rendered (a blog post, normal page, etc).

As a developer, you'll always interact directly with the `sfSympalContent`
instance, which manages the relationship to `sfSympalContentType`.
To create a new `sfSympalContent` you should use the `createNew()` method:

    [php]
    $content = sfSympalContent::createNew('sfSympalPage');

Now you can access the type object with the following:

    [php]
    echo $content->getType()->getName(); // The model name. For example sfSympalPage

For the most part, the `sfSympalContentType` won't be terribly useful directly.
It's true importance is that each record(row) in sfSympalContentType is a
pointer to another model. For example, an `sfSympalContentType` record
with the name `sfSympalPage` is a pointer to a real model called `sfSympalPage`.
This is known as the "type model".

The type model is automatically instantiated and added as a relationship.
In the above example, the `sfSympalContent` record will have a one-to-one
relationship with an `sfSympalPage` record.

[asset:documentation-1-0-content-model-diagram]

With some magic, the properties/fields of the content type model
can be accessed from the content record. For example the `sfSympalPage`
content type has a property named `title` and it can be manipulated or
retrieved from the `sfSympalContent` record:

    [php]
    // equivalent to $content->sfSympalPage->setTitle('Sample title');
    $content->setTitle('Sample title');

    // equivalent to $content->sfSympalPage->getTitle();
    echo $content->getTitle();

### Content Slots

In addition to the content type record, each content record (`sfSympalContent`)
has a many relationship to `sfSympalContentSlot`. This allows arbitrary
areas of data to be defined and added to a template without having to
modify the schema in your database.

You can define these slots by using the `get_sympal_content_slot()` helper:

    [php]
    <h1><?php echo get_sympal_content_slot('title') ?></h1>

    <?php echo get_sympal_content_slot('body') ?>

In the above example the first slot renders the title property of the
content type record and the second is a `sfSympalContentSlot` record
with a name of `body`. If the slot does not exist it will automatically
be added.

The idea is simple but powerful. The content type record (e.g. `sfSympalPage`)
should be designed to contain most of the information/fields you'll need
to output your page. In the case of the `title` slot, its data is both retrieved
and saved on the `sfSympalPage` record as you'd expect.

In some cases, however, you may need to store and render an extra field of
data that does not exist on the content or content type records. In the
example above, the `body` field doesn't exist on `sfSympalContent` or
`sfSympalPage`. In these cases, sympal allows you to specify and use this
extra field without making changes to your model. An `sfSympalContentSlot`
record will be inserted and will store the value of your extra field.
When that extra field is rendered, it will be retrieved from the `sfSympalContentSlot`
record.

Here is a screenshot of the rendered content template for the default
`sfSympalPage` content type which is very similar to the above example
content template.

[asset:36]

## Menu Structure

The menu system in Sympal consists of one model named `sfSympalMenuItem`
and is included in tse `sfSympalMenuPlugin`. It is a normal Doctrine model
with the `NestedSet` behavior enabled. This behavior gives us the functionality
needed to describe the hierarchy of our sites menus. Each `sfSympalMenuItem`
instance can map to a content record which allows each menu item to easily
link to the URL of the content. A menu item link can also be customized
and pointed to any URL on the web.