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
  animal: { name: 'elephant', 
			type: 'mammal', 
			origin: { continent: 'Africa', country: 'Gabon', } }
  animal: {	name: 'shark',
		    type: 'fish',
		    origin: { continent: 'Australia', country: 'Sydney', } },
  animal: { name: 'eagle',
		    type: 'bird'}
		    origin: { continent: 'USA', country: 'Arizona', }, }
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

## Filtering 

## Sorting

## Operational
#### add
```
R.add(2.1, 3.45)


<!--stackedit_data:
eyJoaXN0b3J5IjpbNTE1MTc4ODY5LDUwODU1NzQ2NSwyNzc0NT
YxLDU3MTk4OTg3NSwxNzc4MjEyMzk4LDE0NzEyMzg4MzAsMTg5
MTgyMDM1LC00NDkyNjg0ODMsNTk4OTkxNDIwLC0xMTAxNDY0OD
AwLC00MjI5MDU3NjUsLTExMzIxOTM3Nl19
-->