---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: Criando páginas de ajuda para API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 04/01/2013
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: 2d8758034ca4339ed7e9699cf2f2643bfab87ba4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834081"
---
<a name="creating-help-pages-for-aspnet-web-api"></a>Criando páginas de ajuda para API Web ASP.NET
====================
por [Mike Wasson](https://github.com/MikeWasson)

Quando você cria uma API da web, muitas vezes é útil criar uma página de Ajuda, para que outros desenvolvedores saibam como chamar sua API. Você pode criar toda a documentação manualmente, mas é melhor usado para gerar automaticamente tanto quanto possível.

Para facilitar essa tarefa, a API Web do ASP.NET fornece uma biblioteca em tempo de execução para a geração automática de páginas de Ajuda.

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a>Criando páginas de Ajuda da API

Instale [ASP.NET e Web Tools 2012.2 atualização](https://go.microsoft.com/fwlink/?LinkId=282650). Essa atualização integra-se a páginas de ajuda para o modelo de projeto de API da Web.

Em seguida, crie um novo projeto ASP.NET MVC 4 e selecione o modelo de projeto de API da Web. O modelo de projeto cria um controlador de API de exemplo chamado `ValuesController`. O modelo também cria as páginas de Ajuda da API. Todos os arquivos de código para a página de ajuda são colocados na pasta áreas do projeto.

![](creating-api-help-pages/_static/image2.png)

Quando você executa o aplicativo, a página inicial contém um link para a página de Ajuda da API. Na home page, o caminho relativo é /Help.

![](creating-api-help-pages/_static/image3.png)

Este link leva você para uma página de resumo da API.

![](creating-api-help-pages/_static/image4.png)

A exibição do MVC para essa página é definida em Areas/HelpPage/Views/Help/Index.cshtml. Você pode editar esta página para modificar o layout, introdução, título, estilos e assim por diante.

A parte principal da página é uma tabela de APIs, agrupadas pelo controlador. As entradas da tabela são geradas dinamicamente, usando o **IApiExplorer** interface. (Falarei mais sobre essa interface posteriormente.) Se você adicionar um novo controlador de API, a tabela é automaticamente atualizada em tempo de execução.

A coluna de "API" lista o método HTTP e o URI relativo. A coluna "Descrição" contém a documentação para cada API. Inicialmente, a documentação é apenas o texto de espaço reservado. Na próxima seção, mostrarei como adicionar a documentação de comentários XML.

Cada API tem um link para uma página com informações mais detalhadas, incluindo os corpos de solicitação e resposta de exemplo.

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a>Adicionar páginas de ajuda para um projeto existente

Você pode adicionar páginas de ajuda para um projeto de API da Web existente usando o Gerenciador de pacotes NuGet. Essa opção é útil que começar a partir de um modelo de projeto diferente que o modelo de "API Web".

Dos **ferramentas** menu, selecione **Gerenciador de pacotes de biblioteca**e, em seguida, selecione **Package Manager Console**. No [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) janela, digite um dos seguintes comandos:

Para um **c#** aplicativo: `Install-Package Microsoft.AspNet.WebApi.HelpPage`

Para um **Visual Basic** aplicativo: `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`

Há dois pacotes, um para c# e outro para o Visual Basic. Certifique-se de usar aquele que corresponde ao seu projeto.

Esse comando instala os assemblies necessários e adiciona as exibições do MVC para as páginas de Ajuda (localizadas na pasta áreas/HelpPage). Você precisará adicionar manualmente um link para a página de Ajuda. O URI é /Help. Para criar um link em um modo de exibição do razor, adicione o seguinte:

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

Além disso, certifique-se registrar áreas. No arquivo global. asax, adicione o seguinte código para o **Application\_iniciar** método, se ainda não estiver lá:

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a>Adicionando documentação da API

Por padrão, a Ajuda páginas têm cadeias de caracteres de espaço reservado para a documentação. Você pode usar [comentários da documentação XML](https://msdn.microsoft.com/library/b2s063f7.aspx) para criar a documentação. Para habilitar esse recurso, abra o arquivo HelpPage/aplicativo/áreas\_Start/HelpPageConfig.cs e remova a seguinte linha:

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

Agora habilite a documentação XML. No Gerenciador de soluções, clique com botão direito no projeto e selecione **propriedades**. Selecione o **Build** página.

![](creating-api-help-pages/_static/image6.png)

Sob **saída**, verifique **arquivo de documentação XML**. Na caixa de edição, digite "aplicativo\_Data/XmlDocument.xml".

![](creating-api-help-pages/_static/image7.png)

Em seguida, abra o código para o `ValuesController` controlador da API, que é definido no /Controllers/ValuesControler.cs. Adicione alguns comentários de documentação para os métodos do controlador. Por exemplo:

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> Dica: Se você posiciona o cursor na linha acima do método e digite as três barras "/", o Visual Studio automaticamente insere os elementos XML. Em seguida, você pode preencher os espaços em branco.


Agora, criar e executar o aplicativo novamente e navegue até as páginas de Ajuda. As cadeias de caracteres de documentação devem aparecer na tabela de API.

![](creating-api-help-pages/_static/image8.png)

A página de Ajuda lê as cadeias de caracteres do arquivo XML em tempo de execução. (Quando você implanta o aplicativo, verifique se implantar o arquivo XML.)

## <a name="under-the-hood"></a>Nos bastidores

As páginas de ajuda são criadas sobre o **ApiExplorer** classe, que é parte da estrutura da API Web. O **ApiExplorer** classe fornece matéria-prima para a criação de uma página de Ajuda. Para cada API **ApiExplorer** contém uma **ApiDescription** que descreve a API. Para essa finalidade, "API" é definida como a combinação de URI relativo e o método HTTP. Por exemplo, aqui estão algumas APIs distintas:

- OBTER /api/Products
- OBTER/API/produtos / {id}
- POSTAR/api/produtos

Se uma ação do controlador dá suporte a vários métodos HTTP, o **ApiExplorer** trata cada método como uma API distinta.

Para ocultar uma API do **ApiExplorer**, adicione o **ApiExplorerSettings** atributo para a ação e o conjunto *IgnoreApi* como true.

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

Você também pode adicionar esse atributo para o controlador, para excluir o controlador inteiro.

A classe ApiExplorer obtém cadeias de caracteres de documentação do **IDocumentationProvider** interface. Como você viu anteriormente, a biblioteca de páginas de Ajuda fornece um **IDocumentationProvider** que obtém a documentação de cadeias de caracteres de documentação XML. O código está localizado em /Areas/HelpPage/XmlDocumentationProvider.cs. Você pode obter documentação de outra origem, escrevendo seus próprios **IDocumentationProvider**. Para fazer a conexão, chame o **SetDocumentationProvider** definido no método de extensão, **HelpPageConfigurationExtensions**

**ApiExplorer** chama automaticamente o **IDocumentationProvider** a interface para obter cadeias de caracteres de documentação para cada API. Ele armazena na **documentação** propriedade da **ApiDescription** e **ApiParameterDescription** objetos.

## <a name="next-steps"></a>Próximas etapas

Você não fica limitado às páginas de ajuda mostradas aqui. Na verdade, **ApiExplorer** não está limitado a criar páginas de Ajuda. Yao Huang Lin escreveu que postagens no blog alguns ótimo para ajudá-lo a pensar fora da caixa:

- [Adicionando um cliente de teste simples para a página de Ajuda do ASP.NET Web API](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [Tornando a página de Ajuda do ASP.NET Web API trabalhar em serviços de hospedagem interna](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [Geração de tempo de design da Ajuda page (ou cliente) para API Web ASP.NET](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [Personalizações avançadas da página de ajuda](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
