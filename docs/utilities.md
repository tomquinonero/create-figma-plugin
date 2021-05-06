# Utilities

`@create-figma-plugin/utilities` is a library of helpful utility functions for common Figma plugin operations.

```
$ npm install @create-figma-plugin/utilities
```

## Events

```ts
import {
  emit,
  on,
  once
} from '@create-figma-plugin/utilities'
```

### emit(name, ...args)

Calling `emit` in the main context invokes the event handler for the
matching event `name` in your UI. Correspondingly, calling `emit` in your
UI invokes the event handler for the matching event `name` in the main
context.

All `args` passed after `name` will be directly
[applied](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)
on the event handler.

***Parameters***

- **`name`** (`T["name"]`)
- **`...args`** (`Parameters<T["handler"]>`)

***Return type***

```
void
```

### on(name, handler)

Registers an event `handler` for the given event `name`.

***Parameters***

- **`name`** (`T['name']`)
- **`handler`** (`T['handler']`)

***Return type***

Returns a function for deregistering the `handler`.

```
() => void
```

### once(name, handler)

Registers an event `handler` that will run at most once for the given
event `name`.

***Parameters***

- **`name`** (`T['name']`)
- **`handler`** (`T['handler']`)

***Return type***

Returns a function for deregistering the `handler`.

```
() => void
```


## Node

```ts
import {
  areSiblingNodes,
  collapseLayer,
  computeBoundingBox,
  computeMaximumBounds,
  computeSiblingNodes,
  createImagePaint,
  deduplicateNodes,
  getAbsolutePosition,
  getDocumentComponents,
  getSelectedNodesOrAllNodes,
  insertAfterNode,
  insertBeforeNode,
  isWithinInstance,
  loadFontsAsync,
  setAbsolutePosition,
  setRelaunchButton,
  sortNodesByCanonicalOrder,
  sortNodesByName,
  traverseNode,
  updateNodesSortOrder
} from '@create-figma-plugin/utilities'
```

### areSiblingNodes(nodes)

Checks if all nodes in `nodes` are sibling nodes.

***Parameters***

- **`nodes`** (`Array<SceneNode>`)

***Return type***

Returns `true` if all nodes in `nodes` are sibling nodes,
else `false`.

```
boolean
```

### collapseLayer(node)

Collapses `node` and all its child nodes in the layer list.

***Parameters***

- **`node`** (`SceneNode`)

***Return type***

```
boolean
```

### computeBoundingBox(node)

Computes the coordinates (`x`, `y`) and dimensions (`width`, `height`) of
the smallest bounding box that contains the given `node`.

***Parameters***

- **`node`** (`SceneNode`)

***Return type***

```
Rect
```

### computeMaximumBounds(nodes)

Computes the absolute coordinates of the top-left and bottom-right
corners of the smallest bounding box that contains the given `nodes`.

***Parameters***

- **`nodes`** (`Array<SceneNode>`)

***Return type***

```
[Vector, Vector]
```

### computeSiblingNodes(nodes)

Splits `nodes` into groups of sibling nodes.

***Parameters***

- **`nodes`** (`Array<SceneNode>`)

***Return type***

```
Array<Array<SceneNode>>
```

### createImagePaint(bytes)

Creates an `ImagePaint` object from the `bytes` of an image.

***Parameters***

- **`bytes`** (`Uint8Array`)

***Return type***

```
ImagePaint
```

### deduplicateNodes(nodes)

Deduplicates the nodes in `nodes`. Does not modify the original
`nodes` array.

***Parameters***

- **`nodes`** (`Array<SceneNode>`)

***Return type***

Returns a new array of `SceneNode` objects.

```
Array<SceneNode>
```

### getAbsolutePosition(node)

Returns the `x` and `y` position of the given `node` relative to the page.

***Parameters***

- **`node`** (`SceneNode`)

***Return type***

```
{
  x: number;
  y: number;
}
```

### getDocumentComponents()

