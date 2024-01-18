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
