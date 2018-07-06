---
uid: mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-cs
title: Melhorando o desempenho com saída de cache (c#) | Microsoft Docs
author: microsoft
description: Neste tutorial, você aprenderá como você pode melhorar drasticamente o desempenho dos aplicativos web ASP.NET MVC, aproveitando o cache de saída. Você...
ms.author: aspnetcontent
ms.date: 01/27/2009
ms.assetid: 521c9117-81cd-4d8d-9d96-0256dc7bf50f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-cs
msc.type: authoredcontent
ms.openlocfilehash: 26e65cb5f0e256d4ca819dfde4a748f00d56f08e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832536"
---
<a name="improving-performance-with-output-caching-c"></a>Melhorando o desempenho com a saída de cache (c#)
====================
por [Microsoft](https://github.com/microsoft)

> Neste tutorial, você aprenderá como você pode melhorar drasticamente o desempenho dos aplicativos web ASP.NET MVC, aproveitando o cache de saída. Você aprenderá a armazenar em cache o resultado retornado de uma ação do controlador para que o mesmo conteúdo não precisa ser criado um novo usuário invoca a ação de cada vez.


O objetivo deste tutorial é explicar como você pode melhorar drasticamente o desempenho de um aplicativo ASP.NET MVC, aproveitando o cache de saída. O cache de saída permite armazenar em cache o conteúdo retornado por uma ação do controlador. Dessa forma, o mesmo conteúdo não precisa ser gerado a cada vez que a mesma ação de controlador é invocada.

Por exemplo, imagine que seu aplicativo ASP.NET MVC exibe uma lista de registros de banco de dados em uma exibição nomeada de índice. Normalmente, cada vez que um usuário invoca a ação do controlador que retorna o modo de exibição de índice, o conjunto de registros do banco de dados deve ser recuperado do banco de dados executando uma consulta de banco de dados.

Se, por outro lado, você aproveitar o cache de saída, em seguida, você pode evitar a execução de uma consulta de banco de dados sempre que qualquer usuário invoca a mesma ação de controlador. O modo de exibição pode ser recuperado do cache em vez de serem gerados a partir de ação do controlador. Armazenamento em cache permite que você para evitar executar redundantes trabalhar no servidor.

## <a name="enabling-output-caching"></a>Habilitação do cache de saída

Habilitar o cache de saída com a adição de um atributo [OutputCache] para uma ação de controlador individual ou uma classe de controlador inteiro. Por exemplo, o controlador na listagem 1 expõe uma ação chamada index (). A saída da ação Index () é armazenado em cache por 10 segundos.

**Listagem 1 – Controllers\HomeController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample1.cs)]

Nas versões Beta do ASP.NET MVC, o cache de saída não funciona para uma URL como [ http://www.MySite.com/ ](http://www.mysite.com/). Em vez disso, você deve inserir uma URL como [ http://www.MySite.com/Home/Index ](http://www.mysite.com/Home/Index). 

Na listagem 1, a saída da ação Index () é armazenado em cache por 10 segundos. Se você preferir, você pode especificar uma duração de cache muito maior. Por exemplo, se você quiser armazenar em cache a saída de uma ação do controlador para um dia, em seguida, você pode especificar uma duração de cache de 86400 segundos (60 segundos \* 60 minutos \* 24 horas).

Não há nenhuma garantia de que o conteúdo será armazenado em cache pelo período de tempo que você especificar. Quando recursos de memória se tornar baixo, o cache inicia automaticamente remover conteúdo.

O controlador Home na listagem 1 retorna a exibição de índice na listagem 2. Não há nada de especial sobre este modo de exibição. O modo de exibição de índice simplesmente exibe a hora atual (consulte a Figura 1).

**Listagem 2 – Views\Home\Index.aspx**

[!code-aspx[Main](improving-performance-with-output-caching-cs/samples/sample2.aspx)]

**Figura 1 – exibição de índice em cache**

![clip_image002](improving-performance-with-output-caching-cs/_static/image1.jpg)

Se você invocar a ação Index () várias vezes inserindo o URL /Home/índice na barra de endereços do navegador e pressionar o botão de atualização/recarregamento em seu navegador repetidamente, em seguida, o horário exibido pelo modo de exibição de índice não será alterado para 10 segundos. O mesmo tempo é exibido porque o modo de exibição é armazenada em cache.

É importante entender que a mesma exibição é armazenada em cache para todas as pessoas que visitam seu aplicativo. Qualquer pessoa que invoca a ação Index () receberá a mesma versão em cache do modo de exibição de índice. Isso significa que a quantidade de trabalho que o servidor web deve executar para servir o modo de exibição de índice é drasticamente reduzida.

O modo de exibição na listagem 2 acontece estar fazendo algo muito simples. O modo de exibição exibe apenas a hora atual. No entanto, você poderia apenas como cache facilmente uma exibição que exibe um conjunto de registros do banco de dados. Nesse caso, o conjunto de registros do banco de dados não precisa ser recuperado do banco de dados cada vez que a ação de controlador que retorna o modo de exibição é invocada. Armazenamento em cache pode reduzir a quantidade de trabalho que devem executar o seu servidor web e o servidor de banco de dados.

Não use a página &lt;% @ OutputCache %&gt; diretiva em uma exibição do MVC. Essa diretiva sangramento ao longo do mundo de Web Forms e não deve ser usada em um aplicativo ASP.NET MVC.

## <a name="where-content-is-cached"></a>Qual conteúdo é armazenado em cache

Por padrão, quando você usa o atributo [OutputCache], o conteúdo é armazenado em três locais: o servidor web, todos os servidores proxy e o navegador da web. Você pode controlar exatamente qual conteúdo é armazenado em cache, modificando a propriedade de local do atributo [OutputCache].

Você pode definir a propriedade Location em qualquer um dos seguintes valores:

> · Qualquer
> 
> · Cliente
> 
> · Downstream
> 
> · Servidor
> 
> · None
> 
> · ServerAndClient


Por padrão, a propriedade Location tem o valor Any. No entanto, há situações em que você talvez queira apenas no navegador ou apenas no servidor de cache. Por exemplo, se você estiver armazenando em cache informações que são personalizadas para cada usuário, em seguida, você deve não armazenar em cache as informações no servidor. Se você estiver exibindo informações diferentes para diferentes usuários, em seguida, você deve armazenar em cache as informações apenas no cliente.

Por exemplo, o controlador na listagem 3 expõe uma ação chamada GetName () que retorna o nome de usuário atual. Se Jack faz logon no site e invoca a ação GetName (), em seguida, a ação retorna a cadeia de caracteres "Hi Jack". Se, posteriormente, Jill faz logon no site e invoca a ação GetName (), em seguida, ela também obterá a cadeia de caracteres "Hi Jack". A cadeia de caracteres é armazenado em cache no servidor web para todos os usuários depois Jack inicialmente invoca a ação do controlador.

**Listagem 3 – Controllers\BadUserController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample3.cs)]

