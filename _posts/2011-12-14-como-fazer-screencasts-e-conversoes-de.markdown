---
layout: post
title: Como fazer ScreenCasts e Conversões de Video e Audio
date: '2011-12-14T06:46:00.000-08:00'
description: Como fazer ScreenCasts e Conversões de Video e Audio
main-class: 'linux'
tags:
- Debian
- Linux
- Dicas
image: http://3.bp.blogspot.com/-fSeIdEUjOMU/Tui2LQ5K0OI/AAAAAAAAASI/Fgv0cEeipIU/s72-c/multimidia.gif
twitter_text: Como fazer ScreenCasts e Conversões de Video e Audio
introduction: Como fazer ScreenCasts e Conversões de Video e Audio
---
![Blog Linux](http://3.bp.blogspot.com/-fSeIdEUjOMU/Tui2LQ5K0OI/AAAAAAAAASI/Fgv0cEeipIU/s320/multimidia.gif "Blog Linux")
Para executar estas façanhas você pode utilizar alguns aplicativos, como:FFmpeg, XvidCap, Mencoder...segue:
Primeiramente você deve instalar o FFmpeg.Em distros Debian e Debian-Likes você pode instalá-lo via apt-get, com o seguinte comando:
{% highlight bash %}
# apt-get install ffmpeg
{% endhighlight %}
Para converão de video, o comando é , supondo que você vai transformar de OGV para AVI:{% highlight bash %}
ffmpeg -i entrada.ogv saida.avi
{% endhighlight %}, outra possibilidade entre várias é dividir um video em varias partes, exmplo, você quer dividir um video de 20:45s em dois videos, primeiro você cria a primeira parte do video que você quer que tenha 10:45s (10 x 60 + 45 = 645):{% highlight bash %}
ffmpeg -i video.avi -ss 00:00:00 -t 645 video_1.avi
{% endhighlight %} o corte começa do minuto, segundo e hora ZERO e corta 645 segundos e depois cria outro com 600s {% highlight bash %}
ffmpeg -i video.avi -ss 00:10:45 -t 600 video_1.avi
{% endhighlight %} onde -t é o tempo é o tempo e 0 -ss é o inicio de onde será gerado.
Para juntar arquivos você pode utilizar o mencoder:{% highlight bash %}
# apt-get install mencoder
{% endhighlight %}, e para juntar basta executar o comando, supondo que você tenha 5 partes do video:{% highlight bash %}
mencoder -ovc lavc -oac lavc 1.avi 2.avi 3.avi 4.avi 5.avi 6.avi -o all.avi
{% endhighlight %}, outra dica pra ScreenCast também sugiro o RecordMyDesktop, Gtk-RecordMyDesktop e o XvidCap, para instalá-los, respectivamente:{% highlight bash %}
# apt-get install recordmydesktop
# apt-get install gtk-recordmydesktop
# apt-get install xvidcap
{% endhighlight %}
O XvidCap é o melhor, porém ele utiliza api de audio OSS e não Alsa, tem de fazer alguns manobras pra conseguir, porém você pode iniciar o record de audio separado e depois juntar o audio e o vídeo com o PiTiVi, para criar uma gravação de audio, execute o comando pelo terminal:{% highlight bash %}
# rec audio.wav
{% endhighlight %}.
Para especificar a resolução do video, você pode usar o comando:
{% highlight bash %}
ffmpeg -i entrada.ogv -s 1360×768 saida.flv
{% endhighlight %} , Onde o -s é a resolução do video que será 1360x768.
É possível também fazer screencast (Captura da Tela) com o FFmpeg, o comando é:{% highlight bash %}
ffmpeg -f x11grab -s 1360x768 -r 30 -i :0.0 video_capturado.mpeg
{% endhighlight %}
Você pode converter muitos formatos tais como: flv, mp4, mpeg, ogv, yum, ...entre outros.
Para conversão de audio, por exemplo, de FLV para MP3, o comando é:
{% highlight bash %}
ffmpeg -i video.flv -ab 128k -ac 2 -ar 44100 musica.mp3
{% endhighlight %}, os itens -ab é a taxa de transferência(o bitrate), -ac é a qualidade de canais, e o -ar é a frequência.
é com essa ferramentas que eu gravo video-aulas entre outras coisas!
