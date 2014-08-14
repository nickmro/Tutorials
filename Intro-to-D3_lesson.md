# D3 Part 1: Intro to D3 #

## What is D3? ##

D3 is a JavaScript framework that allows web developers to build data visualizations on their web pages. It was developed by Mike Bostock who now works on the New York Times graphics desk.

D3 visualizes data (including JSON and CSV formats) and uses that data to draw scalable vector graphic (SVG) images to a web page.

## What is an SVG? ##

A scalable vector graphic is an image in XML format, which means that it is nothing more than text that can be rendered into an image in HTML5 and edited in illustration software such as Adobe Illustrator.

One of the things that makes an SVG so special (and which gives it its name) is that, unlike a PNG image, it can be increased in size without losing any quality.

Most importantly for our purposes, they can be interpreted by HTML5-compatible browsers to draw images to web pages.

One great example of what an SVG can be seen in this image of a tiger: [Ghostscript_Tiger.svg](http://upload.wikimedia.org/wikipedia/commons/f/fd/Ghostscript_Tiger.svg). If we save and open this image in a text editor, we can see that it is essentially just text.

## D3, SVG, and Data Visualization ##

Because SVGs are made of text that can be manipulated, scaled, and rendered by a web page, we can manipulate the text using JavaScript. This is what D3 does for us. It appends an SVG to the DOM to which it will draw circles, rectangles, and paths based on the data we feed it.

## Make a Simple Bar Graph  ##

### Include D3.js in your Scripts ###

In the head of your page (between the `<head>` and `</head>` tags, include the following code:

```
<script src="http://d3js.org/d3.v3.min.js" charset="utf-8"></script>
```

Create another `<script>` section. You are going to place your entire script in here.

### Initialize your Data ###

D3 accepts multiple formats for data, including JSON and CSV, but in keeping things simple, we will feed it an array.

```
var data = [1,5,4,9,2];
```

Note: the data variable name can be whatever you want, but you will reference it later.

### Specify the Size of your SVG and Padding ###

Specify the height and width of your SVG, as well as the padding you want between bars. Assign them to variables for the time being; they will be called on later.

```
var w = 500;
var h = 400;
var padding = 5;

```

### Set Up the Y Scale ###

As the data is now, it consists only of single-digit numbers. If we were to generate our bars so that the pixels matched the value of each value, our bars would be very small. That's why we need to set up the domain and range.

The domain is the range in which our data exists. For our y-scale, the domain will generally be from 0 through the maximum value in our dataset (in this case 9).

The range is the range in which we want our data to be outputted. For example, we don't want our maximum data point of 9 to show up as 9 pixels high; we want it to be much bigger, perhaps the height of the SVG element itself.

Set the y-scale domain as 0 to the maximum value in our data set.

Set the y-scale range as 0 to the height of our SVG.

```
var yScale = d3.scale.linear()
			.domain([0, d3.max(data, function(d) { return d })])
			.range([0, h]);
```

Note: we need functions to return values from our date

### Append the SVG to the DOM ###

In order to append the SVG to the DOM, we need to specify a div with a unique ID in which we would like to place it. In this case, we will attach it to a div with the ID "svgDiv". We also must specify the width and height.

```
var svg = d3.select("#svgDiv")
			.append("svg")
			.attr("width", w)
			.attr("height", h);
```

### Draw the Data to the SVG ###

This is where the magic happens. We are now going to draw rectangles to the SVG to create the bars in our bar graph.

```
svg.selectAll("rect")
			.data(data)
			.enter()
			.append("rect")
			.attr("x", function(d, i) {
				return i * ((w / data.length) + padding)
			})
			.attr("y", function(d){
				return h - yScale(d)
			})
			.attr("width", w/data.length - padding)
			.attr("height", function(d) {
				return yScale(d)
			})
			.attr("fill", "red");
```

Let's break it down:

```
svg.selectAll("rect")
```
tells D3 we are making rectangles

```
.data(data)
```
takes in your data

```
.enter()
```
enters the data

```
.append("rect")
```
appends a rectangle for each of your data points

```
.attr("x", function(d, i) {
				return i * ((w / data.length) + padding)
			})
```
sets the starting x coordinate of each bar according to its index number in the dataset

```
.attr("y", function(d){
				return h - yScale(d)
			})
```
sets the starting y coordinate for each bar according to the value of the data point; must be subtracted from bottom because y coordinate 0 is located at the top

```
.attr("width", w/data.length - padding)
```
sets the width of each bar according to the width of the SVG element, the number of values we have, and the padding between each bar

```
.attr("height", function(d) {
				return yScale(d)
			})
```
sets the height of each bar according to the value of each data point

```
.attr("fill", "red")
```
sets the fill color for each bar


### JavaScript Event to Execute the Script ###

In order for the script to execute, we should attach it to a JavaScript event. In this case, let's just wrap the function in a `window.onload` function so that it renders when the window loads:

```
window.onload = function() { //D3 Script in here }
```

## Next Lesson ##

In the next lesson I will discuss how to incorporate your D3 so that it reads with your MongoDB data from your Rails app.

## Resources ##

[D3.js Website](http://http://d3js.org/)

[Interactive Data Visualization for the Web](http://chimera.labs.oreilly.com/books/1230000000345/) - a great introduction to D3.js

[Other JavaScript Frameworks and Tools for Data Visualization](http://www.fastcodesign.com/3029239/infographic-of-the-day/30-simple-tools-for-data-visualization)