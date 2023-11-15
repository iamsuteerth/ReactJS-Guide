# Diving into React
The first project (with comments for explanations should be used for reference here)

You can go to <a href="https://github.com/iamsuteerth/Basic-ReactJS-Projects/tree/main/01_pizza_menu_lv1">Pizza Menu (01)</a> to take a look at it.

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

# Handling Events
Refer to this project <a href="https://github.com/iamsuteerth/Basic-ReactJS-Projects/tree/main/02_steps_lv1">Steps (02)</a> to understand this section

We handle events right where they happen, like by using onClick

## State
State is the most important concept in react. 

State is data a component can hold over time, its like memory of a component.

State variables are `piece of state`. React re renders component views (which make up the UI) when state is changed.

React keeps data and UI in sync using state.
```js
const [step, setStep] = useState(1);
```
This is how state variables are created

You should not update state variables manually, mutating objects MAY work though but its BAD PRACTICE

React calls the component function every time it is re rendered where the state is maintained unless it is unmounted.

## Updating state based on current state
Use callbacks when updating state based on current state
```jsx
setStep(step + 1);
setStep(step + 1);
// This won't work 
setStep((step) => step + 1);
setStep((step) => step + 1);
// This WILL 
```
- ONE Component, ONE STATE
- UI = f(state)
- React application is all about changing states all the time

# Travel List App
Refer to this project <a href="">Travel List (03)</a> to understand this section

Component Tree
> Logo
>
> Form
>
> Packing List
> > Items
> >
> > Buttons
> Stats 

### How to add ternary strike through text
```jsx
<span style={item.packed ? { textDecoration: "line-through" } : {}}>
```
### How to auto create an array with selection options 
```jsx
Array.from({ length: 20 }, (_, i) => i + 1).map();
```
### How to prevent auto refresh of page on submitting form?
```jsx
function handeSubmit(e){
    e.preventDefault();
}
```
We use controlledElements while working with forms instead of using the `event e object`

Form input fields maintain their own states inside DOM and makes it hard to read. 

- Create piece of state
- Add an onChange property 
```jsx
 <input type="text" placeholder="Item..." value={description} onChange={(e) => setDescription(e.target.value)}></input>
 ```
 We cannot pass data from list to form as they are sibling components.

Refer to this project <a href="https://github.com/iamsuteerth/Basic-ReactJS-Projects/tree/main/03_travel_list_lv1">Travel List (03)</a> to understand this section

 ## State vs Props
- State is internal data and owned by components
- Props are maintained by parent components
- Props are read only
- State are like components' memory and can be updated by the component itself. 
- Updating state causes component to re render. However, when a component receives updated props, it re renders as well.
- Props are used by parent to configure the child components.

## Thinking in REACT
- Imperative to know `how to work with react API`
- Mastering the skill of thinking in REACT 

### The process
> Break the desired UI into `components` and establish the component tree.

> Build a static version without state

> Think about state
> > When to use state
>
> > Types of state : Local vs global
>
> > Where to place each pice of state

> Establish data flow
> > Child to parent communication
>
> > Accessing global state

### Fundamental questions you should be able to answer while developing a react app
- How to break up a UI design into components
- How to make some components reusable
- How to assemble UI from reusable components
- What pieces of state do I need for interactivity
- Where to place state
- What types of state can or should I use?
- How to make data flow through app

#### Give each piece of state a home

## Types of State
### Local State
- Needed only by one or few components
- Defined in a component and only its children have access to it
- Always start with local state
### Global State
- State which many components might need
- Shared state that is accessible to every component in the entire application such as the shopping cart content in the entire application
> Context API, REDUX

## Guideline for state
```cpp
// When to create state
if(need to store data){
    if(will data change at some point){
        if(can be computer from existing state){
            DERIVE STATE
        }
        else{
            if(should it re-render component){
                PLACE NEW PICE OF STATE IN COMPONENT
            }
            else{
                REF(useRef)
            }
        }
    }
    else{
        REGULAT CONST VARIABLE
    }
}
// Where to place state
if(only used by component){
    LEAVE IN COMPONENT
}
else{
    if(also used by child component){
        PASS TO CHILD VIA PROPS
    }
    else{
        if(used by one or few sibling components){
            LIFT STATE UP TO FIRST COMMON PARENT
        }
        else{
            PROBABLY GLOBAL STATE
        }
    }
}
```
For most of the elements, RUN this `pseudocode` to THINK in react.

The only goal of the form components is to add new items to the array

While lifting the state, you simply move to the immediate parent component

We passed onAddItems which updates the `items` state as a prop to the FORMS component

### Sharing state with Sibling component can only be done by lifting the state up and then we can use props to give data to both the components
### The setState function needs to be passed as a prop to the child widget
This can be called as inverse data flow in some sort. Its a workaround.

## Derived State
Computed from another piece of state

Consider this snippet
```js
const [car, setCart] = useState([
    {name: "JS Course", price: 15.99},
    {name: "React Course", price: 14.99},
]);
const [numItems, setNumItems] = useState(2);
const [totalPrice, setTotalPrice] = useState(30.98);
```
Issues
- Three separate pieces of state, even though the number of items and proce depend on cart
- Need to keep them in sync
- 3 state updates will cause 3 re renders

Solution
```js
const [car, setCart] = useState([
    {name: "JS Course", price: 15.99},
    {name: "React Course", price: 14.99},
]);
const numItems = cart.length;
const totalPrice = cart.reduce((acc, cur) => acc + cur/price, 0);
```
Cart state is the single source of truth. Automatic re calculation for derived state upon state update

## Children prop
### Reusable Button
It is an implicit prop which all components receive which is the content between <></>

- Allows us to pass JSX into an element (USP feature)
- Essential tool to make reusable and configurable components
- Really useful for generic componenets that don't know their content before being used

#
Refer to this project <a href="https://github.com/iamsuteerth/Basic-ReactJS-Projects/tree/main/04_eat_n_split_lv1">Eat N Split (04)</a> as it has all the elements from all the guide pages covered till now!
