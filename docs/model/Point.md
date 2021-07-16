---
sidebar_position: 2
---

# Point

Point extends the [TreeNode](/docs/model/TreeNode) class, adding `pressure`, `flowrate`, and `temperature` properties - although these are not supposed to be set independently as Point objects aren't intended to do anything. Its properties are calculated based on the Point object's `source`s. They exist to mark points on the network and could possibly be renamed to become `Meter`s. (The plan was for a Meter to be a type of Point object.)

```js
export default class Point extends TreeNode {
  constructor(props?: any) {
    super(props);
  }

  calcPressure(): number {
    let min = Infinity;
    const selectLowerPressure = (n: Point) => {
      if (n !== this) min = Math.min(min, n.pressure);
    };
    postOrder(this, selectLowerPressure);

    return min;
  }

  get pressure(): number {
    return this.calcPressure();
  }

  get flowrate(): number {
    let sum = 0;
    const addFlow = (n: Point) => {
      if (n !== this) sum += n.flowrate;
    };
    postOrder(this, addFlow);
    return sum;
  }

  get temperature(): number {
    return 10;
  }
}
```

## get `pressure`

Returns the lowest pressure value of any preceding node.

## get `flowrate`

Returns the sum of the flowrates of the preceding nodes.

## get `temperature`

:::info

Temperature has not been properly implemented. This is a placeholder value.

:::

Returns 10.
