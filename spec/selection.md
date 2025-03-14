# selection

A selection is a dictionary-like object describing the ranges or sets coordinates, which point to multiple elements or sub-datacubes within a datacube.

For each key-value pair, the key may be any string and the values must be a list of values or ranges.

## Examples

**JSON**
```JSON
{
    "class": "operations",
    "stream": ["forecast"],
    "date": ["2024-01-01", "2024-01-03"],
    "parameter": ["t", "p", "q"],
    "step": {
        "start": 1,
        "end": 30,
        "step": 2
    }
}
```
* Ranges are specified as a dictionary specifying the start, end and (optionally) step.
* Lists of values are specified as a JSON list.
* Individual values can be specified just as in a [identifier](identifier.md), or as a single-value list.
* It is also valid to use MARS-like syntax for ranges and lists:
    * `"date": "20240101/to/20240103"`
* A selection is indistinguishable from an identifier if all the values are simple individual values.

**YAML**
```YAML
class: operations
stream: 
  - forecast
date:
  - 2024-01-01
  - 2024-01-03
parameter: [t, p, q]
step:
  start: 1
  end: 30
  step: 2
```

* All valid YAML formulations of the JSON specification are permitted.

**MARS**
```
class=operations,stream=forecast,date=20240101/to/20240103,parameter=t/p/q,step=1/to/30/by/2
```
* Ranges are specified as `start/to/end`, or optionally with a step as `start/to/end/by/step`.
* Lists of values are specified as `item1/item2/item3`
* Individual values can be specified just as in a [identifier](identifier.md)
* A selection is indistinguishable from a identifier if all the values are simple individual values.

**URL Query**
```
class=operations&stream=forecast&date=(2024-01-01,2024-01-03)&parameter=(t,p,q)&step=(from=1,to=30,step=2)
```