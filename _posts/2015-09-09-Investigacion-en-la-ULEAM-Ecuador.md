---
layout: post
title: "Visibilidad de la Investigación en la ULEAM, Ecuador"
modified: 2015-09-11 08:15:36 -0500
category:
tags: [Ecuador, ciencia, R, text-mining, investigación]
image:
  feature: texture-feature-23.jpg
  credit: Nevado el Altar, Ecuador. Diego J. Lizcano
  creditlink: http://500px.com/dlizcano
comments: true
share: true
---


##Mi tiempo en Ecuador

Pronto voy a completar un año de haber llegado a Manta, Ecuador, a trabajar como investigador del DCI-ULEAM [http://www.uleam.edu.ec/](http://www.uleam.edu.ec/) en el proyecto Fauna de Manabí [http://faunamanabi.github.io/](http://faunamanabi.github.io/). 
La verdad debo confesar que Ecuador no me ha tratado mal, a pesar de la perdida de Kaila y algunas dificultades al tratar de adaptarme a las particularidades culturales del Ecuador. Aunque ya lo logré, la adaptación no fue fácil, muy a pesar de ser de Colombiano, y tal vez por el formato cuadriculado que he adquirido en Inglaterra, el país donde estudie, y por haber vivido en USA antes de venir acá. 

##Que tan buena y visible es la investigación que se hace en la ULEAM?

Luego de algún tiempo en esta institución he conocido el gran esfuerzo que ha hecho esta universidad para atraer investigadores y [prometeos](http://prometeo.educacionsuperior.gob.ec/). También hemos recibido algunas explicaciones, sobre cual es la investigación que le da puntos a la ULEAM frente a [CEAACES](http://www.ceaaces.gob.ec/), pero la verdad es que aun no entiendo....?

Lo que he querido hacer con este "post" de mi blog, es tratar de entender mejor el impacto de la investigación que realiza la ULEAM.  

###Como ven a la ULEAM desde lejos?

Lo que planteo es bien sencillo. Imaginen a alguien en Corea del Sur, Bélgica, Estados Unidos, o en cualquier otro lugar y que quiera buscar lo que hace la ULEAM en términos de investigación científica. Esta persona muy probablemente 
recurrirá a una base de datos especializada como [Web of Sciences de Thomson Reuters](http://wokinfo.com/) o [Scopus de Elsevier](http://www.elsevier.com/solutions/scopus).  Bueno, pues he hecho la tarea buscando en esas bases de datos y arbitrariamente comparo a la ULEAM con otras instituciones de Ecuador. 

###Una buena evaluación es comparativa

Las instituciones que he seleccionado para comparar a la ULEAM son:

- [Universidad San Francisco de Quito, (USFQ)](http://www.usfq.edu.ec/Paginas/Inicio.aspx). Una universidad privada y "Top" que hace muy buena investigación. A pesar de tener san en el nombre, no es religiosa.
- [Pontificia Universidad Católica del Ecuador (PUCE)](http://www.puce.edu.ec/). Una universidad de carácter religioso, con unas colecciones increíbles de la Biodiversidad del Ecuador.
- [Universidad Central del Ecuador (UCE)](http://www.uce.edu.ec/). Es una universidad publica, la más antigua del Ecuador y tal vez la más grande en numero de estudiantes. 
- [Universidad Técnica Particular del Loja (UTPL)](http://www.utpl.edu.ec/). Una universidad de origen religioso, que también hace muy buena investigación en Biodiversidad y bosques.  
- [Universidad Técnica de Manabí (UTM)](http://www.utm.edu.ec/). Una universidad publica de carácter regional, de características similares a la ULEAM y ubicada en la capital de Manabí. 

La comparación es muy sencilla y tal vez bastante "naive", pero muy diciente. Se trata de tener en cuenta el numero de artículos publicados entre el 2000 y 2015.
Algo que hay que aclarar es que no estoy teniendo en cuenta, el numero de citaciones de los artículos, ni el conteo de cuantiles en su factor de impacto. La búsqueda en las dos bases de datos la realice usando la institución de afiliación del autor con el nombre completo y su sigla como variación.

####Aquí el Resultado
Publicaciones de autores con filiación a una universidad
<figure>
	<a href="/images/uleam/juntas.png"><img src="/images/uleam/juntas.png"></a>
</figure>

#####Es muy claro que:
Hay muchísimas más publicaciones registradas en la base de datos de Scopus que en ISI Web of Science. Y muy seguramente tiene que ver con que Scopus indexa también 
memorias de congresos y publicaciones de Elsevier, su propia casa comercial, los cuales no tienen [factor de impacto](https://es.wikipedia.org/wiki/Factor_de_impacto). Del factor de impacto se encarga el Instituto para la Información Científica (Institute for Scientific Information (ISI) de Thomson Reuters, usando una metodología clara y bien establecida.

En pocas palabras las publicaciones registradas en ISI Web of Science tienen más alto perfil en términos científicos. 

####Y que tal si graficamos solo ISI Web of Science?
<figure>
	<a href="/images/uleam/isi.png"><img src="/images/uleam/isi.png"></a>
</figure>

######Se aprecia que:
La Universidad Católica es la dominante en publicaciones de alto factor de impacto, seguida por la San Francisco de Quito. Lo interesante acá es ver como la ULEAM tiene más publicaciones, con alto factor de impacto, que la 
Universidad Central del Ecuador, Incluso y aunque parezca sorprendente, más que la Universidad Técnica Particular de Loja y de lejos la ULEAM supera a la Universidad Técnica de Manabí (UTM).  

###Calidad Vs. Cantidad?
Indudablemente el numero de investigadores, tal como lo refleja el ranking de [Researchgate](https://www.researchgate.net/institutions/Ecuador?order=rgScore&method=total) tiene un gran peso en el ranking de las instituciones de investigación en Ecuador. Pero también debemos preguntarnos que estrategia adoptar? Muchos artículos sin o con poco factor de impacto? o pocos con alto impacto? En este sentido hay que resaltar la alta Calidad de lo poco que se publica en la ULEAM. 

En Conclusión las publicaciones de la ULEAM son de buena calidad!. Yo simplemente me pregunto; que pasaría si los investigadores de la ULEAM tuvieran más apoyo?

###Y que se publica en la ULEAM
Bueno he bajado de ISI Web of Science las citas de los artículos, y he hecho una nube de palabras con las más frecuentes en los resúmenes de los artículos.

###Y la infaltable nube de palabras.
<figure>
  <a href="/images/uleam/wordcloud.png"><img src="/images/uleam/wordcloud.png"></a>
</figure>


Acá Ud. puede acceder a las publicaciones de la ULEAM como un archivo para [Mendeley](/content/uleam.ris) [.bib](/content/uleam.bib) o [EndNote](/content/uleam_endnote.xml) y este es el código para que lo pueda replicar:


{% highlight css %}
library(plyr)
library(RefManageR)
library(tm)

referencias<-ReadBib("uleam.bib")

# build a corpus from a list
myCorpus <- Corpus(VectorSource(referencias$abstract))

# remove punctuation & numbers
myCorpus <- tm_map(myCorpus, removePunctuation)
myCorpus <- tm_map(myCorpus, removeNumbers)
# remove URLs
removeURL <- function(x) gsub("http[[:alnum:]]*", "", x)
myCorpus <- tm_map(myCorpus, removeURL)

# remove  from stopwords
myStopwords <- setdiff(stopwords("english"))
# remove stopwords
myCorpus <- tm_map(myCorpus, removeWords, myStopwords)
# remove more words 
myCorpus<- tm_map(myCorpus, removeWords, c("however","  the","the"," The"))
corpus_clean <- tm_map(myCorpus, PlainTextDocument)
myTdm <- TermDocumentMatrix(corpus_clean)

# Word Cloud
library(wordcloud)
library(RColorBrewer)
pal2 <- brewer.pal(8,"Dark2")

m <- as.matrix(myTdm)
freq <- sort(rowSums(m), decreasing=T)

png("wordcloud.png", width=1000,height=800)
wordcloud(words=names(freq), freq=freq, scale=c(8,.5), min.freq=2, 
          max.words=Inf, random.order=F, rot.per=.15, colors=pal2)
dev.off()
{% endhighlight %}

Luego de revisar una por una las referencias de las publicaciones ULEAM debo decir que: Oye [Cristiano](https://www.researchgate.net/profile/Cristiano_Araujo2) deja de publicar tanto!!! jejejee...


<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="http://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.