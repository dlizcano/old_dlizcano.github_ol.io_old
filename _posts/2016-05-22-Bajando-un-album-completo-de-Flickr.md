---
layout: post
title: "Recuperando fotos y datos de Flickr con R"
modified: 2016-05-23 01:10:36 -0500
category:
tags: [Camera trap, ciencia, R, backup, trampas cámara]
image:
  feature: texture-feature-22.jpg
  credit: Cuniculus paca, Machalilla, Ecuador. Fauna de Manabi
  creditlink: https://faunamanabi.github.io/
comments: true
share: true
---

## Bajando un album completo y los datos exif de Flickr.com.


Luego de mas de un año en Manta, Ecuador, trabajando como investigador del [DCI-ULEAM](http://www.uleam.edu.ec/] en el proyecto (Fauna de Manabí)[http://faunamanabi.github.io/). Hemos usado [Flickr.com](http://www.flickr.com) como un backup para almacenar las fotos del Proyecto. Flickr ofrece algunas ventajas como; una interfaz sencilla, un Terabyte de almacenamiento gratis, geotags y fácil organización de las fotos en albums y colecciones. Hasta el momento son cerca de 15 mil fotos las que hemos obtenido con el proyecto y muchas de ellas están en Flickr y en los computadores de la ULEAM. Sin embargo, el reciente [terremoto de Ecuador](https://en.wikipedia.org/wiki/2016_Ecuador_earthquake) nos hizo pensar que existía una gran posibilidad de perder los datos del proyecto, los cuales se encuentran almacenados en los discos duros de algunos computadores de la ULEAM.


Si bien se puede descargar un álbum completo (set, en el lenguaje de Flickr) con un simple click en la flecha de cada álbum.

<figure>
  <a href="/images/flickr/flickr_album.jpg"><img src="/images/flickr/flickr_album.jpg"></a>
</figure>

En algún momento pensé que eran muchos clicks, y que sería chévere y divertido poder bajar los albums usando R. Adicionalmente por me preguntaba si al bajar las fotos podría recuperar los datos exif de cada foto.  Luego de algunas horas buscando como podría ser me encontré con  las fantásticas y simples instrucciones de web scraping elaboradas por [Rolf Fredheim](http://blog.rolffredheim.com/2014/03/web-scraping-scaling-up-digital-data.html).


Definitivamente lo mejor es trabajar con los API de  Flickr.com. Para lo cual se debe establecer obtener una key para usarlos, [acá pueden encontrar más detalles](http://www.flickr.com/services/api/misc.api_keys.html).


Al final, termine invirtiendo más horas en entender la estructura de los API de Flickr y desarrollando el código que baja los datos exif y los álbums, que si hubiera bajado cada álbum haciendo click en la flechita de cada uno.

Espero esto le sirva a alguien.

### Acá el código para obtener los datos exif a partir del nombre de cada foto.

{% highlight css %}
#' flickrlist - Get metadata  for items available on Flickr for the picture
#' @import XML
#' @import RCurl
#' @param apikey - API key provided by Flickr.com website. (Refer http://www.flickr.com/services/api/misc.api_keys.html for more details.)
#' @param picture_id - Test to search: ="17966004672"
#' @examples \dontrun{
#'  get_exif("apikey","17966004672")
#' }
#' @export
get_exif <- function (apikey="NA", picture_id=NA){
  if(is.na(apikey)){
    print("Need to supply API key for Flicker.com website. \n Get yours at http://www.flickr.com/services/api/misc.api_keys.html")
    return(NULL)
  }
  if(is.na(picture_id)){
    print("Need to supply a picture id.")
    return(NULL)
  }

  url<-paste("https://api.flickr.com/services/rest/?method=flickr.photos.getExif&api_key=",apikey,
             "&photo_id=",as.character(picture_id),
             "&format=json",
             "&nojsoncallback=1", sep="")
             # "&api_sig=a29a2542460ecb9684f045acab7567e7", sep="")

  #x <- getURL(url, ssl.verifypeer = FALSE)
  #data <- fromJSON(x, unexpected.escape="keep", method="R")
  tag<-vector()
  info<-data.frame()
  lookUp <- URLencode(url)
  rd <- readLines(lookUp, warn="F")
  dat <- fromJSON(rd)
  for (i in 1: length(dat$photo$exif)){
    tag[i]<-dat$photo$exif[[i]]$tag
    info[1,i]<-dat$photo$exif[[i]]$raw$`_content`
  }  
  colnames(info)<-tag
  return(info)
}
{% endhighlight %}


### Y ahora el código para bajar el álbum completo (set) y obtener los datos de cada foto en una tabla.

{% highlight css %}
#-------------------------------------------------------------------
# Downloading all Flickr pictures for a set (album)
# Diego J. Lizcano, 2016
# Modified from: Benedikt Koehler, 2013
# @furukama
#-------------------------------------------------------------------

library(RCurl)
library(rjson)
library(dplyr)

source(get_exif.R)

# Login credentials
apikey <- "your_API" # API key for Flickr API goes here
oauth_token <- "" # get yours

# Search data: by folder = set

workdir <- "C:/Temp/"
set     <- "72157649989156607" # picture folders
user_id <- "128565749%40N04"# equivale a: "128565749@N04"

flickr_get_pic <- function (workdir="NA",set="NA",user_id="NA",apikey="your_API"){
  if(is.na(apikey)){
    print("Need to supply API key for Flicker.com website. \n Get yours at http://www.flickr.com/services/api/misc.api_keys.html")
    return(NULL)
  }
  if(is.na(set)){
    print("Need to supply a folder (set).")
    return(NULL)
  }

# Search parameters
n <- 500 # pictures per page
p <- 10 # Number of pages

# Downloading the images
for (i in 1:p) {

    api <- paste("https://api.flickr.com/services/rest/?method=flickr.photosets.getPhotos",
                 "&api_key=5d6a54435c9da0c091e94cf2232e94eb",
                 "&photoset_id=", set,
                 "&user_id=128565749%40N04",
                 "&extras=url_o",
                 "&per_page=", n,
                 "&page=", i,
                 "&format=rest", sep="")

  raw_data <- getURL(api)
  test<-xmlToList(raw_data)
  fields<-names(test$photoset[[1]])
  album<-test$photoset$.attrs[10]
  data_table<- data.frame() # empty table
  url_link<- vector()
  for (j in 2:length(test$photoset)-1){
    for( z in 1: length(test$photoset$photo)){
      data_table [j,z] <- test$photoset[[j]][[z]]
    }
    url_link[j] <- paste("https://www.flickr.com/photos/",user_id,"/",data_table[j,1],"/in/album-",set,sep="")
  }
  colnames(data_table)<-fields
  data_table<-cbind(data_table,url_link) # Table to store info
  # data <- fromJSON(raw_data, unexpected.escape="keep", method="R")
  imgdir <- paste(workdir, test$photoset$.attrs[10], sep="")
  dir.create(imgdir)

  exiflist<-list()
  for (u in 2:length(test$photoset)-1) {
    # id <- data$photos$photo[[u]]$id
    # farm <- data$photos$photo[[u]]$farm
    # server <- data$photos$photo[[u]]$server
    # secret <- data$photos$photo[[u]]$secret
    # url_donload<-paste("https://c2.staticflickr.com/",farm,"/",server,"/",id,"_",secret,"_o.jpg",sep="")
    url_download<- gsub("farm6.staticflickr.com/", paste("c2.staticflickr.com/", data_table$farm[u],"/", sep=""), data_table$url_o[u]) # trick to get full resol
    temp <- paste(imgdir, "/", data_table$title[u], ".jpg", sep="")
    exiflist[[u]]<-get_exif(apikey = apikey, picture_id=data_table$id[u]) # get exif info  
    download.file(url_download, temp, method="internal", mode="wb")
  }
  a<-ldply(exiflist)
ouput<-cbind(data_table,a)
return(ouput)

}
}
{% endhighlight %}


<p>
<p></p>
</p>

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="http://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.
