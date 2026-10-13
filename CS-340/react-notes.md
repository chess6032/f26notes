# React Notes

> *Reading: [Learning React, 2nd Edition, by Alex Banks & Eve Porcello. Published by O'Reilly Media, Inc. 2020.](https://learning.oreilly.com/library/view/learning-react-2nd/9781492051718/).*

- Two libraries: `React` and `ReactDOM`. 
  - `React`: Creating.
  - `ReactDOM`: Rendering.
- JSX: HTML-like tag syntax inside JavaScript.

## React Elements

### DOM elements

- HTML is a set of instructions for the browser on how to construct the DOM. **HTML elements become DOM elements when the browser loads and renders the HTML.**
  - For reactive applications, JavaScript has a [DOM API](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model) that can modify the DOM as well. 
    - e.g., `document.createElement` or `document.appendChild`.
    - This allows for Single Page Applications (SPA).
- **React updates the browser DOM for us**. 
  - We do not interact with JS's DOM API directly. Instead, we write instructions for what we want React to build, and React takes care of rendering & reconciling the elements we instructed it to create.

### React elements vs. DOM elements

Look the same, but actually very different.

- REACT element: description of what the DOM element should look like. i.e., instructions for how the browser DOM should be created.
  - More specifically, a *JavaScript literal* containing instructions for how the browser DOM should be created.

### React element fields

- `type`: Type of HTML (or SVG(?)) element to create.
- `props`: Data & child elements required to construct a DOM elem.
  - `children`: for displaying other nested elements as text.
- `_owner`
- `_store`
- `$$typeof`
- `key`
- `ref`

### Rendering React elements (without components)

```tsx
const dish = React.createElement(
  "h1", // type
  null, // props (id, class, etc.)
  // All properties after this specify the element's props.children,
  //   which may be text or more React elements.
  "Baked Salmon" 
);

ReactDOM.render(
  dish, // React element to add to DOM. Could also be an array of siblings(as of 2017).
  document.getElementById("root"); // JS DOM API to get root element.
);
```

> [!NOTE]
> Use `className` to specify an HTML `class` property.

> [!TIP]
> If instancing a bunch of elements by iterating over an array, set each element's `key` property to its index in the array.

Of course, nowadays, instead of using `React.createElement()` and `ReactDOM.render()`, we would use JSX syntax to build the React elements and React components to hold their data (and stuff).

## React components

- Each individual part of the user interface is built out of a React *component*.
- React components are *functions*. **React components return React elements.**

### JSX

<pre><code><<i>ComponentName</i> <i>prop1</i>="&hellip;" <i>prop2</i>={<i>javaScript&hellip;</i>} &hellip; />
</code></pre>

- Properties may either be **strings** or **JS expressions**.
- JS expressions are enclosed in curly braces (`{}`).
  - This includes non-string JS literals. e.g., `{false}`.
- `className` specifies the HTML "class" property.
  - (JSX can't use `class` because that is already a JS keyword.)

#### Example

Suppose you have a React component called `IngredientsList` that takes in a `list` property. 

```js
function IngredentsList({ list }) { 
  // ... 
}
```

<!-- In React/JS, creating an instance of that component would look like this:

```js
React.createElement(IngredientsList, {list: [ /* ... */ ]} );
``` -->

Creating an instance of that component in JSX would look like this:

```jsx
<IngredientsList list={[ /* ... */ ]} />
```

(This is in contrast to a `React.createElement()` call.)

#### Babel (Converting JSX &rarr; React)

JSX is not browser-readable&mdash;that is, there is *no* browser that supports JSX syntax. JSX must first be converted into `React.createElement()` calls before it can be rendered by a browser. 

A commonly-used tool to convert JSX to React source code is [**Babel**](https://babeljs.io/).

That's all the notes I'm going to take on this bc I don't think it will be on the midterm.

### React fragments

- React components must return a single, top-level element. That is, you can't have it return siblings. 
- ^ But oft times you want to just return siblings. 
- ^ To get around this, **wrap the siblings in a fragment:** `<React.Fragment> ... </React.Fragment>`
  - Shorthand (JSX): **`<> ... </>`**. 
- React Fragments are **not visible in the DOM.**

## Debugging React in Chrome

With the React Developer Tools Extension for Google Chrome, you can look at the present state of the component tree.

## Webpack

I don't think we covered this in class, so I'm not going to take notes on it. But it sounds like it's kinda like a toolchain or ecosystem for compiling React code.
