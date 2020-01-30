# React Redux Todo List


Below is our current code. How would we hook this up to a redux app?
Let's implement a view layer. 

First, let's add some CDN links to our html within our online IDE. 
This way we can start using react and react-dom.

- react
- react-dom
- redux

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <title>JS Bin</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/react/16.12.0/umd/react.development.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/react-dom/16.11.0/cjs/react-dom.development.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/redux/4.0.5/redux.js"></script>
</head>
<body>
  <div id="root"></div>
</body>
</html>
```

Let's build a new React component called ```TodoApp```
```javascript
const { Component } = React;

let nextToDoId = 0

class TodoApp extends Component {
    render(){
        return (
            <div>
                <input ref={node => {
                  this.input = node;
                }} />
                <button
                    onClick={() => {
                        store.dispatch({
                            type: 'ADD_TODO',
                            text: this.input.value,
                            id: nextToDoId++
                        });
                        this.input.value = '';
                    }}
                >
                    Add Todo
                </button>
                <ul>
                    {this.props.todos.map(todo =>
                        <li 
                           key={todo.id}
                           onClick={() => {
                             store.dispatch({
                               type: 'TOGGLE_TODO',
                               id: todo.id
                             });
                           }}
                           style={{
                                  textDecoration: 
                                    todo.completed ? 
                                    'line-through' : 
                                    'none'
                                 }}
                        >
                            {todo.text}
                        </li>
                    )}
                </ul>
            </div>
        )
    }
}
```
- Above is our TodoApp component. 
- It consists of a input and button, which on click will dispatch the 'ADD_TODO' action. 
- There is also a list of the current TODO's. 
- When a TODO item is clicked, it will dispatch the 'TOGGLE_TODO' action. 
- When a todo.completed is true, the text decoration style is also changed to 'lline-through'.

```javascript
const render = () => {
    ReactDOM.render(
        <TodoApp todos={store.getState().todos}/>, 
        document.getElementById('root')
    )
}

store.subscribe(render);
render();
```
- In order to access the store to this component, we are passing it as a prop. ```store.getState().todos ```
- Any change to the state will be re-rendered because the render function, which controlls the where the ToDoApp component is rendered, is subscribed to the store. 

The javascript code should look like this. 
```javascript
const todo = (state = [], action) => {
    switch (action.type) {
        case 'ADD_TODO':
            console.log('Adding in todo!');
            return {
                id: action.id,
                text: action.text,
                completed: false
            };
        case 'TOGGLE_TODO':
                if (state.id !== action.id) {
                    return state;
                }

                return {
                    ...state,
                    completed: !state.completed
                };
        default:
            return state;
    }
};

const todos = (state = [], action) => {
    switch(action.type){
        case 'ADD_TODO':
            console.log('Adding in todos!');
            return [
                ...state,
                todo(undefined, action)
            ]
        case 'TOGGLE_TODO':
            return state.map( t => 
                todo(t, action)
            );
        default:
            return state;
    }
}

const visibilityFilter = (
    state = 'SHOW_ALL',
    action
) => {
    switch (action.type){
      case 'SET_VISIBILITY_FILTER':
            return action.filter;
        default:
            return state;
    }
}

const { combineReducers } = Redux;
const todoApp = combineReducers({
    todos, 
    visibilityFilter
})

const { createStore } = Redux;
const store = createStore(todoApp);

const { Component } = React;

let nextToDoId = 0

class TodoApp extends Component {

    render(){
        // console.log("props: ", this.props);
        return (
            <div>
                <input ref={node => {
                  this.input = node;
                }} />
                <button
                    onClick={() => {
                        store.dispatch({
                            type: 'ADD_TODO',
                            text: this.input.value,
                            id: nextToDoId++
                        });
                        this.input.value = '';
                    }}
                >
                    Add Todo
                </button>
                <ul>
                    {this.props.todos.map(todo =>
                        <li 
                           key={todo.id}
                           onClick={() => {
                             store.dispatch({
                               type: 'TOGGLE_TODO',
                               id: todo.id
                             });
                           }}
                           style={{
                                  textDecoration: 
                                    todo.completed ? 
                                    'line-through' : 
                                    'none'
                                 }}
                        >
                            {todo.text}
                        </li>
                    )}
                </ul>
            </div>
        )
    }
}

const render = () => {
    ReactDOM.render(
        <TodoApp todos={store.getState().todos}/>, 
        document.getElementById('root')
    )
}

store.subscribe(render);
render();
```
## Next Steps
- Move on to [React Redux Todo List Filtering](./13.react-redux-todo-filter.md)