---
title: percentile Function
keywords: query language reference
tags: [reference page]
sidebar: doc_sidebar
permalink: ts_percentile.html
summary: Reference to the percentile() function
---

## Summary

```
percentile(<percentage>, <expression>[,metrics|sources|sourceTags|pointTags|<pointTagKey>])
```
Returns the estimated `percentage` percentile across the set of time series described by the expression. 
The results might be computed from real reported values and interpolated values.
Use  [`rawpercentile()`](ts_rawpercentile.html) if you don't want interpolation.

## Parameters
<table>
<tbody>
<thead>
<tr><th width="30%">Parameter</th><th width="70%">Description</th></tr>
</thead>
<tr>
<td>percentage</td>
<td>A number greater than 0 and less than or equal to 100 that specifies the percentile of interest. You can include as many decimal points as you like.</td></tr>
<tr>
<td markdown="span"> [expression](query_language_reference.html#expressions)</td>
<td>Expression describing the set of time series to estimate the specified percentile for. </td></tr>
<tr>
<td>metrics&vert;sources&vert;sourceTags&vert;pointTags&vert;&lt;pointTagKey&gt;</td>
<td>Optional 'group by' parameter for organizing the time series into subgroups and then estimating the specified percentile for each subgroup.
Use one or more parameters to group by metric names, source names, source tag names, point tag names, values for a particular point tag key, or any combination of these items. Specify point tag keys by name.</td>
</tr>
</tbody>
</table>


## Description

The `percentile()` aggregation function lets you estimate the specified percentile for the data values at each moment in time, across the time series that are represented by the expression. 

A percentile is a value below which a particular percentage of data values fall. You specify the percentage as a positive real number that is less than or equal to 100. For example, if you specify a percentage of 99.4, `percentile()` returns a value at the 99.4th percentile for each successive moment in time. You can specify a percentage of 50 to obtain the median value across the time series. See [Computing an Estimated Percentile](#computing-an-estimated-percentile) for more details.

By default, `percentile()` returns a single series of percentile values across all time series. You can optionally group the time series based on one or more characteristics, and obtain a separate series of percentile values for each group.

If any time series has data gaps, `percentile()` fills them in by interpolation whenever possible. 

### Grouping

Like all aggregation functions, `percentile()` returns a single series of percentiles by default.  You can include a 'group by' parameter to return percentiles for separate groups of time series that share common metric names, source names, source tags, point tags, or values for a particular point tag key. 
The function returns a separate series of results corresponding to each group.

You can specify multiple 'group by' parameters to group the time series based on multiple characteristics. For example, `percentile(ts(50, "cpu.cpu*"), metrics, Customer)` first groups by metric names, and then groups by the values of the `Customer` point tag.

### Interpolation

If any time series has gaps in its data, Wavefront attempts to fill these gaps with interpolated values before applying the function. 
A value can be interpolated into a time series only if at least one other time series reports a real data value at the same moment in time.

Within a given time series, an interpolated value is calculated from two real reported values on either side of it. 
Sometimes interpolation is not possible--for example, when a new value has not been reported yet in a live-view chart. 
In this case, Wavefront finds the last known reported value in the series, and assigns it to any subsequent moment in time for which a real reported data value is present in some other time series. We use the last known reported value only if interpolation can’t occur _and_ if the last known reported value has been reported within the last 15% of the query time in the chart window.

You can use [`rawpercentile()`](ts_rawpercentile.html) to suppress interpolation.  See [Standard Versus Raw Aggregation Functions](query_language_aggregate_functions.html).

### Computing an Estimated Percentile

A percentile is a value below which a particular percentage of data values fall. For example, in a given group of data values, the 75th percentile is a value that is greater than 75% of the data values in that group.

In general, percentiles can be estimated in various ways. The `percentile()` function computes the estimated percentile value for a given moment in time as follows:
1. Obtain the set of reported and/or interpolated data values, one from each time series in the group. 
2. Sort these data values into an ordered list.
3. Use the specified `percentage` to compute the estimated percentile position in the list, and then:
  * If the estimated percentile position is a whole number, return the data value at that position. 
  * If the estimated percentile position is fractional, find the two data values on either side of it, and use them to interpolate the value to return. 
  * If the estimated percentile position falls before the list's first position (or after the list's last position), return the smallest (or largest) data value in the list.   

The formula for computing the estimated percentile position `pos` is `pos = percentage * (n+1)/100`, where `n` is the number of data values.



## Examples

The following simple example shows the median (50th percentile) for the requests latency. It's a single line because `percentile()` is an aggregation function.

![percentile simple](images/ts_percentile.png)

The following example shows the 75th percentile for the request latency. Here, we group by the `env` point tag and get a line for `env=production` and a line for `env=dev`.

![percentile grouped](images/ts_percentile_groupby.png)

<!---See also ticket 2775--->
