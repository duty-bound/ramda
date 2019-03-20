# Ramda

## Sample Data

**Single Dimension Data**
```
const arr = [1.1, 2.2, 3.3]

const obj = {name: 'elephant', 
			type: 'mammal', 
			origin: { continent: 'Africa', country: 'Gabon', } }
```
**Multi-Dimension Data**
```
const arrList = [
  [1.1, 1.2, 1.3],
  [2.1, 2.2, 2.3],
  [3.1, 3.2, 3.3]
]

const objList = {
  el: { name: 'elephant', 
			type: 'mammal', 
			origin: { continent: 'Africa', country: 'Gabon', }, },
  sk: {	name: 'shark',
		    type: 'fish',
		    origin: { continent: 'Australia', country: 'Sydney', }, },
  ea: { name: 'eagle',
		    type: 'bird',
		    origin: { continent: 'USA', country: 'Arizona', }, }
}
```
## Extracting Data

### Extracting Data from Single Dimension Containers

#### prop
```
const arr = [1.1, 2.2, 3.3]

R.prop(1, arr)

//2.2
```
```
const obj = {name: 'elephant', 
             type: 'mammal',
            origin: {
              continent: 'Africa',
              country: 'Gabon',
            }
         }

R.prop('name', obj)

//elephant
```
Note that prop is not able to dig deep enough to extract the contents of 'origin'. In that case, nesting in another prop would be required:
```
R.prop('continent', R.prop('origin', obj))
```
Or better, using a function like 'path'.

#### path
```
const arrList = [
  [1.1, 1.2, 1.3],
  [2.1, 2.2, 2.3],
  [3.1, 3.2, 3.3]
]

R.path([2, 1], arrList)

//3.2
```
```
const obj = {name: 'elephant', 
             type: 'mammal',
            origin: {
              continent: 'Africa',
              country: 'Gabon',
            }
         }

R.path(['origin', 'country'], obj)

//Africa
```
## Relational Functions
These functions are used to determine whether a condition is true or not, such as the existence of a specified element in a given path, or whether a variable is greater or smaller than another variable, checking for equality, etc.

### Works on a Single Element
### Works on a List

#### all

Returns `true` if all elements of the list match the predicate, `false` if there are any that don't.

```
const arr = [1.1, 2.2, 3.3]

R.all(n => n < 4, arr)

//true
```
```
const arr = [1.1, 2.2, 3.3]

R.all(n => n < 3, arr)

//false
```
#### allPass

Whereas 'all' checks that all members in a list have a single trait, 'allPass' checks whether a single entity has multiple traits.

Also 'allPass' returns a function; it can't be applied directly on a variable.
```
const greaterThan10 = n => n > 10
const smallerThan20 = n => n < 20

const inRange = R.allPass([greaterThan10, smallerThan20])

inRange([11])

//true
```
```
const isQueen = R.propEq('rank', 'Q')
const isSpade = R.propEq('suit', '♠︎')
const isQueenOfSpades = R.allPass([isQueen, isSpade])

isQueenOfSpades({rank: 'Q', suit: '♣︎'})
//false

isQueenOfSpades({rank: 'Q', suit: '♠︎'})
//true
```
#### and

Returns true if both arguments are true.
```
const n = 15

R.and(n > 10, n < 20)

//true
```
#### any

Returns true if any of the elements in the list matches the predicate.
```
const arr = [1.1, 2.2, 3.3]

R.any(n => n < 2, arr)

//true
```

#### anyPass

Contrary to 'any', which executes a single test on a list of variables, 'anyPass' carries several tests on a single variable.
```
const isQueen = R.propEq('rank', 'Q')
const isSpade = R.propEq('suit', '♠︎')
const isQueenOrSpades = R.anyPass([isQueen, isSpade])

isQueenOrSpades({ rank: 'Q', suit: '♠︎'})
//true

isQueenOrSpades({ rank: '10', suit: '♠︎'})
//true

isQueenOrSpades({ rank: '10', suit: '♣︎'})
//false
```

#### both

Returns a function which return true if both of the functions supplied to `both` are true.

```
const greaterThan10 = n => n > 10
const lessThan20 = n => n < 20

const isInRange = R.both(greaterThan10, lessThan20)

isInRange(15) //true
isInRange(150) //false
```

#### comparator

Returns a comparator function that checks whether the first parameter is smaller than the second, and returns -1, 0, or 1 in case it is respectively smaller, equal, or greater.

```
const arr = [3, 6, 8, 4, 6, 10, 1]

const asc = R.comparator((a, b) => a < b)

R.sort(asc, arr)

//[1, 3, 4, 6, 6, 8, 10]
```

#### complement

Takes a function that returns either true or false and creates a function that returns the opposite of what the provided function would return.

