# something before path--d3 lines and text
```
var dataArray = [5,11,18];

var svg = d3.select("body").append("svg").attr("height","100%").attr("width","100%");

svg.selectAll("rect")
      .data(dataArray)
      .enter().append("rect")
                .attr("height",function(d,i){ return d*15; })
                .attr("width","50")
                .attr("fill","pink")
                .attr("x",function(d,i){ return 60*i; })
                .attr("y",function(d,i){ return 300-(d*15); });

var newX = 300;
svg.selectAll("circle.first")
      .data(dataArray)
      .enter().append("circle")
                .attr("class","first")
                .attr("cx",function(d,i){ newX+=(d*3)+(i*20); return newX; })
                .attr("cy","100")
                .attr("r",function(d){ return d*3; });

var newX = 600;
svg.selectAll("ellipse")
      .data(dataArray)
      .enter().append("ellipse")
                .attr("class","second")
                .attr("cx",function(d,i){ newX+=(d*3)+(i*20); return newX; })
                .attr("cy","100")
                .attr("rx",function(d){ return d*3; })
                .attr("ry","30");

var newX = 900;
svg.selectAll("line")
      .data(dataArray)
      .enter().append("line")
                .attr("x1",newX)
                .attr("stroke-width","2")
                .attr("y1",function(d,i){ return 80+(i*20); })
                .attr("x2",function(d){ return newX+(d*15); })
                .attr("y2",function(d,i){ return 80+(i*20); });

var textArray = ['start','middle','end'];
svg.append("text").selectAll("tspan")
    .data(textArray)
    .enter().append("tspan")
      .attr("x",newX)
      .attr("y",function(d,i){ return 150 + (i*30); })
      .attr("fill","none")
      .attr("stroke","blue")
      .attr("stroke-width","2")
      .attr("dominant-baseline","middle")
      .attr("text-anchor","start")
      .attr("font-size","30")
      .text(function(d){ return d; });

svg.append("line")
      .attr("x1",newX)
      .attr("y1","150")
      .attr("x2",newX)
      .attr("y2","210");
```
# D3 path 
## for more things about D3 Path, see https://www.w3.org/TR/SVG/paths.html
d3.line is different from svg line, the d3.line is a generator 

Create a group in D3
```var dataArray=[{x:5,y:5},{x:10,y:15},{x:20,y:7},{x:30,y:10},{x:40,y:10}];

var svg=d3.select('body').append('svg').attr('height','100%').attr('width','100%');

var line=d3.line()
                .x(function(d,i){return d.x*6; })
                .y(function(d,i){return d.y*6 ;})
                //.curve(d3.curveStep);
                .curve(d3.curveCardinal)

svg.append('path')
                .attr('fill','none')
                .attr('stroke','blue')
                .attr('d',line(dataArray));
```
###Go here to check more interpolate functions: https://github.com/d3/d3/blob/master/API.md
Build an area by D3.
```
var dataArray = [25,26,28,32,37,45,55,70,90,120,135,150,160,168,172,177,180];
var dataYears = ['2000','2001','2002','2003','2004','2005','2006','2007','2008','2009','2010','2011','2012','2013','2014','2015','2016'];

var height = 200;
var width = 500;


var area = d3.area()
                .x(function(d,i){ return i*20; })
                .y0(height)
                .y1(function(d){ return height-d; });
var svg = d3.select("body").append("svg").attr("height","100%").attr("width","100%");
svg.append("path").attr("d",area(dataArray));
```
### For http://bl.ocks.org more code and D3 resourses
### Or **Mike Bostock** example, which is good

