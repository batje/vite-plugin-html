# vite-plugin-html

**English** | [中文](./README.zh_CN.md)

[![npm][npm-img]][npm-url] [![node][node-img]][node-url]

A vite plugin for processing html. It is developed based on lodash template

### Install (yarn or npm)

**node version:** >=12.0.0

**vite version:** >=2.0.0-beta.4

`yarn add vite-plugin-html@next -D` or `npm i vite-plugin-html@next -D`

### Example

**Run Example**

```bash

cd ./examples

yarn install

yarn serve

```

## Usage

- Config plugin in vite.config.ts

```ts
import { UserConfigExport, ConfigEnv } from 'vite';
import vue from '@vitejs/plugin-vue';

import html from 'vite-plugin-html';

export default ({ command, mode }: ConfigEnv): UserConfigExport => {
  return {
    plugins: [
      vue(),
      html({
        title: 'Vite App',
        minify: command === 'build',
        options: {
          injectConfig: '<script src="./a.js"></script>',
        },
        tags: [
          {
            tag: 'div',
            attrs: {
              src: './',
            },
            children: 'content',
            injectTo: 'body',
          },
        ],
      }),
    ],
  };
};
```

### Options Description

**title**

type: `string`

default: ''

description: The content of the title tag of the index.html tag

**minify**

type: `boolean|Options`, [Options](https://github.com/terser/html-minifier-terser#options-quick-reference)

default: `command === 'build'` .

If it is an object type，Default:

```ts
  minifyCSS: true,
  minifyJS: true,
  minifyURLs: true,
  removeAttributeQuotes: true,
  trimCustomFragments: true,
  collapseWhitespace: true,

```

description: html compression configuration

**options**

type: `Record<string,any>`,

default: `{}`

description: User-defined configuration variables. You can use `viteHtmlPluginOptions.xxx` in `index.html` to get

Html Use [lodash.template](https://lodash.com/docs/4.17.15#template) syntax for template processing

**tags**

type: HtmlTagDescriptor[]

```ts
interface HtmlTagDescriptor {
  tag: string;
  attrs?: Record<string, string>;
  children?: string | HtmlTagDescriptor[];
  /**
   * default: 'head-prepend'
   */
  injectTo?: 'head' | 'body' | 'head-prepend';
}
```

description: An array of tag descriptor objects ({ tag, attrs, children }) to inject to the existing HTML. Each tag can also specify where it should be injected to (default is prepending to `<head>`)

e.g

**vite.config.ts**

```ts
import { UserConfigExport, ConfigEnv } from 'vite';
import vue from '@vitejs/plugin-vue';

import html from 'vite-plugin-html';

export default ({ command, mode }: ConfigEnv): UserConfigExport => {
  return {
    plugins: [
      vue(),
      html({
        options: {
          opt1: '<script src="./a.js"></script>',
          opt2: '<script src="./a.js"></script>',
        },
      }),
    ],
  };
};
```

**index.html**

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <%= viteHtmlPluginOptions.opt1 %>
  </head>
  <body>
    <%= viteHtmlPluginOptions.opt2 %>
  </body>
</html>
```

## License

MIT

[npm-img]: https://img.shields.io/npm/v/vite-plugin-html.svg
[npm-url]: https://npmjs.com/package/vite-plugin-html
[node-img]: https://img.shields.io/node/v/vite-plugin-html.svg
[node-url]: https://nodejs.org/en/about/releases/
