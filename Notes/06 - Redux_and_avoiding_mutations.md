# Redux and Avoiding Mutations with concat(), Slice() & ...Spread

let's import two libraries, expect and deep-freeze.
- expect is an assursion test library 
- deep-freeze will test if any data has been mutated. 
  
```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width" />
    <script src="https://wzrd.in/standalone/expect@latest"></script>
    <script src="https://wzrd.in/standalone/deep-freeze@latest"></script>
    <title>JS Bin</title>
  </head>
  <body>
  </body>
</html>
```


## Concat()
let's create a simple ```addCounter``` function and some test coverage.  
We want to make sure that the original list is not mutated.  
The problem is that .push mutates the original. What other options do we have?
```javascript
const addCounter = (list) => {
  list.push(0);
  return list;
}

const testAddCounter = () => {
  const listBefore = [];
  const listAfter = [0];
  
  deepFreeze(listBefore);
  expect(
    addCounter(listBefore)
  ).toEqual(listAfter);
};

testAddCounter();
console.log('All tests passed.')
```

We can use the concat method, which doesn't mutate the original array. 
```javascript
const addCounter = (list) => {
 // list.push(0);
 //  return list;
 return list.concat[(0)]
 // or
 return [...list, 0]; // this is more concise. 
}
```

## Slice()
Next, let's add another function for removing items. 

```javascript
const removeCounter = (list, index) => {
  list.splice(index, 1);
  return list;
}

const testRemoveCounter = () => {
  const listBefore = [0, 10, 20];
  const listAfter = [0, 20];
  
  deepFreeze(listBefore);
  
  expect(
  removeCounter(listBefore, 1)
  ).toEqual(listAfter);
}
testAddCounter();
console.log('All tests passed.')
```
This will fail because splice mutates the original array. 
What is another approach?

using slice
```javascript
const removeCounter = (list, index) => {
  return list
    .slice(0, index)
    .concat(list.slice(index + 1));
}
```
or using the spread operator
```javascript
const removeCounter = (list, index) => {
  return [
      ...list.slice(0, index),
      ...list.slice(index + 1)
  ]
}
```

## Increment the counter
let's test an increment counter. 

```javascript
const incrementCounter = (list, index) => {
  list[index]++
  return list;
}

const testIncrementCounter = () => {
  const listBefore = [0, 10, 20];
  const listAfter = [0, 11, 20];
  
  deepFreeze(listBefore);
  
  expect(
  incrementCounter(listBefore, 1)
  ).toEqual(listAfter);
}
```
Incrementing using ```++``` mutates the original list. 

Very similar to removing and adding items to an array with mutation, we can using the concat operator. 
```javascript
const incrementCounter = (list, index) => {
  return list
    .slice(0, index)
    .concat([list[index] + 1])
    .concat(list.slice(index + 1));
};
```
or 
```javascript
const incrementCounter = (list, index) => {
  return [
    ...list.slice(0, index),
    list[index] + 1,
    ....list.slice(index + 1)
  ]
};
```

In the end, the javascript code should look like this...
```javascript
const addCounter = (list) => {
 return [...list, 0]; // this is more concise. 
}

const removeCounter = (list, index) => {
  return [
      ...list.slice(0, index),
      ...list.slice(index + 1)
  ]
}

const incrementCounter = (list, index) => {
  return [
    ...list.slice(0, index),
    list[index] + 1,
    ...list.slice(index + 1)
  ];
};

const testAddCounter = () => {
  const listBefore = [];
  const listAfter = [0];
  
  deepFreeze(listBefore);
  expect(
    addCounter(listBefore)
  ).toEqual(listAfter);
};

const testRemoveCounter = () => {
  const listBefore = [0, 10, 20];
  const listAfter = [0, 20];
  
  deepFreeze(listBefore);
  
  expect(
  removeCounter(listBefore, 1)
  ).toEqual(listAfter);
}

const testIncrementCounter = () => {
  const listBefore = [0, 10, 20];
  const listAfter = [0, 11, 20];
  
  deepFreeze(listBefore);
  
  expect(
  incrementCounter(listBefore, 1)
  ).toEqual(listAfter);
}
testAddCounter();
testRemoveCounter();
testIncrementCounter();
console.log('All tests passed.')

```