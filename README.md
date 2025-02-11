# chunk-arr
[![Version](https://img.shields.io/npm/v/chunk-arr.svg)](https://www.npmjs.com/package/chunk-arr)
[![XO code style](https://img.shields.io/badge/code_style-XO-5ed9c7.svg)](https://github.com/xojs/xo)
[![codecov](https://codecov.io/gh/nam288/chunk-arr/branch/main/graph/badge.svg?token=fbqIQ8uk7t)](https://codecov.io/gh/nam288/chunk-arr)
![build](https://github.com/nam288/chunk-arr/actions/workflows/main.yml/badge.svg)

> Split array into chunks by size or certain condition.

## Why?

* Minimal APIs
* Written in TypeScript
* Actively maintained
* Fully coverage tests
* Inspired from [Ruby chunk](https://ruby-doc.org/core-3.0.0/Enumerable.html#method-i-chunk)

## Install

```sh
$ npm install chunk-arr
```

## Usage


```js
const { chunkBy } = require('chunk-arr');

chunkBy([3, 1, 4, 1, 5, 9, 2, 6], n => n % 2);
// [ [3, 1], [4], [1, 5, 9], [2, 6] ]

```

## API

### `chunk(array, [size = 1])`

Like lodash/chunk, split the `array` into chunks of `size`. If array can not be split evenly, the final chunk will be the remaining elements.

#### array

* Require: `true`
* Type: `any[]`

The array to process

#### size

* Require: `false`
* Type: `number`
* Default: `1`

The size of each chunk. Return empty array if `size` smaller than `1`.

**Note:** The `size` will be converted to the largest number that smaller than `size` by `Math.floor`.

**Example:**
```js
const { chunk } = require('chunk-arr');
// or
// const chunk = require('chunk-arr').default;

const chars = ['a', 'b', 'c', 'd', 'e'];

console.log(chunk(chars));
// Output: [ ['a'], ['b'], ['c'], ['d'], ['e'] ]

console.log(chunk(chars, 2.7));
// Output: [ ['a', 'b'], ['c', 'd'], ['e'] ]
```

### `chunkBy(array, func)`

Iterate over `array` elements, chunking them together based on the returned value of `func`.

#### array

* Require: `true`
* Type: `T[]`

The array to process

#### func

* Require: `true`
* Type: `(element: T, index: number): U`

The function used to chunk `array` based on its return value. It takes two arguments: the current element is iterating and its index

**Example:**
* Split based on element types:

```js
const { chunkBy } = require('chunk-arr');

const arr = [true, -3, 1, 'a', 'b', 'c'];

console.log(chunkBy(arr, e => typeof e));
// Output: [ [true], [-3, 1], ['a', 'b', 'c'] ]
```

* Split strings by their sizes:
```js
const { chunkBy } = require('chunk-arr');

const arr = ['Lorem', 'Ipsum', 'is', 'simply', 'dummy', 'text'];

console.log(chunkBy(arr, s => s.length));
// Output: [ ['Lorem', 'Ipsum'], ['is'], ['simply', 'dummy'], ['text'] ]
```

### `chunkWhile(array, func)`

This method splits the `array` between adjacent elements if the `func` receives those elements returns `false`.

#### array

* Require: `true`
* Type: `Array<T>`

The array to process

#### func

* Require: `true`
* Type: `(previous: T, current: T): boolean`

The function, which receives the two adjacent elements alternatively, splits array between them if it returns `false`.

**Example:**

* Split array into non-decreasing chunks:

```js
const { chunkWhile } = require('chunk-arr');

const nums = [0, 9, 2, 2, 3, 2, 7, 5, 9, 5];

console.log(chunkWhile(nums, (prev, curr) => prev <= curr));
// Output: [[0, 9], [2, 2, 3], [2, 7], [5, 9], [5]]
```

* Convert increasing numbers into the compact string:

```js
const { chunkWhile } = require('chunk-arr');

const nums = [1, 2, 4, 9, 10, 11, 12, 15, 16, 19, 20, 21];

console.log(chunkWhile(nums, (prev, curr) => prev + 1 === curr)
		    .map((chunk) => chunk.length > 2 ? `${chunk[0]}-${chunk[chunk.length - 1]}` : chunk)
		    .join(',')
);
// Output: '1,2,4,9-12,15,16,19-21'
```

* Split Markdown file into sections:

```js
const Fs = require('fs');
const { chunkWhile } = require('chunk-arr');

try {
	const data = Fs.readFileSync('./README.md', 'utf8');
	console.log(chunkWhile(data.split('\n'), (_, current) => !current.startsWith('#')));
} catch (error) {
	console.error(error);
}

// Output:
// [
//   ['# chunk-arr', ...],
//   ['## Why?, ...],
//   ...
//   ['## Author', ...]
// ]
```

## Author

* **Nam Hoang Le**

Give a ⭐️ if this package helped you!
