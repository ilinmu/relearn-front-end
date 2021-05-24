# 数据类型

- Number

```jsx
Number.MAX_SAFE_INTEGER + 1 === Number.MAX_SAFE_INTERGER + 2;  // true
Number.NaN === Number.NaN;  // false
// NaN 实际上是一群特殊数字的合集， 9007199254740990
// 所以 NaN !== NaN，又因为它是一组数字的合集，所以通过 isNaN 可以判断是否是 NaN

0/0;  // NaN
1/0;  // Infinity
1/-1;  // -Infinity
// How to judge a number is 0 or -0?
let x = 0;
1/x === Number.Infinity ? 0 : -0;

0.1 + 0.2 == 0.3;  // false
0.1 + 0.2 = 0.30000000000000004;
// Number 类型为双精度IEEE 754 64 位浮点
// 由于浮点数在存储中无法精确表示，所以无法通过 a == b 来判断两个浮点数是否相等。
// 通过 Number.EPSILON 最小精度来判断则可以得到正确返回
Math.abs(0.1 + 0.2 - 0.3) <= Number.EPSILON;  // true
// 确定保留小数后几位，转换成整型后进行计算比较
```

- BigInt

```jsx
// 可以操作超过 Number.MAX_SAFE_INTEGER 值。通常在整数末尾加  n  来创建。
let x = 9007199254740992n;
let y = x + 1n;
y = 9007199254740993n;

typeof y // 'bigint'
```

- String
- Boolean
- Undefined

```jsx
// 一个没有被赋值的变量类型是 undefined。
// 为什么有些编程规范要求用 void 0 代替 undefined？
// 因为 undefined 不是保留字，它可以是一个变量，也就存在被更改的可能性
let a;
a === void 0  // true
```

- Null
- Object

```jsx
typeof [];
typeof {};
typeof new Map(); // WeakMap
typeof new Set();  // WeakSet
typeof new Date();
```

- Symbol

```jsx
// 每个从Symbol()返回的symbol值都是唯一的。用以创建无重复 key 的 Object。
const foo = Symbol();
const bar = Symbol();
typeof foo;  // 'symbol'
let obj = {};
obj[foo] = 'foo';
obj[bar] = 'bar';
JSON.stringify(obj);  // "{}"
Object.keys(obj);  // []
Object.getOwnPropertyNames(obj);  // []
Object.getOwnPropertySymbols(obj);  // [Symbol(), Symbol()]
```

```jsx
// 为什么基础类型可以操作方法，以及添加在对象上的方法，为什么可以用在基础类型上？
let str = 'abc';
str.charAt(0);
(1).toFixed(2);
String.prototype.hello = () => console.log('hello string');
str.hello();

// Boolean Number String 是 ECMAScript 为了方便操作原始值，提供的三种特殊引用类型。
// 每当用到基础值得属性或者方法时，后台都会自动创建一个原始值包装类型得对象，从而暴露出各种方法，在调用后，销毁对象
// 即调用方法时，会有一个装箱的操作。
```

### 类型转换

```jsx
// StringToNumber
Number('10');
Number.parseFloat('12.34');
Number.parserInt('12.34');

// NumberToString
String('10');

// 在 Javascript 中，没有任何方法可以更改私有的 Class 属性
// 因此 Object.prototype.toString 可以准确识别对象对应的基本类型，比 instanceof 更加准确。

// 装箱转化
let symbolObject = Object(Symbol('a'));
Object.prototype.toString.call(symbolObject);  // [object Symbol]
Object.prototype.toString.call([]);  // [object Array]
// 仅作为举例，基础类型直接用 typeof 即可
Object.prototype.toString.call('123');  // [object String]

// 拆箱转换
// 拆箱转换会尝试调用 valueOf 和 toString 方法来获得基本类型。
// 如果 valueOf 和 toString 都不存在，或者没有返回基本类型，则会产生 TypeError。
// 类型转换时，调用 toPrimitive 方法，默认转换类型为 number, valueOf 先执行，如果执行为 string 则 toString 先执行。

// Symbol.toPrimitive 是一个内置的 Symbol 值
// 它是作为对象的函数值属性存在的，当一个对象转换为对应的原始值时，会调用此函数。
const obj = {
	[Symbol.toPrimitive](hint) {
		if (hint === 'number') {
			return 10;
		}
		if (hint === 'string') {
			return 'world';
		}
		return 'default';
	}
}

obj * 2;  // 20
'hello' + ' ' + String(obj);  // hello world
'hello' + ' ' + obj;  // hello default

```