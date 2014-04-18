---
layout: post
title: "1st post in github"
modified: 2014-04-16 01:24:36 -0400
category: [post, blog]
tags: [post,blog]
image:
  feature: texture-feature-06.jpg
  credit: Palm leave, Ecuador. Diego J. Lizcano
  creditlink: http://500px.com/dlizcano
comments: true
share: true
---

## New in github
Inspired by several pages hosted by github and amaze by the fantastic possibilities such as [this map](http://finiterank.github.io/homicidios/). I decided to create my website and to migrate my [old blog "Tapir ologist"](http://tapirologist.wordpress.com/) to github.
 
### How I did it using Windows7
The process is simple but you can encounter some minor problems easy to fix. I began following carefully the instructions provided by [github](https://pages.github.com/). However when I pass to the second step the first problem arose, the installation of ruby in Windows. To solve this, github suggest to follow [Madhur Ahuja instructions](http://www.madhur.co.in/blog/2011/09/01/runningjekyllwindows.html). Madhur made the process simpler by putting all in a [portable version](http://www.madhur.co.in/blog/2013/07/20/buildportablejekyll.html).
Once I finish the Ruby and gems installation and after making my first hello world website using Jekill, I did not like the default templates provided by Jekill. Looking around I found [Minimal Mistakes](http://jekyllthemes.org/themes/minimal-mistakes/), but after trying to install it, my first error arouse in Jekill. Minimal Mistakes use a different version of rake. The solution was to install each gem manually using this in the command prompt:    

{% highlight css %}
gem install gem.name --version "~> x.xx.x.xxx"
{% endhighlight %}

Just change gem.name for the name of your conflicting gem and put the version number instead x.xx.x.xxx. Then run Rake, if does not work, install the next conflicting gem and so on. After several hours all including Rake worked fine.

### If I were to do it again
Reading [Fellgermon Bit](http://lcolladotor.github.io/2013/11/09/new-Fellgernon-Bit-setup-in-Github/#.U1DKBfldWSr) a simpler method is just fork the blog of another person you liked and customize things like pictures and posts. Of course Jekyll gives more control over your site, but you can spend a lot of time make sure that Ruby and gems work. 
