## Minification

> **SIDEBAR**
> What is minification?
>
> Stylesheet and Javascript minifiction is a common practice in web application development. It 
> involves removing all unnecessary characters from the CSS and Javascript that are for readability but 
> not required for execution and combines the files into single cached files. This is one of the rules that 
> the [Yahoo! YSlow](http://developer.yahoo.com/yslow) rules recommends you implement. The result is 
> a faster perceived loading of a web page in your browser. You can read more about 
> [Minification on Wikipedia](http://en.wikipedia.org/wiki/Minification_%28programming%29).

This feature is on by default in Sympal but you can disable it using your `app.yml` configuration. Below is an example where we disable the minifier:

    [yml]
    all:
      sympal_config:
        # ...
        minifier:
          enabled: false

> **NOTE**
> In order for Sympal to be able to minify the assets for your layout you must use the `sympal_minify()` 
> helper before you include your stylesheets and javascripts.

Here is an example barebones layout where we use this helper:

    [php]
    <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN"
     "http://www.w3.org/TR/html4/strict.dtd">
    <html>
    <head>
      <?php include_http_metas() ?>
      <?php include_metas() ?>
      <?php include_title() ?>
      <?php sympal_minify() ?>
      <?php include_stylesheets() ?>
      <?php include_javascripts() ?>
    </head>
    <body>
      <?php echo $sf_content ?>
    </body>
    </html>

### Excluding

Sometimes you may want to exclude an asset from being minified. This can be accomplished by using the `exclude` array:

    [yml]
    all:
      sympal_config:
        # ...
        minifier:
          exclude: [/js/my_javascript.js]

Now when you view your page the configured javascript path will not be minified and will be included as is.

> **TIP**
> When minification is enabled and you modify some javascript or css you need clear your cache 
> so that in the next request the minification process will run again rebuilding the cached files. 
> Sympal hooks into the `symfony cc` task and also clears the cache contained in the `web/cache` folder
> which contains any minified assets.
>
>     $ php symfony cc

## Overriding

Sympal comes with many stylesheet and javascript files that style and make the Sympal UI what it is. You may need to or want to override these in order to change or add things. You can do this in your `app.yml` configuration under the `asset_paths` key. Here is an example where we override the stylesheet that is used to render the Sympal admin dashboard:

    all:
      sympal_config:
        asset_paths:
          "/sfSympalAdminPlugin/css/dashboard.css": "/css/my_dashboard.css"

Now you can customize how the dashboard is rendered in your project.