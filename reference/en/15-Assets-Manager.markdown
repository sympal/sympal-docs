> **NOTE**
> The Assets Manager in Sympal is handled by the `sfSympalAssetsPlugin` and was created by forking
> the [`sfMediaBrowserPlugin`](http://www.symfony-project.org/plugins/sfMediaBrowserPlugin). It was modified, enhanced and integrated with Sympal to handle all media, files, etc.

## Introduction

The Sympal assets manager allows you to easily upload files, images, videos, etc. and embed them in your content:

[asset:42 title=Assets browser]

It provides a simple interface which allows you to click in an editor where you want the asset to be inserted and click the asset to insert a special code used to render the asset:

[asset:46 title=Insert assets]

## Asset Syntax

The asset syntax is how you can link to and embed assets in any of your Sympal content. It is simple, flexible and easy to understand. Continue reading to learn how to take advantage of this feature.

Here is a basic example where we embed an image in the Sympal documentation:

**[asset:42 embed=true title=Assets browser]**

> **TIP**
> The embed option is the default for for image asset types so you can optionally leave out this and just
> do something like **[asset:42 title=Assets browser]**

The syntax for the above asset renders like the following:

[asset:42 title=Assets browser]

## Asset Syntax Arguments

The asset syntax supports two arguments, the ID of the asset, and the options to use to render the asset syntax the way you want. The asset system supports many different options so continue reading to learn about what different options you can use:

| Option                   | Description |
| ------------------------- | ----------------- |
| thumbnail              | Render thumbnail for the asset. |
| linked_thumbnail | Render thumbnail for the asset linked to the asset itself. |
| thumbnail_url       | Output the URL of the asset thumbnail. |
| url                          | Output the URL of the asset. |
| icon_url                 | Output the URL of the asset type icon. |
| icon                       | Render the icon for the asset type. |
| embed                   | Render the embedded asset. The different asset types handle how the asset are embedded. |
| link                        | Link to the asset URL. |

Some options can be used together for some pretty neat rendering abilities. For example you can use the `thumbnail` and `link` option together to produce a link to the asset with the thumbnail to the left of the link.

Here is an example syntax:

**[asset:42 thumbnail=true link=true]**

The above syntax would render like the following:

[asset:42 thumbnail=true link=true]<br/><br/><br/><br/>

You can also combine the `icon` and `link` options:

**[asset:42 icon=true link=true]**

The above syntax would render like the following

[asset:42 icon=true link=true]

<br/><br/>

> **NOTE**
> Every asset type has a icon that represents it. Currently the asset system supports images, videos
> and documents/files out of the box. Of course you can easily add your own. Continue reading to learn 
> how to add your own asset types.

## Asset Types

Sympal supports multiple asset types. Out of the box it supports images, videos and documents/files. Thought, this is completely configurable and you can add your own asset types to customize how your
assets are rendered and embedded in your content.

In the `sfSympalAssetsPlugin` configuration we have all the abilities to configure what types we can handle:

    [yml]
    all:
      sympal_config:
        assets:
          # ...
          file_types:                           # define file_types is usefull for displaying icons in browser
            document:                           # type of file (also used as default icon name)
              extensions: [doc, xls, xcf, ai]   # extensions associated to this type
              icon:       doc                   # optional icon file name, without extension
            image:                              
              extensions: [png, jpg, jpeg, gif]
              class:      sfSympalAssetImageObject
            pdf:
              extensions: [pdf]
            bin:
              extensions: [bin, exe, sh, bat, deb, yum]
            video:
              extensions: [wmv, avi, mpg, mpeg, flv, mp4, swf]
              class:      sfSympalAssetVideoObject
            audio:
              extensions: [ogg, mp3, flac, wma, cda]
            text:
              extensions: [txt]
            tarball:
              extensions: [tar, gz, zip, bzip, gzip, rar, 7z]

All assets are represented by a base `sfSympalAssetObject`. It can of course be extended for every defined type here. Sympal only provides three extensions of this class to handle images, videos and files:

* **sfSympalAssetFileObject** - Class for handling any file/document that doesn't have a custom class.
* **sfSympalAssetImageObject** - Class for handling image asset types.
* **sfSympalAssetVideoObject** - Class for handling video asset types.

You can create your own classes to handle different asset types, just create a class new class and configure the asset type in your `app.yml` to use the class.

    [php]
    class sfSympalAssetTextObject extends sfSympalAssetObject
    {
    }

Configure the `text` type to use the new class:

    [yml]
    all:
      sympal_config:
        assets:
          # ...
            text:
              extensions: [txt]
              class: sfSympalAssetTextObject

Now you can begin to override methods and customize how this asset is handled!