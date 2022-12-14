# javascript-helper-functions
Most commonly used helper functions in javascript projects.

## List of Functions
* flipObject( object )
* flipArray( array )
* normalized({ objects: data, id: "uid" })
* countObject({   
 &nbsp;&nbsp;&nbsp;  object: data,  
 &nbsp;&nbsp;&nbsp;   excludeType:['undefined','null','function',"object", "array", "string","number"]  
})  
-------------------------------
* isNumber( number )
* isBigInt( 15464n )
* isInteger( 4897 | 749789n )
* isString( string )
* isObject( object )
* isFunction( function )
* isArray( array )
* isBoolean( boolean )
* isDate( new Date() )
* isRegExp( /re/ | new RegExp() )
* isSymbol( Symbol() )
* isMap( new Map() )
* isWeakMap( new WeakMap() )
* isSet( new Set() )
* isWeakSet( new WeakSet() )
* isNull( null )
* isUndefined( undefined )
* isNegativeZero( -0 )
* isEmpty( String | Object | Array )



### flipObject( object )
```javascript
/**
 * flip an object i.e. transform { key : value} into {value : key}
 *
 * good for small data set only. 
 *
 * return new Object without mutate original object
 *
 * this function can throw error, if argument is not type of Object 
 * 
 * remove other types (i.e, Array, Object, Function etc) from value of { key : value }
 *
 ****
 * @has dependancy to other functions : [ isObject(), isString(), isNumber() ]
 *
 * @parameter (input)
 *
 * @return flip object as { value : key } pairs
 * 
**/

  function flipObject ( input ) {

    if (! isObject ( input ) ) {
      throw new TypeError ( "given argument is not type of Object" );
    }
    
    const flipObj = {}
  
    for ( [ key, value ] of Object.entries (input) ) { 
      if ( isString ( value ) || isNumber ( value) ) {
          flipObj[ value ] = key;
      }
    }
  
    return flipObj;
  }
```

### flipArray( array )
```javascript
/**
 * flip an Array i.e. transform ["a","b","c"] into {"a":0, "b":1, "c":2}
 *
 * this function can throw error, if argument is not type of Array 
 * 
 * remove other types (i.e, Array, Object, Function etc) from value of { key : value }
 * add only types (string or number) as keys of object from array
 *
 ****
 * @has dependancy to other functions : [ isArray(), isString(), isNumber() ]
 *
 * @parameter (array)
 *
 * @return flip object as { value : key } pairs
 * 
**/
function flipArray( array ) {
  if (! isArray(array) ) {
    throw new TypeError("argument is not an array, flipArray( array ) ");
  }

  const flipArr = {};

  array.forEach( ( element, index ) => {
    if ( isString( element ) ) {
      flipArr[ element.trim() ] = index;
    }
    else if ( isNumber( element ) ) {
      flipArr[ element] = index;
    }
  });

  return flipArr;
}
```
### normalized({ objects: data, id:'uid' })

```javascript
/**
*
Convert Json format Array into object to increase performance  
e.g. [ { }, { } ] into {ids: [], entities: {}}  
  
access of Array item is O(N)  
access of Object item is O(1)
  
Data Array format from server is:  
  [  
    {id: "user1", firstName, lastName},  
    {id: "user2", firstName, lastName},  
    {id: "user3", firstName, lastName},  
  ]    
    
Data after Nomrlized format is:  
  {  
    ids: ["user1", "user2", "user3"],  
    entities: {  
      "user1": {id: "user1", firstName, lastName},  
      "user2": {id: "user2", firstName, lastName},  
      "user3": {id: "user3", firstName, lastName},  
    }  
  }
 *
 ****
 * @has dependancy to other functions : [ isObject(), isEmpty(),  ]
 *
 * @parameter (objects, id)
 *
 * @return {ids: [], entities: {}}
 * 
**/
function normalized ({ objects = [], id = "id" }) {
    
    if ( isEmpty( objects ) ) {
        return {ids: [], entities: {}};
    }
    else if (! isObject( objects[0] )){
        throw new TypeError( "Cannot find objects in Array list" );
    }
    else if ((! (id in objects[0])) ) {
        throw new Error( `cannot find (${id}) field in Object.

            Usage Example:

              const data = [
                {uid:"user1", name: "Pkfan Amir"},
                {uid:"user2", name: "Sir Kyle Simpson"}
              ];

              normalized({ objects: data, id: 'uid' })
        `); 
    }    


    const ids = [];
    const entities = {};

    objects.forEach( object => {
       ids.push(object[id]);
       
       entities[object[id]] = object;
    });

    return {ids, entities};
}
```
### countObject({ object: data, excludeType:['undefined','null','function',"object", "array", "string","number"] })

