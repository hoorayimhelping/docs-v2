---
title: strings.trimLeft() function
description: >
  The strings.trimLeft() function removes leading characters specified in the cutset from a string.
menu:
  v2_0_ref:
    name: strings.trimLeft
    parent: Strings
weight: 301
related:
  - /v2.0/reference/flux/functions/strings/trim
  - /v2.0/reference/flux/functions/strings/trimright
  - /v2.0/reference/flux/functions/strings/trimprefix
  - /v2.0/reference/flux/functions/strings/trimsuffix
  - /v2.0/reference/flux/functions/strings/trimspace
---

The `strings.trimLeft()` function removes leading characters specified in the
[`cutset`](#cutset) from a string.

_**Output data type:** String_

```js
import "strings"

strings.trimLeft(v: ".abc.", cutset: ".")

// returns "abc."
```

## Parameters

### v
String to remove characters from.

_**Data type:** String_

### cutset
The leading characters to remove from the string.
Only characters that match the `cutset` string exactly are removed.

_**Data type:** String_

## Examples

###### Trim leading periods from all values in a column
```js
import "strings"

data
  |> map(fn: (r) => ({
      r with
      variables: strings.trimLeft(v: r.variables, cutset: ".")
    })
  )
```