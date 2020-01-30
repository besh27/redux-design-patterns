# Combine Reducers

The practice of combining reducers is so prevalent in redux that there is a method built in to the library; ```combineReducers()```


```javascript
const { combineReducers } = Redux;
const todoApps = combineReducers({
    todos: todos, 
    visibilityFilter: visibilityFilter
});
```
In this exampe, we are using the ```combineReducers``` method to combine the ```todos``` & ```visibilityFilter``` reducers. We do this by providing the property that will be added to the store, which is stated at the left of the colon. Then we specify the reducer that will be handling that property's state, which is to the right of the colon. 

It is a convension to name state object after the reducer in the example above. By doing this we can use the es6 object literal shorthand notation approach and combine the property/value. See Below. 
```javascript
const { combineReducers } = Redux;
const todoApps = combineReducers({
    todos, 
    visibilityFilter
});
```

## What is going on under the hood?

Let's build a combineReducers function from scratch to get a better understanding of what is happeneing. 

- it is a function that takes in the reducers as an argument. 
- The return value is another reducer, thus it's a function that takes in a function and returns one as well. 
- The signature of the returned function is a reducer signature (state, action)
- Next we use the Object.key method to gather all the reducer keys.
- Next, we reduce over the keys to produce a single value( the nextState)
- The reducer will reduce over all the keys and handle each property according to the current state and the action. 
- In the end, the nextState is returned. 
  
```javascript
const combineReducers = (reducers) => {
    return (state = {}, action) => {
        return Object.keys(reducers).reduce(
            (nextState, key) => {
                nextState[key] = reducers[key](
                    state[key],
                    action
                );
                return nextState;
            },
            {}
        )
    };
};
```

## Next Steps
- Move on to [React Redux Todo List](./12-react-redux-todo-list.md)