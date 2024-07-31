# Request

A request is a combination of a **verb** describing an action and a query describing the subject of that action. A request may include other parameters beyond the datacube dimensions, which control the behaviour of the action.

## Examples

**MARS Native**
```
retrieve,class=operations,stream=forecast,date=20240101/to/20240103,parameter=t/p/q,step=1/to/30/by/2,grid=0.1/0.1,feature=
```
* The verb is included before the query.
* The retrieve action uses the additional parameter "grid" which triggers post-processing of the result.

**JSON**
```JSON
{
    "verb": "retrieve",
    "class": "operations",
    "stream": ["forecast"],
    "date": ["2024-01-01", "2024-01-03"],
    "parameter": ["t", "p", "q"],
    "step": {
        "start": 1,
        "end": 30,
        "step": 2
    },
    "grid" : [ 0.1, 0.1 ]
}
```
* Ranges are specified as a dictionary specifying the start, end and (optionally) step.
* Lists of values are specified as a JSON list.
* Individual values can be specified just as in a [key](key.md), or as a single-value list.
* It is also valid to use MARS-like syntax for ranges and lists:
    * `"date": "20240101/to/20240103"`


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

**URL Query**
```
class=operations&stream=forecast&date=(2024-01-01,2024-01-03)&parameter=(t,p,q)&step=(from=1,to=30,step=2)
```