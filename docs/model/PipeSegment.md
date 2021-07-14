---
sidebar_position: 2
---

# Pipe Segment

Pipe segments are connections between [nodes](/docs/model/Node).

A pipe segment's `in pressure` and `mass flow rate` are determined by its `source node`.

## Properties

| Property       | Unit                | Default      | Notes                                                                                  |
| -------------- | ------------------- | ------------ | -------------------------------------------------------------------------------------- |
| `name`         | -                   | pipeseg      |                                                                                        |
| `length`       | m                   | 200          |                                                                                        |
| `diameter`     | m                   | 2            |                                                                                        |
| `massFlow`     | kg/s                | 1            |                                                                                        |
| `pressure.in`  | Pa                  | 0            |                                                                                        |
| `pressure.out` | Pa                  | 0            |                                                                                        |
| `source`       | -                   | new `Node()` | Must be a `Node` object                                                                |
| `destination`  | -                   | new `Node()` | Must be a `Node` object                                                                |
| `direction`    | `boolean` \| `null` | `null`       | `true` if `pressure.in` > `pressure.out`<br/>`false` if `pressure.out` > `pressure.in` |
| `valve`        | `Valve` \| `false`  | `false`      |                                                                                        |
| `roughness`    | -                   | 0            |                                                                                        |

The `PipeSegment()` constructor accepts an `x` input, which sets the x position of its source node.

### Constructor parameters

```js
export interface IPipeSegment {
  name?: string
  length?: number
  diameter?: number
  massFlow?: number
  source?: Node
  destination?: Node
  x?: number
  endElevation?: number
}
```

## Methods

### `destinationPressure()`

A pipe segment's `pressure.out` is calculated based on its physical properties and the properties of the fluid. This is used to set the pressure at the destination node.

```js
destinationPressure(): number {
  const w = this.massFlow
  const D = this.diameter
  const A = 0.25 * Math.PI * this.diameter ** 2
  const ρ = this.source.density
  const v = 1 / ρ
  const L = this.length
  const P1 = this.pressure.in

  // Friction factor
  const u = w / (A * ρ)
  const μ = this.source.viscosity
  const Re = (ρ * u * D) / μ
  const f = Re < 2000 ? 64 / Re : 0.094 / (D * 1000) ** (1 / 3)

  return (
    (A * Math.sqrt(D)) ** -1 *
    Math.sqrt(P1) *
    Math.sqrt(A ** 2 * D * P1 - f * L * v * w ** 2)
  )
}
```

### `addValve()`

Adds a new [Valve](/docs/model/Valve) to the end of the pipe segment.

The valve's pressure is determined by the `pressure.out` of the pipe segment and `pressure` of the destination node.

### `removeValve()`

Removes the pipe segment's [Valve](/docs/model/Valve).

### _set_ `source(n: Node)`

Updates the private `_source` property to be the received [node](/docs/model/Node) then triggers side effects:

- Updates `pressure.in` to match the new source pressure.
- Updates `pressure.out` to be `pressure.in - this.pressureDrop()`

### _set_ `destination(n: Node)`

Sets pressure of the new destination node to the lowest of `pressure.out` and `node.pressure`.

Sets `pressure.out` to the new `node.pressure` value.

Updates the private `_destination` property to be the received [node](/docs/model/Node).

### _get_ `pressureContinuity`

Returns true if the pipe segment's `pressure.out` matches the pressure at the destination node.
