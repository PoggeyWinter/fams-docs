---
sidebar_position: 3
---

# Pipe

A section is a [network](/docs/model/Network) of [pipes](/docs/model/Pipe) that (approximately uniformly) spans the gap between two [nodes](/docs/model/Node), or covers a specified length. If a source or destination node is not provided, one will be created.

Its resolution specifies the length of the pipes in the series. If the length is not perfectly divisible by the resolution, remainder will be the length of the last pipe.

:::tip Plan

`Pipes` will be allowed to be nested within `Networks` to connect nodes with fixed positions.

:::

## Properties

| Property      | Unit | Default      | Notes                                                     |
| ------------- | ---- | ------------ | --------------------------------------------------------- |
| `name`        | -    | pipe         |                                                           |
| `resolution`  | m    | 200          | Pipe segment length                                       |
| `length`      | m    | 200          | Overall length                                            |
| `source`      | -    | new `Node()` | Must be a `Node` object                                   |
| `destination` | -    | new `Node()` | Must be a `Node` object                                   |
| `cosine`      | -    | 1            | Calculated based on source and destination node positions |

### Constructor parameters

```js
interface IPipe {
  name?: string
  resolution?: number
  length?: number
  source?: Node
  destination?: Node
}
```

## Methods

### `chain()`

Generates a network of linked pipes, starting with the source node and ending with the destination node. Called during initialisation.

Returns the generated network.

### _get_ `height`

Returns the elevation difference between the destination and source nodes.
