---
layout: post
title: "Visibilidad de la Investigación en la ULEAM, Ecuador"
modified: 2015-09-09 11:15:36 -0500
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
La verdad debo confesar que Ecuador no me ha tratado mal, a pesar de algunas dificultades iniciales al tratar de adaptarme a las particularidades culturales del Ecuador. Muy a pesar de ser de Colombia, el país vecino, y tal vez por el formato cuadriculado que he adquirido en Inglaterra, el país donde estudie, y por haber vivido en USA antes de venir acá. 

##Que tan buena y visible es la investigación que se hace en la ULEAM?

Luego de algún tiempo en esta institución hemos recibido algunas explicaciones, sobre cual es la investigación que le da puntos a la ULEAM frente a [CEAACES](http://www.ceaaces.gob.ec/), que la verdad aun no entiendo.... Lo que he querido hacer con este "post" de mi blog, es tratar de entender mejor el impacto de la investigación que se realiza la ULEAM.  

###Como ven a la ULEAM desde lejos?

Lo que planteo es bien sencillo. Imaginen a alguien en Corea del Sur, Bélgica, Estados Unidos, o en cualquier otro lugar y que quiera buscar lo que hace la ULEAM en términos de investigación científica. Esta persona muy probablemente 
recurrirá a una base de datos especializada como [Web of Sciences de Thomson Reuters](http://wokinfo.com/) o [Scopus de Elsevier](http://www.elsevier.com/solutions/scopus).  Bueno pues he hecho la tarea buscando en esas bases de datos y arbitrariamente comparo a la ULEAM con otras instituciones de Ecuador. 

###Una buena evaluación es comparativa

Las instituciones que he seleccionado para comparar a la ULEAM son:

- [Universidad San Francisco de Quito, (USFQ)](http://www.usfq.edu.ec/Paginas/Inicio.aspx). Una universidad privada y "Top" que hace muy buena investigación. A pesar de tener san en el nombre, no es religiosa.
- [Pontificia Universidad Católica del Ecuador (PUCE)](http://www.puce.edu.ec/). Una universidad de carácter religioso, con unas colecciones increíbles de la Biodiversidad del Ecuador.
- [Universidad Central del Ecuador (UCE)](http://www.uce.edu.ec/). Es una universidad publica, la mas antigua del Ecuador y tal vez la mas grande en numero de estudiantes. 
- [Universidad Técnica Particular del Loja (UTPL)](http://www.utpl.edu.ec/]. Una universidad de origen religioso que también hace muy buena investigación en Biodiversidad.  
- [Universidad Técnica de Manabí (UTM)](http://www.utm.edu.ec/). Una universidad publica de carácter regional, de características similares a la ULEAM y ubicada en la capital de Manabí. 

La comparación es muy sencilla y tal vez bastante "naive", pero muy diciente. Se trata de tener en cuenta el numero de artículos publicados entre el 2000 y 2015.
Algo que hay que aclarar es que no estoy teniendo en cuenta, ni el factor de impacto, ni el numero de citaciones. La búsqueda en las dos bases de datos la realice usando la institución de afiliación del autor con el nombre completo y su sigla como variación. Aquí están los resultados:

 




Con este "post" de mi blog, quiero usar las técnicas de minería de texto y el mismo clasificador de sentimientos de una [entrada anterior](http://dlizcano.github.io/2014/04/30/Opinion-Mining-Tweets.html), para analizar un discurso de René Ramírez, el actual Secretario de Educación Superior Ciencia, Tecnología e Innovación [SENECYT](http://www.educacionsuperior.gob.ec/) y Presidente del Consejo de Educación Superior [CES](http://www.ces.gob.ec/). Quien por cierto es bien activo en twitter, como [@compaiRENE](https://twitter.com/compaiRENE). El discurso se titula: La disputa política por el sentido del (bio)conocimiento y lo baje de su lo baje de su [página web](http://reneramirez.ec/ponencia-la-disputa-politica-por-el-sentido-del-bioconocimiento/). 

Si bien el análisis de sentimientos es bien controversial por su aparente falta de rigor estadístico y por la debilidad de su clasificador bayesiano "naive", que por cierto, ahora me entero, se traduce como ingenuo, me gusta esa traducción!. El hecho es que a pesar de ser polémico, este tipo de análisis, es una poderosa herramienta de análisis de mercados, usada por muchas empresas de minería de datos. 

El discurso fue analizado dividiendolo en 3 partes, por párrafos. A cada párrafo se le asignó un puntaje dependiendo de las palabras usadas y que tan positiva o negativa es la palabra, comparada con un corpus hibrido en español que he ido construyendo, el cual ya tiene mas de 3300 palabras. Y aquí el resultado:

### Polaridad
<figure>
  <a href="/images/Ecuador/opinion_score.png"><img src="/images/Ecuador/opinion_score.png"></a>
</figure>

Curiosamente a medida que avanza el discurso, este se hace más positivo (score mayor que cero).

### Análisis de sentimientos
<figure>
  <a href="/images/Ecuador/emotion_analysis.png"><img src="/images/Ecuador/emotion_analysis.png"></a>
</figure>

Sorprendentemente muchos de sus párrafos son alegres, seguido por algunos tristes. Pero en términos generales el discurso es positivo, neutral,  tal como se ve en la próxima gráfica. 

###Polaridad global
<figure>
  <a href="/images/Ecuador/polarity_clasification.png"><img src="/images/Ecuador/polarity_clasification.png"></a>
</figure>

###Y la infaltable nube de palabras.
<figure>
  <a href="/images/Ecuador/nubedepalabras.png"><img src="/images/Ecuador/nubedepalabras.png"></a>
</figure>

Si desea repetir este análisis o hacer uno parecido. Acá está el código de R:

{% highlight css %}
Discurso <- read.csv("discurso_rene.txt", header = F)
Discurso$text<-as.factor(Discurso$V1)
Discurso.scores = score.sentiment(Discurso$text, pos.words,neg.words, .progress='text')
hist(Discurso.scores$score)



###### wordcloud
mh370_corpus = Corpus(VectorSource(Discurso.scores$text))

tdm = TermDocumentMatrix(
  mh370_corpus,
  control = list(
    removePunctuation = TRUE,
    stopwords = c("prayformh370", "prayformh", stopwords("spanish")),
    removeNumbers = TRUE, tolower = TRUE)
    )

m = as.matrix(tdm)
# get word counts in decreasing order
word_freqs = sort(rowSums(m), decreasing = TRUE) 
# create a data frame with words and their frequencies
dm = data.frame(word = names(word_freqs), freq = word_freqs)

wordcloud(dm$word, dm$freq, random.order = FALSE, colors = brewer.pal(10, "Dark2"))
{% endhighlight %}


<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="http://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.