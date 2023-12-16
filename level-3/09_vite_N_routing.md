# Vite

It's basically a MUCH better tool to build react apps instead of create-react-app

## How to create an app?

```js
npm create vite
// Select config
cd vite-project
code .
// Config ESLint
npm install --save-dev eslint vite-plugin-eslint eslint-config-react-app
```

### Configuring ESLint

```js
// Create .eslintrc.json at root
{
    "extends" : "react-app"
}
```

```js
// In vit.config.js
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
import eslint from "vite-plugin-eslint";

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [react(), eslint()],
});
```

# Routing

**Client Side Routing**

- We match different URLs to different UI views

- This enables users to navigate between different application screens using browser URL

- Keeps the UI in sync with current browser URL
  _Routing is handled by react router_

## SPAs

- Apps executed entirely on client
- Routes are different URLs correspond to different views
- Javascript is used to update the page
- Page is NEVER reloaded
- So the feel is that of a native app
- Additional data might be loaded from a web API

## Styling Options
| Styling Options | Where?                          | How?                  | Scope         | Based On |
|-----------------|---------------------------------|-----------------------|---------------|----------|
| Inline CSS      | JSX Elements                    | style prop            | JSX Elemenent | CSS      |
| CSS/Sass File   | External file                   | className prop        | Entire App    | CSS      |
| CSS Modules     | One external file PER component | className prop        | Component     | CSS      |
| CSS in JS       | External file or component file | Creates new Component | Component     | JS       |
| Tailwind CSS    | JSX Elements                    | className prop        | JSX Element   | CSS      |

Alternative to styling with CSS with UI Libraries like ChakraUI, MaterialUI etc.

CSS modules come along with vite and create-react-app

**Global CSS**
```css
:global(.test){
  background-color: red;
}
```
Global function is important when we are working with classes provded by external sources.

*Use camel case for css modules*

## Nested Routes
When the component inside a component changes with change in the URL.

**<Outlet/>** is where a nested route is rendered which is similar to children prop.

**Index** route is basically the default child route if none of the children routes match

## The URL for state management
- The url is an excellent place to store UI state and an alternative to useState 
  - Open/Closed panels, currently selected list item, list sorting order etc.
1. Easy way to store state in a global place, accessible to all components in the app
2. Good way to pass data from one page to another
3. Makes it possible to bookmark and share the page with exact UI state it had at the time

### useSearchParams() and useParams()
It is like useState but to get values from query parameters.
```jsx
const [searchParams, setSearchParams] = useSearchParams();
const lat = searchParams.get("lat");
const lng = searchParams.get("lng");
const x = useParams();
  console.log(x);
```

### Programmatic navigation with useNavigate
Move to a new url without clicking anything such as right after submitting a form.

We use useNavigate() for this which was called useHistory() previously.
