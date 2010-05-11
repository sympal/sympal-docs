## Introduction

In this chapter we'll talk a little more about Content Slots. We already
talked a little bit about them in the previous
[link:content-templates label="Content Templates"] chapter. To recap, a
content slot is an arbitrary editable piece of content related to a
Sympal content record. Each content slot has a type which determines the
form used to edit the value of that slot, and also the class used to render
the value of that slot. This means content slots are extremely flexible
and can be used to edit and store simple or very complex values. They can
store a serialized PHP value, HTML, Markdown, etc. or whatever it is you
prefer.

## Using Content Slots

Using a content slots are pretty simple, it just requires that you use
the `get_sympal_content_slot()` helper method. This method will create
the slot if it does not exist, allow you to edit the value inline, and
will render the value of the slot when not in edit mode.

In order to use a content slot all you need is an instance of the
`sfSympalContent` record to create the slot for. This isn't anything
special to Sympal, you can just retrieve the instance of the content
record with a normal `Doctrine_Query`:

    [php]
    $content = Doctrine_Core::getTable('sfSympalContent')
      ->getBaseQuery('c')
      ->where('c.slug = ?', 'my_content')
      ->fetchOne();

Now you can use the `$content` instance to render slots for:

    [php]
    echo get_sympal_content_slot($content, 'name_of_slot', 'Markdown');

This allows you to easily use slots in your own hand written modules where
you manually retrieve the content record.