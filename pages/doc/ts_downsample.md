---
title: downsample Function
keywords: query language reference
tags: [reference page]
sidebar: doc_sidebar
permalink: ts_downsample.html
summary: Reference to the downsample() function
---
## Summary
```
downsample(<timeWindow>, <expression>)
```
Returns the values in the expression that occur in each time window. For example, `downsample(30m, ts(my.metric)` returns the values of `my.metric` every half hour.


## Parameters
<table>
<tbody>
<thead>
<tr><th width="20%">Parameter</th><th width="80%">Description</th></tr>
</thead>
<tr>
<td>timeWindow</td>
<td>The filter time window. The function filters the data so you see only the data for the specified time window.  </td>
</tr>
<tr>
<td markdown="span"> [expression](query_language_reference.html#expressions)</td>
<td>Expression to downsample. </td>
</tr>
</tbody>
</table>

## Description

The `downsample()` function allows you to filter any `ts()` expression to just the values occurring every `timeWindow`.

For example, to see the values for every half-hour of a given metric,  enter the following `ts()` expression:

``
downsample(30, ts(“my.metric”))
``

## Examples

The following example shows the CPU load average for a specified source (blue line). The dashed orange line shows only one value every 30 minutes.

![downsample example](images/ts_downsample.png)


## See Also

[Series Matching](query_language_series_matching.html)
