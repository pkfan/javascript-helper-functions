# javascript-helper-functions

### isNumber( number )
```javascript
/**
 * check if given argument is type of Number or not
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

function isNumber ( input ) {
  try {
    return input.constructor.name == "Number";
  }
  catch ( error ) {
    return false;
  }
}
```

### isString( string )
```javascript
/**
 * check if given argument is type of String or not
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

function isString ( input ) {
  try {
    return input.constructor.name == "String";
  }
  catch ( error ) {
    return false;
  }
}
```

### isObject( object )
```javascript
/**
 * check if given argument is type of Object or not
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

function isObject ( input ) {
  try {
    return input.constructor.name == "Object";
  }
  catch ( error ) {
    return false;
  }
}
```

### flip( object )
```javascript
/**
 * flip an object i.e. transform { key : value} into {value : key}
 *
 * good for small data set only. 
 *
 * return new Object without mutate original object
 *
 * this function can throw error, if argument is not object 
 * Or in Object { key : value}, where value is not type of Number or String
 *
 ****
 * @has dependancy to other functions : [ isObject(), isString(), isNumber() ]
 *
 * @parameter (input)
 *
 * @return true/false
 * 
**/

function flip ( input ) {

  if (! isObject ( input ) ) {
    return new Error ( "TypeError: argument is not an Object" );
  }
  
  let object = input;

  const allObjectValuesAreStringOrNumber = (object) => {
     return (
        Object
          .values( object )
          .every( ( value ) => isString ( value ) || isNumber ( value) )
      );
   }

  if (! allObjectValuesAreStringOrNumber( object ) ) {
    return new Error ( "Object Values Error: in Object { key : value }, where value must be a number or a string, but other type exist in object" ); 
  }

  const flipObject = {}

  for ( [ key, value ] of Object.entries (object) ) { 
    flipObject[ value ] = key;
  }

  return flipObject;
}
```
