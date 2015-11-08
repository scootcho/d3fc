---
layout: component
title: Mode Median
component: data/sampler/modeMedian.js
namespace: samplers

example-code: |
    // create some random data
    var data = fc.data.random.financial()(1000);

    // configure the sampler
    var sampler = fc.data.sampler.modeMedian()
      .bucketSize(20)
      .value(function(d) { return d.low; });

    // sample the data
    var sampledData = sampler(data);

    xScale.domain(fc.util.extent().fields('date')(sampledData))
    yScale.domain(fc.util.extent().fields('low')(sampledData));

    // render the sampled data
    var sampledLine = fc.series.line()
        .xScale(xScale)
        .yScale(yScale)
        .yValue(function (d) { return d.low; });

    container.append('g')
         .datum(sampledData)
         .call(sampledLine);

    // render the original data
    var originalLine = fc.series.line()
         .xScale(xScale)
         .yScale(yScale)
         .yValue(function (d) { return d.low; })
         .decorate(function(selection) {
             selection.enter().style('stroke-opacity', '0.5');
         });

    container.append('g')
          .datum(data)
          .call(originalLine);

---

The mode-median sampling component is a method of subsampling data to improve performance with large data sets. The component take an array of data as an input, returning a smaller, sampled array. You can configure the sampling frequency by setting the `bucketSize` property.

The example below creates an array of 1,000 datapoints, sampling it using a bucket-size of 20:

```js
{{{example-code}}}
```

Which gives the following:

{{>example-fixture}}