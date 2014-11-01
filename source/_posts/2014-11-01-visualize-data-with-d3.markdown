---
layout: post
title: "visualize data with d3"
date: 2014-11-01 22:00:10 +0800
comments: true
categories: 
---

## what is d3js

(http://d3js.org)[d3js] is very popular in visualization recently, it has unique way to render chart compare to other libraries. d3 refer to data-driven document.

## setup html

    <html>
      <head>
        <title>D3</title>
        <script src="http://d3js.org/d3.v3.min.js" charset="utf-8"></script>
      </head>
      <body>
        <h2>D3js example</h2>
        <svg width=500 height=300>
        </svg>
        <script>
        </script>
      </body>
    </html>
    
## use data in previous post

    var crimesData = [
	{
		"Crime type" : "Anti-social behaviour",
		"total" : 5593
	},
	{
		"Crime type" : "Burglary",
		"total" : 888
	},
	{
		"Crime type" : "Criminal damage and arson",
		"total" : 1215
	},
	{
		"Crime type" : "Other theft",
		"total" : 1343
	},
	{
		"Crime type" : "Violence and sexual offences",
		"total" : 2123
	},
	{
		"Crime type" : "Drugs",
		"total" : 292
	},
	{
		"Crime type" : "Public order",
		"total" : 577
	},
	{
		"Crime type" : "Shoplifting",
		"total" : 1019
	},
	{
		"Crime type" : "Vehicle crime",
		"total" : 689
	},
	{
		"Crime type" : "Possession of weapons",
		"total" : 22
	},
	{
		"Crime type" : "Bicycle theft",
		"total" : 271
	},
	{
		"Crime type" : "Other crime",
		"total" : 88
	},
	{
		"Crime type" : "Theft from the person",
		"total" : 172
	},
	{
		"Crime type" : "Robbery",
		"total" : 91
	}
    ]

## write code to render bar chart

      var extent = d3.extent(crimesData, function(d){return d.total})
      var widthScale = d3.scale.linear().domain(extent).range([0, 500])
      var colorScale = d3.scale.category10()

      d3.select("svg").selectAll("rect")
        .data(crimesData)
      .enter()
        .append("rect")
        .attr("x", function(d, i){ return 0 })
        .attr("y", function(d, i){ return i * 30})
        .attr("width", function(d, i){ return widthScale(d.total)})
        .attr("height", function(d, i){ return 25})
        .attr("fill", function(d, i){ return colorScale(i)})

### get min/max value with d3.exten

    var extent = d3.extent(crimesData, function(d){return d.total})

### add scale to map domain value to width in chart

    var widthScale = d3.scale.linear().domain(extent).range([0, 500])

###  add color scale to fill different bar with colors

    var colorScale = d3.scale.category10()

### select element and bind data 

    d3.select("svg").selectAll("rect")
      .data(crimesData)
    .enter()
      .append("rect")
      .attr("x", function(d, i){ return 0 })
      .attr("y", function(d, i){ return i * 30})
      .attr("width", function(d, i){ return widthScale(d.total)})
      .attr("height", function(d, i){ return 25})
      .attr("fill", function(d, i){ return colorScale(i)})

Here I first select the svg in the html, then select 'rect' in this svg and bind crimes data with those rect, enter function is used to start render the element that with data binding, first I append a new rect in the svg, assign different attributes with scales defined before, notice that the function as attr second parameter as two parameter d and i, d represent each item in the crimes data and i of cource is the index of the item in crimes data.


### Final result

  [http://codepen.io/anon/pen/IwlDA](http://codepen.io/anon/pen/IwlDA)


Noted that we still missing a lot of stuff in this chart like x/y axis, labels though, d3js offer functions/generators for that.

If you understand how svg works, you'll know that d3js just help us to render svg elements with helper functions.
