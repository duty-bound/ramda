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

### Non-Iterative

#### either

This function is both non-iterative and iterative.

This function is a wrapper to the two supplied function; it calls the first and return its result if it is true. The second function is only called if the first returns false. `either` will return true if either of the function returns true.

`either` can be used to iterate through an array and trigger functions conditionally to create a new array with the desired results.
```
const lt30 = n => n < 30  
const gt40 = n => n > 40
const f = R.either(lt30, gt40)

f(24)
//true
```
```
const f = R.either(n => R.lt(n, 30), n => R.gt(n, 40))
f(24)
//true
```

Look at the below:
```
R.either([false, false, 'a'], [11])  
//[11, 11, "a"]

R.either([isEven(1), isEven(2), isEven(3)], [11])
//[11, true, 11]

R.either([11], [isEven(1), isEven(2), isEven(3)])
//[11, 11, 11]

R.either([isEven(1), isEven(2), isEven(3)], ['hey1', 'hey2', 'hey3'])
//["hey1", "hey2", "hey3", true, true, true, "hey1", "hey2", "hey3"]
```

#### empty

Returns an empty type of the supplied parameter:
```
const isEven = n => n % 2 === 0

R.empty(isEven(42)) //undefined
R.empty([1, 2, 3]) //[]
R.empty({name: 'Charles', surname: 'Dickens'}) //{}
R.empty('This is a string') //""
```

#### endsWith

Checks if:
- a string ends with the provided sub-string
-a list ends with the provided sub-list
```
R.endsWith('c', 'abc') //true
R.endsWith('bc', 'abc') //true
R.endsWith(['c'], ['a', 'b', 'c']) //true
R.endsWith(['b', 'c'], ['a', 'b', 'c']) //true
```

#### eqBy

Takes a function and two other parameters. The function is applied to the last two parameters, if the result is the same, `eqBy` returns true.
```
const isEven = n => n % 2 === 0
R.eqBy(isEven, 2, 4) //true
R.eqBy(isEven, 2, 5) //false
```

#### eqProps

Returns true if two objects have the same value for the specified property.
```
const o1 = { a: 1, b: 2, c: 3, d: 4 }
const o2 = { a: 10, b: 20, c: 3, d: 40 }

R.eqProps('a', o1, o2); //=> false
R.eqProps('c', o1, o2); //=> true
```
```
const arr1 = [1, 2, 3, 4, 5]
const arr2 = [1, 2, 3, 4, 4]

R.eqProps(4, arr1, arr2) //false
```

#### equals

Returns true if the two supplied parameters are equal (equivalent of `===` not `==`)
```
const arr1 = [1, 2, 3, 4, 5]
const arr2 = [1, 2, 3, 4, 4]
R.equals(arr1, arr2) //false
```
```
const o1 = { a: 1, b: 2, c: 3, d: 4 }
const o2 = { a: 10, b: 20, c: 3, d: 40 }
R.equals(o1, o2) //false
```
```
const n = 3
const number = '3'
R.equals(n, number) //false
```

#### F

A function that always returns false.
```
R.F() //false
R.F(true) //false
```

#### gt

Returns true if the first argument is greater than the second, false of otherwise.
```
R.gt(2, 1) //true
R.gt('Slayer','Sepultura') //true
```

#### gte

Returns true if the first argument is greater or equal to the second, false if otherwise.
```
R.gte(2, 1) //true
R.gte(2, 2) //true
```

#### has

Returns true if an object has the provided property, false if otherwise. Note: this function does not parse nested objects, vide the last two examples:
```
const obj = {name: 'elephant', 
			type: 'mammal', 
			origin: { continent: 'Africa', country: 'Gabon', } }

R.has('name', obj) //true
R.has('origin', obj) //true
R.has('country', obj) //false
R.has('country', obj.origin) //true
```

#### hasIn

Returns true if an object or its prototype chain has the specified property.
```
function Rectangle(width, height) {
	this.width = width
	this.height = height
}
Rectangle.prototype.area = function() {
 return this.width * this.height
}
const square = new Rectangle(2, 2)
R.hasIn('width', square) //true
R.hasIn('area', square) // true
```

#### hasPath

Checks whether an object has the specified path.
```
const obj = {name: 'elephant', 
			type: 'mammal', 
			origin: { continent: 'Africa', country: 'Gabon', } }

R.hasPath(['origin', 'country'], obj)
//true
```

#### identical

Returns true if two objects are equal.
```
const x = 7
const y = '7'
const z = x

x == y //true
x === y //false
R.identical(x, y) //false
R.identical(x, z) //true
R.equals(x, y) //false
R.equals(x, z) //true
```

#### includes

Returns true of the first parameter is present in the list in the second parameter.

```
R.includes(3, [1, 2, 3]) //true
R.includes('ap', 'apple') //true
```

#### ifElse
Checks whether the supplied parameter satisfies the first parameter, if so the second parameter function is executed, else the third parameter function is executed.
```
const f = R.ifElse(R.equals(0), R.inc, R.dec)

f(4) //3
f(0) //1
```


#### is
Kind of javascript's `typeof`.
```
const str = 'abcdef'
const n = 8
const arr = [1, 2, 3, 4, 5]
  
R.is(String, str) //true
R.is(Number, 8) //true
R.is(Array, arr) //true
```

#### isEmpty
Returns `true` if the given value is its type's empty value; `false` otherwise.  

```
R.isEmpty([1, 2, 3]) //false
R.isEmpty([]) //true
R.isEmpty('') //true
R.isEmpty(null) //false
R.isEmpty({}) //true
R.isEmpty({length: 0}) //false
```

#### isNil
Checks if the supplied value is `null` or `defined`.

```
R.isNil(null) //true
R.isNil(undefined) //true
R.isNil(0) //false
R.isNil([]) //false
```

#### lt
Returns true if the first argument is less than the second.
```
R.lt(1, 2) //true
R.lt('a', 'z') //true
```

#### lte
Returns true if the first argument is less than or equal to the second.
```
R.lte(1, 2) //true
R.lte(2, 2) //true
R.lte('ae', 'ad') //false
R.lte('ae', 'ae') //true
```

#### or
Similar to `||`.
```
R.or(true, true) //true
R.or(true, false) //true
R.or(false, true) //true
R.or(false, false) //false
```

#### propIs
Returns `true` if the specified object property is of the given type.  
```
R.propIs(Number, 'age', {name: 'Max', age: 18}) //true
R.propIs(Number, 'age', {name: 'Max', age: 'eihgteen'}) //false
```

#### startsWith
Checks if a string starts with the provided substring, or if an array starts with the provided sub-array.
```
R.startsWith('/product', '/product/categories')
//true
```
```
R.startsWith('a', ['a', 'b', 'c']) //false
R.startsWith(['a'], ['a', 'b', 'c']) //true
```

### Iterative

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
#### either

This function is both non-iterative and iterative.

This function is a wrapper to the two supplied function; it calls the first and return its result if it is true. The second function is only called if the first returns false. `either` will return true if either of the function returns true.

`either` can be used to iterate through an array and trigger functions conditionally to create a new array with the desired results.
```
const lt30 = n => n < 30  
const gt40 = n => n > 40
const f = R.either(lt30, gt40)

f(24)
//true
```
```
const f = R.either(n => R.lt(n, 30), n => R.gt(n, 40))
f(24)
//true
```

Look at the below:
```
R.either([false, false, 'a'], [11])  
//[11, 11, "a"]

R.either([isEven(1), isEven(2), isEven(3)], [11])
//[11, true, 11]

R.either([11], [isEven(1), isEven(2), isEven(3)])
//[11, 11, 11]

R.either([isEven(1), isEven(2), isEven(3)], ['hey1', 'hey2', 'hey3'])
//["hey1", "hey2", "hey3", true, true, true, "hey1", "hey2", "hey3"]
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

#### descend

Create a comparator function that returns a value that can be used with `<` and `>`.

```
const objList = [
	{ name: 'shark'},
	{ name: 'eagle'},
	{ name: 'antelope'},
]

