# useReducer
This can be used in place of useState as it offers some additional features

```js
const [count, dispatch] = useReducer(reducer, 0);
function reducer(state, action){
  console.log(state, action);
  return state + action;
}
const inc = function() {
    dispatch(1);
}
// This becomes the action in reducer
// It returns new state based on these
dispatch({ type: "dec", payload: -1 });
// Standard way of working with reducer is to use an object
```
Now this may seem redundant but a big advantage is this.

```js
const initState = { count: 0, step: 1 };

function reducer(state, action) {
  console.log(state, action);
  switch (action.type) {
    case "dec":
      return { ...state, count: state.count - state.step };
    case "inc":
      return { ...state, count: state.count + state.step };
    case "setCount":
      return { ...state, count: action.payload };
    case "setStep":
      return { ...state, step: action.payload };
    case 'reset':
      return initState;
    default:
      return { ...state };
  }
}
const reset = function () {
    dispatch({type : 'reset'});
};
// Being able to reset multiple states at ONCE, essentially "reducing"
// You can then simply replace the handlers with dispatch events and handle all of it in ONE place
// This is the beauty of useReducer
```
# Some theory
## Why useReducer?
- State management with useState is NOT enough in certain situations
    1. When components have **a lot of state variables and state updates,** spread across many event handlers **all over the component**
    2. When **multiple state updates** need to happen **at the same time** (as a reaction to the same event like starting a game)
    3. When updating once piece of state **depends on one or multiple other pieces of state**
*useReducer can be of great help in these situations*

## State with useReducer
- An alternate way of setting state. Ideal for complex state and related piece of state.
- Stores related pieces of state in a state object
- Needs a reducer function containing all logic to update state. Decouples state logic from component
- It must be a pure function without side effects and return next state
- action object describes HOW to update the state
- dispatch function to trigger state updates by sending actions (instead of setState) from event handlers to the reducer

Updating State -> `dispatch` (action) -> `reducer` -returns-> `next state` -> re-render

Just like array.reduce() accumulates all array values, this is where the name comes from.

## Mental model for reducers
`Real world task, withdraw 5000$ from bank account`

User : I would like to withdraw 5000$ from account XYZ 

Operator : Okay, I will search and confirm with vault

(Checks and gives money)

For small amounts like 50$, you can just go to an ATM for this.

This is exactly the analogy for useReducer vs useState

The vault is the state, the customer is the dispatcher who is requesting the state update. The reducer is the bank operator (who makes the update receiving the instructions from dispatcher)

```js
dispatch({
    type : 'withdraw',
    payload : {
        amount : 5000,
        account : 'XYZ',
    },
},);
```
Refer to <> from this point.

`npm install json-server`, we will pretend to fetch questions from an API.
- Add a new npm script to `package.json`

`"server": "json-server --watch data/questions.json --port 8000"`

Followed by

`npm run server`

`http://localhost:8000/questions` will be used to access, reason is given below

```
{
  "questions": [
...
```
The questions endpoint is what we need, so if this was abc, then the url would be http://localhost:8000/abc

### Future Practice Ideas
- Have a checkbox to select difficulty of questions
- Store highscore online fetching it through API

| useState                                                                                            | useReducer                                                                  |
|-----------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------|
| Ideal for single independent pieces of state                                                        | Ideal for multiple related pieces of state and complex state                |
| Logic to update state is placed directly over eventHanlders, spread over one or multiple components | Logic to update state lives in ONE central place decoupled from components. |
| state is updated by calling setState                                                                | State is updated by dispatchings an action to reducer.                      |
| Imperative State updates                                                                            | Declarative state updates                                                   |
| Easier to understand                                                                                | More difficult to understand and implement                                  |