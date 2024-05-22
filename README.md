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

  