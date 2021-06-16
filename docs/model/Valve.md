---
sidebar_position: 3
---

# Valve

An object that can exist on the end of a pipe. It reads properties from the connected pipe and destination node.

Its `pressure.out` must not be higher than its `pressure.in`.

## Properties

| Property       | Unit | Default             | Notes              |
| -------------- | ---- | ------------------- | ------------------ |
| `name`         | -    | valve               |                    |
| `pressure.in`  | Pa   | Pipe `pressure.out` | Required, non-zero |
| `pressure.out` | Pa   | Node `pressure`     | Required, non-zero |

### Constructor parameters

```js
export interface IValve {
  name?: string
  pressure?: {
    in: number
    out: number
  }
  temperature?: {
    in: number
  }
}
```
