---
layout: post
title: Loop for na Linguagem C
date: '2012-11-24T15:08:00.000-08:00'
description: Loop for na Linguagem C
main-class: 'misc'
tags:
- Linguagem C
image: http://2.bp.blogspot.com/-V2_oKxC7bXM/ULFS0lcQi5I/AAAAAAAAAs8/hI89zy28Qq0/s72-c/for_c.png
twitter_text: Loop for na Linguagem C
introduction: Loop for na Linguagem C
---
![Blog Linux](http://2.bp.blogspot.com/-V2_oKxC7bXM/ULFS0lcQi5I/AAAAAAAAAs8/hI89zy28Qq0/s320/for_c.png "Blog Linux")
Fazer coisas básicas e aos poucos fazem memorização e fixação melhor dos detalhes.Estou postando 2 exemplos de loop simples em for, ambos no mesmo código, como compilar, está nos posts antigos sobre C, é só pesquisar, breve passarei teorias sobre tudo. PasteBin: 
<iframe src="http://pastebin.com/raw/M034W8X7" style="border:none;width:100%;"><iframe> CódigoNoBlog: 
{% highlight bash %}
#include 
int main(){
 int i;
 char letra;
 for(i=1;i<=10;i++)
  printf("%d\n",i);
 
 for(letra = 'A';letra<='Z';letra++)
  printf("%c\n",letra);
  return(0);
}
{% endhighlight %}
