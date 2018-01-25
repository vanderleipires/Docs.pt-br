---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: "Criando páginas de ajuda para o ASP.NET Web API | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2013
ms.topic: article
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: 37fd26ebaea192cb540c443eff8a07343ab8c15b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="creating-help-pages-for-aspnet-web-api"></a>Criando páginas de ajuda para a API da Web do ASP.NET
====================
por [Mike Wasson](https://github.com/MikeWasson)

Quando você cria uma API da web, geralmente é útil criar uma página de Ajuda, para que outros desenvolvedores saibam como chamar sua API. Você pode criar toda a documentação manualmente, mas é melhor gerar tanto quanto possível.

Para facilitar essa tarefa, a API Web do ASP.NET fornece uma biblioteca em tempo de execução para a geração automática de páginas de Ajuda.

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a>Criando páginas de Ajuda da API

Instalar [2012.2 atualização das ferramentas de ASP.NET e Web](https://go.microsoft.com/fwlink/?LinkId=282650). Esta atualização se integra a páginas de ajuda para o modelo de projeto de API da Web.

Em seguida, crie um novo projeto ASP.NET MVC 4 e selecione o modelo de projeto de API da Web. O modelo de projeto cria um controlador de API de exemplo chamado `ValuesController`. O modelo também cria as páginas de Ajuda da API. Todos os arquivos de código para a página de ajuda são colocados na pasta áreas do projeto.

![](creating-api-help-pages/_static/image2.png)

Quando você executa o aplicativo, a home page contém um link para a página de Ajuda da API. Na página inicial, o caminho relativo é /Help.

![](creating-api-help-pages/_static/image3.png)

Este link o leva para uma página de resumo de API.

![](creating-api-help-pages/_static/image4.png)

O modo de exibição do MVC para essa página é definido em Areas/HelpPage/Views/Help/Index.cshtml. Você pode editar essa página para modificar o layout, introdução, título, estilos e assim por diante.

A parte principal da página é uma tabela de APIs, agrupados pelo controlador. As entradas da tabela são geradas dinamicamente, usando o **IApiExplorer** interface. (Falarei mais sobre essa interface posteriormente.) Se você adicionar um novo controlador de API, a tabela é atualizada automaticamente em tempo de execução.

A coluna "API" lista o URI relativo e método HTTP. A coluna "Descrição" contém a documentação para cada API. Inicialmente, a documentação é apenas o texto de espaço reservado. Na próxima seção, mostrarei como adicionar a documentação de comentários XML.

Cada API tem um link para uma página com informações mais detalhadas, incluindo corpos de solicitação e resposta de exemplo.

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a>Adição de páginas de ajuda para um projeto existente

Você pode adicionar páginas de ajuda para um projeto de API da Web existente usando o NuGet Package Manager. Essa opção é útil para que iniciar de um modelo de projeto diferente que o modelo "API Web".

Do **ferramentas** menu, selecione **Gerenciador de biblioteca de pacote**e, em seguida, selecione **Package Manager Console**. No [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) janela, digite um dos seguintes comandos:

Para uma **c#** aplicativo:`Install-Package Microsoft.AspNet.WebApi.HelpPage`

Para uma **Visual Basic** aplicativo:`Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`

Há dois pacotes, uma para c# e outra para o Visual Basic. Certifique-se de usar que corresponde a seu projeto.

Esse comando instala os assemblies necessários e adiciona as exibições do MVC para as páginas de Ajuda (localizadas na pasta áreas/HelpPage). Você precisará adicionar manualmente um link para a página de Ajuda. O URI é /Help. Para criar um link em um modo de exibição razor, adicione o seguinte:

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

Além disso, certifique-se de registrar áreas. No arquivo global. asax, adicione o seguinte código para o **aplicativo\_iniciar** método, se ainda não estiver lá:

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a>Adicionando documentação da API

Por padrão, a Ajuda páginas têm cadeias de caracteres de espaço reservado para documentação. Você pode usar [comentários de documentação XML](https://msdn.microsoft.com/library/b2s063f7.aspx) para criar a documentação. Para habilitar esse recurso, abra o arquivo HelpPage/áreas/aplicativo\_Start/HelpPageConfig.cs e remova os comentários da linha a seguir:

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

Ativar agora documentação XML. No Gerenciador de soluções, clique com o botão direito e selecione **propriedades**. Selecione o **criar** página.

![](creating-api-help-pages/_static/image6.png)

Em **saída**, verifique **arquivo de documentação XML**. Na caixa de edição, digite "aplicativo\_Data/XmlDocument.xml".

![](creating-api-help-pages/_static/image7.png)

Em seguida, abra o código para o `ValuesController` controlador API, que é definido em /Controllers/ValuesControler.cs. Adicione alguns comentários de documentação para os métodos do controlador. Por exemplo:

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> Dica: Se você posiciona o cursor na linha acima do método e digite as três barras, Visual Studio insere automaticamente os elementos XML. Em seguida, você pode preencher os espaços em branco.


Agora, criar e executar o aplicativo novamente e navegue até as páginas de Ajuda. As cadeias de caracteres de documentação devem aparecer na tabela de API.

![](creating-api-help-pages/_static/image8.png)

A página de Ajuda lê as cadeias de caracteres do arquivo XML em tempo de execução. (Quando você implanta o aplicativo, verifique se implantar o arquivo XML.)

## <a name="under-the-hood"></a>Nos bastidores

As páginas de ajuda são criadas sobre o **ApiExplorer** classe, que é parte da estrutura de API da Web. O **ApiExplorer** classe fornece as matérias-primas para criar uma página de Ajuda. Para cada API **ApiExplorer** contém um **ApiDescription** que descreve a API. Para essa finalidade, "API" é definida como a combinação de método HTTP e o URI relativo. Por exemplo, aqui estão algumas APIs distintos:

- OBTER /api/Products
- OBTER /api/produtos / {id}
- Lançar/api/produtos

Se uma ação do controlador oferece suporte a vários métodos HTTP, o **ApiExplorer** trata cada método como uma API distinta.

Para ocultar uma API do **ApiExplorer**, adicione o **ApiExplorerSettings** para a ação e o conjunto de atributos *IgnoreApi* como true.

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

Você também pode adicionar esse atributo para o controlador, para excluir o controlador de inteiro.

A classe ApiExplorer obtém cadeias de caracteres de documentação do **IDocumentationProvider** interface. Como você viu anteriormente, a biblioteca de páginas de Ajuda fornece um **IDocumentationProvider** que obtém a documentação de cadeias de caracteres de documentação XML. O código está localizado em /Areas/HelpPage/XmlDocumentationProvider.cs. Você pode obter documentação de outra origem, escrevendo seu próprio **IDocumentationProvider**. Para fazer a conexão, chame o **SetDocumentationProvider** definido no método de extensão, **HelpPageConfigurationExtensions**

**ApiExplorer** chama automaticamente o **IDocumentationProvider** interface para obter cadeias de caracteres de documentação para cada API. Ele os armazenará no **documentação** propriedade o **ApiDescription** e **ApiParameterDescription** objetos.

## <a name="next-steps"></a>Próximas etapas

Você não está limitado a páginas de ajuda mostradas aqui. Na verdade, **ApiExplorer** não está limitado a criar páginas de Ajuda. Yao Huang Lin gravou que postagens no blog alguns ótimo para ajudá-lo a pensar fora da caixa:

- [Adicionar um cliente de teste simples para a página de Ajuda do ASP.NET Web API](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [Fazendo a página de Ajuda do ASP.NET Web API funcionam em serviços de hospedagem interna](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [Geração de tempo de design de ajuda página (ou cliente) para API Web do ASP.NET](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [Personalizações de página de ajuda avançadas](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
