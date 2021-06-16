---
sidebar_position: 3
---

# Valve

An object that can exist on the end of a pipe. It reads properties from the connected pipe and destination node.

Its `pressure.out` must not be higher than its `pressure.in`.

## Properties

| Property          | Unit | Default | Notes                                          |
| ----------------- | ---- | ------- | ---------------------------------------------- |
| `name`            | -    | valve   |                                                |
| `pressure.in`     | Pa   | 0       | From pipe                                      |
| `pressure.out`    | Pa   | 0       | From node                                      |
| `temperature.in`  | K    | 0       | From pipe                                      |
| `temperature.out` | K    | 0       | Calculated based on contant enthalpy expansion |

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
