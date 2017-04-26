# Applications

To create applications use the [app](/docs/api#app) function.

```jsx
app({
  view: () => <h1>Hi.</h1>
})
```

The app function renders the given view and appends it to [document.body](https://developer.mozilla.org/en-US/docs/Web/API/Document/body).

To mount the application on a different element, use the [root](/docs/api.md#root) property.

```jsx
app({
  view: () => <h1>Hi.</h1>,
  root: document.getElementById("app")
})
```

## View and State

The [view](/docs/api.md#view) is a function of the state. It is called every time the state is modified to reconstruct the application's [virtual node](/docs/virtual-nodes.md) tree which is used to update the DOM.

```jsx
app({
  state: "Hi.",
  view: state => <h1>{state}</h1>
})
```

Use the [state](/docs/api.md#state) to describe your application's data model.

```jsx
app({
  state: ["Hi", "Hola", "こんにちは"],
  view: state =>
    <ul>
      {state.map(hello => <li>{hello}</li>)}
    </ul>
})
```

## Actions

[Actions](/docs/api.md#actions) are the primary way you can modify the state.

```jsx
app({
  state: "Hi.",
  view: (state, actions) =>
    <h1 onclick={actions.upcase}>{state}</h1>,
  actions: {
    upcase: state => state.toUpperCase()
  }
})
```

To modify the state, an action can return a new state or a part of it.

```jsx
app({
  state: 0,
  view: (state, actions) =>
    <main>
      <h1>{state}</h1>
      <button onclick={actions.addOne}>+1</button>
    </main>,
  actions: {
    addOne: state => state + 1
  }
})
```

You can pass data to actions as well.

```jsx
app({
  state: 0,
  view: (state, actions) => (
    <main>
      <h1>{state}</h1>
      <button
        onclick={() => actions.addSome(1))}>More
      </button>
    </main>
  ),
  actions: {
    addSome: (state, actions, data = 0) => state + data
  }
})
```

Actions are not required to have a return value. You can use them to call other actions, for example after an asynchronous operation has completed.

```jsx
app({
  state: 0,
  view: (state, actions) =>
    <main>
      <h1>{state}</h1>
      <button onclick={actions.addOneDelayed}></button>
    </main>,
  actions: {
    addOne: state => state + 1,
    addOneDelayed: (state, actions) =>
      setTimeout(actions.addOne, 1000)
  }
})
```

An action may return a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise). This enables you to use [async functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function).

```jsx
const delay = seconds =>
  new Promise(done => setTimeout(done, seconds * 1000))

app({
  state: 0,
  view: (state, actions) =>
    <main>
      <h1>{state}</h1>
      <button onclick={actions.addOneDelayed}>+1</button>
    </main>,
  actions: {
    addOne: state => state + 1,
    addOneDelayed: async (state, actions) => {
      await delay(1)
      actions.addOne()
    }
  }
})
```

### Namespaces

Namespaces let you organize actions under categories and help reduce name collisions as your application grows large.

```jsx
app({
  state: 0,
  view: (state, actions) =>
    <main>
      <button onclick={actions.counter.add}>+</button>
      <h1>{state}</h1>
      <button onclick={actions.counter.sub}>-</button>
    </main>,
  actions: {
    counter: {
      add: state => state + 1,
      sub: state => state - 1
    }
  }
})
```

## Events

[Events](/docs/api.md#events) are functions called to notify your application that something interesting has happened.

```jsx
app({
  state: { x: 0, y: 0 },
  view: state => <h1>{state.x + ", " + state.y}</h1>,
  actions: {
    move: (state, { x, y }) => ({ x, y })
  },
  events: {
    loaded: (state, actions) =>
      addEventListener("mousemove", e =>
        actions.move({
          x: e.clientX,
          y: e.clientY
        })
      )
  }
})
```

Events can be used to hook into the update and render pipeline.

```jsx
app({
  view: state => <h1>Hi.</h1>,
  events: {
    render: (state, ations, data) => {
      if (location.pathname === "/warp") {
        return state => <h1>Welcome to warp zone!</h1>
      }
    }
  }
})
```

For a practical example see the implementation of the [[[[Router]]]].

### Custom Events

To create custom events, use the [emit](/docs/api.md#emit) function which is passed as the last argument to actions and events.

```jsx
app({
  view: (state, actions) =>
    <button onclick={actions.fail}>Fail</button>,
  actions: {
    fail: (state, actions, data, emit) =>
      emit("error", "Fail")
  },
  events: {
    error: (state, actions, error) => {
      throw error
    }
  }
})
```

## Plugins

Plugins allow you to reuse bits of functionality across your application or multiple applications.

```jsx
const Logger = () => ({
  events: {
    action: (state, actions, data) => console.log(data),
  }
})

app({
  state: 0,
  view: (state, actions) =>
    <main>
      <h1>{state}</h1>
      <button onclick={actions.addOne}>+1</button>
    </main>,
  actions: {
    addOne: state => state + 1
  },
  plugins: [Logger]
})
```


