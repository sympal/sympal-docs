A Sympal content template is just a Symfony partial or component. Inside a content template you render the data for a content type as HTML. You can use some bundled Sympal helpers to define slots for inline editing and rendering.

## Configuring Available Templates

Throughout the interface of Sympal you have the ability to choose what content template should be used to render a certain content type or content record so we need to configure what templates are available in your `config/app.yml`. Below is an example from the core of Sympal where we configure the available content templates for the `page` content type which uses the

    [yml]
    all:
      sympal_config:
        page:
          content_templates:
            default_view:
              template: sympal_page/view
            register:
              template: sympal_register/page_template

Now when you view the dropdown of available templates you will see the select box populated with the content templates you configured:

[asset:101 title=Content template dropdown]

## Writing Content Templates

A content template is just a Symfony partial so you don't need to know too much else. The configured template is passed some additional parameters which you should know:

| Name     | Description |
| ----------- | ----------------- |
| `$format` | The format being rendered. |
| `$content` | The `sfSympalContent` instance being rendered. |
| `$menuItem` | The `sfSympalMenuItem` that is related to the content record being rendered. |

You can make use of these variables in your content templates. Here is an example content template which just renders some data from the `$content` variable:

    [php]
    <h1><?php echo $content->getTitle() ?></h1>

    <p>This is a content template</p>

## Content Slots

Often times you need more flexibility with dynamically defining editable areas of content within a page without having to alter your database schema and add a new column. For this we have content slots. A content record may have many slots and each slot has a type. This allows us to define custom forms and widgets to edit the value of these content slot types as well as a custom class to render the slot type value!

### Defining Slots

You can easily define slots in your content templates with the `get_sympal_content_slot()` helper function. Using this method to render properties and slots for your content record allow Sympal to integrate with the rendering of these values and implement such functionality as inline editing.

Here is an example where we define a simple content template to render a `sfSympalContent` record that is of the type `page`. It will render some breadcrumbs, the title of the related `sfSympalPage` instance and two slots named `introduction` and `body`.

You can create a new module to hold your content template with the `generate:module` task:

    $ php symfony generate:module sympal sympal_page

Now create a partial named `my_custom_content_template`:

    [php]
    // apps/sympal/modules/sympal_page/templates/_my_custom_content_template.php

    <?php echo get_sympal_breadcrumbs($menuItem, $content) ?>

    <h1><?php echo get_sympal_content_slot($content, 'title') ?></h1>

    <p><?php echo get_sympal_content_slot($content, 'introduction', 'RawHtml') ?></p>

    <p><?php echo get_sympal_content_slot($content, 'body', 'Markdown') ?></p>

Before we can use the content template we need to make it available by configuring it in our `config/app.yml`:

    [yml]
    all:
      sympal_config:
        page:
          content_templates:
            my_custom_template:
              template: sympal_page/my_custom_content_template

Now a piece of content can be edited and the content template changed. In this example we'll change the homepage to use this new content template:

[asset:102 title=picture-10.png]

Now when we view the homepage you will see the content rendered with the new content template:

[asset:103 title=picture-9.png]