const d = R.descend(R.prop('name'))

R.sort(d, objList)

//[{"name": "shark"}, {"name": "eagle"}, {"name": "antelope"}]
```
Not to be used with more than one parameter, it would no return the expected results:

```
const objList = [
{ name: 'shark', type: 'elephant', weight: '20'},
{ name: 'shark', type: 'white', weight: '8'},
{ name: 'eagle', type: 'american', weight: '0.004'},
{ name: 'antelope', type: 'african', weight: '0.3'},
]

const d = R.descend(R.prop('weight'), R.prop('name'))

R.sort(d, objList)

//R.descend(R.prop('name'), R.prop('weight'))
/*
[{"name": "antelope", "type": "african", "weight": "0.3"},
{"name": "eagle", "type": "american", "weight": "0.004"},
"name": "shark", "type": "elephant", "weight": "20"},
{"name": "shark", "type": "white", "weight": "8"}]
*/

//R.descend(R.prop('weight'), R.prop('name'))
/*
[{"name": "shark", "type": "elephant", "weight": "20"},
{"name": "shark", "type": "white", "weight": "8"},
{"name": "eagle", "type": "american", "weight": "0.004"},
{"name": "antelope", "type": "african", "weight": "0.3"}]
*/
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

Note: passing `true` or `false` does not trigger the default value.

```
const defaultToZero = R.defaultTo(0)

defaultToZero(null)
//0
```
```
defaultToZero(56)
//56

defaultToZero(-3 < 0)
//true

defaultToZero(-3 >0)
//false
```

#### difference

Returns the elements in the first array that are not present in the second list array:

```
const objList1 = [
	{ name: 'shark', type: 'elephant', weight: '20'},
	{ name: 'shark', type: 'white', weight: '20'},
	{ name: 'eagle', type: 'american', weight: '0.004'},
	{ name: 'antelope', type: 'african', weight: '0.3'},
]

const objList2 = [
	{ name: 'shark', type: 'elephant', weight: '20'},
	{ name: 'shark', type: 'cat', weight: '20'},
	{ name: 'eagle', type: 'american', weight: '0.004'},
	{ name: 'antelope', type: 'african', weight: '0.3'},
]

R.difference(objList1, objList2)

//[{"name": "shark", "type": "white", "weight": "20"}]
```

#### differenceWith

Checks which elements are present in the first array but not in the second. This function has more fine tuning than `difference` because it can take a comparative function that dictates what the comparison should actually be. This time round the same arrays used with the example in `difference` are being used, but the comparison function is checking only for the 'name' property; the other properties are not taken into account. In this case, no difference is found.

```
const objList1 = [
	{ name: 'shark', type: 'elephant', weight: '20'},
	{ name: 'shark', type: 'white', weight: '20'},
	{ name: 'eagle', type: 'american', weight: '0.004'},
	{ name: 'antelope', type: 'african', weight: '0.3'},
]

const objList2 = [
	{ name: 'shark', type: 'elephant', weight: '20'},

	{ name: 'shark', type: 'cat', weight: '20'},

	{ name: 'eagle', type: 'american', weight: '0.004'},

	{ name: 'antelope', type: 'african', weight: '0.3'},
]

const cmp = (x, y) =>  [x.name](http://x.name/)  ===  [y.name](http://y.name/)

R.difference(objList1, objList2)

R.differenceWith(cmp, objList1, objList2)

//[]
```

#### dissoc 

Returns a new object that does not contain the indicated property.

```
const obj = { a: 1, b: 2, c: 3}

R.dissoc('b', obj)

//{"a": 1, "c": 3}
```
It does not work on arrays.

Note that it cannot be used to remove nested properties, in that case use `dissocPath`.


#### dissocPath

Returns a new object that does not contain the indicated property, even if it is nested.

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

R.dissocPath(['el', 'type'], objList)

//{"ea": {"name": "eagle", "origin": {"continent": "USA", "country": "Arizona"}, "type": "bird"},
//"el": {"name": "elephant", "origin": {"continent": "Africa", "country": "Gabon"}}, 
//"sk": {"name": "shark", "origin": {"continent": "Australia", "country": "Sydney"}, "type": "fish"}}
```

#### divide

Returns a function that divides two numbers, equivalent to `a / b`.

```
R.divide(9, 3)
//3

R.divide(45, 14)
//3.2142857142857144

const div = R.divide()
div(8, 2)
//4
```

#### drop

Returns all but the first `n` elements of the given list.

```
R.drop(1, ['foo', 'bar', 'baz']) //['bar', 'baz']
R.drop(2, ['foo', 'bar', 'baz']) //['baz']
R.drop(3, ['foo', 'bar', 'baz']) //[]
R.drop(4, ['foo', 'bar', 'baz']) //[]
R.drop(3, 'ramda') //'da'
```

#### dropLast

Returns a list containing all but the last `n` elements of the given `list`.  

```
R.dropLast(1, ['foo', 'bar', 'baz']) //['foo', 'bar']
R.dropLast(2, ['foo', 'bar', 'baz']) //['foo']
R.dropLast(3, ['foo', 'bar', 'baz']) //[]
R.dropLast(4, ['foo', 'bar', 'baz']) //[]
R.dropLast(3, 'ramda') //'ra'
```


#### dropLastWhile

Returns a new list trimmed from the original with the supplied predicate function. The function is applied from the last element in the list and moves left, until it returns false.

```
const equals3 = n => n === 3

R.dropLastWhile(equals3, [1, 2, 3, 3, 4, 3, 3, 5])
//[1, 2, 3, 3, 4, 3, 3, 5]

R.dropLastWhile(equals3, [1, 2, 3, 3, 4, 3, 3])
//[1, 2, 3, 3, 4]

R.dropLastWhile(equals3, [1, 2, 3, 3, 4, 3])
//[1, 2, 3, 3, 4]

R.dropLastWhile(equals3, [1, 2, 3, 3, 4])
//[1, 2, 3, 3, 4]

R.dropLastWhile(equals3, [1, 2, 3, 3])
//[1, 2]
```

 
#### dropRepeats

Returns a list without any consecutively repeating elements.

```
const arr = [1, 2, 2, 3, 3, 3, 4, 4, 4, 4, 3, 3]

R.dropRepeats(arr)
//[1, 2, 3, 4, 3]
```


#### dropRepeatsWith

Returns a trimmed version of the supplied list by applying the supplied predicate function from left to right through the list, to return a list without consecutively repeating elements. It is the first element that satisfies uniqueness that is preserved, so the position of elements in the list is very important.

```
const arr = [1, -1, 1, 3, 4, -4, -4, -5, 5, 3, 3]

R.dropRepeatsWith(R.eqBy(Math.abs), arr)
//[1, 3, 4, -5, 3]
```
```
const letters = ['a', 'A', 'B', 'b', 'C', 'c', 'a', 'A']

R.dropRepeatsWith(R.eqBy(R.toUpper), letters)

//["a", "B", "C", "a"]
```

  
#### dropWhile

Parses a list from left to right and stops as soon as the predicate function returns false. It drops all the elements that return true.

```
const lteTwo = x => x <= 2

R.dropWhile(lteTwo, [1, 2, 3, 4, 3, 2, 1])
//[3, 4, 3, 2, 1]
```
  
#### evolve

What a powerful function. It applies a set of functions to specific properties of an object.

```
const data = {name: "stephen", balance: {starting: 1000, closing: 2000}, bank: "APS"}
const transformations = {  
		name: R.toUpper,
		surname: R.toUpper, //will not be invoked
		balance: { starting: n => R.subtract(800, n),
		closing: R.add(2000)
	},
}

R.evolve(transformations, data)  
//{"balance": {"closing": 4000, "starting": -200}, "bank": "APS", "name": "STEPHEN"}
```
 

#### filter

Filters a list with the provided predicate function.
```
const arr = [1, 2, 3, 4]
const gt2 = n => R.gt(n, 2)

