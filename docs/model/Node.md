---
sidebar_position: 1
---

# Node

Nodes are the points at the start and end of each pipeline, whether that's a CO2 supplier, a manifold, or a wellhead.

The input and output properties of a node can be specified or calculated.

## Properties

| Property      | Unit | Default | Notes                                                                                                                                                                   |
| ------------- | ---- | ------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `name`        | -    | node    |                                                                                                                                                                         |
| `x`           | m    | 0       | Position                                                                                                                                                                |
| `elevation`   | m    | 0       | Position, relative to sea level                                                                                                                                         |
| `pressure`    | Pa   | 0       |                                                                                                                                                                         |
| `temperature` | K    | 0       | Not used yet, not sure about the unit                                                                                                                                   |
| `inflow`      | kg/s | 0       | Inward mass flow rate                                                                                                                                                   |
| `outflow`     | kg/s | 0       | Outward mass flow rate                                                                                                                                                  |
| `type`        | -    | closed  | `closed` - `outflow` and `inflow` are both zero<br/>`internal` - `inflow` equals `outflow`<br/>`source` - `outflow` > `inflow`<br/>`destination` - `outflow` < `inflow` |

When `inflow` changes, `outflow` is updated to match.
