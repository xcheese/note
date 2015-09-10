# set

ES6提供了新的数据结构Set。它类似于数组，但是成员的值都是唯一的，没有重复的值。

Set本身是一个构造函数，用来生成Set数据结构。

```
var items = new Set([1,2,3,4,5,5,5,5]);
items.size // 5
```

向Set加入值的时候，不会发生类型转换，所以5和“5”是两个不同的值。Set内部判断两个值是否不同，使用的算法类似于精确相等运算符（===），这意味着，两个对象总是不相等的。唯一的例外是NaN等于自身（精确相等运算符认为NaN不等于自身）。

```
let set = new Set();

set.add({})
set.size // 1

set.add({})
set.size // 2
```
由于两个空对象不是精确相等，所以它们被视为两个值。

* add(value)：添加某个值，返回Set结构本身。
* delete(value)：删除某个值，返回一个布尔值，表示删除是否成功。
* has(value)：返回一个布尔值，表示该值是否为Set的成员。
* clear()：清除所有成员，没有返回值。

```
s.add(1).add(2).add(2);
// 注意2被加入了两次

s.size // 2

s.has(1) // true
s.has(2) // true
s.has(3) // false

s.delete(2);
s.has(2) // false
```

Array.from方法可以将Set结构转为数组。

	var items = new Set([1, 2, 3, 4, 5]);
	var array = Array.from(items);
	
这就提供了一种去除数组的重复元素的方法。

```
function dedupe(array) {
  return Array.from(new Set(array));
}

dedupe([1,1,2,3]) // [1, 2, 3]
```
### 遍历set

```
let set = new Set(['red', 'green', 'blue']);

for (let x of set) {
  console.log(x);
}
// red
// green
// blue
```

* 由于扩展运算符（...）内部使用for...of循环，所以也可以用于Set结构。

```
let set = new Set(['red', 'green', 'blue']);
let arr = [...set];
// ['red', 'green', 'blue']
```
* 除数组重复元素的方法

```
let arr = [3, 5, 2, 2, 5, 5];
let unique = [...new Set(arr)];
```


	
* 数组的map和filter方法也可以用于Set了

```
let set = new Set([1, 2, 3]);
set = new Set([...set].map(x => x * 2));
// 返回Set结构：{2, 4, 6}

let set = new Set([1, 2, 3, 4, 5]);
set = new Set([...set].filter(x => (x % 2) == 0));
// 返回Set结构：{2, 4}
```
* 并集（Union）和交集（Intersect）

```
let a = new Set([1, 2, 3]);
let b = new Set([4, 3, 2]);

let union = new Set([...a, ...b]);
// [1, 2, 3, 4]

let intersect = new Set([...a].filter(x => b.has(x)));
```

# map

更适合hash类型

Object的键值对，键只能是字符串，而map可以存任一数据类型

```
var m = new Map();
var o = {p: "Hello World"};

m.set(o, "content")
m.get(o) // "content"

m.has(o) // true
m.delete(o) // true
m.has(o) // false
```
如果对同一个键多次赋值，后面的值将覆盖前面的值。

```
let map = new Map();
map.set(1, 'aaa');
map.set(1, 'bbb');
map.get(1) // "bbb"
```

读取一个未知的键，则返回undefined。

```
new Map().get('asfddfsasadf')
// undefined
```

##### 注意，只有对同一个对象的引用，Map结构才将其视为同一个键。这一点要非常小心。

```
var map = new Map();

map.set(['a'], 555);
map.get(['a']) // undefined
```
Map的键实际上是跟内存地址绑定的，只要内存地址不一样，就视为两个键。这就解决了同名属性碰撞（clash）的问题，我们扩展别人的库的时候，如果使用对象作为键名，就不用担心自己的属性与原作者的属性同名。

## 实例的属性和操作方法

* size：返回成员总数。
* set(key, value)：设置key所对应的键值，然后返回整个Map结构。如果key已经有值，则键值会被更新，否则就新生成该键。
* get(key)：读取key对应的键值，如果找不到key，返回undefined。
* has(key)：返回一个布尔值，表示某个键是否在Map数据结构中。
* delete(key)：删除某个键，返回true。如果删除失败，返回false。
* clear()：清除所有成员，没有返回值。

set()方法返回的是Map本身，因此可以采用链式写法。

```
let map = new Map()
  .set(1, 'a')
  .set(2, 'b')
  .set(3, 'c');
```
##### 遍历

* keys()：返回键名的遍历器。
* values()：返回键值的遍历器。
* entries()：返回所有成员的遍历器。

```
let map = new Map([
  ['F', 'no'],
  ['T',  'yes'],
]);

for (let key of map.keys()) {
  console.log(key);
}
// "F"
// "T"

for (let value of map.values()) {
  console.log(value);
}
// "no"
// "yes"

for (let item of map.entries()) {
  console.log(item[0], item[1]);
}
// "F" "no"
// "T" "yes"
```
Map结构转为数组结构。

```
let map = new Map([
  [1, 'one'],
  [2, 'two'],
  [3, 'three'],
]);

[...map.keys()]
// [1, 2, 3]

[...map.values()]
// ['one', 'two', 'three']

[...map.entries()]
// [[1,'one'], [2, 'two'], [3, 'three']]

[...map]
// [[1,'one'], [2, 'two'], [3, 'three']]
```

### 与其他数据结构的互相转换

* Map转为数组

```
let myMap = new Map().set(true, 7).set({foo: 3}, ['abc']);
[...myMap]
// [ [ true, 7 ], [ { foo: 3 }, [ 'abc' ] ] ]
```
* 数组转为Map

```
new Map([[true, 7], [{foo: 3}, ['abc']]])
// Map {true => 7, Object {foo: 3} => ['abc']}
```

* Map转为对象

```
function strMapToObj(strMap) {
  let obj = Object.create(null);
  for (let [k,v] of strMap) {
    obj[k] = v;
  }
  return obj;
}

let myMap = new Map().set('yes', true).set('no', false);
strMapToObj(myMap)
// { yes: true, no: false }
```

* 对象转为Map

```
function objToStrMap(obj) {
  let strMap = new Map();
  for (let k of Object.keys(obj)) {
    strMap.set(k, obj[k]);
  }
  return strMap;
}

objToStrMap({yes: true, no: false})
// [ [ 'yes', true ], [ 'no', false ] ]
```	

