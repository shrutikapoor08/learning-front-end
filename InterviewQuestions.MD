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

1. DOM tree updates
  


1. Create a funciton that takes a string and returns a string with duplicate strings removed


```
function removeDupesInStr(sentence) {
  const lookupObject = {};
  
 const sentenceArr = sentence.split(' ');
  
  for(let i =0; i< sentenceArr.length; i++) {
    let word = sentenceArr[i];
    if(lookupObject[word]) {
      sentenceArr.splice(i, 1);
    } else {
      lookupObject[word] = true;
    }
  };
  
  return sentenceArr.join(' ');
  
}

console.log(removeDupesInStr("This is a test test str"));
