---
layout: default
title: Matplotlib
parent: Python
grand_parent: Data science notes
permalink: /docs/data-science-notes/python/matplotlib/
nav_order: 12
---

# Matplotlib

## Import

```python
import matplotlib.pyplot as plt
%matplotlib inline
```

## Figure and axes

```python
fig, ax = plt.subplots() # shortcut, good practice for starting (even if only one plot - enables to extend it to multiple plots afterwards)

fig = plt.figure(figsize=(12, 5))
ax1 = fig.add_subplot(2, 1, 1) # 2 rows, 1 column, 1st ax
ax2 = fig.add_subplot(2, 1, 2) # 2 rows, 1 column, 2nd ax
# OR
fig, axs = plt.subplots(2,1) # shortcut, good practice for starting
ax1 = axs[0] # select the ax when there are multiples axes
ax2 = axs[1]

fig = plt.gcf() # select the fig of the current plot and assign it to variable fig
ax = plt.gca() # select the ax of the current plot and assign it to variable ax
```

## Generating and displaying the plot

```python
plt.show()
```

## Saving the plot

```python
plt.savefig('biology_degrees.png')
```

## Plot types

### Histogram

```python
plt.hist(x)
Axes.hist(x)
plt.hist(x,range=(0,5)) #specify lower and upper range of hist
plt.hist(x, bins=20) # specify number of bins
```

### Boxplot

```python
plot.boxplot(x)
Axes.boxplot(x)
Axes.boxplot(df[cols].values) # boxplot for multiple columns
```

### Line chart

```python
plt.plot(x,y)
Axes.plot(x,y)
```

### Scatter plot

```python
plt.scatter(x,y)
Axes.scatter(x,y)
```

### Bar plot

```python
bar_positions = arange(5) + 0.75
plt.bar(bar_positions, bar_heights, width)
Axes.bar(bar_positions, bar_heights, width)
```

## Aesthetics

### Title and labels

```python
plt.title('Title')
Axes.set_title('Unemployment Trend, 1948') # Subplot titles
plt.xlabel('Month')
plt.ylabel('Unemployment Rate')
```

### Legend

```python
plt.legend(loc="upper left")
```

### x-axis and y-axis

```python
Axes.spines["right"].set_visible(False) # render axis invisible
Axes.set_ylim(0,50) # force axis start and end
```

### x-ticks and y-ticks

```python
Axes.tick_params(bottom="off", left="off", top="off", right="off") # turn off ticks
```

##### Ticks location

```python
Axes.set_ticks([1, 2, 3, 4, 5])
```

##### Ticks labels

```python
plt.xticks(rotation=90)
Axes.set_xticklabels(['blah1','blah2','blah3','blah4','blah5'], rotation=90)
```

### Lines

```python
plt.plot(x,y,c='red') # line colour
plt.plot(x,y,linewidth=3) # line width
```

### Annotation

```python
Axes.text(x,y,'annotation') # places 'annotation' at coordinates (x,y)
```

### Style

```python
plt.style.available # list available styles
plt.style.use(['style']) # change global style
with plt.style.context(('style')): # apply style to only certain plots
    plt.plot(x,y)
```

