# React Hooks
- Special built in functions that allow u to hook into react internals
    - Creating and accessing state from Fiber tree
    - Registering side effects in fiber tree
    - Manual DOM selections
- Always start with use
- Enable easy reusing of non visual logic : We can compose multiple hooks into our own custom hooks
- Give function components the ability to own state and run side effects at different lifecycle poitns

## Most used hooks
- useState
- useEffect
- useReducer
- useContext
## Less used hooks
- useRef
- useCallback
- useMemo
- useTransition
- useDeferredvalue

# Rules of hooks
1. Only call these at top level
    - Don't call hooks inside conditionals, loops, nested functions or after an early return
2. Only call hooks from function components or custom hooks

`Autmatically enforced by ESLint`

Refer to `PLACEBO` for explanation

DONOT use `useState` just like that because react will call it in ALL renders. Always pass in a function which react can call later.

This is called lazy evaluation when we create state using a function.

The callback functions while setting a state **MUST BE PURE**

*Never mutate objects, but create new objects (like by using spread operator)*

## useRef hook (one of the easier hooks)
Used for creating reference
- "box" with a mutable `.current` property that is persisted across renders wheras normal variables are always resetted

**Use Cases**
- Creating a variable that stays the same between renders such as previous state
- Selecting and storing DOM elements
- Refs are for data that is NOT rendered (only appear in eventHandlers or effects)
- Do not read write or read .current in render logic (like state)

|      | Persists Across Renders | Updating causes re-render | Immutable  | Async Updates
|-------|-------------------------|---------------------------|---------------------------|---------------------------|
| **State** | ✅                       | ✅                         | ✅                         | ✅                         |
| **Ref**   | ✅                       | ❌                         | ❌                         | ❌                         |

`A customHook needs to use a react hook, otherwise it is just a regular function`

Refer to <a href='https://github.com/iamsuteerth/Basic-ReactJS-Projects/tree/main/05_use_popcorn_lv2'>Use Popcorn Project</a> to see the hooks in place.

Refer to <a href='https://github.com/iamsuteerth/Basic-ReactJS-Projects/tree/main/05_use_popcorn_lv2_vanilla'>Use Popcorn Project Vanilla</a> for lean code.

|                       | Function Components                                                                                                                                                    | Class Componenets                       |
|------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------|
| Introduced             | v16.8 (2019)                                                                                                                                                           | v0.13 (2015)                            |
| How to create          | JS function                                                                                                                                                            | ES6 class extending React.Component     |
| Reading props          | Destructuring or props.X                                                                                                                                               | this.props.X                            |
| Local State            | useState hook                                                                                                                                                          | this.setState()                         |
| Side Effects/Lifecycle | useEffect hook                                                                                                                                                         | Lifecycle Methods                       |
| Event Handlers         | Functions                                                                                                                                                              | Class methods                           |
| Returning JSX          | Return JSX from function                                                                                                                                               | Return JSX from render method           |
| Advantages             | 1. Easier to build <br> 2. Cleaner Code as useEffect combines ALL lifecycle related code in a single place <br> 3. Easier to share stateful login <br> 4. No need to use THIS keyword | Lifecycle might be easier for beginners |

```js
// Example
import React from "react";

class Counter extends React.Component {
  constructor(props) {
    super(props);
    this.state = { count: 5 };
    this.handleDecrement = this.handleDecrement.bind(this);
    this.handleIncrement = this.handleIncrement.bind(this);
  }
  handleDecrement() {
    // this here is undefined because upon call, a copy is called
    this.setState((currentState) => {
      return { count: currentState.count - 1 };
    });
  }
  handleIncrement() {
    this.setState((currentState) => {
      return { count: currentState.count + 1 };
    });
  }
  render() {
    const date = new Date('June 21 2027');
    date.setDate(date.getDate() + this.state.count);
    return (
      <div>
        <button onClick={this.handleDecrement}>-</button>
        <span>{date.toDateString()}</span>
        <span>[{this.state.count}]</span>
        <button onClick={this.handleIncrement}>+</button>
      </div>
    );
  }
}

export default Counter;
```
Refer to <a href='https://github.com/iamsuteerth/Basic-ReactJS-Projects/tree/main/06_classy_weather'>Classy Weather App</a> for a project built using class components.