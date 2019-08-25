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

## Altair Visualization Exploration


```python
import pandas as pd
import altair as alt
```

```python
from vega_datasets import data
```

```python
cars = data.cars()
type(cars)
```

```python
cars.head()
```

```python
data.ffox.url
```

```python
data.cars.url
```

```python
df = pd.DataFrame({
    'city': ['Seattle', 'Seattle', 'Seattle', 'New York', 'New York', 'New York', 'Chicago', 'Chicago', 'Chicago'],
    'month': ['Apr', 'Aug', 'Dec', 'Apr', 'Aug', 'Dec', 'Apr', 'Aug', 'Dec'],
    'precip': [2.68, 0.87, 5.31, 3.94, 4.13, 3.58, 3.62, 3.98, 2.56]
})

df
```

```python
chart = alt.Chart(df)
```

```python
alt.Chart(df).mark_point()
```

```python
alt.Chart(df).mark_point().encode(
  y='city',
)
```

```python
alt.Chart(df).mark_point().encode(
    x='precip',
    y='month',
    shape='city'
)
```

```python
alt.Chart(df).mark_point().encode(
    x='precip',
    y='city'
)
```

```python
alt.Chart(df).mark_area().encode(
    alt.X('precip'),
    alt.Y('city')
)
```

You can override type of vairable with encoding specificiation - https://altair-viz.github.io/user_guide/encoding.html#encoding-data-types

```python
alt.Chart(df).mark_point().encode(
    alt.X('precip'),
    alt.Y('city:N') 
)
```

```python
# summarize data
# "mean", "average", "median", "q1", "q3", "min", "max" 
# For other aggregations that produce values outside of the raw data domain (e.g. "count", "sum" )
alt.Chart(df).mark_point().encode(
    x='average(precip)', #, average, sum, etc.
    y='city'
).display()
alt.Chart(df).mark_point().encode(
    x='median(precip)', #, average, sum, median, etc.
    y='city'
).display()
```

```python
a = alt.Chart(df).mark_point(color='red').encode(
    alt.X('average(precip)'), #, average, sum, etc.
    y='city',
    color=alt.Color('Average:N', scale=alt.Scale(range=['red']))
).display()

a = alt.Chart(df).mark_point(color='red').encode(
    alt.X('average(precip)'), #, average, sum, etc.
    y='city',
    color=alt.Color('average(precip):N', title='Average', scale=alt.Scale(range=['red'], domain=['precip']))
)


m = alt.Chart(df).mark_point(color='green').encode(
    x='median(precip)', #, average, sum, median, etc.
    y='city',
    color=alt.Color('median(precip):N', title='Median', scale=alt.Scale(range=['green'], domain=['precip']))
)
(a+m).resolve_scale(color='independent')
```

```python
alt.Chart(df).mark_bar().encode(
    x='average(precip)',
    y='city'
)
```

```python
alt.Chart(df).mark_bar().encode(
    x='city',
    y='average(precip)'
)
```

```python
alt.Chart(df).mark_point(color='firebrick').encode(
  alt.X('precip', scale=alt.Scale(type='log'), axis=alt.Axis(title='Log-Scaled Values')),
  alt.Y('city', axis=alt.Axis(title='Category')),
)
```

```python
#if just specify color, it is like specifying stroke only
alt.Chart(df).mark_point(stroke='red', size=50, fill='blue').encode(
  alt.X('precip', scale=alt.Scale(type='log'), axis=alt.Axis(title='Log-Scaled Values')),
  alt.Y('city', axis=alt.Axis(title='Category')),
)
```

```python
#if just specify color, it is like specifying stroke and fill='white'
alt.Chart(df).mark_point(stroke='red', fill='white').encode(
  alt.X('precip', scale=alt.Scale(type='log'), axis=alt.Axis(title='Log-Scaled Values')),
  alt.Y('city', axis=alt.Axis(title='Category')),
)
```

```python
alt.Chart(cars).mark_line().encode(
    alt.X('Year'),
    alt.Y('average(Miles_per_Gallon)')
)
```

To combine multiple charts into one, `+` them

To view charts separately, `.display()` them

```python
line = alt.Chart(cars).mark_line().encode(
    alt.X('Year'),
    alt.Y('average(Miles_per_Gallon)')
)

point = alt.Chart(cars).mark_circle().encode(
    alt.X('Year'),
    alt.Y('average(Miles_per_Gallon)')
)

line + point
```

```python
mpg = alt.Chart(cars).mark_line().encode(
    alt.X('Year'),
    alt.Y('average(Miles_per_Gallon)')
)

mpg + mpg.mark_circle(size=100)
```

```python
hp = alt.Chart(cars).mark_line().encode(
    alt.X('Year'),
    alt.Y('average(Horsepower)')
)

(mpg + mpg.mark_circle(size=50)) | (hp + hp.mark_circle(size=50))
```

```python

```
