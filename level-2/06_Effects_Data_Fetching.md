# Component Instance Lifecycle
### Mount/Initial Render
- Rendered for the first time
- Fresh state and props are created
### Re render(can happen multiple times)
- State change
- Props change
- Parent re renders
- Context changes
### Unmount
- Component instance destroyed and removed
- States and props are destroed
## How NOT to fetch data?
```js
function Component(){
    const [state, setState] = useState([]);
    fetch(uri).then(res => res.json()).then(data => setState(data.search));
    return ();
}
```
Here, we will get into an infinite loop as a rebuild will keep on getting triggered as the component runs setState again and again in the init area where `we are not supposed to have side effects`

Refer to <a href='https://github.com/iamsuteerth/Basic-ReactJS-Projects/tree/main/05_use_popcorn_lv2'>Use Popcorn Project</a> for detailed explanation of this part

`The commented code has the code relevant to this section`

- Side effects can be made in `Event Handlers` and `Effects(useEffect)`

| Event Handers                                  | Effects(useEffect)                                                                   |
|------------------------------------------------|--------------------------------------------------------------------------------------|
| Executed when the correspoinding event happens | Executedafter the component mounts and after subsequent re-renders                   |
| NA                                             | Effects can return cleanup functions which are executed before unmount or re renders |
| Used to react to an event                      | Used to keep a component in sync with some external system                           |

```js
// One way to use effect
useEffect(function () {
    fetch(`${baseURI}&s=interstellar`)
      .then((res) => res.json())
      .then((data) => setMovies(data.Search));
  }, []);
```
`Effect callbacks are synchronous to prevent race conditions`

Setting states is asynchronous so logging values immediately after setting state MAY result in `stale state`

## Dependency Array
- By default, effects run after every renderr. It can be prevented by using dependency arrays.
- Without this, react doesn't know when to run the effect
- Each time one of the dependencies change, effect is executed again.
- Every `state variable` and `prop` must be included in the dependency array. Otherwise, we will get a stale closure

### useEffect is a synchronization mechanism
It is like an event listener that is listening for one dependency to change. The effect is executed if depenencies change

Therefore, effects are relative

`Components` -> **synchronize with** -> `external system (side effect)`

- Effects and component lifecycle are deeply connected

### Types of dependency array
#### [x,y,z]
- Effect syncs with x,y,z 
- Runs on mount and re renders upon change in x,y,z
#### []
- No sync with no state and props
- Runs only on mount 
#### NA
- syncs with everything
- runs on every render ⛔

## When are effects executed
1. Mount
2. Commit
3. Browser Paint
4. EFFECT (hence async)
- If an effect sets state, an additional re render will be required, so dont use effects too much
5. Re render
6. Commit
- Layout effect
7. Browser Paint
- Mystery Step 1*
8. Effect
9. Unmount
- Mystery Step 2*

## Small Experiment
```js
useEffect(function () { // SECOND
    console.log("A");
  }, []);
  useEffect(function () { // THIRD
    console.log("B");
  });
  console.log("C"); // FIRST as it is a render logic 
```

## Mystery Step 1,2*
- Function that we can return from an effect
- It is optional
- Runs on 2 scenarios
  - Before the effect is executed again
  - After the componenthas unmounted
- We need a cleanup function when the side effect KEEPS happening after the component has re rendered or unmounted.

### Some examples
| Effect             | Potential Cleanup     |
|--------------------|-----------------------|
| HTTP Request       | Cancel Request        |
| API Subscription   | Cancel Subscription   |
| Start timer        | Stop timer            |
| Add event listener | Remove event listener |

#### Each effect should ONE thing. Makes them easier to understand as well as cleanup.

### How to define cleanup function
```js
useEffect(
  function () {
    if (!title) return;
    document.title = `Movie | ${title}`;
    // How to define cleanup function
    return function () {
      document.title = "Use Popcorn";
    };
  },
  [title]
);
```
The function will remember all the variables at the time it was called with (such as the title variable in this case) because of something called `javascript closure`.

## Race condition
When multiple HTTP requests are called simultaneously raciing to be the `last one to survive`.

## How to respond to a keypress event globally
This can be done by attaching an EventListener to the entire document.

The useEffect is called an escape hatch because we are can do things the `non react way` by using this.

### An example
```js
useEffect(
  function () {
    function listenerCallBack(e) {
      // An event listener is attached EVERY single time this event is mounted
      if (e.code === "Escape") {
        onCloseMovie();
      }
    }
    document.addEventListener("keydown", listenerCallBack);
    return function () {
      document.removeEventListener("keydown", listenerCallBack);
    };
  },
  [onCloseMovie]
);
```