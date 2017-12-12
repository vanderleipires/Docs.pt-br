---
uid: whitepapers/mvc3-release-notes
title: O ASP.NET MVC 3 | Microsoft Docs
author: rick-anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/06/2010
ms.topic: article
ms.assetid: f44c166e-7e91-48a0-a6f8-d9285f3594e5
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/mvc3-release-notes
msc.type: content
ms.openlocfilehash: a86fae5698c54a71cb598f508aa91e7d96d1b409
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-3"></a>O ASP.NET MVC 3
====================
- [Visão Geral](#overview)
- [Notas de instalação](#installation-notes)
- [Requisitos de software](#software-requirements)
- [Documentação](#documentation)
- [Suporte](#support)
- [Atualizando um projeto ASP.NET MVC 2 para o ASP.NET MVC 3 ferramentas de atualização](#upgrading)
- [O ASP.NET MVC 3 ferramentas de atualização (12 de abril de 2011)](#tu-changes)

    - [Caixa de diálogo "Adicionar controlador" agora pode scaffold controladores com código de acesso a dados e exibições](#tu-AddControllerDialog)
    - [Aprimoramentos para o "ASP.NET MVC 3 novo projeto" caixa de diálogo](#tu-ImprovementsNewDialogBox)
    - [Modelos de projeto agora incluem Modernizr 1.7](#tu-Modernizr)
    - [Modelos de projeto incluem versões atualizadas do jQuery, jQuery e jQuery UI validação](#tu-UpdatedJQuery)
    - [Modelos de projeto agora incluem o ADO.NET Entity Framework 4.1 como um pacote do NuGet pré-instalados](#tu-EF)
    - [Modelos de projeto incluem bibliotecas JavaScript como previamente instalados pacotes do NuGet](#tu-JavaScriptLibsNuget)
    - [Problemas conhecidos](#tu-KI)
- [RTM do ASP.NET MVC 3 (13 de janeiro de 2011)](#MVC3RTM)

    - [Alterar: A versão atualizada do jQuery UI para 1.8.7](#RTM-1)
    - [Alteração: Alterado o padrão ModelMetadataProvider de volta para DataAnnotationsModelMetadataProvider](#RTM-2)
    - [Correção: Colar parte de uma expressão Razor que contém resultados de espaço em branco em que ele está sendo revertida](#RTM-3)
    - [Correção: Renomear um arquivo Razor que é aberto no editor desabilita coloração de sintaxe e IntelliSense](#RTM-4)
    - [Problemas conhecidos](#RTM-KI)
    - [Alterações mais recentes](#RTM-BC)
- [O ASP.NET MVC 3 versão Release Candidate 2 (10 de dezembro de 2010)](#_Toc2)

    - [Modelos de projeto alterado para incluir jQuery 1.4.4, 1.7 de validação do jQuery e jQuery UI 1.8.6y 1.8.6 de interface do usuário](#_Toc2_1)
    - [Classe de "AdditionalMetadataAttribute" adicionado](#_Toc2_2)
    - [Exibição avançada Scaffolding](#_Toc2_3)
    - [Método de Html.Raw adicionado](#_Toc2_3)
    - [Propriedades renomeadas "Controller.ViewModel" e "View" para "ViewBag"](#_Toc2_4)
    - [Classe renomeado "ControllerSessionStateAttribute" para "SessionStateAttribute"](#_Toc2_5)
    - [Propriedade de "Campos" RemoteAttribute renomeado para "AdditionalFields"](#_Toc2_6)
    - [Renomeado "SkipRequestValidationAttribute" para "AllowHtmlAttribute"](#_Toc2_7)
    - [Método alterado "Html.ValidationMessage" para exibir a primeira mensagem de erro úteis](#_Toc2_8)
    - [Fixa @model declaração não adicionar espaço em branco ao documento](#_Toc2_9)
    - [Propriedade "FileExtensions" adicionado para mecanismos de exibição para dar suporte a nomes de arquivo específicas do mecanismo](#_Toc2_10)
    - [Auxiliar fixa "LabelFor" para emitir o valor correto para o atributo "For"](#_Toc2_11)
    - [Método fixa "RenderAction" dar prioridade valores explícitos durante a associação de modelo](#_Toc2_12)
    - [Alterações mais recentes](#_Toc2_BC)
    - [Problemas conhecidos](#_Toc2_KI)
- [O ASP.NET MVC 3 versão Release Candidate (9 de novembro de 2010)](#TOC_ASP_NET_3_RC)

    - [Novos recursos no ASP.NET MVC 3 RC](#_Toc276711785)
    - [Gerenciador de pacotes do NuGet](#_Toc276711786)
    - [Melhor "Novo projeto" caixa de diálogo](#_Toc276711787)
    - [Sem sessão controladores](#_Toc276711788)
    - [Novos atributos de validação](#_Toc276711789)
    - [Novas sobrecargas dos métodos de "LabelForModel" e "LabelFor"](#_Toc276711790)
    - [Ação filha cache de saída](#_Toc276711791)
    - [Aprimoramentos de caixa de diálogo "Adicionar modo de exibição"](#_Toc276711792)
    - [Validação de solicitação granular](#_Toc276711793)
    - [Alterações mais recentes](#_Toc276711794)
    - [Problemas conhecidos](#_Toc276711795)
- [AS PÁGINAS ASP. Notas de versão Beta MVC 3 (6 de outubro de 2010)](#TOC_ASP_NET_3_Beta)

    - [Novos recursos na versão Beta do ASP.NET MVC 3](#0.1__Toc274034215)
    - [Gerenciador de pacote NuPack](#0.1__Toc274034216)
    - [Caixa de diálogo Novo projeto de aprimorado](#0.1__Toc274034217)
    - [Maneira simplificada para especificar fortemente tipado modelos nos modos de exibição do Razor](#0.1__Toc274034218)
    - [Suporte a métodos auxiliares para novas páginas da Web do ASP.NET](#0.1__Toc274034219)
    - [Suporte de injeção de dependência adicional](#0.1__Toc274034220)
    - [Novo suporte para discreto Ajax com base em jQuery](#0.1__Toc274034221)
    - [Novo suporte para jQuery discreto validação](#0.1__Toc274034222)
    - [Novo aplicativo sinalizadores para a validação do cliente e o JavaScript discreto](#0.1__Toc274034223)
    - [Novo suporte para o código que é executado antes da execução de modos de exibição](#0.1__Toc274034224)
    - [Novo suporte para a sintaxe do Razor VBHTML](#0.1__Toc274034225)
    - [Controle mais Granular sobre ValidateInputAttribute](#0.1__Toc274034226)
    - [Auxiliares converter sublinhados hifens para nomes de atributo HTML especificados usando objetos anônimos](#0.1__Toc274034227)
    - [Correções de bugs](#0.1__Toc274034228)
    - [Alterações mais recentes](#0.1__Toc274034229)
    - [Problemas conhecidos](#0.1__Toc274034230)
- [Isenção de responsabilidade](#0.1__Toc274034231)

<a id="overview"></a>
## <a name="overview"></a>Visão Geral

Este documento descreve a versão do ASP.NET MVC 3 RTM para Visual Studio 2010. ASP.NET MVC é uma estrutura para desenvolvimento de aplicativos Web que usa o padrão Model-View-Controller (MVC). O instalador do ASP.NET MVC 3 inclui os seguintes componentes:

- Componentes de tempo de execução do ASP.NET MVC 3
- Ferramentas do ASP.NET MVC 3 Visual Studio 2010
- Componentes de tempo de execução de páginas da Web do ASP.NET
- Ferramentas do Visual Studio 2010 da páginas da Web ASP.NET
- Gerenciador de pacotes do Microsoft para .NET (NuGet)
- Uma atualização para o Visual Studio 2010 que habilita o suporte para a sintaxe do Razor. (Para obter detalhes, consulte o artigo da Base de Conhecimento 2483190.)

O conjunto completo de notas de versão para cada versão de pré-lançamento do ASP.NET MVC 3 pode ser encontrado no site do ASP.NET na seguinte URL:

https://www.ASP.NET/Learn/whitepapers/mvc3-Release-Notes

<a id="installation-notes"></a>
## <a name="installation-notes"></a>Notas de instalação

Para instalar o ASP.NET MVC 3 RTM usando o Web Platform Installer (Web PI), visite o seguinte:

[https://www.microsoft.com/Web/Gallery/Install.aspx?AppID=MVC3](https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3)

Como alternativa, você pode baixar o instalador para o ASP.NET MVC 3 RTM para Visual Studio 2010 da seguinte página:

https://go.microsoft.com/fwlink/?LinkId=208140

ASP.NET MVC 3 pode ser instalado e pode executar lado a lado com o ASP.NET MVC 2.

<a id="software-requirements"></a>
## <a name="software-requirements"></a>Requisitos de software

Os componentes de tempo de execução do ASP.NET MVC 3 exigem o software a seguir:

- .NET framework versão 4. 

    Ferramentas do ASP.NET MVC 3 Visual Studio 2010 exigem o software a seguir:
- Visual Studio 2010 ou Visual Web Developer 2010 Express.

<a id="documentation"></a>
## <a name="documentation"></a>Documentação

Documentação para o ASP.NET MVC está disponível no site do MSDN na seguinte URL:

[https://go.microsoft.com/fwlink/?LinkId=205717](https://go.microsoft.com/fwlink/?LinkId=205717)

Tutoriais e outras informações sobre o ASP.NET MVC estão disponíveis na página do MVC do site da Web do ASP.NET na seguinte URL:

[https://www.ASP.NET/MVC/](../mvc/index.md)

<a id="support"></a>
## <a name="support"></a>Suporte

Esta é uma versão com suporte total. Informações sobre como obter suporte técnico, visite o [site do Microsoft Support](https://support.microsoft.com/).

Também fique à vontade para postar dúvidas sobre esta versão para o fórum do ASP.NET MVC, onde os membros da comunidade do ASP.NET são frequentemente pode oferecer suporte informal:

[https://forums.ASP.NET/1146.aspx](https://forums.asp.net/1146.aspx)

<a id="upgrading"></a>
## <a name="upgrading-an-aspnet-mvc-2-project-to-aspnet-mvc-3-tools-update"></a>Atualizando um projeto ASP.NET MVC 2 para o ASP.NET MVC 3 ferramentas de atualização

ASP.NET MVC 3 pode ser instalado lado a lado com o ASP.NET MVC 2 no mesmo computador, o que proporciona flexibilidade na escolha quando atualizar um aplicativo ASP.NET MVC 2 para o ASP.NET MVC 3.

Para atualizar manualmente um aplicativo ASP.NET MVC 2 existente para a versão 3, faça o seguinte:

1. Crie um novo projeto vazio do ASP.NET MVC 3 em seu computador. Este projeto contém alguns arquivos que são necessários para a atualização.
2. Copie os seguintes arquivos do projeto ASP.NET MVC 3 para o local correspondente de seu projeto ASP.NET MVC 2. Você precisará atualizar as referências à biblioteca jQuery para levar em conta o novo nome de arquivo (1.5.1.js jQuery): 

    - /Views/Web.config
    - /Packages.config
    - /scripts/\*. js
    - /Conteúdo/temas/\*.\*
3. Copie o *pacotes* pasta na raiz da solução de projeto ASP.NET MVC 3 vazia para a raiz de sua solução, que está no diretório onde o arquivo da solução está localizado.
4. Se seu projeto ASP.NET MVC 2 contém todas as áreas, copie o arquivo de /Views/Web.config para o *exibições* pasta de cada área.
5. Em ambos os arquivos Web. config no projeto ASP.NET MVC 2, globalmente pesquisar e substituir a versão do ASP.NET MVC. Localize o seguinte: 

    [!code-console[Main](mvc3-release-notes/samples/sample1.cmd)]

    Substitua-o com o seguinte:

    [!code-console[Main](mvc3-release-notes/samples/sample2.cmd)]
6. No Gerenciador de soluções, exclua a referência ao *System.Web.Mvc* (que aponta para a DLL de versão 2), em seguida, adicione uma referência a *System.Web.Mvc* (v3.0.0.0).
7. Adicione uma referência para System.Web.WebPages.dll e System.Web.Helpers.dll. Esses assemblies estão localizados nas seguintes pastas: 

    - %ProgramFiles%\ Microsoft ASP.NET\ASP.NET MVC 3\Assemblies
    - %ProgramFiles%\ Microsoft ASP.NET\ASP.NET Web Pages\v1.0\Assemblies
8. No Gerenciador de soluções, clique no nome do projeto e selecione Unload Project. Em seguida, clique no nome do projeto novamente e selecione Editar *ProjectName*. csproj.
9. Localize o *ProjectTypeGuids* elemento e substitua {F85E285D-A4E0-4152-9332-AB1D724D3325} por {E53F8FEA-EAE0-44A6-8774-FFD645390401}.
10. Salvar as alterações, clique com o botão direito e, em seguida, selecione Recarregar projeto.
11. No arquivo de Web. config de raiz do aplicativo, adicione as seguintes configurações para o *assemblies* seção. 

    [!code-xml[Main](mvc3-release-notes/samples/sample3.xml)]
12. Se o projeto referencia todas as bibliotecas de terceiros que são compiladas usando o ASP.NET MVC 2, adicione o seguinte realçado *bindingRedirect* elemento para o arquivo Web. config na raiz do aplicativo sob o  *configuração* seção: 

    [!code-xml[Main](mvc3-release-notes/samples/sample4.xml)]

<a id="tu-changes"></a>
## <a name="changes-in-aspnet-mvc-3-tools-update"></a>Alterações no ASP.NET MVC 3 ferramentas de atualização

Esta seção descreve as alterações feitas na versão de atualização de ferramentas do ASP.NET MVC 3 desde a versão RTM do ASP.NET MVC 3.

<a id="tu-AddControllerDialog"></a>
### <a name="add-controller-dialog-box-can-now-scaffold-controllers-with-views-and-data-access-code"></a>Caixa de diálogo "Adicionar controlador" agora pode scaffold controladores com código de acesso a dados e exibições

Scaffolding é uma maneira de gerar rapidamente um controlador e modos de exibição para seu aplicativo. O código foi gerado, você pode editá-lo para atender às necessidades do seu projeto.

Para iniciar o *Adicionar controlador* caixa de diálogo no ASP.NET MVC 3, clique o *controladores* pasta *Solution Explorer*, clique em *adicionar*e, em seguida, clique em *controlador*. A caixa de diálogo foi aprimorada para oferecer opções adicionais de scaffolding.

![](mvc3-release-notes/_static/image1.png)

Existem três modelos de scaffolding disponíveis por padrão.

#### <a name="empty-controller"></a>Controlador vazio

Esse modelo gera um arquivo de controlador vazio. Este modelo é equivalente a selecionar não *ações para criar, editar, detalhes de adicionar, excluir cenários* nas versões anteriores do ASP.NET MVC. Se você escolher essa opção, não há opções adicionais estão disponíveis.

#### <a name="controller-with-empty-readwrite-actions"></a>Controlador com ações de leitura/gravação vazias

Esse modelo gera um arquivo de controlador que tem todos os métodos de ação necessária, mas nenhum código de implementação nos métodos. Este modelo é equivalente a selecionar *ações para criar, editar, detalhes de adicionar, excluir cenários* nas versões anteriores do ASP.NET MVC. Se você escolher essa opção, não há opções adicionais estão disponíveis.

#### <a name="controller-with-readwrite-actions-and-views-using-entity-framework"></a>Controlador com ações de leitura/gravação e modos de exibição, usando o Entity Framework

Este modelo permite que você crie rapidamente uma interface de usuário de entrada de dados de trabalho. Ele gera o código que manipula uma variedade de cenários, como o seguinte e requisitos comuns:

- *Acesso a dados*. O código gerado lê e grava as entidades em um banco de dados. Ele funciona com a abordagem de Entity Framework Code First, se você escolher uma classe de contexto de dados existente ou se você permitir que o modelo de gerar um novo *DbContext* classe. Também funciona com a abordagem de banco de dados do Entity Framework First ou Model First se você escolher um existente *ObjectContext* classe.
- *Validação*. O código gerado usa associação de modelo do ASP.NET MVC e recursos de metadados para que os envios de formulário são validados de acordo com a regras declaradas em sua classe de modelo. Isso inclui as regras de validação, como o *necessário* e *StringLength* atributos e regras de validação personalizadas.
- *Relações um-para-muitos*. Se você definir relações de chave estrangeira de um-para-muitos entre as classes de modelo, o código gerado produzirá listas suspensas para selecionar as entidades relacionadas. Por exemplo, você pode definir as seguintes classes de modelo do Entity Framework Code First convenções a seguir: 

    [!code-csharp[Main](mvc3-release-notes/samples/sample5.cs)]

    Quando você scaffold, em seguida, um controlador para o *produto* classe, seus modos de exibição permitirá que os usuários escolham um *categoria* objeto para cada *produto* instância.

    Esse modelo permite que opções adicionais de *Adicionar controlador* caixa de diálogo. Para *classe modelo*, você pode escolher qualquer classe de modelo em sua solução, que determina o tipo de dados que os usuários poderão criar ou editar:
- Se você quiser usar o Entity Framework Code First, você pode escolher qualquer classe de modelo.
- Se você estiver usando o Entity Framework Model First ou banco de dados do Entity Framework First, certifique-se de escolher uma classe de entidade definida no modelo conceitual.

Para *classe de contexto de dados*, você pode fazer essas opções:

- Se você quiser usar o Code First e não têm nenhum contexto de dados existente de classe, escolha  *&lt;novo contexto de dados... &gt;*". Uma classe de contexto de dados, em seguida, será gerada para você.
- Se você quiser usar o Code First e ter uma classe de contexto de dados existente, escolha-o aqui. Ele será atualizado para manter a classe de modelo que você selecionou.
- Se você estiver usando o Database First ou Model First, escolha sua classe de contexto de objeto aqui.

Para exibições, escolha o mecanismo de exibição que você deseja usar ou escolha Nenhum se não desejar scaffold nenhum modo de exibição.

Você pode selecionar opções assíncronapara avançado especificar mais opções para os modos de exibição gerados. Por exemplo, você pode escolher o layout ou página mestra.

<a id="tu-ImprovementsNewDialogBox"></a>
### <a name="improvements-to-the-aspnet-mvc-3-new-project-dialog-box"></a>Aprimoramentos para o "ASP.NET MVC 3 novo projeto" caixa de diálogo

A caixa de diálogo, que você pode usar para criar novos projetos do ASP.NET MVC 3 inclui vários aprimoramentos, como listado abaixo.

![](mvc3-release-notes/_static/image2.png)

#### <a name="new-intranet-project-template"></a>Novo modelo de "Projeto Intranet"

A lista de modelos de projeto inclui um novo modelo de aplicativo de Intranet. Este modelo contém configurações para a criação de um aplicativo web usando autenticação do Windows em vez da autenticação de formulários. Como um aplicativo de intranet requer algumas configurações do IIS que não podem ser encapsuladas em um modelo de projeto, o modelo inclui um arquivo Leiame com instruções sobre como tornar o modelo de projeto funcionam no IIS. Documentação para o novo modelo de aplicativo Intranet está disponível no site do MSDN na seguinte URL:

[https://msdn.microsoft.com/en-us/library/gg703322 (VS.98).aspx](https://msdn.microsoft.com/en-us/library/gg703322(VS.98).aspx)

#### <a name="project-templates-are-now-html5-enabled"></a>Modelos de projeto estão agora HTML5 habilitado

Agora, a caixa de diálogo Novo projeto contém uma opção para adicionar recursos específicos do HTML5 para os modelos de projeto. Selecionando a opção faz com que os modos de exibição a ser gerado que contêm o HTML5 novo  *&lt;cabeçalho&gt;*,  *&lt;rodapé&gt;*, e  *&lt;navegação&gt;*  elementos.

Observe que as versões anteriores dos navegadores não dão suporte a marcas específicas do HTML5. Para resolver essa limitação, os modelos de projeto do HTML5 incluem uma referência para a biblioteca Modernizr. (Consulte a próxima seção).

<a id="tu-Modernizr"></a>
### <a name="project-templates-now-include-modernizr-17"></a>Modelos de projeto agora incluem Modernizr 1.7

Modernizr é uma biblioteca de JavaScript que habilita o suporte para CSS 3 e HTML5 em navegadores que ainda não dão suporte a esses recursos. Essa biblioteca é incluída como um pacote do NuGet pré-instalados nos modelos de projetos do ASP.NET MVC 3. Para obter mais informações sobre Modernizr, consulte [http://www.modernizr.com/](http://www.modernizr.com/).

<a id="tu-UpdatedJQuery"></a>
### <a name="project-templates-include-updated-versions-of-jquery-jquery-ui-and-jquery-validation"></a>Modelos de projeto incluem versões atualizadas do jQuery, jQuery e jQuery UI validação

Agora, os modelos de projeto incluem as seguintes versões dos scripts jQuery:

- jQuery 1.5.1
- jQuery 1.8 de validação
- jQuery UI 1.8.11

Essas bibliotecas são incluídas como pacotes do NuGet pré-instalados.

<a id="tu-EF"></a>
### <a name="project-templates-now-include-adonet-entity-framework-41-as-a-pre-installed-nuget-package"></a>Modelos de projeto agora incluem o ADO.NET Entity Framework 4.1 como um pacote do NuGet pré-instalados

O ADO.NET Entity Framework 4.1 inclui o recurso de Code First. Primeiro, o código é um novo padrão de desenvolvimento para o ADO.NET Entity Framework que fornece uma alternativa para os padrões Database First e Model First existentes.

Código primeiro se concentra em definir seu modelo usando classes POCO ("plain old CLR objects") escritos em Visual Basic ou c#. Essas classes, em seguida, podem ser mapeadas para um banco de dados ou ser usadas para gerar um esquema de banco de dados. Configuração adicional pode ser fornecida usando *DataAnnotations* atributos ou usando APIs fluente.

Documentação para usar o código Firstwith ASP.NET MVC está disponível no site do ASP.NET nas seguintes URLs:

[https://www.ASP.NET/MVC/Tutorials/Getting-Started-with-mvc3-part1-CS](../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) [https://www.asp.net/entity-framework/tutorials/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)

<a id="tu-JavaScriptLibsNuget"></a>
### <a name="project-templates-include-javascript-libraries-as-pre-installed-nuget-packages"></a>Modelos de projeto incluem bibliotecas JavaScript como previamente instalados pacotes do NuGet

Quando você cria um novo projeto ASP.NET MVC 3, o projeto inclui os arquivos do JavaScript mencionados anteriormente (por exemplo, a biblioteca Modernizr) instalando-os usando o NuGet em vez de adicionar diretamente os scripts para a pasta de Scripts no modelo de projeto conteúdo. Isso permite que você use NuGet para atualizar os scripts para a versão mais recente quando são lançadas novas versões dos scripts.

Por exemplo, dada a frequência de novos lançamentos jQuery, a versão do jQuery incluída no modelo de projeto em algum momento será desatualizada. No entanto, como jQuery é incluído como um pacote do NuGet instalado, você será notificado na caixa de diálogo NuGet quando versões mais recentes do jQuery estão disponíveis.

Como jQuery inclui o número de versão no nome do arquivo, também jQuery para a versão mais recente de atualização requer a atualização do  *&lt;script&gt;*  marca que faz referência ao arquivo jQuery para usar o novo nome de arquivo. Outras bibliotecas de script incluídos não incluem o número de versão no nome do script, para que eles possam ser atualizados mais facilmente suas versões mais recentes.

<a id="tu-KI"></a>
## <a name="known-issues"></a>Problemas conhecidos

- Em alguns casos, a instalação pode falhar com o erro mensagem "Falha na instalação com o código de erro (0x80070643)". Para obter informações sobre como solucionar esse problema, consulte [artigo da Base de Conhecimento 2531566](https://support.microsoft.com/kb/2531566).
- O scaffolding para adicionar um controlador não scaffold entidades que tiram proveito do suporte a herança de entidade dentro do Entity Framework. Por exemplo, dada uma base de *pessoa* classe é herdada por um *aluno* classe, estrutura de *aluno* classe resultará no código gerado não compila.
- Criar um novo projeto ASP.NET MVC 3 dentro de uma pasta de solução faz com que um *NullReferenceException* erro. A solução alternativa é criar o projeto ASP.NET MVC 3 na raiz da solução e, em seguida, mova-o para a pasta de solução.
- IntelliSense para a sintaxe do Razor não funciona quando ReSharper está instalado. Se você tiver ReSharper instalado e quiser aproveitar o suporte do IntelliSense Razor em ASP.NET MVC 3, consulte a entrada [Razor Intellisense e ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) no blog de Hadi Hariri, que descreve maneiras de usá-los juntos hoje.
- Durante a instalação, a caixa de diálogo de aceitação de EULA exibe os termos de licença em uma janela que é menor do que se destina.
- Quando você estiver editando uma exibição do Razor (. cshtml ou. *vbhtml* arquivo), modos de exibição. ASP.NET MVC 3 não inclui qualquer trechos de código para modos de exibição do Razor. aspxselecting um trecho de código para o ASP.NET MVC mostrará os trechos de código para
- Se você instala o ASP.NET MVC 3 do Visual Web Developer Express em um computador onde o Visual Studio não está instalado e depois instala o Visual Studio, você deverá reinstalar o ASP.NET MVC 3. O Visual Studio e o Visual Web Developer Express compartilham componentes que são atualizados pelo instalador do ASP.NET MVC 3. O mesmo problema se aplica se você instalar o ASP.NET MVC 3 para o Visual Studio em um computador que não tenha o Visual Web Developer Express e, em seguida, instalar posteriormente Visual Web Developer Express.

<a id="MVC3RTM"></a>
## <a name="changes-in-aspnet-mvc-3-rtm"></a>Alterações no RTM do ASP.NET MVC 3

Esta seção descreve as alterações e correções feitas na versão RTM do ASP.NET MVC 3 desde a versão RC2.

<a id="RTM-1"></a>
### <a name="change-updated-the-version-of-jquery-ui-to-187"></a>Alterar: A versão atualizada do jQuery UI para 1.8.7

Os modelos de projeto do ASP.NET MVC para o Visual Studio foram atualizados para incluir a versão mais recente da biblioteca de interface do usuário do jQuery. Os modelos também incluem o conjunto mínimo de arquivos de recursos exigidos pelo jQuery UI, como o CSS associados e arquivos de imagem.

<a id="RTM-2"></a>
### <a name="change-changed-the-default-modelmetadataprovider-back-to-dataannotationsmodelmetadataprovider"></a>Alteração: Alterado o padrão ModelMetadataProvider de volta para DataAnnotationsModelMetadataProvider

A versão RC2 do ASP.NET MVC 3 introduzidos um *CachedDataAnnotationsMetadataProvider* classe que forneceu cache sobre existente *DataAnnotationsModelMetadataProvider* classe como um melhoria no desempenho. No entanto, alguns bugs foram relatados com essa implementação, para que a alteração foi revertida e movida para o projeto MVC futuros, que está disponível em [WebStack ASP.NET](https://github.com/aspnet/AspNetWebStack).

<a id="RTM-3"></a>
### <a name="fixed-pasting-part-of-a-razor-expression-that-contains-whitespace-results-in-it-being-reversed"></a>Correção: Colar parte de uma expressão Razor que contém resultados de espaço em branco em que ele está sendo revertida

Em versões de pré-lançamento do ASP.NET MVC 3, quando você colar uma parte de uma expressão Razor que contém espaço em branco em um arquivo Razor, a expressão resultante é invertida. Por exemplo, considere o seguinte bloco de código Razor:

[!code-cshtml[Main](mvc3-release-notes/samples/sample6.cshtml)]

Se você selecionar o texto "primeiro param" no primeiro método e colá-lo como um argumento para o segundo método, o resultado é o seguinte:

[!code-cshtml[Main](mvc3-release-notes/samples/sample7.cshtml)]

O comportamento correto é que a operação de colagem deve resultar no exemplo a seguir:

[!code-cshtml[Main](mvc3-release-notes/samples/sample8.cshtml)]

Esse problema foi corrigido na versão RTM, para que a expressão corretamente é preservada durante a operação de colagem.

<a id="RTM-4"></a>
### <a name="fixed-renaming-a-razor-file-that-is-opened-in-the-editor-disables-syntax-colorization-and-intellisense"></a>Correção: Renomear um arquivo Razor que é aberto no editor desabilita coloração de sintaxe e IntelliSense

Renomear um arquivo Razor usando o Gerenciador de soluções enquanto o arquivo é aberto na janela do editor faz com que o realce de sintaxe e IntelliSense parem de funcionar para esse arquivo. Esse problema foi corrigido para que o realce e IntelliSense são mantidas após uma renomeação.

<a id="RTM-KI"></a>
## <a name="known-issues"></a>Problemas conhecidos

- Se você fechar o Visual Studio 2010 SP1 Beta enquanto o NuGet Package Manager Console está aberto, o Visual Studio falhar e tenta reiniciar. Isso será corrigido na versão RTM do Visual Studio 2010 SP1.
- O instalador do ASP.NET MVC 3 somente é possível instalar uma versão inicial do Gerenciador de pacotes do NuGet. Depois que você instalou a versão inicial, o NuGet pode ser instalado e atualizados usando o Gerenciador de extensões do Visual Studio. Se você já tiver instalado o NuGet, vá para a Galeria de extensão do Visual Studio para atualizar para a versão mais recente do NuGet.
- Criar um novo projeto ASP.NET MVC 3 dentro de uma pasta de solução faz com que um *NullReferenceException* erro. A solução alternativa é criar o projeto ASP.NET MVC 3 na raiz da solução e, em seguida, mova-o para a pasta de solução.
- O instalador pode levar mais tempo do que as versões anteriores do ASP.NET MVC para concluir. Isso ocorre porque ele atualiza os componentes do Visual Studio 2010.
- IntelliSense para a sintaxe do Razor não funciona quando ReSharper está instalado. Se você tiver ReSharper instalado e quiser aproveitar o suporte do IntelliSense Razor em ASP.NET MVC 3, consulte a entrada [Razor Intellisense e ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) no blog de Hadi Hariri, que descreve maneiras de usá-los juntos hoje.
- CCSHTML e VBHTML exibições criadas com a versão Beta do ASP.NET MVC 3 têm suas ações de compilação definida corretamente, sabendo que essas exibir tipos são omitidos quando o projeto é publicado. O valor de ação de compilação para esses arquivos deve ser definido como "Conteúdo". RTM do ASP.NET MVC 3 corrige esse problema para novos arquivos, mas não corrigir a configuração de arquivos existentes para um projeto criado com as versões de pré-lançamento.
- ![](mvc3-release-notes/_static/image3.png)
- Durante a instalação, a caixa de diálogo de aceitação de EULA exibe os termos de licença em uma janela que é menor do que pretendido. / li&gt;
- Quando você estiver editando um modo de exibição do Razor (cshtml arquivo), o item de menu Ir para o controlador no Visual Studio não estará disponível e não há nenhum trechos de código.
- Se você instala o ASP.NET MVC 3 do Visual Web Developer Express em um computador onde o Visual Studio não está instalado e depois instala o Visual Studio, você deverá reinstalar o ASP.NET MVC 3. O Visual Studio e o Visual Web Developer Express compartilham componentes que são atualizados pelo instalador do ASP.NET MVC 3. O mesmo problema se aplica se você instalar o ASP.NET MVC 3 para o Visual Studio em um computador que não tenha o Visual Web Developer Express e, em seguida, instalar posteriormente Visual Web Developer Express.

<a id="RTM-BC"></a>
## <a name="breaking-changes"></a>Alterações significativas

- Em versões anteriores do ASP.NET MVC, a ação filtros são criar por solicitação, exceto em alguns casos. Esse comportamento nunca foi um comportamento garantido, mas apenas um detalhe de implementação e o contrato para filtros era considerá-las sem monitoração de estado. No ASP.NET MVC 3, os filtros são armazenados em cache mais agressiva. Portanto, os filtros de ação personalizada que incorretamente armazenam o estado da instância podem ser interrompidos.
- A ordem de execução de filtros de exceção foi alterado para filtros de exceção que têm o mesmo *ordem* valor. No ASP.NET MVC 2 e versões anteriores, filtros de exceção no controlador que têm o mesmo *ordem* valor, como aqueles em um método de ação são executados antes dos filtros de exceção no método de ação. Isso normalmente seria o caso quando os filtros de exceção são aplicados sem um especificado *ordem* valor. ASP.NET MVC 3, nesta ordem foi revertida para que o manipulador de exceção mais específico é executado pela primeira vez. Como nas versões anteriores, se o *ordem* propriedade seja explicitamente especificada, os filtros são executados na ordem especificada.
- Uma nova propriedade chamada *FileExtensions* foi adicionado para o *VirtualPathProviderViewEngine* classe base. Quando o ASP.NET procura uma exibição pelo caminho (e não por nome), somente exibições com uma extensão de arquivo contidos na lista especificada por essa nova propriedade são consideradas. Isso é uma alteração significativa em aplicativos onde um provedor de compilação personalizada é registrado para habilitar uma extensão de arquivo personalizados para modos de exibição de formulário da Web e o provedor faz referência a esses modos de exibição usando um caminho completo em vez de um nome. A solução é modificar o valor da *FileExtensions* propriedade inclua a extensão de arquivo personalizado.
- Implementações de fábrica do controlador personalizado que implementam diretamente o *IControllerFactory* interface deve fornecer uma implementação do novo *GetControllerSessionBehavior* método adicionado à interface nesta versão. Em geral, é recomendável que você não implementar essa interface diretamente e em vez disso, derive a classe de *DefaultControllerFactory*.

<a id="_Toc2"></a>
## <a name="changes-in-aspnet-mvc-3-rc2"></a>Alterações no ASP.NET MVC 3 RC2

Esta seção descreve alterações (novos recursos e correções de bugs) feitas na versão do ASP.NET MVC 3 RC2 desde a versão RC.

<a id="_Toc2_1"></a>
### <a name="project-templates-changed-to-include-jquery-144-jquery-validation-17-and-jquery-ui-186"></a>Modelos de projeto alterado para incluir jQuery 1.4.4, 1.7 de validação do jQuery e jQuery UI 1.8.6

Os modelos de projeto do ASP.NET MVC 3 agora incluem as versões mais recentes do jQuery e jQuery validação jQuery UI. jQuery UI é uma novidade para os modelos de projeto e fornece widgets de interface do usuário útil. Para obter mais informações sobre o jQuery UI, visite sua home page: [http://jqueryui.com/](http://jqueryui.com/).

<a id="_Toc2_2"></a>
### <a name="added-additionalmetadataattribute-class"></a>Classe de "AdditionalMetadataAttribute" adicionado

Você pode usar o *AdditionalMetadataAttribute* classe para preencher o *ModelMetadata.AdditionalValues* dicionário para uma propriedade de modelo.

Por exemplo, suponha que um modelo de exibição tem propriedades que devem ser exibidas apenas a um administrador. Esse modelo pode ser anotado com o novo atributo usando AdminOnly como a chave e true como o valor, como no exemplo a seguir:

[!code-csharp[Main](mvc3-release-notes/samples/sample9.cs)]

Esses metadados ficam disponíveis para qualquer modelo de exibição ou editor quando um modelo de exibição do produto é renderizado. É cabe a você como desenvolvedor de aplicativos para interpretar as informações de metadados.

<a id="_Toc2_3"></a>
### <a name="improved-view-scaffolding"></a>Exibição avançada Scaffolding

Os modelos T4 usados para exibições de scaffolding agora geram chamadas para métodos de auxiliares de modelo como *EditorFor* em vez de auxiliares, como *TextBoxFor*. Essa alteração melhora o suporte para metadados no modelo na forma de atributos de anotação de dados quando a caixa de diálogo Adicionar modo de exibição gera uma exibição.

O scaffolding adicionar modo de exibição também inclui detecção aprimorada e uso de informações de chave primária no modelo, com base na convenção. Por exemplo, a caixa de diálogo Adicionar modo de exibição usa essas informações para garantir que o valor de chave primária é Scaffold não como um campo de formulário editável.

O padrão de editar e criar modelos de incluem referências a scripts jQuery necessários para a validação do cliente.

<a id="_Toc2_4"></a>
### <a name="added-htmlraw-method"></a>Método de Html.Raw adicionado

Por padrão, o Razor de mecanismo de exibição HTML codifica todos os valores. Por exemplo, o trecho de código a seguir codifica o HTML dentro a variável de saudação para que ele seja exibido na página como &amp;lt; forte&amp;gt; Olá, mundo! &amp;lt; / forte&amp;gt;.

[!code-cshtml[Main](mvc3-release-notes/samples/sample10.cshtml)]

O novo *Html.Raw* método fornece uma maneira simples de exibição de HTML não codificado quando o conteúdo é conhecido para ser seguro. O exemplo a seguir exibe a mesma cadeia de caracteres, mas a cadeia de caracteres é renderizada como marcação:

[!code-cshtml[Main](mvc3-release-notes/samples/sample11.cshtml)]

<a id="_Toc2_5"></a>
### <a name="renamed-controllerviewmodel-property-and-the-view-property-to-viewbag"></a>Propriedades renomeadas "Controller.ViewModel" e "View" para "ViewBag"

Anteriormente, o *ViewModel* propriedade *controlador* correspondia ao *exibição* propriedade de uma exibição. Essas duas propriedades fornecem uma maneira para acessar valores da *ViewDataDictionary* usando a sintaxe de acessador de propriedade dinâmica de objeto. Ambas as propriedades foram renomeadas para ser o mesmo para evitar confusão e ser mais consistente.

<a id="_Toc2_6"></a>
### <a name="renamed-controllersessionstateattribute-class-to-sessionstateattribute"></a>Classe renomeado "ControllerSessionStateAttribute" para "SessionStateAttribute"

O *ControllerSessionStateAttribute* classe foi introduzida na versão RC do ASP.NET MVC 3. A propriedade foi renomeada para ser mais sucinta.

<a id="_Toc2_7"></a>
### <a name="renamed-remoteattribute-fields-property-to-additionalfields"></a>Propriedade de "Campos" RemoteAttribute renomeado para "AdditionalFields"

O *RemoteAttribute* da classe *campos* propriedade causado alguma confusão entre usuários. Renomear essa propriedade como *AdditionalFields* esclarece sua intenção.

<a id="_Toc2_8"></a>
### <a name="renamed-skiprequestvalidationattribute-to-allowhtmlattribute"></a>Renomeado "SkipRequestValidationAttribute" para "AllowHtmlAttribute"

O *SkipRequestValidationAttribute* atributo foi renomeado para *AllowHtmlAttribute* representar melhor seu uso pretendido.

<a id="_Toc2_9"></a>
### <a name="changed-htmlvalidationmessage-method-to-display-the-first-useful-error-message"></a>Método alterado "Html.ValidationMessage" para exibir a primeira mensagem de erro úteis

O *Html.ValidationMessage* método foi corrigido para mostrar a primeira mensagem de erro úteis em vez de simplesmente exibir o primeiro erro.

Durante a associação de modelo, o *ModelState* dicionário pode ser preenchido de várias fontes com mensagens de erro sobre a propriedade, inclusive o próprio modelo (se ele implementa *IValidatableObject* ), de atributos de validação aplicados à propriedade e de exceções lançadas enquanto a propriedade está sendo acessada.

Quando o *Html.ValidationMessage* método exibe uma mensagem de validação, ele ignora as entradas de estado de modelo que incluem uma exceção, porque eles geralmente não são destinados ao usuário final. Em vez disso, o método procura a primeira mensagem de validação que não está associada uma exceção e exibe essa mensagem. Se essa mensagem não for encontrada, o padrão é uma mensagem de erro genérica que está associada com a primeira exceção.

<a id="_Toc2_10"></a>
### <a name="fixed-model-declaration-to-not-add-whitespace-to-the-document"></a>Fixa @model declaração não adicionar espaço em branco ao documento

Em versões anteriores, o  *@model*  declaração na parte superior de uma exibição adicionada uma linha em branco para a saída HTML renderizada. Esse problema foi corrigido para que a declaração não apresenta um espaço em branco.

<a id="_Toc2_11"></a>
### <a name="added-fileextensions-property-to-view-engines-to-support-engine-specific-file-names"></a>Propriedade "FileExtensions" adicionado para mecanismos de exibição para dar suporte a nomes de arquivo específicas do mecanismo

Um mecanismo de exibição pode retornar uma exibição usando um caminho de exibição explícita como no exemplo a seguir:

[!code-csharp[Main](mvc3-release-notes/samples/sample12.cs)]

O mecanismo de exibição primeiro sempre tenta renderizar a exibição. Por padrão, o mecanismo de exibição de formulários da Web é o mecanismo de exibição primeiro; porque o mecanismo de formulários da Web não pode processar um modo de exibição Razor, ocorrerá um erro. Mecanismos de exibição agora tem um *FileExtensions* propriedade que é usada para especificar quais extensões de arquivo que eles suportam. Essa propriedade é verificada quando o ASP.NET determina se um mecanismo de exibição pode processar um arquivo. Isso é uma alteração significativa e mais detalhes são incluídos na [alterações significativas](#_Toc2_BC) seção deste documento.

<a id="_Toc2_12"></a>
### <a name="fixed-labelfor-helper-to-emit-the-correct-value-for-the-for-attribute"></a>Auxiliar fixa "LabelFor" para emitir o valor correto para o atributo "For"

Um bug foi corrigido onde o *LabelFor* método renderizado uma *para* atributo que corresponde a *entrada* do elemento *nome* atributo em vez disso de sua ID. Acordo com o W3C, o *para* atributo deve corresponder a *entrada* ID. do elemento

<a id="_Toc2_13"></a>
### <a name="fixed-renderaction-method-to-give-explicit-values-precedence-during-model-binding"></a>Método fixa "RenderAction" dar prioridade valores explícitos durante a associação de modelo

Em versões anteriores, os valores explícitos foram passados para o *RenderAction* método foram sendo ignorado em favor de valores de formulário atual durante a associação de modelo dentro de uma ação filha. A correção garante que os valores explícitos têm precedência durante a associação de modelo.

<a id="_Toc2_BC"></a>
## <a name="breaking-changes"></a>Alterações significativas

- Em versões anteriores do ASP.NET MVC, filtros de ação foram criados por solicitação, exceto em alguns casos. Esse comportamento nunca foi um comportamento garantido, mas apenas um detalhe de implementação e o contrato para filtros era considerá-las sem monitoração de estado. No ASP.NET MVC 3, os filtros são armazenados em cache mais agressiva. Portanto, os filtros de ação personalizada que incorretamente armazenam o estado da instância podem ser interrompidos.
- A ordem de execução de filtros de exceção foi alterado para filtros de exceção que têm o mesmo *ordem* valor. No ASP.NET MVC 2 e versões anteriores, filtros de exceção no controlador que tinha o mesmo *ordem* valor, como aqueles em um método de ação foram executados antes dos filtros de exceção no método de ação. Isso normalmente seria o caso quando foram aplicados filtros de exceção sem uma especificado *ordem* valor. ASP.NET MVC 3, nesta ordem foi revertida para que o manipulador de exceção mais específico é executado pela primeira vez. Como nas versões anteriores, se o *ordem* propriedade seja explicitamente especificada, os filtros são executados na ordem especificada.
- Uma nova propriedade chamada *FileExtensions* foi adicionado para o *VirtualPathProviderViewEngine* classe base. Quando o ASP.NET procura uma exibição pelo caminho (e não por nome), somente exibições com uma extensão de arquivo contidos na lista especificada por essa nova propriedade são consideradas. Isso é uma alteração significativa em aplicativos onde um provedor de compilação personalizada é registrado para habilitar uma extensão de arquivo personalizados para modos de exibição de formulário da Web e o provedor faz referência a esses modos de exibição usando um caminho completo em vez de um nome. A solução é modificar o valor da *FileExtensions* propriedade inclua a extensão de arquivo personalizado.
- Implementações de fábrica do controlador personalizado que implementam diretamente o *IControllerFactory* interface deve fornecer uma implementação do novo *GetControllerSessionBehavior*  *método que foi adicionado à interface nesta versão*. Em geral, é recomendável que você não implementar essa interface diretamente e em vez disso, derive a classe de *DefaultControllerFactory*.

<a id="_Toc2_KI"></a>
## <a name="known-issues"></a>Problemas conhecidos

- O instalador do ASP.NET MVC 3 somente é possível instalar uma versão inicial do Gerenciador de pacotes do NuGet. Depois que você instalou a versão inicial, o NuGet pode ser instalado e atualizados usando o Gerenciador de extensões do Visual Studio. Se você já tiver instalado o NuGet, vá para a Galeria de extensão do Visual Studio para atualizar para a versão mais recente do NuGet.
- Criar um novo projeto ASP.NET MVC 3 dentro de uma pasta de solução faz com que um *NullReferenceException* erro. A solução alternativa é criar o projeto ASP.NET MVC 3 na raiz da solução e, em seguida, mova-o para a pasta de solução.
- O instalador pode levar mais tempo do que as versões anteriores do ASP.NET MVC para concluir. Isso ocorre porque ele atualiza os componentes do Visual Studio 2010.
- IntelliSense para a sintaxe do Razor não funciona quando ReSharper está instalado. Se você tiver ReSharper instalado e quiser aproveitar o suporte do IntelliSense Razor no RC2 do ASP.NET MVC 3, consulte a entrada [Razor Intellisense e ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) no blog de Hadi Hariri, que descreve maneiras de usá-los juntos hoje.
- CSHTML e VBHTML exibições criadas com a versão Beta do ASP.NET MVC 3 têm suas ações de compilação definida corretamente, sabendo que essas exibir tipos são omitidos quando o projeto é publicado. O *ação de compilação* valor para esses arquivos devem ser definidos para o conteúdo ". ASP.NET MVC 3 RC2 corrige esse problema para novos arquivos, mas não corrigir a configuração de arquivos existentes para um projeto criado com a versão Beta.![](mvc3-release-notes/_static/image4.png)
- Durante a instalação, a caixa de diálogo de aceitação de EULA exibe os termos de licença em uma janela que é menor do que se destina.
- Quando você estiver editando um modo de exibição do Razor (cshtml arquivo), o item de menu Ir para o controlador no Visual Studio não estará disponível e não há nenhum trechos de código.
- Se você instala o ASP.NET MVC 3 do Visual Web Developer Express em um computador onde o Visual Studio não está instalado e depois instala o Visual Studio, você deverá reinstalar o ASP.NET MVC 3. O Visual Studio e o Visual Web Developer Express compartilham componentes que são atualizados pelo instalador do ASP.NET MVC 3. O mesmo problema se aplica se você instalar o ASP.NET MVC 3 para o Visual Studio em um computador que não tenha o Visual Web Developer Express e, em seguida, instalar posteriormente Visual Web Developer Express.
- Instalar o ASP.NET MVC 3 RC 2 não atualizar o NuGet, se você já tiver instalado. Para atualizar o NuGet, acesse o Gerenciador de extensões do Visual Studio e ele deve aparecem como uma atualização disponível. Você pode atualizar o NuGet para a versão mais recente a partir daí.

<a id="TOC_ASP_NET_3_RC"></a>
## <a name="aspnet-mvc-3-release-candidate"></a>Versão Release Candidate do ASP.NET MVC 3

Versão Release Candidate do ASP.NET MVC foi lançada em 9 de novembro de 2010.

<a id="_Toc276711785"></a>
## <a name="new-features-in-aspnet-mvc-3-rc"></a>Novos recursos no ASP.NET MVC 3 RC

Esta seção descreve os recursos que foram introduzidos na versão RC do ASP.NET MVC 3 desde a versão Beta.

<a id="_Toc276711786"></a>
### <a name="nuget-package-manager"></a>Gerenciador de pacotes do NuGet

O ASP.NET MVC 3 inclui o NuGet Package Manager (anteriormente conhecido como NuPack), que é uma ferramenta de gerenciamento de pacote integrado para adicionar bibliotecas e ferramentas para projetos do Visual Studio. Essa ferramenta automatiza as etapas que os desenvolvedores usam hoje para obter uma biblioteca em sua árvore de origem.

Você pode trabalhar com o NuGet como uma ferramenta de linha de comando, como uma janela de console integrado no Visual Studio 2010, no menu de contexto do Visual Studio e como um conjunto de cmdlets do PowerShell.

Para obter mais informações sobre o NuGet, leia o [Nuget documentação](https://docs.microsoft.com/nuget/).

<a id="_Toc276711787"></a>
### <a name="improved-new-project-dialog-box"></a>Melhor "Novo projeto" caixa de diálogo

Quando você cria um novo projeto, a caixa de diálogo Novo projeto agora permite especificar o mecanismo de exibição, bem como um tipo de projeto do ASP.NET MVC.

![](mvc3-release-notes/_static/image5.png)

Suporte para modificar a lista de modelos e exibição mecanismos listada na caixa de diálogo está incluído nesta versão.

Os modelos padrão são as seguintes:

Vazio. Contém um conjunto mínimo de arquivos de um projeto ASP.NET MVC, incluindo a estrutura de pastas padrão para projetos do ASP.NET MVC, um arquivo de site que contém o padrão que ASP.NET MVC estilos e um diretório de Scripts que contém os arquivos do JavaScript padrão.

Aplicativo de Internet. Contém a funcionalidade de exemplo que demonstra como usar o provedor de associação com o ASP.NET MVC.

A lista de modelos de projeto que é exibida na caixa de diálogo é especificada no registro do Windows.

<a id="_Toc276711788"></a>
### <a name="sessionless-controllers"></a>Sem sessão controladores

O novo *ControllerSessionStateAttribute* oferece mais controle sobre o comportamento do estado de sessão para controladores, especificando um [System.Web.SessionState.SessionStateBehavior](https://msdn.microsoft.com/en-us/library/system.web.sessionstate.sessionstatebehavior.aspx) valor de enumeração.

O exemplo a seguir mostra como desativar o estado da sessão para todas as solicitações para um controlador.

[!code-csharp[Main](mvc3-release-notes/samples/sample13.cs)]

O exemplo a seguir mostra como definir o estado de sessão somente de leitura para todas as solicitações para um controlador.

[!code-csharp[Main](mvc3-release-notes/samples/sample14.cs)]

<a id="_Toc276711789"></a>
### <a name="new-validation-attributes"></a>Novos atributos de validação

#### <a name="compareattribute"></a>CompareAttribute

O novo *CompareAttribute* atributo de validação permite comparar os valores de duas propriedades diferentes de um modelo. No exemplo a seguir, o *ComparePassword* propriedade deve corresponder a *senha* campo para serem válidas.

[!code-csharp[Main](mvc3-release-notes/samples/sample15.cs)]

#### <a name="remoteattribute"></a>RemoteAttribute

O novo *RemoteAttribute* atributo de validação se beneficia de validador remoto jQuery validação do plug-in, que habilita a validação do lado do cliente chamar um método no servidor que executa a lógica de validação real.

No exemplo a seguir, o *UserName* propriedade tem o *RemoteAttribute* aplicado. Ao editar essa propriedade em uma exibição de edição, a validação do cliente chamará uma ação chamada *UserNameAvailable* no *UsersController* classe a fim de validar esse campo.

[!code-csharp[Main](mvc3-release-notes/samples/sample16.cs)]

O exemplo a seguir mostra o controlador correspondente.

[!code-csharp[Main](mvc3-release-notes/samples/sample17.cs)]

Por padrão, o nome da propriedade que o atributo é aplicado a é enviado para o método de ação como um parâmetro de cadeia de caracteres de consulta.

<a id="_Toc276711790"></a>
### <a name="new-overloads-for-labelfor-and-labelformodel-methods"></a>Novas sobrecargas dos métodos de "LabelForModel" e "LabelFor"

Foram adicionadas novas sobrecargas para o *LabelFor* e *LabelForModel* métodos que permitem que você especificam o texto do rótulo. O exemplo a seguir mostra como usar essas sobrecargas.

[!code-cshtml[Main](mvc3-release-notes/samples/sample18.cshtml)]

<a id="_Toc276711791"></a>
### <a name="child-action-output-caching"></a>Ação filha cache de saída

O *OutputCacheAttribute* oferece suporte a cache de saída de ações de filho que são chamadas usando o *Html.RenderAction* ou *Html.Action* métodos auxiliares. O exemplo a seguir mostra uma exibição que chama outra ação.

[!code-cshtml[Main](mvc3-release-notes/samples/sample19.cshtml)]

O *GetDate* ação está anotada com o *OutputCacheAttribute*:

[!code-csharp[Main](mvc3-release-notes/samples/sample20.cs)]

Quando esse código é executado, o resultado da chamada para Html.Action("GetDate") é armazenado em cache por 100 segundos.

<a id="_Toc276711792"></a>
### <a name="add-view-dialog-box-improvements"></a>Aprimoramentos de caixa de diálogo "Adicionar modo de exibição"

Quando você adiciona uma exibição fortemente tipada, a caixa de diálogo Adicionar modo de exibição agora filtra mais tipos de não ser aplicável que em versões anteriores, como muitos tipos de .NET Framework core. Além disso, a lista agora é classificada pelo nome da classe e não pelo nome de tipo totalmente qualificado, que torna mais fácil localizar tipos. Por exemplo, o nome de tipo agora é exibido como no exemplo a seguir:

Nome da classe (namespace)

Em versões anteriores, isso seria foi exibido como o seguinte:

ClassName

<a id="_Toc276711793"></a>
### <a name="granular-request-validation"></a>Validação de solicitação granular

O *excluir* propriedade *ValidateInputAttribute* não existe mais. Em vez disso, para que a validação de solicitação ignorada para propriedades específicas de um modelo durante a associação de modelo, use o novo *SkipRequestValidationAttribute*.

Por exemplo, suponha que um método de ação é usado para editar uma postagem de blog:

[!code-csharp[Main](mvc3-release-notes/samples/sample21.cs)]

O exemplo a seguir mostra o modelo de exibição para uma postagem de blog.

[!code-csharp[Main](mvc3-release-notes/samples/sample22.cs)]

Quando um usuário envia algumas marcações para a propriedade de descrição, a associação de modelo falhará devido a validação de solicitação. Para desativar a validação de solicitação durante a associação de modelo para a descrição de postagem de blog, aplicar o *SkipRequpestValidationAttribute* para a propriedade, conforme mostrado neste exemplo:.

[!code-csharp[Main](mvc3-release-notes/samples/sample23.cs)]

Como alternativa, para desativar a validação de solicitação para todas as propriedades do modelo, aplicar *ValidateInputAttribute* com um valor de *false* para o método de ação:

[!code-csharp[Main](mvc3-release-notes/samples/sample24.cs)]

<a id="_Toc276711794"></a>
## <a name="breaking-changes"></a>Alterações significativas

- A ordem de execução de filtros de exceção foi alterado para filtros de exceção que têm o mesmo *ordem* valor. No ASP.NET MVC 2 e versões anteriores, filtros de exceção no controlador que tinha o mesmo *ordem* como aqueles em um método de ação foram executados antes dos filtros de exceção no método de ação. Isso normalmente seria o caso quando foram aplicados filtros de exceção sem uma especificado *ordem* valor. ASP.NET MVC 3, nesta ordem foi revertida para que o manipulador de exceção mais específico é executado pela primeira vez. Como nas versões anteriores, se o *ordem* propriedade seja explicitamente especificada, os filtros são executados na ordem especificada.
- Adicionar uma nova propriedade chamada *FileExtensions* para o *VirtualPathProviderViewEngine* classe base. Ao procurar uma exibição pelo caminho (e não por nome), somente as exibições com uma extensão de arquivo contidos na lista especificada por essa nova propriedade é considerada. Isso é uma alteração significativa para aqueles que registrar uma personalizada de compilação para permitir uma extensão de arquivo personalizados para modos de exibição de formulário da web e e fazem referência a essas exibições usando um caminho completo em vez de um nome. A solução é modificar o valor da *FileExtensions* propriedade inclua a extensão de arquivo personalizado.

<a id="_Toc276711795"></a>
## <a name="known-issues"></a>Problemas conhecidos

- O instalador pode levar mais tempo do que as versões anteriores do ASP.NET MVC para ser concluída porque ele atualiza os componentes do Visual Studio 2010.
- O scaffolding adicionar modo de exibição ao selecionar astrongly digitado exibir scaffolds propriedades somente gravação. Eles sempre devem ser ignorados por scaffolding. A caixa de diálogo Adicionar modo de exibição também scaffolds propriedades somente leitura ao gerar uma exibição de "Editar" ou "Criar". Propriedades somente leitura devem ser Scaffold apenas para os modos de exibição de lista e de exibição.
- A depuração não funciona quando o ASP.NET MVC 3 é instalado junto com o CTP Async. ASP.NET MVC 3 não pode ser instalada lado a lado com o CTP Async. Desinstale o CTP Async para reparar a depuração. Para obter mais detalhes, leia [esta postagem de blog](http://drew-prog.blogspot.com/2010/11/how-to-uninstall-microsoft-aspnet-mvc-3.html) sobre a desinstalação de todas as partes do ASP.NET MVC 3 RC.
- Razor Intellisense não funciona quando Resharper está instalado. Se você tiver ReSharper instalado e quiser aproveitar o suporte do intellisense Razor no ASP.NET MVC 3 RC, leia [esta postagem de blog](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) da JetBrains que descreve maneiras de usá-los juntos hoje.
- CSHTML e VBHTML exibições criadas com a versão Beta do ASP.NET MVC 3 não têm suas ações de compilação corretamente que omite-los da publicação. O *ação de compilação* para esses arquivos devem ser definidos como "Conteúdo". ASP.NET MVC 3 RC corrige esse problema para novos arquivos, mas não corrigir a configuração de arquivos existentes para um projeto criado com a versão Beta.
- O instalador pode levar mais tempo do que as versões anteriores do ASP.NET MVC para ser concluída porque ele atualiza os componentes do Visual Studio 2010.
- O scaffolding adicionar modo de exibição ao selecionar "Editar" fortemente tipado scaffolds exibição propriedades somente leitura. Da mesma forma, as propriedades somente gravação são Scaffold para modos de exibição de "Exibir".
- Durante a instalação, a caixa de diálogo de aceitação de EULA exibe os termos de licença em uma janela que é menor do que se destina.
- Instalando o [CTP do Visual Studio Async](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=18712f38-fcd2-4e9f-9028-8373dc5732b2&amp;displaylang=en) causa um conflito com a versão do Razor que é incluída como parte da instalação de ferramentas do ASP.NET MVC 3. Certifique-se de que você não tente instalar o CTP do Visual Studio Async e a versão do Razor na mesma máquina.
- Quando você estiver editando um modo de exibição do Razor (cshtml arquivo), o item de menu Ir para o controlador no Visual Studio não estará disponível e não há nenhum trechos de código.

<a id="TOC_ASP_NET_3_Beta"></a>
## <a name="aspnet-mvc-3-beta"></a>Versão Beta do ASP.NET MVC 3

Versão Beta do ASP.NET MVC 3 foi lançado em 6 de outubro de 2010. As observações a seguir são específicas para a versão Beta e estão sujeitas a todas as atualizações ou alterações mencionadas na seção Release Candidate do ASP.NET MVC 3 acima.

## <a id="0.1__Toc274034215"></a>Versão Beta do novo Featuresin ASP.NET MVC 3

<a id="0.1__Default_validation_system"></a>Esta seção descreve os recursos que foram introduzidos na versão Beta do ASP.NET MVC 3.

### <a id="0.1__Toc274034216"></a>Gerenciador de pacotes do NuGet

O ASP.NET MVC 3 inclui o NuGet Package Manager, que é uma ferramenta de gerenciamento de pacote integrado para a adição de bibliotecas e ferramentas para projetos do Visual Studio. A maior parte do tempo, ele automatiza as etapas que os desenvolvedores usam hoje para obter uma biblioteca em sua árvore de origem.

Você pode trabalhar com o NuGet como uma ferramenta de linha de comando, como uma janela de console integrado no Visual Studio 2010, no menu de contexto do Visual Studio e o conjunto de cmdlets do PowerShell.

Para obter mais informações sobre o NuGet, leia o [NuGet documentação](https://docs.microsoft.com/nuget/).

### <a id="0.1__Toc274034217"></a>Caixa de diálogo Novo projeto de aprimorado

Quando você cria um novo projeto, a caixa de diálogo Novo projeto agora permite especificar o mecanismo de exibição, bem como um tipo de projeto do ASP.NET MVC.

![](mvc3-release-notes/_static/image6.png)

Suporte para modificar a lista de modelos e exibição mecanismos listada na caixa de diálogo não está incluído nesta versão.

Os modelos padrão são as seguintes:

Vazio. Contém um conjunto mínimo de arquivos de um projeto ASP.NET MVC, incluindo a estrutura de pastas padrão para projetos do ASP.NET MVC, um pequeno arquivo de site que contém o padrão que ASP.NET MVC estilos e um diretório de Scripts que contém os arquivos do JavaScript padrão.

Aplicativo de Internet. Contém a funcionalidade de exemplo que demonstra como usar o provedor de associação no ASP.NET MVC.

### <a id="0.1__Toc274034218"></a>Maneira simplificada para especificar fortemente tipado modelos nos modos de exibição do Razor

A maneira de especificar o tipo de modelo para modos de exibição Razor fortemente tipados foi simplificada usando o novo @model diretiva para exibições CSHTML e @ModelType diretiva para exibições VBHTML. Em versões anteriores do ASP.NET MVC, você deve especificar que um modelo fortemente tipado para Razor exibições desta forma:

[!code-cshtml[Main](mvc3-release-notes/samples/sample25.cshtml)]

Nesta versão, você pode usar a seguinte sintaxe:

[!code-cshtml[Main](mvc3-release-notes/samples/sample26.cshtml)]

### <a id="0.1__Toc274034219"></a>Suporte a métodos auxiliares para novas páginas da Web do ASP.NET

A nova tecnologia de páginas da Web ASP.NET inclui um conjunto de métodos auxiliares que são úteis para adicionar funcionalidades mais comumente usadas para controladores e exibições. ASP.NET MVC 3 oferece suporte a esses métodos auxiliares dentro de controladores e exibições (onde apropriado). Esses métodos estão contidos no assembly Helpers. A tabela a seguir lista alguns dos métodos auxiliares páginas da Web ASP.NET.

| **Auxiliar** | **Descrição** |
| --- | --- |
| Gráfico | Renderiza um gráfico em uma exibição. Contém métodos como Chart.ToWebImage, Chart.Save e Chart.Write. |
| Criptografia | Usa para criar corretamente os algoritmos de hash com salt e senhas de hash. |
| WebGrid | Uma coleção de objetos (normalmente, os dados de um banco de dados) é renderizada como uma grade. Dá suporte à paginação e classificação. |
| WebImage | Renderiza uma imagem. |
| WebMail | Envia uma mensagem de email. |

Um tópico de referência rápida que lista os auxiliares e a sintaxe básica está disponível como parte da documentação de sintaxe do Razor ASP.NET na seguinte URL:

[https://www.ASP.NET/WebMatrix/Tutorials/ASP-NET-Web-Pages-API-Reference](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)

### <a id="0.1__Toc274034220"></a>Suporte de injeção de dependência adicional

Aproveitando a versão do ASP.NET MVC 3 Preview 1, a versão atual que inclui o suporte adicionado para dois novos serviços e quatro serviços existentes e suporte aprimorado para o localizador de serviço comum e resolução de dependência.

#### <a name="new-icontrolleractivator-interface-for-fine-grained-controller-instantiation"></a>Nova Interface IControllerActivator para instanciação do controlador refinada

A nova interface IControllerActivator fornece controle mais refinado sobre como os controladores são instanciados por meio de injeção de dependência. O exemplo a seguir mostra a interface:

[!code-csharp[Main](mvc3-release-notes/samples/sample27.cs)]

Compare com a função da fábrica do controlador. Uma fábrica do controlador é uma implementação da interface IControllerFactory, que é responsável para localizar um tipo de controlador e criando uma instância desse tipo de controlador.

Ativadores de controlador serão responsáveis somente para instanciar uma instância de um tipo de controlador. Eles não executam a pesquisa de tipo de controlador. Depois de localizar um tipo de controlador apropriada, as fábricas do controlador devem delegar a uma instância de IControllerActivator para tratar da instanciação real do controlador.

A classe de DefaultControllerFactory tem um novo construtor que aceita uma instância de IControllerFactory. Isso permite que se aplicam a injeção de dependência para gerenciar esse aspecto da criação do controlador sem a necessidade de substituir o comportamento de pesquisa de tipo de controlador padrão.

#### <a name="iservicelocator-interface-replaced-with-idependencyresolver"></a>Interface IServiceLocator substituído com IDependencyResolver

Com base nos comentários da comunidade, a versão Beta do ASP.NET MVC 3 substituiu o uso da interface IServiceLocator com uma interface de IDependencyResolver reduzida específica às necessidades do ASP.NET MVC. O exemplo a seguir mostra a nova interface:

[!code-csharp[Main](mvc3-release-notes/samples/sample28.cs)]

Como parte dessa alteração, a classe ServiceLocator também foi substituída com a classe DependencyResolver. Registro de um resolvedor de dependência é semelhante às versões anteriores do ASP.NET MVC:

[!code-csharp[Main](mvc3-release-notes/samples/sample29.cs)]

Implementações dessa interface devem delegar simplesmente para o contêiner de injeção de dependência subjacente para fornecer o serviço registrado para o tipo solicitado.

Quando não houver nenhum serviços registrados do tipo solicitado, o ASP.NET MVC espera implementações dessa interface para retornar nulo dos GetService e para retornar uma coleção vazia de GetServices.

A nova classe DependencyResolver permite registrar classes que implementam a nova interface IDependencyResolver ou a interface de localizador de serviço comum (IServiceLocator). Para obter mais informações sobre o Common Service Locator, consulte [CommonServiceLocator no GitHub](https://github.com/unitycontainer/commonservicelocator).

<a id="0.1__Breaking_Changes"></a>

#### <a name="new-iviewactivator-interface-for-fine-grained-view-page-instantiation"></a>Nova Interface IViewActivator para instanciação de página de exibição refinada

A nova interface IViewPageActivator fornece controle mais refinado sobre como exibir páginas são instanciadas por meio de injeção de dependência. Isso se aplica a instâncias de WebFormView e RazorView instâncias. O exemplo a seguir mostra a nova interface:

[!code-csharp[Main](mvc3-release-notes/samples/sample30.cs)]

Agora, essas classes aceitam um argumento de construtor IViewPageActivator, que permite que você usar injeção de dependência para controlar como os tipos ViewPage, ViewUserControl e WebViewPage são instanciados.

#### <a name="new-dependency-resolver-support-for-existing-services"></a>Novo suporte de resolvedor de dependência para os serviços existentes

A nova versão inclui suporte à resolução de dependências para os seguintes serviços:

- Provedores de validação de modelo. Classes que implementam ModelValidatorProvider podem ser registradas no resolvedor de dependência e o sistema usará para dar suporte a validação do lado do cliente e do servidor.
- Provedor de metadados do modelo. Uma única classe que implementa ModelMetadataProvider pode ser registrada no resolvedor de dependência e o sistema usará para fornecer metadados para os sistemas de modelagem e validação.
- Provedores de valor. Classes que implementam ValueProviderFactory podem ser registradas no resolvedor de dependência e o sistema usará para criar provedores de valor que são consumidos pelo controlador e durante a associação de modelo.
- Associadores de modelo. Classes que implementam IModelBinderProvider podem ser registradas no resolvedor de dependência e o sistema usará para criar associadores de modelo que são consumidos pelo sistema de associação de modelo.

### <a id="0.1__Toc274034221"></a>Novo suporte para discreto Ajax com base em jQuery

O ASP.NET MVC inclui métodos auxiliares de Ajax como o seguinte:

- Ajax.ActionLink
- Ajax.RouteLink
- Ajax.BeginForm
- Ajax.BeginRouteForm

Esses métodos usam o JavaScript para invocar um método de ação no servidor, em vez de usar um postback completo. Essa funcionalidade foi atualizada para tirar proveito do jQuery de maneira discreta. Em vez de forma intrusiva emitindo embutido scripts de cliente, esses métodos auxiliares separam o comportamento da marcação pelo emissor HTML5 atributos usando o *dados ajax* prefixo. Comportamento é então aplicado à marcação referenciando os arquivos apropriados do JavaScript. Certifique-se de que os seguintes arquivos JavaScript são referenciados:

- 1.4.1.js jQuery
- jQuery.unobtrusive.AJAX.js

Este recurso é habilitado por padrão no arquivo Web. config no ASP.NET MVC 3 novos modelos de projeto, mas é desabilitado por padrão para projetos existentes. Para obter mais informações, consulte [adicionado sinalizadores de aplicativo para validação do cliente e o JavaScript discreto](#0.1_AddedApplicationWideFlagsForClientValida) mais adiante neste documento.

### <a id="0.1__Toc274034222"></a>Novo suporte para jQuery discreto validação

Por padrão, o ASP.NET MVC 3 Beta usa validação jQuery de maneira discreta para realizar a validação do lado do cliente. Para habilitar a validação do cliente discreto, fazer uma chamada com o seguinte de dentro de um modo de exibição:

[!code-csharp[Main](mvc3-release-notes/samples/sample31.cs)]

Isso requer que a propriedade de ViewContext.UnobtrusiveJavaScriptEnabled é definida como true, o que você pode fazer ao fazer a chamada a seguir:

[!code-csharp[Main](mvc3-release-notes/samples/sample32.cs)]

Verifique também se que os seguintes arquivos JavaScript são referenciados.

- 1.4.1.js jQuery
- jQuery.Validate.js
- jQuery.Validate.unobtrusive.js

Este recurso está ativado por padrão no arquivo Web. config no ASP.NET MVC 3 novos modelos de projeto, mas é desabilitado por padrão para projetos existentes. Para obter mais informações, consulte [novo aplicativo sinalizadores para a validação do cliente e o JavaScript discreto](#0.1_AddedApplicationWideFlagsForClientValida) mais adiante neste documento.

<a id="0.1__Toc274034223"></a>

### <a id="0.1_AddedApplicationWideFlagsForClientValida"></a>Novo aplicativo sinalizadores para a validação do cliente e o JavaScript discreto

Você pode habilitar ou desabilitar a validação do cliente e o JavaScript discreto usando globalmente membros estáticos da classe HtmlHelper, como no exemplo a seguir:

[!code-csharp[Main](mvc3-release-notes/samples/sample33.cs)]

Os modelos de projeto padrão habilitam o JavaScript discreto por padrão. Você também pode habilitar ou desabilitar esses recursos no arquivo raiz Web. config do seu aplicativo usando as seguintes configurações:

[!code-xml[Main](mvc3-release-notes/samples/sample34.xml)]

Como você pode habilitar esses recursos por padrão, novas sobrecargas foram introduzidas para a classe HtmlHelper que permite que você substitua as configurações padrão, conforme mostrado nos exemplos a seguir:

[!code-csharp[Main](mvc3-release-notes/samples/sample35.cs)]

Para compatibilidade com versões anteriores, esses dois recursos estão desabilitadas por padrão.

### <a id="0.1__Toc274034224"></a>Novo suporte para o código que é executado antes da execução de modos de exibição

Agora você pode colocar um arquivo chamado \_viewstart.cshtml (ou \_viewstart.vbhtml) no diretório visualizações e adicione o código que será compartilhada entre várias exibições no diretório e seus subdiretórios. Por exemplo, você pode colocar o seguinte código para o \_viewstart.cshtml página na pasta ~/Views:

[!code-cshtml[Main](mvc3-release-notes/samples/sample36.cshtml)]

Isso define a página de layout para cada modo de exibição dentro da pasta de modos de exibição e todas as suas subpastas. Quando um modo de exibição está sendo processado, o código de \_viewstart.cshtml arquivo será executado antes de executar o código de exibição. O \_viewstart.cshtml código aplica-se a cada exibição nessa pasta.

Por padrão, o código de \_viewstart.cshtml arquivo também se aplica a exibições em qualquer subpasta. No entanto, as subpastas individuais podem ter sua própria versão do \_viewstart.cshtml arquivo; nesse caso, a versão local tem precedência. Por exemplo, para executar o código que é comum a todos os modos de exibição para HomeController, coloque um \_viewstart.cshtml arquivo na pasta ~/Views/Home.

### <a id="0.1__Toc274034225"></a>Novo suporte para a sintaxe do Razor VBHTML

A visualização anterior do ASP.NET MVC incluído o suporte para modos de exibição usando a sintaxe Razor com base em c#. Esses modos de exibição usam a extensão de arquivo. cshtml. Como parte de um trabalho contínuo para dar suporte a Razor, o ASP.NET MVC 3 Beta introduz suporte para a sintaxe do Razor no Visual Basic, que usa a extensão de arquivo. vbhtml.

Para obter uma introdução ao uso de sintaxe do Visual Basic em páginas VBHTML, consulte o tutorial na seguinte URL:

[https://www.ASP.NET/WebMatrix/Tutorials/ASP-NET-Web-Pages-Visual-Basic](../web-pages/overview/getting-started/introducing-razor-syntax-vb.md)

### <a id="0.1__Toc274034226"></a>Controle mais Granular sobre ValidateInputAttribute

ASP.NET MVC sempre inclui a classe ValidateInputAttribute, que chama a infraestrutura de validação de solicitação ASP.NET principal para certificar-se de que a solicitação de entrada não contém entrada potencialmente mal-intencionada. Por padrão, a validação de entrada está habilitada. É possível desabilitar a validação de solicitação usando o atributo ValidateInputAttribute, como no exemplo a seguir:

[!code-csharp[Main](mvc3-release-notes/samples/sample37.cs)]

No entanto, muitos aplicativos web têm campos de formulário individuais que precisam permitir HTML, enquanto os campos restantes não deveriam. A classe ValidateInputAttribute agora permite especificar uma lista de campos que não devem ser incluídos na validação de solicitação.

Por exemplo, se você estiver desenvolvendo um mecanismo de blog, convém permitir marcação em campos de corpo e resumo. Esses campos podem ser representados por dois elemento de entrada, cada um com um atributo de nome correspondente ao nome da propriedade ("Body" e "Resumo"). Para desabilitar a solicitação de validação para esses campos somente, especifique os nomes (separada por vírgulas) na propriedade de exclusão da classe ValidateInput, como no exemplo a seguir:

[!code-csharp[Main](mvc3-release-notes/samples/sample38.cs)]

### <a id="0.1__Toc274034227"></a>Auxiliares converter sublinhados hifens para nomes de atributo HTML especificados usando objetos anônimos

Métodos auxiliares permitem que você especifique pares de nome/valor de atributo usando um objeto anônimo, como no exemplo a seguir:

[!code-csharp[Main](mvc3-release-notes/samples/sample39.cs)]

Essa abordagem não permite que você use hifens no nome do atributo, porque um hífen não pode ser usado para um nome de propriedade no ASP.NET. No entanto, os hifens são importantes para os atributos personalizados do HTML5; Por exemplo, o HTML5 usa o prefixo "data-".

Ao mesmo tempo, sublinhados não podem ser usados para nomes de atributo em HTML, mas são válidos em nomes de propriedade. Portanto, se você especificar atributos usando um objeto anônimo, e se os nomes de atributo incluir um sublinhado, métodos auxiliares converterá os sublinhados hifens. Por exemplo, a seguinte sintaxe de auxiliar usa um sublinhado:

[!code-csharp[Main](mvc3-release-notes/samples/sample40.cs)]

O exemplo anterior processa a seguinte marcação quando o auxiliar é executado:

[!code-html[Main](mvc3-release-notes/samples/sample41.html)]

## <a id="0.1__Toc274034228"></a>Correções de bugs

O modelo de objeto padrão para os auxiliares de modelo EditorFor e DisplayFor agora dá suporte a ordenação especificado na propriedade DisplayAttribute.Order. (Nas versões anteriores, a configuração de ordem não foi usada.)

Validação do cliente agora oferece suporte à validação de propriedades substituídas com atributos de validação aplicados.

JsonValueProviderFactory agora é registrado por padrão.

## <a id="0.1__Toc274034229"></a>Alterações mais recentes

A ordem de execução de filtros de exceção foi alterado para filtros de exceção que têm o mesmo valor de ordem. No ASP.NET MVC 2 e versões anteriores, exceção filtros no controlador com a mesma ordem, como aqueles em um método de ação foram executados antes dos filtros de exceção no método de ação. Isso normalmente seria o caso quando filtros de exceção foram aplicados sem um valor de ordem especificado. ASP.NET MVC 3, nesta ordem foi revertida para que o manipulador de exceção mais específico é executado pela primeira vez. Como nas versões anteriores, se a propriedade de ordem for especificada explicitamente, os filtros são executados na ordem especificada.

## <a id="0.1__Toc274034230"></a>Problemas conhecidos

Durante a instalação, a caixa de diálogo de aceitação de EULA exibe os termos de licença em uma janela que é menor do que se destina.

Modos de exibição Razor não tem suporte do IntelliSense nem realce de sintaxe. Espera-se que o suporte para a sintaxe do Razor no Visual Studio será incluído como parte de uma versão posterior.

Quando você estiver editando uma exibição de Razor (CSHTML arquivo), o <a id="0.1__Toc224729061"> </a> <a id="0.1__Toc238051347"> </a> item de menu Ir para o controlador no Visual Studio não estará disponível e não há nenhum trechos de código.

Ao usar o @model exibir a sintaxe para especificar um CSHTML fortemente tipada, atalhos específicos do idioma para tipos não são reconhecidos. Por exemplo, @model int não funcionará, mas @model Int32 funcionará. A solução alternativa para esse erro é usar o nome do tipo real quando você especificar o tipo de modelo.

Ao usar o @model sintaxe para especificar um modo de exibição fortemente tipado CSHTML (ou @ModelType para especificar um modo de exibição fortemente tipado VBHTML), não há suporte para tipos anuláveis e declarações de matriz. Por exemplo, @model int? não tem suporte. Em vez disso, use @model Nullable&lt;Int32&gt;. A sintaxe @model string [] também não tem suporte; em vez disso, use @model IList&lt;cadeia de caracteres&gt;.

Ao atualizar um projeto ASP.NET MVC 2 para o ASP.NET MVC 3, certifique-se de adicionar o seguinte à seção appSettings do arquivo Web. config:

[!code-xml[Main](mvc3-release-notes/samples/sample42.xml)]

Há um problema conhecido que faz com que a autenticação de formulários para sempre redirecionar os usuários não autenticados para ~/Account/Login, ignorando a configuração de autenticação de formulários usada em Web. config. A solução alternativa é adicionar a seguinte configuração de aplicativo.

[!code-xml[Main](mvc3-release-notes/samples/sample43.xml)]

## <a id="0.1__Toc274034231"></a>Isenção de responsabilidade

© Microsoft Corporation. de 2011. Todos os direitos reservados. Este documento é fornecido "como-é." Informações e opiniões expressadas neste documento, incluindo URLs e outras referências a sites da Internet, podem ser alteradas sem aviso prévio. Você assume o risco de utilizá-las.

Este documento não lhe concede nenhum direito legal à nenhuma propriedade intelectual de nenhum produto da Microsoft. Você pode copiar e usar este documento para fins internos de referência.
