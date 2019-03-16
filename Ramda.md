# Ramda

## Sample Data

**Single Dimension Data**
```
const arr = [1.1, 2.2, 3.3]

const obj = {name: 'elephant', 
			type: 'mammal', 
			origins: { continent: 'Africa', country: 'Gabon', } }
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
			origins: { continent: 'Africa', country: 'Gabon', } }
  animal: {	name: 'shark',
		    type: 'fish',
		    origins: { continent: 'Australia', country: 'Sydney', } },
  animal: { name: 'eagle',
		    type: 'bird'}
		    origins: { continent: 'USA', country: 'Arizona', }, }
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
            origins: {
              continent: 'Africa',
              country: 'Gabon',
            }
         }

R.prop('name', obj)

//elephant
```

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
            origins: {
              continent: 'Africa',
              country: 'Gabon',
            }
         }

R.path(['origins', 'country'], obj)

//Africa
```



<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExMjE3MDcwODYsMTg5MTgyMDM1LC00ND
kyNjg0ODMsNTk4OTkxNDIwLC0xMTAxNDY0ODAwLC00MjI5MDU3
NjUsLTExMzIxOTM3Nl19
-->