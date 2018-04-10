---
uid: whitepapers/aspnet-and-web-tools-20122-release-notes
title: ASP.NET e Web 2012.2 notas de versão das ferramentas | Microsoft Docs
author: rick-anderson
description: Notas de versão do ASP.NET e Web Tools 2012.2.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/14/2013
ms.topic: article
ms.assetid: bdb18d02-9f61-4676-836d-6fdea94f9282
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-and-web-tools-20122-release-notes
msc.type: content
ms.openlocfilehash: 52559a47f86e572f873d4eaaab50e87eb51722fd
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/10/2018
---
<a name="aspnet-and-web-tools-20122-release-notes"></a>Notas de versão do ASP.NET e Web Tools 2012.2
====================
> Este documento descreve a versão do ASP.NET e Web Tools 2012.2. É uma atualização para ferramentas do Visual Studio Web e ASP.NET.


- [Notas de instalação](#_Installation)
- [Documentação](#_Documentation)
- [Suporte](#_Support)
- [Requisitos de software](#_Software_Requirements)
- [Novos recursos no ASP.NET e Web Tools 2012.2](#_New_Features_in)

    - [Ferramentas](#_Tooling)
    - [Publicação na Web](#_Web_Publishing)
    - [Modelos ASP.NET MVC](#_Templates)
    - [API Web ASP.NET](#_ASP.NET_Web_API)

    - [SignalR do ASP.NET](#_ASP.NET_SignalR)
    - [URLs amigáveis do ASP.NET](#_ASP.NET_Friendly_URLs)
- [Problemas conhecidos e as alterações recentes](#_Known_Issues_and)

<a id="_Installation"></a>
## <a name="installation-notes"></a>Notas de instalação

ASP.NET e Web 2012.2 de ferramentas para Visual Studio 2012 podem ser instalado usando [Web Platform installer](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2). Esta é uma atualização para o Visual Studio 2012 ou Visual Studio Express 2012 para Web, que é necessária. Se você não tiver instalado o Visual Studio, o Visual Studio Express 2012 para Web será instalado.

Você também pode instalar o ASP.NET e Web Tools 2012.2 manualmente. Você deve ter o Visual Studio 2012 ou Visual Studio Express 2012 para Web instalada. Em seguida, use as instruções a seguir: 

1. Baixar [ASP.NET e Web Frameworks 2012.2](https://download.microsoft.com/download/6/5/6/6562AFBE-9503-4E64-970C-1427133FCD73/AspNetWebTools2012Setup.exe) instalador do Centro de Download.
2. Quando executado clique solicitada. Você também pode salvar o arquivo para executá-lo mais tarde.
3. Verifique se a versão do Visual Studio, você será atualizado. Você pode fazer isso inicializando o Visual Studio que você deseja atualizar. Clique no item de menu de Ajuda.   
    ![](aspnet-and-web-tools-20122-release-notes/_static/image1.jpg)
4. Se você vir o item de menu &quot;sobre o Microsoft Visual Studio 2012 para Web&quot; Baixe [2012.2 de ferramentas de desenvolvedor da Web - Visual Studio Express 2012 para Web](https://go.microsoft.com/fwlink/?LinkID=282228). Caso contrário, faça o download [2012.2 de ferramentas de desenvolvedor da Web - Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228).
5. Quando executado clique solicitada. Você também pode salvar o arquivo para executá-lo mais tarde.

> [!NOTE]
> Versão do ASP.NET e Web Tools 2012.2 não inclui ferramentas de dados do SQL Server. SQL Server e bancos de dados SQL do Windows Azure fornece um valioso conjunto de ferramentas incluindo desenvolvimento baseado em projeto offline, comparação de esquemas e os recursos de implantação de banco de dados aprimorado de banco de dados. Para obter mais informações ou instalar o SQL Server Data Tools, visite [ https://go.microsoft.com/fwlink/?LinkID=237127 ](https://go.microsoft.com/fwlink/?LinkID=237127).

<a id="_Documentation"></a>
## <a name="documentation"></a>Documentação

Tutoriais e outras informações sobre o ASP.NET e Web Tools 2012.2 estão disponíveis no site da web ASP.NET ( https://www.asp.net).

<a id="_Support"></a>
## <a name="support"></a>Suporte

ASP.NET e Web Tools 2012.2 é oficialmente lançado e suporte. Você pode usar o canal de suporte normais. Você também pode postar perguntas nos fóruns do ASP.NET ([https://forums.asp.net/](https://forums.asp.net/)), onde os membros da comunidade do ASP.NET são frequentemente pode oferecer suporte informal.

<a id="_Software_Requirements"></a>
## <a name="software-requirements"></a>Requisitos de software

O ASP.NET e Web Tools 2012.2 requer o Visual Studio 2012 ou Visual Studio Express 2012 para Web.

<a id="_New_Features_in"></a>
## <a name="new-features-in-aspnet-and-web-tools-20122"></a>Novos recursos no ASP.NET e Web Tools 2012.2

Esta seção descreve os recursos que foram introduzidos na versão ASP.NET e Web Tools 2012.2.

<a id="_Tooling"></a>
### <a name="tooling"></a>Ferramentas

- Inspetor de Página 

    - Suporte para o mapeamento de seleção de JavaScript, permitindo que o Inspetor de página Mapear itens que foram adicionados dinamicamente para a página de volta para o código JavaScript correspondente.
    - A capacidade de ver atualizações CSS em tempo real.
    - Para obter mais informações, leia [sincronização automática de CSS e JavaScript seleção de mapeamento no Page Inspector](https://blogs.msdn.com/b/webdev/archive/2012/12/14/css-auto-sync-and-javascript-selection-mapping-in-page-inspector.aspx).
- Editor 

    - Suporte para CoffeeScript, Mustache, Guidões e JsRender realce de sintaxe.
    - O editor de HTML fornece Intellisense para associações de separação.
    - MENOS edição e o compilador oferecem suporte para habilitar a criação dinâmica CSS usando menor.
    - Colar JSON como uma classe .NET. Usar este comando Colar especial para colar JSON em um c# ou VB.NET arquivo de código e o Visual Studio gerará automaticamente classes .NET inferidos do JSON.
- Suporte a emulador móvel adiciona ganchos de extensibilidade para que os emuladores de terceiros podem ser instalados como um VSIX. Os emuladores instalados aparecerão na lista suspensa F5, para que os desenvolvedores podem visualizar seus sites em uma variedade de dispositivos móveis. Leia mais sobre esse recurso na entrada de blog de Scott Hanselman em [a nova BrowserStack integração com o Visual Studio](http://www.hanselman.com/blog/CrossBrowserDebuggingIntegratedIntoVisualStudioWithBrowserStack.aspx).

<a id="_Web_Publishing"></a>
### <a name="web-publishing"></a>Publicação na Web

- Projetos de site da Web agora têm a mesma experiência de publicação de projetos de aplicativo Web, incluindo a publicação para o Windows Azure Web Sites.
- Publicação seletiva &#8211; para um ou mais arquivos, você pode executar as seguintes ações (após a publicação para um ponto de extremidade de implantação da Web): 

    - Publica arquivos selecionados.
    - Ver a diferença entre um arquivo local e um arquivo remoto.
    - Atualize o arquivo local com o arquivo remoto ou atualizar o arquivo remoto com o arquivo local.

<a id="_Templates"></a>
### <a name="aspnet-mvc-templates"></a>Modelos ASP.NET MVC

- O novo modelo de aplicativo do Facebook facilita a criação de aplicativos fáceis de Canvas do Facebook. Em algumas etapas simples, você pode criar um aplicativo do Facebook que obtém dados de um usuário conectado e integra-se com seus amigos. O modelo inclui uma nova biblioteca para cuidar de todo o encanamento envolvido na criação de um aplicativo do Facebook, incluindo autenticação, permissões, acesso aos dados do Facebook e muito mais. Para obter mais informações sobre como usar o modelo de aplicativo do Facebook, consulte [ https://go.microsoft.com/fwlink/?LinkID=269921 ](https://go.microsoft.com/fwlink/?LinkID=269921).
- Um novo modelo MVC de aplicativo de página única permite aos desenvolvedores compilar aplicativos web interativos de cliente usando HTML 5, CSS 3 e o populares Knockout e jQuery JavaScript bibliotecas, na parte superior da API Web do ASP.NET. O modelo inclui um aplicativo de lista "todo" que demonstra práticas comuns para a criação de um aplicativo JavaScript HTML5 que usa um API RESTful do servidor. Você pode ler mais em [ https://www.asp.net/single-page-application ](../single-page-application/index.md).
- Agora você pode criar um VSIX que adiciona novos modelos para a caixa de diálogo Novo projeto do ASP.NET MVC. Saiba como fazer isso aqui: [https://go.microsoft.com/fwlink/?LinkId=275019](https://go.microsoft.com/fwlink/?LinkId=275019)
- Pacote de FixedDisplayModes &#8211; modelos de projeto MVC foram atualizados para incluir o novo pacote do NuGet 'FixedDisplayModes', que contém uma solução alternativa para um bug no MVC 4. Para obter mais informações sobre a correção contida no pacote, consulte esta postagem de blog ([https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)) da equipe do MVC.

<a id="_ASP.NET_Web_API"></a>
### <a name="aspnet-web-api"></a>API da Web do ASP.NET

API da Web do ASP.NET foi aprimorado com vários recursos novos:

- OData da API da Web ASP.NET
- Rastreamento de API da Web ASP.NET
- Página de Ajuda da API da Web ASP.NET

#### <a name="aspnet-web-api-odata"></a>OData da API da Web ASP.NET

ASP.NET Web API OData fornece a flexibilidade que necessária para criar pontos de extremidade OData com lógica de negócios avançados em qualquer fonte de dados. Com o ASP.NET Web API OData você controlar a quantidade de semântica de OData que você deseja expor. ASP.NET Web API OData é incluído com os modelos de projeto do ASP.NET MVC 4 e também estão disponível do NuGet ([http://www.nuget.org/packages/microsoft.aspnet.webapi.odata](http://www.nuget.org/packages/microsoft.aspnet.webapi.odata)).

ASP.NET Web API OData atualmente suporta os seguintes recursos:

- Habilite a semântica de consulta OData aplicando o atributo [Queryable].
- Validar consultas OData e restringir o conjunto de opções de consulta com suporte, operadores e funções com facilidade.
- Associação de parâmetro para ODataQueryOptions diretamente para obter uma representação de árvore de sintaxe abstrata da consulta que pode ser validada e aplicada a um IQueryable ou IEnumerable.
- Habilite paginação controlada por serviço e a próxima geração de link de página especificando limites de resultado de atributo [Queryable].
- Solicite uma contagem embutida do número total de recursos correspondentes usando $inlinecount.
- Controlar a propagação nula.
- Qualquer/todos os operadores em $filter.
- Inferir um modelo de dados de entidade por convenção, ou explicitamente personalizar um modelo de maneira semelhante para o Entity Framework Code-First.
- Conjuntos de entidades de expor derivando de EntitySetController.
- Convenções de simples e personalizáveis para expor as propriedades de navegação, manipular os links e implementar ações de OData.
- Simplificado roteamento usando o método de extensão MapODataRoute.
- Suporte para controle de versão ao expor vários modelos EDM.
- Expor $metadata e documento de serviço para que você pode gerar clientes (.NET, Windows Phone, Windows Store, etc.) para a API da Web.
- Suporte para os formatos OData Atom, JSON e JSON detalhados.
- Criar, atualizar, parcialmente atualizar (PATCH) e excluir entidades.
- Consultar e manipular relações entre entidades.
- Crie links de relação de transmissão até sua rotas.
- Tipos complexos.
- Herança de tipo de entidade.
- Propriedades da coleção.
- Enums.
- Ações de OData.
- Criado com base no mesmo como o WCF Data Services, ou seja, ODataLib ([http://www.nuget.org/packages/microsoft.data.odata](http://www.nuget.org/packages/microsoft.data.odata)).

Para obter mais informações sobre o ASP.NET Web API OData consulte [ https://go.microsoft.com/fwlink/?LinkId=271141 ](https://go.microsoft.com/fwlink/?LinkId=271141).

#### <a name="aspnet-web-api-tracing"></a>Rastreamento de API da Web ASP.NET

Rastreamento de API da Web ASP.NET integra dados de rastreamento de suas APIs da web com o rastreamento do .NET. Agora, ele é habilitado por padrão no modelo de projeto de API da Web. APIs de rastreamento de dados para a web é enviada para a janela de saída e é disponibilizado por meio do IntelliTrace. Rastreamento do ASP.NET Web API permite rastrear as informações sobre a API da Web quando hospedado no Windows Azure por meio da integração com [diagnóstico do Windows Azure](https://msdn.microsoft.com/library/windowsazure/hh411529.aspx). Você também pode instalar e habilitar o rastreamento do ASP.NET Web API em qualquer aplicativo usando o pacote NuGet de rastreamento do ASP.NET Web API ([http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing](http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing)).

Para obter mais informações sobre como configurar e usar o ASP.NET Web API Tracing consulte [ https://go.microsoft.com/fwlink/?LinkID=269874 ](https://go.microsoft.com/fwlink/?LinkID=269874).

#### <a name="aspnet-web-api-help-page"></a>Página de Ajuda da API da Web ASP.NET

A página de Ajuda do ASP.NET Web API agora está incluída por padrão no modelo de projeto de API da Web. A página de Ajuda do ASP.NET Web API gera automaticamente a documentação de APIs, incluindo os pontos de extremidade HTTP, os métodos HTTP suportados, parâmetros e cargas de mensagem de solicitação e resposta de exemplo da web. Documentação automaticamente é extraída de comentários em seu código. Você também pode adicionar a página de Ajuda do ASP.NET Web API para qualquer aplicativo usando o pacote ASP.NET Web API ajuda Page NuGet ([http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage](http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage)).

Para obter mais informações sobre como configurar e personalizar consulte a página de Ajuda do ASP.NET Web API [ https://go.microsoft.com/fwlink/?LinkId=271140 ](https://go.microsoft.com/fwlink/?LinkId=271140).

<a id="_ASP.NET_SignalR"></a>
### <a name="aspnet-signalr"></a>ASP.NET SignalR

ASP.NET SignalR torna simples para adicionar recursos da web em tempo real para seu aplicativo ASP.NET, usando WebSockets se disponível e automaticamente voltando para outras técnicas quando não estiver.

Para obter mais informações sobre como usar o ASP.NET SignalR consulte [ https://go.microsoft.com/fwlink/?LinkId=271271 ](https://go.microsoft.com/fwlink/?LinkId=271271).

<a id="_ASP.NET_Friendly_URLs"></a>
### <a name="aspnet-friendly-urls"></a>URLs amigáveis do ASP.NET

ASP.NET FriendlyURLs facilita para os desenvolvedores de formulários da web gerar a limpeza procurando URLs (sem a extensão. aspx). Ele requer pouca ou nenhuma configuração e pode ser usado com aplicativos ASP.NET v 4.0. O recurso de FriendlyURLs também torna mais fácil para os desenvolvedores adicionar suporte móvel para seus aplicativos, oferecendo suporte a alternância entre os modos de desktop e mobile.

Para obter mais informações sobre como instalar e usar URLs amigáveis do ASP.NET, consulte [ http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx ](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx).

<a id="_Known_Issues_and"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemas conhecidos e as alterações recentes

Esta seção descreve problemas conhecidos e alterações de quebra a versão do ASP.NET e Web Tools 2012.2.

### <a name="installation-issues"></a>Problemas de Instalação

#### <a name="out-of-order-installs-of-visual-studio-2012"></a>Instala fora de ordem do Visual Studio 2012

Instalando um adicionais SKUS do Visual Studio 2012 após a instalação do ASP.NET e Web Tools 2012.2 exigirá uma operação de reparo. Considere a seguinte sequência:

1. Instalar o Visual Studio 2012 Express para Web
2. Instalar o ASP.NET e Web Tools 2012.2
3. Instalar o Visual Studio 2012 Professional, Premium ou Ultimate

Etapa 2 só pode resultar na instalação de atualizações para o Express para Web. Para garantir que o SKU adicional instalado na etapa 3 contém a atualização, você precisará reparar o ASP.NET e Web Tools 2012.2 para instalar as atualizações para o último SKU instalado. Isso também se aplica se os SKUs na etapa 1 e 3 são revertidas.

#### <a name="installing-microsoft-aspnet-and-web-tools-20122-when-visual-studio-is-open"></a>Instalando o Microsoft ASP.NET e Web Tools 2012.2 quando o Visual Studio é aberto

Se o VS for aberto durante a instalação do Microsoft ASP.NET e Web Tools 2012.2, Visual Studio pode terminar em um estado inválido. É recomendável que os usuários fechem todas as instâncias do Visual Studio antes de continuar com a instalação.

#### <a name="canceling-aspnet-and-web-tools-20122-setup-in-the-middle-of-installation"></a>Cancelando a instalação do ASP.NET e Web Tools 2012.2 no meio de instalação

Cancelando ASP.NET e Web Tools 2012.2 instalação no meio de instalação deixará o Visual Studio em um estado inválido. Para tratar deste problema, siga estas etapas: 

- Vá para adicionar ou remover programas
- Desinstale o Microsoft ASP.NET e Web Tools 2012.2, se presentes.
- Reinstale o Microsoft ASP.NET e ferramentas da Web 2012.2

#### <a name="after-uninstalling-aspnet-and-web-tools-20122-the-aspnet-mvc-4-templates-and-razor-v2-web-site-templates-are-missing"></a>Após a desinstalação do ASP.NET e 2012.2 de ferramentas da Web do ASP.NET MVC 4 modelos e modelos de Site do Razor v2 estão ausentes

Desinstalar o ASP.NET e Web Tools 2012.2 também desinstalará todos ASP.NET MVC 4 e modelos de Site do Razor v2 do Visual Studio 2012.

A solução alternativa é reparar a instalação do Visual Studio 2012 para reinstalar o ASP.NET MVC 4 e modelos de Site do Razor v2.

### <a name="tooling-issues"></a>Problemas de ferramentas

#### <a name="nuget-error-reported-during-project-creation"></a>Erro de NuGet relatado durante a criação do projeto

Depois de instalar o ASP.NET e Web Tools 2012.2 você pode ver o seguinte erro ao criar um projeto MVC 4

![](aspnet-and-web-tools-20122-release-notes/_static/image1.png)

O ASP.NET e Web Tools 2012.2 fornecido NuGet 2.1 e atualizará a extensão no Visual Studio 2012. Em alguns casos, o instalador do VSIX não atualizarão corretamente VSIX. As etapas a seguir permitirá que você resolva esse problema:

1. Inicie o Visual Studio 2012 como administrador
2. Vá para ferramentas -&gt;extensões e atualizações e desinstalar o NuGet.
3. Feche o Visual Studio
4. Navegue até a pasta de instalação do ASP.NET e Web Tools 2012.2:

    1. Para o Visual Studio 2012: **programa de Programas\Microsoft ASP.NET\ASP.NET Web Stack\Visual Studio 2012**
    2. Para o Visual Studio 2012 Express para Web: **Program Files\Microsoft ASP.NET\ASP.NET Web Stack\Visual Studio Express 2012 para Web**
5. Clique duas vezes em NuGet.Tools.vsix para reinstalar o NuGet

### <a name="web-api-issues"></a>Problemas de API da Web

#### <a name="parsing-issues-in-filter-and-datetime-literals"></a>Análise de problemas em $filter e literais de data e hora

O analisador de URI do OData não pode analisar corretamente a literais de data e hora parcial. Por exemplo, $filter = data/hora de início eq'2012-12-31T12:00' Falha ao analisar corretamente. Uma solução alternativa é usar o completo literal, $filter = data/hora de início eq'2012-12-31T12:00:00'.

#### <a name="odata-doesnt-support-case-insensitive-property-names"></a>OData não dá suporte a nomes de propriedade maiusculas de minúsculas.

OData não dá suporte a nomes de propriedade de maiusculas e minúsculas em consultas de OData e o caminho odata. Consulte itens de trabalho:

- [http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366)
- [http://aspnetwebstack.codeplex.com/workitem/704](http://aspnetwebstack.codeplex.com/workitem/704)

Se os usuários tiverem diferenciam maiusculas de minúsculas em javascript lado do cliente e do lado do servidor, eles provavelmente encontrará esse problema. Esse problema ocorre por design no protocolo odata. Porém, muitos usuários informa esse problema. Para resolvê-lo, os usuários têm que corrigir seus casos na URL.

#### <a name="default-odata-routing-conventions-doesnt-support-postput-on-navigation-property"></a>Padrão OData convenções de roteamento não dá suporte a POST/PUT na propriedade de navegação.

Padrão OData convenções de roteamento não dá suporte a POST/PUT na propriedade de navegação. Consulte o item de trabalho [ http://aspnetwebstack.codeplex.com/workitem/366 ](http://aspnetwebstack.codeplex.com/workitem/366). Nós não essa convenção comumente usada em convenções padrão.

Para resolvê-lo, os usuários precisam estender nova convenção de roteamento para dar suporte a ele.

### <a name="facebook-template-issues"></a>Problemas de modelo do Facebook

#### <a name="facebook-application-template-only-works-using-net-45"></a>Modelo de aplicativo do Facebook só funciona com o .NET 4.5

Você deve selecionar o .NET 4.5 na lista suspensa do framework na caixa de diálogo Novo projeto para ver o modelo de aplicativo do Facebook no ASP.NET MVC 4.

#### <a name="real-time-update-controller"></a>Controlador de atualização em tempo real

O modelo de aplicativo do Facebook permite que usuário facilmente criar um controlador de API da Web para manipular atualizações em tempo real do Facebook. Se sua máquina de desenvolvimento estiver atrás de NAT, o controlador pode não funcionar sem qualquer configuração de rede. Para obter detalhes, consulte aqui: [http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook](http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook)

#### <a name="query-string-values-conflict-with-facebook-oauth-parameters"></a>Conflito de valores de cadeia de caracteres com OAuth do Facebook parâmetros de consulta

Os campos a seguir estão em conflito com a chamada da caixa de diálogo OAuth do Facebook fazer URL. Não adicione seus próprios valores de cadeia de caracteres de consulta com os seguintes nomes: código de erro, erro\_descrição, erro\_motivo.

#### <a name="using-page-inspector-with-facebook-template"></a>Com o Page Inspector com o modelo do Facebook

Você não pode usar o recurso de Inspetor de página no Visual Studio 2012 durante a depuração de seu aplicativo do Facebook. O Page Inspector atualmente não dá suporte a iframes.

### <a name="single-page-application-template-issues"></a>Problemas do modelo de aplicativo de página única

#### <a name="with-jquery-19knockout-221-update-when-running-default-mvc-spa-project-new-todo-item-edit-enter-focus-event-is-not-handled-properly"></a>Com JQuery atualização 1.9/Knockout 2.2.1, ao executar o projeto padrão MVC SPA novo todo item Editar Inserir evento de foco não é manipulado corretamente.

Com JQuery 1.9/Knockout 2.2.1 atualizar, ao executar o projeto do SPA MVC padrão, novo todo item Editar digite não mais foco volta para a caixa de edição do novo item de tarefas depois de inserir o novo item de tarefas à lista de tarefas.

A referência de solução alternativa [ http://knockoutjs.com/documentation/hasfocus-binding.html ](http://knockoutjs.com/documentation/hasfocus-binding.html)e fazer correção semelhante para o código de exemplo a seguir:

Arquivo todo.model.js  
 função todolist(data), adicione seguintes:  
 **self.isSelected = ko.observable(false);**

função todoList.prototype.addTodo, adicione o seguinte texto blacked:  
 **self.isSelected(true);**  
 self.newTodoTitle(&quot;&quot;);

Arquivos cshtml, adicione o seguinte texto blacked:  
 &lt;form data-bind=&quot;submit: addTodo&quot;&gt;  
 &lt;classe de entrada =&quot;addTodo&quot; tipo =&quot;texto&quot; data-bind =&quot;valor: newTodoTitle, espaço reservado: 'Digite aqui para adicionar', blurOnEnter: true, **hasfocus: isSelected**, evento: {desfoque: addTodo}&quot; /&gt;  
 &lt;/form&gt;