(Check the example in `cond` for a useful implementation of `complement`)

```
const notNil = R.complement(R.isNil)

R.isNil(null) //true
notNil(null) //false
```

```
const isEven = n => n % 2 === 0
const isOdd = R.complement(isEven)

isEven(4) //true
isOdd(5) //true
```

#### cond

This is Ramda's equivalent of nested if/else statements. Conditions are passed to this functions in a 'condition', 'transformer' fashion.

(Note in the below example that a function such as `R.lt` returns true if the first argument is less than the second. However when using `cond` the second parameter is always the value passed by `cond` resulting in the opposite results to that desired being returned. Thus `complement` is used to rectify this.

```
const fn = R.cond([
  [R.complement(R.lt(18)), R.always("too young")],
  [R.complement(R.gte(18)), R.always("OK")]
]);

fn(17) //"too young"
fn(18) //"OK"
```

## Filtering 

## Sorting

#### ascend / descend

Return a comparator function which returns a value that can be used with `<` and `>`.

```
const objList = {
  el: { name: 'elephant', type: 'mammal' },
  sh: {	name: 'shark', type: 'fish' },
  ea: { name: 'eagle', type: 'bird' },
}
const animals = R.values(objList)

const asc = R.ascend(R.prop('name'))
R.sort(asc, animals)

//[{"name": "eagle", "type": "bird"},
//{"name": "elephant", "type": "mammal"},
//{"name": "shark", "type": "fish"}]
```

## Operational

#### add
```
R.add(2.1, 3.45)
//5.550000000000001
```

#### adjust

Applies a function to a specific index in an array 

```
const arr = [1.1, 2.2, 3.3]

R.adjust(1, (n) => n * 2, arr)

//[1.1, 4.4, 3.3]
```

```
const animals = ['elephant', 'tiger', 'shark']

R.adjust(0, R.toUpper, animals)

//["ELEPHANT", "tiger", "shark"]
```

#### always

Always returns the same value.
```
const pi = R.always(3.142)

pi()

//3.142
```
```
const bike = R.always({ brand: 'Ducati', model: 'Monster'})

bike().brand

//Ducati
```

#### ap

Applies the provided list of functions to each element of a list and returns everything in a single array.
```
const arr = [1.1, 2.2, 3.3]

const x2 = n => n * 2
const add3 = n => n+ 3

R.ap([x2, add3], arr)

//[2.2, 4.4, 6.6, 4.1, 5.2, 6.3]
```
```
R.ap([R.toUpper, R.toLower],['Crazy'])
["CRAZY", "crazy"]
```

#### aperture

Returns a new list of consecutive combinations of 'n' each.

```
R.aperture(2, [1, 2, 3, 4, 5])
//[[1, 2], [2, 3], [3, 4], [4, 5]]

R.aperture(3, [1, 2, 3, 4, 5])
//[[1, 2, 3], [2, 3, 4], [3, 4, 5]]

R.aperture(7, [1, 2, 3, 4, 5])
//[]
```

#### append
Appends the provided element (not elements) to the end of the provided array.
```
R.append('C', ['A', 'B'])
//["A", "B", "C"]

R.append('C', [])
//["C"]
```

#### apply

Applies the provided function to the list provided. The function to be provided should be one that operates on the list as an aggregate and returns one single variable: 'apply' will not return a list.
```
const arr = [1.1, 2.2, 3.3]

R.apply(Math.max, arr)

//3.3
```
```
const letters = ['a', 'b', 'c']

R.apply(R.toUpper, letters)

//"A"
```
#### applyTo

Takes a value and returns a function that passes this value to other functions.
```
const num = R.applyTo(3)

num(R.inc())

//4
```

```
const pi = R.applyTo(3.142)

const circumference = (d) => R.multiply(d)

pi(circumference(2))

//6.284
```

#### assoc
Inserts a key / value set in a clone of the provided object literal. Note that the new object literal is sorted in ascending order.

```
const objList = {
  el: { name: 'elephant', type: 'mammal' },
  sh: {	name: 'shark', type: 'fish' },
  ea: { name: 'eagle', type: 'bird' },
}

R.assoc('mm', {name: 'elephant', type: 'mammal'}, objList);

//{"ea": {"name": "eagle", "type": "bird"},
//"el": {"name": "elephant", "type": "mammal"},
//"mm": {"name": "elephant", "type": "mammal"}, 
//"sh": {"name": "shark", "type": "fish"}}
```

#### assocPath

Makes a clone of the provided object while overriding any values in the path provided in the first parameter by the value provided in the second parameter. Note that the object literal created is sorted in ascending order.

```
const objList = {
  el: { name: 'elephant', 
			type: 'mammal', 
			origin: { continent: 'Africa', country: 'Gabon', }, },
  sk: {	name: 'shark',
		    type: 'fish',
		    origin: { continent: 'Australia', country: 'Sydney', }, },
  ea: { name: 'eagle',
		    type: 'bird',
		    origin: { continent: 'USA', country: 'Arizona', }, }
}
R.assocPath(['sk', 'origin', 'country'], 'South Africa', objList);

//{"ea": {"name": "eagle", "origin": {"continent": "USA", "country": "Arizona"}, "type": "bird"},
//"el": {"name": "elephant", "origin": {"continent": "Africa", "country": "Gabon"}, "type": "mammal"},
//"sk": {"name": "shark", "origin": {"continent": "Australia", "country": "South Africa"}, "type": "fish"}}
```

#### binary

Takes a function of any arity as a parameter and returns a function of exactly two parameters.

```
const func3 = (m, n, o, p) => `${m} ${n} ${o} ${p}`
func3('so', 'far', 'so', 'good')
//"so far so good"

const func2 = R.binary(func3)
func2('so', 'far', 'so', 'good')
//"so far undefined undefined"

const func2 = R.binary(func3)
func2('so', 'far')
//"so far undefined undefined"
```

#### call
This is a very powerful function. It takes a function as a first parameter, and an arbitrary number of other arguments which will be passed to the function provided.

```
R.call(R.prop(2), ['a', 'b', 'c'])

//"c"
```

#### chain

Takes a function, applies it to a list, and concatenates the result into an array.

```
const arr = ['a', 'b', 'c']

const addUpper = c => [c, R.toUpper(c)]

R.chain(addUpper, arr)

//["a", "A", "b", "B", "c", "C"]
```
Check the belwo to see what `map` would return:
```
const arr = ['a', 'b', 'c']

const addUpper = c => [c, R.toUpper(c)]

R.map(addUpper, arr)

//[["a", "A"], ["b", "B"], ["c", "C"]]
```

#### clamp

Takes three number parameters. The first two are the lower and higher values of a range, and the third is the number which will be restricted to that range.

If the number is within the range, it is returned as is, if not, the closest of the range limits is returned.

```
R.clamp(30, 40, 33) //33
R.clamp(30, 40, 25) //30
R.clamp(30, 40, 47) //40
```

#### clone

Creates a deep copy.

Note that the result is sorted (vide second example).

```
const arr = ['a', 'b', 'c']

R.clone(arr)

//["a", "b", "c"]
```
```
const objList = {
  el: { name: 'elephant', 
			type: 'mammal', 
			origin: { continent: 'Africa', country: 'Gabon', }, },
  sk: {	name: 'shark',
		    type: 'fish',
		    origin: { continent: 'Australia', country: 'Sydney', }, },
  ea: { name: 'eagle',
		    type: 'bird',
		    origin: { continent: 'USA', country: 'Arizona', }, }
}

R.clone(objList)

//{"ea": {"name": "eagle", "origin": {"continent": "USA", "country": "Arizona"}, "type": "bird"},
//"el": {"name": "elephant", "origin": {"continent": "Africa", "country": "Gabon"}, "type": "mammal"},
//"sk": {"name": "shark", "origin": {"continent": "Australia", "country": "Sydney"}, "type": "fish"}}
```

#### compose

Performs right-to-left function composition. The rightmost function may have any arity; the remaining functions must be unary.

```
const x2 = n => n * 2
const plus3 = n => n + 3

R.compose(x2, plus3)(3) //12
R.compose(plus3, x2)(3) //9
```

```
const joinToUpper = R.compose(R.toUpper, R.concat)
joinToUpper('foo', 'bar')  //"FOOBAR"

//or:
R.compose(R.toUpper, R.concat)('foo', 'bar') //"FOOBAR"
```

#### concat

Concatenates the provided strings or arrays. Takes a maximum of two parameters.

```
R.concat(['a', 'b', 'c'], ['d', 'e', 'f'])

//["a", "b", "c", "d", "e", "f"]
```

```
R.concat('foo', 'bar') 

//"foobar"
```

#### construct

This function returns a function equivalent to the `new` keyword that creates an object from a constructor function.

```
class Car {
  constructor(car) {
    this.name = car.name
    this.engine= car.engine
  }
}

const vehicles = [ 
  { name: 'mazda', engine: 16 },
  { name: 'alfa', engine: 18 },
  { name: 'tesla', engine: 10 }
]

const CarConstructor = R.construct(Car)

const cars = R.map(CarConstructor, vehicles)

cars[2]
//{"engine": 10, "name": "tesla"}
```


#### converge

Takes two parameters and returns a function. The first parameter is a converging function that takes as arguments the result of the each of the branching functions supplied in the second parameter.

Each branching function takes the exact same argument supplied to the created function. So the order in which the branching functions are executed is not important.

The arity of the new function is the same as the arity of the longest branching function.

const average = R.converge(R.divide, [R.sum, R.length])

average([1, 2, 3, 4, 5])
//3


#### countBy

Counts the number of unique occurrences in a list that is constructed from the function supplied to the function.

Thus, a list cannot be directly supplied to the `countBy`, the below does not work:
```
const nums = [1, 2, 2, 3, 3, 3, 4, 4, 4, 4]

R.countBy(nums)
```

The below however does, because the supplied function is used to construct an index:
```
const nums = [1, 2, 2, 3, 3, 3, 4, 4, 4, 4]

R.countBy(i => i)(nums)

//{"1": 1, "2": 2, "3": 3, "4": 4}
```
```
const nums = [1, 2, 3, 4, 5, 6, 7, 8, 9]

R.countBy(R.lt(5), nums)

//{"false": 5, "true": 4}
```

```
const numbers = [1.0, 1.1, 1.2, 2.0, 3.0, 2.2]

R.countBy(Math.floor)(numbers)

//{'1': 3, '2': 2, '3': 1}
```
The below example is tricky. `R.lt` checks whether the first parameter is smaller than the second. But `the argument supplied by R.countBy`  to `R.lt` would be `R.lt`'s second parameter. So, in order to do a 'less than' check, the result of `R.lt` would give exactly the opposite of what is required. So one should in this case use either of the below:

```
const nums = [1, 2, 2, 3, 3, 3, 4, 4, 4, 4]

R.countBy(R.complement(R.lte(4)), nums)
//{"false": 4, "true": 6}

R.countBy(R.gt(4), nums)
//{"false": 4, "true": 6}
```

#### curry

Imagine a function such as the below:
```
const f = (x, y, z) => x + y + z

f(1, 2, 3) //6
f(1, 2)(3) //error
```
No currying is allowed. `curry` solves this problem; the below are all allowed:
```
g(1, 2, 3) //6
g(1)(2)(3) //6
g(1, 2)(3) //6
g(1)(2, 3) //6
g()(1, 2, 3) //6
```
Secondly, the special placeholder value `R._` may be used to specify "gaps", allowing partial application of any combination of arguments, regardless of their positions:
```
g(1, 2, 3)
g(_, 2, 3)(1)
g(_, _, 3)(1)(2)
g(_, _, 3)(1, 2)
g(_, 2)(1)(3)
g(_, 2)(1, 3)
g(_, 2)(_, 3)(1)
```
```
const addFourNumbers = (a, b, c, d) => a + b + c + d;

const curriedAddFourNumbers = R.curry(addFourNumbers);
const f = curriedAddFourNumbers(1, 2);
const g = f(3);
g(4)
//10
```

#### dec

Decrements a number.

```
R.dec(4) //3
R.dec(4.4) //3.4000000000000004
```

#### defaultTo

Returns a function that returns the specified value if passed either `null`, `undefined`, or `NaN`. 

Note: passing `false` does not trigger the default value.

```
const defaultToZero = R.defaultTo(0)

defaultToZero(null)
//0
```

## Mapping

#### addIndex

Returns an iterative function capable of making use of an index, from an existing one. Therefore it needs to be supplied with an iterative function such as 'map'.

It can in fact be used to mimic javascript's Array.prototype.map
```
const animals = ['elephant', 'tiger', 'shark']

const myMap = R.addIndex(R.map)

myMap((animal, i) => `<li key=${i}>${animal}</li>`, animals)

//["<li key=0>elephant</li>", "<li key=1>tiger</li>", "<li key=2>shark</li>"]
```

## String Manipulation

#### concat

Concatenates the provided strings or arrays. Takes a maximum of two parameters.

```
R.concat(['a', 'b', 'c'], ['d', 'e', 'f'])

//["a", "b", "c", "d", "e", "f"]
```

```
R.concat('foo', 'bar') 

//"foobar"
```


## Not Covered
- applySpec
- bind
- composeWith
- constructN
- curryN (how is it different from `curry`?
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjg1NDkyNDk2LDEyOTk4NDAwMzEsMTYzMj
M1NDcyNiwtODcyMTM0MjA1LDIxMDA4NDU1NTcsLTUzNDk0MjEx
MCwxMzQyMTM4ODU1LDIxMDcxOTI2NjgsMTI0MjYwOTUwOCwxMj
MyNTA5NDcyLDE0NDA1NjQ2NjAsMTk5MzQwMjk0NSwxNzA2MDgy
MDQ5LC0xMTMyNTYwOTc0LDkyNDc5ODQzLC0yMTA0MTk5MDU1LC
00MzM1MDM2ODgsLTExNjc4OTg3MSwtOTcxODg2MjU1LDg5NjM2
NjUyXX0=
-->