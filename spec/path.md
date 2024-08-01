# Path

A path is a dictionary describing the coordinates of a datacube which point to an individual object or sub-datacube. Each entry in the dictionary may only have a single value.

For each key-value pair, the key may be any string and the values may be any type, but will be parsed as string (❗TO REVIEW).

## Examples

**JSON**
```JSON
{
    "class": "operations",
    "stream": "forecast",
    "date": "2024-01-01",
    "parameter": "t",
    "step": 1
}
```

**YAML**
```YAML
class: operations,
stream: forecast,
date: 2024-01-01,
parameter: t
```

**MARS**
```
class=operations,stream=forecast,date=2024-01-01,parameter=t,step=1
```

**URL Query**
```
class=operations&stream=forecast&date=2024-01-01&parameter=t&step=1
```