R.filter(gt2, arr)
//[3, 4]
```

```
const list = {a: 1, b: 2, c: 3, d: 4}
const gt2 = n => R.gt(n, 2)
R.filter(gt2, list)
//{"c": 3, "d": 4}
```

#### find

Returns the first element in the list that satisfies the predicate condition.  

```
const list = [{index: 1}, {index: 2}, {store: 3}, {store: 4}]

R.find(R.propEq('index', 2))(list)
//{"index": 2}
```

#### findLast

Returns the last element in the list that satisfies the predicate condition.
```
const list = [{name: 'Albert', grade: 'A', marks: 100},
	{name: 'Nikola', grade: 'A', marks: 99},
	{name: 'Edison', grade: 'B', marks: 80}]
R.findLast(R.propEq('grade', 'A'))(list)
//{"grade": "A", "marks": 99, "name": "Nikola"}
```

#### R.findIndex

Returns the index of the first element in the list that matches the predicate. If no element matches, `-1` is returned.
```
const arr = [{a: 1}, {b: 2}, {b: 3}]
R.findIndex(R.prop('b'), arr) //1
R.findIndex(R.prop('c'), arr) //-1
R.findIndex(R.propEq('b', 3), arr) //1
```

```
const arr = [1, 2, 3]
R.findIndex(R.equals(3), arr) //2
```

#### findLastIndex

Returns the index of the last element in the list that matches the predicate. If no element matches, '-1' is returned.

```
const arr = [{a: 1}, {b: 2}, {b: 3}]
R.findLastIndex(R.prop('b'), arr) //2
R.findIndex(R.prop('c'), arr) //-1
R.findIndex(R.propEq('b', 3), arr) //2
```

#### R.flatten

Flattens an array with nested arrays, just like the spread operator.
```
R.flatten([1, 2, [3, 4], 5, [6, [7, 8, [9, [10, 11], 12]]]])
//[1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]
```


#### flip

Swaps the first two parameters of the supplied function.
```
const concatNums = (x, y, z) => [x, y, z]
R.flip(concatNums)(1, 2, 3)
//[2, 1, 3]
```

#### forEach

Applies the supplied function to each element in the provided list. The original array is returned.
```
const arr = [1, 2, 3]
R.forEach(R.inc, arr) //[1, 2, 3] //the original array is returned
```
```
R.forEach(n => console.log(R.inc(n)), arr)
//2
//3
//4
//[1, 2, 3]
```
```
const arr = [1, 2, 3]
const x2 = n => console.log(n * 2)
R.forEach(x2, arr)
//2
//4
//6
//[1, 2, 3]
```

#### forEachObjIndexed

Iterates over an object, calling the provided function over each key and value.
```
const obj = {brand: 'metabo', COO: 'Germany'}
const print = (key, value) => console.log(key + ': ' + value )

R.forEachObjIndexed(print, obj)
//metabo: brand
//Germany: COO
//{"COO": "Germany", "brand": "metabo"}
```

#### fromPairs

Returns an object from the supplied list of keys and values.
```
const arr = [ ['a', 1], ['b', 2], ['c', 3] ]
R.fromPairs(arr)
//{"a": 1, "b": 2, "c": 3}
```

#### groupBy

Categorises a list into arrays based on the supplied condition.

```
const students = [{name: 'Abby', score: 84},
	{name: 'Eddy', score: 84},
	{name: 'Jack', score: 69}]

const byGrade = R.groupBy(function(student) {
	const score = student.score
		return score < 65 ? 'F' :
		score < 70 ? 'D' :
		score < 80 ? 'C' :
		score < 90 ? 'B' : 'A'
})

R.byGrade(students)
// {"B": [{"name": "Abby", "score": 84}, {"name": "Eddy", "score": 84}],
//"D": [{"name": "Jack", "score": 69}]}
```

#### groupWith

Caution, this function does not run the supplied function on every element in the list, it runs the function on consecutive pairs in the list. The function takes two of the elements each time.
```
const arr = [2, 2, 2, 2, 2, 1, 0]
R.groupWith(R.equals, arr)
//[[2, 2, 2, 2, 2], [1], [0]]
```
```
const arr = [1, 2, 1, 3, 2, 4, 3, 5]
R.groupWith(R.gt, arr)
//[[1], [2, 1], [3, 2], [4, 3], [5]]
```

#### head

Returns the first element from an array or string. Note that it does not work on an object.
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

R.head(objList) 
//undefined

R.head(R.values(objList))
//{"name": "elephant", "origin": {"continent": "Africa", "country": "Gabon"}, "type": "mammal"}
```
```
const str = 'Motorhead'
R.head(str)
//"M"
```


#### identity

Simply returns the parameter supplied to it, does nothing else. Good as placeholder.

```
const arr = [1, 2, 3] 
R.identity(arr)
//[1, 2, 3]
```
```
const f = n => n * 2
const g = R.identity(f)
g(2)
//4
```


#### inc

Increments the supplied parameter.

```
R.inc(4) //5  
```
   
#### indexBy

Turns a list of objects into an object indexing the objects with the given key.

```
const arr = [{id: 1, name: 'apple'}, {id: 2, name: 'peach'}]

R.indexBy(R.prop('id'), arr)
//{"1": {"id": 1, "name": "apple"}, "2": {"id": 2, "name": "peach"}}
```

#### indexOf
Returns the location of the element being searched. Does not work on object literals.

```
const arr = [1, 2, 3]
R.indexOf('3', arr)
//2

const arr = [1, 2, 3]
R.indexOf('3', arr)
//-1
```

#### init
Returns all but the last element of a list. Does not work on object literals.
```
const arr = [1, 2, 3]

R.init(arr)
//[1, 2]
```

#### innerJoin
Takes a predicate function and two lists. The predicate function is used as a check to compare records.

```
R.innerJoin(
(record, id) => record.id === id,
	[{id: 824, name: 'Richie Furay'},
	{id: 956, name: 'Dewey Martin'},
	{id: 313, name: 'Bruce Palmer'},
	{id: 456, name: 'Stephen Stills'},
	{id: 177, name: 'Neil Young'}],
	[177, 456, 999]
)
//[{id: 456, name: 'Stephen Stills'}, {id: 177, name: 'Neil Young'}]
```
```
R.innerJoin(
	(record, id) => record.id < id,
	[{id: 824, name: 'Richie Furay'},
	{id: 956, name: 'Dewey Martin'},
	{id: 313, name: 'Bruce Palmer'},
	{id: 456, name: 'Stephen Stills'},
	{id: 177, name: 'Neil Young'}],
	[200]
)
//[{"id": 177, "name": "Neil Young"}]
```

#### insert

Inserts the provided element in the specified position of the supplied list.

```

const arr = ['a', 'b', 'd']

R.insert(2, 'c', arr)

//["a", "b", "c", "d"]

```

#### insertAll
Inserts the provided list in the specified position of the supplied list.  

```
const arr = ['a', 'b', 'c']

R.insertAll(2, ['d', 'e', 'f'], arr)
//["a", "b", "d", "e", "f", "c"]
```

#### intersection
Combines the common elements from two lists into a new list.

```
const arr1 = ['a', 'b', 'c']
const arr2 = ['d', 'e', 'f']

R.intersection(arr1, arr2)
//[]
```
```
const arr1 = ['a', 'b', 'c', 'd', 'e']
const arr2 = ['d', 'e', 'f']
R.intersection(arr1, arr2)
//["d", "e"]
```

#### intersperse

Takes a list and inserts the provided element between each element of the list.
```
R.intersperse('a', ['b', 'n', 'n', 's'])
//['b', 'a', 'n', 'a', 'n', 'a', 's']
```

#### into
Takes three parameters:
- a destination list
- a transducer function
- a list on which the transducer will be applied

It will append the transformed list to the destination list.

