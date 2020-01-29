# Redux Store Methods

## Practice with Redux in JSBin

Add this cdn link to the head of the html in jsbin.

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/redux/4.0.5/redux.js"></script>
```

in jsbin:

````javascript
const { createStore } = Redux;

/* in a regular application, use
import {createStore} from redux;
*/

## The Store
```javascript
const store = createStore(counter);
````

The store will bind together the three core principles of redux

- One immutable object state tree
- Let's you dispatch actions to it.
- Need to specify the reducer function. In this example counter is the reducer.

## Three important methods

- store.getState()
- store.dispatch()
  - One of the most used because it dispatches the actions to the reducer.
- store.subscribe()
  - registers a callback that can update your UI whenever an action is dispatched.
  - a much more elegant approach compared to `console.log()`

```javascript
store.subscribe(() => {
  document.body.innerText = store.getState();
});


document.addEventListener('click'), () => {
    store.dispatch({ type: 'INCREMENT' })
};
```

let's refactor or JSBin code. 

```javascript
const counter = (state = 0, action) => {
    switch(action.type){
        case 'INCREMENT':
            return state + 1;
        case 'DECREMENT':
            return state - 1;
        default:
        return state;
    }
}

const { createStore } = Redux;
const store = createStore(counter);

const render = () => {
    document.body.innerText = store.getState();
}
store.subscribe(render);
render();


document.addEventListener('click', () => {
    store.dispatch({ type: 'INCREMENT' })
});
```

## Next Steps
- Move on to [createStore](./03-store-methods.md)