
[![NPM](https://img.shields.io/badge/Module-Npm-blue.svg)](https://www.npmjs.com/package/hashbounds)
[![Donate](https://img.shields.io/badge/Donate-Paypal-brightgreen.svg)](https://paypal.me/andrews54757)

[![Demo](https://cloud.githubusercontent.com/assets/13282284/23081424/b7cd5f16-f522-11e6-8fe9-dfdde154340d.png)](https://threeletters.github.io/HashBounds/browser/visual/)

# HashBounds
A super efficient collision check reducer. Also a good snack made out of potato made for programmers on their lunch breaks

# Usage
> npm install hashbounds

##### Hashbounds(baseCellSize,lvls,preloadX,preloadY)
* baseCellSize - Size of the largest cells (In squares of 2).
* lvls - Number of levels of sizes to go down. (EG: if baseCellSize is 4, and lvls is 1, the smallest cell will be size 3)
* preloadX/preloadY - OPTIONAL: Preloads by creating buckets at initialization

```js
var HashBounds = require('hashbounds')
var hashBounds = new HashBounds(10,2,100,100) // size of base cells (In squares of 2), amount of levels, Preload valueX,Y
var node = {
    hello: "world"
}
var bounds = {
    x: 10,
    y: 10,
    width: 5,
    height: 3
}

hashBounds.insert(node,bounds) // add node

bounds.x = 4

hashBounds.update(node,bounds) // Update node (for moving objects)

var searchBounds = {
    x: 3,
    y: 0,
    width: 10,
    height: 10
}
var nodes = hashBounds.toArray(searchBounds) // gets nodes that is in/near the bounds
console.log(nodes.length)
hashBounds.delete(node)// delete node
```


## How does it work?

https://github.com/ThreeLetters/HashBounds/blob/master/EXPLANATION.md

## Requirements
In order for this to work, you must pass a `bounds` object into the insert/update/query functions

```js

// Bounds can look like this:

var bounds = {
        x: 0, // x
        y: 0, // y
        width: 5, // width
        height: 5
    }
    
// Or look like this:

var bounds = {
        minX: 0,
        minY: 0,
        maxX: 5,
        maxY: 5
    }
```

## Browser

`<script type="text/javascript" src="https://cdn.rawgit.com/ThreeLetters/HashBounds/master/browser/HashBounds.js"></script>`

## Methods:

1. insert(node,bounds): Insert a node into the map
2. delete(node): Remove a node from the map
3. update(node,bounds): Update the node in the map
4. clear(): Clear all nodes in the map
5. toArray(bounds): Get an array of objects in certain bounds
6. forEach(bounds,call): Loop through objects in certain bounds
7. every(bounds,call): Same as .forEach(bounds), but stops execution when returning false

## Efficiency

### Insert/delete/update - O(1)

Insert, delete, and update are the most efficient operations for HashBounds. The level of an object and it's position in that level is determined at O(1) time. Since the hashes are "layered", an object will generally belong to 4 buckets max. However, if the object is larger than the biggest bucket, then the efficiency would be the same in a normal spatial hash.

### forEach/every/toArray - O(n log(n))

Looking for objects generally take O(n log(n)) time if the search area is smaller than the greatest bucket as objects are retrieved in a quad-tree like fashion. This way, empty "branches of buckets" are quickly tossed, and costly loops are avoided.


