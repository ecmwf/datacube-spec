# Request

A request is a combination of a **verb** describing an action and an [identifier](identifier.md) or [query](query.md) describing the subject of that action. A request may include other parameters beyond the datacube dimensions, which control the behaviour of the action.

These verb and its parameters are not dictated by the Datacube Specification, but their syntax and encoding to different formats is.

## Examples

In the examples below, the verb is `retrieve` for a data retrieval, and the user can provide the `target` location for the data and the `grid` option for post-processing (interpolation).

**JSON**
```JSON
{
    "verb" : "retrieve",
    "class": "operations",
    "stream": ["forecast"],
    "date": ["2024-01-01", "2024-01-03"],
    "parameter": ["t", "p", "q"],
    "step": {
        "start": 1,
        "end": 30,
        "step": 2
    },
    "target": "foo.data",
    "grid": [0.1, 0.1]
}
```
* The verb must be a string and is added to the query or identifier.
* Additional parameters may be a string, dictionary or list, with arbitrary nesting.

**YAML**
```YAML
verb: retrieve
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
target: foo.data
grid: [0.1, 0.1]
```

* The verb must be a string and is added to the query or identifier.
* Additional parameters may be a string, dictionary or list, with arbitrary nesting.

**MARS**
```
retrieve,class=operations,stream=forecast,date=20240101/to/20240103,parameter=t/p/q,step=1/to/30/by/2,target=foo.data,grid=0.1/0.1
```
* The verb must be a string and prepends the query or identifier
* Additional parameters may be a string or slash-separated list.
* **Requests which require dictionary parameters are not supported in MARS format.**

**URL Query**
```
verb=retrieve&class=operations&stream=forecast&date=(2024-01-01,2024-01-03)&parameter=(t,p,q)&step=(from=1,to=30,step=2)&target=foo.data&grid=(0.1,0.1)
```
* The verb must be a string and is added to the query or identifier.
* Additional parameters may be a string, dictionary or list, with arbitrary nesting.