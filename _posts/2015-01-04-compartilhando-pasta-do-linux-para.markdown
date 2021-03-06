---
layout: post
title: Compartilhando pasta do Linux para acesso via rede por comando
date: '2015-01-04T12:26:00.001-08:00'
description: Compartilhando pasta do Linux para acesso via rede por comando
main-class: 'linux'
tags:
- Linux
- GNU
- Redes
image: http://2.bp.blogspot.com/-n3Uc5NdQi-I/VKmhyvLuNwI/AAAAAAAABH4/sjFm_voXAJw/s72-c/www.facebook.com.png
twitter_text: Compartilhando pasta do Linux para acesso via rede por comando
introduction: Compartilhando pasta do Linux para acesso via rede por comando
---
![Blog Linux](http://2.bp.blogspot.com/-n3Uc5NdQi-I/VKmhyvLuNwI/AAAAAAAABH4/sjFm_voXAJw/s320/www.facebook.com.png "Blog Linux")
Nesse post vou mostrar como compartilhar uma pasta no Linux para acesso via rede por comando, o processo é bem simples.Esse acesso será baseado em NFS, não tem nada a ver com o Samba, isso é pra compartilhamento entre duas ou mais máquinas Linux(de Linux pra Linux), mais pra frente vou explicar via samba, mas por enquanto isso aqui é mais importante.
NFS é um protocolo de sistema de arquivos para compartilhamento entre arquivos e diretórios entre computadores conectados em rede.è com NFS que é feito o acesso, existem outras formas, mas certifique-se que essa é a mais simples e a melhor.
Primeiramente temos de ter dois pacotes instalados, precisa ser root # su:
1 - nfs-kernel-server ---- que deve ser instalado no Server(a máquina que compartilhará o arquivo ou a pasta).
Para instalá-lo no Debian e similares:
{% highlight bash %}
# apt-get install nfs-kernel-server
{% endhighlight %}
Convém também instalar o portmap, caso não esteja instalado:
{% highlight bash %}
# apt-get install portmap
{% endhighlight %}
2- nfs-common ---- que deve ser instalado no Client(máquina que acessará a tal pasta ou arquivo).
Para instalá-lo no Debian e similares:
{% highlight bash %}
# apt-get install nfs-common
{% endhighlight %}
No Server, precisamos informar qual a pasta será compartilhada e quais as permissões, para isso edite o arquivo:
{% highlight bash %}
# vim /etc/exports
{% endhighlight %}
Após as linhas iniciais e comentado com #(tralha), crie uma nova linha sem #(tralha) e informe o endereço da pasta no PC e as permissões do mesmo, exemplo:
{% highlight bash %}
/home/usuario/pasta_a_ser_compartilhada 192.168.1.102(rw,async)
{% endhighlight %}
, ou seja, o caminho da pasta;"rw" permissão de leitura(r) e escrita(w); o item async permite que o NFS transfira arquivos de forma "assíncrona", sem precisar esperar pela resposta do cliente; e o IP que pode acessar a pasta que foi compartilhada.Depois de adicionar a linha, salve o arquivo, lembrando que caso você deseje liberar uma faixa de ips, basta usar o *(asterisco), exemplo: 192.168.1.*, todos dessa faixa poderão acessar, ou até mesmo utilizar nome da Estação.
Para aplicar as alterações no arquivo exports para que o mesmo possa ser lido pelo Kernel, é necessario, exportar e reiniciar o serviço, reinicie também o Portmaps, com os comando:
{% highlight bash %}
# exportfs -ra
# /etc/init.d/nfs-kernel-server restart
# /etc/init.d/portmap restart
{% endhighlight %}
Agora vamos no cliente(máquina que acessará a pasta).Para isso iremos precisar montar a pasta compartilhada da rede, então nada melhor do que você criar uma pasta para receber a montagem, então crie no local onde você deseja essa pasta, exemplo:
{% highlight bash %}
# mkdir /home/usuario/nome_da_pasta
{% endhighlight %}
, e então montamos a pasta compartilhada dentro da que criamos com o seguinte comando:
{% highlight bash %}
# mount -t nfs 192.168.1.101:/home/server/pasta_compartilhada /home/cliente/pasta_criada
{% endhighlight %} , ou seja, a opção (-t) do mount informa o tipo, e eespecificamos o tipo com o nome do tipo que é "nfs", depois informamos o IP que está a pasta compartilhada(o Server) e o local onde montaremos essa pasta, endeço no cliente.
Caso não consiga o acesso, lembre-se de dar permissões locais com o "chmod".Se seu PC pegou um IP via DHCP diferente do que você permitiu no "exports", você pode alterálo com o seguinte comando:
{% highlight bash %}
# ifconfig eth0 192.168.1.102/16 dev eth0
{% endhighlight %}(note que o ip fica a seu critério e o /16 é a mascara 255.255.0.0 na tabela), se desejar especificar um gateway, utilize:{% highlight bash %}
# route add default gw 192.168.1.1 netmask 255.255.0.0 dev eth0
{% endhighlight %},  mas isso é outro assunto...
Para mais informações sobre o NFS, Clique Aqui
Pronto, parece ser complicado, mas é muito simples, é só pôr a mão na massa e pronto!Espero que gostem e comentem.
