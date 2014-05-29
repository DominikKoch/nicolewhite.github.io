---
title: getIndex
layout: rneo4j
---

`getIndex`

# Indexes

## Description

Get all indexes for a given label or for the entire graph database.

## Usage

```r
getIndex(graph, label = character())
```

## Arguments

| Parameter | Description     |
| --------- | --------------- |
| `graph`   | A graph object. |
| `label`   | A node label for which to view all indexes. Accepts a string. |

## Output

A data frame. Returns NULL if no indexes are found.

## Details

Supplying only a graph object as an argument returns all indexes in the graph database.

## Examples

```r
createNode(graph, "Person", name = "Nicole", status = "Employed")
createNode(graph, "Person", name = "Drew", status = "Employed")
createNode(graph, "Person", name = "Aaron", status = "Unemployed")

createNode(graph, "School", name = "University of Texas at Austin", type = "Public")
createNode(graph, "School", name = "Louisiana State University", type = "Public")

addIndex(graph, "Person", "status")
addIndex(graph, "School", "type")
```

Get all indexes on the `Person` node label.

```r
getIndex(graph, "Person")

#   property_keys  label
# 1        status Person
```

Get all indexes in the graph database.

```r
getIndex(graph)

#   property_keys  label
# 1        status Person
# 2          type School
```

## See Also

[`addIndex`](add-index.html), [`dropIndex`](drop-index.html)