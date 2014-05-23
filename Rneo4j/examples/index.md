---
layout: rneo4j
---

# Example

Load `Rneo4j` and establish a connection to the currently-running Neo4j server.

```r
library(Rneo4j)

graph = startGraph("http://localhost:7474/db/data/")

graph$version
# [1] "2.0.3"
```

Clear the database. This deletes all nodes, relationships, indexes, and constraints from the graph. You will have to answer a Y/N prompt in order to do so.

```r
clear(graph)
```

Create nodes with labels and properties. I forget to assign Cheer Up Charlie's to a variable, but I take care of that later.

```r
mugshots = createNode(graph, "Bar", name = "Mugshots", location = "Downtown")
parlor = createNode(graph, "Bar", name = "The Parlor", location = "Hyde Park")
createNode(graph, "Bar", name = "Cheer Up Charlie's", location = "Downtown")
```

Labels can be added after creating the node.

```r
nicole = createNode(graph, name = "Nicole", status = "Student")
addLabel(nicole, "Person")
```

View node properties with `node$property`.

```r
mugshots$location

# [1] "Downtown"
```

Add uniqueness constraints so that `Person` nodes are unique by `name` and `Bar` nodes are unique by `name`.

```r
addConstraint(graph, "Person", "name")
addConstraint(graph, "Bar", "name")
```

View all constraints in the graph.

```r
getConstraint(graph)

# 	property_keys  label       type
# 1          name Person UNIQUENESS
# 2          name    Bar UNIQUENESS
```

Find Cheer Up Charlie's and assign it to `charlies`:

```r
charlies = getNodeByIndex(graph, "Bar", name = "Cheer Up Charlie's")
```

Create relationships.

```r
createRel(nicole, "DRINKS_AT", mugshots, on = "Fridays")
createRel(nicole, "DRINKS_AT", parlor, on = "Saturdays")
rel = createRel(nicole, "DRINKS_AT", charlies, on = "Everyday")
```

View relationship properties with `relationship$property`.

```r
rel$on

# [1] "Everyday"
```

Get the start and end nodes of a relationship object.

```r
start = getStart(rel)
end = getEnd(rel)

start$name

# [1] "Nicole"

end$name

# [1] "Cheer Up Charlie's"
```

Get Cypher query results as a data frame.

```r
query  = "MATCH (p:Person {name:'Nicole'})-[d:DRINKS_AT]->(b:Bar)
		  RETURN p.name, d.on, b.name, b.location"

cypher(graph, query)

# 	p.name      d.on             b.name b.location
# 1 Nicole   Fridays           Mugshots   Downtown
# 2 Nicole Saturdays         The Parlor  Hyde Park
# 3 Nicole  Everyday Cheer Up Charlie's   Downtown
```

Add `eyes` and `hair` properties to the `nicole` node, convert the `status` property to a label, then remove the `status` property.

```r
nicole = updateProp(nicole, eyes = "green", hair = "blonde")

addLabel(nicole, nicole$status)

nicole = deleteProp(nicole, "status")

nicole

# Labels: Person Student
# 
# $name
# [1] "Nicole"
# 
# $hair
# [1] "blonde"
# 
# $eyes
# [1] "green"
```

## Neo4j Browser View

![Neo4j Browser](http://i.imgur.com/P49bwa4.png)