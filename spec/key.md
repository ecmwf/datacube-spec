# Key

A key is fundamentally a dictionary describing the coordinates of a datacube which point to an individual object or sub-datacube. Each entry in the dictionary may only have a single value.

## Examples

**MARS Native**
```
class=operations,stream=forecast,date=2024-01-01,parameter=t,step=1
```

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

**URL Query**
```
class=operations&stream=forecast&date=2024-01-01&parameter=t&step=1
```