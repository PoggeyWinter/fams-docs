---
sidebar_position: 2
---

# Pipe

Pipes are connections between [nodes](/docs/model/Node).

A pipe's `in pressure` and `mass flow rate` are determined by its `source node`.

## Properties

| Property       | Unit                | Default      | Notes                                                                                  |
| -------------- | ------------------- | ------------ | -------------------------------------------------------------------------------------- |
| `name`         | -                   | pipe         |                                                                                        |
| `length`       | m                   | 200          |                                                                                        |
| `diameter`     | m                   | 2            |                                                                                        |
| `massFlow`     | kg/s                | 1            |                                                                                        |
| `pressure.in`  | Pa                  | 0            |                                                                                        |
| `pressure.out` | Pa                  | 0            |                                                                                        |
| `source`       | -                   | new `Node()` | Must be a `Node` object                                                                |
| `destination`  | -                   | new `Node()` | Must be a `Node` object                                                                |
| `direction`    | `boolean` \| `null` | `null`       | `true` if `pressure.in` > `pressure.out`<br/>`false` if `pressure.out` > `pressure.in` |
| `valve`        | `Valve` \| `false`  | `false`      |                                                                                        |

The `Pipe()` constructor accepts an `x` input, which sets the x position of its source node.

### Constructor parameters

```js
export interface IPipe {
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

### destinationPressure()

A pipe's `pressure.out` is calculated based on its physical properties and the properties of the fluid. This is used to set the pressure at the destination node.

```js
destinationPressure(): number {
  const P1 = this.pressure.in
  const viscosity = this.source.viscosity // fluid property
  const L = this.length
  const A = 0.25 * Math.PI * this.diameter ** 2
  const d = this.diameter * 1000 // mm
  const z = this.destination.elevation - this.source.elevation
  const g = 9.81
  const density = this.source.density
  const Q = this.massFlow / density // volumetric flow rate
  return P1 - (32000 * (viscosity * L * Q)) / (A * d ** 2) - z * g * density
}
```

### addValve()

Adds a new [Valve](/docs/model/Valve) to the end of the pipe.

The valve's pressure is determined by the `pressure.out` of the pipe and `pressure` of the destination node.

### removeValve()

Removes the pipe's [Valve](/docs/model/Valve).

### _set_ source

Updates the private `_source` property to be the received [node](/docs/model/Node) then triggers side effects:

- Updates `pressure.in` to match the new source pressure.
- Updates `pressure.out` to be `pressure.in - this.pressureDrop()`

### _set_ destination

Sets pressure of the new destination node to the lowest of `pressure.out` and `node.pressure`.

Sets `pressure.out` to the new `node.pressure` value.

Updates the private `_destination` property to be the received [node](/docs/model/Node).

### _get_ pressureContinuity()

Returns true if the pipes `pressure.out` matches the pressure at the destination node.
