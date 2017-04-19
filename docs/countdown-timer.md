# Countdown Timer

In this section we'll walk through a countdown timer example.

[View Online](https://codepen.io/hyperapp/pen/xdRjrL?editors=0010)

```jsx
const pad = n => (n < 10 ? "0" + n : n)

const humanizeTime = t => {
  const hours = (t / 3600) >> 0
  const minutes = ((t - hours * 3600) / 60) >> 0
  const seconds = (t - hours * 3600 - minutes * 60) >> 0
  return `${hours} : ${pad(minutes)} : ${pad(seconds)}`
}

const SECONDS = 5

app({
  state: {
    count: SECONDS,
    paused: true
  },
  view: (state, actions) => (
    <main>
      <h1>{humanizeTime(state.count)}</h1>

      <button onclick={actions.toggle}>
        {state.paused ? "Start" : "paused"}
      </button>

      <button onclick={actions.reset}>Reset</button>
    </main>
  ),
  actions: {
    toggle: state => ({ paused: !state.paused }),
    reset: state => ({ count: SECONDS }),
    drop: state => ({ count: state.count - 1 }),
    tick: (state, actions) => {
      if (state.count === 0) {
        actions.reset()
        actions.toggle()
      } else if (!state.paused) {
        actions.drop()
      }
    }
  },
  events: {
    loaded: (state, actions) => setInterval(actions.tick, 1000)
  }
})
```

In this application, the state stores two properties: <samp>count</samp>, a Number to track the number of seconds elapsed; and <samp>paused</samp>, a Boolean to check if the clock is running or not.

```jsx
state: {
  count: SECONDS,
  paused: true
}
```

The view displays the current count and binds two buttons to the toggle and reset actions. There's also some logic in <samp>humanizeTime</samp> to display the time in a familiar format <samp>hh:mm:ss</samp>.

The clock is implemented using [<samp>setInterval</samp>](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/setInterval) that calls <samp>actions.tick</samp> every second.

```jsx
events: {
  loaded: (state, actions) => setInterval(actions.tick, 1000)
}
```

Every time <samp>tick</samp> is called, we check the current count to see if it has reached zero, and when it does, reset the counter back to <samp>SECONDS</samp> and toggle the running clock.

```jsx
if (state.count === 0) {
  actions.reset()
  actions.toggle()
} else if (!state.paused) {
  actions.drop()
}
```

If <samp>state.count</samp> is greater than zero and the clock is not paused, we decrement the count by one.

This example demonstrates using <samp>[events.loaded](/docs/api.md#events-loaded)</samp> to register a global event and using an action to call other actions.

