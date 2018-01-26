---
layout: post
title: "A Post with Google Vis motion chart"
description: "Example for displaying a Google Vis motion chart."
tags: [graph post, images, test, R]
comments: true
---

The gvisMotionChart function reads a data.frame and creates text output referring to the Google Visualisation API, which can be included into a web page.

### Chart

#### Areas in GCS

<div id="Area Change" class="section2">
<h2><em>Area Change</em></h2>

<!-- MotionChart generated in R 3.4.0 by googleVis 0.6.2 package -->
<!-- Thu Jan 25 19:29:15 2018 -->


<!-- jsHeader -->
<script type="text/javascript">


// jsData
function gvisDataGCS () {
var data = new google.visualization.DataTable();
var datajson =
[
 [
"1. Bajo Magdalena",
2012,
27499
],
[
"1. Bajo Magdalena",
2013,
155805
],
[
"1. Bajo Magdalena",
2014,
46340
],
[
"1. Bajo Magdalena",
2015,
208456
],
[
"1. Bajo Magdalena",
2016,
72281
],
[
"1. Bajo Magdalena",
2017,
302705
],
[
"2. Valle del r�o Cesar",
2012,
18709
],
[
"2. Valle del r�o Cesar",
2013,
33208
],
[
"2. Valle del r�o Cesar",
2014,
21875
],
[
"2. Valle del r�o Cesar",
2015,
188868
],
[
"2. Valle del r�o Cesar",
2016,
388297
],
[
"2. Valle del r�o Cesar",
2017,
548764
],
[
"3. Boyac� & Santander",
2013,
106040
],
[
"3. Boyac� & Santander",
2014,
38202
],
[
"3. Boyac� & Santander",
2015,
188165
],
[
"3. Boyac� & Santander",
2016,
151921
],
[
"3. Boyac� & Santander",
2017,
350850
],
[
"4. Ecorregi�n Cafetera",
2012,
41071
],
[
"4. Ecorregi�n Cafetera",
2013,
210156
],
[
"4. Ecorregi�n Cafetera",
2014,
80398
],
[
"4. Ecorregi�n Cafetera",
2015,
184669
],
[
"4. Ecorregi�n Cafetera",
2016,
449765
],
[
"4. Ecorregi�n Cafetera",
2017,
639407
],
[
"5. Piedemonte Orinocense",
2012,
34423
],
[
"5. Piedemonte Orinocense",
2013,
119127
],
[
"5. Piedemonte Orinocense",
2014,
75839
],
[
"5. Piedemonte Orinocense",
2015,
140528
],
[
"5. Piedemonte Orinocense",
2016,
122565
],
[
"5. Piedemonte Orinocense",
2017,
308438
]
];
data.addColumn('string','Regional');
data.addColumn('number','year');
data.addColumn('number','sum_area');
data.addRows(datajson);
return(data);
}


// jsDrawChart
function drawChartGCS() {
var data = gvisDataGCS();
var options = {};
options["width"] = 600;
options["height"] = 500;
options["state"] = "\n{\"yAxisOption\":\"2\",\"sizeOption\":\"2\",\"dimensions\":{\"iconDimensions\":[\"dim0\"]}}\n";


    var chart = new google.visualization.MotionChart(
    document.getElementById('GCS')
    );
    chart.draw(data,options);


}



// jsDisplayChart
(function() {
var pkgs = window.__gvisPackages = window.__gvisPackages || [];
var callbacks = window.__gvisCallbacks = window.__gvisCallbacks || [];
var chartid = "motionchart";

// Manually see if chartid is in pkgs (not all browsers support Array.indexOf)
var i, newPackage = true;
for (i = 0; newPackage && i < pkgs.length; i++) {
if (pkgs[i] === chartid)
newPackage = false;
}
if (newPackage)
  pkgs.push(chartid);

// Add the drawChart function to the global list of callbacks
callbacks.push(drawChartGCS);
})();
function displayChartGCS() {
  var pkgs = window.__gvisPackages = window.__gvisPackages || [];
  var callbacks = window.__gvisCallbacks = window.__gvisCallbacks || [];
  window.clearTimeout(window.__gvisLoad);
  // The timeout is set to 100 because otherwise the container div we are
  // targeting might not be part of the document yet
  window.__gvisLoad = setTimeout(function() {
  var pkgCount = pkgs.length;
  google.load("visualization", "1", { packages:pkgs, callback: function() {
  if (pkgCount != pkgs.length) {
  // Race condition where another setTimeout call snuck in after us; if
  // that call added a package, we must not shift its callback
  return;
}
while (callbacks.length > 0)
callbacks.shift()();
} });
}, 100);
}


// jsFooter
</script>


<!-- jsChart -->  
<script type="text/javascript" src="https://www.google.com/jsapi?callback=displayChartGCS"></script>


<!-- divChart -->

<div id="GCS"
  style="width: 600; height: 500;">
</div>


</div>


#### Three Up

Apply the `third` class like so to display three images side by side that share the same caption.

{% highlight html %}
<figure class="third">
	<img src="/images/image-filename-1.jpg">
	<img src="/images/image-filename-2.jpg">
	<img src="/images/image-filename-3.jpg">
	<figcaption>Caption describing these three images.</figcaption>
</figure>
{% endhighlight %}
