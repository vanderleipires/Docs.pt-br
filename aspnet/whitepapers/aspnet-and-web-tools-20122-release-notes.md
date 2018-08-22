---
uid: whitepapers/aspnet-and-web-tools-20122-release-notes
title: ASP.NET e Web Tools 2012.2 notas de versão | Microsoft Docs
author: rick-anderson
description: Notas de versão do ASP.NET e Web Tools 2012.2.
ms.author: riande
ms.date: 02/14/2013
ms.assetid: bdb18d02-9f61-4676-836d-6fdea94f9282
msc.legacyurl: /whitepapers/aspnet-and-web-tools-20122-release-notes
msc.type: content
ms.openlocfilehash: 50e96251f8add00f70193977e73f9af194571c49
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830027"
---
<a name="aspnet-and-web-tools-20122-release-notes"></a>Notas de versão do ASP.NET and Web Tools 2012.2
====================
> Este documento descreve a versão do ASP.NET e Web Tools 2012.2. É uma atualização para as ferramentas do Visual Studio Web e ASP.NET.


- [Notas de instalação](#_Installation)
- [Documentação](#_Documentation)
- [Suporte](#_Support)
- [Requisitos de software](#_Software_Requirements)
- [Novos recursos do ASP.NET and Web Tools 2012.2](#_New_Features_in)

    - [Ferramentas](#_Tooling)
    - [Publicação na Web](#_Web_Publishing)
    - [Modelos do ASP.NET MVC](#_Templates)
    - [API Web ASP.NET](#_ASP.NET_Web_API)

    - [SignalR do ASP.NET](#_ASP.NET_SignalR)
    - [URLs amigáveis do ASP.NET](#_ASP.NET_Friendly_URLs)
- [Problemas conhecidos e alterações recentes](#_Known_Issues_and)

<a id="_Installation"></a>
## <a name="installation-notes"></a>Notas de instalação

ASP.NET e Web Tools 2012.2 para Visual Studio 2012 podem ser instalado usando [Web Platform installer](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2). Esta é uma atualização para o Visual Studio 2012 ou Visual Studio Express 2012 para Web, que é necessária. Se você não tiver instalado o Visual Studio, Visual Studio Express 2012 para Web será instalado.

Você também pode instalar o ASP.NET e Web Tools 2012.2 manualmente. Você deve ter o Visual Studio 2012 ou Visual Studio Express 2012 para Web instalada. Em seguida, use as instruções a seguir: 

1. Baixe [ASP.NET e Web Frameworks 2012.2](https://download.microsoft.com/download/6/5/6/6562AFBE-9503-4E64-970C-1427133FCD73/AspNetWebTools2012Setup.exe) instalador do Centro de Download.
2. Quando executado clique solicitada. Você também pode salvar o arquivo para executá-lo mais tarde.
3. Verifique se a versão do Visual Studio, você atualizará. Você pode fazer isso iniciando o Visual Studio que você deseja atualizar. Clique no item de menu de Ajuda.   
    ![](aspnet-and-web-tools-20122-release-notes/_static/image1.jpg)
4. Se você vir o item de menu &quot;sobre o Microsoft Visual Studio 2012 para Web&quot; , em seguida, baixe [Web Developer Tools 2012.2 - Visual Studio Express 2012 para Web](https://go.microsoft.com/fwlink/?LinkID=282228). Caso contrário, baixe [Web Developer Tools 2012.2 - Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228).
5. Quando executado clique solicitada. Você também pode salvar o arquivo para executá-lo mais tarde.

> [!NOTE]
> Versão do ASP.NET e Web Tools 2012.2 não inclui o SQL Server Data Tools. SQL Server e bancos de dados SQL do Windows Azure fornece um conjunto mais rico de banco de dados incluindo desenvolvimento de projeto com suporte offline, comparação de esquema e os recursos de implantação de banco de dados avançado de ferramentas. Para obter mais informações ou instalar o SQL Server Data Tools, visite [ https://go.microsoft.com/fwlink/?LinkID=237127 ](https://go.microsoft.com/fwlink/?LinkID=237127).

<a id="_Documentation"></a>
## <a name="documentation"></a>Documentação

Tutoriais e outras informações sobre o ASP.NET e Web Tools 2012.2 estão disponíveis no site da web ASP.NET ( https://www.asp.net).

<a id="_Support"></a>
## <a name="support"></a>Suporte

ASP.NET e Web Tools 2012.2 está oficialmente lançado e suporte. Você pode usar o canal de suporte normais. Você também pode postar perguntas nos fóruns do ASP.NET ([https://forums.asp.net/](https://forums.asp.net/)), onde os membros da comunidade do ASP.NET são com frequência pode oferecer suporte informal.

<a id="_Software_Requirements"></a>
## <a name="software-requirements"></a>Requisitos de software

O ASP.NET e Web Tools 2012.2 requer o Visual Studio 2012 ou Visual Studio Express 2012 para Web.

<a id="_New_Features_in"></a>
## <a name="new-features-in-aspnet-and-web-tools-20122"></a>Novos recursos do ASP.NET and Web Tools 2012.2

Esta seção descreve os recursos que foram introduzidos na versão do ASP.NET e Web Tools 2012.2.

<a id="_Tooling"></a>
### <a name="tooling"></a>Ferramentas

- Inspetor de Página 

    - Suporte ao mapeamento de seleção de JavaScript, permitindo que o Inspetor de página Mapear itens que foram adicionados dinamicamente para a página de volta para o código JavaScript correspondente.
    - A capacidade de ver atualizações CSS em tempo real.
    - Para obter mais informações, leia [sincronização automática de CSS e JavaScript de seleção de mapeamento no Inspetor de página](https://blogs.msdn.com/b/webdev/archive/2012/12/14/css-auto-sync-and-javascript-selection-mapping-in-page-inspector.aspx).
- Editor 

    - Suporte para realce de sintaxe para CoffeeScript, Mustache, Handlebars e JsRender.
    - O editor de HTML fornece o Intellisense para associações do Knockout.
    - Suportam a menos de edição e o compilador para habilitar a criação de CSS dinâmico usando o menor.
    - Colar JSON como uma classe do .NET. Usar este comando Colar especial para colar JSON em um c# ou VB.NET arquivo de código e o Visual Studio gerará automaticamente inferidas do JSON de classes do .NET.
- Suporte do emulador móvel adiciona ganchos de extensibilidade para que os emuladores de terceiros podem ser instalados como um VSIX. Os emuladores instalados serão exibida na lista suspensa F5, para que os desenvolvedores podem visualizar seus sites em uma variedade de dispositivos móveis. Leia mais sobre esse recurso na entrada no blog de Hanselman [a nova integração BrowserStack com o Visual Studio](http://www.hanselman.com/blog/CrossBrowserDebuggingIntegratedIntoVisualStudioWithBrowserStack.aspx).

<a id="_Web_Publishing"></a>
### <a name="web-publishing"></a>Publicação na Web

- Projetos de site da Web agora tem a mesma experiência de publicação que projetos de aplicativo Web, incluindo a publicação para o Windows Azure Web Sites.
- Publicação seletiva &#8211; para um ou mais arquivos, você pode executar as ações a seguir (após a publicação para um ponto de extremidade de implantação da Web): 

    - Publica arquivos selecionados.
    - Veja a diferença entre um arquivo local e um arquivo remoto.
    - Atualize o arquivo local com o arquivo remoto ou atualize o arquivo remoto com o arquivo local.

<a id="_Templates"></a>
### <a name="aspnet-mvc-templates"></a>Modelos do ASP.NET MVC

- O novo modelo de aplicativo do Facebook facilita a criação de aplicativos fáceis de Canvas do Facebook. Em poucas etapas simples, você pode criar um aplicativo do Facebook que obtém dados de um usuário conectado e integra-se com seus amigos. O modelo inclui uma nova biblioteca para cuidar de todo o encanamento envolvido na criação de um aplicativo do Facebook, incluindo autenticação, permissões, acesso aos dados do Facebook e muito mais. Para obter mais informações sobre como usar o modelo de aplicativo do Facebook, consulte [ https://go.microsoft.com/fwlink/?LinkID=269921 ](https://go.microsoft.com/fwlink/?LinkID=269921).
- Um novo modelo MVC de aplicativo de página única permite aos desenvolvedores criar aplicativos da web interativos do lado do cliente usando HTML 5, CSS 3 e o Knockout popular e bibliotecas JavaScript jQuery, além da API Web ASP.NET. O modelo inclui um aplicativo de lista "todo" que demonstra práticas comuns para a criação de um aplicativo JavaScript HTML5 que usa um API RESTful do servidor. Você pode ler mais em [ https://www.asp.net/single-page-application ](../single-page-application/index.md).
- Agora você pode criar um VSIX que adiciona novos modelos à caixa de diálogo Novo projeto do ASP.NET MVC. Saiba como aqui: [https://go.microsoft.com/fwlink/?LinkId=275019](https://go.microsoft.com/fwlink/?LinkId=275019)
- Pacote de FixedDisplayModes &#8211; modelos de projeto MVC foram atualizados para incluir o novo pacote do NuGet 'FixedDisplayModes', que contém uma solução alternativa para um bug no MVC 4. Para obter mais informações sobre a correção contida no pacote, consulte esta postagem de blog ([https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)) da equipe do MVC.

<a id="_ASP.NET_Web_API"></a>
### <a name="aspnet-web-api"></a>API da Web do ASP.NET

API Web ASP.NET foi aprimorada com vários recursos novos:

- ASP.NET Web API OData
- Rastreamento de API Web ASP.NET
- Página de Ajuda da API Web ASP.NET

#### <a name="aspnet-web-api-odata"></a>ASP.NET Web API OData

ASP.NET Web API OData lhe dá a flexibilidade que necessária para criar pontos de extremidade de OData com lógica de negócios avançada ao longo de qualquer fonte de dados. Com o ASP.NET Web API OData você controla a quantidade de semântica de OData que você deseja expor. ASP.NET Web API OData está incluído com os modelos de projeto do ASP.NET MVC 4 e também está disponível no NuGet ([http://www.nuget.org/packages/microsoft.aspnet.webapi.odata](http://www.nuget.org/packages/microsoft.aspnet.webapi.odata)).

ASP.NET Web API OData atualmente suporta os seguintes recursos:

- Habilite a semântica de consulta OData aplicando o atributo [Queryable].
- Validar consultas OData e restringir o conjunto de opções de consulta com suporte, operadores e funções facilmente.
- Associação de parâmetro ao ODataQueryOptions diretamente para obter uma representação de árvore de sintaxe abstrata da consulta que pode ser validada e aplicada a um IQueryable ou IEnumerable.
- Habilite paginação controlados por serviço e a próxima geração de link de página especificando limites de resultado de atributo [Queryable].
- Solicite uma contagem embutida do número total de recursos correspondentes usando $inlinecount.
- Controle de propagação nula.
- Qualquer/todos os operadores em $filter.
- Inferir um modelo de dados de entidade por convenção, ou explicitamente personalizar um modelo de maneira semelhante para o Entity Framework Code-First.
- Conjuntos de entidades de expor por meio da derivação do EntitySetController.
- Convenções de simples e personalizáveis para expor as propriedades de navegação, manipular os links e implementar as ações de OData.
- Simplificado roteamento usando o método de extensão MapODataRoute.
- Suporte para controle de versão, expondo vários modelos EDM.
- Expor $metadata e documento de serviço para que você pode gerar clientes (.NET, Windows Phone, Windows Store, etc.) para sua API da Web.
- Suporte para os formatos de detalhado OData Atom, JSON e JSON.
- Criar, atualizar, parcialmente atualizar (PATCH) e excluir entidades.
- Consultar e manipular as relações entre entidades.
- Crie os links do relacionamento de transmissão até suas rotas.
- Tipos complexos.
- Herança de tipo de entidade.
- Propriedades da coleção.
- Enums.
- Ações de OData.
- Criado com base no mesmo como o WCF Data Services, ou seja, ODataLib ([http://www.nuget.org/packages/microsoft.data.odata](http://www.nuget.org/packages/microsoft.data.odata)).

Para obter mais informações sobre o ASP.NET Web API OData, consulte [ https://go.microsoft.com/fwlink/?LinkId=271141 ](https://go.microsoft.com/fwlink/?LinkId=271141).

#### <a name="aspnet-web-api-tracing"></a>Rastreamento de API Web ASP.NET

Rastreamento de API Web ASP.NET integra dados de rastreamento de suas APIs web com o rastreamento do .NET. Agora, ele é habilitado por padrão no modelo de projeto de API da Web. Rastreamento de dados para sua web APIs é enviada para a janela de saída e é disponibilizada por meio do IntelliTrace. Rastreamento do ASP.NET Web API permite que você a rastrear as informações sobre sua API da Web quando hospedado no Windows Azure por meio da integração com [diagnóstico do Windows Azure](https://msdn.microsoft.com/library/windowsazure/hh411529.aspx). Você também pode instalar e habilitar o rastreamento do ASP.NET Web API em qualquer aplicativo usando o pacote NuGet de rastreamento do ASP.NET Web API ([http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing](http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing)).

Para obter mais informações sobre como configurar e usar o rastreamento do ASP.NET Web API, consulte [ https://go.microsoft.com/fwlink/?LinkID=269874 ](https://go.microsoft.com/fwlink/?LinkID=269874).

#### <a name="aspnet-web-api-help-page"></a>Página de Ajuda da API Web ASP.NET

A página de Ajuda do ASP.NET Web API agora está incluída por padrão no modelo de projeto de API da Web. A página de Ajuda do ASP.NET Web API gera automaticamente a documentação para APIs, incluindo os pontos de extremidade HTTP, os métodos HTTP suportados, parâmetros e cargas de mensagem de solicitação e resposta de exemplo da web. Documentação automaticamente retirada do comentários em seu código. Você também pode adicionar a página de Ajuda do ASP.NET Web API para qualquer aplicativo usando o pacote NuGet de Page Ajuda do ASP.NET Web API ([http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage](http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage)).

Para obter mais informações sobre como configurar e personalizar o consulte a página de Ajuda do ASP.NET Web API [ https://go.microsoft.com/fwlink/?LinkId=271140 ](https://go.microsoft.com/fwlink/?LinkId=271140).

<a id="_ASP.NET_SignalR"></a>
### <a name="aspnet-signalr"></a>ASP.NET SignalR

SignalR do ASP.NET torna simples a adição de recursos da web em tempo real ao seu aplicativo ASP.NET, usando WebSockets, se disponível e automaticamente fazer fallback para outras técnicas quando não for.

Para obter mais informações sobre como usar o SignalR do ASP.NET, consulte [ https://go.microsoft.com/fwlink/?LinkId=271271 ](https://go.microsoft.com/fwlink/?LinkId=271271).

<a id="_ASP.NET_Friendly_URLs"></a>
### <a name="aspnet-friendly-urls"></a>URLs amigáveis do ASP.NET

FriendlyURLs ASP.NET facilita muito para desenvolvedores de formulários da web gerar o Limpador procurando URLs (sem a extensão. aspx). Ele requer pouca ou nenhuma configuração e pode ser usado com aplicativos ASP.NET v4.0. O recurso de FriendlyURLs também torna mais fácil para os desenvolvedores adicionar suporte móvel aos seus aplicativos, oferecendo suporte a alternar entre modos de exibição da área de trabalho e móveis.

Para obter mais informações sobre como instalar e usar URLs amigáveis do ASP.NET, consulte [ http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx ](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx).

<a id="_Known_Issues_and"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemas conhecidos e alterações recentes

Esta seção descreve problemas conhecidos e alterações significativas que estão na versão do ASP.NET e Web Tools 2012.2.

### <a name="installation-issues"></a>Problemas de Instalação

#### <a name="out-of-order-installs-of-visual-studio-2012"></a>Instala fora de ordem do Visual Studio 2012

Instalando um adicionais SKU do Visual Studio 2012 depois de instalar o ASP.NET e Web Tools 2012.2 exigirá uma operação de reparo. Considere a seguinte sequência:

1. Instalar o Visual Studio 2012 Express para Web
2. Instalar o ASP.NET and Web Tools 2012.2
3. Instalar o Visual Studio 2012 Professional, Premium ou Ultimate

Etapa 2 só pode resultar em instalando atualizações para o Express para Web. Para garantir que a SKU adicional instalada durante a etapa 3 contém a atualização, você precisará reparar o ASP.NET e Web Tools 2012.2 para instalar as atualizações para a última SKU instalada. Isso também se aplica se os SKUs na etapa 1 e 3 são revertidas.

#### <a name="installing-microsoft-aspnet-and-web-tools-20122-when-visual-studio-is-open"></a>Instalando o Microsoft ASP.NET e Web Tools 2012.2 quando o Visual Studio é aberto

Se o VS for aberto durante a instalação do Microsoft ASP.NET e Web Tools 2012.2, Visual Studio pode terminar em um estado inválido. É recomendável que os usuários fechem todas as instâncias do Visual Studio antes de prosseguir com a instalação.

#### <a name="canceling-aspnet-and-web-tools-20122-setup-in-the-middle-of-installation"></a>Cancelando a instalação do ASP.NET e Web Tools 2012.2 no meio da instalação

Cancelando o ASP.NET e Web Tools 2012.2 instalação no meio da instalação deixará o Visual Studio em um estado inválido. Para tratar deste problema, siga estas etapas: 

- Vá para adicionar ou remover programas
- Desinstale o Microsoft ASP.NET e Web Tools 2012.2, se estiver presente.
- Reinstale o Microsoft ASP.NET e Web Tools 2012.2

#### <a name="after-uninstalling-aspnet-and-web-tools-20122-the-aspnet-mvc-4-templates-and-razor-v2-web-site-templates-are-missing"></a>Depois de desinstalar o ASP.NET e Web Tools 2012.2 o ASP.NET MVC 4 modelos e modelos de Site da Web do Razor v2 estão ausentes

Desinstalar o ASP.NET e Web Tools 2012.2 também desinstalará todos os ASP.NET MVC 4 e modelos de Site da Web do Razor v2 do Visual Studio 2012.

A solução alternativa é reparar sua instalação do Visual Studio 2012 para reinstalar o ASP.NET MVC 4 e modelos de Site da Web do Razor v2.

### <a name="tooling-issues"></a>Problemas de ferramentas

#### <a name="nuget-error-reported-during-project-creation"></a>Erro do NuGet relatado durante a criação do projeto

Depois de instalar o ASP.NET e Web Tools 2012.2 você poderá ver o seguinte erro durante a criação de um projeto MVC 4

![](aspnet-and-web-tools-20122-release-notes/_static/image1.png)

O ASP.NET e Web Tools 2012.2 fornecido 2.1 do NuGet e atualizará a extensão no Visual Studio 2012. Em alguns casos, o instalador do VSIX falhará atualizar corretamente o VSIX. As etapas a seguir permitirá que você resolva esse problema:

1. Inicie o Visual Studio 2012 como administrador
2. Vá para ferramentas -&gt;extensões e atualizações e desinstalar o NuGet.
3. Feche o Visual Studio
4. Navegue até a pasta de instalação do ASP.NET e Web Tools 2012.2:

    1. Para o Visual Studio 2012: **programar Files\Microsoft ASP.NET\ASP.NET Web Stack\Visual Studio 2012**
    2. Para o Visual Studio 2012 Express para Web: **Program Files\Microsoft ASP.NET\ASP.NET Web Stack\Visual Studio Express 2012 para Web**
5. Clique duas vezes o NuGet.Tools.vsix para reinstalar o NuGet

### <a name="web-api-issues"></a>Problemas de API da Web

#### <a name="parsing-issues-in-filter-and-datetime-literals"></a>Analisar problemas em $filter e literais de data e hora

O analisador de URI do OData não pode analisar corretamente o literais de data e hora parcial. Por exemplo, $filter = data e hora de início eq'2012-12-31T12:00' Falha ao ser analisada corretamente. Uma solução alternativa é usar o completo literal, $filter = data e hora de início eq'2012-12-31T12:00:00'.

#### <a name="odata-doesnt-support-case-insensitive-property-names"></a>OData não dá suporte a nomes de propriedade diferencia maiusculas de minúsculas.

OData não dá suporte a nomes de propriedade diferencia maiusculas de minúsculas em consultas de OData e o caminho odata. Consulte itens de trabalho:

- [http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366)
- [http://aspnetwebstack.codeplex.com/workitem/704](http://aspnetwebstack.codeplex.com/workitem/704)

Se os usuários tiverem maiusculas e minúsculas diferentes no lado do cliente javascript e do lado do servidor, eles provavelmente encontrará esse problema. Esse problema ocorre por design no protocolo odata. No entanto, muitos usuários relata esse problema. Solução alternativa para ele, os usuários precisam corrigir seus casos de URL.

#### <a name="default-odata-routing-conventions-doesnt-support-postput-on-navigation-property"></a>OData padrão convenções de roteamento não dá suporte a PUT/POST na propriedade de navegação.

OData padrão convenções de roteamento não dá suporte a PUT/POST na propriedade de navegação. Consulte o item de trabalho [ http://aspnetwebstack.codeplex.com/workitem/366 ](http://aspnetwebstack.codeplex.com/workitem/366). Nós não têm essa convenção comumente usada nas convenções de padrão.

Para resolvê-lo, os usuários precisam estender a nova convenção de roteamento para dar suporte a ele.

### <a name="facebook-template-issues"></a>Problemas de modelo do Facebook

#### <a name="facebook-application-template-only-works-using-net-45"></a>Modelo de aplicativo do Facebook só funciona usando o .NET 4.5

Você deve selecionar o .NET 4.5 na lista suspensa estrutura na caixa de diálogo Novo projeto para ver o modelo de aplicativo do Facebook no ASP.NET MVC 4.

#### <a name="real-time-update-controller"></a>Controlador de atualização em tempo real

O modelo de aplicativo do Facebook permite que usuário facilmente criar um controlador de API da Web para lidar com atualizações em tempo real do Facebook. Se seu computador de desenvolvimento estiver atrás de NAT, o controlador pode não funcionar sem qualquer configuração de rede. Para obter detalhes, consulte aqui: [http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook](http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook)

#### <a name="query-string-values-conflict-with-facebook-oauth-parameters"></a>Consulta de valores de cadeia de caracteres estão em conflito com os parâmetros de OAuth do Facebook

Os campos a seguir em conflito com a chamada da caixa de diálogo OAuth do Facebook URL de volta. Não adicione seus próprios valores de cadeia de caracteres de consulta com os seguintes nomes: código de erro, erro\_descrição, o erro\_motivo.

#### <a name="using-page-inspector-with-facebook-template"></a>Usando o Inspetor de página com o modelo para Facebook

Você não pode usar o recurso de Inspetor de página no Visual Studio 2012 durante a depuração de seu aplicativo do Facebook. O Page Inspector atualmente não dá suporte a iframes.

### <a name="single-page-application-template-issues"></a>Problemas de modelo de aplicativo de página única

#### <a name="with-jquery-19knockout-221-update-when-running-default-mvc-spa-project-new-todo-item-edit-enter-focus-event-is-not-handled-properly"></a>Com o JQuery atualização 1.9/Knockout 2.2.1, ao executar o projeto de SPA MVC padrão, novo Editar item de tarefas pendentes inserir evento de foco não é tratado corretamente.

Com o JQuery 1.9/Knockout 2.2.1 atualizar, ao executar o projeto de SPA MVC padrão, novo Editar item de tarefas pendentes insira não foco volta para a nova caixa de edição de item de todo depois de inserir o novo item de tarefas à lista de tarefas.

A referência de solução alternativa [ http://knockoutjs.com/documentation/hasfocus-binding.html ](http://knockoutjs.com/documentation/hasfocus-binding.html)e faça a correção semelhante ao código de exemplo a seguir:

Arquivo todo.model.js  
 função todolist(data), adicionar seguintes:  
 **self.isSelected = ko.observable(false);**

função todoList.prototype.addTodo, adicione o seguinte texto blacked:  
 **self.isSelected(true);**  
 self.newTodoTitle(&quot;&quot;);

Arquivo index. cshtml, adicione o seguinte texto blacked:  
 &lt;associar dados de formulário =&quot;enviar: addTodo&quot;&gt;  
 &lt;classe de entrada =&quot;addTodo&quot; tipo =&quot;texto&quot; data-bind =&quot;valor: newTodoTitle, espaço reservado: 'Type aqui para adicionar', blurOnEnter: true, **hasfocus: isSelected**, evento: {desfoque: addTodo}&quot; /&gt;  
 &lt;/Form&gt;
