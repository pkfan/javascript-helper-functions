# javascript-helper-functions

```javascript
/**
 * check if given argument is number or not
 * 
 * this function cannot throw any exception, only return true/false
 *
 ****
 * @has dependancy to other functions : [none]
 *
 * @parameter (input)
 *
 * @return true/false
 * 
**/

function isNumber (input) {
  try {
    return input.constructor.name == "Number";
  }
  catch (error) {
    return false;
  }
}
```

```javascript
/**
 * check if given argument is string or not
 * 
 * this function cannot throw any exception, only return true/false
 *
 ****
 * @has dependancy to other functions : [none]
 *
 * @parameter (input)
 *
 * @return true/false
 * 
**/

function isString (input) {
  try {
    return input.constructor.name == "String";
  }
  catch (error) {
    return false;
  }
}
```

