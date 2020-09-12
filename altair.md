# Overview

## Bar Chart

```python
trend_bar = alt.Chart(trends).mark_bar().encode(
    x="search_term",
    y="value",
    color="search_term"
)
```
## Line Chart
```python
trend_line = alt.Chart(trends).mark_line().encode(
    x='date:T',
    y='value',
    color='search_term'
)
```
## Scatter Chart
```python
alt.Chart(snake).mark_circle().encode(
    x=alt.X("evidence"),
    y="popularity",
    color='type'
)
```
## Map
```python
temp = world_temp.groupby("city").mean().reset_index()

points = alt.Chart(temp).mark_circle().encode(
    latitude="lat",
    longitude="lon",
    color=alt.Color("mean(temp)",scale=alt.Scale(domain=[-30,10,40],range=["lightblue","orange","red"]))
)

# remote geojson data object
url = "https://raw.githubusercontent.com/datasets/geo-countries/master/data/countries.geojson"
data_geojson_remote = alt.Data(url=url, format=alt.DataFormat(property='features',type='json'))

# chart object
background = alt.Chart(data_geojson_remote).mark_geoshape(
    fill="lightgrey",
    stroke="white"
).encode(
    color="properties.name:N"
).properties(
    projection={'type': 'identity', 'reflectY': True}
)

(background+points)
```
## Histogram
```python
alt.Chart(brain).mark_bar().encode(
    x=alt.X('Brain Weight',bin=True),
    y = 'count()'
)
```
## Selection
```python
select_country = alt.selection(type="single",encodings=["x"])
temp = world_temp.groupby(["country","date"]).mean().reset_index()

temp_bar = alt.Chart(temp).mark_bar().encode(
    x=alt.X("country",sort=alt.Sort(field="temp",op="mean",order="descending"),axis=None),
    y="mean(temp)",
    tooltip="country"
).properties(
    width=600
).add_selection(select_country)

temp_line = alt.Chart(temp).mark_line().encode(
    x=alt.X("date:T"),
    y="mean(temp)",
).properties(
    width=600
).transform_filter(
    select_country
)

temp_bar&temp_line
```
## Interval
```python
select_date = alt.selection(type="interval",encodings=["x"])

trend_line = alt.Chart(trends).mark_line().encode(
    x="yearmonth(date):T",
    y="mean(value)",
    color="search_term"
).properties(
    selection=select_date
)

trend_bar = alt.Chart(trends).mark_bar().encode(
    x="search_term",
    y="value",
    color="search_term",
    tooltip="search_term"
).transform_filter(
    select_date
)

trend_bar|trend_line
```
## Comp Hor
```python
background|points
```
## Comp Ver
```python
background&points
```
## Comp Sup
```python
background+points
```
