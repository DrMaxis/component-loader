# component-loader

## Installation

- Use `yarn add @drmaxis/component-loader | npm i @drmaxis/component-loader`

## Usage
**The Component Loader can attach any dialect of javascript or typescript to a direct DOM element and expose scoped functionality without using external dependencies (just plain js or ts).** 

Attaching JS to HTML within your app revolves around using the Component Loader to provide the functionality to the targeted DOM element using `data-attributes.`

```js
import Component from '@drmaxis/component-loader';

const DEFAULTS = {};

export default class ExampleComponent extends Component {
  static selector = '[data-comp-component-name]';

  constructor(containerEl, options = DEFAULTS) {
    super(containerEl, options);

    this.helloWorld();
  }

  helloWorld() {
    console.log(`Component Name Loaded`, { date: new Date() });
  }
}
```

### Steps

##### Create Component
- Create a new folder for your component.
- Within the `{ComponentName}` folder, create an `index.js` file.

##### Loading The Component
Depending on the way your assets are fed to the dom, you can inject the component module using either script tags or by importing it into your main entry js file.

- Create an `index.js` file
- Add the following:

```js

import { ComponentLoader } from '@drmaxis/component-loader';
import ComponentName from 'components/ComponentName';

window.globalComponents = new ComponentLoader(document, [
  ComponentName
]);
```

- Expose the index.js file `<script src="index.js"></> to the DOM

- On the DOM element (component-name.blade.php or component-name.html), add a data attribute for the container element

```php
 <div class="example-component" data-comp-component-name> 
      <h1> Hello world </h1>
	  <p> {{ $dataValue, $dataString }} </p>
  </div>
```
- Pass in values to the JS module using `data-attributes`

```php
 <div class="example-component" data-comp-component-name> 
      <h1 data-comp-example-component-title data-comp-value="{$dataValue}" > Hello world </h1>
	  <p> {{ $dataValue, $dataString }} </p>
  </div>
```

----

```js
import Component from '@drmaxis/component-loader';

const DEFAULTS = {
   titleSelector: '[data-comp-example-component-title]'
};

export default class ExampleComponent extends Component {
  static selector = '[data-comp-component-name]';

  constructor(containerEl, options = DEFAULTS) {
    super(containerEl, options);

    let titleElement = this.$container.querySelector(this.$options.titleSelector);
    this.titleValue  = titleElement.dataset.compValue;
    this.helloWorld();
  }

  helloWorld() {
    console.log(`Component Name Loaded`, { date: new Date() });
  }
}
```


## Features
- Provide a way to initialize multiple identical JS “components” on a page
- Provide a “table of contents” for all the JS that runs on a page* -- In progress
- Provide a standard way of initializing a growing list of JS without worrying about creating bottlenecks when the page loads. 

### Loader Priority

- TBD

### Event Bus

- TBD

### Communication between Components
