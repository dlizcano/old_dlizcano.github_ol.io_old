---
layout: post
title: "Mammal Biodiversity in Colombia."
modified: 2014-05-17 10:24:36 -0400
category:
tags: [R, map, Colombia]
image:
  feature: texture-feature-13.jpg
  credit: Trees in the winter, Silver Spring, MD, USA. Diego J. Lizcano
  creditlink: http://500px.com/dlizcano
comments: true
share: true
---

## Mammal Collection in Colombia

Disappointed because the last [report of Colombia to the CBD](http://www.pnud.org.co/sitio.shtml?apc=i1-----&x=75608#.U3b1wfldV8G) do not mention how much Colombia knows his biodiversity, I decided to carry a basic analysis using mammals. 

In my opinion the report is only focus on ecosystem services and the only mention of biodiversity loss is trough deforestation. But my opinion on this matter will be subject of another post.

## My Example

First I downloaded the mammal data set for Colombia from [GBIF, which you can get here](http://www.gbif.org/occurrence/search?TAXON_KEY=359&COUNTRY=CO).
The data set has 85318 records (probably more if you get it after May 2014). From those records I used the one with hard evidence: the ones having preserved specimens, eliminating 1893 observations, mainly from [iNaturalist](http://www.inaturalist.org/) and [Corantioquia.](http://www.corantioquia.gov.co/) 

Some georeferences were really bad made, plotting points outside Colombia. So I decided to overwrite those coordinates using NAs. At the end I used just 36031 georeferenced points to make the maps, less than half the total data set.  It is sad to discover that more than half the records are bad or not georeferenced.

Before making the maps I fixed some issues, such as empty records and the 8200001422-01 problem, which you can see in detail at [the end of this post.](http://tapirologist.wordpress.com/2013/06/04/biodiversity-by-colombian-institutions/)


#### Plotting all the records the map looks like this. 
<figure>
	<img src="/images/mammal/map1.png">
</figure>

{% highlight css %}
require(ggmap)
require(raster)
require(plyr)

bigtable<-read.csv(file="data\\GBIF\\occurrenceGBIF_Mammal.txt",header=T,sep="\t")
bigtable<-subset(bigtable, basisofrecord=="PRESERVED_SPECIMEN") #only collected specimens

#get poligon of Colombia
co<-getData("GADM",country="CO",level=1,download=TRUE)
col_depto <- fortify(co,region="NAME_1") # make compatible to ggplot2

mapbase<- ggplot(col_depto, aes(long,lat,group=group)) + 
	geom_polygon(fill="grey60") + coord_equal() +
	geom_path(color="grey") 

delet<-which(x=bigtable$decimallongitude> -66) #select points way outside Colombia
bigtable[delet,79]<-NA
bigtable[delet,80]<-NA
delet<-which(x=bigtable$decimallongitude< -83) #select points way outside Colombia
bigtable[delet,79]<-NA
bigtable[delet,80]<-NA
delet<-which(x=bigtable$decimallatitude> 14) #select points way outside Colombia
bigtable[delet,79]<-NA
bigtable[delet,80]<-NA
delet<-which(x=bigtable$decimallatitude< -5) #select points way outside Colombia
bigtable[delet,79]<-NA
bigtable[delet,80]<-NA

# fix empty values in publishingcountry <- empty are colombian
nada<-which(x=bigtable$publishingcountry=="")
bigtable$publishingcountry[nada]<-"CO"
# make a new category Colombia Vs International
CO<-which(x=bigtable$publishingcountry=="CO")
bigtable$int_col<-"International"
bigtable$int_col[CO]<-"Colombia"
# fix 8200001422-01 problem. See http://tapirologist.wordpress.com/2013/06/04/biodiversity-by-colombian-institutions/
Iavh<-which(x=bigtable$institutioncode=="8200001422-01") 
bigtable$institutioncode[Iavh]<-"IAvH"

## mapping all points
map1<-mapbase + geom_point(aes(x = decimallongitude, y = decimallatitude, group = TRUE,
                      colour = "red"), 
                      data = bigtable, size = 3,
                      alpha = 1/15) +
                      theme_bw() + guides(guide_legend((title = NULL)))+
                      theme(legend.position = "none")

  {% endhighlight %}

#### Putting colors to each collection, the map looks like this.  
<figure>
	<a href="/images/mammal/map2.png"><img src="/images/mammal/map2.png"></a>
</figure>
    
{% highlight css %}
### mapping all mammal collections for Col in GBIF
map2<-mapbase + geom_point(aes(x = decimallongitude, y = decimallatitude, group = TRUE,
                          colour = factor(institutioncode)), 
                          data = bigtable, size = 2,
                          alpha=2/3) + 
  theme(legend.position = "right") + 
  theme_bw() + guides(col = guide_legend(ncol = 2)) +
  theme(legend.key = element_rect(colour = "white"))
  {% endhighlight %}

There are some problems. ROM is duplicated and there are weird institutions such as “O”. But overall you can see that the only Colombian institutions reporting mammals to GBIF are ICN and IAvH. It is sad that other large mammal collections such as Universidad del Valle, Universidad de Antioquia, Universidad del Cauca, and Universidad Javeriana are not reporting data to GBIF.
  
####Putting a hexagons of approx 100 km around each collection point we can discover places undersampled in the mammal collections.
<figure>
	<a href="/images/mammal/map3.png"><img src="/images/mammal/map3.png"></a>
</figure>

{% highlight css %}
#### Were have been collected the mammals 
map3<-mapbase + geom_point(aes(x = decimallongitude, y = decimallatitude, group = FALSE), size=1, 
                           data = bigtable,alpha=I(0.25),colour="red") + 
  stat_binhex(aes(x = decimallongitude, y = decimallatitude, group = FALSE),
              size = .5, binwidth = c(.5,.5), alpha = 2/4,data = bigtable) + theme_bw() 
  {% endhighlight %}

#### What about if we compare the Colombian institutions against the others?	              
<figure>
	<a href="/images/mammal/map4.png"><img src="/images/mammal/map4.png"></a>
</figure>

Well the mammals collected by Colombian institutions are less.

{% highlight css %}
##### Colombia vs International
map4<-mapbase + geom_point(aes(x = decimallongitude, y = decimallatitude, group = FALSE), size=2, 
                           data = bigtable,alpha=I(0.5),colour="red") + 
                          facet_wrap(~ int_col) + theme_bw()
  {% endhighlight %}

####Which are those countries and what kind of mammals do they have? 
<figure>
	<a href="/images/mammal/bar1.png"><img src="/images/mammal/bar1.png"></a>
</figure>
Bats are the most popular guys in collections. 

{% highlight css %}
### bar graph by country and order collected
barcountry<-ggplot(bigtable, aes(publishingcountry, fill=order_)) + 
  geom_bar() + scale_colour_brewer(palette="Set1")
  {% endhighlight %}
  
####What about if we ask for the years of collection?
<figure>
	<a href="/images/mammal/mammalpeak.png"><img src="/images/mammal/mammalpeak.png"></a>
</figure>
The records start before 1800 and end in 2006. Interesting: There is a big peak in 1967

Unfortunately that huge collection peak do not feed a Colombian institution. 
<figure>
	<a href="/images/mammal/bar2.png"><img src="/images/mammal/bar2.png"></a>
</figure>
The big contribution from 1967 was for the USNM and the ROM 
<figure>
	<a href="/images/mammal/all_yr.jpg"><img src="/images/mammal/all_yr.jpg"></a>
</figure>
The doted red line in red is the year 1967
{% highlight css %}
count<-ddply(bigtable,.(year), summarize, freq=length(year))
count<-count[2:120,] # ignore 1700

colectime<-ggplot(count, aes(x=year, y=freq, xmin=1800)) + geom_line() + 
  geom_vline(xintercept = 1967,colour="red", linetype = "longdash") + 
  annotate("text", label = "1967", x = 1967, y = 6900, size = 5, colour = "blue")

ggplot(bigtable, aes(year)) + geom_bar() + xlim(1850, 2006) + 
  geom_vline(xintercept = 1967,colour="red", linetype = "longdash")+
  facet_wrap(~ int_col)

ggplot(bigtable, aes(year)) + geom_bar() + xlim(1850, 2006) + 
  geom_vline(xintercept = 1967,colour="red", linetype = "longdash")+
  facet_wrap(~ institutioncode,nrow = 10)
  {% endhighlight %}

#### So what happened in 1967?

#### Why the last records for ICN and IAvH is 2006? 

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="http://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.