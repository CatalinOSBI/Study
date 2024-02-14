# Study

## Arrays 



```
const myArray = [1,2,3]
const myArray = ["A","B","C"]

const firstItem = myArray[0] | returns 1
const firstItem = myArray[1] | returns 2

myArray[0] = "A" | returns ["A",2,3]
```
_________________________________________
```	
const myArray = [1,2,3],["A","B","C"]
                           
const Data = myArray[0][0] | returns 1
const Data = myArray[0][1] | returns 2
const Data = myArray[1][0] | returns A
const Data = myArray[1][1] | returns B
```
_________________________________________
```
.unshift() - ADDS an item at the START of an array ( usage myArray.unshift() )

const myArray = [1,2,3]

myArray.unshift(0) | returns [ 0, 1, 2, 3 ]
```
_________________________________________
```
.push() - ADDS an item at the END of an array ( usage myArray.push() )

const myArray = [1,2,3]

myArray.push(4) | returns [ 1, 2, 3, 4 ]
```
_________________________________________
```
.pop() - REMOVES the LAST item of an array ( usage myArray.pop() )

const myArray = [1,2,3,4]

myArray.pop() | returns [ 1, 2, 3 ]
```
_________________________________________
```
.shift() - REMOVES the FIRST item of an array ( usage myArray.shift() )

const myArray = [1,2,3,4]

myArray.shift() | returns [ 2, 3, 4 ]
```
_________________________________________
```
.length - returns the number of items in an array

const myArray = ["A","B","C"]

myArray.length | returns 3
```
_________________________________________

## Objects



```
.filter() - Think about it as FILTERING trough a Database ( usage myArray.filter(filterFunction) )

const myArray = ['spray', 'elite', 'exuberant', 'destruction', 'present']

const filterFunction = (arrayItem) => {
  arrayItem.length > 6
}

myArray.filter(filterFunction) | returns ["exuberant", "destruction", "present"]
```
_________________________________________
```
.map() - Think about it as applying a FUNCTION to an entire Database ( usage myArray.map(mapFunction) )

const myArray = ['spray', 'elite', 'exuberant', 'destruction', 'present']

const mapFunction = (arrayItem) => {
  arrayItem.length > 6
}

myArray.map(mapFunction) | returns [false, false, true, true, true]
```
_________________________________________
```
.reduce() - Think about it as applying a CALCULATION to an entire Database ( usage myArray.reduce((reduceFunction), initialValue) )

const myArray = [1, 2, 3, 4]

const initialValue = 0 (0 is usually the default value from what I've seen)

const reduceFunction = (total, arrayItem) => {
 return total + arrayItem
}

myArray.reduce((reduceFunction), initialValue) | returns 10 | How it works: 0+1+2+3+4 ( 10 )
						            | If the initialValue were 10: 10+1+2+3+4 ( 20 )
						            | If the total were total + 1: 0+1+1+2+1+3+1+4+1 ( 14 )
```
_________________________________________


