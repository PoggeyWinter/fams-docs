---
sidebar_position: 5
---

# Valve

Valves extend the [Point](/docs/model/Point) class. Their purpose is to calculate and return a new temperature value based on the specified output pressure.

The constructor takes one argument, `outPressure`

```js
interface IValve {
  outPressure: number;
}

export default class Valve extends Point {
  outPressure: number;

  constructor(props: IValve) {
    super();
    this.outPressure = props.outPressure;
  }

  get temperature(): number {
    return this.inPressure / this.outPressure + 5;
  }

  get pressure(): number {
    return this.outPressure;
  }

  get inPressure(): number {
    return super.calcPressure();
  }
}
```

:::info

The temperature calculation above is a placeholder because we haven't properly specified how to verify the actual calculation.

:::

## get `temperature`

Performs enthalpy calculations using the `input pressure`, `input temperature` and `output pressure` to find the new temperature value.

Returns the calculated temperature.

## get `pressure`

Returns the output pressure.

## get `inPressure`

Returns the lowest pressure value of any preceding node.
