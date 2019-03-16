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

#### assoc
Inserts a key / value set in the provided object literal. Note that the new object literal is sorted in ascending order.

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
``

## Not Covered
applySpec
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTUyOTkwMzg5NywtMjQ0NjkzODMsLTkwOD
Y0MDM4MCw2NzcyNzU3ODAsLTM5MDMwMTAwNCwtMzcwODI3NjU4
LC04ODQ3ODk4MzUsLTE4NDkzMTg3LC01NjU5NTM5ODYsMTUzOD
AyODg1LDIwNTE5Mzg0NTIsMTk5NzM2NjI1MywxNTIzMjQ3MzMz
LC0xNzExODY5MTgsMTEyMTMyNzMwLDEzMDU4MTYwNTMsMTI4Nz
YyOTc4NiwyMTA4NjcyNjI3LC03NDA1NDU5NzUsLTI0OTE3NTcx
XX0=
-->