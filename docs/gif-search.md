# Gif Search

In this section we'll learn how to implement a gif search using the [Giphy API](https://api.giphy.com/).

[View Online](https://codepen.io/hyperapp/pen/LybmLe?editors=0010)

```jsx
const GIPHY_API_KEY = "dc6zaTOxFJmzC"

app({
  state: {
    url: "",
    isFetching: false
  },
  view: (state, actions) => (
    <div>
      <input
        type="text"
        placeholder="Type to search..."
        onkeyup={actions.search}
      />
      <div>
        <img
          src={state.url}
          style={{
            display: !state.url || state.isFetching ? "none" : "block"
          }}
        />
      </div>
    </div>
  ),
  actions: {
    search: (state, actions, { target }) => {
      const text = target.value

      if (state.isFetching || text === "") {
        return { url: "" }
      }

      actions.toggleFetching()

      fetch(
        `//api.giphy.com/v1/gifs/search?q=${text}&api_key=${GIPHY_API_KEY}`
      )
        .then(data => data.json())
        .then(({ data }) => {
          actions.toggleFetching()
          data[0] && actions.setUrl(data[0].images.original.url)
        })
    },
    setUrl: (state, actions, url) => ({ url }),
    toggleFetching: state => ({ isFetching: !state.isFetching })
  }
})
```
