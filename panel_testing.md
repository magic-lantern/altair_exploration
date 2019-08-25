---
jupyter:
  jupytext:
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.1'
      jupytext_version: 1.2.1
  kernelspec:
    display_name: Python 3
    language: python
    name: python3
---

```python
import panel as pn
import altair as alt
from altair import datum
import pandas as pd
from vega_datasets import data
import datetime as dt
```

```python
pn.extension('vega')
```

```python
type(data.stocks())
```

```python
source = data.stocks()
source.head()
```

```python
source['date'].describe()
```

```python
# this creates the date range slider
date_range_slider = pn.widgets.DateRangeSlider(
    name='Date Range Slider',
    start=dt.datetime(2000, 1, 1),
    end=dt.datetime(2010, 3, 1),
    value=(dt.datetime(2001, 1, 1), dt.datetime(2010, 1, 1))
)
```

```python
# create list of company names (tickers) to use as options
tickers = ['AAPL', 'GOOG', 'IBM', 'MSFT']# this creates the dropdown widget
ticker = pn.widgets.Select(name='Company', options=tickers)
```

```python
title = '### Stock Price Dashboard'
subtitle = 'This dashboard allows you to select a company and date range to see stock prices.'
```

```python
@pn.depends(ticker.param.value, date_range_slider.param.value)
def get_plot(ticker, date_range): # start function

    # Load and format the data
    df = source # define df
    df['date'] = pd.to_datetime(df['date']) # format date as datetime
    
    # create a date filter that uses values from the date range slider
    start_date = date_range_slider.value[0] # store the first date range slider value in a var
    end_date = date_range_slider.value[1] # store the end date in a var
    mask = (df['date'] > start_date) & (df['date'] <= end_date) # create filter mask for the dataframe
    df = df.loc[mask] # filter the dataframe
    
    # create the Altair chart object
    chart = alt.Chart(df).mark_area(color="#0c1944", opacity=0.8).encode(x='date', y='price', tooltip=alt.Tooltip(['date','price'])).transform_filter(
        (datum.symbol == ticker) # this ties in the filter from the dropdown selection
    )
    
    return chart
```

```python
dashboard = pn.Row(
    pn.Column(title, subtitle, ticker, date_range_slider),
    get_plot, # draw chart function!
    margin=(0,0,30,0) # default leaves some text chopped off in Firefox, Safari, Chrome on macOS
)
```

```python
dashboard.servable()
dashboard.show()
```

```python
dashboard
```

```python
? pn.Column
```

```python

```
