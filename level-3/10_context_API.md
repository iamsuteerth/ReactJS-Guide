# Task

### Passing state into multiple deeply nested child components

#### Solution 1 : Passing Props

#### Solution 2 : Context API

Consider this

App -> Main -> CP1 -> CP2

This requires 3 stages of prop drilling but contextAPI lets you pass the prop directly to CP2

## Context API

- System to pass data throughout the app without manually passing props down the tree
- Allows us to broadcast state to the entire app
  - Provider : Gives all child components access to value
  - Value : Data that we want to make available
  - Consumers : All components that read the provided context value

`Value update` -> `All consumers re-render`

Refer to <Placeholder> to understand usage of contextAPI

One big advantage is that it increases reusability.

Say a component depends on a prop through prop drilling, it will then require prop drilling again to use that prop which increases code complexity.

### NOTE

Context provides values ONLY to its children so its undefined in a parent component.

#### General Procedure

1. Create context provider component
2. Create custom hook to provide context

## Remote state vs UI State

| Remote State                                           | UI State                                      |
| ------------------------------------------------------ | --------------------------------------------- |
| All application data loaded from a remote server (API) | Everything else                               |
| Usually asynchronous                                   | Theme, list filters, form data                |
| Needs re-fetching + updating                           | Usually synchronous and stored in application |

## State placement options
| Where to place state? | Tools                             | When to use?                        |
|-----------------------|-----------------------------------|-------------------------------------|
| Local Component       | useState, useReducer, useRef      | Local State                         |
| Parent Component      | useState, useReducer, useRef      | Lifting up state                    |
| Context               | Context API + useState/useReducer | Global State(Preferably UI State)   |
| 3rd Party Libraries   | Redux, React Query etc.           | Global State(Remote / UI)           |
| URL                   | React Router                      | Global State, passing between pages |
| Browser               | Local storage, Local Session      | Storing data in user's browser      |

## State Management Tool Options
|              | Local State                                                              | Global State                                                                                                                        |
|--------------|--------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| **UI State**     | useState, useReducer, useRef                                             | Context API + useState/useReducer or Redux, React Router                                                                            |
| **Remote State** | fetch + useEffect + useState/useReducer (*Mostly in small applications*) | Context API + useState/useReducer or Redux. Tools highly specialized in handling remote states can also be used such as React Query |

The full benefit of useReducer is not seen when we have a lot of async code. As in we cant just pass on dispatch functions

You may pass the dispatch function into components and create the actions in the components