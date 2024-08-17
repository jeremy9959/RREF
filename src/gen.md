---
title: Testing
---





```js
function* f() {
    for(let j=0;j<100; j++) {
        yield j ; 
    }
}
```

<button onclick="console.log(f().next())">
