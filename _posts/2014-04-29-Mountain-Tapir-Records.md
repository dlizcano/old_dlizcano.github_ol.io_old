---
layout: post
title: "Mountain Tapir locations in rMaps"
modified: 2014-04-29 01:24:36 -0400
category: [R]
tags: [R, map, rMaps]
image:
  feature: texture-feature-07.jpg
  credit: Andes Mountains, Peru. Diego J. Lizcano
  creditlink: http://500px.com/dlizcano
comments: true
share: true
---

## Mountain Tapir Distribution
Taking as excuse the new package [rMaps](https://github.com/ramnathv/rMaps). I wanted to plot my old points for the Mountain tapir. I hope this could serve as a starting point for a new collaborative project.

#### Why to Update the Mountain Tapir Distribution?

- The information available as map in the red list has more than 10 years old. 
  [Just take a look to the red list info for mountain tapir](http://maps.iucnredlist.org/map.html?id=21473).
  
- In the last 10 years researchers have studied mountain tapirs at new locations.
- New powerful tools to build robust distribution models have been developed. 

##### Those are some locations

<iframe src='
content/2.html
' scrolling='no' seamless class='rChart 
leaflet
 '
id=iframe-
chart1f4cd407aa8
></iframe>
<style>iframe.rChart{ width: 100%; height: 400px;}</style>

#### The code to make it

{% highlight css %}
require(rMaps)
fieldpoints_Diego<-read.csv("C:\\Users\\Diego\\Documents\\CodigoR\\Tapirus_SDM\\data\\T_pin.csv")

cord<-c(2.5,-73) #central location in the map
map3 <- Leaflet$new()
map3$setView(cord, zoom = 6)
map3$tileLayer(provider = 'MapQuestOpen.OSM')#Stamen.TonerLite
# put the info over the clicking points
for (i in 1:nrow(fieldpoints)){
  binpop<-paste("<p>",as.character(fieldpoints[i,5]),
  as.character(fieldpoints[i,6]), as.character(fieldpoints[i,9]),"</p>", sep=" " )  
map3$marker(c(fieldpoints[i,8], fieldpoints[i,7]), bindPopup = binpop)
  }
{% endhighlight %}

#### ecuacion aqui
con texto

{% m %}
E(Y|X) = G{X^T _B_}, donde:    
_B_ es el parametro a ser estimado     
G es la funcion de enlace (Bernoulli)!
{% em %} 

mas texto... y que pasa?.

{% math %}
E(Y|X) = G{X^T _B_}, donde:    
_B_ es el parametro a ser estimado     
G es la funcion de enlace (Bernoulli)!
{% endmath %}

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="http://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.