```
const arr1 = [1, 2, 3, 4]
const arr2 = [4, 5, 6, 7]
const transducer = R.map(R.inc())

R.into(arr1, transducer, arr2)
//[1, 2, 3, 4, 5, 6, 7, 8]
```

#### invert
Same as `invertObj` however it puts duplicates into an array.

```
const raceResults = {
	first: 'alice',
	second: 'jake'
}

R.invertObj(raceResults);
// { 'alice': 'first', 'jake':'second' }

// Alternatively:
const raceResults = ['alice', 'jake']

R.invertObj(raceResults)
// { 'alice': '0', 'jake':'1' }
```

#### invertObj
Swaps keys with values in an object.

```
const obj = { name: 'Nikola', surname: 'Tesla' }

R.invertObj(obj)
//{"Nikola": "name", "Tesla": "surname"}
```

#### join
Returns a string interspersed with the provided separator.
```
const arr = [1, 2, 3, 4]

R.join('|', arr)
//"1|2|3|4"
```

#### keys
Returns an array with the keys of an object.
```
R.keys({a: 1, b: 2, c: 3})
//['a', 'b', 'c']
```

#### keysIn
Returns a list containing the names of all the properties of the supplied object, including prototype properties.  
```
const F = function() { this.x = 'X' }
F.prototype.y = 'Y'
const f = new F()

R.keysIn(f)
// ['x', 'y']
```

#### last
Returns the last element of a given list.
```
const arr = [1, 2, 3]
R.last(arr)
//3

const str = 'The Greate Seige'
R.last(str)
//"e"
```

#### lastIndexOf
Returns the index of the last position of an occurrence of a given element in a list.
```
const arr = [1, 2, 3, 1, 2, 3]
R.lastIndexOf(2, arr)
//4

const str = 'The Greate Seige'
R.lastIndexOf('e', str)
//15
```

#### length
Return the length of a list.
```
const arr = [1, 2, 3, 1, 2, 3]
R.length(arr)
//6

const str = 'The Greate Seige'
R.length(str)
//16
```

#### lens
Create a lens to be used to focus on a particular location in an object literal/array.  
```
const xLens = R.lens(R.prop('x'), R.assoc('x'))

R.view(xLens, {x: 1, y: 2}) // 1
R.set(xLens, 4, {x: 1, y: 2}) // {x: 4, y: 2}
R.over(xLens, R.negate, {x: 1, y: 2}) // {x: -1, y: 2}
```
```
const arr = [1, 2, 3]
const xLens = R.lens(R.prop(0), R.assoc(0))

R.view(xLens, arr)
//1
```

#### lensIndex
Returns a lens whose focus is the specified index. Does not work on object literals.
```
const headLens = R.lensIndex(0)

R.view(headLens, ['a', 'b', 'c']) // 'a'
R.set(headLens, 'x', ['a', 'b', 'c']) // ['x', 'b', 'c']
R.over(headLens, R.toUpper, ['a', 'b', 'c']); // ['A', 'b', 'c']
```

#### lensPath
Returns a lens whose focus is the specified path.

```
const xHeadYLens = R.lensPath(['x', 0, 'y'])

R.view(xHeadYLens, {x: [{y: 2, z: 3}, {y: 4, z: 5}]})
// 2

R.set(xHeadYLens, 1, {x: [{y: 2, z: 3}, {y: 4, z: 5}]})
// {x: [{y: 1, z: 3}, {y: 4, z: 5}]}

R.over(xHeadYLens, R.negate, {x: [{y: 2, z: 3}, {y: 4, z: 5}]})
// {x: [{y: -2, z: 3}, {y: 4, z: 5}]}
```
```
const arr = [ [1, 2], [3, 4], [5, 6] ]
const myLens = R.lensPath([1, 1])

R.view(myLens, arr)
// 4
```

#### lensProp
Returns a lens to the specified prop.
```
const xLens = R.lensProp('x')

R.view(xLens, {x: 1, y: 2}) // 1
R.set(xLens, 4, {x: 1, y: 2}) // {x: 4, y: 2}
R.over(xLens, R.negate, {x: 1, y: 2}) // {x: -1, y: 2}
```

#### lift
Takes the first parameter and maps it against all possible combinations of the other parameters.

So in the example below, the first element of the first parameter, is mapped against all combinations of the second parameter (1 + 1, 1 + 2, 1 + 3, and then, 2 + 1, 2 + 2, etc)
```
const madd2 = R.lift((a, b) => a + b)
madd2([1,2,3], [1,2,3])
// [2, 3, 4, 3, 4, 5, 4, 5, 6]  
```
```
const madd3 = R.lift((a, b, c) => a + b + c)
madd3([1,2,3], [1,2,3], [1])
// [3, 4, 5, 4, 5, 6, 5, 6, 7]
```
```
const arr = ['a', 'b', 'c']
const combine = R.lift((a, b) => a + b)

combine(arr, arr)
// ["aa", "ab", "ac", "ba", "bb", "bc", "ca", "cb", "cc"]
```

#### liftN
Lifts a function to the specified arity.
```
const madd3 = R.liftN(3, (...args) => R.sum(args))

madd3([1,2,3], [1,2,3], [1])
//[3, 4, 5, 4, 5, 6, 5, 6, 7]
```

#### map
Applies the function to each element of the provided array or object.
```
const double = n => n * 2
const arr = [1, 2, 3]

R.map(double, arr)
//[2, 4, 6]

const obj = { a: 1, b: 2, c : 3}
R.map(double, obj)
//{"a": 2, "b": 4, "c": 6}

const obj2 = { a: [1, 2], b: [3, 4]}

R.map(double, obj2)
//{"a": NaN, "b": NaN}
```

#### mapAccum
Applies a function from left to right, passing an accumulating parameter from left to right. It returns the final value of this accumulation, together with the resulting list.
```
const arr = ['1', '2', '3', '4']
const appender = (a, b) => [a + b, a + b]

R.mapAccum(appender, 0, arr)  
//["01234", ["01", "012", "0123", "01234"]]

R.mapAccum(appender, 0, arr)[1]  
//["01", "012", "0123", "01234"]
```

#### mapAccumRight
Similar to `mapAccum`, except it traverses from right to left.
```
const arr = ['1', '2', '3', '4']
const appender = (a, b) => [a + b, a + b]

R.mapAccumRight(appender, 0, arr)
//["04321", "0432", "043", "04"]
```

#### mapObjectIndex
Similar to `map`, however the function is applied to three parameters: `value`, `key`, and `object`.

Note that in the below example, the object parameter is not being consumed; the function will still work if this parameter was not passed.

```
const xyz = { x: 1, y: 2, z: 3 }
const prependKeyAndDouble = (num, key, obj) => key + (num * 2)

R.mapObjIndexed(prependKeyAndDouble, xyz)
// {"x": "x2", "y": "y4", "z": "z6"}
```

#### match
Tests a regular expression against a string. Returns an empty array when there is no match.
```
R.match(/([a-z]a)/g, 'bananas')
//['ba', 'na', 'na']
```

#### mathMod
Behaves like the `%` should behave mathematically, so, where `-17 % 5 = -2`, `R.mathMod(-17, 5) = 3.
```
R.mathMod(-17, 5) // 3
R.mathMod(17, 5) // 2
```

#### max
Returns the maximum of the two provided parameters.
```
R.max(342, 532)
//532

R.max('a', 'b')
//"b"
```

#### maxBy
Takes a function and two parameters, applies the function to both parameters, and returns the maximum value of the results.
```
const square = n => n * n
R.maxBy(square, 3, -4)
//-4
```

#### mean
Returns the mean of the numbers in the provided array.
```
R.mean([1, 2, 3, 4])
//2.5
```

#### median
Returns the median of the given list of numbers.
```
R.median([1, 2, 3, 4, 5, 6, 19, 104])
//4.5
```

#### memoizeWith
Caches the result of a function with a specific set of parameter values. When the function has the exact same parameters, it is not run again, but rather the result is stored in cache.
```
let count = 0
const factorial = R.memoizeWith(R.identity, n => {
	count += 1
	return R.product(R.range(1, n + 1))
})

