---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: Notas de versão do ASP.NET e Web Tools 2013.1 para Visual Studio 2012 | Microsoft Docs
author: microsoft
description: Este documento descreve a versão do ASP.NET e Web Tools 2013.1 para Visual Studio 2012.
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2013
ms.topic: article
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: c11e2ef9c33b0cae1f196690533094ce1c342da5
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2018
ms.locfileid: "28036421"
---
<a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>Notas de versão do ASP.NET e Web Tools 2013.1 para Visual Studio 2012
====================
por [Microsoft](https://github.com/microsoft)

> Este documento descreve a versão do ASP.NET e Web Tools 2013.1 para Visual Studio 2012.


## <a name="contents"></a>Conteúdo

- [Notas de instalação](#install)
- [Requisitos de software](#requirements)
- Novos recursos no ASP.NET e Web Tools 2013.1 para Visual Studio 2012

    - [Bootstrap](#bootstrap)
    - [Modelos](#templates)

        - [Modelo do ASP.NET MVC 5](#mvc5template)
        - [Modelo do ASP.NET Web API 2](#apitemplate)
        - [Modelos de item](#itemtemplate)
    - [Entity Framework 6](#ef6)
    - [Estrutura do ASP.NET](#scaffold)
    - [Razor Editor](#razor)
    - [NuGet 2.7](#nuget)
- Problemas conhecidos e as alterações recentes

    - [Estrutura do ASP.NET](#issuescaffolding)

        - [MVC e Web API Scaffolding - HTTP 404 não encontrado erro](#404issue)
        - [O Visual Studio Express 2012 para Web para de funcionar após a adição de um item de scaffolding](#expressissue)
    - [ASP.NET Razor 3](#issuerazor)

        - [Exibir o arquivo cshtml procurar com ou F5 causa um erro de servidor](#browseissue)
        - [Tilde(~) e regravação de Url](#rewriteissue)
    - [Modelos](#templateissue)

<a id="install"></a>
## <a name="installation-notes"></a>Notas de instalação

[Instalar](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET e Web ferramentas 2013.1 para Visual Studio 2012.

<a id="requirements"></a>
## <a name="software-requirements"></a>Requisitos de software

Você deve ter o Visual Studio 2012 ou Visual Studio Express 2012 para Web.

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>Novos recursos no ASP.NET e Web Tools 2013.1 para Visual Studio 2012

<a id="bootstrap"></a>
### <a name="bootstrap"></a>inicialização

Quando você fizer scaffold MVC 5 controladores e exibições, usa a marcação para os modos de exibição [inicialização](http://getbootstrap.com/).

<a id="templates"></a>
### <a name="templates"></a>Modelos

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a>Modelo do ASP.NET MVC 5

Adicionamos um novo modelo MVC 5. Ele faz referência a pacotes NuGet do MVC 5 mais recentes e você pode usar o scaffolding para adicionar controladores e exibições.

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a>Modelo do ASP.NET Web API 2

Adicionamos um novo modelo de API Web 2. Ele faz referência os pacotes de Web API 2 NuGet mais recentes, e você pode usar o scaffolding para adicionar controladores e exibições.

<a id="itemtemplate"></a>
#### <a name="item-templates"></a>Modelos de item

Adicionamos novos modelos de item para modos de exibição MVC 5, páginas da Web (Razor 3) e controladores de API Web 2. Instalar os pacotes do NuGet relacionados ao projeto durante a adição de novos itens.

<a id="ef6"></a>
### <a name="entity-framework-6"></a>Entity Framework 6

Quando você fizer scaffold um controlador MVC ou Web API usando o Entity Framework, usamos o Framework 6. Para obter mais informações sobre o Entity Framework, consulte o [histórico de versão do Entity Framework](https://msdn.com/data/jj574253).

Você também pode baixar e instalar as ferramentas do Entity Framework 6 para o Visual Studio 2012. Consulte o [obter o Entity Framework](https://msdn.com/data/ee712906#tooling).

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>Estrutura do ASP.NET

Estrutura do ASP.NET é uma estrutura de geração de código para aplicativos Web ASP.NET. Torna mais fácil adicionar código clichê ao seu projeto que interage com um modelo de dados.

Nas versões anteriores do Visual Studio, scaffolding foi limitado a projetos do ASP.NET MVC. Com essa atualização, agora você pode usar o scaffolding para qualquer projeto ASP.NET, incluindo formulários da Web. Esta atualização não oferece suporte a gerar páginas para um projeto de formulários da Web, mas você ainda pode usar o scaffolding com formulários da Web Adicionando dependências MVC ao projeto. O suporte para a geração de páginas para formulários da Web será adicionado em uma atualização futura.

Ao usar o scaffolding, podemos garantir que todos os dependências estejam instaladas no projeto. Por exemplo, se você iniciar com um projeto Web Forms do ASP.NET e, em seguida, use o scaffolding para adicionar um controlador de API da Web, os pacotes do NuGet necessários e as referências são adicionadas ao seu projeto automaticamente.

Para adicionar o scaffolding MVC para um projeto de Web Forms, adicione um **Novo Item de Scaffold** e selecione **MVC 5 dependências** na janela de diálogo. Há duas opções para a estrutura MVC; Mínimo e completo. Se você selecionar no mínimo, somente os pacotes NuGet e referências para o ASP.NET MVC são adicionadas ao seu projeto. Se você selecionar a opção completo, as dependências mínimo são adicionadas, bem como os arquivos de conteúdo necessários para um projeto MVC.

Suporte para a estrutura async controladores usa os novos recursos de assíncronas do Entity Framework 6.

Para obter mais informações e tutoriais, consulte [visão geral do ASP.NET Scaffolding](../2013/aspnet-scaffolding-overview.md). Esses tutoriais mostram scaffolding com o Visual Studio 2013, mas eles também são aplicáveis ao ASP.NET e Web 2013.1 de ferramentas para Visual Studio 2012.

<a id="razor"></a>
### <a name="razor-editor"></a>Razor Editor

Com essa atualização, o Visual Studio 2012 agora dá suporte a ferramentas/edição 3 Razor.

<a id="nuget"></a>
### <a name="nuget-27"></a>NuGet 2.7

2.7 NuGet inclui um conjunto avançado de novos recursos que são descritas em detalhes em [notas de versão 2.7 NuGet](http://docs.nuget.org/docs/release-notes/nuget-2.7).

Esta versão do NuGet elimina a necessidade de usuários permitir explicitamente o NuGet para restaurar os pacotes ausentes. Ao instalar o NuGet 2.7, os usuários implicitamente dar consentimento para restaurar automaticamente os pacotes ausentes. Os usuários explicitamente podem recusar a restauração de pacote por meio das configurações do NuGet no Visual Studio. Essa alteração simplifica como funciona a restauração do pacote.

## <a name="known-issues-and-breaking-changes"></a>Problemas conhecidos e as alterações recentes

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a>Estrutura do ASP.NET

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC e Web API Scaffolding - HTTP 404 não encontrado erro

Se você encontrar um erro ao adicionar um item de scaffolding para um projeto, é possível que seu projeto será deixado em um estado inconsistente. Algumas das alterações feitas a ser scaffolding serão revertidas, mas outras alterações, como os pacotes do NuGet instaladas, serão não ser revertidas. Se as alterações de configuração de roteamento são revertidas, os usuários receberão um erro HTTP 404 quando navegar para Scaffold itens.

Para corrigir esse erro para MVC, adicionar um novo item de scaffolding e selecione as dependências do MVC 5 (mínimo ou completo). Esse processo adicionará todas as alterações necessárias ao seu projeto.

Para corrigir esse erro para a API da Web:

1. Adicione a seguinte classe WebApiConfig ao seu projeto.

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. Configure o aplicativo WebApiConfig.Register\_iniciar método em global. asax, da seguinte maneira:

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a>O Visual Studio Express 2012 para Web para de funcionar após a adição de um item de scaffolding

Se o Visual Studio Express 2012 para Web parar de funcionar após a adição de item de scaffolding com o Entity Framework (como Web 2 controlador API com ações, usando o Entity Framework), é possível que o Visual Studio Express não pôde carregar a imagem nativa de um assembly Extensions dependentes.

Para corrigir esse problema, configure o Visual Studio Express para trabalhar com a imagem MSIL de Extensions:

1. Abra o Prompt de comando no modo de administrador.
2. Vá para %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE ou % ProgramFiles(x86) %\Microsoft Visual Studio 11.0\Common7\IDE (para 64 bits Windows).
3. Abra VWDExpress.exe.config em um editor de texto.
4. Adicione a seguinte linha sob o &lt;configuração&gt;/&lt;tempo de execução&gt; elemento:  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. Reinicie o Visual Studio Express 2012 para Web.

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a>ASP.NET Razor 3

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-withbrowse-withorf5causes-a-server-error"></a>Exibindo cshtml arquivo withBrowse WithorF5causes um erro de servidor

Quando você cria um projeto MVC 5 no Visual Studio 2012 (ou abrir no projeto do Visual Studio 2012 um MVC 5 que foi criado no Visual Studio 2013) e tenta exibir um arquivo cshtml usando Procurar com ou F5, você receberá um erro informando - **erro de servidor no Aplicativo '/'**. O servidor tenta navegar para `http://localhost:XXXX/Views/../XXXX.cshtml`

Para resolver esse problema, altere o **iniciar ação** configuração em seu projeto para **página específica**. Você não precisa fornecer um valor para a página.

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

Depois de fazer essa alteração, selecionar F5 navega para a raiz do seu aplicativo (`http://localhost:XXXX`). Esse comportamento não é o mesmo que o comportamento para projetos MVC 5 no Visual Studio 2013, onde o **página atual** configuração inicia a página aberta.

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a>Tilde(~) e regravação de Url

Depois de atualizar para o ASP.NET Razor 3 ou 5 do ASP.NET MVC, a notação tilde(~) pode não funcionar corretamente se você estiver usando regravações de URL. A regravação de URL afeta a notação de tilde(~) em elementos HTML, como &lt;um /&gt;, &lt;SCRIPT /&gt;, &lt;LINK /&gt;, e assim o til não mapeia para o diretório raiz.

Por exemplo, se você reescrever solicitações para **asp.net/content** para **asp.net**, o atributo href na &lt;A href = "~/content/" /&gt; resolve **/content/ conteúdo /** em vez de **/**. Para suprimir essa alteração, você pode definir o **IIS\_WasUrlRewritten** contexto para false em cada página da Web ou em **aplicativo\_BeginRequest** em global. asax.

<a id="templateissue"></a>
### <a name="templates"></a>Modelos

Quando você cria o ASP.NET MVC projetos com o Visual Studio 2012 no Windows 8.1 ou Windows Server 2012 R2, o Visual Studio exibe uma mensagem de erro afirmando que "Configurando Web [url] para ASP.NET 4.5 falhou".

![Erro de configuração](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

Você verá esse erro porque o Visual Studio 2012 não habilita o recurso ASP.NET 4.5 quando ele é instalado nessas versões do Windows. Para habilitar o ASP.NET 4.5, execute as etapas descritas em [ou desativar recursos do Windows ativar](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).

![Ativar ou desativar recursos do Windows](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

Como alternativa, você pode habilitar o ASP.NET 4.5 pela linha de comando.

1. Abra o Prompt de comando no modo de administrador.
2. Execute o seguinte comando para habilitar o ASP.NET 4.5.  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
