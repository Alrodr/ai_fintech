---
title: "Making API request in Python"
description: "How to make an API request in Python"
lead: "How to make an API request in Python"
date: 2022-06-06
lastmod: 2022-06-06
draft: false
images: []
menu:
  docs:
    parent: "Section 3: Data Acquisition Methods"
weight: 342
toc: true
---

Now that you have a pretty good idea of how the Census Data API works, let’s take a look at how to pull the data in Python. Begin by importing the requests library with this command:

```python
import requests
```

Next, we can use the `get()` method to return the data from our desired URL:

```python
r = requests.get('https://api.census.gov/data/2020/acs/acs5?get=NAME,B08303_001E&for=state:*')
```

The result is a response object, but this time we stored it in a variable named `r`. We can look at that response data by using the `.text` attribute. The text attribute turns the data into a string.

We can also use the `.json()` method that can automatically decode JSON data into the appropriate Python object. This is useful when working with JSON data, as in the case of the Census API, to have the data in a more intuitive data structure.

```python
# Access data as JSON string
print(r.text)
 
# Access decoded JSON data as Python object
print(r.json())
```