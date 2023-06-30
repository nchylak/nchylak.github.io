---
layout: default
title: OLAP cubes
parent: Data engineering
grand_parent: Data science notes
permalink: /docs/data-science-notes/data-engineering/olap-operations/
nav_order: 3
---

# OLAP cubes

## OLAP operations

"Cube" of N-dimensions where a metric is aggregated according to these dimensions. In 3D, that could be sales by month, by region and by product.

Type of OLAP operations:

* **Roll-up**: Aggregates or combines values and reduces number of rows or columns.
  * Example: Group by continent instead of by country
* **Drill-down**: Decomposes values and increases number of rows or columns.
  * Example: Group by city instead of by country
* **Slice**: Reducing N dimensions to N-1 dimensions by restricting one dimension to a single value.
  * Example: Where Dimension1 = Value1
* **Dice**: Same dimensions but computing a sub-cube by restricting, some of the values of the dimensions.
  * Example: Where Dimension1 in ('Value1', Value2) and Dimension2 in ('Value3', 'Value4') and Dimension3 in ('Value5', 'Value6')

## OLAP approaches

**MOLAP**: Pre-aggregate OLAP cubes and save them

**ROLAP**: Aggregate OLAP cubes on the fly

## OLAP operations in SQL

> **WARNING**: Before using grouping sets and cube, one should make sure none of the dimensions to group by have null values. Indeed, the query returns "None" for columns <u>NOT</u> grouped by so it can be confusing if there are "None" <u>groups</u> in the mix.

### Grouping sets

SQL operation enable to perform different levels of aggregation in ONE PASS (rather than several passes and several queries).

* Example: 
  * Group by nothing, 
  * group by Dimension1, 
  * group by Dimension 2, 
  * group by Dimension 3, 
  * Group by Dimension1 and Dimension2, 
  * group by Dimension2 and Dimension3, 
  * group by Dimension1 and Dimension3,
  * group by Dimension1, Dimension2 and Dimension3.

```sql
SELECT dim1, dim2, dim3, sum(fact1)
FROM my_cube
GROUP BY grouping sets (
	(),
	dim1,
	dim2,
  dim3,
	(dim1, dim2),
  (dim2, dim3),
  (dim1, dim3),
  (dim1, dim2, dim3)
)
```

`()` is for grouping by nothing

### Cubes

To perform all possible aggregations, one can also use `cube`. For querying the same as above:

```sql
SELECT dim1, dim2, dim3, sum(fact1)
FROM my_cube
GROUP BY cube(dim1, dim2, dim3)
```
