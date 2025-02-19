---
title: toString() function
description: The `toString()` function converts all values in the `_value` column to strings.
aliases:
  - /v2.0/reference/flux/functions/transformations/type-conversions/tostring
  - /v2.0/reference/flux/functions/built-in/transformations/type-conversions/tostring/
menu:
  v2_0_ref:
    name: toString
    parent: built-in-type-conversions
weight: 501
---

The `toString()` function converts all values in the `_value` column to strings.

_**Function type:** Type conversion_  
_**Output data type:** String_

```js
toString()
```

{{% note %}}
To convert values in a column other than `_value`, define a custom function
patterned after the [function definition](#function-definition),
but replace `_value` with your desired column.
{{% /note %}}

## Examples
```js
from(bucket: "telegraf")
  |> filter(fn:(r) =>
    r._measurement == "mem" and
    r._field == "used"
  )
  |> toString()
```

## Function definition
```js
toString = (tables=<-) =>
  tables
    |> map(fn:(r) => ({ r with _value: string(v: r._value) }))
```

_**Used functions:**
[map()](/v2.0/reference/flux/stdlib/built-in/transformations/map),
[string()](/v2.0/reference/flux/stdlib/built-in/transformations/type-conversions/string)_
