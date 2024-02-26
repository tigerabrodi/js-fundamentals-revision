# JS Fundamentals Notes

# Promises

The Promise object is used for deferred and asynchronous computations. A Promise represents a value which may be available now, or in the future, or never.

It has three states:

- Pending
- Fulfilled
- Rejected

Looking at this piece of code:

```js
new Promise((resolveOuter) => {
  resolveOuter(
    new Promise((resolveInner) => {
      setTimeout(resolveInner, 1000);
    })
  );
});
```

We have a promise that resolves to another promise. The outer promise is pending until the inner promise resolves. The outer promise is fulfilled when the inner promise is fulfilled. Therefore the outer promise is resolved 1 second later.

## Chaining

When a promise becomes settled, you can use some of its methods to do further action: `.then()`, `.catch()`, and `.finally()`.

```js
const myPromise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("foo");
  }, 300);
});

myPromise
  .then(handleFulfilledA, handleRejectedA)
  .then(handleFulfilledB, handleRejectedB)
  .then(handleFulfilledC, handleRejectedC);
```

`.then()` takes two arguments, the first one is a function to be called if the promise is fulfilled, and the second one is a function to be called if the promise is rejected.

`.catch()` is a shorthand for `.then(null, handleRejected)`. It is used to handle the errors.

`.finally()` is used to run some code after the promise is settled.

## Promise Concurrency

There are 4 methods to handle multiple promises:

- `Promise.all()`: waits for all promises to be resolved or for any to be rejected.
- `Promise.allSettled()`: waits for all promises to be settled
- `Promise.race()`: waits for the first promise to be resolved or rejected.
- `Promise.any()`: waits for the first promise to be resolved.

## Async/Await

Async/await is a new way to write asynchronous code. It is built on top of promises and non-blocking. It makes the code look synchronous, but it is still asynchronous.

```js
async function myFunction() {
  try {
    const valueA = await promiseA;
    const valueB = await promiseB;
    const valueC = await promiseC;
  } catch (error) {
    console.error(error);
  }
}
```

The `await` keyword is used to wait for a promise to be resolved. If the promise is rejected, the function will throw an error.

`async` functions are functions that return a promise. The `await` keyword is used to wait for the promise to be resolved.

`try/catch` blocks are used to handle errors that may occur in the asynchronous code.

`promiseB` won't be executed until `promiseA` is resolved. If you want to execute them concurrently, you can use `Promise.all()`.
