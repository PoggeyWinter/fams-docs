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
| `temperature` | K    | 0       | Not used yet, not sure about the unit            |
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

### _set_ inflow

Updates `flow.in` and `flow.out` (mass conservation).

### _get_ type

#### Closed

A node that has zero inflow and outflow.

#### Source

A node whose outflow is greater than its inflow.

#### Destination

A node whose inflow is greater than its outflow.

#### Internal

A node that has equal (and non-zero) inflow and outflow.
