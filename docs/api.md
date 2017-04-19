# API

* [h](#h)
* [app](#app)
  * [state](#state)
  * [view](#view)
  * [actions](#actions)
    * [[_namespace._]action](#actions-action)
  * [events](#events)
    * [loaded](#events-loaded)
    * [action](#events-action)
    * [update](#events-update)
    * [render](#events-render)
  * [plugins](#plugins)
    * [Plugin](#plugins-plugin)
  * [root](#root)
* [emit](#emit)

## <a name="h"></a> h

Type: <samp>([tag](#h-tag), [data](#h-data), [children](#h-children)): [vnode](#vnode)</samp>

* <a name="h-tag"></a>**tag**: a string, e.g. <samp>div</samp> or a [custom tag](/docs/applications.md#custom-tags) function of the type: <samp>([props](#h-data), [children](#h-children)): [vnode](#vnode)</samp>
* <a name="h-data"></a>**data**: an object with [attributes](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes), [styles](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference), [events](https://developer.mozilla.org/en-US/docs/Web/API/GlobalEventHandlers), [lifecycle events](/docs/lifecycle-events.md), etc.
* <a name="h-children"></a>**children**: a string or an array of virtual nodes.
* <a name="vnode"></a>**vnode**: the [virtual node](/docs/virtual-nodes.md).

## <a name="app"></a> app

Type: <samp>([props](#app-props))</samp>

```jsx
app({
  state,
  view,
  actions,
  events,
  plugins,
  root
})
```

<a name="app-props"></a>

### <a name="state"></a> state

Type: <samp>Any</samp>

### <a name="view"></a> view

Type: <samp>([state](#state), [actions](#actions)): [vnode](#vnode)</samp>

### <a name="actions"></a> actions
#### <a name="actions-action"></a> [_namespace._]action

Type: <samp>([state](#state), [actions](#actions), [data](#actions-data), [emit](#emit))</samp>

* <a name="actions-data"></a> **data**: the data passed to the action.

```jsx
actions: {
  foo: {
    bar: (state, actions, data, emit) => {}
  }
}

actions.foo.bar(data)
```

### <a name="events"></a> events
#### <a name="events-loaded"></a> loaded

Type: <samp>([state](#state), [actions](#actions), _, [emit](#emit)) | Array\<[Type](#events-loaded)\></samp>

Called after the view is mounted on the DOM.

#### <a name="events-action"></a> action

Type: <samp>([state](#state), [actions](#actions), [data](#events-action-data), [emit](#emit)): [data](#events-action-data) | Array\<[Type](#events-action)\></samp>

<a name="events-action-data"></a>

* **data**
  * **name**: the name of the action.
  * **data**: the data passed to the action.

Called before an action is triggered.

#### <a name="events-update"></a> update

Type: <samp>([state](#state), [actions](#actions), [data](#events-update-data), [emit](#emit)): [data](#events-update-data) | Array\<[Type](#events-update)\></samp>

* <a name="events-update-data"></a>**data**: the updated fragment of the state.

Called before the state is updated.

#### <a name="events-render"></a> render

Type: <samp>([state](#state), [actions](#actions), [data](#events-render-data), [emit](#emit)): [data](#events-render-data) | Array\<[Type](#events-render)\></samp>

* <a name="events-render-data"></a>**data**: the [view](#view) function.

Called before the view is rendered.

### <a name="plugins"></a> plugins

Type: <samp>Array\<[Plugin](#plugins-plugin)\></samp>

#### <a name="plugins-plugin"></a> Plugin

Type: <samp>([props](#app-props)): [props](#plugin-props)</samp>

* <a name="plugin-props"></a> **props**
  * [state](#state)
  * [actions](#actions)
  * [events](#events)

### <a name="root"></a> root

Type: <samp>[Element](https://developer.mozilla.org/en-US/docs/Web/API/Element) = [document.body](https://developer.mozilla.org/en-US/docs/Web/API/Document/body)</samp>

## <a name="emit"></a> emit

Type: <samp>([event](#emit-event), [data](#emit-data)): [data](#emit-data)</samp>

* <a name="emit-event"></a> **event**: the name of the event.
* <a name="emit-data"></a> **data**: the data passed to the event.
