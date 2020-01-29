# A React Counter Example

## How to

In previous excersises we have been manually updating the DOM with the subscribe method, but this doesn't scale well.

add these two cdn links to the html head.

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/react/16.12.0/umd/react.development.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/react-dom/16.11.0/cjs/react-dom.development.js"></script>
```

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width" />
    <title>JS Bin</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/redux/4.0.5/redux.js"></script>
    <script src="https://fb.me/react-0.14.0.js"></script>
    <script src="https://fb.me/react-dom-0.14.0.js"></script>
  </head>
  <body>
    <div id="root"></div>
  </body>
</html>
```

Next in the JS code let's state rendering using ReactDOM

```javascript
const counter = (state = 0, action) => {
  switch (action.type) {
    case "INCREMENT":
      return state + 1;
    case "DECREMENT":
      return state - 1;
    default:
      return state;
  }
};

const { createStore } = Redux;
const store = createStore(counter);

const Counter = ({ value, onIncrement, onDecrement }) => (
  <div>
    <h1>{value}</h1>
    <button onClick={onIncrement}>+</button>
    <button onClick={onDecrement}>-</button>
  </div>
);

const render = () => {
  ReactDOM.render(
    <Counter
      value={store.getState()}
      onIncrement={() =>
        store.dispatch({
          type: "INCREMENT"
        })
      }
      onDecrement={() =>
        store.dispatch({
          type: "DECREMENT"
        })
      }
    />,
    document.getElementById("root")
  );
};
```
