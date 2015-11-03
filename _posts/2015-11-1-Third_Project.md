---
layout: post
title: Third Project
---

Last week I completed my 3rd Data Science Project.  I analyzed course data from 16 different MOOCs and used a variety of machine learning algorithms to fine tune my predicitve models.  Along the way I learned about several different types of classification, imputing missing data and D3 visualizations.  Speaking of D3 visualizations, here is a D3 bar-chart that shows the accuracies of the baseline "dumb" predictors before any deeper analysis.

<center>
	<div border="black">
		<h2>Dumbly Predicting Student Grades</h2>
		<svg id="dumb">
		</svg>
	</div>
</center>

<script src="https://cdnjs.cloudflare.com/ajax/libs/d3/3.5.5/d3.min.js"></script>
<script src="http://labratrevenge.com/d3-tip/javascripts/d3.tip.v0.6.3.js"></script>

<script>

function graphPlot1() {

var margin = {top: 50, right: 50, bottom: 150, left: 40},
    width = 1050 - margin.left - margin.right,
    height = 500 - margin.top - margin.bottom;

var x0 = d3.scale.ordinal()
    .rangeRoundBands([0, width], .1);

var x1 = d3.scale.ordinal();

var y = d3.scale.linear()
    .range([height, 0]);

var formatPercent = d3.format(".0%");

var color = d3.scale.ordinal()
    .range(["#98abc5", 'white', "#7b6888", "#6b486b", "#a05d56", "#d0743c", "#ff8c00"]);

var xAxis = d3.svg.axis()
    .scale(x0)
    .orient("bottom");

var yAxis = d3.svg.axis()
    .scale(y)
    .orient("left")
    .tickFormat(d3.format(".2s"));


var tip = d3.tip()
  .attr('class', 'd3-tip')
  .offset([-10, 0])
  .html(function(d) {
    return "<strong>Accuracy:</strong> <span style='color:red'>" + Math.round(d.value) + "</span><strong> %</strong>";
  })

var svg = d3.select("div#dumb").append("svg")
    .attr("width", width + margin.left + margin.right)
    .attr("height", height + margin.top + margin.bottom)
  .append("g")
    .attr("transform", "translate(" + margin.left + "," + margin.top + ")");

 d3.csv("https://github.com/GregMFriedman/ds5/tree/master/projects/03-mcnulty/greg/comp.csv", function(error, data) {
  if (error) throw error;

  var modelNames = d3.keys(data[0]).filter(function(key) { return key == "Base"; });

  data.forEach(function(d) {
    d.scores = modelNames.map(function(name) { return {name: name, value: +d[name]}; });
  });

  x0.domain(data.map(function(d) { return d.Class; }));
  x1.domain(modelNames).rangeRoundBands([0, x0.rangeBand()]);
  y.domain([0, d3.max(data, function(d) { return d3.max(d.scores, function(d) { return d.value; }); })]);

  svg.append("g")
     svg.append("g")
        .attr("class", "x axis")
        .attr("transform", "translate(0," + height + ")")
        .call(xAxis)
        .selectAll("text")  
            .style("text-anchor", "end")
            .attr("dx", "-.8em")
            .attr("dy", ".15em")
            .attr("transform", function(d) {
                return "rotate(-65)" 
                });

  svg.call(tip);

  svg.append("g")
      .attr("class", "y axis")
      .call(yAxis)
    .append("text")
      .attr("transform", "rotate(-90)")
      .attr("y", 6)
      .attr("dy", ".71em")
      .style("text-anchor", "end")
      .text("Accuracy Score");

  var Class = svg.selectAll(".Class")
      .data(data)
    .enter().append("g")
      .attr("class", "g")
      .attr("transform", function(d) { return "translate(" + x0(d.Class) + ",0)"; });


  Class.selectAll("rect")
      .data(function(d) { return d.scores; })
    .enter().append("rect")
      .attr("width", x1.rangeBand())
      .attr("x", function(d) { return x1(d.name); })
      .attr("y", function(d) { return y(d.value); })
      .attr('id', function(d) {return d.name;})
      .attr("class", "bar")
      .attr("height", function(d) { return height - y(d.value); })
      .on('mouseover', tip.show)
      .on('mouseout', tip.hide)
      .on("click", animate);

      

  var legend = svg.selectAll(".legend")
      .data(modelNames.slice().reverse())
    .enter().append("g")
      .attr("class", "legend")
      .attr("transform", function(d, i) { return "translate(0," + i * 20 + ")"; });

  legend.append("rect")
      .attr("x", width + 40)
      .attr("width", 10)
      .attr("height", 10)
      .attr('id', function(d) {return d.name;})
      .style("fill", color);

      

  legend.append("text")
      .attr("x", width + 30)
      .attr("y", 5)
      .attr("dy", ".35em")
      .style("text-anchor", "end")
      .text(function(d) { return d; });



function animate() {
    d3.select(this).transition()
        .duration(500)
        .ease('elastic')
        .attr("y", function(d) { return y(d.value + 20); })
        .attr("height", function(d) { return height - y(d.value+20); })
      .transition()
        .delay(500)
        .attr("y", function(d) { return y(d.value); })
        .attr("height", function(d) { return height - y(d.value); })

};

  function somethingCool() {
  d3.select(this)
  	.attr("height", function(d) { return height - y(d.value); })
    .on('mouseover', tip.show)
    .on('mouseout', tip.hide)
    .transition()
    .duration(5000)
    .ease("elastic")
    .delay(100)
      .style("fill", "#7b6888")
      .style("stroke-width","0em");
}

d3.selectAll("#G-Model").on("mouseover",somethingCool);

});
}

graphPlot1();
    </script>




The main point of this project was to explore different types of classification techniques such as **Logistic Regression, Gaussian Naive Bayes, K Nearest Neighbors, Support Vector Machines, Decision Trees** and **Random Forests**.  In order to use these models, I needed to look at an aspect of the data set that was categorical.  I noticed that one feature of the data was level of education, which was classified into **less than secondary, Secondary, Bachelor's Degree, Master's Degree or Doctorate**.  I ended up drawing a line between Secondary and Bachelor's Degree and making it a binary classification exercise.  

After writing my own grid-search algorithm (Sklearn's took too long to run and didn't give me the output the way I wanted), I was able to predict level of education with an average of 85% accuracy (no lower than 78% accuracy in any course and as high as 94% in some courses).  Equipped with this fairly accurate predictor, I decided to impute whether or not a student had at least a Bachelor's degree for most of the rows for which Level of Education was previously missing.  This helped me salvage about 10% of my data set overall.  More importantly, for some of the MOOCs, imputing allowed me to restore over 25% of the incomplete data, which in turn greatly improved my prediction accuracy for student grades in those courses.


