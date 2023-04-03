---
layout: default
title: Web scrapping
parent: Python
grand_parent: Data science notes
permalink: /docs/data-science-notes/python/web-scrapping/
nav_order: 15
---

# Web scrapping

## Introduction

**Web scrapping**: programmatically extract data from internet pages. Usually, one would use web scrapping because the data is not available otherwise (e.g. via an API, csv file, etc.).

**Is scrapping ethical?** Some websites do not want to be scrapped. A good practice is consulting the `robots.txt` file (e.g. www.imdb.com/robots.txt). For not overloading a website’s server, it is also good practice to space out requests.

```python
import time
time.sleep(0.01) # sleep 0.01 sec
```

## Best practices

* Space requests by at least 6 ms
* Scrap as much as you can as you never know what additional information you may need
* Use regular expressions only as last resort.

## BeautifulSoup

Package to extract data from HTML. BeautifulSoup does not download HTML, for this we need the `requests` module.

```python
import requests
response = requests.get("http.....com/blash", timeout=5)
html_string = response.content
```

You can also specify a number of times requests need to be repeated until the script stops trying.

```python
s = requests.Session()
a = requests.adapters.HTTPAdapter(max_retries=3)
s.mount('http://', a)
```

### Import

```python
from bs4 import BeautifulSoup
```

### Parsing and navigating HTML

```python
parsed_html = BeautifulSoup(html_string, "html.parser")
print(parsed_html.prettify()) # to see HTML incl. indentation

# Retrieve first occurence
# based on tag type
parsed_html.body.div # or
parsed_html.find("div")
# based on id
parsed_html.find(id="ID")
parsed_html.select("#ID")[0] # using CSS style

# Retrieve all occurences
# based on tag type
parsed_html.find_all("div")
parsed_html.select("div") # using CSS style
# based on class
parsed_html.find_all(class_="cls") # because class is a reserved word in Python
parsed_html.select(".cls") # using CSS style
# based on attributes
parsed_html.find_all(attrs={"data-example":"yes"})
parsed_html.select("[data-example]") # using CSS style
# based on several 
```

### Accessing data in elements

```
element.get_text() # to retrieve the text of the element (also iterable)
element.name # to retrieve tags names
element.attrs["attribute_name_optional"] # to retrieve a dictionary of attributes
element["attribute_name_optional"] # shortcut of above
```

### Navigating with BeautifulSoup

```python
element.contents[1].contents # you can stack them
element.next_sibling
element.parent

# methods
element.find_parent()
element.find_next_sibling()
element.find_previous_sibling()
```

## Alternative libraries

* Selenium (pages incl. javascript elements)
* Scrapy