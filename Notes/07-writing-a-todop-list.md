# Writing a todo list

```javascript
const todos = (state = [], action) => {
  switch (action.type){
    case 'ADD_TODO':
      return [
        ...state, 
        {
          id: action.id,
          text: action.text,
          completed: false
        }
      ];
    default:
       return state;
  }
};

const testAddTodo = () => {
  const stateBefore = [];
  const action = {
    type: 'ADD_TODO', 
    id: 0,
    text: 'Learn Redux'
  };
  const stateAfter = [
    {
      id: 0,
      text: 'Learn Redux',
      completed: false
    }
  ];
  deepFreeze(stateBefore);
  deepFreeze(action);
  
  expect(
  todos(stateBefore, action)
  ).toEqual(stateAfter)
};

testAddTodo();
console.log('All tests passed.')
console.log(state);
```

## Next Steps
- Move on to how to create a todolist toggle [here](./08-wrting-a-todo-list-(toggling).md) & [here](./09-toggling-a-todolist.md)