---
uid: mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-cs
title: "Melhorando o desempenho com saída cache (c#) | Microsoft Docs"
author: microsoft
description: "Neste tutorial, você aprenderá como você pode melhorar drasticamente o desempenho dos aplicativos da web ASP.NET MVC, aproveitando o cache de saída. Você..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 521c9117-81cd-4d8d-9d96-0256dc7bf50f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-cs
msc.type: authoredcontent
ms.openlocfilehash: 47f0aa976c5876991ccc2406fb8f7402e59ec556
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="improving-performance-with-output-caching-c"></a>Melhorando o desempenho com a saída de cache (c#)
====================
por [Microsoft](https://github.com/microsoft)

> Neste tutorial, você aprenderá como você pode melhorar drasticamente o desempenho dos aplicativos da web ASP.NET MVC, aproveitando o cache de saída. Você aprenderá como armazenar em cache o resultado retornado de uma ação do controlador para que o mesmo conteúdo não precisa ser criado cada vez que um novo usuário invoca a ação.


O objetivo deste tutorial é explicar como você pode melhorar drasticamente o desempenho de um aplicativo ASP.NET MVC, aproveitando o cache de saída. O cache de saída permite armazenar em cache o conteúdo retornado por uma ação do controlador. Dessa forma, o mesmo conteúdo não precisa ser gerado sempre que a mesma ação de controlador é invocada.

Por exemplo, imagine que seu aplicativo ASP.NET MVC exibe uma lista de registros do banco de dados em uma exibição nomeada índice. Normalmente, cada vez que um usuário invoca a ação de controlador que retorna a exibição do índice, o conjunto de registros do banco de dados deve ser recuperado do banco de dados executando uma consulta de banco de dados.

Se, por outro lado, você se beneficiar do cache de saída, você poderá evitar a execução de uma consulta de banco de dados sempre que qualquer usuário chama a mesma ação de controlador. O modo de exibição pode ser recuperado do cache em vez de serem gerados a partir de ação do controlador. Cache permite que você para evitar executar redundantes trabalhar no servidor.

## <a name="enabling-output-caching"></a>Habilitar o cache de saída

Habilitar o cache de saída com a adição de um atributo [OutputCache] para uma ação de controlador individual ou uma classe inteira de controlador. Por exemplo, o controlador na listagem 1 expõe uma ação chamada index (). A saída da ação Index () é armazenado em cache por 10 segundos.

**Listando 1 – Controllers\HomeController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample1.cs)]

