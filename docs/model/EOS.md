---
sidebar_position: 6
---

# Equation of State

:::note

This code is not yet part of the `ccs-sim` package.

:::

:::danger poorly implemented (maybe?)

This needs work. Maybe flowcharts would help with planning what it should do. It does **_something_** but who knows if it does what it's supposed to. The units of the values it looks up are wrong at the very least.

:::

The equation of state entity is used for calculations related to enthalpy.  
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
