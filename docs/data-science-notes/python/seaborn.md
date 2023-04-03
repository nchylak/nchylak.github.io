---
layout: default
title: Seaborn
parent: Python
grand_parent: Data science notes
permalink: /docs/data-science-notes/python/seaborn/
nav_order: 13
---

# Seaborn

[Seaborn tutorials](https://seaborn.pydata.org/tutorial)

## Import

```python
import seaborn as sns
```

## Plot types

### Distribution plot

```python
sns.distplot(x, hist=True, kde=True, rug=True, bins=20) # visualise the distribution
# hist = histogram
# kde = density
# rug = ind. obs.
# bins = nb of bins in histogram
```

### Kernel density plot

```python
sns.kdeplot(x, shade=True, bw=.2, label="curve 1") # same as kde=True above but easier access to options when only interested in density 
# shade = coloured area under curve
# bw = how tight the fit should be
# label = label of curve (visible if legend is added)
```

### Pairwise relationships

```python
sns.pairplot(df)
```

### Custom facet grids

```python
g = sns.FacetGrid(df, col="col_to_group_by",
                  row="row_to_group_by",
                  hue="col_to_colour_differently") # specify groupings
g.map(plot_type, "col_name_x", "col_name_y", kwargs) # plot
```

## Aesthetics

### x-axis and y-axis

```python
sns.despine(left=True, bottom=True) # remove right and top spines
```

### Grid

```python
sns.set_style("white")
```

### Legend

```python
plt.legend() # use of matplotlib
g.add_legend()
```

