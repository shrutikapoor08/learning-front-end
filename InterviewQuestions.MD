# Front End Interview and questions

1. Flatten all sub-arrays
```
const test = [ [2,3], 2, [5, [5 , 7], 8], 123];


const flattenArray = (arr) => {
  const flattenedArray = [];
  
  for ( let i = 0; i< arr.length; i++) {
    console.log(arr[i])
    let subArray = arr[i];
    if (typeof subArray === "object") {
      flattenedArray.push( ... flattenArray(subArray) )
    } 
    else {
      flattenedArray.push(subArray);
    }
  } 
  
  return flattenedArray;
}

console.log(flattenArray(test));

```