Nas versões Beta do ASP.NET MVC, o cache de saída não funciona para uma URL como [http://www.MySite.com/](http://www.mysite.com/). Em vez disso, você deve inserir uma URL como [http://www.MySite.com/Home/Index](http://www.mysite.com/Home/Index). 

Na listagem 1, a saída da ação Index () é armazenado em cache por 10 segundos. Se preferir, você pode especificar uma duração de cache muito mais. Por exemplo, se você deseja armazenar em cache a saída de uma ação do controlador para um dia, em seguida, você pode especificar a duração do cache de 86400 segundos (60 segundos \* 60 minutos \* 24 horas).

Não há nenhuma garantia de que o conteúdo será armazenado em cache pelo período de tempo que você especificar. Quando os recursos de memória se tornar baixos, o cache inicia automaticamente remover conteúdo.

O controlador Home na listagem 1 retorna a exibição do índice na listagem 2. Não há nada de especial sobre este modo de exibição. A exibição índice simplesmente exibe a hora atual (consulte a Figura 1).

**A listagem 2 – Views\Home\Index.aspx**

[!code-aspx[Main](improving-performance-with-output-caching-cs/samples/sample2.aspx)]

**Figura 1 – o modo de exibição do índice de cache**

![clip_image002](improving-performance-with-output-caching-cs/_static/image1.jpg)

Se você chama a ação Index () várias vezes, digitando o URL /Home/índice na barra de endereços do navegador e pressionar o botão de atualização/Recarregar no seu navegador repetidamente, em seguida, horário exibido pela exibição de índice não será alterado para 10 segundos. O mesmo tempo é exibido porque o modo de exibição é armazenada em cache.

É importante entender que a mesma exibição é armazenada em cache para todas as pessoas que visitam seu aplicativo. Qualquer pessoa que chama a ação Index () receberá a mesma versão em cache do modo de exibição do índice. Isso significa que a quantidade de trabalho que o servidor web deve executar para atender a exibição do índice é drasticamente reduzida.

O modo de exibição na lista 2 ocorre para fazer algo bastante simples. O modo de exibição exibe apenas a hora atual. No entanto, você pode apenas como cache facilmente uma exibição que exibe um conjunto de registros do banco de dados. Nesse caso, o conjunto de registros do banco de dados não precisa ser recuperado do banco de dados sempre a ação de controlador que retorna a exibição é invocada. O cache pode reduzir a quantidade de trabalho que seu servidor web e o servidor de banco de dados devem executar.

Não use a página &lt;% @ OutputCache %&gt; diretiva em uma exibição do MVC. Essa diretiva é transferem do mundo formulários da Web e não deve ser usada em um aplicativo ASP.NET MVC.

## <a name="where-content-is-cached"></a>Qual conteúdo é armazenado em cache

Por padrão, quando você usa o atributo [OutputCache], conteúdo é armazenado em cache em três locais: o servidor web, todos os servidores proxy e o navegador da web. Você pode controlar exatamente qual conteúdo é armazenado em cache, modificando a propriedade local do atributo [OutputCache].

Você pode definir a propriedade de local para qualquer um dos seguintes valores:

> · Qualquer
> 
> · Cliente
> 
> · Downstream
> 
> · Servidor
> 
> · Nenhum
> 
> · ServerAndClient


Por padrão, a propriedade Location tem o valor de qualquer. No entanto, há situações em que você talvez queira apenas no navegador ou apenas no servidor de cache. Por exemplo, se você estiver armazenando em cache informações personalizado para cada usuário, em seguida, você não armazenar em cache as informações no servidor. Se você estiver exibindo informações diferentes para usuários diferentes, em seguida, você deve armazenar em cache as informações de somente no cliente.

Por exemplo, o controlador na listagem 3 expõe uma ação chamada GetName que retorna o nome do usuário atual. Se tomada fizer logon no site e invoca a ação GetName a ação retorna a cadeia de caracteres "Hi tomada". Se, posteriormente, Jill logs para o site e invoca a ação GetName, ela também obterá a cadeia de caracteres "Hi tomada". A cadeia de caracteres é armazenado em cache no servidor web para todos os usuários depois tomada inicialmente invoca a ação de controlador.

**A listagem 3 – Controllers\BadUserController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample3.cs)]

Provavelmente, o controlador na listagem 3 não funcionar da maneira que você deseja. Você não deseja exibir a mensagem "Conector Hi" a Jill.

Você nunca deve armazenar em cache o conteúdo personalizado no cache do servidor. No entanto, você talvez queira armazenar em cache o conteúdo personalizado no cache do navegador para melhorar o desempenho. Se você armazenar em cache o conteúdo no navegador, e um usuário invoca a ação de controlador mesmo várias vezes, o conteúdo pode ser recuperado do cache do navegador em vez do servidor.

O controlador modificado na listagem 4 armazena a saída da ação GetName. No entanto, o conteúdo é armazenado em cache apenas no navegador e não no servidor. Dessa forma, quando vários usuários invoca o método GetName, cada pessoa que obtém seu próprio nome de usuário e o nome de usuário não outra pessoa.

**A listagem 4 – Controllers\UserController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample4.cs)]

Observe que o atributo [OutputCache] na listagem 4 inclui uma propriedade de local definida como o valor OutputCacheLocation.Client. O atributo [OutputCache] também inclui uma propriedade NoStore. A propriedade NoStore é usada para informar o navegador e servidores proxy que eles não devem armazenar uma cópia permanente do conteúdo armazenado em cache.

## <a name="varying-the-output-cache"></a>Variar o Cache de saída

Em algumas situações, convém versões em cache diferentes do mesmo conteúdo. Por exemplo, imagine que você está criando uma página de detalhes/mestre. A página mestra exibe uma lista de títulos de filme. Quando você clica em um título, você obterá detalhes para o filme selecionado.

Se você armazenar a página de detalhes, os detalhes para o mesmo filme serão exibidos, independentemente de qual filme que você clica. Primeiro filme selecionado pelo usuário primeiro será exibido para todos os usuários futuras.

