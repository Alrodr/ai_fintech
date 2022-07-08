---
title: "How to build and share ML apps with Streamlit. Part I"
description: "A faster way to build and share data apps"
lead: "A faster way to build and share data apps"
date: 2022-07-07
lastmod: 2022-07-07
draft: false
images: []
menu:
  docs:
    parent: "Section 4: Machine Learning Deployment"
weight: 500
toc: true
---
{{< alert icon="💻" context="info" text="<b>TL;DR</b> Streamlit turns data scripts into shareable web apps in minutes. All in pure Python. No front‑end experience required." />}}

Notice that this tutorial is only a recap of what can be found in the [official documentation](https://docs.streamlit.io/).

## Setting up

Before you get started, you are going to need a few things. Your favorite IDE, Python 3.7-3.10 and _PIP_. Regardless of which package management tool you are using, I recommend running the commands on this page in a virtual environment. This ensures that the dependencies pulled in for Streamlit don't impact any other Python projects you are working on.

### Create a new environment with Streamlit

``` bash
pip install streamlit

// Test that the installation worked:
streamlit hello
```

Streamlit's Hello app should appear in a new tab in your web browser!

## Create your first app

1. The first step is to create a new Python script. Let's call it `app.py`
2. Open `app.py` in your IDE, then add these lines:

``` python
import streamlit as st
import pandas as pd
import numpy as np
```

3. Add a title and run Streamlit from the command line doing `streamlit run app.py`. Running a Streamlit app is no different than any other Python script. Whenever you need to view the app, you can use this command.

``` python
st.title('My awesome App')
```

4. As usual, the app should automatically open in a new tab in your browser.

### Fetch some data

Now that you have an app, the next thing you'll need to do is fetch your dataset. Let's start by writing a function to load the data. Add this code to your script:

``` python
DATE_COLUMN = 'date/time'
DATA_URL = ('https://s3-us-west-2.amazonaws.com/'
         'streamlit-demo-data/uber-raw-data-sep14.csv.gz')

def load_data(nrows):
    data = pd.read_csv(DATA_URL, nrows=nrows)
    lowercase = lambda x: str(x).lower()
    data.rename(lowercase, axis='columns', inplace=True)
    data[DATE_COLUMN] = pd.to_datetime(data[DATE_COLUMN])
    return data
```

You'll notice that `load_data` is a plain old function that downloads some data, puts it in a Pandas dataframe, and converts the date column from text to datetime. The function accepts a single parameter (`nrows`), which specifies the number of rows that you want to load into the dataframe.

Now let's test the function and review the output. Below your function, add these lines:

``` python
# Create a text element and let the reader know the data is loading.
data_load_state = st.text('Loading data...')
# Load 10,000 rows of data into the dataframe.
data = load_data(10000)
# Notify the reader that the data was successfully loaded.
data_load_state.text('Loading data...done!')
```

### Inspect the raw data

It's always a good idea to take a look at the raw data you're working with before you start working with it. Let's add a subheader and a printout of the raw data to the app:

``` python
st.subheader('Raw data')
st.write(data)
```

### Draw a histogram

Now that you've had a chance to take a look at the dataset and observe what's available, let's take things a step further and draw a histogram

``` python
# To start, let's add a subheader just below the raw data section:
st.subheader('Number of pickups by hour')

# Use NumPy to generate a histogram that breaks down pickup times binned by hour:
hist_values = np.histogram(data[DATE_COLUMN].dt.hour, bins=24, range=(0,24))[0]

# Now, let's use Streamlit's st.bar_chart() method to draw this histogram
st.bar_chart(hist_values)
```

To draw this diagram we used Streamlit's native `bar_chart()` method, but it's important to know that Streamlit supports more complex charting libraries like Altair, Bokeh, Plotly, Matplotlib and more. For a full list, see [supported charting libraries](https://docs.streamlit.io/library/api-reference/charts).

### Filter results with a slider and use a button to toggle data

In this last section I have added a slider and a button. I have added all the code so you can test it immediately without any problem. You can find all explanations in the streamlist's documentation.

```python
import streamlit as st
import pandas as pd
import numpy as np

st.title('Uber pickups in NYC')

DATE_COLUMN = 'date/time'
DATA_URL = ('https://s3-us-west-2.amazonaws.com/'
            'streamlit-demo-data/uber-raw-data-sep14.csv.gz')

@st.cache
def load_data(nrows):
    data = pd.read_csv(DATA_URL, nrows=nrows)
    lowercase = lambda x: str(x).lower()
    data.rename(lowercase, axis='columns', inplace=True)
    data[DATE_COLUMN] = pd.to_datetime(data[DATE_COLUMN])
    return data

data_load_state = st.text('Loading data...')
data = load_data(10000)
data_load_state.text("Done! (using st.cache)")

if st.checkbox('Show raw data'):
    st.subheader('Raw data')
    st.write(data)

st.subheader('Number of pickups by hour')
hist_values = np.histogram(data[DATE_COLUMN].dt.hour, bins=24, range=(0,24))[0]
st.bar_chart(hist_values)

# Some number in the range 0-23
hour_to_filter = st.slider('hour', 0, 23, 17)
filtered_data = data[data[DATE_COLUMN].dt.hour == hour_to_filter]

st.subheader('Map of all pickups at %s:00' % hour_to_filter)
st.map(filtered_data)
```

## Run and test your App

Finally, it's time to run Streamlit from the command line:

```bash
streamlit run uber_pickups.py
```