# NOTE - Theoretical Page
## Component vs Instance vs Element
| Component                                      | Instance                                                             | Element                                                                                        |
|------------------------------------------------|----------------------------------------------------------------------|------------------------------------------------------------------------------------------------|
| Description of a piece of UI                   | Created when we "use" components as the component function is called | JSX is converted to react.createElement() calls                                                |
| Returns a react element usually written in JSX | Actual physical instance of a component                              | A react element is result of these function calls                                              |
| Blueprint or a template                        | Has a lifecycle and has its own states and props                     | Contains all the information necessary to create DOM elements converted to DOM elements (html) |

The $$Type is simply a security feature that React has implemented in order to protect against cross-site scripting attacks.

Symbols are one of the JavaScript primitives, which cannot be transmitted via JSON, or in other words, this means that a symbol
cannot come from an API call.

So if some hacker would try to send us a fake React element from that API, then React would not see it as a symbol.

And so then React would not include that fake React element into the DOM so protecting us against Cross Site Scripting.
#
When we call a component like this `Component()`, react does see it but it doesn't treat it like a component but instead has the raw outpute content

You shouldn't do this as it violates the rule of hooks. The state is also not managed inside the component either. Always render it inside JSX

## How are components displayed on the screen?
1. Render is triggered (By a state update somewhere)
2. Render Phase
    - React calls component functions and figures how the DOM should be updated
- Rendering doesn't update the DOM. Rendering only happens internally inside react and doesn't produce visual changes
3. Commit Phase
    - React actually writes to the DOM, updating, inserting and deleting elements.
    - This is the phase where the "rendering" occurs
4. Browser Paint

### Render Triggers
> Initial render of application

> State is updated in one or more component instances (re-render)

The render process is triggered for the entire application

NOT necessary for the DOM to be changed as the component functions are called for re renders

It just appears that only the components with changed state are re rendered

`Renders are not triggered immediately, but scheduled for when the JS engine has some 'free time'. There are also batching of multiple setState calls in event handlers`

# IMP
## How react works behind the scenes
1. ~~Rendering updating the screen/DOM~~
2. ~~React completely discards the OLD VIEW (DOM) on re-render~~

# Render Phase
`Component instances that triggered re render` -> `React elements` -> `New Virtual DOM`
> 1. Initial Render
>       - Component tree -> React element tree (virtual dom)
>
> - Relatively fast to create multiple trees
> - Overused and overhyped word for people to describe how react works. The virtual DOM is just an object in realty. `It has nothing to do with shadow DOM`
> 2. Re renders
> - The updated state creates a NEW virtual DOM. What's important to know here is that a parent causes ALL it's child to "re render" as well even if the props are changed or not.
>
> - A root update will cause the entire virtual dom to be re created. This is necessary as react doesn't know whether chilren will be affected or not.
> 3. Reconciliation + Diffing 
>       - Current fiber tree goes into this phase to result in an updated fiber tree

## Reconciliation and why do we need it?
Why not update the entire DOM whenever state changes?
- Inefficient and wasteful because
    - Writing to the DOM is slow
    - Usually only a small part of the DOM needs to be updated

This is where reconciliation comes into play. Where decision is made for which DOM elements to be inserted/deleted or updated in order to reflect the latest changes. This is processed by a `reconciler`. Its the `heart of react` and is called ***fibre***

Virtual DOM --Initial-Render--> Fiber tree

Fiber tree : Internal tree that has a fiber for each component instance and DOM element. This is NOT re-created on every render. It is mutated over and over again in future reconciliation.

The props, state, side effects and hooks are stored in these fibers in the fiber tree. It also contains a queue of work 
- Updating state
- Updating ref
- Performing DOM updates
- Running registered side effects

`Fiber is often called a unit of work`

They are arranged in a different way compared to the typical parent child relationship

Each first child has a link to its parent and the siblings are arranged in a linked list. This is sort of like a `B tree in DSA`

Both VDOM and Fiber tree contain react elements AS WELL as the regular DOM elements. They are a complete representation of the DOM structure.

Work here can be done in async
- Rendering process can be split into chunks, tasks that can be prioritized, and work can be paused, reused or thrown away.
- Enables concurrent features like suspense or transitions
- Long renedrs won't block JS engine

`This is possible because this render phase DOESN't produce any visible change to the DOM yet`
## Reconciliation in action
1. State change
2. New VDOM
3. Fiber tree parsing to check what changed based on VDOM
4. This is the process of diffing
5. DOM deletions and updations are marked
6. List of DOM updates is created for the NEXT phase
7. This is the result of the reder phase.

# Commit Phase
- The list of dom updates are reflected into dom synchronously here so that the DOM never shows partial runs

- The fiber tree then becomes the tree for next render cycle

- Thus the browser proceeds to update the screen where the UI is updated on the screen.

- The commit phase is carried out by `ReactDOM`. `React` doesn't actually touch the DOM and thus can be used on different platforms

- React Native can be used for IOS and android and many other `'renderers'` update the UI. Its a list of updates made to the components.

