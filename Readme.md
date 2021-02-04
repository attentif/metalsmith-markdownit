[![Build Status](https://travis-ci.org/attentif/metalsmith-markdownit.svg?branch=master)](https://travis-ci.org/attentif/metalsmith-markdownit)

# metalsmith-markdownit

A Metalsmith plugin to convert markdown files using [markdown-it](https://github.com/markdown-it/markdown-it) library.

This plugin is not one to one replacement for metalsmith-markdown. There are slight differences in how the underlying libraries behave, but I find I get better results with markdown-it. Markdown-it, however, does not support all GFM features.

## Installation

    $ npm install metalsmith-markdownit

## CLI Usage

Install via npm and then add the `metalsmith-markdownit` key to your `metalsmith.json` plugins with any [markdown-it](https://github.com/markdown-it/markdown-it) options you want, like so:

```json
{
  "plugins": {
    "metalsmith-markdownit": {
      "typographer": true,
      "html": true
    }
  }
}
```

## Javascript Usage

Pass `options` to the markdown plugin and pass it to Metalsmith with the `use` method:

```js
var markdown = require('metalsmith-markdownit');

metalsmith.use(markdown({
  typographer: true,
  html: true
}));
```

You can also pass a markdown-it preset to the plugin:

```js
var markdown = require('metalsmith-markdownit');

metalsmith.use(markdown('default', {
  typographer: true,
  html: true
}));
```

If you want to control plugin behaviour, you can set options under the `plugin` key of the `options` object (these options are not passed to Markdown It):

```js
metalsmith.use(markdown('default', {
  typographer: true,
  html: true,
  plugin: {
    // options.plugin.pattern (string) - glob pattern. Defaults to '**/*@(md|markdown)'
    pattern: '**/*.html',  
    // options.plugin.fields (string|Array) - field or list of fields to parse with Markdown-It. Defaults to 'contents'
    fields: ['contents', 'excerpt']  
    // options.plugin.extension (string) - the file extension for parsed files. Defaults to 'html'
    extension: 'htm'
  }
}));
```

If you need access to markdown-it directly to enable features or use plugins, you can access the parser directly:

```js
var markdown = require('metalsmith-markdownit');

var md = markdown('zero', { html: true });
md.parser.enable(['emphasis', 'html_block', 'html_tag']);

metalsmith.use(md);
```

The parser's `enable`, `disable`, `use` and `set` methods are proxied on the metalsmith plugin, so you may access them like so:

```js
var markdown = require('metalsmith-markdownit');

metalsmith.use(markdown('zero', {html: true}).enable('emphasis', 'html_block', 'html_tag'))
```

You may provide a function to set the parser & renderer's environment on a per-page basis, should you need to:

```js
var markdown = require('metalsmith-markdownit');

metalsmith.use(markdown('default').env(function(page){ return page; }))
```

## License

  MIT
