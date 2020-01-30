# React Refactor: Part 1

The ```TodoApp``` component has been getting progressively larger throughout these tutorials. Let's start refactoring it into smaller components.

First, let's start with the list items, which are the actual todo items. 
Here is what we currently have. 
```javascript
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
```

We are going to declare a new ```Todo``` component.  
- Let's copy the old <li> code into this new component get to refactoring!
- Remove the key property as it's only needed if we enumerate over an array:  ~~key={todo.id}~~
- Next let's remove any type of functionality. This will make components more flexible.
  - Remove the onClick code:  ```onClick={}``` and pass it in as a argument
- pass in ```completed``` and ```text``` as separate props. 
- This component is now a functional presentational component. It no longer specifies any type of behavior. 
```javascript
const Todo = ({
    onClick,
    completed,
    text
}) => (
    <li 
        onClick={onClick}
        style={{
            textDecoration: 
            completed ?
            'line-through' :
            'none'
        }}
    >
        {text}
    </li>
);
```

Next, Let's refactor the TodoList. Like the Todo component, this component will only be interested in how it looks. 
```javascript
const TodoList = () => ({
    todos,
    onTodoClick
}) => (
    <ul>
        {todos.map(todo =>
            <Todo
                key={todo.id}
                {...todo}
                onClick={() => onTodoClick(todo.id)}
            />)}
    </ul>
)
```

Both of these components ```Todo``` & ```Todolist``` are functional presentational components. 
We should create a container component that dictates these component's functionality. 
In this example our top level ```TodoApp``` will act as the container component. 

```javascript
<TodoList
    todos={visibleTodos}
    onTodoClick={id => 
        store.dispatch({
            type: 'TOGGLE_TODO', 
            id
        })
    }
/>
```

The code so far should look like this.
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
    switch (action.type) {
        case 'ADD_TODO':
            console.log('Adding in todos!');
            return [
                ...state,
                todo(undefined, action)
            ]
        case 'TOGGLE_TODO':
            return state.map(t =>
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
    switch (action.type) {
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

const FilterLink = ({
    filter,
    currentFilter,
    children
}) => {
    if (filter === currentFilter) {
        return <span>{children}</span>
    }
    return (
        <a href="#"
            onClick={e => {
                e.preventDefault();
                store.dispatch({
                    type: 'SET_VISIBILITY_FILTER',
                    filter
                });
            }}
        >
            {children}
        </a >
    )
}

const getVisibleTodos = (
    todos,
    filter
) => {
    switch (filter) {
        case 'SHOW_ALL':
            return todos;
        case 'SHOW_COMPLETED':
            return todos.filter(
                t => t.completed
            );
        case 'SHOW_ACTIVE':
            return todos.filter(
                t => !t.completed
            )
    }
}

const Todo = ({
    onClick,
    completed,
    text
}) => (
        <li
            onClick={onClick}
            style={{
                textDecoration:
                    completed ?
                        'line-through' :
                        'none'
            }}
        >
            {text}
        </li>
    );

const TodoList = () => ({
    todos,
    onTodoClick
}) => (
        <ul>
            {todos.map(todo =>
                <Todo
                    key={todo.id}
                    {...todo}
                    onClick={() => onTodoClick(todo.id)}
                />)}
        </ul>
    )

let nextToDoId = 0

class TodoApp extends Component {
    render() {
        const {
            todos,
            visibilityFilter
        } = this.props;

        const visibleTodos = getVisibleTodos(
            todos,
            visibilityFilter
        );

        console.log("props: ", this.props);
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
                <TodoList
                    todos={visibleTodos}
                    onTodoClick={id =>
                        store.dispatch({
                            type: 'TOGGLE_TODO',
                            id
                        })
                    }
                />
                <p>
                    Show:
                    {' '}
                    <FilterLink
                        filter='SHOW_ALL'
                        currentFilter={visibilityFilter}
                    >
                        All
                    </FilterLink>
                    {' '}
                    <FilterLink
                        filter='SHOW_ACTIVE'
                        currentFilter={visibilityFilter}
                    >
                        Active
                    </FilterLink>
                    {' '}
                    <FilterLink
                        filter='SHOW_COMPLETED'
                        currentFilter={visibilityFilter}
                    >
                        Completed
                    </FilterLink>
                </p>
            </div>
        )
    }
}

const render = () => {
    ReactDOM.render(
        <TodoApp {...store.getState()} />,
        document.getElementById('root')
    )
}

store.subscribe(render);
render();
```
