# Diving into React
The first project (with comments for explanations should be used for reference here)

You can go to <a href="https://github.com/iamsuteerth/Basic-ReactJS-Projects/tree/main/01_pizza_menu">Pizza Menu (01)</a> to take a look at it.

## Debugging 
- Make sure your app is running if your changes are not getting implemented
- Stop and restart the app to see if the issue resolves
- Keep the consoles open to check for error codes
- Use the error messages to debug your code using tool like google, chatGPT and stackOverflow (These are your best friends as a develoepr)
- Always use ESLint while working on a react project.

For this basic project, and honestly the first react project. The code is not divided into modules like it is done in a professional work environment.

## JSX
Components return block of JSX which allows embedding of JS, CSS, Other components and HTML

- While working with jsx, a rookie mistake can be to use <div class=""> but instead, we have to use `className` instead

if else statements are not allows

JSX produces a JS expression

## Props
- They are used to transfer data between parent and child components. 
- Props are read only
- Props is data coming from outside and can be controlled only by the parent component
- State is internal data that can be updated by component logic
- React has a ONE WAY data flow unlike angular
- Never forget to include the prop arguments in `{}` while destructuring!

## React Fragment
Lets you group up components using `<></>` OR `<React.Fragment key=""><//React.Fragment>`

## Component Tree Structure
App -> Header, Menu, Footer

Menu -> Pizza,...

Footer -> Order

Component -> Data, JSX, ( Appearance -> HTML, CSS, JS { } )