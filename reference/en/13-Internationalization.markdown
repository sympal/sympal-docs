Sympal contains several different feature which help you with building fully internationalized sites with multiple languages. In this section of the documentation we'll document how to utilize these features.

## Enabling

By default internationalization in Sympal is off. Though, it can be easily enabled with a simple configure command:

    $ php symfony sympal:configure i18n=true

> **TIP**
> Don't forget your Symfony application must have i18n enabled as well in your applications
> `config/settings.yml`:
>
>     [yml]
>     prod:
>       .settings:
>         i18n: true

## Configuring Available Language

You can easily configure the available languages by setting the `language_codes` sympal configuration option. Open your `project/config/app.yml` file and update it to have the new option:

    [yml]
    all:
      sympal_config:
        language_codes: [en, fr, es]

## Changing Language

You can easily change your the current language by using the `sympal_change_language` route:

    [php]
    $this->redirect('@sympal_change_language?language=fr');

This would result in the action redirecting to a URL like the following:

    http://www.sympalphp.org/change_language/fr

When changing your language, after the language is changed it will redirect back to the referrer or the homepage if no referrer was found.

You can also change the language by using the `sfFormLanguage` form. We have it ready to use in a component:

    [php]
    echo get_component('sympal_default', 'language');