Gets all the components in the current document.

***Return type***

```
Array<ComponentNode>
```

### getSelectedNodesOrAllNodes()

Gets the selected nodes, or all the top-level nodes on the current page if
no nodes are selected.

***Return type***

```
Array<SceneNode>
```

### insertAfterNode(node, referenceNode)

Inserts `node` after the `referenceNode` in the layer list.

***Parameters***

- **`node`** (`SceneNode`)
- **`referenceNode`** (`SceneNode`)

***Return type***

```
void
```

### insertBeforeNode(node, referenceNode)

Inserts `node` before the `referenceNode` in the layer list.

***Parameters***

- **`node`** (`SceneNode`)
- **`referenceNode`** (`SceneNode`)

***Return type***

```
void
```

### isWithinInstance(node)

Checks if the given `node` is within an Instance.

***Parameters***

- **`node`** (`SceneNode`)

***Return type***

Returns `true` if the `node` is within an Instance, else `false`.

```
boolean
```

### loadFontsAsync(nodes)

Loads the fonts used in all the text nodes in `nodes`.

***Parameters***

- **`nodes`** (`Array<SceneNode>`)

***Return type***

```
Promise<void>
```

### setAbsolutePosition(node, vector)

Sets the `node` to the given `x` and `y` absolute position.

***Parameters***

- **`node`** (`SceneNode`)
- **`vector`** (`object`)
  - **`x`** (`number`) – *Optional.*
  - **`y`** (`number`) – *Optional.*

***Return type***

```
void
```

### setRelaunchButton(node, relaunchButtonId [, options])

