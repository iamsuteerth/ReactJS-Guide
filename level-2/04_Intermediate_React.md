# Some resources

| Resource Name                                                                | Link                                                                                                                                              | Author                           |
| ---------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------- |
| Writing Resilient Components                                                 | <a href ="https://overreacted.io/writing-resilient-components/?ref=jonas.io">LINK 1</a>                                                           | Dan Abramov from the React team  |
| Things I think about when I write React code                                 | <a href ="https://github.com/mithi/react-philosophies?ref=jonas.io">LINK 2</a>                                                                    |                                  |
|  A (Mostly) Complete Guide to React Rendering Behavior                       | <a href ="https://blog.isquaredsoftware.com/2020/05/blogged-answers-a-mostly-complete-guide-to-react-rendering-behavior/?ref=jonas.io">LINK 3</a> | Mark Erikson from the redux team |
|  A Visual Guide to React Rendering                                           | <a href ="https://alexsidorenko.com/blog/react-render-always-rerenders/?ref=jonas.io">LINK 4</a>                                                  |                                  |
| Inside Fiber: in-depth overview of the new reconciliation algorithm in React | <a href ="https://indepth.dev/posts/1008/inside-fiber-in-depth-overview-of-the-new-reconciliation-algorithm-in-react?ref=jonas.io">LINK 5</a>     |                                  |
| A Cartoon Intro to Fiber                                                     | <a href ="https://www.youtube.com/watch?v=ZCuYPiUIONs?ref=jonas.io">LINK 6</a>                                                                    | Youtube Video                    |
| What Is React Fiber? React.js Deep Dive                                      | <a href ="https://www.youtube.com/watch?v=0ympFIwQFJw?ref=jonas.io">LINK 7</a>                                                                    | Youtube Video                    |
| The React and React Native Event System Explained                            | <a href ="https://levelup.gitconnected.com/how-exactly-does-react-handles-events-71e8b5e359f2?ref=jonas.io">LINK 8</a>                            |                                  |
| Under the hood of event listeners in React                                   | <a href ="https://gist.github.com/romain-trotard/76313af8170809970daa7ff9d87b0dd5?ref=jonas.io">LINK 9</a>                                        |                                  |
| A DIY guide to build your own React                                          | <a href ="https://github.com/pomber/didact?ref=jonas.io">LINK 10</a>                                                                              |                                  |
| useSyncExternalStore First Look                                              | <a href ="https://julesblom.com/writing/usesyncexternalstore?ref=jonas.io">LINK 11</a>                                                            |                                  |
| Under the hood of React's hooks system                                       | <a href ="https://the-guild.dev/blog/react-hooks-system?ref=jonas.io">LINK 12</a>                                                                 |                                  |
| Why Do React Hooks Rely on Call Order?                                       | <a href ="https://overreacted.io/why-do-hooks-rely-on-call-order/?ref=jonas.io">LINK 13</a>                                                       | Dan Abramov from the React team  |
| So you think you know everything about React refs                            | <a href ="https://blog.thoughtspile.tech/2021/05/17/everything-about-react-refs/?ref=jonas.io">LINK 14</a>                                        |                                  |
| react-use: Reusable React Hook Library                                       | <a href ="https://github.com/streamich/react-use?ref=jonas.io">LINK 15</a>                                                                        | Github                           |
| react-hookz: React hooks done right                                          | <a href ="https://github.com/react-hookz/web?ref=jonas.io">LINK 16</a>                                                                            | Github                           |

#

Refer to this project <a href="https://github.com/iamsuteerth/Basic-ReactJS-Projects/tree/main/05_use_popcorn_lv2">Use Popcorn (05)</a> to understand this section

# Back to thinking in react

`How to split a UI into components?`

### Component size matters ; )

They can be VERY small OR VERY BIG

When is a component too BIG?

- Too many responsibilities
- Might need TOO many props
- Hard to reuse
- Complex code, hard to understand

When is a component TOO small?

- 100s of components
- Confusing codebase
- TOO abstracted

Balance is important

## How to split a UI into components

- Logical separation
- Some components can be reused
- Low complexity

### Criteria

- Logical separation of content
- Reusability
- Responsiblities/Complexity
- Personal preference

