# Functional Javascript


## Table of Contents

1. [Introduction](#introduction)
1. [Functional Purity](#functional-purity)
1. [Higher Order Functions](#higher-order-functions)
1. [Closures](#closures)
1. [Currying](#currying)
1. [Composing](#composing)
1. [Further Reading](#further-reading)





------------------------------------------------

## Introduction
* Functional programming helps developers to abstract common collection operations into reusable and composable building blocks.
## First-Class Functions
* Function can be used anywhere that any other value can be passed.
* i.e. similar to the usage of values, functions can  be stored in arrays, passed in functions and  assigned to variables too.
* Example:

```js

var hi = function(name){
  return "Hi " + name;
};

var greeting = function(name) {
  return hi(name);
};
// instead greeting can be written as
greeting  = hi

greeting('Jones'); // Hi Jones

```

* Here, the function wrapper around `hi` in `greeting` is completely redundant. 

```js
var BlogController = (function() {
  var index = function(posts) {
    return Views.index(posts);
  };
  var destroy = function(post) {
    return Db.destroy(post);
  };

  return {
    index: index, destroy: destroy
  };
})();
```

* We could be writen as:

```js
var BlogController = {
  index: Views.index,
  destroy: Db.destroy
};
```
## Functional Purity
* The Purity of the function means that the function doesn't change any value, it just receives data and output data
* They don't cause any side-effects in other parts of the program.

Example:
```js
// impure
var minimum = 21;

var checkAge = function(age) {
  return age >= minimum;
};
// the result is dependant on variable minimum 



// pure
var checkAge = function(age) {
  var minimum = 21;
  return age >= minimum;
};
```


## Higher-Order Functions
* Higher Order functions can do one or both of the following:
    - Take a function as an argument
    - Return a function as a result
* Passing function as parameter
```js
 function logArrayElements(element, index, array) {
  console.log('a[' + index + '] = ' + element);
}
```

* Returning function as a result
```js
function song(start, end, lyricGen) {
    return _.reduce(_.range(start,end,-1),
    function(acc,n) {
        return acc.concat(lyricGen(n));
    }, []);
}

song(99, 0, f);
```
### Traversing an Array
```js
    var names = ["Ben", "Jafar", "Matt", "Priya", "Brian"];

    names.forEach(function(name) {
        console.log(name);
    });
        
```

### Implementing map()
* Map accepts the function to be applied to each item in the source array, and returns the resulting array.

```js
Array.prototype.map = function(projectionFunction) {
    var results = [];
    this.forEach(function(itemInArray) {
        results.push(projectionFunction(itemInArray));

    });

    return results;
};

// JSON.stringify([1,2,3].map(function(x) { return x + 1; })) === '[2,3,4]'
        
        
```

### Implementing filter()
* The filter() function accepts a predicate and returns the resultset mathing the condition in the predicate. 
* [ A predicate is a function that accepts an item in the array, and returns a boolean indicating whether the item should be retained in the resulting array.]

```js
Array.prototype.filter = function(predicateFunction) {
    var results = [];
    this.forEach(function(itemInArray) {
      if (predicateFunction(itemInArray)) {
        results.push(itemInArray);
      }
    });

    return results;
};

// JSON.stringify([1,2,3].filter(function(x) { return x > 2})) === "[3]"
        
        
```
### Implementing reduce()
* The reduce() function returns an array with single Item by applying predicate on input array items. 

```js

var ratings = [2,3,1,4,5];
var result = ratings.reduce(function(acc, curr) {
                if(acc > curr) {
                  return acc;
                }
                else {
                  return curr;
                }
            });
// returns an array containing only the largest rating Item in the input array.
        
```


## Closures

* Closure save some data inside a function that's only accessible to a specific returning function, i.e the returning function keeps its execution environment.
.Example:
```js
function functionClosure() {
var CAPTURED = "Oh hai";
return function() {
return "The local was: " + CAPTURED;
};
}
// value for the variable CAPTURED can be accessible in the returning function.
```


## Currying
* The concept is simple: You can call a function with fewer arguments than it expects. It returns a function that takes the remaining arguments. 

```js
var add = function(x) {
  return function(y) {
    return x + y;
  };
};

var increment = add(1);
var addTen = add(10);

addTen(2);
// 12

increment(2);
// 3


```

## Composing
* The composition of two functions returns a new function. i.e. when two or more functions are passed into the compose function which inturns returns a new function which is composition of all passed functions as parameters.

```js
var compose = function(f,g) {
  return function(x) {
    return f(g(x));
  };
};
var toUpperCase = function(x) { return x.toUpperCase(); };
var exclaim = function(x) { return x + '!'; };
var shout = compose(exclaim, toUpperCase);

shout("send in the clowns");
//=> "SEND IN THE CLOWNS!"
```
Composition of multiple functions
```js
// previously we'd have to write two composes, but since it's associative, we can give compose as many fn's as we like and let it decide how to group them.
var lastUpper = compose(toUpperCase, head, reverse);

lastUpper(['jumpkick', 'roundhouse', 'uppercut']);
//=> 'UPPERCUT'


var loudLastUpper = compose(exclaim, toUpperCase, head, reverse)

loudLastUpper(['jumpkick', 'roundhouse', 'uppercut']);
//=> 'UPPERCUT!'
```
## Underscore, lodash and Ramda

### [Ramda](http://ramdajs.com/) 
* A practical functional library for Javascript programmers. Ramda is designed specifically for a functional programming style, one that makes it easy to create functional pipelines and never mutates user data.
### [lodash](https://lodash.com/)
It is a modern JavaScript utility library offers high performance and modularity.

### [Underscore](http://underscorejs.org/)
* Underscore is a JavaScript library that provides lot of useful functional programming helpers.

## Further Reading

###### [Functional Javascript with Ramda](https://github.com/MostlyAdequate/mostly-adequate-guide/tree/69ebed50eded952a86082fb7ac745db7323d3e91)
###### [From Map/Reduce to JavaScript Functional Programming](https://hacks.mozilla.org/2015/01/from-mapreduce-to-javascript-functional-programming/)
###### [Functional Programming in Javascript](http://reactivex.io/learnrx/)
