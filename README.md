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


