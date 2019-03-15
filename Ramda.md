# Ramda

## Sample Data

**Single Dimension Data**
```
const arr = [1.1, 2.2, 3.3]

const obj = {name: 'elephant', type: 'mammal'}
```
**Multi-Dimension Data**
```
const arrList = [
  [1.1, 1.2, 1.3],
  [2.1, 2.2, 2.3],
  [3.1, 3.2, 3.3]
]

const objList = {
  animal: {name: 'elephant', type: 'mammal'},
  animal: {name: 'shark', type: 'fish'},
  animal: {name: 'eagle', type: 'bird'},
}
```

## Extracting Data

### Extracting Data from Single Dimension Containers

#### prop
```
R.prop(1, arr)

//2.2
```


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEwMDc2NTQ0NjAsLTExMDE0NjQ4MDAsLT
QyMjkwNTc2NSwtMTEzMjE5Mzc2XX0=
-->