---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: 'Parte 4: Acesso a dados e modelos | Microsoft Docs'
author: jongalloway
description: "Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo do repositório de música do ASP.NET MVC. Parte 4 aborda o acesso a dados e modelos."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: 727336344493b439130b2cf0ec6e7b5925dd5637
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="part-4-models-and-data-access"></a>Parte 4: Modelos e acesso a dados
====================
por [Jon Galloway](https://github.com/jongalloway)

> O repositório de música MVC é um aplicativo tutorial que apresenta e explica passo a passo sobre como usar o ASP.NET MVC e o Visual Studio para desenvolvimento na web.  
>   
> O repositório de música MVC é uma implementação de repositório de exemplo leve que vende álbuns de música online e implementa a administração de site básico, entrada do usuário e funcionalidade do carrinho de compras.
> 
> Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo do repositório de música do ASP.NET MVC. Parte 4 aborda o acesso a dados e modelos.


Até agora, nós já apenas foi passando "dados fictícios" de nossos controladores para nossos modelos de exibição. Agora estamos prontos para conectar um banco de dados real. Neste tutorial falaremos sobre como usar o SQL Server Compact Edition (geralmente chamado de SQL CE) como nosso mecanismo de banco de dados. SQL CE é um arquivo incorporado, gratuito, com base em dados que não requerem qualquer instalação ou configuração, o que dificulta muito conveniente para desenvolvimento local.

## <a name="database-access-with-entity-framework-code-first"></a>Acesso de banco de dados com o Entity Framework Code-First

Vamos usar o suporte do Entity Framework (EF) que está incluído em projetos do ASP.NET MVC 3 para consultar e atualizar o banco de dados. EF é uma API que permite aos desenvolvedores consultar e atualizar dados armazenados em um banco de dados de uma maneira orientada a objeto de dados do (ORM) de mapeamento relacional de objeto flexível.

Versão 4 do Entity Framework oferece suporte a um paradigma de desenvolvimento chamado código primeiro. Primeiro código permite que você crie o objeto de modelo, escrevendo classes simples (também conhecido como POCO dos objetos de CLR "plain old") e pode até mesmo criar o banco de dados em tempo real de suas classes.

### <a name="changes-to-our-model-classes"></a>Alterações em nossos Classes de modelo

Podemos aproveitará o recurso de criação de banco de dados do Entity Framework neste tutorial. Antes de fazer isso, no entanto, vamos fazer algumas pequenas alterações para nosso classes de modelo para adicionar algumas coisas que será usado posteriormente.

#### <a name="adding-the-artist-model-classes"></a>Adicionando as Classes de modelo artista

Nosso álbuns será associados artistas, então vamos adicionar uma classe de modelo simples para descrever um artista. Adicione uma nova classe para a pasta modelos chamada Artist.cs usando o código mostrado abaixo.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a>Atualizando nossas Classes de modelo

Atualize a classe álbum conforme mostrado abaixo.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

Em seguida, faça as seguintes atualizações para a classe de gênero.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-appdata-folder"></a>Adicionar o aplicativo\_pasta de dados

Vamos adicionar um aplicativo\_diretório de dados ao nosso projeto para manter os arquivos de banco de dados SQL Server Express. Aplicativo\_dados são um diretório especial do ASP.NET que já possui as permissões de acesso de segurança corretas para acesso ao banco de dados. No menu projeto, selecione Adicionar pasta do ASP.NET e, em seguida, aplicativo\_dados.

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a>Criando uma cadeia de caracteres de Conexão no arquivo Web. config

Adicionaremos algumas linhas ao arquivo de configuração do site para que o Entity Framework saiba como conectar-se ao nosso banco de dados. Clique duas vezes no arquivo Web. config localizado na raiz do projeto.

![](mvc-music-store-part-4/_static/image2.png)

Role até o final desse arquivo e adicione um &lt;connectionStrings&gt; seção diretamente acima da última linha, conforme mostrado abaixo.

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a>Adicionando uma classe de contexto

Com o botão direito na pasta modelos e adicione uma nova classe chamada MusicStoreEntities.cs.

![](mvc-music-store-part-4/_static/image3.png)

Essa classe será representar o contexto de banco de dados do Entity Framework e será lidar com nosso criar, ler, atualizar e excluir operações para nós. O código para essa classe é mostrado abaixo.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

Isso é tudo - não há nenhuma outra configuração interfaces especiais, etc. Estendendo a classe base DbContext, nossa classe MusicStoreEntities é capaz de lidar com nossas operações de banco de dados para nós. Agora que temos que vinculada, vamos adicionar mais propriedades para nossos classes de modelo para tirar proveito de algumas das informações adicionais em nosso banco de dados.

### <a name="adding-our-store-catalog-data"></a>Adicionando nossos dados de catálogo do repositório

Nós o conduziremos proveito de um recurso no Entity Framework que adiciona dados de "semente" para um banco de dados recém-criado. Isso preencherá previamente nosso catálogo de repositório com uma lista de gêneros, artistas e álbuns. O download de MvcMusicStore Assets.zip - que nosso site design de arquivos incluídos usados neste tutorial - tem um arquivo de classe com esses dados semente, localizados em uma pasta denominada de código.

Dentro do código de pasta de modelos, localize o arquivo SampleData.cs e solte-o para a pasta de modelos em nosso projeto, conforme mostrado abaixo.

![](mvc-music-store-part-4/_static/image4.png)

Agora, precisamos adicionar uma linha de código para informar o Entity Framework sobre classe SampleData. Clique duas vezes no arquivo global asax na raiz do projeto para abri-lo e adicione a seguinte linha à parte superior do aplicativo\_método Start.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

Neste ponto, o trabalho necessário para configurar o Entity Framework para nosso projeto foram concluídas.

## <a name="querying-the-database"></a>Consultando o banco de dados

Agora vamos atualizar nossos StoreController para que em vez de usar "fictício dados" em vez disso, chame em nosso banco de dados para consultar todas as suas informações. Vamos começar declarando um campo no **StoreController** para manter uma instância da classe MusicStoreEntities, denominada storeDB:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a>Atualizando o índice de repositório para consultar o banco de dados

A classe MusicStoreEntities é mantida pelo Entity Framework e expõe uma propriedade de coleção para cada tabela em nosso banco de dados. Vamos atualizar ação de índice do nosso StoreController para recuperar todos os gêneros em nosso banco de dados. Anteriormente fizemos isso por codificar dados de cadeia de caracteres. Agora podemos em vez disso, usar apenas o contexto do Entity Framework Generes coleção:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

Nenhuma alteração precisa ocorrer ao nosso modelo de exibição, pois ainda estamos retornando o mesmo StoreIndexViewModel são retornados antes - estamos apenas retornando dados dinâmicos do nosso banco de dados agora.

Quando executar o projeto novamente e visita a URL "/ armazenar", agora veremos uma lista de todos os gêneros em nosso banco de dados:

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a>Navegue de repositório de atualização e detalhes para usar dados dinâmicos

Com o repositório/procurar? gênero =*[alguns gênero]* método de ação, estamos pesquisando um gênero por nome. Esperamos que apenas um resultado, desde que não deve ter duas entradas para o mesmo nome de gênero e, portanto, podemos usar o. Extensão de Single() em LINQ para consultar o objeto apropriado de gênero assim (não digite isso ainda):

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

O único método usa uma expressão Lambda como um parâmetro que especifica que desejamos um único objeto de gênero, de modo que seu nome corresponde ao valor que definimos. Nesse caso, estamos carregando um único objeto de gênero com um valor de nome correspondente de Disco.

Vamos dar um recurso do Entity Framework que permite indicar outras entidades relacionadas que desejamos também carregados quando o objeto de gênero é recuperado. Esse recurso é chamado de formatação de resultados de consulta e nos permite reduzir o número de vezes que é necessário para acessar o banco de dados para recuperar todas as informações que necessárias. Queremos a buscar previamente álbuns de gênero recuperamos, portanto vamos atualizar a consulta para incluir em Genres.Include("Albums") para indicar que desejamos também álbuns relacionados. Isso é mais eficiente, pois ela recuperará dados nossos gênero e álbum na solicitação de um único banco de dados.

Com explicações de lado, é como nosso ação do controlador procurar atualizada:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

Agora podemos atualizar a exibição do repositório de procurar para exibir os álbuns que estão disponíveis em cada gênero. Abra o modelo de exibição (localizado em /Views/Store/Browse.cshtml) e adicione uma lista com marcadores de álbuns, conforme mostrado abaixo.

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

Executando o nosso aplicativo e navegando até o repositório/procurar? gênero = mostra Jazz que nossos resultados agora são sendo extraídos do banco de dados, exibir álbuns de todos os nossos gênero selecionado.

![](mvc-music-store-part-4/_static/image2.jpg)

Verifique o mesmo alterar para nosso /Store/detalhes / [id] URL e substitua nossos dados fictícios com uma consulta de banco de dados que carrega um álbum cuja ID corresponde ao valor do parâmetro.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

Executando o nosso aplicativo e navegando até /Store/Details/1 mostram que nossos resultados agora são sendo extraídos do banco de dados.

![](mvc-music-store-part-4/_static/image5.png)

Agora que nossa página de detalhes do repositório está configurada para exibir um álbum pela ID de álbum, vamos atualizar o **procurar** link para o modo de exibição de detalhes no modo de exibição. Usaremos ActionLink, exatamente como foi feito para link do índice de repositório para procurar repositório no final da seção anterior. A fonte completa para o modo de exibição do navegador é exibida abaixo.

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

Agora estamos capazes de navegar em nossa página de armazenamento para uma página de gênero, que lista os álbuns disponíveis, e clicando em um álbum, pode exibir detalhes de álbum.

![](mvc-music-store-part-4/_static/image6.png)

>[!div class="step-by-step"]
[Anterior](mvc-music-store-part-3.md)
[Próximo](mvc-music-store-part-5.md)
