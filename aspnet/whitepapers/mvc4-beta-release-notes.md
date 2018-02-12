---
uid: whitepapers/mvc4-beta-release-notes
title: ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: "Este documento descreve a versão do ASP.NET MVC 4 Beta para Visual Studio 2010."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/09/2011
ms.topic: article
ms.assetid: 666407bb-81de-4319-89ba-0302c382a208
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/mvc4-beta-release-notes
msc.type: content
ms.openlocfilehash: d6797d1dbacff7503f74782d325ff5a9598970c0
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/12/2018
---
<a name="aspnet-mvc-4"></a>ASP.NET MVC 4
====================
> Este documento descreve a versão do ASP.NET MVC 4 Beta para Visual Studio 2010.
> 
> > [!NOTE]
> > Isso não é a versão mais recente. As notas de versão do ASP.NET MVC 4 RC estão disponíveis [aqui](mvc4-release-notes.md).


- [Notas de instalação](#_Toc303253802)
- [Documentação](#_Toc303253803)
- [Suporte](#_Toc303253804)
- [Requisitos de software](#_Toc303253805)
- [Atualizando um projeto ASP.NET MVC 3 para o ASP.NET MVC 4](#_Toc303253806)
- [Novos recursos no ASP.NET MVC 4 Beta](#_Toc303253807)

    - [API Web ASP.NET](#_Toc317096197)
    - [Aplicativo de página única do ASP.NET](#_Toc317096198)
    - [Aprimoramentos para modelos de projeto padrão](#_Toc303253808)
    - [Modelo de projeto móvel](#_Toc303253809)
    - [Modos de exibição](#_Toc303253810)
    - [jQuery Mobile, a alternância de exibição e sobreposição de navegador](#_Toc303253811)
    - [Receitas para geração de código no Visual Studio](#_Toc303253812)
    - [Suporte a tarefa assíncronos controladores](#_Toc303253813)
    - [SDK do Azure](#_Toc303253814)
    - [Problemas conhecidos e as alterações recentes](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>Notas de instalação

ASP.NET MVC 4 Beta para Visual Studio 2010 pode ser instalado a partir de [home page do ASP.NET MVC 4](../mvc/mvc4.md) usando o Web Platform Installer.

Você deve desinstalar qualquer visualizações instaladas anteriormente do ASP.NET MVC 4 antes de instalar o ASP.NET MVC 4 Beta.

Esta versão não é compatível com a visualização do desenvolvedor do .NET Framework 4.5. Você deve desinstalar o .NET 4.5 Developer Preview antes de instalar o ASP.NET MVC 4 Beta.

ASP.NET MVC 4 pode ser instalado e pode executar lado a lado com o ASP.NET MVC 3.

<a id="_Toc303253803"></a>
## <a name="documentation"></a>Documentação

Documentação do ASP.NET MVC está disponível no site do MSDN na seguinte URL:

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

Tutoriais e outras informações sobre o ASP.NET MVC estão disponíveis na página do site da Web do ASP.NET MVC 4 ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)).

<a id="_Toc303253804"></a>
## <a name="support"></a>Suporte

Isso é uma versão de visualização e não é oficialmente suportado. Se você tiver dúvidas sobre como trabalhar com esta versão, poste-as para o fórum do ASP.NET MVC ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)), onde os membros da comunidade do ASP.NET são frequentemente pode oferecer suporte informal.

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>Requisitos de software

Os componentes do ASP.NET MVC 4 para o Visual Studio requerem PowerShell 2.0 e o Visual Studio 2010 com Service Pack 1 ou Visual Web Developer Express 2010 com Service Pack 1.

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>Atualizando um projeto ASP.NET MVC 3 para o ASP.NET MVC 4

ASP.NET MVC 4 pode ser instalado lado a lado com o ASP.NET MVC 3 no mesmo computador, o que proporciona flexibilidade na escolha quando atualizar um aplicativo ASP.NET MVC 3 para o ASP.NET MVC 4.

A maneira mais simples de atualizar é para criar um novo projeto ASP.NET MVC 4 e copie todos os os modos de exibição, controladores, código e conteúdo arquivos de projeto MVC 3 existente para o novo projeto e, em seguida, atualizar o assembly faz referência a no novo projeto para coincidir com o projeto antigo. Se você fez alterações no arquivo Web. config no projeto MVC 3, você também deve mesclar as alterações no arquivo Web. config no projeto MVC 4.

Para atualizar manualmente um aplicativo ASP.NET MVC 3 existente para a versão 4, faça o seguinte:

1. Em todos os arquivos Web. config no projeto (há um na raiz do projeto, um em modos de exibição e um na pasta modos de exibição para cada área em seu projeto), substitua cada ocorrência do texto a seguir:

    [!code-console[Main](mvc4-beta-release-notes/samples/sample1.cmd)]

    com o seguinte texto correspondente:

    [!code-console[Main](mvc4-beta-release-notes/samples/sample2.cmd)]
2. No arquivo Web. config raiz, atualize o *webPages:Version* elemento "2.0.0.0" e adicione um novo *PreserveLoginUrl* chave que tem o valor "true":

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample3.xml)]
3. No Gerenciador de soluções, exclua a referência ao *System.Web.Mvc* (que aponta para a versão de DLL 3). Em seguida, adicione uma referência a *System.Web.Mvc* (v4.0.0.0). Em particular, faça as seguintes alterações para atualizar as referências de assembly. Aqui estão os detalhes:

    1. No Gerenciador de soluções, exclua as referências para os seguintes assemblies: 

        - *System.Web.Mvc*(v3.0.0.0)
        - *System.Web.WebPages*(v1.0.0.0)
        - *System.Web.Razor*(v1.0.0.0)
        - *System.Web.WebPages.Deployment*(v1.0.0.0)
        - *System.Web.WebPages.Razor*(v1.0.0.0)
    2. Adicione um referências para os seguintes assemblies: 

        - *System.Web.Mvc*(v4.0.0.0)
        - *System.Web.WebPages*(v2.0.0.0)
        - *System.Web.Razor*(v2.0.0.0)
        - *System.Web.WebPages.Deployment*(v2.0.0.0)
        - *System.Web.WebPages.Razor*(v2.0.0.0)
4. No Gerenciador de soluções, clique no nome do projeto e selecione Unload Project. Em seguida, clique no nome novamente e selecione Editar *ProjectName*. csproj.
5. Localize o *ProjectTypeGuids* elemento e substitua {E53F8FEA-EAE0-44A6-8774-FFD645390401} por {E3E379DF-F4C6-4180-9B81-6769533ABE47}.
6. Salvar as alterações, feche o arquivo de projeto (. csproj) que você estava editando, clique com o botão direito e, em seguida, selecione Recarregar projeto.
7. Se o projeto referencia todas as bibliotecas de terceiros que são compiladas usando versões anteriores do ASP.NET MVC, abra o arquivo Web. config de raiz e adicione as três seguintes *bindingRedirect* elementos sob o  *configuração* seção: 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample4.xml)]

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4-beta"></a>Novos recursos no ASP.NET MVC 4 Beta

Esta seção descreve os recursos que foram introduzidos na versão Beta do ASP.NET MVC 4.

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>API da Web do ASP.NET

ASP.NET MVC 4 agora inclui a API da Web ASP.NET, uma nova estrutura para criar serviços HTTP que pode alcançar uma ampla gama de clientes, incluindo navegadores e dispositivos móveis. ASP.NET Web API também é uma plataforma ideal para criar serviços RESTful.

ASP.NET Web API inclui suporte para os seguintes recursos:

- **Modelo de programação HTTP moderno:** diretamente acessar e manipular solicitações HTTP e respostas em suas APIs da Web usando um modelo de objeto novo, fortemente tipado do HTTP. O mesmo modelo de programação e pipeline HTTP é simetricamente disponível no cliente por meio do novo tipo de HttpClient.
- **Suporte total para rotas**: APIs da Web agora dão suporte ao conjunto completo de recursos de rota que sempre ter sido uma parte da pilha da Web, incluindo os parâmetros de rota e restrições. Além disso, mapeamento para ações tem total suporte para convenções, portanto você não precisa aplicar atributos como [HttpPost] para suas classes e métodos.
- **Negociação de conteúdo**: O cliente e o servidor podem trabalhar juntos para determinar o formato correto para os dados que estão sendo retornados de uma API. Fornecemos suporte padrão para XML, JSON e formatos de codificação de URL do formulário e você pode estender esse suporte adicionando seus próprios formatadores ou até mesmo substituir a estratégia de negociação de conteúdo padrão.
- **Modelo de associação e de validação:** associadores de modelo fornecem uma maneira fácil para extrair dados de várias partes de uma solicitação HTTP e converter as partes da mensagem em objetos .NET que podem ser usados pelas ações de API da Web.
- **Filtros:** APIs da Web agora dá suporte a filtros, inclusive filtros conhecidos como o atributo [autorizar]. Você pode criar e conectar seus próprios filtros para ações, autorização e tratamento de exceção.
- **Composição de consultas:** retornando simplesmente IQueryable&lt;T&gt;, a API da Web dará suporte a consulta por meio de convenções de URL do OData.
- **Melhor capacidade de teste de detalhes HTTP:** em vez de definir os detalhes HTTP em objetos de contexto estático, ações de API da Web agora podem trabalhar com instâncias de HttpRequestMessage e HttpResponseMessage. Também existem versões genéricas desses objetos para que você possa trabalhar com seus tipos personalizados além dos tipos de HTTP.
- **Melhor inversão da controle (IoC) por meio do DependencyResolver:** API da Web agora usa o padrão de localizador de serviço implementado pelo resolvedor de dependência do MVC para obter instâncias para muitos recursos diferentes.
- **Configuração baseada em código:** configuração da API da Web é realizada apenas por meio de código, deixando a sua configuração de limpeza de arquivos.
- **Auto-host:** APIs da Web pode ser hospedado em seu próprio processo além do IIS enquanto estiver usando toda a capacidade de rotas e outros recursos da API da Web.

Para obter mais detalhes sobre a API da Web do ASP.NET, visite [https://www.asp.net/web-api](../web-api/index.md).

<a id="_Toc317096198"></a>
### <a name="aspnet-single-page-application"></a>Aplicativo de página única do ASP.NET

O ASP.NET MVC 4 agora inclui uma visualização prévia da experiência de criação de aplicativos de única página com significativas interações de cliente usando JavaScript e APIs da Web. Esse suporte inclui:

- Um conjunto de bibliotecas JavaScript mais rica interações local com dados armazenados em cache
- Componentes de API da Web adicionais para a unidade de trabalho e suporte DAL
- Um modelo de projeto MVC com scaffolding para começar rapidamente

Para obter mais detalhes sobre o aplicativo de página única compatível no ASP.NET MVC 4, visite [https://www.asp.net/single-page-application](../single-page-application/index.md).

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>Aprimoramentos para modelos de projeto padrão

O modelo que é usado para criar novos projetos do ASP.NET MVC 4 foi atualizado para criar um site de aparência mais modernos:

![](mvc4-beta-release-notes/_static/image1.png)

Além de melhorias superficiais, aumenta a funcionalidade no novo modelo. O modelo usa uma técnica chamada processamento adaptável a aparência em navegadores de desktop e navegadores móveis sem nenhuma personalização.

![](mvc4-beta-release-notes/_static/image2.png)

Para ver o processamento adaptável em ação, você pode usar um emulador móvel ou apenas tente redimensionar a janela do navegador de área de trabalho para um tamanho menor. Quando a janela do navegador é pequena o suficiente, o layout da página será alterado.

Outro aprimoramento para o modelo de projeto padrão é o uso de JavaScript para fornecer uma interface de usuário mais rica. Os links de logon e registro são usados no modelo são exemplos de como usar o caixa de diálogo de interface do usuário do jQuery para apresentar uma tela de logon avançado:

![](mvc4-beta-release-notes/_static/image3.png)

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>Modelo de projeto móvel

Se você estiver iniciando um novo projeto e deseja criar um site especificamente para dispositivos móveis e navegadores de tablet, você pode usar o novo modelo de projeto de aplicativo móvel. Isso se baseia no jQuery Mobile, uma biblioteca de código aberto para a criação de interface do usuário otimizada para toque:

![](mvc4-beta-release-notes/_static/image4.png)

Este modelo contém a mesma estrutura de aplicativo como o modelo de aplicativo de Internet (e o código do controlador é praticamente idêntico), mas é com o jQuery Mobile para uma boa aparência e comportamento bem em dispositivos móveis baseados em toque estilo. Para obter mais informações sobre a estrutura e estilo da interface do usuário móvel, consulte o [site do projeto móvel jQuery](http://jquerymobile.com/).

Se você já tiver um site que você deseja adicionar exibições mobile otimizado para, ou se você quiser criar um único site que funciona diferentemente com estilo modos de exibição para os navegadores de desktop e mobile e orientada a área de trabalho, você pode usar o novo recurso de modos de exibição. (Consulte a próxima seção).

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>Modos de exibição

O novo recurso de modos de exibição permite que um aplicativo selecione exibições dependendo do navegador que está fazendo a solicitação. Por exemplo, se um navegador desktop solicitar a página inicial, o aplicativo pode usar o modelo Views\Home\Index.cshtml. Se um navegador móvel solicitar a página inicial, o aplicativo pode retornar o modelo de Views\Home\Index.mobile.cshtml.

Layouts e existe meio-termo também pode ser substituído para tipos de navegador específico. Por exemplo:

- Se sua pasta exibições \ compartilhadas contém o \_cshtml e \_Layout.mobile.cshtml modelos, por padrão, o aplicativo usará \_Layout.mobile.cshtml durante solicitações de navegadores móveis e \_Cshtml durante outras solicitações.
- Se uma pasta contém \_MyPartial.cshtml e \_MyPartial.mobile.cshtml, a instrução @Html.Partial("\_MyPartial") será renderizado \_MyPartial.mobile.cshtml durante solicitações de celular navegadores, e \_MyPartial.cshtml durante outras solicitações.

Se você quiser criar modos de exibição mais específicos, layouts ou modos de exibição parciais para outros dispositivos, você pode registrar um novo *DefaultDisplayMode* instância para especificar qual nome para pesquisar quando uma solicitação satisfaz condições específicas. Por exemplo, você pode adicionar o código a seguir para o *aplicativo\_iniciar* método no arquivo global. asax para registrar a cadeia de caracteres "iPhone" como um modo de exibição que se aplica quando o navegador de iPhone da Apple faz uma solicitação:

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample5.cs)]

Depois que esse código é executado, quando um navegador de iPhone da Apple faz uma solicitação, o aplicativo usará a exibições \ compartilhadas\\_Layout.iPhone.cshtml layout (se houver).

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-the-view-switcher-and-browser-overriding"></a>jQuery Mobile, a alternância de exibição e sobreposição de navegador

jQuery Mobile é uma biblioteca de código aberto para a criação de interface da web otimizada para toque. Se você quiser usar o jQuery Mobile com um aplicativo ASP.NET MVC 4, você pode baixar e instalar um pacote do NuGet que ajuda você a começar. Para instalá-lo do Console do Gerenciador de pacote do Visual Studio, digite o seguinte comando:

[!code-powershell[Main](mvc4-beta-release-notes/samples/sample6.ps1)]

Isso instala o jQuery Mobile e alguns arquivos auxiliares, incluindo o seguinte:

- Exibições/compartilhadas/\_Layout.Mobile.cshtml, que é um layout com Mobile do jQuery.
- Um componente de alternância de exibição, que consiste deexibições/compartilhadas/\_exibição parcial ViewSwitcher.cshtml e o controlador de ViewSwitcherController.cs.

Depois de instalar o pacote, execute o aplicativo usando um navegador móvel (ou equivalente, como o Firefox [alternador de agente do usuário](http://chrispederick.com/work/user-agent-switcher/) complemento). Você verá que suas páginas ter aparência bastante diferentes, porque o jQuery Mobile manipula o layout e estilo. Para tirar proveito disso, você pode fazer o seguinte:

- Criar substituições de exibição específicas para dispositivos móveis, conforme descrito em [modos de exibição](#_Toc303253810) anteriormente (por exemplo, criar Views\Home\Index.mobile.cshtml para substituir Views\Home\Index.cshtml navegadores móveis).
- Leitura de [jQuery Mobile documentação](http://jquerymobile.com/) para saber mais sobre como adicionar elementos de interface do usuário otimizada para toque nos modos de exibição móveis.

É uma convenção mobile otimizado páginas da web para adicionar um link cujo texto é algo como o modo de exibição da área de trabalho ou modo completo do site que permite aos usuários a alternar para uma versão de área de trabalho da página. O pacote jQuery.Mobile.MVC inclui um exemplo de componente de alternância de exibição para essa finalidade. Ele é usado no padrão de exibições \ compartilhadas\\cshtml exibição, e ele esta aparência quando a página é renderizada:

![](mvc4-beta-release-notes/_static/image5.png)

Se visitantes clicarem no link, eles são alternados para a versão da área de trabalho da mesma página.

Porque seu layout de área de trabalho não incluirá uma alternância de exibição, por padrão, os visitantes não têm uma maneira de obter modo móvel. Para habilitar isso, adicione a seguinte referência ao  *\_ViewSwitcher* em seu layout de área de trabalho, apenas dentro de  *&lt;corpo&gt;*  elemento:

[!code-cshtml[Main](mvc4-beta-release-notes/samples/sample7.cshtml)]

A alternância de exibição usa um novo recurso chamado substituição do navegador. Esse recurso permite que seu aplicativo lide com solicitações como se estivessem vindo de um navegador diferente (agente do usuário) que o enviou realmente. A tabela a seguir lista os métodos de substituição do navegador fornece.

| `HttpContext.SetOverriddenBrowser(userAgentString)` | Substitui o valor do agente de usuário real da solicitação usando o agente do usuário especificado. |
| --- | --- |
| `HttpContext.GetOverriddenUserAgent()` | Retorna o valor de substituição do agente de usuário da solicitação ou a cadeia de caracteres de agente de usuário real se nenhuma substituição tiver sido especificada. |
| `HttpContext.GetOverriddenBrowser()` | Retorna um *HttpBrowserCapabilitiesBase* instância que corresponde ao agente do usuário definido atualmente para a solicitação (reais ou substituído). Você pode usar esse valor para obter as propriedades como *IsMobileDevice*. |
| `HttpContext.ClearOverriddenBrowser()` | Remove qualquer agente de usuário substituído para a solicitação atual. |

Substituição do navegador é dos principais recursos do ASP.NET MVC 4 e estará disponível mesmo se você não instalar o pacote jQuery.Mobile.MVC. No entanto, ele afeta somente exibição, layout e seleção de exibição parcial — não afeta qualquer outro recurso do ASP.NET que depende de *Request. browser* objeto.

Por padrão, a substituição de agente do usuário é armazenada usando um cookie. Se você deseja armazenar a substituição em outro lugar (por exemplo, em um banco de dados), você pode substituir o provedor padrão (*BrowserOverrideStores.Current*). Documentação para esse provedor estarão disponível para acompanhar uma versão posterior do ASP.NET MVC.

<a id="_Toc303253812"></a>
### <a name="recipes-for-code-generation-in-visual-studio"></a>Receitas para geração de código no Visual Studio

O novo recurso de receitas permite que o Visual Studio gerar o código de solução específica com base em pacotes que podem ser instalados usando o NuGet. A estrutura de receitas facilita para os desenvolvedores escrevam plugins de geração de código, que você também pode usar para substituir os geradores de código internos para adicionar modo de exibição, adicionar área e adicionar controlador. Porque receitas são implantadas como pacotes do NuGet, podem facilmente ser verificados no controle de origem e compartilhadas com todos os desenvolvedores no projeto automaticamente. Eles também estão disponíveis em uma base por solução.

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>Suporte a tarefa assíncronos controladores

Agora você pode escrever os métodos de ação assíncrono como uma única métodos que retornam um objeto do tipo *tarefa* ou *tarefa&lt;ActionResult&gt;*.

Por exemplo, se você estiver usando o Visual c# 5 (ou usando o [Async CTP](https://msdn.microsoft.com/vstudio/async.aspx)), você pode criar um método de ação assíncrono é semelhante ao seguinte:

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample8.cs)]

No método de ação anterior, as chamadas para *newsService.GetHeadlinesAsync* e *sportsService.GetScoresAsync* são chamados de forma assíncrona e não bloqueiam um thread do pool de threads.

Métodos de ação assíncrono que retornam *tarefa* instâncias também podem dar suporte a tempos limite. Para tornar o seu método de ação pode ser cancelado, adicione um parâmetro de tipo *CancellationToken* à assinatura do método de ação. O exemplo a seguir mostra um método de ação assíncrono, que tem um tempo limite de milissegundos de 2500 e que exibe um *TimedOut* exibir para o cliente se ocorrer um tempo limite.

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample9.cs)]

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>SDK do Azure

ASP.NET MVC 4 Beta suporta a versão 1.5 de setembro de 2011 do SDK do Windows Azure.

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemas conhecidos e as alterações recentes

- **Depois de instalar o ASP.NET MVC 4 Beta, o editor CSHTML/VBHTML no editor do Visual Studio 2010 Service Pack 1 CSHTML/VBHTML pausar por muito tempo depois de digitar o trecho de código ou JavaScript arquivos cshtml ou vbhtml.** Isso ocorre apenas em aplicativos ASP.NET MVC 4 que acabou de ser criados e ainda não foi compilados.

    A solução alternativa é compilar o projeto para obter os assemblies da pasta bin. Observe que, se você limpar o projeto que remove os assemblies da pasta bin, o problema de editor voltará.

    Isso será corrigido na próxima versão.
- **Modelos de projeto c# para o Visual Studio 11 Beta contêm uma cadeia de caracteres de conexão incorretas em Global.asax.cs.** A conexão padrão especificado no aplicativo\_método Start para projetos criados no Visual Studio 11 Beta contêm uma cadeia de caracteres de conexão de LocalDB que contém uma barra invertida sem escape (\) caracteres. Isso resulta em um erro de conexão ao tentar acessar um Entity Framework DbContext, que gera uma SqlException.

    Para corrigir esse problema, o caractere de barra invertida no aplicativo de escape\_iniciar o método de Global.asax.cs para exibi-lo da seguinte maneira:

    [!code-csharp[Main](mvc4-beta-release-notes/samples/sample10.cs)]
- **Aplicativos ASP.NET MVC 4 que destino .NET 4.5 lançará um FileLoadException após a tentativa de acessar o assembly System.Net.Http.dll quando executado sob o .NET 4.0.** Aplicativos ASP.NET MVC 4 criados no .NET 4.5 contêm uma associação de redirecionamento que resulta em um FileLoadException que indicando "não foi possível carregar arquivo ou assembly 'System' ou uma de suas dependências." Quando o aplicativo é executado em um sistema com o .NET 4.0 instalado. Para corrigir esse problema, remova o redirecionamento de associação a seguir do Web. config:

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample11.xml)]

    O elemento de associação de assembly no Web. config modificada deve aparecer da seguinte maneira:

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample12.xml)]
- **O modelo de item "Adicionar controlador" em projetos do Visual Basic gera um namespace incorreto quando invocado * de dentro de uma área.** Quando você adiciona um controlador para uma área em um projeto do ASP.NET MVC que usa o Visual Basic, o modelo de item insere o espaço para nome incorreto no controlador. O resultado é um erro de "arquivo não encontrado" quando você navegar para qualquer ação no controlador.  
  
 O namespace gerado omite tudo após o namespace raiz. Por exemplo, o namespace gerado é *RootNamespace* , mas deve ser *RootNamespace.Areas.AreaName.Controllers* .
- **Alterações significativas no mecanismo de exibição Razor.** Como parte de uma reconfiguração do analisador Razor, os seguintes tipos foram removidos do *System.Web.Mvc.Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

 Os métodos a seguir também foram removidos: 

    - *MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
    - *MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*
    - *MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
- **Quando WebMatrix.WebData.dll é incluído em uma pasta de um aplicativos ASP.NET MVC 4 /bin, ele assume a URL para autenticação de formulários.** Adicionar o assembly WebMatrix.WebData.dll ao seu aplicativo (por exemplo, selecionando "ASP.NET páginas da Web com sintaxe Razor" ao usar a caixa de diálogo Adicionar dependências implantáveis) substituirá o redirecionamento de logon de autenticação para o logon/conta em vez de / conta/logon conforme o esperado pelo controlador de conta do ASP.NET MVC padrão. Para evitar esse comportamento e usar a URL especificada já na seção de autenticação da Web. config, você pode adicionar um appSetting chamado PreserveLoginUrl e defina-a como true: 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample13.xml)]
- **O NuGet package manager Falha na instalação ao tentar instalar o ASP.NET MVC 4 para instalações lado a lado do Visual Studio 2010 e o Visual Web Developer 2010.** Para executar o Visual Studio 2010 e o Visual Web Developer 2010 lado a lado com o ASP.NET MVC 4, você deve instalar ASP.NET MVC 4 depois de já tem sido instaladas ambas as versões do Visual Studio.
- **Desinstalar o ASP.NET MVC 4 falhará se os pré-requisitos já tem sido desinstalados.** Para desinstalar completamente o ASP.NET MVC 4you deve desinstalar o ASP.NET MVC 4 antes de desinstalar o Visual Studio.
- **Executar um projeto de API da Web padrão mostra instruções incorretamente direcionam o usuário adicionar rotas usando o método RegisterApis, que não existe.** Rotas devem ser adicionadas no método RegisterRoutes usando a tabela de rotas do ASP.NET.
- **Instalar o ASP.NET MVC 4 Beta quebras de aplicativos ASP.NET MVC 3 RTM.** Aplicativos ASP.NET MVC 3 que foram criados com o RTM versão (não com a versão de atualização de ferramentas do ASP.NET MVC 3) requerem as seguintes alterações para funcionar lado a lado com o ASP.NET MVC 4 Beta. Compilar o projeto sem fazer esses resultados de atualizações em erros de compilação. 

    **Atualizações necessárias**

    1. No arquivo Web. config raiz, adicione um novo  *&lt;appSettings&gt;*  entrada com a chave *webPages:Version* e o valor *1.0.0.0*.

        [!code-xml[Main](mvc4-beta-release-notes/samples/sample14.xml)]
    2. No Gerenciador de soluções, clique no nome do projeto e selecione Unload Project. Em seguida, clique no nome novamente e selecione Editar *ProjectName*. csproj.
    3. Localize as seguintes referências de assembly: 

        [!code-xml[Main](mvc4-beta-release-notes/samples/sample15.xml)]

        Substitua-os com o seguinte:

        [!code-xml[Main](mvc4-beta-release-notes/samples/sample16.xml)]
    4. Salvar as alterações, feche o arquivo de projeto (. csproj) que você estava editando e, em seguida, clique com o botão direito e selecione Recarregar.