factorial(5) // 120
factorial(5) // 120
factorial(5) // 120
count //=> 1
```

#### mergeAll
Merges a list of objects into one object. If a property exists in multiple objects, the parameters to the right take precedence. The objects must be supplied in an array.
```
const obj1 = {a: 1, b: 1}
const obj2 = {a: 1, b: 2}
const obj3 = {c: 3, d: 4}

R.mergeAll([obj1, obj2])
//{"a": 1, "b": 2}

R.mergeAll([obj1, obj3])
//{"a": 1, "b": 1, "c": 3, "d": 4}

R.mergeAll([obj1, obj2, obj3])
//{"a": 1, "b": 2, "c": 3, "d": 4}
```

#### mergeDeepLeft
Recursive merging of objects with nested objects. The first object takes precedence.

```
R.mergeDeepLeft({ name: 'fred', age: 10, contact: { email: '[moo@example.com](mailto:moo@example.com)' }},
{ age: 40, contact: { email: '[baa@example.com](mailto:baa@example.com)' }})
// { name: 'fred', age: 10, contact: { email: '[moo@example.com](mailto:moo@example.com)' }}
```

#### mergeDeepRight
Recursive merging of objects with nested objects. The second object takes precedence.  

```
R.mergeDeepRight({ name: 'fred', age: 10, contact: { email: '[moo@example.com](mailto:moo@example.com)' }},
{ age: 40, contact: { email: '[baa@example.com](mailto:baa@example.com)' }})
// { name: 'fred', age: 40, contact: { email: '[baa@example.com](mailto:baa@example.com)' }}
```

#### mergeDeepWith
Creates a new object with the properties of both objects. If a property exists in both objects, the provided function is applied to the respective values.

```
R.mergeDeepWith(R.concat,
{ a: true, c: { values: [10, 20] }},
{ b: true, c: { values: [15, 35] }})
//{ a: true, b: true, c: { values: [10, 20, 15, 35] }}
```

#### mergeDeepWithKey
Creates a new object with the properties of both objects. If a property exists in both objects, the provided function is applied to the respective values. Has the option to target a specific key.  
```
let concatValues = (k, l, r) => k == 'values' ? R.concat(l, r) : r

R.mergeDeepWithKey(concatValues,
{ a: true, c: { thing: 'foo', values: [10, 20] }},
{ b: true, c: { thing: 'bar', values: [15, 35] }})
//{ a: true, b: true, c: { thing: 'bar', values: [10, 20, 15, 35] }}
```

#### mergeLeft
If properties are common in both object, the object on the left takes precedence.

```
R.mergeLeft({ 'age': 40 }, { 'name': 'fred', 'age': 10 })
//{ 'name': 'fred', 'age': 40 }

const resetToDefault = R.mergeLeft({x: 0})
resetToDefault({x: 5, y: 2})
//{x: 0, y: 2}
```

#### R.mergeRight

If properties are common in both object, the object on the right takes precedence.  

```
R.mergeRight({ 'age': 40 }, { 'name': 'fred', 'age': 10 })
//{ 'name': 'fred', 'age': 10 }

const resetToDefault = R.mergeRight({x: 0})
resetToDefault({x: 5, y: 2})
//{x: 5, y: 2}
```

#### mergeWith
If a property is common in both objects, the provided function will be used to merge the values.
```
R.mergeWith(R.concat,
{ a: true, values: [10, 20] },
{ b: true, values: [15, 35] })
//{ a: true, b: true, values: [10, 20, 15, 35] }
```

#### mergeWithKey
A fine-tuned version of `mergeWith`, it allows to target a specific property.
```
let concatValues = (k, l, r) => k == 'values' ? R.concat(l, r) : r

R.mergeWithKey(concatValues,
{ a: true, thing: 'foo', values: [10, 20] },
{ b: true, thing: 'bar', values: [15, 35] })
//{ a: true, b: true, thing: 'bar', values: [10, 20, 15, 35] }
```

#### min
Returns the minimum of two values.
```
R.min(1, 2)
//1
```

#### minBy
Returns the parameter which results in the minimum value when the provided function is applied.
```
const square = n => n * n
R.minBy(square, -4, 3)
//3
```
```
R.reduce(R.minBy(square), -3, [-2, 3, -4, 5, -6])
//-2
```

#### modulo
The same functionality as Javascript's `%`
```
R.modulo(17, 3) //2
R.modulo(-17, 3) //-2
R.modulo(17, -3) //2
```
```
const isOdd = R.modulo(R.__, 2)
isOdd(42) //0
isOdd(21) //1
```

#### move
Move an element from one index to another.
```
const arr = [1, 2, 3]
R.move(0, 2, arr)
//[2, 3, 1]
```
```
const arr = [1, 2, 3]
R.move(-1, 0, arr)
//[3, 1, 2]
```

#### multiply
Multiplies two numbers, this is a curried function.
```
R.multiply(3, 4)
//12
```
```
const triple = R.multiply(3)
triple(4)
//12
```

#### nAry
Wraps a function of any arity (including nullary) in a function that accepts exactly n parameters.  
```
const takesTwoArgs = (a, b) => [a, b]
takesTwoArgs.length //2
takesTwoArgs(1, 2)
//[1, 2]

const takesOneArg = R.nAry(1, takesTwoArgs)
takesOneArg.length //1
takesOneArg(1, 2)
//[1, undefined]
```

#### negate
Negates the provided argument.
```
R.negate(83)//-83
R.negate(true)//-1
R.negate(false) //-0
```

#### none
Returns true of no elements in a list match the supplied function.
```
const isEven = n => n % 2 === 0
R.none(isEven, [2, 4, 6, 8]) //false
R.none(isEven, [1, 3, 5, 7]) //true
R.none(isEven, [1, 3, 5, 2]) //fale
```

#### not
Returns the `!` of its argument.
```
R.not(true) //false
R.not(false) //true
R.not(0) //true
R.not(3) //false
```

#### nth
Returns the nth element of a given list.
```
const arr = [1, 2, 3, 4, 5, 6, 7]

R.nth(0, arr) //1
R.nth(-1, arr) //7
R.nth(-2, arr) //6
```

#### nthArg
Returns the nth of its arguments.
```
R.nthArg(2)(1, 2, 3)
//3

const f = R.nthArg(2)
f(1, 2, 3)
//3
```

#### o
`o` is a curried composition function that returns a unary function. Like compose, `o` performs right-to-left function composition. Unlike compose, the rightmost function passed to `o` will be invoked with only one argument. Also, unlike compose, `o` is limited to accepting only 2 unary functions.  
```
const classyGreeting = name => "The name's " + name.last + ", " + name.first + " " + name.last
const yellGreeting = R.o(R.toUpper, classyGreeting)

yellGreeting({first: 'James', last: 'Bond'})
//"THE NAME'S BOND, JAMES BOND"
```
```
R.o(R.multiply(10), R.add(10))(-4)
//60
```

#### objOf
Takes two parameters to use as key and value to create an object.
```
R.objOf("name", "Marcus")
//{"name": "Marcus"}
```
```
const names = ["Paul", "John", "George", "Ringo"]
R.map(R.objOf("name"), names)
//[{"name": "Paul"}, {"name": "John"}, {"name": "George"}, {"name": "Ringo"}]
```
```
const names = ["Paul", "John", "George", "Ringo"]
R.compose(R.objOf("Beatles"), R.map(R.objOf("name")))(names)
//{"Beatles": [{"name": "Paul"}, {"name": "John"}, {"name": "George"}, {"name": "Ringo"}]}
```
```
const names = ["Paul", "John", "George", "Ringo"]
const formBand = R.compose(R.objOf("Beatles"), R.map(R.objOf("name")))
formBand(names)