Provavelmente, o controlador na listagem 3 não funciona da maneira que desejar. Você não deseja exibir a mensagem "Olá Jack" a Jill.

Você nunca deve armazenar em cache o conteúdo personalizado no cache do servidor. No entanto, você talvez queira armazenar em cache o conteúdo personalizado no cache do navegador para melhorar o desempenho. Se você armazenar em cache o conteúdo no navegador e um usuário chama a mesma ação de controlador várias vezes, o conteúdo pode ser recuperado do cache do navegador em vez do servidor.

O controlador de modificada na listagem 4 armazena em cache a saída da ação GetName (). No entanto, o conteúdo é armazenado em cache apenas no navegador e não no servidor. Dessa forma, quando vários usuários invocar o método GetName (), cada pessoa obtém seu próprio nome de usuário e nome de usuário não outra pessoa.

**Listagem 4 – Controllers\UserController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample4.cs)]

Observe que o atributo [OutputCache] na listagem 4 inclui uma propriedade local definida como o valor OutputCacheLocation.Client. O atributo [OutputCache] também inclui uma propriedade NoStore. A propriedade NoStore é usada para informar ao navegador e servidores proxy que eles não devem armazenar uma cópia permanente do conteúdo armazenado em cache.

## <a name="varying-the-output-cache"></a>Variar o Cache de saída

Em algumas situações, convém versões armazenadas em cache diferentes do mesmo conteúdo. Por exemplo, imagine que você está criando uma página mestre/detalhes. A página mestra exibe uma lista de títulos de filmes. Quando você clica em um título, você obtém detalhes para o filme selecionado.

Se você armazenar em cache a página de detalhes, em seguida, os detalhes do mesmo filme aparecerão não importa qual filme que você clicar. Primeiro filme selecionado pelo primeiro usuário será exibido para todos os usuários futuros.