```javascript
/**
 *
 * @has dependancy to other functions : [ flipArray(), isObject(), isNull(), isUndefined(), isArray() ]
 *
 * @parameter ({ object, excludeType = [] })
 *
 * @return Number (integer)
 * 
**/

function countObject ({ object, excludeType = [] }) {

  if (! isObject( object ) ) {
    throw new TypeError( "first argument is not an 'object' type" );
  }

  const objectValues = Object.values(object);

  if ( excludeType.length <= 0 ) {
    return objectValues.length;
  }
  
  const excludeTypeObj = flipArray( excludeType );

  const finalValues = objectValues.filter( (value) => {
    try {
      return (! (value.constructor.name.toLowerCase() in excludeTypeObj) );
    }
    catch ( error ) {
      if ( 
        ( "null" in excludeTypeObj && isNull( value ) )
        || ( "undefined" in excludeTypeObj && isUndefined( value ) )
      ) {
        return false;
      }
      else {
        return true;
      };

    }
  });

  return finalValues.length;
}
```

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
      return (! isNaN( input ) ) &&  input.constructor.name == "Number";
    }
    catch ( error ) {
      return false;
    }
}
```

### isBigInt( 126464n )
```javascript
/**
 * check if given argument is type of BigInt or not
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

function isBigInt ( input ) {
    try {
        return input.constructor.name == "BigInt";
    }
      catch ( error ) {
        return false;
    }
}
```

### isInteger(456465 | 126464n )
```javascript
/**
 * check if given argument is type of (Number | BigInt) or not
 * 
 * this function cannot throw any exception, only return true/false
 *
 ****
 * @has dependancy to other functions : [isBigInt(), isNumber()]
 *
 * @parameter (input)
 *
 * @return true/false
 * 
**/

function isInteger ( number ) {
    if ( isBigInt( number ) ) {
        return true;
    } 
    else if (! isNumber(number) ) {
        throw new TypeError( "argument must be a number, isNumber( number )" );
    };

     return Number().constructor.isInteger(number);
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

### isFunction( function )
```javascript
/**
 * check if given argument is type of function or not
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

function isFunction ( input ) {
    try {
      return input.constructor.name == "Function";
    }
    catch ( error ) {
      return false;
    }
}
```
### isArray( array )
```javascript
/**
 * check if given argument is type of Array or not
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

function isArray ( input ) {
    try {
      return input.constructor.name == "Array";
    }
    catch ( error ) {
      return false;
    }
}
```

### isBoolean( boolean )
```javascript
/**
 * check if given argument is type of Boolean or not
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

function isBoolean ( input ) {
    try {
      return input.constructor.name == "Boolean";
    }
    catch ( error ) {
      return false;
    }
}
```
### isDate( new Date() )
```javascript
/**
 * check if given argument is type of Date or not
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

function isDate ( input ) {
    try {
      return input.constructor.name == "Date";
    }
    catch ( error ) {
      return false;
    }
}
```

### isRegExp( /re/ | new RegExp() )
```javascript
/**
 * check if given argument is type of Date or not
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

function isRegExp ( input ) {
    try {
      return input.constructor.name == "RegExp";
    }
    catch ( error ) {
      return false;
    }
}
```

### isSymbol( Symbol() )
```javascript
/**
 * check if given argument is type of Symbol or not
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

function isSymbol ( input ) {
    try {
      return input.constructor.name == "Symbol";
    }
    catch ( error ) {
      return false;
    }
}
```

### isMap( new Map() )
```javascript
/**
 * check if given argument is type of Map or not
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

function isMap ( input ) {
    try {
      return input.constructor.name == "Map";
    }
    catch ( error ) {
      return false;
    }
}
```

### isWeakMap( new WeakMap() )
```javascript
/**
 * check if given argument is type of WeakMap or not
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

function isWeakMap ( input ) {
    try {
      return input.constructor.name == "WeakMap";
    }
    catch ( error ) {
      return false;
    }
}
```

### isSet( new Set() )
```javascript
/**
 * check if given argument is type of Set or not
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

function isSet ( input ) {
    try {
      return input.constructor.name == "Set";
    }
    catch ( error ) {
      return false;
    }
}
```

### isWeakSet( new WeakSet() )
```javascript
/**
 * check if given argument is type of WeakSet or not
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

function isWeakSet ( input ) {
    try {
      return input.constructor.name == "WeakSet";
    }
    catch ( error ) {
      return false;
    }
}
```

### isNull( null )
```javascript
/**
 * check if given argument is type of null or not
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

function isNull ( input ) {
   return input === null;
}
```

### isUndefined( undefined )
```javascript
/**
 * check if given argument is type of undefined or not
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

function isUndefined  ( input ) {
   return input === undefined ;
}
```

### isNegativeZero( -0 )
```javascript
/**
 * check if given argument is negative zero ( -0 ) or not
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

function isNegativeZero(zero) {
    return Object.is(zero,-0)
}
 ```
 
### isEmpty( String | Object | Array )
```javascript
/**
 * check if given argument is empty or not
 * 
 * only supported types are (String | Object | Array)
 * 
 * this function can throw exception, if given argument is not one of these (String | Object | Array)
 *
 ****
 * @has dependancy to other functions : [ isString(), isArray(), isObject() ]
 *
 * @parameter (input)
 *
 * @return true/false
 * 
**/

function isEmpty ( input ) {
    
  if ( isString( input ) ) {
    return input.trim()[0] == undefined;
  }
  if ( isArray( input ) ) {
    return input.length === 0;
  }
  else if ( isObject(input ) ) {
    return Object.keys(input).length === 0;
  }
  else {
    throw new TypeError("only supported types are (String | Object | Array) for isEmpty() function");
  }
}
```