//{"Beatles": [{"name": "Paul"}, {"name": "John"}, {"name": "George"}, {"name": "Ringo"}]}
```

#### of
Takes only one parameter and returns a singleton array.
```
R.of(23) //[23]
R.of(23, 24) //[23]
R.of("React") //["React"]
```

#### omit
Returns a partial copy of an object less the specified properties.
```
R.omit(['a', 'd'], {a: 1, b: 2, c: 3, d: 4})
//{"b": 2, "c": 3}
```

#### once
Executes a function just once, no matter how many times it is called. The first returned value is returned for any subsequent calls.
```
const double = R.once(n => n * 2)
double(2) //4
double(2) //4
```

#### over
Used with a lens operation to set values.
```
const arr = ['a', 'b', 'c']
const midLens = R.lensIndex(1)

R.over(midLens, R.toUpper, arr)
//["a", "B", "c"]
```

#### pair
Takes two arguments and puts them in an array.
```
const obj = { brand: "oneplus", model: "one" }
R.pair(obj.brand, obj.model)
//["oneplus", "one"]
```

#### partial
Takes a function `f` and a list of parameters, and returns a function `g`. The returned function `g` will first make use of the parameters supplied to it in its definition, if these are not as much as the arity of `f`, it will take as much as parameters as required from those supplied to `g`.
```
const f = (x, y, z) => x + y + z
const g = R.partial(f, [1, 2])
g(4)
//7

const h = R.partial(f, [1, 2, 3])
h(4)
//6 (the parameter supplied to 'h' is ignored)

const i = R.partial(f, [1])
i(4, 5)
//10

const j = R.partial(f, [])
j(4, 5, 6)
//15
```
#### partialRight
Same as `partial`, however the parameters supplied to the function returned from `partialRight` are consumed first.

```
const f = (x, y, z) => x + y + z
const g = R.partialRight(f, [1, 2])
g(4)
//7

const h = R.partialRight(f, [1, 2, 3])
h(4)
//7 ('3' is ignored)

const i = R.partialRight(f, [1])
i(4, 5)
//10

const j = R.partialRight(f, [1, 2, 3])
j(4, 5, 6)
//15
```

#### partition
Applies the provided function to the provided list and returns two lists: one with a list that satisfies the function, the other with a list that does not satisfy the function. The list supplied can be wither an array or an object.
```
const isEven = n => n % 2 === 0
R.partition(isEven, [1, 2, 3, 4, 5, 6])
//[[2, 4, 6], [1, 3, 5]]
```
```
const isEven = n => n % 2 === 0
R.partition(isEven, { one: 1, two: 2, three: 3, four: 4, five: 5, six: 6})
//[{"four": 4, "six": 6, "two": 2}, {"five": 5, "one": 1, "three": 3}]
```

#### path
Takes a path and an array or object, and returns the element/property and the path's location.
```
R.path(['nest1', 'nest2'], { nest1: {nest2: 'hello'}}) //"hello"
R.path([3, 1], [ [1, 2], [3, 4], [5, 6], [7, 8] ]) //8
```

#### pathEq
Takes a path and a value, and returns a function that will determine if the element/property at the supplied path is equal to the value provided.
```
const user1 = { address: { zipCode: 90210 } }
const user2 = { address: { zipCode: 55555 } }
const user3 = { name: 'Bob' }
const users = [ user1, user2, user3 ]
const isFamous = R.pathEq(['address', 'zipCode'], 90210)

R.filter(isFamous, users)
//[{"address": {"zipCode": 90210}}]
```

#### pathOr
If the given, non-null object has a value at the given path, returns the value at that path. Otherwise returns the provided default value.  
```
R.pathOr('N/A', ['a', 'b'], {a: {b: 2}}) //2
R.pathOr('N/A', ['a', 'b'], {c: {b: 2}}) //"N/A"
```

#### pathSatsifies
Returns true if the value at the specified path satisfies the given predicate.
```
const isAdult = n => n >= 18

R.pathSatisfies(isAdult, ['player', 'age'], { player: {age: 23 }})
//true
```

#### pick
Returns the specified properties/elements from an object/array into a new object.
```
const obj = { name: 'James', surname: 'Hetfield', band: 'Metallica', age: 58}

R.pick(['name', 'surname'], obj)
//{"name": "James", "surname": "Hetfield"}
```
```
const arr = [1, 2, 3, 4, 5, 6]

R.pick([0, 3], arr)
//{"0": 1, "3": 4}
```

#### pickAll
Similar to `pick` but this one returns `undefined` for properties that do not exist.
```
const obj = { name: 'James', surname: 'Hetfield', band: 'Metallica', age: 58}

R.pickAll(['name', 'surname', 'previousBand'], obj)
//{"name": "James", "previousBand": undefined, "surname": "Hetfield"}
```
```
const arr = [1, 2, 3, 4, 5, 6]

R.pickAll([0, 7], arr)
//{"0": 1, "3": 4}
```  

#### pickBy
Returns an object with the properties/elements that satisfy the predicate.
```
const isAdult = player => player.age >= 18
const players = [ {name: 'John', age: 17}, {name: 'Paul', age: 21} ]

R.pickBy(isAdult, players)
//{"1": {"age": 21, "name": "Paul"}}
```
```
const isEven = n => n % 2 === 0
const arr = [1, 2, 3, 4, 5, 6]

R.pickBy(isEven, arr)
//{"1": 2, "3": 4, "5": 6}
```

#### pipe
Performs left-to-right function composition. The leftmost function may have any arity; the remaining functions must be unary.

```
const f = R.pipe(Math.pow, R.negate, R.inc);

f(3, 4)
// -80
```

#### pluck
Returns a new array/object by extracting the specified element/property from the supplied list.
```
const getAges = R.pluck('age')

getAges([{name: 'fred', age: 29}, {name: 'wilma', age: 27}])
//[29, 27]
```  
```  
R.pluck(0, [[1, 2], [3, 4]])
//[1, 3]
```  
```  
R.pluck('val', {a: {val: 3}, b: {val: 5}})
//{"a": 3, "b": 5}
```

#### prepend
Prepends the given list with the element provided.
```
R.prepend(0, [1, 2, 3])
//[0, 1, 2, 3]
```

#### product
Multiplies all the elements in the provided array.
```
R.product([5, 4, 3, 2, 1])
//120
```

#### prop
Returns a functions that gets the indicated property.
```
R.prop(1, [0, 1, 2])
//1
```
```
R.prop('name', {name: 'Max', surname: 'Cavalera'})
//Max
```
```
const obj = {name: 'Max', surname: 'Cavalera'}
const getName = R.prop('name')

getName(obj)
//"Max"
```

#### R.propEq
Returns `true` if the property/element at the indicated location is equal to the second parameter.
```
R.propEq(1, 1, [0, 1, 2])
//true
```
```
const obj = {name: 'Max', surname: 'Cavalera'}
const checkName = R.propEq('name', 'Max')
checkName(obj)
//true
```
```
const abby = {name: 'Abby', age: 7, hair: 'blond'}
const fred = {name: 'Fred', age: 12, hair: 'brown'}
const rusty = {name: 'Rusty', age: 10, hair: 'brown'}
const alois = {name: 'Alois', age: 15, disposition: 'surly'}
const kids = [abby, fred, rusty, alois]
const hasBrownHair = R.propEq('hair', 'brown')

R.pluck('name', R.filter(hasBrownHair, kids))
//["Fred", "Rusty"]
```

#### propOr
Returns the first parameter if the property indicated by the second parameter is not found in the third parameter.
```
const obj = { name: 'Max', surname: undefined }

R.propOr('Cavalera', 'surname', obj)
//"Cavalera"
```

#### props
Returns multiple properties.
```
const obj = {name: 'Max', surname: 'Cavalera', age: '52', country: 'Brazil'}
const getFullName = R.props(['name', 'surname'])

R.pipe(getFullName, R.join(' '))(obj)
//"Max Cavalera"
```
```
const arr = [1, 2, 3, 4]