Você pode corrigir esse problema, tirando proveito da propriedade VaryByParam do atributo [OutputCache]. Essa propriedade permite que você crie diferentes versões armazenadas em cache o conteúdo mesmo quando um parâmetro de formulário ou parâmetro de cadeia de caracteres de consulta varia.

Por exemplo, o controlador na listagem 5 expõe duas ações chamadas Master() e Details(). A ação Master() retorna uma lista de títulos de filmes e a ação de Details() retorna os detalhes para o filme selecionado.

**Listagem 5 – Controllers\MoviesController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample5.cs)]

A ação Master() inclui uma propriedade VaryByParam com o valor "none". Quando exibe o Master() ação é invocada, a mesma versão em cache do mestre será retornado. Quaisquer parâmetros de formulário ou cadeia de caracteres de consulta os parâmetros são ignorados (veja a Figura 2).

**Figura 2 – o modo de exibição /Movies/Master**

![clip_image004](improving-performance-with-output-caching-cs/_static/image2.jpg)

**Figura 3 – o modo de exibição de detalhes/Movies /**

![clip_image006](improving-performance-with-output-caching-cs/_static/image3.jpg)

A ação de Details() inclui uma propriedade VaryByParam com o valor "Id". Quando valores diferentes do parâmetro Id são passados para a ação do controlador, diferentes versões armazenadas em cache do modo de exibição de detalhes são geradas.

É importante entender que usando os resultados da propriedade VaryByParam na mais armazenamento em cache e não é menor. Uma versão em cache diferente do modo de exibição de detalhes é criada para cada versão diferente do parâmetro Id.

Você pode definir a propriedade VaryByParam com os seguintes valores:

> \* = Crie uma versão em cache diferente sempre que varia de um parâmetro de cadeia de caracteres de consulta ou formulário.
> 
> None = nunca criar diferentes versões armazenadas em cache
> 
> Lista de ponto e vírgula dos parâmetros = criar diferentes versões em cache sempre que qualquer um dos parâmetros de cadeia de caracteres de formulário ou a consulta na lista varia de acordo


## <a name="creating-a-cache-profile"></a>Criando um perfil de Cache

Como uma alternativa para configurar propriedades de cache de saída, modificando as propriedades do atributo [OutputCache], você pode criar um perfil de cache no arquivo de configuração (Web. config) da web. Criar um perfil de cache no arquivo de configuração da web oferece algumas vantagens importantes.

Primeiro, configurando o cache de saída no arquivo de configuração da web, você pode controlar como as ações do controlador armazenar em cache o conteúdo em um local central. Você pode criar um perfil de cache e aplique o perfil para vários controladores ou ações do controlador.

Em segundo lugar, você pode modificar o arquivo de configuração da web sem recompilar seu aplicativo. Se você precisar desabilitar o cache para um aplicativo que já foi implantado para produção, em seguida, você pode simplesmente modificar os perfis de cache definidos no arquivo de configuração web. Todas as alterações ao arquivo de configuração da web serão detectadas automaticamente e aplicadas.

Por exemplo, o &lt;caching&gt; seção de configuração da web na listagem 6 define um perfil de cache chamado Cache1Hour. O &lt;caching&gt; seção deve ser exibido entre os &lt;System. Web&gt; seção de um arquivo de configuração da web.

**Listagem 6 – seção de cache para a Web. config**

[!code-xml[Main](improving-performance-with-output-caching-cs/samples/sample6.xml)]

O controlador na listagem 7 ilustra como você pode aplicar o perfil Cache1Hour para uma ação do controlador com o atributo [OutputCache].

**Listagem 7 – Controllers\ProfileController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample7.cs)]

Se você invocar a ação Index () exposta pelo controlador na listagem 7 será retornado o mesmo tempo por 1 hora.

## <a name="summary"></a>Resumo

O cache de saída fornece um método muito fácil de melhorar drasticamente o desempenho de seus aplicativos ASP.NET MVC. Neste tutorial, você aprendeu a usar o atributo [OutputCache] para armazenar em cache a saída das ações do controlador. Você também aprendeu como modificar as propriedades do atributo [OutputCache] como as propriedades de duração e VaryByParam para modificar como conteúdo é armazenado em cache. Por fim, você aprendeu como definir perfis de cache no arquivo de configuração da web.

> [!div class="step-by-step"]
> [Anterior](understanding-action-filters-cs.md)
> [Próximo](adding-dynamic-content-to-a-cached-page-cs.md)
