---
layout: post
title: Como compilar o Kernel 3.0.4
date: '2012-01-06T03:17:00.000-08:00'
description: Como compilar o Kernel 3.0.4
main-class: 'linux'
tags:
- Linux
twitter_text: Como compilar o Kernel 3.0.4
introduction: Como compilar o Kernel 3.0.4
---
Bom, antes demais nada você deve verificar se depois de atualizações feitas pela sua distro se você não já o possui ou se já tem até um superior.Para verificar qual a vesão do seu Kernel, abra o terminal e digite:{% highlight bash %}
$ uname -ar
{% endhighlight %}, agora acesse o diretório src/:{% highlight bash %}
$ cd /usr/src
{% endhighlight %}, já dentro do diretŕorio, baixe o Kernel do Site Oficial:{% highlight bash %}
# wget -c http://www.kernel.org/pub/linux/kernel/v3.0/linux-3.0.4.tar.bz2
{% endhighlight %}Descompacte{% highlight bash %}
# tar xvjf linux-3.0.4.tar.bz2
{% endhighlight %}Entre no diretório descompactado{% highlight bash %}
# cd linux-3.0.4
{% endhighlight %}Para manter o .config  utilizado atualmente pelo seu sistema, execute o comando:{% highlight bash %}
# cp /boot/config-2* /usr/src/linux-3.0.4/.config
{% endhighlight %}, para configurar o módulos execute o comando:{% highlight bash %}
# make menuconfig
{% endhighlight %} Crie uma imagem do Kernel conmpactada e os módulos configurados de uma só vez:{% highlight bash %}
# make -j 3 all
{% endhighlight %}Instale os módulos:{% highlight bash %}
# make -j 3 modules_install
{% endhighlight %}criando o initrd para o Kernel instalado:{% highlight bash %}
# mkinitramfs 3.0.4 -o /boot/initrd.img-3.0.4
{% endhighlight %}Este comando copiará o bzImage para o diretório /boot, renomeando para vmlinuz-3.0.4:{% highlight bash %}
# cp arch/i386/boot/bzImage /boot/vmlinuz-3.0.4
{% endhighlight %}Atualização do grub:{% highlight bash %}
# update-grub ou edite o grub manualmente =) # vim /boot/grub/grub.cfg
{% endhighlight %}
Pronto!
