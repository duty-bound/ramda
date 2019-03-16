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
		    origins: { continent: 'Australia', country: 'Sy', } },
  animal: {name: 'eagle', type: 'bird'},
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
const obj = {name: 'elephant', type: 'mammal'}

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
              country: 'Africa',
              date: 2002,
            }
         }

R.path(['origins', 'country'], obj)

//Africa
```



<!--stackedit_data:
eyJoaXN0b3J5IjpbMTEwNzc2ODEyMywxODkxODIwMzUsLTQ0OT
I2ODQ4Myw1OTg5OTE0MjAsLTExMDE0NjQ4MDAsLTQyMjkwNTc2
NSwtMTEzMjE5Mzc2XX0=
-->