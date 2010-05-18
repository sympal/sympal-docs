As we've learned, the _data_ of your site is stored in content records
and retrieved based on the url being requested. The data, however, can
be rendered in many differeny ways through the creation of _templates_.

A sympal content template is just a symfony partial or component. Inside
a content template you render the data for your content record as HTML. You
can use some bundled sympal helpers to define slots for inline editing
and rendering.

## Configuring available templates

When a particular content record is rendered, it must determine which
template to use. Each content type defines a list of possible templates
that can be used when rendering a content record of that type. This is
configurable on your `config/app.yml` file. Below is an example from the
core of sympal:

    [yml]
    all:
      sympal_config:
        content_types:
          sfSympalPage:
            content_templates:
              default_view:
                template: sympal_page/view
              register:
                template: sympal_register/page_template

This configures the available content templates for any content types
that uses the `sfSympalPage` content type model.

Notice the `default_view` content template as it is special. Each content
type must define at least one content template called `default_view`.
This template is used by default if none of the other templates have been
explicitly requested.

Now, when viewing the dropdown of available templates for a particular
content type, you will see the select box populated with the content
templates you configured:

[asset:101 title=Content template dropdown]

>**NOTE**
>For each content type model (e.g. `sfSympalPage`), many `sfSympalContentType`
>records can be created. Each `sfSympalContentType` record will use the
>configuration defined by its data model (`sfSympalPage`). This allows you
>to organize your content into different "pieces", each which use the same
>basic configuration and data model. For more information, see the
>Content Types chapter.


## Writing content templates

A content template is just a symfony partial so you don't need to know
much else. The configured template is passed some additional parameters
which you should know:

| Name     | Description |
| ----------- | ----------------- |
| `$sf_sympal_content` | The `sfSympalContent` instance being rendered. |
| `$sf_format` | The format being rendered. |
| `$sf_sympal_site` | The `sfSympalSite` record of your current site. |
| `$sf_sympal_context` | The `sfSympalContext` instance. |

>**NOTE**
>You can also retrieve that is related to the content record being rendered
>via `$sf_sympal_content->getMenuItem()`.

You can make use of these variables in your content templates. Here is
an example content template which just renders some data from the `$content`
variable:

    [php]
    <h1><?php echo $sf_sympal_content->getTitle() ?></h1>

    <p>This is a content template</p>

>**CAUTION**
>The `sfSympalContent` record is also available via the `$content` variable.
>This, however, is deprecated and will be removed in future releases.

## Content slots

Sometimes, you need the flexibility to  dynamically define editable
areas of content within a page without having to alter your database schema
and add a new column. For example, if you had a `Product` model and needed
an extra `color` data field on some cases, you could use slots to store that
data without modifying your schema.

A content record may have many slots and each slot has a type, which defines
how the slot should be editing and rendered. This allows us to define custom
forms and widgets to edit the value of each slot as well as define custom
ways to render that value (e.g. convert Markdown to HTML).

### Defining slots

You can easily define and use slots in your content templates with the
`get_sympal_content_slot()` helper function. Using this helper accomplishes
two things simultaneously:

  1. The helper enables inline editing as well as text processing.

     If you have a field on your content record called `title`, you could
     easily render it via `$sf_sympal_content->title`. Alternatively, that
     same data could be rendered via `get_sympal_content_slot('title')`.
     The latter enables inline editing and can also be used to process
     the text automatically.

  1. The helper allows you to define non-existent fields

     As mentioned above, the name of the slot does not need to be the name
     of a real field on your content record. For example, you could do
     the following, even though `color` isn't a real field on your
     content type: `get_sympal_content_slot('color')`.

Here is an example where we create a content template to render a
`sfSympalContent` record that is of the type `page`. It will render some
breadcrumbs, the `title` of the related `sfSympalPage` instance and two
slots named `introduction` and `body`.

You can create a new module to hold your content template with the
`generate:module` task:

    $ php symfony generate:module sympal sympal_page

Now create a partial named `my_custom_content_template`:

    [php]
    // apps/sympal/modules/sympal_page/templates/_my_custom_content_template.php

    <?php echo get_sympal_breadcrumbs($sf_sympal_content->getMenuItem(), $sf_sympal_content) ?>

    <h1><?php echo get_sympal_content_slot('title') ?></h1>

    <p><?php echo get_sympal_content_slot('introduction', array('type' => 'RawHtml')) ?></p>

    <p><?php echo get_sympal_content_slot('body', array('type' => 'Markdown')) ?></p>

Before we can use the content template we need to make it available by
configuring it in our `config/app.yml`:

    [yml]
    all:
      sympal_config:
        content_types:
          sfSympalPage:
            content_templates:
              my_custom_template:
                template: sympal_page/my_custom_content_template

Now a piece of content can be edited and the content template changed.
In this example we'll change the homepage to use this new content template:

[asset:102 title=picture-10.png]

Now when we view the homepage you will see the content rendered with
the new content template:

[asset:103 title=picture-9.png]