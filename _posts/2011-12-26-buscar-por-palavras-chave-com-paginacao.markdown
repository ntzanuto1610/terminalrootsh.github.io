---
layout: post
title: Buscar por palavras-chave com paginação em PDO
date: '2011-12-26T16:56:00.000-08:00'
description: Buscar por palavras-chave com paginação em PDO
main-class: 'misc'
tags:
- MySQL
- PHP
twitter_text: Buscar por palavras-chave com paginação em PDO
introduction: Buscar por palavras-chave com paginação em PDO
---
Fala rapaziada, mais um post até antes do fim do ano, pretendo lançar no mínimo mais uns 4 hehe...hoje vou mostrar como buscar com palavras chaves, como o Google, o Bing, o Yahoo, o DuckDuckGo e outros buscadores grandes fazem, e paginam, e vamos fazer tudo isso sempre com PDO, não é só porque curto o PDO, mas é a maneira correta, vai que depois você decide mudar o banco pra pra PostGreSQL, Oracle...não precisará mudar todos.
Esse processo é bem simples, apesar de alguns lugares q eu consultei ter dificaultado e embolado tudo.Leiam os comentários em AZUL e onde houver o item [PAGINAÇÃO] é um dado posto só por causa da paginação, pra separar o que é pra BUSCA POR PALAVRA CHAVE e o que é pra PAGINAÇÃO, pois esse post "mata dois coelhos numa só cajadada" e ainda com PDO.Separei em 3 arquivos:
index.php
pesquisa.php
paginacao.php
index.php
{% highlight bash %}
 
 Buscar por palavras-chave com paginação em PDO
 
 
  Digite o termo que deseja pesquisar
  
   
   pesquisar por Título
   pesquisar por Texto
   
     
  
  
  ">Atualizar Página
  Cadastrar Notícia
 
 
{% endhighlight %}
pesquisa.php
{% highlight bash %}
<?php 
 /* definimos o método das variaveis [PAGINAÇÃO] */
 if(isset($_GET['palavra'])){
  $palavra = $_GET['palavra'];
 extract($_GET); 
 }elseif(isset($_POST['palavra'])){
  $palavra = $_POST['palavra'];
 extract($_POST);
 }
 /* se existir pesquisa e for diferente de branco, entramos nesse bloco */
 if(isset($palavra) AND !empty($palavra)){
 /* Conexão como banco com PDO */
  $db = new PDO("mysql:host=localhost;dbname=paginacao","usuario","senha");
 /* máximo de resultados por página [PAGINAÇÃO] */
  $qtde = 2;
  /* caso não exista a variavel GET, ela será 1 [PAGINAÇÃO] */
  $pagina = (isset($_GET['pagina'])) ? (int)$_GET['pagina'] : 1;
  /* definimos de onde começará a paginação [PAGINAÇÃO] */
  $inicio = ($qtde * $pagina) - $qtde;
  /* para evitar o erro de variavel indefinida, pois somaremos a ela */
  $busca = "";
  /* transforma num array, separando os espaços vazis */
  $explode = explode(" ", $palavra);
  /*conta os valores de um array */
  $num = count($explode);
  /* preparamos os valores para jogarmos no bindValue do PDO */
  for ($i=0; $i < $num; $i++) {
    /* resultará em, ex.: titulo LIKE :busca1 OR titulo LIKE :busca2 ... */ 
   $busca .= " `$radio` LIKE :busca$i ";
   if($i$num -1){
    /* vc pode usar AND, depende de como quer sua pesquisa */
    $busca .= " OR ";
   }
  }
  /* preparando a consulta dos dados */
  $sql = "SELECT * FROM `noticias` WHERE $busca ORDER BY `id` ASC LIMIT $inicio, $qtde";
  $buscar = $db->prepare($sql);
  /* separará o :busca$i pelas palavras chaves digitadas */
  for ($i=0; $i < $num; $i++) {
   $buscar->bindValue(":busca$i",'%'.$explode[$i].'%',PDO::PARAM_STR);
  }  
  /* como não foram passadas variaveis pro prepare, basta executar */
  $buscar->execute();
  /* se a consulta houver pelo menos 1 resultado, exibirá o título */
  if($buscar->rowCount() > 0){
   echo '';
   echo "";
   while($res = $buscar->fetch(PDO::FETCH_ASSOC)){
    echo ''.$res['titulo'].'';
   }
  echo "";  
  echo '';
  echo '';
  /* incluímos a paginação [PAGINAÇÃO] */
  require_once('paginacao.php'); 
  echo '';   
  }
 }
?>  
{% endhighlight %}
paginacao.php
{% highlight bash %}
<?php
 /* serve pra saber quantos resultados existirá para tal pesquisa */
  $sqlTotal = "SELECT * FROM `noticias` WHERE $busca";
  $pagTotal = $db->prepare($sqlTotal);  
   for ($i=0; $i < $num; $i++) {
    $pagTotal->bindValue(":busca$i",'%'.$explode[$i].'%',PDO::PARAM_STR);
   }  
  $pagTotal->execute();
  /* arqmaneza a quantidade de resultados, não usamos a outra consulta pra isso,     * pois sempre será o informado pelo LIMIT */
  $numTotal = $pagTotal->rowCount();
  /* arredondamos o total divido pela quantidade */
  $totalPag = ceil($numTotal/$qtde);
 /* agora basta exibir a paginação, se quiser ainda pode criar pagina inicial = 1,    * e ultima pagina = total de paginas */
  for($i=1;$i<=$totalPag;$i++){
   if($i == $pagina)
    echo $i;
   else 
    echo " $i ";    
  }
?>
{% endhighlight %}
Espero que gostem e comentem!
