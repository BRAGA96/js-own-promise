# Own promise example

## with callback
```javascript
promise((resolve) => {
    setTimeout(() => {
        resolve('data');
    }, 1e3);
}).then((data) => {
    console.log(data); // "data" after 1s
});
```

---

## as instance
```javascript
const instance = promise();

instance.then((data) => {
    console.log(data); // "data" after 1s
});

setTimeout(() => {
    instance.resolve('data');
}, 1e3);
```

---

## static method `resolve`
```javascript
promise.resolve(1).then((data) => {
    console.log(data); // 1
}).catch((error) => {
    console.log(error); // never
});
```

---

## static method `reject`
```javascript
promise.reject(1).then((data) => {
    console.log(data); // never
}).catch((error) => {
    console.log(error); // 1
});
```

---

## static method `delay`
```javascript
promise.delay(1e3).then(() => {
    console.log('resolved'); // "resolved" after 1s
});
```

---

## static method `all`
```javascript
promise.all(
    promise.delay(1e3).then(() => 1),
    promise.delay(3e3).then(() => 3),
    promise.delay(5e3).then(() => 5)
).then(([a, b, c]) => {
    console.log(a, b, c); // 1, 3, 5 after Math.max(1e3, 3e3, 5e3)s
});
```

## static method `race`
```javascript
promise.race(
    promise.delay(1e3).then(() => 1),
    promise.delay(3e3).then(() => 3),
    promise.delay(5e3).then(() => 5)
).then((value) => {
    console.log(value); // 1 after Math.min(1e3, 3e3, 5e3)s
});
```

---

## static method `queue`
```javascript
promise.queue([1, 2, 3], (value) => {
    return promise.delay(value * 1e3).then(() => value);
}).then(([a, b, c]) => {
    console.log(a, b, c); // 1, 2, 3 after (1 + 2 + 3)s
});
```

---

## static method `parallel`
```javascript
promise.parallel([1, 2, 3], (value) => {
    return promise.delay(value * 1e3).then(() => value);
}).then(([a, b, c]) => {
    console.log(a, b, c); // 1, 2, 3 after Math.max(1, 2, 3)s
});
```

---

## chaining
```javascript
promise.resolve(1).then((a) => a + 1).then((a) => a + 1).then((data) => {
    console.log(data); // (1 + 1); (2 + 1) = 3
});
```

---

## chaining with promises
```javascript
promise.resolve()
    .then(() => promise.delay(3e3).then(() => 3))
    .then((value) => console.log(value)) // 3 after 3s
    .then(() => promise.delay(3e3).then(() => 6))
    .then((value) => console.log(value)) // 6 after 6s
```

---

## catch handler
```javascript
promise(() => {
    window.gesg.gesgs();
}).then((data) => {
    console.log(data); // never
}).catch((error) => {
    console.log(error); // log TypeError: Cannot read property 'gesgs' of undefined
});
```

---

## errors without catch handler
```javascript
promise(() => {
    window.gesg.gesgs(); // throw Uncaught TypeError: Cannot read property 'gesgs' of undefined
});
```

---

## async/await
```javascript
;(async () => {
    const a = await promise.delay(1e3).then(() => 1);
    const b = await promise.delay(1e3).then(() => 2);
    const c = await promise.delay(1e3).then(() => 3);

    console.log(a, b, c); // 1, 2, 3 after (1 + 1 + 1)s
})();
```