---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: 'Parte 4: Acesso a dados e modelos | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo de Store de música do ASP.NET MVC. Parte 4 aborda o acesso a dados e modelos.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: ea8fe623a1b59b80fd7f087036b9ed716eafadbe
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402032"
---
<a name="part-4-models-and-data-access"></a>Parte 4: Modelos e acesso a dados
====================
por [Jon Galloway](https://github.com/jongalloway)

> A Store de música do MVC é um aplicativo tutorial que apresenta e explica passo a passo de como usar o ASP.NET MVC e o Visual Studio para desenvolvimento da web.  
>   
> A Store de música do MVC é uma implementação de repositório de exemplo leve que vende álbuns de música online e implementa a administração de site básico, entrada do usuário e a funcionalidade de carrinho de compras.
> 
> Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo de Store de música do ASP.NET MVC. Parte 4 aborda o acesso a dados e modelos.


Até agora, podemos tiver apenas foi passando "dados fictícios" de nossos controladores para nossos modelos de exibição. Agora estamos prontos para ligar a um banco de dados real. Neste tutorial falaremos sobre como usar o SQL Server Compact Edition (geralmente chamado de SQL CE) como nosso mecanismo de banco de dados. SQL CE é um banco de dados de arquivo incorporado, gratuito, com base em que não requer qualquer instalação ou configuração, o que torna muito conveniente para o desenvolvimento local.

## <a name="database-access-with-entity-framework-code-first"></a>Acesso de banco de dados com o Entity Framework Code-First

Vamos usar o suporte do Entity Framework (EF) que é incluído em projetos do ASP.NET MVC 3 para consultar e atualizar o banco de dados. O EF é uma API que permite aos desenvolvedores consultar e atualizar dados armazenados em um banco de dados de forma orientada a objeto de dados do (ORM) de mapeamento relacional de objeto flexível.

Entity Framework versão 4 dá suporte a um paradigma de desenvolvimento, primeiro código de chamada. Primeiro de código permite que você crie o objeto de modelo ao escrever classes simples (também conhecido como POCO de objetos CLR "bom e velho") e pode até mesmo criar o banco de dados em tempo real de suas classes.

### <a name="changes-to-our-model-classes"></a>Alterações em nossas Classes de modelo

Vamos aproveitar o recurso de criação de banco de dados no Entity Framework neste tutorial. Antes de fazer isso, no entanto, vamos fazer algumas pequenas alterações nossas classes de modelo para adicionar algumas coisas que usaremos mais tarde.

#### <a name="adding-the-artist-model-classes"></a>Adicionando Classes de modelo do artista

Nosso álbuns será associados com artistas, portanto, vamos adicionar uma classe de modelo simples para descrever um artista. Adicione uma nova classe para a pasta modelos chamada Artist.cs usando o código mostrado abaixo.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a>Atualizando a nossas Classes de modelo

Atualize a classe de álbum, conforme mostrado abaixo.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

Em seguida, faça as seguintes atualizações para a classe de gênero.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-appdata-folder"></a>Adicionar o aplicativo\_pasta de dados

Vamos adicionar um aplicativo\_diretório de dados ao nosso projeto para manter nossos arquivos de banco de dados SQL Server Express. Aplicativo\_dados são um diretório especial no ASP.NET que já tem as permissões de acesso de segurança correto para acesso ao banco de dados. No menu projeto, selecione Adicionar pasta ASP.NET e, em seguida, aplicativo\_dados.

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a>Criando uma cadeia de Conexão no arquivo Web. config

Adicionaremos algumas linhas ao arquivo de configuração do site para que o Entity Framework saiba como se conectar ao nosso banco de dados. Clique duas vezes no arquivo Web. config localizado na raiz do projeto.

![](mvc-music-store-part-4/_static/image2.png)

Role até o final desse arquivo e adicione uma &lt;connectionStrings&gt; seção diretamente acima da última linha, conforme mostrado abaixo.

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a>Adicionando uma classe de contexto

Clique com botão direito na pasta modelos e adicione uma nova classe chamada MusicStoreEntities.cs.

![](mvc-music-store-part-4/_static/image3.png)

Essa classe será representar o contexto de banco de dados do Entity Framework e será lidar com nosso criar, ler, atualizar e excluir operações para nós. O código para essa classe é mostrado abaixo.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

É isso – não há nenhuma outra configuração, interfaces especiais, etc. Estendendo a classe base DbContext, nossa classe MusicStoreEntities é capaz de lidar com nossas operações de banco de dados para nós. Agora que temos que ligado, vamos adicionar mais algumas propriedades para nossas classes de modelo para aproveitar algumas das informações adicionais em nosso banco de dados.

### <a name="adding-our-store-catalog-data"></a>Adição de nossos dados de catálogo do repositório

Obtemos proveito de um recurso no Entity Framework, que adiciona dados de "semente" para um banco de dados recém-criado. Isso irá preencher previamente o nosso catálogo de repositório com uma lista dos álbuns, artistas e gêneros. O download de MvcMusicStore Assets.zip - que incluía nossos arquivos de design de site usados neste tutorial - tem um arquivo de classe com esses dados de semente, localizados em uma pasta chamada código.

Dentro do código / pasta de modelos, localize o arquivo SampleData.cs e solte-o para a pasta de modelos em nosso projeto, conforme mostrado abaixo.

![](mvc-music-store-part-4/_static/image4.png)

Agora, precisamos adicionar uma linha de código para informar ao Entity Framework sobre essa classe SampleData. Clique duas vezes no arquivo global asax na raiz do projeto para abri-lo e adicione a seguinte linha à parte superior do aplicativo\_método Start.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

Neste ponto, concluímos o trabalho necessário para configurar o Entity Framework para nosso projeto.

## <a name="querying-the-database"></a>Consultando o banco de dados

Agora vamos atualizar nossos StoreController para que em vez de usar "fictício dados" em vez disso, chame em nosso banco de dados para consultar todas as suas informações. Vamos começar com a declaração de um campo na **StoreController** para manter uma instância da classe MusicStoreEntities, chamada storeDB:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a>Atualizar o índice da Store para consultar o banco de dados

A classe MusicStoreEntities é mantida pelo Entity Framework e expõe uma propriedade de coleção para cada tabela no banco de dados. Vamos atualizar ação de índice do nosso StoreController para recuperar todos os gêneros em nosso banco de dados. Anteriormente fizemos isso por codificar dados de cadeia de caracteres. Agora, podemos em vez disso, basta usar o contexto do Entity Framework Generes coleção:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

Nenhuma alteração precisa acontecer ao nosso modelo de exibição, pois ainda estamos retornando o mesmo StoreIndexViewModel nós retornados antes - estamos apenas retornando dados ao vivo do nosso banco de dados agora.

Quando executar o projeto novamente e acesse a URL "/ Store", podemos agora verá uma lista de todos os gêneros em nosso banco de dados:

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a>Atualizando Store procurar e detalhes para usar dados dinâmicos

Com/Store/procurar? genre =*[alguns gênero]* método de ação, procurando por um gênero por nome. Esperamos que apenas um resultado, já que não deve nunca temos duas entradas para o mesmo nome de gênero e, portanto, podemos usar o. Extensão de Single () em LINQ para consultar o objeto de gênero apropriado como esta (não digite isso ainda):

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

O método único usa uma expressão Lambda como um parâmetro que especifica o que queremos que um único objeto de gênero, de modo que seu nome corresponde ao valor que definimos. No caso acima, estamos carregando um único objeto de gênero com um valor de nome correspondente de Disco.

Vamos dar vantagem de um recurso do Entity Framework que permite indicar a outras entidades relacionadas que queremos também carregados quando o objeto de gênero é recuperado. Esse recurso é chamado de Shaping de resultado de consulta e nos permite reduzir o número de vezes que é necessário para acessar o banco de dados para recuperar todas as informações que necessárias. Queremos que a buscar previamente os álbuns de gênero recuperamos, portanto, atualizaremos nossa consulta para incluir em Genres.Include("Albums") para indicar que desejamos álbuns relacionados também. Isso é mais eficiente, porque ele irá recuperar dados de nosso gênero e do álbum em uma solicitação de banco de dados individual.

Com as explicações fora do caminho, esta é a aparência de nossa ação de controlador procurar atualizada:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

Agora podemos atualizar a exibição do Store Procurar para exibir os álbuns que estão disponíveis em cada gênero. Abra o modelo de exibição (encontrado na /Views/Store/Browse.cshtml) e adicione uma lista com marcadores de álbuns, conforme mostrado abaixo.

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

Executar o aplicativo e navegando até/Store/procurar? genre = mostra Jazz nossos resultados agora estão sendo extraídos do banco de dados, exibindo todos os álbuns em nosso gênero selecionado.

![](mvc-music-store-part-4/_static/image2.jpg)

Podemos fazer o mesmo alterar para nossa URL de /Store/detalhes / [id] e substituir nossos dados fictícios com uma consulta de banco de dados que carrega um álbum cuja identificação corresponde ao valor do parâmetro.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

Executar o aplicativo e navegando até /Store/Details/1 mostram que nossos resultados agora estão sendo extraídos do banco de dados.

![](mvc-music-store-part-4/_static/image5.png)

Agora que nossa página de detalhes de Store é configurada para exibir um álbum pela ID do álbum, vamos atualizar o **procurar** modo de exibição para vincular a exibição de detalhes. Nós usaremos o HTML. ActionLink, exatamente como fizemos para vincular de Store de índice para procurar Store no final da seção anterior. A fonte completa para o modo de exibição do navegador é exibida abaixo.

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

Agora estamos capazes de navegar de nossa página da Store para uma página de gênero, que lista os álbuns disponíveis, e clicando em um álbum, podemos exibir detalhes do álbum.

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-3.md)
> [Próximo](mvc-music-store-part-5.md)
