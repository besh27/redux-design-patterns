# Redux Reduce Function

## Redux Reducer
- The entire state stucture is built on a reduce function. 
- It holds the current state, the incoming action and returns the next state. 
- By passing in the action, it doesn't modify the original state, it returns a new one. It is a pure function. 
- this function is called the reducer. 

### Code in JSBin
- Add the expect library cdn link to the html header
```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/expect/1.20.2/expect.js"></script>
```

Next, using the ES6 panel, let's create a counter reducer function and some tests. 
```javascript
function counter(state, action){
    if(typeof state === 'undefined'){
      return 0;
    }
    if(action.type === 'INCREMENT'){
        return state + 1;
    } else if (action.type === 'DECREMENT'){
        return state - 1;
    } else {
      return state;
    }
}

expect(
  counter(0, {type: 'INCREMENT'})
).toEqual(1);

expect(
  counter(1, {type: 'DECREMENT'})
).toEqual(0);

// This test tests if the reducer reaches the return state upon a type that isn't described in the reducer. 
expect(
  counter(0, {type: 'HACKING!'})
).toEqual(0);

// the code won't reach this console if these tests don't pass. 
console.log("Tests Passed!")

```

Next, let's refactor this code to use a switch statement and es6 arrow function. 
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

expect(
  counter(0, {type: 'INCREMENT'})
).toEqual(1);

expect(
  counter(1, {type: 'DECREMENT'})
).toEqual(0);

// This test tests if the reducer reaches the return state upon a type that isn't described in the reducer. 
expect(
  counter(0, {type: 'HACKING!'})
).toEqual(0);

// the code won't reach this console if these tests don't pass. 
console.log("Tests Passed!")
```