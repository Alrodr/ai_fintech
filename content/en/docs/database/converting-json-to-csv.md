---
title: "Converting JSON to CSV"
description: "How to convert a JSON to CSV"
lead: "How to convert a JSON to CSV"
date: 2022-06-07
lastmod: 2022-06-07
draft: false
images: []
menu:
  docs:
    parent: "Section 3: Data Acquisition Methods"
weight: 344
toc: true
---
{{< alert icon="♻️" context="info" text="<b>TL;DR</b> While JSON is a great universal format for data interchange, it might not be the ideal format in other aspects, such as readability. Instead, having the data in a tabular format (like a CSV) can make it much more human-readable and accessible. Therefore, being able to convert between file types is essential." />}}

There are several libraries in Python to work with different data formats. For example, to convert the Census data from JSON to CSV, we can use the built-in [csv](https://docs.python.org/3/library/csv.html) library:

```python
import csv
```

As a refresher, visit `https://api.census.gov/data/2020/acs/acs5?get=NAME,B08303_001E&for=state:*` in your browser to view the total commuters count for all states.

The JSON data we got from the Census API is a list of lists in Python, where each inner list corresponds to a single row of data. To convert from JSON to CSV, we want to write each sublist as a comma-separated row of data to file. This bit of code is a little complicated. We will use the `writerows()` method from the `csv` library:

```python
with open('census.csv', mode='w', newline='') as file:
  writer = csv.writer(file)
  writer.writerows(r.json())
```

We first make a variable and call it file. Then we use `open()` to open a file, since we are going to write that file, we open it with `mode='w'` for writing mode. The `newline=''` ensures that newlines are always interpreted correctly.

Next, we use the `writer()` function from the csv library to make a writer object (don’t worry about what this is right now). We then use the `writerows()`method to write each row of data into comma-separated format.