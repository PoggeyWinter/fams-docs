---
sidebar_position: 5
---

# Valve

Similarly to the [Point](/docs/model/Point), the Valve object extends the [TreeNode](/docs/model/TreeNode) class. Its purpose is to calculate and return a new temperature value based on the specified output pressure.

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

## get `inPressure`

Returns the lowest pressure value of any preceding node.  
This exists because `this.pressure` will be the the output pressure.
