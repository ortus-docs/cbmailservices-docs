---
description: Accept mail and silently discard it.
icon: ban
---

# Null

Does nothing and always reports success. Nothing is stored. Use it to disable mail in an environment or to mock delivery.

```javascript
mailers : {
    "null" : { class : "Null" }
}
```

It has no properties. If you need to inspect what was sent, use [InMemory](inmemory.md) instead.
