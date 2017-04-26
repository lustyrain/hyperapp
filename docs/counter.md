# Counter

In this section we'll walk through a simple counter example.

[View Online](https://codepen.io/hyperapp/pen/GmjLYb?editors=0010)

```jsx
app({
  state: 0,
  view: (state, actions) => (
    <main>
      <h1>{state}</h1>
      <button onclick={actions.add}>+</button>
      <button onclick={actions.sub} disabled={state <= 0}>-</button>
    </main>
  ),
  actions: {
    add: state => state + 1,
    sub: state => state - 1
  }
})
```

Let's start with the state of the application. In this case it's of type Number and its initial value is 0.

```jsx
state: 0
```

When the app function runs for the first time, the state is passed to the view and its value is displayed inside an <samp>\<h1\></samp> tag.

```jsx
<h1>{state}</h1>
```

There are also two buttons in the view that have onclick handlers attached to them. The handlers are the same actions passed to the view in the second argument.

```jsx
<button onclick={actions.add}>+</button>
<button onclick={actions.sub} disabled={state <= 0}>-</button>
```

The disabled property is dynamically set to true or false depending on the value of the counter. This prevents the decrement button from being clicked if the counter reaches zero.

```jsx
disabled={state <= 0}
```

Neither add or sub modify the state directly. Instead, they describe how to calculate the new state.

```jsx
add: state => state + 1,
sub: state => state - 1
```

When an action is called, the current state is updated with a new state and the view function is called again.
