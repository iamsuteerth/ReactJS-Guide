Earlier, server side rendering was used to create webpages 

View + Data => Render Webpage => Client

Now we have single page websites which are appropriately termed as web apps because the rendering dynamic has shifted towards the client

Data => API <-> Browser + Web => Render Webpage 

Web apps are about handling data and displaying it in a UI and its KEY to keep UI and data in sync

Vanilla JS makes it almost impossible to keep data in sync (pieces of data are termed as pieces of state)

Frameworks take this problem away from us, enforce a structured code writing method and give devs a consistent way of building front end apps.

Like flutter is all about WIDGETS, react is all about COMPONENTS which are the building blocks of UI.

JSX is a declarative syntax where we tell what a component looks like based on the state. It is abstracted from DOM. JSX involves CSS, HTML and JS

React REACTS to state changes (manual) by re rendering the UI

React is just a library which is a VIEW layer. NextJS is a framework built ON TOP of react.

```js
// Create app
npx create-react-app@5 pizza-menu
```
Code -> src, Assets -> public