R.pipe(R.props([0, 3]), R.sum)(arr)
//5
```

#### propSatisfies
Returns `true` if the indicated property satisfies the predicate.
```
const student = { name: 'Simon', age: 23 }
const isAdult = n => n >= 18

R.propSatisfies(isAdult, 'age', student)
//true
```
```
const student = { name: 'Simon', age: 23 }
const isAdult = R.propSatisfies(n => n >= 18, 'age')

isAdult(student)
//true
```

#### range
Returns a list of numbers from (included) to (excluded).
```
R.range(0, 7)
//[0, 1, 2, 3, 4, 5, 6]
```

#### reduce
Takes a function, a starting value (2nd parameter), and a list, and applies the function to the starting parameter and the first element in the list, and applies function to the result and the second parameter in the list, and iterates likewise through all the list.
```
R.reduce(R.multiply, 2, [1, 2, 3, 4])
//48
```

#### reduceBy
This is a more refined `groupBy` function. It groups the elements of a list according to the variable specified in the third parameter.
```
const groupNames = (acc, {name}) => acc.concat(name)

const toGrade = ({score}) =>
score < 65 ? 'F' :
score < 70 ? 'D' :
score < 80 ? 'C' :
score < 90 ? 'B' : 'A'

var students = [
	{name: 'Abby', score: 83},
	{name: 'Bart', score: 62},
	{name: 'Curt', score: 88},
	{name: 'Dora', score: 92},
]

reduceBy(groupNames, [], toGrade, students)
//{"A": ["Dora"], "B": ["Abby", "Curt"], "F": ["Bart"]}
```

#### reduced
Stops the reducing operation once a condition is satisfied. In the below example, the iteration continues (appending the result of the function to the supplied empty array, 'acc' being the empty array, 'item' being the supplied list) until the item is greater than 3 and the iteration stops.
```
R.reduce(
(acc, item) => item > 3 ? R.reduced(acc) : acc.concat(item),
[],
[1, 2, 3, 4, 5, 6])
//[1, 2, 3]
```

#### reduceRight
Similar to `reduce` except it moves from right to left.
```
R.reduceRight(R.subtract, 0, [1, 2, 3, 4])
//-2
```

#### reduceWhile
Performs a reduce operation while a condition is `true`. In the below example, the elements of the list are added together and the resulting sum added to the next element in the list, until this sum is less than 6.
```
const arr = [1, 2, 3, 4, 5, 6, 7]
const isSmallerThanSix = n => n < 6

R.reduceWhile(isSmallerThanSix, R.add, 0, arr)
//6
```

#### reject
`reject` is the compliment of `filter`. It returns the elements that fail the check.
```
const arr = [1, 2, 3, 4, 5, 6, 7]
const isEven = n => n % 2 === 0

R.reject(isEven, arr)
//[1, 3, 5, 7]
```

#### remove
Removes elements from an array, starting from the position indicated by the first parameter and removing the number of characters specified in the second parameter.
```
const arr = [1, 2, 3, 4, 5, 6, 7, 8]

R.remove(1, 3, arr)
//[1, 5, 6, 7, 8]
```

#### repeat
Repeats the first parameter for the number of times specified in the second parameter.
```
R.repeat(32, 4)
//[32, 32, 32, 32]
```
```
R.repeat([32, 33], 4)
//[[32, 33], [32, 33], [32, 33], [32, 33]]
```
```
R.repeat({a: 1, b: 2}, 2)
//[{"a": 1, "b": 2}, {"a": 1, "b": 2}]
```

#### replace
Replaces part of a string. Regex can be provided.
```
R.replace('e', '', 'Stephen')
//"Stphen"
```
```
R.replace(/e/g, '', 'Stephen')
//"Stphn"
```

#### reverse
Reverses the order of an array.
```
R.reverse([1, 2, 3])
//[3, 2, 1]
```
```
R.reverse('reverse')
//"esrever"
```

#### scan
Similar to reduce, however includes the result of each iteration. Takes the 2nd argument and uses it as a parameter to the predicate function along with the first element of the supplied array. It includes the result in the output array and inputs it again in the function together with the next element of the array, and so on, until all the elements of the list are used.
```
R.scan(R.add, 0, [1, 2, 3, 4, 5])
//[0, 1, 3, 6, 10, 15]
```
```
R.scan(R.multiply, 1, [1, 2, 3, 4, 5])
//[1, 1, 2, 6, 24, 120]
```

#### set
Used with lenses to set values.
```
const bLens = R.lensProp('b')
R.set(bLens, 2, {a: 1, b: 2})
//{"a": 1, "b": 2}
```

#### sort
Returns a copy of a provided list sorted by the provided function. The comparison function should compare two parameters at a time, and return a negative number if the first value is smaller, zero if they are equal, and a positive number if the second value is larger.
```
const arr = [3, 5, 4, 7, 8, 4, 2, 1, 8, 6, 9]
const compare = (a, b) => a - b

R.sort(compare, arr)
//[1, 2, 3, 4, 4, 5, 6, 7, 8, 8, 9]
```

#### sortBy
Sorts the list according to the supplied function.
```
const sortByFirstItem = R.sortBy(R.prop(1))
const pairs = [[-1, -8], [-2, -7], [-3, -6]]

sortByFirstItem(pairs)
//[[-1, -8], [-2, -7], [-3, -6]]
```
```
const sortByNameCaseInsensitive = R.sortBy(R.compose(R.toLower, R.prop('name')));
const alice = {
	name: 'ALICE',
	age: 101
}
const bob = {
	name: 'Bob',
	age: -10
}
const clara = {
	name: 'clara',
	age: 314.159
}
const people = [clara, bob, alice]

sortByNameCaseInsensitive(people)
//[alice, bob, clara]
```

#### sortWith
Sorts a list according to a list of comparators.
```
const alice = {
	name: 'alice',
	age: 40
}
const bob = {
	name: 'bob',
	age: 30
}
const clara = {
	name: 'clara',
	age: 40
}
const people = [clara, bob, alice]
const ageNameSort = R.sortWith([
	R.descend(R.prop('age')),
	R.ascend(R.prop('name'))
])

ageNameSort(people)
//[alice, clara, bob]
```

#### splitAt
Splits an array at the indicated index.
```
R.splitAt(1, [1, 2, 3]) //[[1], [2, 3]]
R.splitAt(5, 'hello world') //['hello', ' world']
R.splitAt(-1, 'foobar') //["fooba", "r"]
```

#### splitEvery
Splits an array into substrings of the indicated length.
```
R.splitEvery(3, [1, 2, 3, 4, 5, 6])
//[[1, 2, 3], [4, 5, 6]]
```
```
R.splitEvery(3, 'foobarbaz')
//["foo", "bar", "baz"]
```

### splitWhen
Splits a string into substrings when an element satisfies the predicate function.
```
R.splitWhen(R.equals(' '), 'Hello World')
//[["H", "e", "l", "l", "o"], [" ", "W", "o", "r", "l", "d"]]
```
```
const isGreaterThan3 = n => n > 3

R.splitWhen(isGreaterThan3, [1, 2, 3, 4, 5, 6])
//[[1, 2, 3], [4, 5, 6]]
```

#### symmetricDifference
Takes two arrays and return the elements that are not present in both of the arrays provided.
```
const arr1 = [1, 2, 3, 4]
const arr2 = [2, 3, 4, 5]

R.symmetricDifference(arr1, arr2)
//[1, 5]
```
```
const arr1 = [{a: 1}, {b: 2}]
const arr2 = [{a: 2}, {b: 2}, {c: 3}]

R.symmetricDifference(arr1, arr2)
//[{"a": 1}, {"a": 2}, {"c": 3}]
```
#### symmetricDifferenceWith
Takes two arrays and return the elements that are not present in both of the arrays provided, based on the predicate function.  

(I still did not manage to find a situation that requires this function rather than symmetricDifference)
```
const eqA = R.eqBy(R.prop('a'))
const l1 = [{a: 1}, {a: 2}, {a: 3}, {a: 4}]
const l2 = [{a: 3}, {a: 4}, {a: 5}, {a: 6}]

