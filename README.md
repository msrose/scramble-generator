# scramble-generator

[![npm version](https://badge.fury.io/js/scramble-generator.svg)](https://badge.fury.io/js/scramble-generator)
[![Build Status](https://travis-ci.org/msrose/scramble-generator.svg?branch=master)](https://travis-ci.org/msrose/scramble-generator)

Rubik's cube scramble generator

## Usage

    npm install scramble-generator

```javascript
import { formatted, generate, parse } from 'scramble-generator';

formatted({ cubeSize: 3 }) // "R U' F2 D2 F2 L2 B2 R F' L' U' F' U2 R F U R D' L2 U2"

generate({ cubeSize: 3 }) // [ { face: 'B', longFace: 'BACK', inverted: false, double: true },
                          // { face: 'R', longFace: 'RIGHT', inverted: true, double: false }, ... ]

parse("B2 R'") // [ { face: 'B', longFace: 'BACK', inverted: false, double: true },
               // { face: 'R', longFace: 'RIGHT', inverted: true, double: false } ]
```

## API

<!-- Generated by documentation.js. Update this documentation by updating the source code. -->

### scramble-generator

**Meta**

-   **author**: Michael Rose
-   **license**: https&#x3A;//github.com/msrose/scramble-generator/blob/master/LICENSE

### Move

A turn of the cube is represented throughout as a Move object, which has all the properties necessary to describe how a given turn must be executed.

Type: [object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)

**Properties**

-   `face` **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** The short name of the face to turn e.g. 'R'
-   `longFace` **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** The long name of the face to turn e.g. 'RIGHT'
-   `inverted` **[boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** Indicates if the turn is to be made clockwise (`false`) or counter-clockwise (`true`)
-   `double` **[boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** Indicates if the turn is 180 degrees. If `true`, `inverted` will always be `false`.
-   `layerCount` **[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)** Indicates how many layers should be turned. Will be at least `1` and at most `Math.floor(cubeSize / 2)`.

### Faces

Map of constants representing names of cube faces. The keys are 'RIGHT', 'UP', 'LEFT', 'DOWN', 'BACK', and 'FRONT'.

**Examples**

```javascript
import { Faces } from 'scramble-generator';
Faces.RIGHT; // 'RIGHT'
Faces.R; // 'R'
```

### generate

Generates a random scramble for the given cube size.

**Parameters**

-   `$0` **[object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)?** 
    -   `$0.cubeSize` **[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)?** The size (number of layers) of the cube to generate a scramble for (optional, default `3`)
    -   `$0.length` **[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)?** The number of moves in the generated scramble. Default value depends on cube size. (optional, default `(cubeSize-2)*20||8`)

**Examples**

```javascript
import { generate } from 'scramble-generator';
generate({ cubeSize: 3 });
// [ { face: 'U', longFace: 'UP', inverted: false, double: true },
// { face: 'R', longFace: 'RIGHT', inverted: true, double: false },
// { face: 'D', longFace: 'DOWN', inverted: false, double: true }, ... ]
```

Returns **[Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[Move](#move)>** A list of moves representing the scramble for the cube.

### format

Formats a given scramble as a string.

**Parameters**

-   `scramble` **[Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[Move](#move)>?** List of Move objects representing a scramble to be formatted.

**Examples**

```javascript
import { format, Faces } from 'scramble-generator';
format([{
  face: Faces.R,
  inverted: true
}, {
  face: Faces.U,
  double: true
}, {
  face: Faces.L
}])
// "R' U2 L"
```

Returns **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** String representation of the given scramble.

### formatted

-   **See: [generate](#generate)**

Generates a random formatted scramble.

**Parameters**

-   `options` **[object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)?** Same options passed to `generate`

**Examples**

```javascript
import { formatted } from 'scramble-generator';
formatted({ cubeSize: 3 }); // "F2 R2 F D2 L U2 L U2 F2 D2 R' F2 L' D' B2 R2 F2 R2 F2 R2"
```

Returns **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** The formatted scramble.

### parse

Takes a given string and parses it into a scramble of [Move](#move) objects.

**Parameters**

-   `scrambleString` **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** The string to be parse as a scramble.

**Examples**

```javascript
import { parse } from 'scramble-generator';
parse("R' U F D2");
// [ { face: 'R', longFace: 'RIGHT', inverted: true, double: false },
// { face: 'U', longFace: 'UP', inverted: false, double: false },
// { face: 'F', longFace: 'FRONT', inverted: false, double: false },
// { face: 'D', longFace: 'DOWN', inverted: false, double: true } ]

parse("R J Q D2 F U'"); // null
```

Returns **([Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[Move](#move)> | null)** An array of Move objects representing the given scramble, or null if the scramble isn't valid.
