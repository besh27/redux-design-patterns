# Redux createStore

## createStore from scratch
To better understand our tools, it is good to build them from scratch. 

```javascript
const createStore = (render) => {
    let state;
    let listeners = [];

    const getState = () => state;

    const dispatch = (action) => {
        state = reducer(state, action);
        listeners.forEach(listener => listener())
    }

    const subscribe = (listener) => {
        listeners.push(listener);
        return () => {
            listeners = listeners.filter(l => l !== listener);
        }
    }

    return { getState, dispatch, subscribe };
}
```
The returned value of the state, dispatch fn and subscribe fn is what we call the store. 
