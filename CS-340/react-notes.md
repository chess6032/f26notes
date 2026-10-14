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

> [!NOTE]
> "React *element*" &ne; "React *component*". The *component* is the *function itself*, e.g. `function App()`, while the *element* is `<App />`, whose *type* is that component.  
> 
> With that said, I use the terms "element" and "component" pretty interchangeably across this page. (Mostly because I don't know enough to tell when the textbook/slides are talking about one vs. the other.)

### JSX

<pre><code><<i>ComponentName</i> <i>prop1</i>="&hellip;" <i>prop2</i>={<i>javaScript&hellip;</i>} &hellip; />
</code></pre>

- Properties may either be **strings** or **JS expressions**.
- JS expressions are enclosed in curly braces (`{}`).
  - This includes non-string JS literals. e.g., `{false}`.
  - This includes more JSX.
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

## Conditional rendering of React Components

### If/else

Use the ternary operator (<code>&hellip; ? &hellip; : &hellip;</code>).

e.g.:

```jsx
<BrowserRouter>
  {isAuthenticated() ? (
    <AuthenticatedRoutes />
  ) : (
    <UnauthenticatedRoutes />
  )}
</BrowserRouter>
```

### If

Use <code><i>condition</i> && <i>elementJSX</i></code>.

e.g.:

```jsx
<div className="col p-3">
  {!displayedUser.equals(currentUser) && (
    <p id="returnToLoggedInUser">
    Return to{" "}
    <Link to={`./${currentUser.alias}`} onClick={switchToLoggedInUser} />
    </p>
  )}
</div>
```

## React Router

In a single-page app, browser features like bookmarks, history, URLs, etc. become problematic since each page is no longer tied to a separate HTML document. [Routing](https://oreil.ly/jD1HC) solves this issue.

- Routing: Defining endpoints for client's requests.
  - These endpoints work in conjunction w/ browser's location & history objects.
  - These endponints identify requested content so JS can load & render the appropriate UI.

React doesn't come with a standard Router (unlike most web frameworks, e.g. Angular). But a router created by engineers Michael Jackson and Ryan Florence, titled simply the React Router, has been widely adopted. This section covers notes on that project.

### Usage

> Package name: `"react-router-dom"` (or sometimes just `"react-router"`? idk the difference)

- Wrap your top-level `<App />` component in a `<BrowserRouter>` (often imported as just `Router`.)
- Set up route configurations in your top-level `<App />` component: a `<Routes>` component, with `<Route>` children.
- Recommended: Have a `<nav>` component full of `<Link>` components that link to these routes.
  - Links' `to` property (string): path.

### `<Route>` component properties

- `path`: Path that appears in URL.
- `element`: React element (component?) to load for that path.

When the browser's location (path in the URL, I think?) matches that `path`, that `element` is displayed.

> [!TIP]
> - Define a home page by using the path `"/"`.
> - Include at least one route with a path of `"*"` to catch unknown paths. Have this route's element be a 404 page.

### Other properties passed to routes

In addition to component properties (see above), you can use (what I believe are) React hooks to grab other properties the `<BrowserRouter>` passes to its children. 

e.g., `let location = useLocation()` &rarr; `location.pathname` shows the window's present location.

### Nested Routes

Suppose you want to map a `Foo` element to `/foo` and a `FooBar` element to `/foo/bar`.

The route set up would look as such:

```jsx
<Routes>
  <Route path="foo" element={<Foo />}>
    <Route path="bar" element={<FooBar />} />
  </Route>
</Routes>
```

Then, the `Foo` component's function would have to include an `<Outlet />` in its return. This is where those nested components will render (if the path matches).

> [!NOTE]
> The slides called these "Layout Routes".

### Redirects

> [!NOTE]
> As of React Router v6, the `<Redirect>` component is deprecated&mdash;completely removed, in fact&mdash;in favor of `<Navigate>`, route-level redirects, and `redirect()`. The book I'm reading was written/published before that version released.

```jsx
<Routes>
  {/* other routes 'n shih... */ }
  <Redirect from="src" to="dest">
</Routes>
```

- If the brower's location matches the path specified in `from`, the browser is "redirected" to the path specified in `to`.
  - It's best practice to have the paths start with `"/"`.
  - The `to` field can also be a location object (`{ pathname, search, state }`).

Useful if a page is moved from one path to another, so that old links (or bookmarks, etc.) to that page are still supported.

### `useRoutes()`

`<Routes>` and `<Route>` are wrappers around `useRoutes()`. 

`useRoutes()` takes in an array of `{ path, element, children? }` objects, and returns the equivalent of a `<Routes>` object w/ `<Route>` children.

### Route parameters (`useParams()`)

You can add parameters to a path w/ route params, which are retrieved with the `useParams()` hook.

- Params are defined in the `path` property of a `Route`. They are passed to its `element`, who uses `useParams()` to retrieve them.
  - **Params are indicated with a leading `:` in the path**. 
    - e.g., `foo/bar/:param/fizz`.
- **`useParams()` returns an object whose properties match the name of the path's route parameters**.
  - Values are *always strings*.
- **You can make parameters *optional* by adding a succeeding `?`**&mdash;called an **optional segment**.
  - e.g., `foo/bar` and `foo/bar/fizzbang` will both match `path="foo/bar/:sillyString"`.
  - When an optional segment is absent, its parameter's value is `undefined`.

```jsx
// Route definition
<Route path="/users/:userId/posts/:postId" element={<Post />} />

// Post.jsx
import { useParams } from "react-router-dom";

export function Post() {
  const { userId, postId } = useParams();
  return <h1>Post {postId} by user {userId}</h1>;
}
```

> [!NOTE]
> Prior to React Router v5.1, parameters were passed as a `match` prop to any component a `<Route>` component renders.

### Query string parameters

You can use `useSearchParams()` to access the parameters in the *query string* of the current path/location/route.

e.g.:

```js
// URL query string: "?page=2&filter=active"
const [ searchParams, setSearchParams ] = useSearchParams();
const page = searchParams.get("page");
const filter = searchParams.get("filter");
```

### Navigation with `useNavigate()`

`useNavigate()` returns a function that changes the URL programmatically. This function accepts a (string) path and "redirects" the browser to that path.

- Use `<Link to="..." />` for plain clickable links, and `useNavigate()` when navigation is a side effect of something else happening.
- Conventionally, `useNavigate()`'s return is bound to a variable called `navigate`.
- You can add a `{ replace: true }` parameter to *replace* the previous history with the new path (instead of pushing).
- You can pass in `-1` to go to the previous page.

> [!TIP]
> Create a navigation bar with `<NavLink>`... or something? Idk.

## React Hooks/State

For this class, you need to know the following (built-in) React hooks:

* `useEffect()`
* `useCallback()`
* `useMemo()`
* `useState()`
* `useContext()`
* `useRef()`

Those last three are for managing React state.

You'll also need to know [`useNavigate()`](#navigation-with-usenavigate), [`useLocation()`](#other-properties-passed-to-routes), [`useParams()`](#route-parameters-useparams), and [`useSearchParams()`](#query-string-parameters), which are React *Router* hooks.

### Hooks

*Hooks* contain reusable code logic that is **separate from the comopnent tree**. Thus, they allow us to *hook up* functionality to our components.

### Using React hooks

1. Import the hook.
2. Call the method and destructure its children.
3. Use the children.

### React state

Properties are one half of the picture. State is the other half.

- The *state* of a React app is driven by data that has the ability to change.
- State and properties are related to each other. **When the state of a component tree changes, so do its properties.**
  - Thus, when state changes, components must be re-rendered.
- A React component that does *not* modify state is called a *pure* component.
- Good practice: 
  - Pass state *down* the component tree via plain *properties*. 
  - To pass state *up* (e.g., when the state setter was defined in some parent component in the tree), have the parent pass the children a function that can change it. That is, a function that calls the setter.
    - e.g., parent gives `const addColors = id => setColors([...colors, id]);` to child's `onAdd` property, and the child calls it when a button is clicked or something.
    - This will require adding a function to the child's properties. 
      - The signature for that would be `(newVal: T) => void`, where `T` is the state's type.
      - If you're passing in a interface for the properties, make this member *optional* on the interface.
      - Give the child component a *default value* for the function that is a *no-op* (`() => {}`).

### `useState()`

- Returns an array:
  - **Elem 0: *Variable* containing state data.**
    - ("State variable")
  - **Elem 1: *Function* to update the state data**.
    - ("Updater function")
    - This function does NOT update the state variable.
  - Naming convention: `[ statename, setStatename ] = setState( /* initial val... */ );`
- When the updater function is called (and is passed in a value *not* equal to the state's current value), it rerenders the component (that called `useState()`).
- **`useState()` takes in one argument: an *initial* value for the state.**
  - i.e., this value is only used the *first* time the component calls `useState()`.
- The updater function can take two parameter types: a value or a function.
  - Passing in a value updates the internal state&mdash;but does *not* update the *state variable* originally returned (as elem 0) by `useState()`.
  - The function signature is something like `(previous: T) -> T`, where `T` is the state's data type and `previous` is the state's current value; the state's value is then overwritten with the function's return.
  - ^ Between these two, **passing in a *function* is generally preferred**, as it helps avoid "stale closures".
    - (e.g., for `[ count, setCount ] = useState(0);`, calling `setCount(count + 1);` over and over again will repeatedly set the internal count to `1`, because the `count` variable stays `0` across the set calls. Meanwhile, calling `setCount(previous => previous + 1);` *will* increment the internal count as expected.)

> [!NOTE]
> In TypeScript, the state's data type can be inferred from the argument passed into `useState()`. But you can also define it explicitly by calling <code>useState<<i>T</i>>(&hellip;)</code>, where *`T`* represents a type.

## Misc

### Debugging React in Chrome

With the React Developer Tools Extension for Google Chrome, you can look at the present state of the component tree.

### Webpack

I don't think we covered this in class, so I'm not going to take notes on it. But it sounds like it's kinda like a toolchain or ecosystem for compiling React code.
