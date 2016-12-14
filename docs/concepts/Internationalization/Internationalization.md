# Internationalization

### Overview

If your app will touch people or systems from all over the world, internationalization and localization (i18n) may be an important part of your international strategy.  Sails provides built-in support for detecting user language preferences and translating static words/sentences thanks to [i18n-node](https://github.com/mashpie/i18n-node). ([npm](https://www.npmjs.org/package/i18n)).


### Usage


In a view:
```ejs
<h1> <%= __('Hello') %> </h1>
<h1> <%= __('Hello %s, how are you today?', 'Mike') %> </h1>
<p> <%= i18n('That\'s right-- you can use either i18n() or __()') %> </p>
```


In a controller or policy:
```javascript
req.__('Hello'); // => Hola
req.__('Hello %s', 'Marcus'); // => Hola Marcus
req.__('Hello {{name}}', { name: 'Marcus' }); // => Hola Marcus
```


Or if you already know the locale id, you can translate from anywhere in your application using `sails.__`:

```javascript
sails.__({
  phrase: 'Hello',
  locale: 'es'
});
// => 'Hola!'
```

### Locales

See [**Concepts > Internationalization > Locales**](http://sailsjs.org/documentation/concepts/internationalization/locales).


### Additional options

Settings for localization/internationalization may be configured in [`sails.config.i18n`](http://sailsjs.org/documentation/reference/sails.config/sails.config.i18n.html).  The most common reason you'll need to modify these settings is to edit the list of your app's supported locales and/or the location of your translation stringfiles:

```javascript
// Which locales are supported?
locales: ['en', 'es'],

// Where are your locale translations located?
localesDirectory: '/config/locales'
```




### Disabling or customizing Sails' default internationalization support

Of course you can always `require()` any Node modules you like, anywhere in your project, and use any internationalization strategy you want.

But worth noting is that since Sails implements [node-i18n](https://github.com/mashpie/i18n-node) integration in the [i18n hook](http://sailsjs.org/documentation/concepts/Internationalization), you can completely disable or override it using the [`loadHooks`](https://github.com/balderdashy/sails-docs/blob/master/PAGE_NEEDED.md) and/or [`hooks`](https://github.com/balderdashy/sails-docs/blob/master/PAGE_NEEDED.md) configuration options.


### Translating dynamic content

See [**Concepts > Internationalization > Translating dynamic content**](http://sailsjs.org/documentation/concepts/internationalization/translating-dynamic-content).


### What about i18n on the client?

The above technique works great out of the box for server-side views. But what about rich client apps that serve static HTML templates from a CDN or static host? (e.g. performance-obsessed SPAs or PhoneGap apps/Chrome extensions)

There are [lots of different options available](http://stackoverflow.com/questions/9640630/javascript-i18n-internationalization-frameworks-libraries-for-client-side-use) for client-side internationalization.  Like other client-side technologies, you should have no problem integrating any of them with Sails.

> If you'd prefer not to use an external internationalization library, you can actually reuse Sails' i18n support to help you get your translated templates to the browser.  If you want to use Sails to internationalize your _client-side templates_, put your front-end templates in a subdirectory of your app's `/views` folder.
> + In development mode, you should retranslate and precompile your templates each time the relevant stringfile or template changes using grunt-contrib-watch, which is already installed by default in new Sails projects.
> + In production mode, you'll want to translate and precompile all templates on lift(). In loadtime-critical scenarios (e.g. mobile web apps) you can even upload your translated, precompiled, minified templates to a CDN like Cloudfront for further performance gains.


<docmeta name="displayName" value="Internationalization">
