# Use-JS-console-Handler

# console.js

```javascript

export default {
    info() {
        console.info.apply(console,arguments);
    },
    log() {
        console.info.apply(console,arguments);
    },
    error() {
        console.error.apply(console,arguments);
    },
    warn() {
        console.warn.apply(console,arguments);
    }
}

```