Você pode corrigir esse problema, tirando proveito da propriedade VaryByParam do atributo [OutputCache]. Essa propriedade permite que você crie diferentes versões em cache o conteúdo mesmo quando um parâmetro de formulário ou parâmetro de cadeia de caracteres de consulta varia.

Por exemplo, o controlador na listagem 5 expõe duas ações denominadas Master() e Details(). A ação Master() retorna uma lista de títulos de filme e a ação de Details() retorna os detalhes de filme selecionado.

**Listando 5 – Controllers\MoviesController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample5.cs)]

A ação de Master() inclui uma propriedade VaryByParam com o valor "none". Quando visualiza o Master() ação é invocada, a mesma versão em cache do mestre será retornado. Quaisquer parâmetros de formulário ou cadeia de caracteres de consulta os parâmetros são ignorados (consulte a Figura 2).

**Figura 2 – o modo de exibição /Movies/Master**

![clip_image004](improving-performance-with-output-caching-cs/_static/image2.jpg)

**Figura 3 – o modo de exibição de detalhes/filmes**

![clip_image006](improving-performance-with-output-caching-cs/_static/image3.jpg)

A ação de Details() inclui uma propriedade VaryByParam com o valor "Id". Quando valores diferentes do parâmetro Id são passados para a ação de controlador, diferentes versões em cache do modo de exibição de detalhes são geradas.

É importante entender que usando os resultados da propriedade VaryByParam em cache mais e não menor. Uma versão diferente de cache do modo de exibição de detalhes é criada para cada versão diferente do parâmetro Id.

Você pode definir a propriedade VaryByParam com os seguintes valores:

> \*= Crie uma versão diferente de cache sempre que varia de um parâmetro de cadeia de caracteres de consulta ou do formulário.
> 
> None = nunca criar versões diferentes de cache
> 
> Lista de ponto e vírgula de parâmetros = criar versões diferentes de cache sempre que qualquer um dos parâmetros da cadeia de consulta ou formulário na lista varia


## <a name="creating-a-cache-profile"></a>Criar um perfil de Cache

Como alternativa para configurar propriedades de cache de saída, modificando as propriedades do atributo [OutputCache], você pode criar um perfil de cache no arquivo de configuração (Web. config) da web. Criar um perfil de cache no arquivo de configuração da web oferece algumas vantagens importantes.

Primeiro, configurando o cache de saída no arquivo de configuração da web, você pode controlar como ações do controlador de cache de conteúdo em um local central. Você pode criar um perfil de cache e aplicar o perfil para vários controladores ou ações do controlador.

Em seguida, você pode modificar o arquivo de configuração da web sem recompilar o aplicativo. Se você precisar desativar o cache para um aplicativo que já tenha sido implantado para produção, em seguida, você pode modificar simplesmente os perfis de cache definidos no arquivo de configuração da web. As alterações ao arquivo de configuração da web serão detectadas automaticamente e aplicadas.

Por exemplo, o &lt;cache&gt; seção de configuração da web na listagem 6 define um perfil de cache chamado Cache1Hour. O &lt;cache&gt; seção deve aparecer dentro de &lt;System. Web&gt; seção de um arquivo de configuração da web.

**Listando 6 – seção cache para Web. config**

[!code-xml[Main](improving-performance-with-output-caching-cs/samples/sample6.xml)]

O controlador na listagem 7 ilustra como você pode aplicar o perfil Cache1Hour para uma ação do controlador com o atributo [OutputCache].

**Listando 7 – Controllers\ProfileController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample7.cs)]

Se você chamar a ação de Index () exposta pelo controlador na listagem 7 simultaneamente será retornado por 1 hora.

## <a name="summary"></a>Resumo

Cache de saída fornece um método fácil de melhorar drasticamente o desempenho de seus aplicativos ASP.NET MVC. Neste tutorial, você aprendeu a usar o atributo [OutputCache] em cache a saída de ações do controlador. Você também aprendeu como modificar as propriedades do atributo [OutputCache] como as propriedades de duração e VaryByParam para modificar como conteúdo é armazenado em cache. Por fim, você aprendeu como definir perfis de cache no arquivo de configuração da web.

>[!div class="step-by-step"]
[Anterior](understanding-action-filters-cs.md)
[Próximo](adding-dynamic-content-to-a-cached-page-cs.md)
