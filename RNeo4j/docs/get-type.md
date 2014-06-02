---
title: getType
layout: rneo4j
---

`getType`

# Relationship Types

## Description

Get the type of a relationship object or all relationship types in the graph.

## Usage

```r
getType(object)
```

## Arguments

| Parameter | Description     |
| --------- | --------------- |
| `object`  | The object for which to view the relationship type(s). Accepts a relationship or graph object (see details). |

## Output

A string or character vector.

## Details

Supplying a relationship object returns the type of the relationship as a string. Supplying a graph object returns all relationship types in the graph as a character vector. 

## Examples

```r
alice = createNode(graph, "Person", name = "Alice")
bob = createNode(graph, "Person", name = "Bob")
charles = createNode(graph, "Person", name = "Charles")

createRel(bob, "WORKS_WITH", charles)
rel = createRel(alice, "KNOWS", bob)

getType(rel)

# [1] "KNOWS"

getType(graph)

# [1] "KNOWS" "WORKS_WITH"
```


