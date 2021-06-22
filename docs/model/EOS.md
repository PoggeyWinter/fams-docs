---
sidebar_position: 6
---

# Equation of State

The equation of state entity is used for thermodynamic calculations.  
It stores corresponding pressure/temperature/enthalpy values in memory and has a range of methods to read and select data.

:::info

Data must be loaded asynchronously

```js
const eos = await new EOS().load();
```

:::

## Properties

| Property                | Unit | Default     | Notes                                                |
| ----------------------- | ---- | ----------- | ---------------------------------------------------- |
| `data`                  | -    | valve       |                                                      |
| `unique.pressure`       | -    | `new Set()` |                                                      |
| `dataGroupedByPressure` | -    | `{}`        | Keys: unique pressure values, Values: `{PT, TM, HG}` |

:::note

The `unique` object can be extended to record unique values of other variables. I don't know if that's necessary but that's why it's an object in the first place.

:::

## Methods

### `load()`

Reads the CSV input file. Populates `data`, `unique.pressure`, and `dataGroupedByPressure`.

### _async_ `selectPressure(pressure)`

Calls `this.load` if `this.data` is empty.

Returns the closest pressure value in the dataset.

### _async_ `selectRow({PT, TM, HG})`

Calls `this.load` if `this.data` is empty.

Returns the row with the specified values.

PT is a required argument. To look up an enthalpy value, specify PT and TM. To look up a temperature value, specify PT and HG.

### _async_ `getOutTemp(p_in, t_in, p_out)`

Returns the output temperature.

Looks up enthalpy using the specified `p_in` and `t_in`. Finds the corresponding output temperature using `p_out` and the enthalpy value it finds.

### _get_ `uniquePressures`

Returns an array of the values in `this.unique.pressure`. Used to search for the pressure value closest to a given input.