R.symmetricDifference(l1, l2) //[{a: 1}, {a: 2}, {a: 5}, {a: 6}]
R.symmetricDifferenceWith(eqA, l1, l2) //[{a: 1}, {a: 2}, {a: 5}, {a: 6}]
```

#### true
A function that always returns `true`.
```
R.T() //true  
```

#### tail
Returns all but the first element of a list.
```
const arr = [1, 2, 3, 4]

R.tail(arr)
//[2, 3, 4]
```
```
R.tail('/path')
//"path"
```

#### take
Returns the first `n` elements of a given list.
```
const arr = [1, 2, 3, 4]

R.take(2, arr)
//[1, 2]
```

#### takeLast
Returns the last `n` elements of a given list.
```
const arr = [1, 2, 3, 4]

R.takeLast(2, arr)
//[3, 4]
```

#### takeLastWhile
Returns the last consecutive elements in a list that satisfy the predicate function, starting from the end of the list.
```
const arr = [1, 2, 3, 4, 6, 8, 10]
const isEven = n => n % 2 === 0

R.takeLastWhile(isEven, arr)
//[4, 6, 8, 10]
```

#### takeWhile
Returns the last consecutive elements in a list that satisfy the predicate function.
```
const arr = [2, 3, 4, 6, 8, 10]
const isEven = n => n % 2 === 0

R.takeWhile(isEven, arr)
//[2]
```
```
const lt4 = R.lt(R.__, 4)
const arr = [1, 2, 3, 4, 5, 6, 7, 8]

R.takeWhile(lt4, arr)
//[1, 2, 3]
```

#### tap
Runs the given function on the given object, and then return the object. Note the second example below, it is the object hat is returned not the result of the function.
```
const outputName = name => console.log(name)
R.tap(outputName, 'Daniela')
//"Daniela"
```
```
const doubleUp = n => n * 2
R.tap(doubleUp, 3)
//3
```

#### then
Returns the result of applying the onSuccess function to the value inside a successfully resolved promise. This is useful for working with promises inside function compositions.
```
var makeQuery = (email) => ({ query: { email }})

//getMemberName :: String -> Promise ({firstName, lastName})
var getMemberName = R.pipe(
  makeQuery,
  fetchMember,
  R.then(R.pick(['firstName', 'lastName']))
)
```
#### thunkify
Creates a thunk out of a function. It delays execution until the result is required, this providing lazy evaluation of arguments. (I did not fully understand this, nor why an empty parameter `()` is needed.)
```
R.thunkify(R.sum)([1, 2])()
```
#### times
Takes a function which it applies to `0` iterating up to the value of the second parameter by incrementing by 1 with each iteration and including the results of each iteration in an array.
```
R.times(R.identity, 5)
//[0, 1, 2, 3, 4]
```
```
R.times(R.inc, 5)
//[1, 2, 3, 4, 5]
```
```
const doubleUp = n => n * 2
R.times(doubleUp, 5)
//[0, 2, 4, 6, 8]
``` 

#### transpose
Transposes the rows and columns of a 2D list. When passed a list of `n` lists of length `x`, returns a list of `x` lists of length `n`.
```
R.transpose([['a', 'b', 'c'], [4, 5, 6]])
//[["a", 4], ["b", 5], ["c", 6]]
```
```
R.transpose([["a", 4], ["b", 5], ["c", 6]])
//[["a", "b", "c"], [4, 5, 6]]
```

#### type
Returns the type of the parameter provided.
```
R.type({}) //"Object"
R.type(1) //"Number"
R.type(false) //"Boolean"
R.type('s') //"String"
R.type(null) //"Null"
R.type([]) //"Array"
R.type(/[A-z]/) //"RegExp"
R.type(() => {}) //"Function"
R.type(undefined) //"Undefined"
```
#### unFold
Builds a list from a seed value, which is the second parameter. The first parameter is an iterator function which must either return false, in which case the iteration stops, or else an array with two elements: the first is the value to return for the current iteration, and the second is the value to pass to the next iteration.

So, in the below example, 10 is passed, it is not greater than 50 so it is processed according to the first element of the array, in this case it is negated. The original value, 10, is passed to the second element of the array, and has 10 added to it, and this resulting value is passed to the next iteration.
```
const f = n => n > 50 ? false : [-n, n + 10]

R.unfold(f, 10)
//[-10, -20, -30, -40, -50]
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
#### subtract
Subtracts the second parameter from the first parameter.
```
R.subtract(7, 3) //4  
```
```
const minus5 = R.subtract(5, R.__)
minus5(17)
//12
```
```
const minus5 = R.subtract(R.__, 5)
minus5(17)
//-12
```
```
const complementaryAngle = R.subtract(90)

complementaryAngle(30) //60
complementaryAngle(72) //18
```

#### sum
Sums together all the elements of an array.
```
R.sum([1, 2, 3, 4, 5])
//15
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

#### slice
Returns part of an array starting from the index denoted by the first parameter, up to, but excluding, the element denoted by the second parameter.
```
const arr = [1, 2, 3, 4, 5, 6, 7]
R.slice(3, 5, arr)
//[4, 5]
```

#### split
Splits a string into an array of substrings based on the provided separator.
```
const myPath = 'product/categories/mobiles'

R.split('/', myPath)
//["product", "categories", "mobiles"]
```
```
const pathComponents = R.split('/')

R.tail(pathComponents('/usr/local/bin/node'))
//["usr", "local", "bin", "node"]
//'tail' gets rid of a resulting empty element since the string starts with the separator '/'
```

#### test
Determines whether a regular expression matches the provided string.
```
R.test(/^x/, 'xyz') //true
R.test(/^y/, 'xyz') //false
```

#### toLower
Takes a string and converts any upper case characters to lowercase.
```
R.toLower('ABC') //"abc"
```
#### toPairs
Converts an object into an array of key and value elements.
```
const obj = {a: 1, b: 2, c: 3}
R.toPairs(obj)
//[["a", 1], ["b", 2], ["c", 3]]
```
#### toPairsIn
Example on Ramda website not working:
```
const F = function() { this.x = 'X'; };
F.prototype.y = 'Y';
const f = new F();
R.toPairsIn(f);
```

#### toString
Converts to string. Note that if an object has its won custom `toString` implementation, this function will use the object's own `toString` method.
```
R.toString(12) //"12"
R.toString('abc') //"\"abc\""
R.toString([1, 2, 3]) //"[1, 2, 3]"
R.toString(['a', 'b', 'c']) //"[\"a\", \"b\", \"c\"]"
R.toString({a: 1, b: 2}) //"{\"a\": 1, \"b\": 2}"
```

#### toUpper
Takes a string and converts any lower case characters to uppercase.
```
R.toUpper('abc') //"ABC"
```

#### trim
Removes any white space from the beginning and end of a string.
```
R.trim("   Hello World!   ")
//"Hello World!"
```

## Not Covered
- applySpec
- bind
- composeWith
- constructN
- curryN (how is it different from `curry`?
- invoker
- otherwise
- pipeK
- pipeP
- pipeWith
- sequence
- transduce
- traverse
- tryCatch
- unapply
- unary
- unCurryN
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTIyMDgxMzc5MCwxOTUxMjE1NzM0LDEyMj
Q0ODMyODUsMTE4NDU1NDI4MSwyMTM2NjU2MDM2LDEwMjU5OTk0
MDYsNjE5OTAzODc3LC0xNDY4MzM2NTkwLC0xNTE1MTkyNzI5LD
E2NjIxOTkxMjgsMTAwODg4OTYxNCwtMTA5MTY0NTE3MywtNTYw
MTYxNDY1LC0xMjQyNzUyMzE1LC0xNTc4OTg2Nzk2LC0yMDgzMj
Q5MTkzLDE3NDc3MTM0NTYsMTA4MDM0MDA1NiwtMTg3ODM4Njc0
MSw4MjI5OTIyNzddfQ==
-->