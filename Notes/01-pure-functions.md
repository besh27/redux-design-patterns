# Pure Functions

## Pure
```javascript
function square(x){
    return x * x;
}

function squareAll(items){
    return items.map(square);
}
```
Notes:
- No modifications are made to the variables passed into the function. 
- The same returned value should always be returned with the same inputs. 
- No database calls. 
- No Side effects
- No overwritting arguments that are passed in. 



## Impure
```javascript
function square(x){
    updateInDatabase(x);
    return x*x;
}

function squareAll(items){
    for (let i = 0; i < items.length; i++){
        items[i] = square(items[i]);
    }
}
```

## Next Steps
- Move on to [reducer functions](./02-reducer-function.md)