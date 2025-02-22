---
title: uint() function
description: The `uint()` function converts a single value to a UInteger.
aliases:
  - /v2.0/reference/flux/functions/built-in/transformations/type-conversions/uint/
menu:
  v2_0_ref:
    name: uint
    parent: built-in-type-conversions
weight: 502
---

The `uint()` function converts a single value to a UInteger.

_**Function type:** Type conversion_  
_**Output data type:** UInteger_

```js
uint(v: "4")
```

## Parameters

### v
The value to convert.

## Examples
```js
from(bucket: "sensor-data")
  |> range(start: -1m)
  |> filter(fn:(r) => r._measurement == "camera" )
  |> map(fn:(r) => ({ r with exposures: uint(v: r.exposures) }))
```
