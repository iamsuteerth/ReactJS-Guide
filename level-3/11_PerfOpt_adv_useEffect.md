# Performance Optimization Tools

1. **Prevent wasted renders**

- memo
- useMemo
- useCallback
- Passing elements as children or regular prop

2. **Improve app speed/responsiveness**

- useMemo
- useCallback
- useTransition

3. **Reduce size bundle**

- use fewer 3rd party packages
- code splitting and lazy loading

- Not an exhaustive list

- ## Re render causes
  - State changes
  - Context changes
  - Parent re renders

A wasted render is basically when no change is made in the DOM

This can slow down when re renders occur very frequently or the component is very slow

## Profiler

#### Enable this

- Record why each component rendered while profiling.
- Flamegraph

```js
function SlowComponent() {
  // If this is too slow on your maching, reduce the `length`
  const words = Array.from({ length: 100_000 }, () => "WORD");
  return (
    <ul>
      {words.map((word, i) => (
        <li key={i}>
          {i}: {word}
        </li>
      ))}
    </ul>
  );
}

function Counter({ children }) {
  const [count, setCount] = useState(0);
  return (
    <div>
      <h1>Slow counter?!?</h1>
      <button onClick={() => setCount((c) => c + 1)}>Increase: {count}</button>

      {children}
    </div>
  );
}

export default function Test() {
  // const [count, setCount] = useState(0);
  // return (
  //   <div>
  //     <h1>Slow counter?!?</h1>
  //     <button onClick={() => setCount((c) => c + 1)}>Increase: {count}</button>

  //     <SlowComponent />
  //   </div>
  // );

  return (
    <div>
      <h1>Slow counter?!?</h1>
      <Counter>
        <SlowComponent />
      </Counter>
    </div>
  );
}
```

Now what's happening here is that the slow component is being passed in as a children so it was rendered beforehand.

While rendering, it renders slow component and then passes it to counter after that

The catch is that the children prop has already been made once and is simply passed as a prop which doesn't cause it to re-render

You basically have to pass the component as a prop

# memo

Based on **Memoization**

- Optimization technique that executes a pure function once and saves the result in meomry. If we try to execute the function again with the same arguments as before, the previously saved result is returned without executing the function again

With new inputs, it runs and stores the updated result. Otherwise fetches from cache

_Prevent wasted renders_

_Improve app speed/responsiveness_

#

- Memoize **components** with memo
- Memoize **objects** with useMemo
- Memoize **functions** with useCallback

## memo function

- Used to create a component that will not re-render with its parent re-renders as long as the props stay the same between renders
- If props do change, a re-render is needed
- This method ONLY affects props and the component still re-render when its own state changes or when a context changes to which it is subscribed
- Only makes sense when the component is heavy/re-renders often and does so with the same props

## An issue with memo

- In react, everything is **re-rendered on every render** (including objects and functions)

- In JS, 2 objects or functions that look the same are actually different

- Thus the child component will always consider it as a new prop

- This can be solved using useMemo and useCallback

### useMemo and useCallback

- Used to memoize values (useMemo) and functions (useCallback) between renders
- Values passed into useMemo and useCallback will be stored in the memory ("cached") and returned in subsequent re-renders as long as the dependencies ("inputs") stay the same
- useMemo and useCallback have dependency arrays

#

1. Memoizing props to prevent wasted renders (together with memo)
2. Memoizing values to avoid expensive re-calculations on every render
3. Memoizing values that are used in dependency array of another hook

**_Only optimize using these tools when you DO SEE a significant improvement with memoization_**

**_Setter functions of useState hooks always have a stable identity and thus, they won't change upon re-renders_**

Thus its okay to discard them from dependency arrays (they're sort of memoized automatically)

## When to optimize context?

1. State in the context needs to change all the time
2. The context has many consumers
3. The app is actually slow and laggy

```js
<PostProvider>
  <Header />
  <Main />
  <Archive />
  <Footer />
</PostProvider>
```
This alone is a good optimization

You can also memoize the contents ofthe provider

In some cases where you prvide an object to the post provider. Even if object doesn't change on paper but it changes technically and thus you have to memoize the object using `useMemo`

If you have to add functions to dependency array, then make sure to wrap them with a useCallback

Now if you havea lot of values in a context and even if only ONE changes, then all the subscribers re-render which is ALSO WHY you haev only ONE CONSTANT PER STATE
