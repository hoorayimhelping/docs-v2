---
title: int() function
description: The `int()` function converts a single value to an integer.
aliases:
  - /v2.0/reference/flux/functions/built-in/transformations/type-conversions/int/
menu:
  v2_0_ref:
    name: int
    parent: built-in-type-conversions
weight: 502
---

The `int()` function converts a single value to an integer.

_**Function type:** Type conversion_  
_**Output data type:** Integer_

```js
int(v: "4")
```

## Parameters

### v
The value to convert.

## Examples
```js
from(bucket: "sensor-data")
  |> range(start: -1m)
  |> filter(fn:(r) => r._measurement == "camera" )
  |> map(fn:(r) => ({ r with exposures: int(v: r.exposures) }))
```
