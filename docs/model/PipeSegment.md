---
sidebar_position: 3
---

# Pipe Segment

Pipe segments extend the [Point](/docs/model/Point) class.

Their additional properties can be seen below.

## Constructor parameters

```js
interface IPipeSeg {
  name: string
  diameter?: number
  length?: number
  flowrate?: number
  roughness?: number
  start?: {
    pressure?: number
    viscosity?: number
    temperature?: number
    x?: number
    y?: number
  }
}
```

## Methods

### `inflow()`

Returns the sum of the flowrates of the preceding nodes.

### `density()`

Returns the density of the fluid at the start of the pipe segment.

```js
density(): number {
  // ρ=(Pμ)/(RT)
  const μ = 0.044
  const R = 8.31462
  return Number((this.properties.start.pressure * μ) / (R * this.properties.start.temperature))
}
```

### `viscosity()`

Returns the viscosity of the fluid at the start of the pipe segment.

```js
viscosity(): number {
  const μ0 = 0.000018 // Ref viscosity
  const T0 = 373 // Ref temperature
  const C = 240 // Southerland constant
  const T = this.properties.start.temperature
  return μ0 * ((T0 + C) / (T + C)) * (T / T0) ** (3 / 2)
}
```

### `endPressure()`

Returns the pressure at the end of the pipe segment.

```js
endPressure(): number {
  const w = this.properties.flowrate
  const D = this.properties.diameter
  const A = 0.25 * Math.PI * this.properties.diameter ** 2
  const ρ = this.density()
  const v = 1 / ρ
  const L = this.properties.length
  const P1 = this.properties.start.pressure

  // Friction factor
  const u = w / (A * ρ)
  const μ = this.viscosity()
  const Re = (ρ * u * D) / μ
  const f = Re < 2000 ? 64 / Re : 0.094 / (D * 1000) ** (1 / 3)

  return (A * Math.sqrt(D)) ** -1 * Math.sqrt(P1) * Math.sqrt(A ** 2 * D * P1 - f * L * v * w ** 2)
}
```

### `pressureDrop()`

Returns the difference in pressure from the start to the end.

### get `pressure`

Returns the result of `this.endPressure()`

### get `temperature`

:::info

This will eventually have some decrease towards ambient temperature.

:::

Returns the temperature at the start of the pipe segment.

### get `flowrate`

Returns the result of `this.inflow()`

### `pressureContinuity()`

Returns a boolean. True if the destination node has a pressure value equal to `this.pressure`

### `addSource()`

Adds a child node to `this.sources`. Re-evaluates the start pressure of this node.

```js
addSource(node: TreeNode) {
  super.addSource(node)
  this.properties.start.pressure = this.calcPressure()
}
```
