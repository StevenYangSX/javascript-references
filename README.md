# JavaScript References

## 1. Built-in objects

### 1. Array

#### Description

In JavaScript, arrays are **NOT** primitives! It is `Array` objects with the following core characteristics:

- Resizable and can contain a mix of different data types.
- NOT associative array. Must use nonnegative integers. zero-indexed.
- array-copy operations create **SHALLOW COPIES** : A copy whose properties share the same [references](https://developer.mozilla.org/en-US/docs/Glossary/Object_reference) (point to the same underlying values) as those of the source object from which the copy was made. As a result, when you change either the source or the copy, you may also cause the other object to change too.

#### Constructor

```javascript
// Note: Array() can be called with or without new. Both create a new Array instance.
new Array();
new Array(ele1, ele2,..., eleN);
Array(arrayLength)

// when only ONE argument is passed, it is the length of the array.
```

#### Static Methods

- `Array.from()` : The **`Array.from()`** static method creates a new, shallow-copied `Array` instance from an [iterable](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols#the_iterable_protocol) or [array-like](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Indexed_collections#working_with_array-like_objects) object.

  ```javascript
  Array.from(arrayLike)
  Array.from(arrayLike, mapFn)
  Array.from(arrayLike, mapFn, thisArg)
  
  // Array from a String
  Array.from('foo') // [ "f", "o", "o" ]
  
  // Array from a Set
  const set = new Set(["foo", "bar", "baz", "foo"]);
  Array.from(set); // [ "foo", "bar", "baz" ]
  
  // Array from a Map
  const map = new Map([
    [1, 2],
    [2, 4],
    [4, 8],
  ]);
  Array.from(map); // [[1, 2], [2, 4], [4, 8]]
  
  // Using mapFn to create some useful features : genereate a sequence of numbers
  const generatedArr = Array.from({length:5}, (value,index) => {
      return index + 3;
  } )
  //  generatedArr =  [3,4,5,6,7]
  ```

- `Array.fromAsync()`

- `Array.isArray()` : Determine whether the passed value is an Array

  ```javascript
  Array.isArray([1, 3, 5] // true
  Array.isArray('[]') // false
  ```

- `Array.of()` : create a new `Array` instance from a variable number of arguments, regardless of number or type of the arguments.

  ```javascript
  Array.of('foo', 2, 'bar', true)
  // Expected output: Array ["foo", 2, "bar", true]
  
  Array.of()
  // Expected output: Array []
  ```

#### Instance Methods

- `Array.prototype.at()` : takes an integer value and returns the item at that index, allowing for **positive** and **negative** integers. **Negative** integers count **back** from the last item in the array. If argument is `argument < - array.length || argument >= array.length`, return `undefined`.

  ```javascript
  const array1 = [5, 12, 8, 130, 44];
  let index = 2;
  array1.at(index)  // 9;
  array1.at(-2) // 130
  ```

- `Array.prototype.concat()` : merge two or more arrays. This method **DOES NOT** change the existing arrays, but instead returns a **NEW ARRAY**. It returns **SHALLOW COPY** that contains the same elements as the ones from the original arrays.

  ```javascript
  const array1 = ['a', 'b', 'c'];
  const array2 = ['d', 'e', 'f'];
  const array3 = array1.concat(array2); // ["a", "b", "c", "d", "e", "f"]
  
  const num1 = [1, 2, 3];
  const num2 = [4, 5, 6];
  const num3 = [7, 8, 9];
  
  const numbers = num1.concat(num2, num3);
  
  console.log(numbers);
  // results in [1, 2, 3, 4, 5, 6, 7, 8, 9]
  ```

- `Array.prototype.copyWithin()` : shallow copies part of this array to another location in the same array and returns this array without modifying its length.

  ```javascript
  // copyWithin(target, start)
  // copyWithin(target, start, end)
  /*
  * target : index at which to copy the sequence to.
  	-array.length < target < 0    ===>   target + array+length  is used.   -10 < target -7 < 0  =>  10-7 = 3 is used.
      target >= array.length        ===>    nothing is copied
      target < -array.length        ===>    0 is used.
      
    start :  index at which to start copying elements from
    end :  index at which to end copying elements from. up to but not including end.
  */
  
  
  // copy entire array elments from target
  [1, 2, 3, 4, 5].copyWithin(2) // omitting  start is the same as passing start = 0 
  [1, 2, 3, 4, 5].copyWithin(2,0) // same as above
  //   output ===>   [1, 2, 1, 2, 3]
  
  [1, 2, 3, 4, 5].copyWithin(0, 3, 4)  // [4, 2, 3, 4, 5]
  
  ```

- `Array.prototype.entries()` :  returns a new *[array iterator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Iterator)* object that contains the key/value pairs for each index in the array.

  ```javascript
  const a = ["a", "b", "c"];
  for (const [index, element] of a.entries()) {
    console.log(index, element);
      // 0 'a'
  	// 1 'b'
  	// 2 'c'
  }
  
  const array = ["a", "b", "c"];
  const arrayEntries = array.entries();
  for (const element of arrayEntries) {
    console.log(element);
      // [0, 'a']
  	// [1, 'b']
  	// [2, 'c']
  }
  ```

- `Array.prototype.every()` :  tests whether all elements in the array pass the test implemented by the provided function. It returns a Boolean value.

  ```javascript
  // every(callbackFn)
  // every(callbackFn, thisArg)
  const arr = [12, 5, 7, 130, 44];
  const largeEngouth = arr.every((element, index, array) => {
      return element >= 10;
  })
  // largeEngouth === false; 
  
  const numbers1 = [-2, 4, -8, 16, -32];
  const numbers2 = [-2, 4, 8, 16, 32];
  const isIncreasing = (numbers: number[]) => {
    return numbers.every((num, idx, arr) => {
      // Without the arr argument, there's no way to easily access the
      // intermediate array without saving it to a variable.
      if (idx === 0) return true;
      return num > arr[idx - 1];
    });
  };
  console.log(isIncreasing(numbers1)); // false
  console.log(isIncreasing(numbers2)); // true
  ```

- `Array.prototype.fill()` : changes all elements within a range of indices in an array to a static value. It returns the modified array.

  ```javascript
  // fill(value)
  // fill(value, start)
  // fill(value, start, end)
  const tempGirls = Array(5).fill("girl", 0);
  // ["girl","girl","girl","girl","girl"]
  ```

- `Array.prototype.filter()` : creates a [shallow copy](https://developer.mozilla.org/en-US/docs/Glossary/Shallow_copy) of a portion of a given array, filtered down to just the elements from the given array that pass the test implemented by the provided function.

  ```javascript
  // filter(callbackFn)
  // filter(callbackFn, thisArg)
  // callbackFn(element, index, arr)
  
  //searching in array
  const fruits = ["apple", "banana", "grapes", "mango", "orange"];
  const keyLetters = "ap";
  function filterItems(arr: string[], query: string): string[] {
    return arr.filter((el) => el.toLowerCase().includes(query.toLowerCase()));
  }
  const filtered = filterItems(fruits, keyLetters);
  console.log(filtered); // ["apple", "grapes"]
  
  ```

- `Array.prototype.find()` : returns the first element in the provided array that satisfies the provided testing function. If no values satisfy the testing function, [`undefined`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined) is returned.

- `Array.prototype.findIndex()` : returns the index of the first element in an array that satisfies the provided testing function. If no elements satisfy the testing function, -1 is returned.

- `Array.prototype.findLast()` :  iterates the array in reverse order and returns the value of the first element that satisfies the provided testing function. If no elements satisfy the testing function, [`undefined`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined) is returned.

- `Array.prototype.findLastIndex()` : iterates the array in reverse order and returns the index of the first element that satisfies the provided testing function. If no elements satisfy the testing function, -1 is returned.

- `Array.prototype.flat()` : creates a new array with all sub-array elements concatenated into it recursively up to the specified depth.

  ```javascript
  // flat()   using default depth : 1
  // flat(depth)
  
  const arr1 = [1, 2, [3, 4]];
  arr1.flat();
  // [1, 2, 3, 4]
  
  const arr2 = [1, 2, [3, 4, [5, 6]]];
  arr2.flat();
  // [1, 2, 3, 4, [5, 6]]
  
  const arr3 = [1, 2, [3, 4, [5, 6]]];
  arr3.flat(2);
  // [1, 2, 3, 4, 5, 6]
  
  const arr4 = [1, 2, [3, 4, [5, 6, [7, 8, [9, 10]]]]];
  arr4.flat(Infinity);
  // [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
  ```

- `Array.prototype.forEach()` : executes a provided function once for each array element. return None.

  ```javascript
  const array1 = ['a', 'b', 'c'];
  array1.forEach((element) => console.log(element));
  // Expected output: "a"
  // Expected output: "b"
  // Expected output: "c"
  
  callBackFn(element, index, array) // current element, current index, current Array
  ```

- `Array.prototype.includes()` : determines whether an array includes a certain value among its entries, returning `true` or `false` as appropriate.

  ```javascript
  includes(searchElement)
  includes(searchElement, fromIndex)
  ```

- `Array.prototype.indexAt()` : returns the **first index** at which a given element can be found in the array, or -1 if it is not present.

  ```javascript
  indexOf(searchElement)
  indexOf(searchElement, fromIndex)
  ```

- `Array.prototype.join()` :  creates and returns a new string by concatenating all of the elements in this array, separated by commas or a specified separator string. If the array has only one item, then that item will be returned without using the separator.

  ```javascript
  const elements = ['Fire', 'Air', 'Water'];
  
  console.log(elements.join());
  // Expected output: "Fire,Air,Water"
  
  console.log(elements.join(''));
  // Expected output: "FireAirWater"
  
  console.log(elements.join('-'));
  // Expected output: "Fire-Air-Water"
  ```

- `Array.prototype.keys()` :  returns a new *[array iterator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Iterator)* object that contains the keys for each index in the array.

- `Array.prototype.map()` : creates a new array populated with the results of calling a provided function on every element in the calling array.

  ```javascript
  map(callbackFn)
  map(callbackFn, thisArg)
  
  callBackFn(currentElement, currentIndex, currentArray)
  // Since map builds a new array, calling it without using the returned array 
  // is an anti-pattern; use forEach or for...of instead.
  
  
  ```

- `Array.prototype.pop()` : removes the **last** element from an array and returns that element. This method changes the length of the array.

- `Array.prototype.push()` :  adds the specified elements to the end of an array and returns **the new length of the array**.

- `Array.prototype.shift()` : removes the **first** element from an array and returns that removed element. This method changes the length of the array.

- `Array.prototype.unshift()` : adds the specified elements to the beginning of an array and returns the new length of the array.

- `Array.prototype.slice()` :  returns a [shallow copy](https://developer.mozilla.org/en-US/docs/Glossary/Shallow_copy) of a portion of an array into a new array object selected from `start` to `end` (`end` not included) where `start` and `end` represent the index of items in that array. The original array will not be modified.

  ```javascript
  const animals = ['ant', 'bison', 'camel', 'duck', 'elephant'];
  
  console.log(animals.slice(2));
  // Expected output: Array ["camel", "duck", "elephant"]
  
  console.log(animals.slice(2, 4));
  // Expected output: Array ["camel", "duck"]
  
  console.log(animals.slice(1, 5));
  // Expected output: Array ["bison", "camel", "duck", "elephant"]
  
  console.log(animals.slice(-2));
  // Expected output: Array ["duck", "elephant"]
  
  console.log(animals.slice(2, -1));
  // Expected output: Array ["camel", "duck"]
  
  console.log(animals.slice());
  // Expected output: Array ["ant", "bison", "camel", "duck", "elephant"]
  
  ```

- `Array.prototype.splice()` :  changes the contents of an array by removing or replacing existing elements and/or adding new elements [in place](https://en.wikipedia.org/wiki/In-place_algorithm).

  ```javascript
  splice(start)
  splice(start, deleteCount)
  splice(start, deleteCount, item1)
  splice(start, deleteCount, item1, item2)
  splice(start, deleteCount, item1, item2, /* …, */ itemN)
  
  //Arguments  
  
  // 1. start
  // index at which to start changing the array. 
  
  // 2. deleteCount
  // indicating the number of elements in the array to remove from start
  // if omitted, or >= array.length - start, all elements from start to end will be deleted
  
  // An array containing the deleted elements.
  
  // If only one element is removed, an array of one element is returned.
  
  // If no elements are removed, an empty array is returned.
  
  //* if no item are passed, No element will be added. Only as a remove function.
  
  
  const myFish = ["angel", "clown", "mandarin", "sturgeon"];
  const removed = myFish.splice(2, 0, "drum"); // remove 0 emelent from index2 and insert drum to the index
  // myFish is ["angel", "clown", "drum", "mandarin", "sturgeon"]
  // removed is [], no elements removed
  
  
  const myFish = ["angel", "clown", "mandarin", "sturgeon"];
  const removed = myFish.splice(2, 0, "drum", "guitar");// remove 0 element starting from index 2 and insert drum , guitar
  // const myFish = ["angel", "clown", "drum", "guitar", "mandarin", "sturgeon"];
  // removed is [], no elements removed
  
  const myFish = ["angel", "clown", "mandarin", "sturgeon"];
  const removed = myFish.splice(2); // remove starting index 2, deleteCount omitted, so remove all behind 2
  // myFish = [ "angel", "clown"]
  // removed = [ "mandarin", "sturgeon" ]
  ```

- `Array.prototype.some()` :  tests whether at least one element in the array passes the test implemented by the provided function. It returns true if, in the array, it finds an element for which the provided function returns true; otherwise it returns false. It doesn't modify the array.

  ```javascript
  some(callbackFn)
  some(callbackFn, thisArg)
  
  // callbackFn ( elemnet, index, array)
  
  
  ```

- `Array.prototype.sort()` :  sorts the elements of an array *[in place](https://en.wikipedia.org/wiki/In-place_algorithm)* and returns the reference to the same array, now sorted. The default sort order is ascending, built upon converting the elements into strings, then comparing their sequences of UTF-16 code units values.

  ```javascript
  // sort()
  // sort(compareFn)
  
  // compareFn     To memorize this, remember that (a, b) => a - b sorts numbers in ascending order.
  
  const items = [
    { name: "Edward", value: 21 },
    { name: "Sharpe", value: 37 },
    { name: "And", value: 45 },
    { name: "The", value: -12 },
    { name: "Magnetic", value: 13 },
    { name: "Zeros", value: 37 },
  ];
  
  // sort by value :   ascending order
  items.sort((a, b) => a.value - b.value);
  
  // sort by name
  items.sort((a, b) => {
    const nameA = a.name.toUpperCase(); // ignore upper and lowercase
    const nameB = b.name.toUpperCase(); // ignore upper and lowercase
    if (nameA < nameB) {
      return -1;
    }
    if (nameA > nameB) {
      return 1;
    }
  
    // names must be equal
    return 0;
  });
  ```

- `Array.prototype.values()` : returns a new *[array iterator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Iterator)* object that iterates the value of each item in the array.

- `Array.prototype.with()` : is the [copying](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array#copying_methods_and_mutating_methods) version of using the [bracket notation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Property_accessors#bracket_notation) to change the value of a given index. It returns a new array with the element at the given index replaced with the given value.

  ```javascript
  //  arrayInstance.with(index, value)
  
  const arr = [1, 2, 3, 4, 5];
  const newArr = arr.with(2, 6).map((x) => x ** 2); // [1, 4, 36, 16, 25]
  // newArr = [1, 4, 36, 16, 25]
  // arr = [1, 2, 3, 4, 5];
  ```

- `Array.prototype.reduce()` : executes a user-supplied "reducer" callback function on each element of the array, in order, passing in the return value from the calculation on the preceding element. The final result of running the reducer across all elements of the array is a single value.

  The first time that the callback is run there is no "return value of the previous calculation". If supplied, an initial value may be used in its place. Otherwise the array element at index 0 is used as the initial value and iteration starts from the next element (index 1 instead of index 0).

  ```javascript
  // reduce(callbackFn)
  // reduce(callbackFn, initialValue)
  // !!! if initialValue is omitted, array[0] is used as the initial value and iteration starts from index 1
  
  // callbackFn ( accumulator , currentValue, currentIndex, array )
  
  
  const array = [15, 16, 17, 18, 19];
  
  function reducer(accumulator, currentValue, index) {
    const returns = accumulator + currentValue;
    console.table(
      `accumulator: ${accumulator}, currentValue: ${currentValue}, index: ${index}, returns: ${returns}`,
    );
    return returns;
  }
  
  array.reduce(reducer); // no initValue ! start from index 1 
  
  // accumulator: 15, currentValue: 16, index: 1, returns: 31
  // accumulator: 31, currentValue: 17, index: 2, returns: 48
  // accumulator: 48, currentValue: 18, index: 3, returns: 66
  // accumulator: 66, currentValue: 19, index: 4, returns: 85
  
  
  array.reduce(reducer, 0); // with initValue , start from index 0 
  // accumulator: 0, currentValue: 15, index: 0, returns: 15
  // accumulator: 15, currentValue: 16, index: 1, returns: 31
  // accumulator: 31, currentValue: 17, index: 2, returns: 48
  // accumulator: 48, currentValue: 18, index: 3, returns: 66
  // accumulator: 66, currentValue: 19, index: 4, returns: 85
  ```

  