Sets a [relaunch button](https://figma.com/plugin-docs/api/properties/nodes-setrelaunchdata/)
on `node` for the command with the given `relaunchButtonId` as configured
under [**`"relaunchButtons"`**](#relaunchbuttons) in
`package.json`. `description` is the text displayed below the relaunch
button in the Figma UI.

See the [recipe for configuring relaunch buttons](#configuring-relaunch-buttons).

***Parameters***

- **`node`** (`BaseNode`)
- **`relaunchButtonId`** (`string`)
- **`options`** (`object`) – *Optional.*
  - **`description`** (`string`)

***Return type***

```
void
```

### sortNodesByCanonicalOrder(siblingNodes)

Sorts `siblingNodes` according to their layer list order. Does not modify
the original `siblingNodes` array.

***Parameters***

- **`siblingNodes`** (`Array<SceneNode>`)

***Return type***

Returns a new array of `SceneNode` objects.

```
Array<SceneNode>
```

### sortNodesByName(nodes)

Sorts `nodes` in alphabetical order. Does not modify the original
`nodes` array.

***Parameters***

- **`nodes`** (`Array<SceneNode>`)

***Return type***

Returns a new array of `SceneNode` objects.

```
Array<SceneNode>
```

### traverseNode(node, processNode [, stopTraversal])

Traverses `node` and its child nodes recursively in a *depth-first*
manner, passing each node to the specified `processNode` callback.

Each node is also passed to a `stopTraversal` function. If you return
`false` in `stopTraversal` for a particular node, then its child nodes
will not be traversed.

***Parameters***

- **`node`** (`SceneNode`)
- **`processNode`** (`(node: SceneNode) => void`)
- **`stopTraversal`** (`(node: SceneNode) => boolean`) – *Optional.*

***Return type***

```
void
```

### updateNodesSortOrder(siblingNodes)

Updates the layer list sort order to follow the sort order of the nodes
in the `siblingNodes` array.

***Parameters***

- **`siblingNodes`** (`Array<SceneNode>`)

***Return type***

```
boolean
```


## Number

```ts
import {
  evaluateNumericExpression,
  isValidNumericInput
} from '@create-figma-plugin/utilities'
```

### evaluateNumericExpression(value)

Evaluates the given numeric `expression`.

***Parameters***

- **`value`** (`string`)

***Return type***

Returns the result of evaluating the given `expression`.

```
null | number
```

### isValidNumericInput(value [, options])

Checks if `value` is a numeric expression, as input by a user. “Partial”
inputs are considered valid. Set `options.integersOnly` to `true` to check
that the expression contains only integers. `options.integersOnly` defaults
to `false` if not specified.

***Parameters***

- **`value`** (`string`)
- **`options`** (`object`) – *Optional.*
  - **`integersOnly`** (`boolean`)

***Return type***

Returns `true` if `value` is a valid numeric expression,
else `false`.

```
boolean
```


## Object

```ts
import {
  cloneObject,
  compareObjects,
  compareStringArrays,
  extractAttributes
} from '@create-figma-plugin/utilities'
```

### cloneObject(object)

Creates a deep copy of the given object.

***Parameters***

- **`object`** (`T`)

***Return type***

```
T
```

### compareObjects(a, b)

Performs a *deep* comparison of objects `a` and `b`.

***Parameters***

- **`a`** (`JsonValue | undefined`)
- **`b`** (`JsonValue | undefined`)

***Return type***

Returns `true` if `a` and `b` are the same, else `false`.

```
boolean
```

### compareStringArrays(a, b)

Compares the string arrays `a` and `b`.

***Parameters***

- **`a`** (`Array<string>`)
- **`b`** (`Array<string>`)

***Return type***

Returns `true` if `a` and `b` are the same, else `false`.

```
boolean
```

### extractAttributes(array, attributes)

Extracts the specified list of `attributes` from the given `array` of
objects.

***Parameters***

- **`array`** (`Array<T>`)
- **`attributes`** (`Array<keyof Partial<T>>`)

***Return type***

Returns an array of plain objects.

```
Array<Partial<T>>
```


## Settings

```ts
import {
  loadSettingsAsync,
  saveSettingsAsync
} from '@create-figma-plugin/utilities'
```

### loadSettingsAsync(defaultSettings)

Loads your plugin’s `settings` (stored locally on the user’s computer).
Values in `settings` default to an optional `defaultSettings` object.

***Parameters***

- **`defaultSettings`** (`Settings`)

***Return type***

```
Promise<Settings>
```

### saveSettingsAsync(settings)

Saves the given `settings` for your plugin (stored locally on the user’s
computer).

***Parameters***

- **`settings`** (`Settings`)

***Return type***

```
Promise<void>
```


## String

```ts
import {
  formatErrorMessage,
  formatSuccessMessage,
  formatWarningMessage,
  pluralize
} from '@create-figma-plugin/utilities'
```

### formatErrorMessage(message)

Adds a `✘` prefix to the given `message`.

***Parameters***

- **`message`** (`string`)

***Return type***

```
string
```

### formatSuccessMessage(message)

Adds a `✔` prefix to the given `message`.

***Parameters***

- **`message`** (`string`)

***Return type***

```
string
```

### formatWarningMessage(message)

Adds a `⚠` prefix to the given `message`.

***Parameters***

- **`message`** (`string`)

***Return type***

```
string
```

### pluralize(number, singular [, plural])

Returns `singular` if `number` is exactly `1`, else returns `plural`.
`plural` defaults to `${singular}s` if not specified.

***Parameters***

- **`number`** (`number`)
- **`singular`** (`string`)
- **`plural`** (`string`) – *Optional.*

***Return type***

```
string
```


## UI

```ts
import {
  showUI
} from '@create-figma-plugin/utilities'
```

### showUI(options [, data])

Renders the UI correponding to the command in a modal within the Figma UI.
Specify the width, height, and visibility of the UI via `options`.
Optionally pass on some initialising `data` from the command to the UI.

See the [recipe for adding a UI to a plugin command](#adding-a-ui-to-a-plugin-command).

***Parameters***

- **`options`** (`ShowUIOptions`)
- **`data`** (`T`) – *Optional.*

***Return type***

```
void
```