`Start with a big component and break it into smaller ones as it becomes necessary`
You may skip this if you NEED to REUSE (such as a button). Don't focus on reusability anc complexity early on

## You need a new component :

### Logical separation

Does the component contain pieces of unrelated stuff?

### Reusability

Is it possible to reuse part of component? Do you NEED to reuse it?

### Responsiblities/Complexity

- Is the component doing too many different things?
- Does the component rely on too many props?
- Does the component have too many piece of state
- Is the code too complex?

### Personal Prefernce

Do you prefer smaller components?

## General Guidelines

- Be aware that creating a new component creates a new abstraction as they have a cost, because more abstractions require more mental energy to navigate. Dont create too many components TOO early
- Name a component appropriately
- Co relate related components in same file
- NEVER delcare component inside component
- Some components can BE left small as they are meant to be reused but SOME MIGHT NOT

## Component Categories

### Most of your components will fall into these categories

> Stateless / Presentational (like logo)
>
> > No State
>
> > Can receive props and simply present recieved data or other content

> Stateful (Like the movie list or search component)
>
> > Have state
>
> > Can still be reusable

> Structural
>
> > Pages, Layouts or Screens of the app
>
> > Result of composition
>
> > Can be huge (Airbnb map) and non reusable but thats not necessary

## Prop Drilling

When we have to pass props through a lot of components where they don't even NEED that prop.

## Component Composition

| Using a component                                             | Component Composition                                              |
| ------------------------------------------------------------- | ------------------------------------------------------------------ |
| Component inside component as they are deeply linked together | No predefined component but uses the children prop so we can REUSE |
| Apple lightning (tied to an eco system)                       | USB type A (Universal)                                             |

We do this for:

- Create highly reusable and flexible components
- Fix prop drilling
- Great for layouts

As components don't need to know about their children in advanced!

## How can we fix prop drilling?

A simple example

We have to pass movies prop like this : App -> NavBar -> NumResults

We can instead:

```jsx
<NavBar>
  <Logo />
  <Search />
  <NumResults movies={movies} />
</NavBar>
```

### A much better way of creating layouts

```jsx
export default function App() {
  const [movies, setMovies] = useState(tempMovieData);
  return (
    <>
      <NavBar>
        <NumResults movies={movies} />
      </NavBar>
      <Main>
        <ListBox>
          <MovieList movies={movies} />
        </ListBox>
        <WatchedBox />
      </Main>
    </>
  );
}
```

We repurposed ListBox as Box and used it for both Box and WatchedBox with different children

Using onClick, onMouseEnter and onMouseLeave, we get a really good real life rating funtionality

Here's the snippet

```jsx
// In the StarRating component
<Star
  key={i}
  onHandleRating={() => handleRating(i + 1)}
  full={tempRating ? tempRating >= i + 1 : rating >= i + 1}
  onHoverIn={() => setTempRating(i + 1)}
  onHoverOut={() => setTempRating(0)}
/>
// Individual star
function Star({ onHandleRating, full, onHoverIn, onHoverOut }) {
  return (
    <span
      role="button"
      style={starSyle}
      onClick={onHandleRating}
      onMouseEnter={onHoverIn}
      onMouseLeave={onHoverOut}
    >
      {full ? (Star Full) : (Star Empty);}
    </span>
  );
}
```
We should think carefully about the props our `reusable component` needs

## Props as an API
How many props should we enable for configuration?
- Too little? (Not flexible enough)
- Too many? (Too much of complexity and hard to write)
- Provide good default values

```jsx
// Considering a scenario where the state needs to be used 
function Test(){
  const [movieRating, setMovieRating] = useState(0);
  return (
    <div>
      <StarRating color="blue" maxRating={10} onSetRating={setMovieRating}/>
      <p>This movie was rated {movieRating}</p>
    </div>
  );
}
```
We can then accept this set function in the prop we are using

## Prop types and type-checking
The best option here is to use typescript instead of prop types

```jsx
StarRating.propTypes = {
  maxRating: PropTypes.number,
  defaultRating:PropTypes.number,
  color:PropTypes.string,
  size:PropTypes.number,
  messages:PropTypes.array,
  onSetRating:PropTypes.func
}
```