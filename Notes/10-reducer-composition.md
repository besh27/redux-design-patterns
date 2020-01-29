# Reducer Composition

We currently have a single reducer that handles the creation of single todos and the updating of individual todos. Evenually it will become apparent (after adding even more actions) that there is a conflict of concerns here and that we should break this reducer function into two separate fuctions. 
One will handle the list of todos, one will handle the data attributes of the single todos. 

This is our current code. 
```javascript
const todos = (state = [], action) => {
    switch (action.type) {
        case 'ADD_TODO':
            return [
                ...state, {
                    id: action.id,
                    text: action.text,
                    completed: false
                }
            ];
        case 'TOGGLE_TODO':
            return state.map(todo => {
                if (todo.id !== action.id) {
                    return todo;
                }

                return {
                    ...todo,
                    completed: !todo.completed
                };
            });
        default:
            return state;
    }
};
```

Let's refactor this code by concerns. 
See that the todos reducer invokes the todo reducer within the ```'TOGGLE_TODO'``` case. This is called **reducer composition**.

```javascript

const todo = (state, action) => {
    switch (action.type) {
        case 'ADD_TODO':
            return [
                ...state, {
                    id: action.id,
                    text: action.text,
                    completed: false
                }
            ];
        case 'TOGGLE_TODO':
            return state.map(todo => {
                if (state.id !== action.id) {
                    return todo;
                }

                return {
                    ...state,
                    completed: !todo.completed
                };
            });
        default:
            return state;
    }
}
const todos = (state = [], action) => {
    switch (action.type) {
        case 'ADD_TODO':
            return [
                ...state, 
                todo(undefined, action)
            ];
        case 'TOGGLE_TODO':
            return state.map(t => todo(t, action));
        default:
            return state;
    }
};

```