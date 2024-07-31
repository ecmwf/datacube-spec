
This repository serves as the definitive source for the MARS language syntax specification, along with tools for parsing and translating MARS keys, queries, and requests. The MARS language is broadly a language designed for indexing multi-dimensional datacubes.

# Introduction

The MARS language contains three core objects:

* A **key** is the index of an object or sub-datacube in a datacube
* A **query** is a span of keys for describing a set of objects
* A **request** is a query or key, plus a **verb** specifying an action to be taken on those objects

These objects can be expressed in any of the following four principal formats:
* MARS native format
* JSON
* YAML
* URL query string

Due to varying requirements across different service interfaces and protocols, the MARS language must be represented in multiple formats. This repository provides a comprehensive specification of the MARS syntax in different formats, and includes tools for consistent validation and translation of the language.

# Datacubes

A datacube is a multidimensional dataset, characterized by individual objects each defined by a set of metadata. This metadata acts as coordinates, specifying the object's position across various axes of the datacube. For instance, within a meteorological datacube, a key might define an object's location using the following axes:

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
> This specification addresses the syntax and does not prescribe specific vocabulary for axis names (e.g., "class") or their values (e.g., "operational"). The selection of appropriate vocabulary is domain-specific and falls under the responsibility of the user or their data governance protocols.

## Datacube Properties

There are many intepretations of a datacube. For the purposes of this specification we recognize the following characteristics as intrinsic to datacubes:

* **Hierarchical Structure:**

  A datacube can be conceptualized as a tree of sub-datacubes.

* **Irregularity and Sparsity:**
  
  Datacubes may exhibit variability in the length of their axes. For instance, a sub-datacube filtered by `date: 2023-01-10` might have a step axis extending to `240`, whereas one filtered by `date: 2023-01-11` might extend to `360`. Additionally, these axes can be sparse, meaning they do not necessarily include every sequential index (e.g., `0, 2, 4, 6, ... 240`).

* **Branching**

  The dimensionality of sub-datacubes can vary. Different sub-datacubes may exhibit different numbers of dimensions depending on their data context. For example, a sub-datacube under the `stream: forecast` category might include a step axis, while one under `stream: analysis` might not.

## Axis Properties

Within a datacube, axes are categorized into several types, each handled distinctively within the MARS language specification:

* **Measurable Axes**

  An axis where it makes physical sense to express a range of values. This usually implies some kind of spatio-temporal axis. It is possible to query for a range of indexes across a measurable axis.

* **Countable Axes**

  An axis which is not physically measurable, but can be systematically ordered and numbered using natural numbers. For example, in an ensemble weather forecast, N simultaneous forecasts are executed and stored in a datacube and indexed on a countable axis (`number: 1, 2, ... 50`). Countable axes behave similarly to measurable axes and range queries are possible across a countable axis.

* **Unordered Axes**

  An axis which cannot be ordered in a scientifically-meaningful way, such that it is not possible to express a range of values. In the above example, `class`, `stream` and `parameter` are all unordered axes. These axes are primarily for categorical distinctions within the data.

A **request** may include other key-value pairs in its associated key or query, which are non-indexing axes.

See the full [specification](./spec/readme.md).
  
