---
layout: post
title: Como criar pacotes .deb
date: '2014-12-07T05:46:00.000-08:00'
description: Como criar pacotes .deb
main-class: 'linux'
tags:
- Debian
- Linux
- Dicas
- Debian-Likes
- GNU
image: http://3.bp.blogspot.com/-EF6g9Sz9ERg/VIRZ_8lbAVI/AAAAAAAABCw/-TBNqobvzlY/s72-c/pacotes%2B%2B.deb%2B.png
twitter_text: Como criar pacotes .deb
introduction: Como criar pacotes .deb
---
![Blog Linux](http://3.bp.blogspot.com/-EF6g9Sz9ERg/VIRZ_8lbAVI/AAAAAAAABCw/-TBNqobvzlY/s320/pacotes%2B%2B.deb%2B.png "Blog Linux")
Pacotes .deb facilitam a instalação de aplicativos, aqui no blog tem dois Posts que demonstram como Instalar o Eclipse Galileo e o Aptana, mas tudo poderia ser desprezado, se eu criasse um pacote .deb e disponibilizasse pra download, mas essa não é a idéia desse blog.Os pacotes podem ser instalado em Debian e Debian-Likes (Ubuntu, Linux Mint, Backtrack 5...), que possuem o mesmo caminho para os diretórios dos aplicativos.
è bem simples criar pacotes .deb, vamos começar então:
1 - crie uma pasta onde você desejar que será o nome do programa, aqui eu vou criar a pasta terminalroot dentro da minha Área de Trabalho.
2 - Depois crie dentro da sua pasta uma pasta em maiúsculo, com o nome DEBIAN
3 - dentro da pasta DEBIAN, crie um arquivo com o nome de control, sem extensão.
4 - abra esse arquivo com seu editr de texto(gEdit por exemplo) e preencha com o código abaixo:
{% highlight bash %}
Package:terminalroot
Version: 0.1
Priority:
Architecture: all
Essential:
Depends:
Pre-depends:
Suggests:
Installed-Size:
Maintainer: Marcos da B. M. Oliveira
Conflicts:
Replaces:
Provides:
Description: Acesse o blog terminalroot.com.br e descubra como criar pacotes .deb
{% endhighlight %}
No código acima, as linhas que são obrigatórias, estão preenchidas, porém algumas linhas como Depends, por exemplo, se você preencher com pacotes necessários para o funcionamento do programa o GDebi-Installer não instalará se não houver estas dependências, porém com os campos acima mencionados já podemos instalar o programa pelo GDebi-Installer.
Agora vamos ao programa, se você baixou um tarball e não quer perder o tempo copiando arquivos pra dentro de pastas...você pode criar um .deb e executá-lo, ou até mesmo, dar dois cliques nele, se o pacote foi criado com o root, ele pedirá a senha de root.
5 - Dentro da pasta que você criou que aqui estou chamando de terminalroot, crie as diretivas que você deseja, por exemplo, crie uma pasta com o nome "usr", dentro de "usr", crie uma pasta com o nome "bin" e dentro de "bin"(ISSO SERÁ O MESMO DE VOCÊ INFORMAR QUAIS OS CAMINHOS NO SEU COMPUTADOR QUE DESEJA PARA ONDE OS ARQUIVOS SEJAM COPIADOS) coloque um arquivo, sem extensão com esse código abaixo por exemplo:
{% highlight bash %}
#!/bin/sh
echo "Acesse: terminalroot.com.br e descubra uma pá de coisa!"
{% endhighlight %}
e salve com o nome terminalroot, lembrando que logo após isso dê permissão de execução pra esse arquivo {% highlight bash %}
# chmod +x terminalroot
{% endhighlight %}, no diretório que será criado o .deb, vc também pode criar as pastas e sub-pastas pelo terminal, assim:{% highlight bash %}
# mkdir -p usr/bin
{% endhighlight %} o "usr" não pode ter "/" na frente e você deve estar dentro do diretório terminalroot, pois se se fizer /usr , será criado, se houver permissão, dentro do "usr" do seu computador.Você também pode criar um ícone no painel do Gnome, é só criar a pasta que ficará a imagem no seu computador e a imagem, exemplo:
{% highlight bash %}
# mkdir opt/
# cp imagem.png terminalroot/opt
{% endhighlight %}, e depois criar o icone que aparecerá no painel, exemplo, terminalroot.desktop, dae teria essa configuração dentro do arquivo:
{% highlight bash %}
[Desktop Entry]
Name=Marcos Pinguim
Icon=/opt/terminalroot.png
Type=Application
Categories=GNOME;GTK;Utility;TextEditor;
Exec=terminalroot
StartupNotify=false
Terminal=false
{% endhighlight %}, para isso é necessário haver o carregamento do gtk na execução do programa, que não será esse caso, somente se você digitar no terminal "terminalroot", obterá o texto do salvo no arquivo usr/bin/terminalroot, e salvará esse arquivo terminalroot.desktop dentro da pasta que será criado o pacote .deb:
{% highlight bash %}
# mkdir -p
{% endhighlight %}
6-depois de criar a pasta DEBIAN; o arquivo control; as pasta na raiz da pasta que será copiado os arquivos, e pôr os arquivos que serão copiados dentro das suas respectivas páginas, basta agora "compactar" em .deb
{% highlight bash %}
# dpkg-deb -b terminalroot/ terminalroot.0.1.deb
{% endhighlight %}, e o pacotes será criado.Agora instale-o, ou dando dois cliques(ou abrindo com o Gdebi-installer), ou pelo terminal:{% highlight bash %}
# dpkg -i terminalroot.0.1.deb
{% endhighlight %}
Todas as tarefas restantes serão feitas automaticamente, e o pacote será instalado.Agora vá ao terminal e digite "terminalroot", terá o texto preconfigurado para responder, se fosse um software seria executado, se houve Gtk pre-configurada abriria a janela, até mesmo pelo ícone pre-programado, se você for no painel Aplicativos->Acessórios, verá a imagem lá, entre as opções dos aplicativos, porém se clicar não fará nada.Para remover, basta:
{% highlight bash %}
# apt-get remove terminalroot
{% endhighlight %}, que todos os aruquivos copiados serão remvidos, inclusive as imagens.
Pronto, é só isso, espero que gostem e comentem.
