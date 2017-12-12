---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: Criar um ponto de extremidade OData v4 usando ASP.NET Web API 2.2 | Microsoft Docs
author: MikeWasson
description: "O Open Data Protocol (OData) é um protocolo de acesso de dados para a web. O OData fornece uma maneira uniforme para consultar e manipular os conjuntos de dados por meio de operações CRUD..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/24/2014
ms.topic: article
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: a3f94818f9674b0e1e9a45b2a6cc9455edc79726
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="create-an-odata-v4-endpoint-using-aspnet-web-api-22"></a>Criar um ponto de extremidade OData v4 usando ASP.NET Web API 2.2
====================
por [Mike Wasson](https://github.com/MikeWasson)

> O Open Data Protocol (OData) é um protocolo de acesso de dados para a web. O OData fornece uma maneira uniforme para consultar e manipular os conjuntos de dados por meio de operações CRUD (criar, ler, atualizar e excluir).
> 
> Dá suporte a ASP.NET Web API v3 e v4 do protocolo. Você pode ter um ponto de extremidade v4 executa lado a lado com um ponto de extremidade v3.
> 
> Este tutorial mostra como criar um ponto de extremidade de v4 do OData que suporta operações CRUD.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - 2.2 da API da Web
> - OData v4
> - [Visual Studio 2013 Atualização 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - Entity Framework 6
> - .NET 4.5
> 
> 
> ## <a name="tutorial-versions"></a>Versões do tutoriais
> 
> Para o OData versão 3, consulte [criar um ponto de extremidade OData v3](../odata-v3/creating-an-odata-endpoint.md).


## <a name="create-the-visual-studio-project"></a>Criar o projeto do Visual Studio

No Visual Studio, do **arquivo** menu, selecione **novo** &gt; **projeto**.

Expanda **instalado** &gt; **modelos** &gt; **Visual C#** &gt; **Web**e selecione o  **Aplicativo Web ASP.NET** modelo. Nomeie o projeto &quot;ProductService&quot;.

[![](create-an-odata-v4-endpoint/_static/image2.png)](create-an-odata-v4-endpoint/_static/image1.png)

No **novo projeto** caixa de diálogo, selecione o **vazio** modelo. Em &quot;adicionar pastas e referências de núcleo... &quot;, clique em **API da Web**. Clique em **OK**.

[![](create-an-odata-v4-endpoint/_static/image4.png)](create-an-odata-v4-endpoint/_static/image3.png)

## <a name="install-the-odata-packages"></a>Instalar os pacotes de OData

Do **ferramentas** menu, selecione **NuGet Package Manager** &gt; **Package Manager Console**. Na janela do Console do Gerenciador de pacote, digite:

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

Esse comando instala os pacotes de OData NuGet mais recentes.

## <a name="add-a-model-class"></a>Adicionar uma classe de modelo

Um *modelo* é um objeto que representa uma entidade de dados em seu aplicativo.

No Gerenciador de soluções, clique na pasta de modelos. No menu de contexto, selecione **adicionar** &gt; **classe**.

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> Por convenção, as classes de modelo são colocadas na pasta de modelos, mas você não precisa seguir essa convenção em seus próprios projetos.


Nomeie a classe `Product`. No arquivo Product.cs, substitua o código clichê com o seguinte:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

O `Id` propriedade é a chave de entidade. Os clientes podem consultar entidades por chave. Por exemplo, para obter o produto com ID 5, o URI é `/Products(5)`. O `Id` propriedade também será a chave primária no banco de dados back-end.

## <a name="enable-entity-framework"></a>Habilitar o Entity Framework

Para este tutorial, usaremos Entity Framework (EF) Code First para criar o banco de dados de back-end.

> [!NOTE]
> OData da API da Web não requerem EF. Use qualquer camada de acesso a dados que pode ser traduzidos entidades de banco de dados em modelos.


Primeiro, instale o pacote NuGet para EF. Do **ferramentas** menu, selecione **NuGet Package Manager** &gt; **Package Manager Console**. Na janela do Console do Gerenciador de pacote, digite:

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

Abra o arquivo Web. config e adicione a seguinte seção dentro de **configuração** elemento, após o **configSections** elemento.

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

Essa configuração adiciona uma cadeia de caracteres de conexão para um banco de dados LocalDB. Este banco de dados será usado quando você executar o aplicativo localmente.

Em seguida, adicione uma classe denominada `ProductsContext` para a pasta de modelos:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

No construtor, `"name=ProductsContext"` fornece o nome da cadeia de caracteres de conexão.

## <a name="configure-the-odata-endpoint"></a>Configurar o ponto de extremidade OData

Abra o arquivo de aplicativo\_Start/WebApiConfig.cs. Adicione o seguinte **usando** instruções:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

Em seguida, adicione o seguinte código para o **registrar** método:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

Este código faz duas coisas:

- Cria um modelo de dados de entidade (EDM).
- Adiciona uma rota.

Um EDM é um modelo abstrato dos dados. EDM é usado para criar o documento de metadados do serviço. O **ODataConventionModelBuilder** classe cria um EDM usando convenções de nomenclatura padrão. Essa abordagem exige o mínimo de código. Se você quiser mais controle sobre o EDM, você pode usar o **ODataModelBuilder** classe para criar o EDM Adicionando propriedades, chaves e propriedades de navegação explicitamente.

Um *rota* informa como rotear solicitações HTTP para o ponto de extremidade de API da Web. Para criar uma rota do OData v4, chame o **MapODataServiceRoute** método de extensão.

Se seu aplicativo tiver vários pontos de extremidade do OData, crie uma rota separada para cada um. Dê a cada rota de um nome de rota exclusivo e um prefixo.

## <a name="add-the-odata-controller"></a>Adicionar o controlador de OData

Um *controlador* é uma classe que trata as solicitações HTTP. Você criar um controlador separado para cada entidade definida no seu serviço OData. Neste tutorial, você criará um controlador, para o `Product` entidade.

No Gerenciador de soluções, clique na pasta controladores e selecione **adicionar** &gt; **classe**. Nomeie a classe `ProductsController`.

> [!NOTE]
> A versão deste tutorial para OData v3 usa o **Adicionar controlador** scaffolding. Atualmente, não há nenhum scaffolding para OData v4.


Substitua o código de texto clichê em ProductsController.cs com o seguinte.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

O controlador usa o `ProductsContext` classe para acessar o banco de dados usando EF. Observe que o controlador substitui o **Dispose** método para descartar o **ProductsContext**.

Este é o ponto de partida para o controlador. Em seguida, vamos adicionar métodos para todas as operações CRUD.

## <a name="querying-the-entity-set"></a>Consultando o conjunto de entidades

Adicione os seguintes métodos para `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

A versão para o `Get` método retorna toda a coleção de produtos. O `Get` método com um *chave* parâmetro procura um produto por sua chave (nesse caso, o `Id` propriedade).

O **[EnableQuery]** atributo permite que os clientes modificar a consulta, usando as opções de consulta, como $filter, $sort e $page. Para obter mais informações, consulte [dando suporte a opções de consulta OData](../supporting-odata-query-options.md).

## <a name="adding-an-entity-to-the-entity-set"></a>Adicionando uma entidade para o conjunto de entidades

Para habilitar clientes para adicionar um novo produto no banco de dados, adicione o seguinte método para `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="updating-an-entity"></a>Atualizando uma entidade

OData oferece suporte a dois semânticas diferentes para atualizar uma entidade, PATCH e PUT.

- PATCH executa uma atualização parcial. O cliente especifica apenas as propriedades para atualizar.
- PUT substitui a entidade inteira.

A desvantagem de PUT é que o cliente deve enviar valores para todas as propriedades da entidade, incluindo os valores que não estão sendo alteradas. O [especificação OData](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) afirma que o PATCH é preferencial.

Em qualquer caso, aqui está o código para os métodos de PATCH e PUT:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

No caso de PATCH, o controlador usará o **Delta&lt;T&gt;**  tipo para controlar as alterações.

## <a name="deleting-an-entity"></a>Excluindo uma entidade

Para habilitar clientes para excluir um produto do banco de dados, adicione o seguinte método para `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
