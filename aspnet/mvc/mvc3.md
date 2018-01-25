---
uid: mvc/mvc3
title: ASP.NET MVC 3 | Microsoft Docs
author: rick-anderson
description: "(inclui abril de 2011 ferramentas de atualização) O ASP.NET MVC 3 é uma estrutura para criar aplicativos web escalonável, baseado em padrões usando o padrão de design bem estabelecidos..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/05/2010
ms.topic: article
ms.assetid: dddc8812-a0bc-49f9-aafb-caf2064c2b8c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/mvc3
msc.type: content
ms.openlocfilehash: c7eee987b28a5d7f8b40fe89a7bf7517ec06646f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-mvc-3"></a>ASP.NET MVC 3
====================
> *(inclui abril de 2011 ferramentas de atualização)*
> 
> ASP.NET MVC 3 é uma estrutura para criar aplicativos web escalonável, baseado em padrões usando padrões de design bem estabelecidos e o poder do ASP.NET e o .NET Framework.
> 
> Ele instala lado a lado com o ASP.NET MVC 2, para começar a usá-lo hoje!
> 
> Baixe o [instalador aqui](https://go.microsoft.com/fwlink/?LinkID=208140)


## <a name="top-features"></a>Principais recursos

- Sistema de Scaffolding integrado extensível por meio do NuGet
- Modelos de projeto habilitadas 5 de HTML
- Modos de exibição expressivos, incluindo o novo mecanismo de exibição Razor
- Ganchos poderosos com injeção de dependência e filtros de ação globais
- Suporte a Rich JavaScript com o JavaScript discreto, jQuery validação e associação de JSON
- *Ler a lista completa de recursos [abaixo](#overview)*

## <a name="top-links"></a>Links superior

O que há de novo no ASP.NET MVC 3

- Phil Haack: [ASP.NET MVC 3 lançado](http://haacked.com/archive/2011/01/13/aspnetmvc3-released.aspx)
- Scott Hanselman: [ASP.NET MVC3, o WebMatrix, o NuGet, IIS Express e Orchard liberado - a versão da Web de janeiro de Microsoft no contexto](http://www.hanselman.com/blog/ASPNETMVC3WebMatrixNuGetIISExpressAndOrchardReleasedTheMicrosoftJanuaryWebReleaseInContext.aspx)
- Scott Guthrie: [anunciando o lançamento do ASP.NET MVC 3, IIS Express, SQL CE 4, Framework do Web Farm, Orchard, o WebMatrix](https://weblogs.asp.net/scottgu/archive/2011/01/13/announcing-release-of-asp-net-mvc-3-iis-express-sql-ce-4-web-farm-framework-orchard-webmatrix.aspx)
- [Notas de versão do ASP.NET MVC 3](../whitepapers/mvc3-release-notes.md)

Instalação e ajuda

- Instala o ASP.NET MVC 3 usando a [Web Platform Installer (recomendado)](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MVC3)
- Instala o ASP.NET MVC 3 usando a [executável do instalador](https://go.microsoft.com/fwlink/?LinkID=208140)
- Instalar [ASP.NET MVC 3 do Visual Studio 11 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=208140)
- Leitura de [Introdução ao tutorial do ASP.NET MVC 3](overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)
- Obter ajuda e discutir o ASP.NET MVC 3 no [fóruns](https://forums.asp.net/1146.aspx)

<a id="overview"></a>
## <a name="aspnet-mvc-3-overview"></a>Visão geral do ASP.NET MVC 3

ASP.NET MVC 3 se baseia no ASP.NET MVC 1 e 2, adicionando recursos excelentes que ambas simplificam seu código e permitir que mais profunda extensibilidade. Este tópico fornece uma visão geral dos muitos dos novos recursos incluídos nesta versão, organizadas nas seguintes seções:

- [Estrutura extensível com a integração de MvcScaffold](#BM_MvcScaffolding)
- [Modelos de projeto habilitadas 5 de HTML](#BM_HTML5)
- [O mecanismo de exibição Razor](#BM_TheRazorViewEngine)
- [Suporte para vários mecanismos de exibição](#BM_Support_for_Multiple_View_Engines)
- [Melhorias do controlador](#BM_Controller_Improvements)
- [JavaScript e Ajax](#BM_JavaScript_and_Ajax_Improvements)
- [Aprimoramentos de validação de modelo](#BM_Model_Validation_Improvements)
- [Aprimoramentos de injeção de dependência](#BM_Dependency_Injection_Improvements)
- [Outros novos recursos](#BM_Other_New_Features)

<a id="BM_MvcScaffolding"></a>

## <a name="extensible-scaffolding-with-mvcscaffold-integration"></a>Estrutura extensível com a integração de MvcScaffold

O novo sistema de Scaffolding torna mais fácil acompanhar e iniciar usando produtiva se você estiver totalmente novo para a estrutura e automatizar tarefas comuns de desenvolvimento, se você tiver experiência e já sabe o que está fazendo.

Isso tem suporte pelo NuGet novo *scaffolding* pacote chamado **MvcScaffolding**. O termo "Estrutura" é usada por muitas tecnologias de software, significando "gerar rapidamente uma estrutura de tópicos do software que você pode editar e personalizar". O pacote de scaffolding que estamos criando para o ASP.NET MVC é muito útil em vários cenários:

- **Se você estiver aprendendo o ASP.NET MVC pela primeira vez**, pois ele oferece uma maneira rápida de obter um código de trabalho útil, que, em seguida, você pode editar e adaptar de acordo com suas necessidades. Ele libera você da trauma da olhando para uma página em branco e não sabe onde começar!
- **Se você conhece bem o ASP.NET MVC e agora está explorando alguma nova tecnologia de complemento** como um mapeador relacional de objetos, um mecanismo de exibição, uma biblioteca de teste, etc., como o criador do que a tecnologia pode também criou um pacote de scaffolding para ele.
- **Se o seu trabalho envolve a criação de repetidamente semelhantes classes ou arquivos de algum tipo**, pois você pode criar scaffolders personalizados que geram artefatos de teste, scripts de implantação ou qualquer outra coisa que você precisa. Todos os membros da sua equipe podem usar os scaffolders personalizados, muito.

Outros recursos em MvcScaffolding incluem:

- Suporte para projetos c# e VB
- Suporte para o Razor e o ASPX mecanismos de exibição
- Oferece suporte à estrutura em áreas do ASP.NET MVC e usando o modo de exibição personalizado layouts/mestre
- Você pode personalizar facilmente a saída editando modelos T4
- Você pode adicionar scaffolders totalmente novos usando lógica personalizada do PowerShell e modelos T4 personalizados. Esses (e os parâmetros personalizados que você fornecer) automaticamente aparecem na lista de conclusão de tabulação do console.
- Você pode obter os pacotes do NuGet contendo scaffolders adicionais para tecnologias diferentes (por exemplo, há um prova de conceito um para LINQ to SQL agora) e combinação de -los juntos

A atualização de ferramentas do ASP.NET MVC 3 inclui excelente suporte do Visual Studio para este sistema de scaffolding, como:

- Adicione caixa de diálogo do controlador agora dá suporte a scaffolding completo automático de criar, ler, atualizar e excluir ações de controlador e modos de exibição correspondentes. Por padrão, isso scaffolds código de acesso a dados usando EF Code First.
- Adicionar caixa de diálogo do controlador oferece suporte a *scaffolds extensíveis* por meio do NuGet pacotes como *MvcScaffolding*. Isso permite conectar scaffolds personalizados na caixa de diálogo que permite que você crie scaffolds para outras tecnologias de acesso a dados como NHibernate ou até mesmo JET com ODBCDirect se então você está propenso!

Para obter mais informações sobre a estrutura ASP.NET MVC 3, consulte os seguintes recursos:

- Steve Sanderson postagem séries, incluindo: 

    1. [Introdução: O projeto ASP.NET MVC 3 com o pacote MvcScaffolding Scaffold](http://blog.stevensanderson.com/2011/01/13/scaffold-your-aspnet-mvc-3-project-with-the-mvcscaffolding-package/)
    2. [Uso padrão: casos típicos de uso e opções](http://blog.stevensanderson.com/2011/01/13/mvcscaffolding-standard-usage/)
    3. [Relações um-para-muitos](http://blog.stevensanderson.com/2011/01/28/mvcscaffolding-one-to-many-relationships/)
    4. [Ações de scaffolding e testes de unidade](http://blog.stevensanderson.com/2011/03/28/scaffolding-actions-and-unit-tests-with-mvcscaffolding/)
    5. [Substituindo os modelos T4](http://blog.stevensanderson.com/2011/04/06/mvcscaffolding-overriding-the-t4-templates/)
    6. [Esta postagem: Criando scaffolders personalizados](http://blog.stevensanderson.com/2011/04/07/mvcscaffolding-creating-custom-scaffolders/)
- Postagem de Scott Hanselman de sua sessão PDC 2010 [criar um Blog com o Microsoft "Sem nome pacote da Web adorar"](http://www.hanselman.com/blog/PDC10BuildingABlogWithMicrosoftUnnamedPackageOfWebLove.aspx)
- [Notas de versão do MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_HTML5"></a>

## <a name="html-5-project-templates"></a>Modelos de projeto 5 de HTML

A caixa de diálogo Novo projeto inclui versões de habilitar HTML 5 uma caixa de seleção de modelos de projeto. Esses modelos aproveitam Modernizr 1.7 para fornecer suporte de compatibilidade para 5 de HTML e CSS 3 em navegadores de nível inferior.

<a id="BM_TheRazorViewEngine"></a>

## <a name="the-razor-view-engine"></a>O mecanismo de exibição Razor

ASP.NET MVC 3 vem com um novo mecanismo de exibição denominado Razor que oferece os seguintes benefícios:

- A sintaxe do Razor é limpo e conciso, exigindo um número mínimo de pressionamentos de tecla.
- Razor é fácil de aprender, em parte porque ele se baseia existentes linguagens como c# e Visual Basic.
- O Visual Studio inclui o IntelliSense e o código coloração de sintaxe do Razor.
- Modos de exibição Razor podem ser um teste de unidade sem exigir que você executa o aplicativo ou iniciar um servidor web.

Alguns recursos novos do Razor incluem o seguinte:

- `@model`sintaxe para especificar o tipo que está sendo passado para o modo de exibição.
- `@* *@`sintaxe de comentário.
- A capacidade de especificar os padrões (como `layoutpage`) uma vez para um site inteiro.
- O `Html.Raw` método para exibir um texto sem codificação HTML-lo.
- Suporte para compartilhar código entre vários modos de exibição (*\_viewstart.cshtml* ou  *\_viewstart.vbhtml* arquivos).

Razor também inclui novos auxiliares HTML, como o seguinte:

- `Chart`. Renderiza um gráfico, oferecendo os mesmos recursos que o controle de gráfico no ASP.NET 4.
- `WebGrid`. Renderiza uma grade de dados, com funcionalidade de paginação e classificação.
- `Crypto`. Usa para criar corretamente os algoritmos de hash com salt e senhas de hash.
- `WebImage`. Renderiza uma imagem.
- `WebMail`. Envia uma mensagem de email.

Para obter mais informações sobre Razor, consulte os seguintes recursos:

- [Postagem no blog de Scott Guthrie apresentando Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)
- [Apresentando o postagem no blog de Scott Guthrie o @model palavra-chave](https://weblogs.asp.net/scottgu/archive/2010/10/19/asp-net-mvc-3-new-model-directive-support-in-razor.aspx)
- [Introdução ao layouts de Razor de postagem do blog de Scott Guthrie](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)
- [Referência de API rápida do Razor](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)
- [Notas de versão do MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_Support_for_Multiple_View_Engines"></a>

## <a name="support-for-multiple-view-engines"></a>Suporte para vários mecanismos de exibição

O **adicionar exibição** caixa de diálogo no ASP.NET MVC 3 permite que você escolha o mecanismo de exibição que você deseja trabalhar, e o **novo projeto** caixa de diálogo permite que você especifique o mecanismo de exibição padrão para um projeto. Você pode escolher o mecanismo de exibição de formulários da Web (ASPX), Razor ou um mecanismo de exibição de código-fonte aberto como [Spark](http://sparkviewengine.com/), [NHaml](https://code.google.com/p/nhaml/), ou [NDjango](http://ndjango.org/).

<a id="BM_Controller_Improvements"></a>

## <a name="controller-improvements"></a>Melhorias do controlador

### <a name="global-action-filters"></a>Filtros de ação globais

Às vezes você deseja executar lógica antes de executa um método de ação ou após a execução de um método de ação. Para dar suporte a isso, o ASP.NET MVC 2 fornecido filtros de ação. Filtros de ação são os atributos personalizados que fornecem um meio declarativo para adicionar o comportamento de ação de pré e pós-ação para métodos de ação do controlador específico. No entanto, em alguns casos você talvez queira especificar ação pré ou pós-comportamento que se aplica a todos os métodos de ação. MVC 3 permite que você especifique filtros globais ao adicioná-los para o `GlobalFilters` coleção. Para obter mais informações sobre filtros de ação globais, consulte os seguintes recursos:

- [Blog de Scott Guthrie na visualização MVC 3](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)
- [Filtragem no ASP.NET MVC](https://msdn.microsoft.com/library/gg416513(VS.98).aspx)

### <a name="new-viewbag-property"></a>Nova propriedade "ViewBag"

Suporte a controladores MVC 2 um `ViewData` propriedade que permite a você passar dados para um modelo de exibição usando um API de dicionário de associação tardia. No MVC 3, você também pode usar a sintaxe um pouco mais simples com o `ViewBag` propriedade para realizar a mesma finalidade. Por exemplo, em vez de escrever `ViewData["Message"]="text"`, você pode escrever `ViewBag.Message="text"`. Você não precisa definir as classes digitada para usar o `ViewBag` propriedade. Porque é uma propriedade dinâmica, você pode em vez disso, apenas obter ou definir as propriedades e resolvê-los dinamicamente em tempo de execução. Internamente, `ViewBag` propriedades são armazenadas como pares de nome/valor no `ViewData` dicionário. (Observação: na maioria das versões de pré-lançamento do MVC 3, o `ViewBag` propriedade era denominada o `ViewModel` propriedade.)

### <a name="new-actionresult-types"></a>Novos tipos de "ActionResult"

O seguinte `ActionResult` tipos e métodos auxiliares correspondentes são novos ou aprimorados no MVC 3:

- [HttpNotFoundResult](https://msdn.microsoft.com/library/system.web.mvc.httpnotfoundresult(v=vs.98).aspx). Retorna um código de status HTTP 404 para o cliente.
- [RedirectResult](https://msdn.microsoft.com/library/system.web.mvc.redirectresult(v=VS.98).aspx). Retorna um redirecionamento temporário (código de status HTTP 302) ou um redirecionamento permanente (código de status HTTP 301), dependendo de um parâmetro booliano. Em conjunto com essa alteração, o [controlador](https://msdn.microsoft.com/library/system.web.mvc.controller(v=VS.98).aspx) classe agora tem três métodos para realizar redirecionamentos permanentes: `RedirectPermanent`, `RedirectToRoutePermanent`, e `RedirectToActionPermanent`. Esses métodos retornam uma instância do `RedirectResult` com o `Permanent` propriedade definida como `true`.
- [HttpStatusCodeResult](https://msdn.microsoft.com/library/system.web.mvc.httpstatuscoderesult(v=VS.98).aspx). Retorna um código de status HTTP especificado pelo usuário.

<a id="BM_JavaScript_and_Ajax_Improvements"></a>

## <a name="javascript-and-ajax-improvements"></a>Aprimoramentos de Ajax e JavaScript

Por padrão, a validação e Ajax auxiliares no MVC 3 usam uma abordagem de JavaScript discreta. O JavaScript discreto evita injetando embutido JavaScript no HTML. Isso torna o HTML menores e menos confuso e torna mais fácil alternar ou personalizar bibliotecas JavaScript. Auxiliares de validação no MVC 3 também usam o `jQueryValidate` plug-in por padrão. Se desejar que o comportamento de MVC 2, você pode desabilitar o JavaScript discreto usando um *Web. config* arquivo de configuração. Para obter mais informações sobre aprimoramentos de JavaScript e Ajax, consulte os seguintes recursos:

- [Introdução ao JavaScript discreto no site da Wikipedia](http://en.wikipedia.org/wiki/Unobtrusive_JavaScript)
- [Postagem de JavaScript discreto Brad Wilson](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-ajax.html)
- [Postagem de validação de JavaScript discreto Brad Wilson](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html)
- [Criando um aplicativo MVC 3 com Razor e o JavaScript discreto](overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript.md) (tutorial sobre o site do ASP.NET)
- [Notas de versão do MVC 3](../whitepapers/mvc3-release-notes.md)

### <a name="client-side-validation-enabled-by-default"></a>Validação do lado do cliente habilitada por padrão

Em versões anteriores do MVC, você precisa chamar explicitamente o `Html.EnableClientValidation` método de um modo de exibição a fim de habilitar a validação do lado do cliente. No MVC 3 isso não é necessário porque a validação do lado do cliente é habilitada por padrão. (Você pode desabilitar essa opção usando uma configuração de *Web. config* arquivo.)

Para validação do lado do cliente trabalhar, você ainda necessário fazer referência a jQuery apropriado e bibliotecas de validação jQuery em seu site. Você pode hospedar essas bibliotecas no seu próprio servidor ou fazer referência a eles de uma rede de fornecimento de conteúdo (CDN) como CDNs da Microsoft ou o Google.

### <a name="remote-validator"></a>Validador remoto

ASP.NET MVC 3 oferece suporte a novos [RemoteAttribute](https://msdn.microsoft.com/library/system.web.mvc.remoteattribute(v=VS.98).aspx) classe que permite que você aproveite o validação jQuery plug-in do suporte de validador remoto. Isso permite que a biblioteca de validação do lado do cliente chamar um método personalizado que você define no servidor para executar a lógica de validação que só pode ser feita automaticamente no servidor.

No exemplo a seguir, o `Remote` atributo especifica que a validação do cliente chamará uma ação chamada `UserNameAvailable` no `UsersController` classe para validar o `UserName` campo.

[!code-csharp[Main](mvc3/samples/sample1.cs)]

O exemplo a seguir mostra o controlador correspondente.

[!code-csharp[Main](mvc3/samples/sample2.cs)]

Para obter mais informações sobre como usar o `Remote` de atributo, consulte [como: implementar a validação remota no ASP.NET MVC](https://msdn.microsoft.com/library/gg508808(VS.98).aspx) na biblioteca MSDN.

### <a name="json-binding-support"></a>Suporte à vinculação de JSON

ASP.NET MVC 3 inclui suporte interno de associação de JSON que permite que os métodos de ação receber dados codificados em JSON e modelo de associação-lo aos parâmetros do método de ação. Esse recurso é útil em cenários que envolvem modelos do cliente e associação de dados. (Modelos de cliente permitem que você formatar e exibir um único item de dados ou conjunto de itens de dados usando modelos que executam no cliente.) MVC 3 permite que você se conecte facilmente os modelos de cliente com métodos de ação do servidor que envia e recebe dados JSON. Para obter mais informações sobre o suporte da associação de JSON, consulte o **JavaScript e AJAX melhorias** seção [postagem de blog de visualização do MVC 3 de Scott Guthrie](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx).

<a id="BM_Model_Validation_Improvements"></a>

## <a name="model-validation-improvements"></a>Aprimoramentos de validação de modelo

### <a name="dataannotations-metadata-attributes"></a>Atributos de metadados de "DataAnnotations"

Dá suporte ao ASP.NET MVC 3 `DataAnnotations` atributos de metadados como `DisplayAttribute`.

### <a name="validationattribute-class"></a>Classe "ValidationAttribute"

O `ValidationAttribute` classe foi aprimorada no .NET Framework 4 para dar suporte a um novo `IsValid` sobrecarga que fornece mais informações sobre o contexto atual de validação, como o objeto está sendo validado. Isso permite mais ricas cenários em que você pode validar o valor atual com base em outra propriedade do modelo. Por exemplo, o novo `CompareAttribute` atributo permite comparar os valores de duas propriedades de um modelo. No exemplo a seguir, o `ComparePassword` propriedade deve corresponder a `Password` campo para serem válidas.

[!code-csharp[Main](mvc3/samples/sample3.cs)]

### <a name="validation-interfaces"></a>Interfaces de validação

O [IValidatableObject](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.ivalidatableobject.aspx) interface permite que você execute a validação de nível de modelo e permite que você fornecer mensagens de erro que são específicas para o estado do modelo geral, ou entre duas propriedades dentro do modelo de validação . MVC 3 agora recupera erros do `IValidatableObject` interface quando a associação de modelo e automaticamente sinalizadores ou realces afetaram campos dentro de um modo de exibição usando os auxiliares de formulário HTML internos.

O [IClientValidatable](https://msdn.microsoft.com/library/system.web.mvc.iclientvalidatable(v=VS.98).aspx) interface permite que o ASP.NET MVC descobrir em tempo de execução se um validador tem suporte para validação do cliente. Essa interface foi projetada para que eles podem ser integrados com uma variedade de estruturas de validação.

Para obter mais informações sobre as interfaces de validação, consulte o **melhorias de validação de modelo** seção [postagem de blog de visualização do MVC 3 de Scott Guthrie](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx). (No entanto, observe que a referência a "IValidateObject" no blog deve ser "IValidatableObject".)

<a id="BM_Dependency_Injection_Improvements"></a>

## <a name="dependency-injection-improvements"></a>Aprimoramentos de injeção de dependência

ASP.NET MVC 3 fornece melhor suporte para a aplicação de injeção de dependência (DI) e para a integração com os contêineres de injeção de dependência ou inversão de controle (IOC). Foi adicionado suporte para DI nas seguintes áreas:

- Controladores (Registrando e injeção de fábricas de controlador, injeção de controladores).
- Modos de exibição (Registrando e injeção de mecanismos de exibição, injeção de dependências em páginas de exibição).
- Filtros de ação (localização e injeção de filtros).
- Associadores de modelo (Registrando e injeção de).
- Provedores de validação de modelo (Registrando e injeção de).
- Provedores de metadados de modelo (Registrando e injeção de).
- Provedores de valor (Registrando e injeção de).

MVC 3 oferece suporte a [Common Service Locator](https://github.com/unitycontainer/commonservicelocator) biblioteca e qualquer contêiner de injeção de dependência que oferece suporte a essa biblioteca `IServiceLocator` interface. Ele também oferece suporte a um novo `IDependencyResolver` interface que torna mais fácil de integrar DI estruturas.

Para obter mais informações sobre injeção de dependência no MVC 3, consulte os seguintes recursos:

- [Série de Brad Wilson de postagens de blog no local de serviço](http://bradwilson.typepad.com/blog/2010/07/service-location-pt1-introduction.html)
- [Notas de versão do MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_Other_New_Features"></a>

## <a name="other-new-features"></a>Outros novos recursos

### <a name="nuget-integration"></a>Integração do NuGet

ASP.NET MVC 3 automaticamente instala e habilita o NuGet como parte da sua instalação. NuGet é um Gerenciador de pacote de código-fonte aberto gratuito que torna mais fácil localizar, instalar e usar as ferramentas e bibliotecas do .NET em seus projetos. Ele funciona com todos os tipos de projeto do Visual Studio (incluindo o Web Forms do ASP.NET e ASP.NET MVC).

O NuGet permite aos desenvolvedores responsáveis pela manutenção de projetos de código aberto (por exemplo, projetos como Moq, NHibernate, Ninject, StructureMap, NUnit, Windsor, RhinoMocks e Elmah) para suas bibliotecas do pacote e registrá-los em uma galeria online. Em seguida, é fácil para os desenvolvedores de .NET que desejam usar uma dessas bibliotecas para localizar o pacote e instalá-lo em projetos que estão trabalhando.

Com a atualização de ferramentas do ASP.NET 3, modelos de projeto incluem pacotes de NuGet pré-instalados bibliotecas de JavaScript, para que eles fiquem atualizáveis por meio do NuGet. Entity Framework Code First também está pré-instalado como um pacote do NuGet.

Para saber mais sobre o NuGet, veja a [documentação do NuGet](https://docs.microsoft.com/nuget/).

### <a name="partial-page-output-caching"></a>Cache de saída de página parcial

ASP.NET MVC tem suporte para o cache de saída de respostas de página inteira desde a versão 1. MVC 3 também oferece suporte a cache de saída de página parcial, que permite que você facilmente as regiões de cache ou fragmentos de uma resposta. Para obter mais informações sobre armazenamento em cache, consulte o **cache de saída de página parcial** seção [postagem no blog de Scott Guthrie no MVC 3 versão release candidate](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx) e o **filho ação cache de saída** seção o [notas de versão do MVC 3](../whitepapers/mvc3-release-notes.md).

### <a name="granular-control-over-request-validation"></a>Controle granular sobre validação de solicitação

ASP.NET MVC tem validação de solicitação interna que automaticamente ajuda a proteger contra ataques de injeção XSS e HTML. No entanto, às vezes você deseja explicitamente desabilitar validação da solicitação, como se você deseja permitir que os usuários postagem HTML conteúdo (por exemplo, em entradas de blog ou conteúdo CMS). Agora você pode adicionar uma [AllowHtml](https://msdn.microsoft.com/library/system.web.mvc.allowhtmlattribute(v=VS.98).aspx) para modelos de atributo ou exibir modelos para desabilitar a validação de solicitação em uma base por propriedade durante a associação de modelo. Para obter mais informações sobre a validação de solicitação, consulte os seguintes recursos:

- O **JavaScript discreto e validação** seção [postagem no blog de Scott Guthrie no MVC 3 versão release candidate](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx).
- [Notas de versão do MVC 3](../whitepapers/mvc3-release-notes.md)

### <a name="extensible-new-project-dialog-box"></a>Extensível "novo projeto" caixa de diálogo

No ASP.NET MVC 3, você pode adicionar modelos de projeto, mecanismos de exibição, e as estruturas de projeto para teste de unidade de **novo projeto** caixa de diálogo.

### <a name="template-scaffolding-improvements"></a>Aprimoramentos de Scaffolding de modelo

Modelos de estrutura do ASP.NET MVC 3 realizam um trabalho melhor de identificação de propriedades de chave primária em modelos e resolvê-los adequadamente que em versões anteriores do MVC. (Por exemplo, os modelos de scaffolding agora Certifique-se de que a chave primária é Scaffold não como um campo de formulário editável.)

Por padrão, as criar e editar scaffolds agora usam o `Html.EditorFor` auxiliar em vez do `Html.TextBoxFor` auxiliar. Isso melhora o suporte para metadados no modelo na forma de dados de anotação de atributos quando o **adicionar exibição** caixa de diálogo gera uma exibição.

### <a name="new-overloads-for-htmllabelfor-and-htmllabelformodel"></a>Novas sobrecargas para "Html.LabelFor" e "Html.LabelForModel"

Foram adicionadas novas sobrecargas de método para o `LabelFor` e `LabelForModel` métodos auxiliares. Novas sobrecargas permitem que você especificar ou substituir o texto do rótulo.

### <a name="sessionless-controller-support"></a>Suporte ao controlador sem sessão

No ASP.NET MVC 3, você pode indicar se você deseja que uma classe de controlador para usar o estado da sessão e, nesse caso, se o estado da sessão deve ser leitura/gravação ou somente leitura. Para obter mais informações sobre o suporte de controlador sem sessão, consulte [notas de versão do MVC 3](../whitepapers/mvc3-release-notes.md).

### <a name="new-additionalmetadataattribute-class"></a>Nova classe de "AdditionalMetadataAttribute"

Você pode usar o [AdditionalMetadata](https://msdn.microsoft.com/library/system.web.mvc.additionalmetadataattribute(v=VS.98).aspx) atributo para preencher o `ModelMetadata.AdditionalValues` dicionário para uma propriedade de modelo. Por exemplo, se um modelo de exibição tem uma propriedade que deve ser exibida somente para um administrador, você pode anotar essa propriedade, conforme mostrado no exemplo a seguir:

[!code-csharp[Main](mvc3/samples/sample4.cs)]

Esses metadados ficam disponíveis para qualquer modelo de exibição ou editor quando um modelo de exibição do produto é renderizado. Cabe a você interpretar as informações de metadados.

### <a name="accountcontroller-improvements"></a>Melhorias do AccountController

AccountController no modelo de projeto de Internet foi aprimorada significativamente.

### <a name="new-intranet-project-template"></a>Novo modelo de projeto de Intranet

Há um novo modelo de projeto de Intranet que habilita a autenticação do Windows e remove o AccountController.
