# JavaScript Hardparts

## Higher Order Function

```javascript
function updateArray(array,operation) {
	const updatedArray = [];

	for(let i=0; i< updatedArray.length ; i++) {
		updatedArray[i] =operation(array[i]);
	}

	return updatedArray;
}

function multiplyBy2(input) {
	return input * 2
}

function addFive(input) {
	return input + 5;
}

const myArray = [1,2,3]
const result1 = updateArray(myArray,multiplyBy2);
const result2 = updateArray(myArray,addFive);
```