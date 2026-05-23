# Story-book---adon-

A powerful addon for Storybook that allows you to write and run component tests directly inside your stories and display the results in Storybook UI. ✨

> ⚠️ This project is no longer actively maintained. If anyone is interested in continuing development and maintenance, contributions are welcome.

---

## 🚀 Features

* Write tests directly alongside your stories
* Display test results inside Storybook
* Supports:

  * Jest
  * Mocha
  * Enzyme
* Snapshot testing support
* External test file loading
* Hooks support (`beforeEach`, `afterEach`, etc.)

---

## 📦 Installation

Install the addon as a development dependency:

```bash
npm install -D storybook-addon-specifications
```

Then register the addon inside your Storybook configuration.

Create or update:

```js
.storybook/addons.js
```

```js
import 'storybook-addon-specifications/register';
```

---

## 🛠 Basic Usage

Example using Enzyme and Expect:

```js
import { storiesOf } from '@kadira/storybook';
import { specs, describe, it } from 'storybook-addon-specifications';

import { mount } from 'enzyme';
import expect from 'expect';

const stories = storiesOf('Button', module);

stories.add('Hello World', function () {

  const story = (
    <button>
      Hello World
    </button>
  );

  specs(() =>
    describe('Hello World', function () {

      it('Should render correct text', function () {
        let output = mount(story);
        expect(output.text()).toContain('Hello World');
      });

    })
  );

  return story;
});
```

---

## ⚙️ Enzyme Configuration

Configure Enzyme adapter inside:

```js
.storybook/config.js
```

```js
import { configure } from 'enzyme';
import Adapter from 'enzyme-adapter-react-16';

configure({
  adapter: new Adapter()
});
```

Update webpack configuration:

```js
externals: {
  'jsdom': 'window',
  'cheerio': 'window',
  'react/lib/ExecutionEnvironment': true,
  'react/lib/ReactContext': 'window',
  'react/addons': true,
}
```

---

## 🧪 Using with Jest

### Create a facade file

```js
.storybook/facade.js
```

```js
import {
  storiesOf as storiesOfReal,
  action as actionReal,
  linkTo as linkToReal
} from "@kadira/storybook";

import {
  specs as specsReal,
  describe as describeReal,
  it as itReal
} from 'storybook-addon-specifications';

export const storiesOf = storiesOfReal;
export const action = actionReal;
export const linkTo = linkToReal;
export const specs = specsReal;
export const describe = describeReal;
export const it = itReal;
```

---

### Create Jest mocks

```js
.storybook/__mocks__/facade.js
```

```js
export const storiesOf = function storiesOf() {

  var api = {};

  api.add = (name, func) => {
    func();
    return api;
  };

  return api;
};

export const specs = (spec) => {
  spec();
};

export const describe = jasmine.currentEnv_.describe;
export const it = jasmine.currentEnv_.it;
```

---

### Jest Configuration

```json
{
  "jest": {
    "setupFiles": [
      "./path/to/jest/config.js"
    ],
    "automock": false
  }
}
```

Add:

```js
jest.mock('./.storybook/facade');
```

---

## 📸 Automatic Snapshot Testing

You can automatically create snapshots for every Storybook story.

Example snapshot helper:

```js
export const snapshot = (name, story) => {

  it(name, function () {

    let renderer = require("react-test-renderer");

    const tree = renderer.create(story).toJSON();

    expect(tree).toMatchSnapshot();

  });

};
```

---

## 🧩 Mocha Support

This addon also supports:

* Mocha
* jsdom
* Global Storybook API mocking

Supported hooks/features:

* `before`
* `after`
* `beforeEach`
* `afterEach`
* `describe.only`
* `describe.skip`
* `it.only`
* `it.skip`

---

## 📂 Loading External Test Files

Project structure:

```bash
|- example.stories.js
|- example.test.js
|- example.js
```

Example:

```js
import React from 'react';
import { storiesOf } from '@kadira/storybook';
import { specs } from 'storybook-addon-specifications';

import { tests } from './Example.test';
import Example from './example';

storiesOf('Example', module)
  .add('Default', () => {

    specs(() => tests);

    return <Example />;

  });
```

---

## 📚 Supported Hooks

### Jest

* beforeEach
* afterEach
* fit
* xit
* xdescribe

### Mocha

* before
* after
* beforeEach
* afterEach
* describe.only
* describe.skip
* it.only
* it.skip

---

## 📖 Learn More

If you want to understand the idea behind this addon and living component documentation, check out the original article about building component-driven documentation using Storybook.

---

## 🤝 Contributing

Since the project is currently unmaintained, community contributions and forks are highly appreciated.

---

## 📄 License

MIT License

---
