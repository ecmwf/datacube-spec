![Static Badge](https://img.shields.io/badge/Maturity-Incubating-violet)
![Static Badge](https://img.shields.io/badge/ESEE-Foundation-orange)

This repository serves as the specification for the conceptual structure of datacubes, as well as the syntax specification for interactions with datacubes. It also provides tools for parsing and translating this syntax, to encourage consistent behaviour across software and services.

> A datacube is a sparse, multi-dimensional array, consisting of individual elements each with a unique coordinate. The dimensions of the datacube have an implicit ordering. Navigating through a datacube by successively selecting coordinates on each axis reveals a tree-like structure, whereby each branch of the tree reveals a smaller sub-datacube, and the elements are the leaves of the tree.


# Introduction

The syntax contains three core concepts:

* An **identifier** is the description of a location in the datacube. This may be the coordinates of a specific element, or a sub-datacube.
* A **selection** is a span of identifiers for describing a set of elements or sub-datacubes, defined as ranges or sets on each dimension.
* A **request** is a selection, plus a **verb** specifying an action to be taken on those elements

These concepts can be expressed in any of the following four principal formats:
* JSON
* YAML
* MARS
* URL query string

Due to varying requirements across different service interfaces and protocols, the datacube specification must be represented in multiple formats. This repository provides the specification, and includes tools for consistent validation and translation of the syntax in different programming languages.

TODO: The specification is open to contributors who wish to add other formats or tools for other languages. Domain-specific extensions are also possible.

## Datacube Properties

There are many intepretations of a datacube. For the purposes of this specification we define the following characteristics as intrinsic to datacubes. It is helpful to describe these properties by example, so consider this **identifier** in a datacube designed for meteorological data:

```json
{
      "class": "operational",
      "stream": "forecast",
      "date": "2023-01-10",
      "step": "5",
      "param": "temperature",
}
```

> [!NOTE]
> This specification addresses the syntax and does not prescribe specific vocabulary for axis names (e.g., "class") or their values (e.g., "operational"). The selection of appropriate vocabulary is domain-specific and falls under the responsibility of the user or their data governance protocols.

### Hierarchical Structure

  A datacube can be conceptualized as a tree of lower-dimension datacubes.

  In our example, selecting from the original 5-dimensional datacube with the identifier `date: 2023-01-10` will yield a 4-dimensional datacube. If there are multiple dates in the datacube, there are many branches of the tree at this level.

  Similarly, one can combine multiple datacubes by providing a new axis which distinguishes between them, thus creating a higher-dimension datacube and another level in the tree. For example, if the experiment that created our example datacube was performed at multiple resolutions, we can combine all of the output into a single datacube, by adding a new axis for `resolution`.

  New branches of the tree can be added (for example, if we wanted to add `parameter: pressure`) by simply increasing the length of a dimension.

### Irregularity and Sparsity
  
  Datacubes may exhibit variability in the length of their axes. For instance, a sub-datacube filtered by `date: 2023-01-10` might have a step axis extending to `240`, whereas one filtered by `date: 2023-01-11` might extend to `360`. Additionally, these axes can be sparse, meaning they do not necessarily include every sequential index (e.g., `0, 2, 4, 6, ... 240`).

### Branching Dimensionality

  The dimensionality of sub-datacubes can vary. Different sub-datacubes may exhibit different numbers of dimensions depending on their data context. For example, a sub-datacube under the `stream: forecast` category might include a `step` axis, while one under `stream: analysis` might not.

  This means branches of the tree can be of different lengths, and it also means that we do not assume a datacube has a homogeneous dimensionality.

> [!NOTE]
> Some intepretations of a datacube would not allow this final property, which means that a combination of heterogeneous-dimension datacubes into a larger datacube is not possible. These intepretations then rely on a two-tier syntax to navigate the data, where part of the navigation is tree-like and part of the navigation is datacube-like.

## Axis Properties

Within a datacube, dimensions have axes which are categorized into several types, each handled distinctly within the datacube specification:

### Measurable Axes

  Mathematically this means a dimension where you can measure a physical distance between any two points on an axis. Within a datacube, this is an axis where it makes physical sense to express a range of values. This usually implies some kind of spatio-temporal axis. It is possible to query for a range of coordinates across a measurable axis.

### Countable Axes

  An axis which is not physically measurable, but can be systematically ordered and numbered using natural numbers. For example, in an ensemble weather forecast, N simultaneous forecasts are executed and stored in a datacube and indexed on a countable axis (`number: 1, 2, ... 50`). Countable axes behave similarly to measurable axes and range queries are possible across a countable axis.

### Unordered Axes

  An axis which cannot be ordered in a scientifically-meaningful way, such that it is not possible to express a range of values. In the above example, `class`, `stream` and `parameter` are all unordered axes. These axes are primarily for categorical distinctions within the data.

See the full [specification](./spec/readme.md).


