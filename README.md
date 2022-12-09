# javascript-helper-functions

```javascript
/**
 * check if given argument is number or not
 * 
 * this function cannot throw any exception, only return true/false
 *
 ****
 * @has depency to other functions : [none]
 *
 * @parameter input
 *
 * @retrun true/false
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


