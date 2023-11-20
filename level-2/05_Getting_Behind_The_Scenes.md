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