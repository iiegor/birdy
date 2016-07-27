# Pantheon ![npm-deps](https://david-dm.org/iiegor/pantheon.svg)

> Node.js web server focused on performance and simplicity using the latest technologies

## Features

### CSS class renaming

![compress-diff](http://image.prntscr.com/image/8a848a93db4942239c45e5c435ab1d49.png)

##### **Difference between development and production code*

###### Exporting class names
```css
.login-tab .exportTitle {
  font-size: 20px;
  color: #000;
}
```

The code above will result into something like this: ``.NjA-NTk .exportTitle``

### Resource bundles

You can create bundles that contains multiple static resources and require them when needed.

```javascript
module.exports = {
  path: '/route',
  
  assets: [
    ['css', 'styles/route-style.js', 'sync'], // Source code will be writen into the DOM
    ['js', 'scripts/route-script.js'],
  ].bundle('ApplicationUi'),
  
  // ..
};
```

```html
{% for script in bundles.get('ApplicationUi').js %}
  <script src="{{ script.url }}"></script>
{% endfor %}
```

### Effective asset distribution

![asset-dist-diff](http://image.prntscr.com/image/127e679f1b964b79a823864073c9e743.png)

While on development all assets will be called following this syntax:

``/_/pantheon/_/b={resourceBundle}/{resourceType}/rs={resourceName}``

On production, the resource name will be replaced by a string concatenation of the ``last modification time`` of the file with the resource name (string is hashed).

### DOM binding

###### The ``jsaction`` attribute

```html
<button jsaction="click:button.alert">
  <span>Show alert!</span>
</button>
```

```javascript
let JSActions = {
  /**
   * @namespace button
   */
  'button.alert': () => alert('Hello world!'),
};
```

###### The ``jsname`` attribute

Since all the elements class attributes are minified on production, we didn't have a solid way to reference a desired HTML element so we decided to create the ``jsname`` attr which acts as a reference to the desired HTML element.

```html
<div class="class-name another-class" jsname="my-button"></div>
```

```javascript
let div = document.querySelector('[jsname="my-button"]');
```

*NOTE: The value inside ``jsname`` attr is minified so everything that doesn't pass through the build process won't work on production*

## Installation

You can get the latest stable release from the [releases](https://github.com/iiegor/pantheon/releases) page.

Once you've downloaded it, you are ready to run ``$ npm install`` to install all the needed dependencies by the server.

## Lincese
MIT © [Iegor Azuaga](https://github.com/iiegor)