## How diffing works?
Diffing is based on TWO assumptions
- Two elements of different types will prodct different trees
- Elements with a stable key prop stay the same across renders
- This brings O(n^3) to O(n) since diffing is comparing elements step by step based on the position in their tree.

### Same position, different element
`<div>` -> `<header>`

React assumes that entire sub tree is invalid and old components (including state) are destroyed and removed from the DOM

Tree might be rebuilt if children stated the sames as the state is reset

### Same position, Same element
Element will be kep as well as child elements INCLUDING state

Here, new props/attributes are passed if they are changed between renders

If this is something you don't want, `key prop` can be used

### Key Prop
Special prop to tell the diffing algo that an element is unique. Allows reacts to distinguish between multiple instances of same component type

List without keys -> Same elements but different position. So they are removed and recreated (bad for performance).     

When you give a changing key, upon change, it initiates re-renders which can be the desired behaviour in some cases

## Two type of logic in react components
### Render logic
- Code that lives at the top level of the component function (such as state definitions)
- Participates in describing how the component view looks like
- Executed every time the component renders
### Event handler functions 
- Executed as consequence of the event that the handler is listening for
- Code that actually DOES things such as `onChange`, `onClick` etc.

### Functional Programming Principles
#### Side Effects
Dependency on or modification of any data outside the function scope. "Interaction with the outside world". Mutating external requests, writing to DOM or HTTP requests
    - Not necessarily bad
#### Pure function
A function without side effects such as a computeArea function computing area ONLY based on given input. Same input -> Same Output

## Rules for render logic
- Components must be pure when it comes to render logic given the same props.
- Render logic MUST product no side effects
    - Dont perform network requests or API calls
    - Dont start timers
    - Dont directly use DOM API
    - Dont mutuate objects outside the function scope (why props are not meant to be mutated)
    - Dont update state or referals as it will create an infinite loop
> Side effects are allowed (and encouraged) in event handler functions. There is a special HOOK to register side effects (useEffect)

## State updates are batched
Multiple pieces of state in an event handler are batched and then only render + commit is done. This reduces render wastage.

State is stored in the fiber tree during render phase. If a console.log is done in between some setStates, the ans variable will hold the current state value. Thus updating state in react is async. This is true regardless of the number of states. 

If we need to update state based on previous update, we use setState with callback.

React 18+ introduced batching for timeouts, promises and native events such as addEventListener

```
You can opt out of automatic batching by wrapping a state update in ReactDOM.flushSync()

setState()
setState()
setState()
This wont work because of batching

setState((old) => old + 1)
setState((old) => old + 1)
setState((old) => old + 1)
This will work because a callback function ALWAYS uses the latest value
```
#### if old.state === new.state, then NO render

# DOM Refresher
## Event Propagation and Delegation
Consider a DOM tree with some elements. An onClick event is fired on a button.

The event is created @root and then travels to the target event. (Capture Phase)

Place an EventHandler function to the event (Target Element)

Then it goes to the root ONCE again in the bubbling phase

By default, event handlers listen to events on the target during the bubbling phase

Bubbling can be prevented with e.stopPropagation

For 2 elements having same handler, its fired when the click happens which can be stopped by the method above

## Event Delegation
- Handling events for multiple elements centrally in one single parent element.
- Better for performance and memory, as it needs only one handler function

1. Add handler to parent (.options)
2. Check for target element (e.target)
3. If target is one of the `<button>s`, handle the event

- This is common in vanilla JS apps but not ReactJS

## How react handles events
All the eventHandlers are attached to ROOT dom container where all the events are handled. Pooling of handlers essentially

Behind the scenes, event delegation happens to ROOT 

## Synthetic Events
They are a wrapper over the native event objects. Have the same interface as native event objects like stopPropagation() and preventDefault()

Fixes browser inconsistencies

Most of these bubble (including focus, blur, change etc.) except for `scroll`

- Default behavior cannot be prevented by returning false (only by using preventDefault)

#
## Library are like separate ingedients
## Framework are like AIO kits
| Framework                                                                               | Library                                                                            |
|-----------------------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| Everything you need to build is included in the framework such as routing, styling etc. | You can choose multiple 3rd party libraries to build a complete app                |
|                                                                                         |                                                                                    |
| You are stuck with the framwork's tools and conventions                                 | You need to research, download and stay up to day with multiple external libraries |

# React's 3rd Party Library Ecosystem
## Routing 
**React Router**, React Location
## HTTP Requests
**JS Fetch()**, Axios
## Remote State Management
**React Query**, SWR, Apolllo
## Global State Management
**Context API**, **Redux**, Zustland
## Styling
**CSS Modules**, **Styled Components**, **Tailwind CSS**
## Form Management
**React Hook Form**, Formix
## Animations/Transitions
Motion, React Spring
## UI Components
Material UI, Chakra UI, Mantine

## Frameworks on top of react
NextJS, Remix, Gatsby

- React framworks offer many other features such as server side rendering, static site generation and better developer experience

Some are `Full Stack Frameworks`

<div align="center">
L139 to be refered for full summary in the Udemy course
</div>