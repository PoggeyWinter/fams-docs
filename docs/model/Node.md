---
sidebar_position: 1
---

# Node

Nodes are the points at the start and end of each pipeline, whether that's a CO2 supplier, a manifold, or a wellhead.

The input and output properties of a node can be specified or calculated.

## Properties

| Property      | Unit | Default | Notes                                            |
| ------------- | ---- | ------- | ------------------------------------------------ |
| `name`        | -    | node    |                                                  |
| `x`           | m    | 0       | Position                                         |
| `elevation`   | m    | 0       | Position, relative to sea level                  |
| `pressure`    | Pa   | 0       |                                                  |
| `temperature` | K    | 0       |                                                  |
| `flow.in`     | kg/s | 0       | Inward mass flow rate                            |
| `flow.out`    | kg/s | 0       | Outward mass flow rate                           |
| `type`        | -    | closed  | `closed`, `internal`, `source`, or `destination` |

### Constructor parameters

```js
export interface INode {
  name?: string
  x?: number
  elevation?: number
  pressure?: number
  temperature?: number
  flow?: {
    in?: number
    out?: number
  }
}
```

## Methods

### _set_ `inflow(i: number)`

Updates `flow.in` and `flow.out` (mass conservation).

### _get_ `type`

#### Closed

A node that has zero inflow and outflow.

#### Source

A node whose outflow is greater than its inflow.

#### Destination

A node whose inflow is greater than its outflow.

#### Internal

A node that has equal (and non-zero) inflow and outflow.

### _get_ `density`

```js
get density() {
  // ρ=(Pμ)/(RT)
  const decimalPlaces = 1
  const μ = 0.044
  const R = 8.31462
  return Number(((this.pressure * μ) / (R * this.temperature)).toFixed(decimalPlaces))
}
```

### _get_ `viscosity`

```js
get viscosity() {
  const μ0 = 0.018 // Ref viscosity
  const T0 = 373 // Ref temperature
  const C = 240 // Southerland constant
  const T = this.temperature
  return μ0 * ((T0 + C) / (T + C)) * (T / T0) ** (3 / 2)
}
```
