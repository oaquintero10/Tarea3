## Welcome to GitHub Pages

You can use the [editor on GitHub](https://github.com/oaquintero10/Tarea3/edit/master/README.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown


<!DOCTYPE html>
<meta charset="utf-8">
<style>

svg {
  font: 10px sans-serif;
}

.axis path,
.axis line {
  fill: none;
  stroke: #000;
  shape-rendering: crispEdges;
}

.x.axis path {
  fill:none;
  stroke:#000;
  shape-rendering: crispEdges;
}

.line {
  fill: none;
  stroke-width: 1px;
}

</style>

<body>
<script src="http://d3js.org/d3.v3.js"></script>
<script>

var parseDate = d3.time.format("%Y-%m-%d").parse;

var margin = {top: 20, right: 80, bottom: 30, left: 50},
    width = 900 - margin.left - margin.right,
    height = 500 - margin.top - margin.bottom;

var x = d3.scale.linear()
    .range([width,0])
    .domain([0,100]);

var y = d3.scale.linear()
    .range([height,0])
    .domain([40,170]);

var color = d3.scale.category10();

var xAxis = d3.svg.axis()
    .scale(x)
    .orient("bottom");

var yAxis = d3.svg.axis()
    .scale(y)
    .orient("left");

var line = d3.svg.line()
    .interpolate("basis")
    .x(function(d) { return x(d['percentRemaining']); })
    .y(function(d) { return y(d['heartRate']); });

var svg = d3.select("body").append("svg")
    .attr("width", width + margin.left + margin.right)
    .attr("height", height + margin.top + margin.bottom)
  .append("g")
    .attr("transform", "translate(" + margin.left + "," + margin.top + ")");

d3.csv("merged.csv", function(error, data) {

     color.domain([0,1,2,3,4,5,6,7,8,9,10,11,12]);

  data = data.map( function (d) {
    return {
      lookup: d['lookup'],
      date: parseDate(d['lookup']),
      percentRemaining: +d['percentRemaining']*100,
      heartRate: Math.round(d['heart_rate(bpm)']) };
});

  data = d3.nest()
  .key(function(d) { return d.lookup; })
  .sortKeys(d3.ascending)
  .sortValues(function(a,b) { return b.percentRemaining - a.percentRemaining; } )
  .entries(data);

console.log(data)

  svg.append("g")
      .attr("class", "x axis")
      .attr("transform", "translate(0," + height + ")")
      .call(xAxis);

  svg.append("g")
      .attr("class", "y axis")
      .call(yAxis);

  svg.append("text")
    .attr("x",0)
    .attr("y",-5)
    .attr("transform","rotate(90)")
    .text("Heart Rate (BPM)")

 svg.append("path")
     .attr("d", function(d) { return "M 0," + y(120) + ", L " + width + "," + y(120); })
     .attr("clip-path", "url(#rect-clip)")
     .style("stroke", "#000")
     .style("fill","none")
     .style("stroke-width", 2);

 svg.append("path")
     .attr("d", function(d) { return "M 0," + y(90) + ", L " + width + "," + y(90); })
     .attr("clip-path", "url(#rect-clip)")
     .style("stroke", "#000")
     .style("fill","none")
     .style("stroke-width", 2);

  // define the clipPath
  svg.append("clipPath")
      .attr("id", "rect-clip")
    .append("rect")
      .attr("width", width)
      .attr("height", height) ;

  var runs = svg.selectAll(".run")
      .data(data, function(d) { return d.key; })
    .enter().append("g")
      .attr("id", function(d) { return d.key; })
      .attr("class", "city");

  runs.append("text")
    .attr("class",function(d) { return "linetext" + d.key })
    .text(function(d) { return d.key })
    .attr("x", width - 130)
    .attr("y", 20)
    .style("fill", "none")
    .style("font", "25px sans-serif");

  runs.append("path")
      .attr("class","line")
      .attr("d", function(d) { return line(d.values); })
      .attr("clip-path", "url(#rect-clip)")
      .style("stroke", "#000")
      .style("stroke-opacity", .2)
      .on("mouseover", function() {

        d3.select(this)
        .transition()
        .duration(1)
        .style("stroke", "red")
        .style("stroke-width", 5)
        .style("stroke-opacity", 1);

        d3.selectAll(".linetext" + this.parentNode.id)
        .style("fill","#000");

      })
      .on("mouseout", function() {

        d3.select(this)
        .transition()
        .duration(1)
        .style("stroke", "#000")
        .style("stroke-width", 1)
        .style("stroke-opacity", .2);

        d3.selectAll(".linetext" + this.parentNode.id)
        .style("fill","none");

      });

});

</script>



```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/oaquintero10/Tarea3/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
