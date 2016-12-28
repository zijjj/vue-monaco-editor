# vue-monaco-editor

> Monaco Editor Vue Component

> Work in Progress!

> Based off [React Monaco Editor](https://github.com/superRaytin/react-monaco-editor)

## Setup

``` bash
npm install vue-monaco-editor --save
```

## Simple Vue Use

```js
import MonacoEditor from 'vue-monaco-editor'

// use in component
export default {
  components: {
    MonacoEditor
  }
}
```

## Component Props

| Option        | Type          | Default | Description
|:-------------|:-------------|:-------|:-------|
| language      | String        | "javascript" | |
| height        | Number/String | "100%" ||
| width | Number/String | "100%" ||
| code | String | "// code \n" | Initial code to show |
| theme | String | "vs" | |
| highlighted | Array[Object] | `[{ number: 0, class: ''}]` | Lines to highlight with numbers and `.classes` |
| changeThrottle | Number | 0 | milliseconds to throttle callback bound to `codeChange` event |
|srcPath| String | "" | see *Webpack Use*
| editorOptions | Object | { ... } | see [Monaco Editor Options](https://microsoft.github.io/monaco-editor/api/interfaces/monaco.editor.ieditorconstructionoptions.html) |

## Component Events

*These events are available to parent component*

| Event        | Returns          | Description
|:-------------|:-------------|:-------|
|mounted|`editor`[editor instance]|Emitted when editor has mounted|
|codeChange|`editor`[editor instance]|Emitted when code has changed|

## Example

*Component Implementation*
```vue
<MonacoEditor
    height="600"
    language="javascript"
    :code="code"
    :editorOptions="options"
    :highlighted="highlightLines"
    @mounted="onMounted"
    @codeChange="onCodeChange"
    >
</MonacoEditor>
<button @click="clickHandler">Log value</button>
<input v-model="highlightLines[0].number" placeholder="primary highlight #">
<input v-model="highlightLines[1].number" placeholder="secondary highlight #">
```

*Parent*
```js
module.exports = {
  components: {
    Monaco
  },
  data() {
    return {
      code: '// type your code \n',
      highlightLines: [
        {
          number: 0,
          class: 'primary-highlighted-line'
        },
        {
          number: 0,
          class: 'secondary-highlighted-line'
        }
      ]
    };
  },
  methods: {
    onMounted(editor) {
      this.editor = editor;
    },
    onCodeChange(editor) {
      console.log(this.editor.getValue());
    },
    clickHandler() {
      console.log(this.editor.getValue());
    }
  },
  created() {
    this.options = {
      selectOnLineNumbers: false
    };
  }
};
```

## Webpack Use

By default, monaco-editor is loaded from a cdn asyncronously using `require`. To use a local copy of `monaco-editor` with webpack, we need to expose the dependency in our build directory:

`npm install copy-webpack-plugin --save-dev`

Add this to your webpack.config.js:

```js
const CopyWebpackPlugin = require('copy-webpack-plugin');
module.exports = {
  plugins: [
    new CopyWebpackPlugin([
      {
        from: 'node_modules/monaco-editor/min/vs',
        to: 'vs',
      }
    ])
  ]
};
```

Then, specify the build directory path in the `srcPath` prop. See `src/App.vue` for an example.

## Dev Use

```
git clone [this repo] .
npm install
npm run dev
```

Edit `src/App.vue`
