
# Datacube Specification

The specification contains three core concepts:

* [Identifier](identifier.md)
* [selection](selection.md)
* [Request](request.md)

The concepts are defined above, and their format-specific syntax is defined as follows:

* [JSON](formats/json.md)
* [YAML](formats/yaml.md)
* [MARS](formats/mars.md)
* [URL Query](formats/url-query.md)


## TODO

- [ ] Queries across branches with different dimensionality
- [ ] Validity of over-specified requests
- [ ] Reserved keywords
- [ ] Define the allowable types (value, list, set) and how they are represented in each format
- [ ] Can we represent dictionaries in MARS format?