---
title: "Making API request from browser"
description: "How to make an API request from the browser"
lead: "How to make an API request from the browser"
date: 2022-07-06
lastmod: 2022-07-06
draft: false
images: []
menu:
  docs:
    parent: "Section 3: Data Acquisition Methods"
weight: 340
toc: true
---

The [Census Data API](https://www.census.gov/content/dam/Census/data/developers/api-user-guide/api-guide.pdf) provides a fast and simple way to access its data. To query the [ACS 5-year](https://www.census.gov/data/developers/data-sets/acs-5year.html) data, a request should be made to `https://api.census.gov/data/2020/acs/acs5` specifying the variables to fetch and the geographic level you want. For example, an API call requesting the total population count of commuters (`B08303_001E`) at the state level would look like this: 

```bash
https://api.census.gov/data/2020/acs/acs5?get=NAME,B08303_001E&for=state:*
```

* Visit this URL in a new tab on your browser to see how the data is returned.

Note that the part of the URL after `?`  contains the query parameters, each parameter is separated by `&`.

The `get` parameter specifies a comma-separated list of variables we want to fetch:
* `B08303_001E` is the number of commuters
* `NAME`  is the name of the geographic level

The `for` parameter specifies the geographic level
* we are requesting state-level data
* and we want all states, as indicated by the `*`.

In your browser, you may have also noticed an extra column of data at the end. This is automatically generated and is the code for geographic level of the data. In this case, the last column contains the [2-digit FIPS state code](https://en.wikipedia.org/wiki/Federal_Information_Processing_Standard_state_code#FIPS_state_codes).

A final note about requesting variables is that you can choose to request an entire variable group as well. For example, since we are looking at commute time, if we wanted to request all variables in the Time to Work B08303 group at the state-level, the API query would be:

```bash
https://api.census.gov/data/2020/acs/acs5?get=NAME,group(B08303)&for=state:*
```

Note the differences with our original query. Requesting by variable group will return all the variables in that group, annotations and errors included, no matter how many there are.

