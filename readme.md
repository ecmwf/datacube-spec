
This repository holds the specification for the MARS language syntax as well as multi-language tools for parsing and translating MARS keys, queries and requests. More broadly, it is a specification for a language which describes indexing of multi-dimensional datacubes.

# Introduction

The MARS language contains three core objects:

* A **key** is the index of an object or sub-datacube in a datacube
* A **query** is a span of keys for describing a set of objects
* A **request** is a query or key, plus a **verb** specifying an action to be taken on those objects

Any of these can be represented in four principle formats:
* The MARS native format
* JSON
* YAML
* URL query string

Different service interfaces require the MARS language to be represented in certains ways. This repository aims to provide a definitive specification of the format syntax, and tools to perform validation and translation consistently.

# Datacubes

A datacube is a multidimensional dataset, where individual objects are described by specifying a dictionary of metadata describing their location on different axes. For example, a key describing a specific object in a meteorological datacube specifies a location on each axis:

```json
{
      "class": "operational",
      "stream": "forecast",
      "date": "2023-01-10",
      "step": "5",
      "param": "temperature",
}
```
> [!TIP]
> This specification does not dictate the names of axes (e.g. "class") or their indexes (e.g. "operational"), it just describes the syntax.

We consider a datacube to have the following properties:

* A datacube can be represented as a tree of sub-datacubes

* Datacubes can be irregular or sparse:
  
  They do not have the same **length** in all directions. For example, the sub-datacube selected by `date: 2023-01-10` may have a `step` axis of length 240 whereas the sub-datacube selected by `date: 2023-01-11` may have a `step` axis of length 360. In both cases, the `step` axis can also be sparse, such that it does not contain every index value `0, 2, 4, 6, ... 240`.

* Datacubes can branch:

  This means that the datacube does not have the same **dimensionality** in all directions. Different sub-datacubes can have a different number of dimensions. For example, the sub-datacube selected by `stream: forecast` has a `step` axis, but the sub-datacube selected by `stream: analysis` may not have that axis at all.


There are several types of axis within a datacube, which are handled differently within the MARS language specification:

* Measurable Axes

  An axis where it makes physical sense to express a range of values. This usually implies some kind of spatio-temporal axis. It is possible to query for a range of indexes across a measurable axis.

* Countable Axes

  An axis which is not physically measurable, but can be ordered and numbered using natural numbers. For example, in an ensemble weather forecast, N simultaneous forecasts are executed and stored in a datacube and indexed on a countable axis (`number: 1, 2, ... 50`). Countable axes behave similarly to measurable axes and range queries are possible across a countable axis.

* Unordered Axes

  An axis which cannot be ordered in a scientifically-meaningful way, such that it is not possible to express a range of values. In the above example, `class`, `stream` and `parameter` are all unordered axes.

A **request** may include other key-value pairs in its associated key or query, which are non-indexing axes. See the full specification.
  
