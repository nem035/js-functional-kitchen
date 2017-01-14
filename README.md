# JSFunctionalKitchen

Mini functional recipes in JavaScript.

## Methods

### `callFirst` and `callLast`

```js
const callFirst = (fn, ...leftArgs) => (...rest) => fn(...leftArgs, ...rest);
const callLast = (fn, ...rightArgs) => (...rest) => fn(...rest, ...rightArgs);

const debugLogger = callFirst(console.log, 'DEBUG: ');
debugLogger('a', 'b') // 'DEBUG: a b'

const secondTimer = callLast(setTimeout, 1000);
secondTimer(() => {
  console.log('1 second!');
});
```

### `unary`

```js
const unary = (fn) => fn.length === 1
  ? fn
  : (firstArg) => fn(firstArg);

['1', '2', '3'].map(unary(parseInt)) // [1, 2, 3]
```

### `tap`

```js
const tap = (value) => (fn) =>
  (typeof(fn) === 'function' && fn(value), value)

const tap5 = tap(5);

tap5(console.log); // returns and logs 5
```

### `maybe`

```js
const maybe = (fn) => (...args) => {
  if (args.length === 0) return;
  for (const arg of args) {
    if (arg === null || arg === undefined) return;
  }
  return fn(...args);
};

maybe(sum)(1, 2, 3)    // 6
maybe(sum)(1, null, 3) // undefined
```

### `once`

```js
const once = (fn) => (...args) => {
  let done = false;
  return done
    ? void 0
    : fn(..args);
}

const authenticate = once(login);
authenticate('admin', '123').then(console.log);
authenticate('admin', '123').then(console.log); // Error: undefined is not a function
```

### `firstAndRest`

```js
const firstAndRest = (first, ...rest) => [first, rest];

const logMessage = ['DEBUG:', '1', '2', '3'];
const [prefix] = firstAndRest(...logMessage); // 'DEBUG:'
```

### `restAndLast`

```js
const restAndLast = (...args) => {
  if (args.length < 1) return void 0;
  const last = args[args.length - 1];
  const rest = args.slice(0, args.length - 1);
  return [rest, last];
}

const cString = ['n', 'e', 'm', 'a', 'n', 'j', 'a', '\0'];
const [letters] = restAndLast(...cString); // ['n', 'e', 'm', 'a', 'n', 'j', 'a']
```

### `takeFirst` and `takeLast`

```js
const takeFirst = (amt) => (list) => list.slice(0, amt);
const takeLast = (amt) => (list) => list.slice(-amt);

takeFirst(3)([1, 2, 3, 4, 5]) // [1, 2, 3]
takeLast(3)([1, 2, 3, 4, 5]) // [3, 4, 5]
```

### `mapWith`

```js
const mapWith = (fn) => (list) => list.map(fn);
const squaresOf = mapWith((x) => x ** 2);
squaresOf([1,2,3,4,5]) // [1, 4, 9, 16, 25]
```

### `flip`

```js
const flip = (fn) => (a, b) => fn(b, a);

const callAfter = flip(setTimeout);
callAfter(1000, () => {
  console.log('1 second!');
});
```

### `flipAndCurry`

```js
const flipAndCurry = (fn) => (a) => (b) => fn(b, a);

const callAfterSecond = flipAndCurry(setTimeout)(1000);
callAfterSecond(() => {
  console.log('1 second!');
});
```
