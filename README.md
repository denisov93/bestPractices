# bestPractices


## Deep copy Alternative - structuredClone

- structuredClone(value)
- structuredClone(value, options)
  - `options` - Is an object {transfer: []}
    - `transfer` - An array of transferable objects that will be moved rather than cloned to the returned object. 
- Transferable objects are commonly used to share resources that can only be safely exposed to a single JavaScript thread at a time
```js
const original = new Uint8Array(1024);
const clone = structuredClone(original);
console.log(original.byteLength); // 1024
console.log(clone.byteLength); // 1024

original[0] = 1;
console.log(clone[0]); // 0

// Transferring the Uint8Array would throw an exception as it is not a transferable object
// const transferred = structuredClone(original, {transfer: [original]});

// We can transfer Uint8Array.buffer.
const transferred = structuredClone(original, { transfer: [original.buffer] });
console.log(transferred.byteLength); // 1024
console.log(transferred[0]); // 1

// After transferring Uint8Array.buffer cannot be used.
console.log(original.byteLength); // 0



const calendarEvent = {
  title: "Builder.io Conf",
  date: new Date(123),
  attendees: ["Steve"],
}

const copy = structuredClone(
  calendarEvent,
)
```


## Optional chaining (?.)
The optional chaining (?.) operator accesses an object's property or calls a function. If the object accessed or function called using this operator is undefined or null, the expression short circuits and evaluates to undefined instead of throwing an error.

```js
const adventurer = {
  name: 'Alice',
  cat: {
    name: 'Dinah'
  }
};

const dogName = adventurer.dog?.name;
console.log(dogName);
// Expected output: undefined

console.log(adventurer.someNonExistentMethod?.());
// Expected output: undefined
```

## Typescript/JS
### Don´t suck at memory allocation
For example, this function just stores values in memory somewhere
```ts
function createPoint(x,y) {
  return {
    x, y
  };
};
```
When you create an array, your data will be stored randomly in the space of the memory.
```ts
const list = [
  createPoint(42,420),
  createPoint(69,420),
  createPoint(420,69),
  createPoint(420,42),
];
```
Here is an example of how it could be stored:

![image](https://github.com/denisov93/bestPractices/assets/43911862/f7cdc6da-4743-42d9-a6ee-59b8b0eea77c)

But if you store this like this, explicitly identifying the memory
```ts
const compact = new Float64Array(8);
function readPoint(points, offset) {
  return createPoint(points[offset], points[offset + 1]);
};
```
This will allocate specific sequential memory and store in that specific chunk the information you need like this image
![image](https://github.com/denisov93/bestPractices/assets/43911862/1d45c164-c92d-4610-9970-2994a179a8ea)




