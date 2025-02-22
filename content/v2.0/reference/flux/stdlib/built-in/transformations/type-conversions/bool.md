---
title: bool() function
description: The `bool()` function converts a single value to a boolean.
aliases:
  - /v2.0/reference/flux/functions/built-in/transformations/type-conversions/bool/
menu:
  v2_0_ref:
    name: bool
    parent: built-in-type-conversions
weight: 502
---

The `bool()` function converts a single value to a boolean.

_**Function type:** Type conversion_  
_**Output data type:** Boolean_

```js
bool(v: "true")
```

## Parameters

### v
The value to convert.

## Examples
```js
from(bucket: "sensor-data")
  |> range(start: -1m)
  |> filter(fn:(r) => r._measurement == "system" )
  |> map(fn:(r) => ({ r with responsive: bool(v: r.responsive) }))
```