Create a group thing in D3
```
var dataArray = [{x:5,y:5},{x:10,y:15},{x:20,y:7},{x:30,y:18},{x:40,y:10}];
var interpolateTypes=[d3.curveLinear,d3.curveNatural,d3.curveStep,d3.curveBasis,d3.curveBundle,d3.curveCardinal];

var svg = d3.select("body").append("svg").attr("height","100%").attr("width","100%");

for(var p=0; p<6; p++){

              var line = d3.line()
                              .x(function(d,i){ return d.x*6; })
                              .y(function(d,i){ return d.y*4; })
                              .curve(interpolateTypes[p]);
              var shiftx=250*p;
              var shift7=0;
              var chartGroup=svg.append('g')
                    .attr('class','group'+p)
                    .attr('transform','translate('+shiftx+',0)');
              chartGroup.append("path")
                    .attr("fill","none")
                    .attr("stroke","blue")
                    .attr("d",line(dataArray));

              chartGroup.selectAll('circle.g'+p).data(dataArray)
                    .enter().append('circle')
                    .attr('class', function(d,i){return 'g'+i;})
                    .attr('cx',function(d,i){ return d.x*6; })
                    .attr('cy',function(d,i){ return d.y*4; })
                    .attr('r','2');
}
```
We can change a group style in style.css file, like following:
```
html, body {
  margin: 0;
  height: 100%;
}

line {
  stroke: red;
}
g.group0 path,
g.group0 circle{
  stroke: red;
  fill:none;
}

```
Create a linear scale in D3
```
var dataArray = [25,26,28,32,37,45,55,70,90,120,135,150,160,168,172,177,180];
var dataYears = ['2000','2001','2002','2003','2004','2005','2006','2007','2008','2009','2010','2011','2012','2013','2014','2015','2016'];

var height = 200;
var width = 500;

var y=d3.scaleLinear()
        .domain([0,100])
        .range([height,0]);

console.log(y(0));
console.log(y(90));
console.log(y(180));

var area = d3.area()
                .x(function(d,i){ return i*20; })
                .y0(height)
                .y1(function(d){ return y(d); });
var svg = d3.select("body").append("svg").attr("height","100%").attr("width","100%");
svg.append("path").attr("d",area(dataArray));
```
Create a linear axis
```
var dataArray = [25,26,28,32,37,45,55,70,90,120,135,150,160,168,172,177,180];
var dataYears = ['2000','2001','2002','2003','2004','2005','2006','2007','2008','2009','2010','2011','2012','2013','2014','2015','2016'];

var height = 200;
var width = 500;
var margin={left:50,right:50,top:40,bottom:0};
var y = d3.scaleLinear()
            .domain([0,180])
            .range([height,0]);
var yAxis=d3.axisLeft(y);

var area = d3.area()
                .x(function(d,i){ return i*20; })
                .y0(height)
                .y1(function(d){ return y(d); });
var svg = d3.select("body").append("svg").attr("height","100%").attr("width","100%");
var chartGroup=svg.append('g').attr('transform','translate('+margin.left+','+margin.top+')');
chartGroup.append("path").attr("d",area(dataArray));
chartGroup.append('g').attr('class','axis y').call(yAxis);
```
Formatting your linear axis scale the way you want
```
var dataArray = [25,26,28,32,37,45,55,70,90,120,135,150,160,168,172,177,180];
var dataYears = ['2000','2001','2002','2003','2004','2005','2006','2007','2008','2009','2010','2011','2012','2013','2014','2015','2016'];

var height = 200;
var width = 500;

var margin = {left:50,right:50,top:40,bottom:0};

var y = d3.scaleLinear()
            .domain([0,180])
            .range([height,0]);

var yAxis = d3.axisLeft(y).ticks(3).tickPadding(10).tickSize(10);

var area = d3.area()
                .x(function(d,i){ return i*20; })
                .y0(height)
                .y1(function(d){ return y(d); });
var svg = d3.select("body").append("svg").attr("height","100%").attr("width","100%");
var chartGroup = svg.append("g").attr("transform","translate("+margin.left+","+margin.top+")");

chartGroup.append("path").attr("d",area(dataArray));
chartGroup.append("g")
      .attr("class","axis y")
      .call(yAxis);
```
Also in style.css:
```
html, body {
  margin: 0;
  height: 100%;
}

line {
  stroke: red;
}

g.group0 path,
g.group0 circle {
  stroke: red;
  fill: none;
}
g.tick line{
  stroke:blue;
}
g.tick text{
  font-size: 15px;
  fill:purple;
}
```
Create time-series axis
```
var dataArray = [25,26,28,32,37,45,55,70,90,120,135,150,160,168,172,177,180];
var dataYears = ['2000','2001','2002','2003','2004','2005','2006','2007','2008','2009','2010','2011','2012','2013','2014','2015','2016'];

var parseDate = d3.timeParse("%Y");


var height = 200;
var width = 500;

var margin = {left:50,right:50,top:40,bottom:0};

var y = d3.scaleLinear()
            .domain([0,d3.max(dataArray)])
            .range([height,0]);
var x = d3.scaleTime()
            .domain(d3.extent(dataYears,function(d){ return parseDate(d); }))
            .range([0,width]);



var yAxis = d3.axisLeft(y).ticks(3).tickPadding(10).tickSize(10);
var xAxis=d3.axisBottom(x);

var area = d3.area()
                .x(function(d,i){ return x(parseDate(dataYears[i])); })
                .y0(height)
                .y1(function(d){ return y(d); });
var svg = d3.select("body").append("svg").attr("height","100%").attr("width","100%");
var chartGroup = svg.append("g").attr("transform","translate("+margin.left+","+margin.top+")");

chartGroup.append("path").attr("d",area(dataArray));
chartGroup.append("g")
      .attr("class","axis y")
      .call(yAxis);
chartGroup.append('g').attr('class','axis x')
          .attr('transform','translate(0,'+height+')')
          .call(xAxis);
```
Create the ordinal scale
```
var dataArray = [5,11,18];
var dateDays=['Mon','Wed','Fri'];

var x=d3.scaleBand()
        .domain(dateDays)
        .range([0,170])
        .paddingInner(0.1176);
var xAxis=d3.axisBottom(x);

var svg = d3.select("body").append("svg").attr("height","100%").attr("width","100%");

svg.selectAll("rect")
      .data(dataArray)
      .enter().append("rect")
                .attr("height",function(d,i){ return d*15; })
                .attr("width","50")
                .attr("fill","pink")
                .attr("x",function(d,i){ return 60*i; })
                .attr("y",function(d,i){ return 300-(d*15); });

svg.append('g')
  .attr('class','axis hidden x')
  .attr('transform','translate(0,300)')
  .call(xAxis);
var newX= 300;
svg.selectAll("circle.first")
      .data(dataArray)
      .enter().append("circle")
                .attr("class","first")
                .attr("cx",function(d,i){ newX+=(d*3)+(i*20); return newX; })
                .attr("cy","100")
                .attr("r",function(d){ return d*3; });

var newX = 600;
svg.selectAll("ellipse")
      .data(dataArray)
      .enter().append("ellipse")
                .attr("class","second")
                .attr("cx",function(d,i){ newX+=(d*3)+(i*20); return newX; })
                .attr("cy","100")
                .attr("rx",function(d){ return d*3; })
                .attr("ry","30");

var newX = 900;
svg.selectAll("line")
      .data(dataArray)
      .enter().append("line")
                .attr("x1",newX)
                .attr("stroke-width","2")
                .attr("y1",function(d,i){ return 80+(i*20); })
                .attr("x2",function(d){ return newX+(d*15); })
                .attr("y2",function(d,i){ return 80+(i*20); });

var textArray = ['start','middle','end'];
svg.append("text").selectAll("tspan")
    .data(textArray)
    .enter().append("tspan")
      .attr("x",newX)
      .attr("y",function(d,i){ return 150 + (i*30); })
      .attr("fill","none")
      .attr("stroke","blue")
      .attr("stroke-width","2")
      .attr("dominant-baseline","middle")
      .attr("text-anchor","start")
      .attr("font-size","30")
      .text(function(d){ return d; });

svg.append("line")
      .attr("x1",newX)
      .attr("y1","150")
      .attr("x2",newX)
      .attr("y2","210");
```
For more information about ordinal scale, check https://goo.gl/186ghj
