---
uid: whitepapers/mvc3-release-notes
title: ASP.NET MVC 3 | Microsoft Docs
author: rick-anderson
description: ''
ms.author: riande
ms.date: 10/06/2010
ms.assetid: f44c166e-7e91-48a0-a6f8-d9285f3594e5
msc.legacyurl: /whitepapers/mvc3-release-notes
msc.type: content
ms.openlocfilehash: 7342b5f4a7e2327f3f3850941510a6e46ec30842
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830000"
---
<a name="aspnet-mvc-3"></a>O ASP.NET MVC 3
====================
- [Visão geral](#overview)
- [Notas de instalação](#installation-notes)
- [Requisitos de software](#software-requirements)
- [Documentação](#documentation)
- [Suporte](#support)
- [Atualizando um projeto do ASP.NET MVC 2 para o ASP.NET MVC 3 ferramentas de atualização](#upgrading)
- [Atualização do ASP.NET MVC 3 das ferramentas (12 de abril de 2011)](#tu-changes)

    - [Caixa de diálogo "Adicionar controlador" pode agora criar o scaffolding de controladores com código de acesso a dados e exibições](#tu-AddControllerDialog)
    - [Melhorias para o "ASP.NET MVC 3 novo projeto de" caixa de diálogo](#tu-ImprovementsNewDialogBox)
    - [Modelos de projeto agora incluem o Modernizr 1.7](#tu-Modernizr)
    - [Modelos de projeto incluem versões atualizadas do jQuery, o jQuery UI e o jQuery validação](#tu-UpdatedJQuery)
    - [Modelos de projeto agora incluem o ADO.NET Entity Framework 4.1 como um pacote do NuGet pré-instalados](#tu-EF)
    - [Modelos de projeto incluem bibliotecas JavaScript como previamente instalados pacotes do NuGet](#tu-JavaScriptLibsNuget)
    - [Problemas conhecidos](#tu-KI)
- [RTM do ASP.NET MVC 3 (13 de janeiro de 2011)](#MVC3RTM)

    - [Alteração: A versão atualizada do jQuery UI para 1.8.7](#RTM-1)
    - [Alteração: Alterado o padrão ModelMetadataProvider de volta para DataAnnotationsModelMetadataProvider](#RTM-2)
    - [Corrigido: Colando parte de uma expressão Razor que contém os resultados de espaço em branco nele que está sendo revertido](#RTM-3)
    - [Corrigido: Renomear um arquivo Razor que é aberto no editor de desabilita a colorização de sintaxe e IntelliSense](#RTM-4)
    - [Problemas conhecidos](#RTM-KI)
    - [Alterações significativas](#RTM-BC)
- [O ASP.NET MVC 3 versão Release Candidate 2 (10 de dezembro de 2010)](#_Toc2)

    - [Modelos de projeto alterados para incluir jQuery 1.4.4, o jQuery 1.7 de validação e o jQuery UI 1.8.6y UI 1.8.6](#_Toc2_1)
    - [Classe de "AdditionalMetadataAttribute" adicionado](#_Toc2_2)
    - [Modo de exibição aprimorado Scaffolding](#_Toc2_3)
    - [Método de HTML. RAW adicionado](#_Toc2_3)
    - [Propriedade renomeado "Controller.ViewModel" e a propriedade de "Exibição" para "ViewBag"](#_Toc2_4)
    - [Classe de "ControllerSessionStateAttribute" renomeado para "SessionStateAttribute"](#_Toc2_5)
    - [Propriedade de "Campos" RemoteAttribute renomeado para "AdditionalFields"](#_Toc2_6)
    - [Renomeado "SkipRequestValidationAttribute" para "AllowHtmlAttribute"](#_Toc2_7)
    - [Método alterado "Html.ValidationMessage" para exibir a primeira mensagem de erro úteis](#_Toc2_8)
    - [Fixo @model declaração não adicionar espaço em branco ao documento](#_Toc2_9)
    - [Propriedade "FileExtensions" adicionado aos mecanismos de exibição para dar suporte a nomes de arquivo específico do mecanismo](#_Toc2_10)
    - [Auxiliar de fixa "LabelFor" para emitir o valor correto para o atributo "For"](#_Toc2_11)
    - [Método fixa "RenderAction" dar prioridade valores explícitos durante a associação de modelo](#_Toc2_12)
    - [Alterações significativas](#_Toc2_BC)
    - [Problemas conhecidos](#_Toc2_KI)
- [O ASP.NET MVC 3 versão Release Candidate (9 de novembro de 2010)](#TOC_ASP_NET_3_RC)

    - [Novos recursos no ASP.NET MVC 3 RC](#_Toc276711785)
    - [Gerenciador de pacotes NuGet](#_Toc276711786)
    - [Aprimorado "Novo projeto" caixa de diálogo](#_Toc276711787)
    - [Controladores sem sessão](#_Toc276711788)
    - [Novos atributos de validação](#_Toc276711789)
    - [Novas sobrecargas dos métodos de "LabelForModel" e "LabelFor"](#_Toc276711790)
    - [Ação filha o cache de saída](#_Toc276711791)
    - [Melhorias da caixa de diálogo "Add View"](#_Toc276711792)
    - [Validação de solicitação granular](#_Toc276711793)
    - [Alterações significativas](#_Toc276711794)
    - [Problemas conhecidos](#_Toc276711795)
- [ASP. Notas de versão Beta MVC 3 (6 de outubro de 2010)](#TOC_ASP_NET_3_Beta)

    - [Novos recursos na versão Beta do ASP.NET MVC 3](#0.1__Toc274034215)
    - [Gerenciador de pacotes NuPack](#0.1__Toc274034216)
    - [Aprimorada a caixa de diálogo Novo projeto](#0.1__Toc274034217)
    - [Maneira simplificada de especificar fortemente tipados modelos em exibições do Razor](#0.1__Toc274034218)
    - [Suporte para novos métodos de auxiliares de páginas da Web do ASP.NET](#0.1__Toc274034219)
    - [Suporte à injeção de dependência adicional](#0.1__Toc274034220)
    - [Novo suporte para o Ajax não invasiva baseada no jQuery](#0.1__Toc274034221)
    - [Novo suporte para jQuery Unobtrusive Validation](#0.1__Toc274034222)
    - [Novos sinalizadores de todo o aplicativo para validação do cliente e o JavaScript discreto](#0.1__Toc274034223)
    - [Novo suporte para o código que é executado antes que a execução de modos de exibição](#0.1__Toc274034224)
    - [Novo suporte para a sintaxe do Razor VBHTML](#0.1__Toc274034225)
    - [Controle mais Granular sobre ValidateInputAttribute](#0.1__Toc274034226)
    - [Os auxiliares de converter sublinhados hifens para nomes de atributo HTML especificados usando objetos anônimos](#0.1__Toc274034227)
    - [Correções de bugs](#0.1__Toc274034228)
    - [Alterações significativas](#0.1__Toc274034229)
    - [Problemas conhecidos](#0.1__Toc274034230)
- [Isenção de responsabilidade](#0.1__Toc274034231)

<a id="overview"></a>
## <a name="overview"></a>Visão geral

Este documento descreve a versão do RTM do ASP.NET MVC 3 para Visual Studio 2010. ASP.NET MVC é uma estrutura para desenvolver aplicativos da Web que usa o padrão Model-View-Controller (MVC). O instalador do ASP.NET MVC 3 inclui os seguintes componentes:

- Componentes de tempo de execução do ASP.NET MVC 3
- Ferramentas do ASP.NET MVC 3 Visual Studio 2010
- Componentes de tempo de execução de páginas da Web do ASP.NET
- Ferramentas do ASP.NET páginas da Web Visual Studio 2010
- Gerenciador de pacotes da Microsoft para .NET (NuGet)
- Uma atualização para o Visual Studio 2010 que habilita o suporte à sintaxe do Razor. (Para obter detalhes, consulte o artigo da Base de Conhecimento 2483190.)

O conjunto completo de notas de versão para cada versão de pré-lançamento do ASP.NET MVC 3 pode ser encontrado no site do ASP.NET na seguinte URL:

https://www.asp.net/learn/whitepapers/mvc3-release-notes

<a id="installation-notes"></a>
## <a name="installation-notes"></a>Notas de instalação

Para instalar o ASP.NET MVC 3 RTM usando o Web Platform Installer (Web PI), visite a página a seguir:

[https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3](https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3)

Como alternativa, você pode baixar o instalador para o RTM do ASP.NET MVC 3 para Visual Studio 2010 da seguinte página:

https://go.microsoft.com/fwlink/?LinkID=208140

O ASP.NET MVC 3 pode ser instalado e pode ser executado lado a lado com o ASP.NET MVC 2.

<a id="software-requirements"></a>
## <a name="software-requirements"></a>Requisitos de software

Os componentes de tempo de execução do ASP.NET MVC 3 exigem o software a seguir:

- .NET framework versão 4. 

    Ferramentas do ASP.NET MVC 3 Visual Studio 2010 exigem o software a seguir:
- Visual Studio 2010 ou Visual Web Developer 2010 Express.

<a id="documentation"></a>
## <a name="documentation"></a>Documentação

Documentação do ASP.NET MVC está disponível no site do MSDN na seguinte URL:

[https://go.microsoft.com/fwlink/?LinkId=205717](https://go.microsoft.com/fwlink/?LinkId=205717)

Tutoriais e outras informações sobre o ASP.NET MVC estão disponíveis na página do MVC do site da Web do ASP.NET na seguinte URL:

[https://www.asp.net/mvc/](../mvc/index.md)

<a id="support"></a>
## <a name="support"></a>Suporte

Esta é uma versão com suporte total. Informações sobre como obter suporte técnico podem ser encontradas na [site do Microsoft Support](https://support.microsoft.com/).

Também fique à vontade postar perguntas sobre esta versão para o fórum do ASP.NET MVC, onde os membros da comunidade do ASP.NET são com frequência pode oferecer suporte informal:

[https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)

<a id="upgrading"></a>
## <a name="upgrading-an-aspnet-mvc-2-project-to-aspnet-mvc-3-tools-update"></a>Atualizando um projeto do ASP.NET MVC 2 para o ASP.NET MVC 3 ferramentas de atualização

O ASP.NET MVC 3 pode ser instalado lado a lado com o ASP.NET MVC 2 no mesmo computador, que lhe dá flexibilidade na escolha quando atualizar um aplicativo ASP.NET MVC 2 para o ASP.NET MVC 3.

Para atualizar manualmente um aplicativo existente do ASP.NET MVC 2 para a versão 3, faça o seguinte:

1. Crie um novo projeto vazio do ASP.NET MVC 3 em seu computador. Este projeto contém alguns arquivos que são necessários para a atualização.
2. Copie os seguintes arquivos do projeto ASP.NET MVC 3 para o local correspondente do seu projeto ASP.NET MVC 2. Você precisará atualizar todas as referências à biblioteca jQuery para levar em conta o novo nome de arquivo (1.5.1.js jQuery): 

    - /Views/Web.config
    - /packages.config
    - /scripts/\*. js
    - /Conteúdo/temas/\*.\*
3. Cópia de *pacotes* pasta na raiz do que a solução de projeto vazia do ASP.NET MVC 3 para a raiz de sua solução, que está no diretório onde o arquivo. sln da solução está localizado.
4. Se seu projeto do ASP.NET MVC 2 contém todas as áreas, copie o arquivo de /Views/Web.config para o *modos de exibição* pasta de cada área.
5. Em ambos os arquivos Web. config no projeto do ASP.NET MVC 2, globalmente pesquisar e substituir a versão do ASP.NET MVC. Localize o seguinte: 

    [!code-console[Main](mvc3-release-notes/samples/sample1.cmd)]

    Substitua-o com o seguinte:

    [!code-console[Main](mvc3-release-notes/samples/sample2.cmd)]
6. No Solution Explorer, exclua a referência a *System.Web.Mvc* (que aponta para a DLL da versão 2), em seguida, adicione uma referência ao *System.Web.Mvc* (v3.0.0.0).
7. Adicione uma referência ao System.Web.WebPages.dll e System.Web.Helpers.dll. Esses assemblies estão localizados nas seguintes pastas: 

    - %ProgramFiles%\ Microsoft ASP.NET\ASP.NET MVC 3\Assemblies
    - %ProgramFiles%\ Microsoft ASP.NET\ASP.NET Web Pages\v1.0\Assemblies
8. No Gerenciador de soluções, clique com botão direito no nome do projeto e selecione descarregar projeto. Em seguida, clique com botão direito no nome do projeto novamente e selecione Editar *ProjectName*. csproj.
9. Localize o *ProjectTypeGuids* elemento e substitua {F85E285D-A4E0-4152-9332-AB1D724D3325} por {E53F8FEA-EAE0-44A6-8774-FFD645390401}.
10. Salvar as alterações, clique com botão direito no projeto e, em seguida, selecione Recarregar projeto.
11. No arquivo de Web. config de raiz do aplicativo, adicione as seguintes configurações para o *assemblies* seção. 

    [!code-xml[Main](mvc3-release-notes/samples/sample3.xml)]
12. Se o projeto faz referência a quaisquer bibliotecas de terceiros que são compiladas usando o ASP.NET MVC 2, adicione o seguinte realçado *bindingRedirect* elemento no arquivo Web. config na raiz do aplicativo sob o  *configuração* seção: 

    [!code-xml[Main](mvc3-release-notes/samples/sample4.xml)]

<a id="tu-changes"></a>
## <a name="changes-in-aspnet-mvc-3-tools-update"></a>Alterações no ASP.NET MVC 3 ferramentas de atualização

Esta seção descreve as alterações feitas na versão de atualização de ferramentas do ASP.NET MVC 3, desde a versão RTM do ASP.NET MVC 3.

<a id="tu-AddControllerDialog"></a>
### <a name="add-controller-dialog-box-can-now-scaffold-controllers-with-views-and-data-access-code"></a>Caixa de diálogo "Adicionar controlador" pode agora criar o scaffolding de controladores com código de acesso a dados e exibições

Scaffolding é uma maneira de gerar rapidamente um controlador e exibições para seu aplicativo. O código foi gerado, você pode editá-lo para atender às necessidades do seu projeto.

Para iniciar o *Adicionar controlador* caixa de diálogo no ASP.NET MVC 3, clique com botão direito a *controladores* pasta no *Gerenciador de soluções*, clique em *Add*e, em seguida, clique em *controlador*. A caixa de diálogo foi aprimorada para oferecer opções adicionais de scaffolding.

![](mvc3-release-notes/_static/image1.png)

Há três modelos de scaffolding disponíveis por padrão.

#### <a name="empty-controller"></a>Controlador vazio

Esse modelo gera um arquivo de controlador vazia. Esse modelo é equivalente a não verificando *adicionar ações para criar, editar, detalhes e excluir cenários* nas versões anteriores do ASP.NET MVC. Se você optar por isso, não há mais opções estão disponíveis.

#### <a name="controller-with-empty-readwrite-actions"></a>Controlador com ações de leitura/gravação vazias

Esse modelo gera um arquivo controlador que tem todos os métodos de ação necessária, mas nenhum código de implementação nos métodos. Esse modelo é equivalente a marcar *adicionar ações para criar, editar, detalhes e excluir cenários* nas versões anteriores do ASP.NET MVC. Se você optar por isso, não há mais opções estão disponíveis.

#### <a name="controller-with-readwrite-actions-and-views-using-entity-framework"></a>Controlador com ações de leitura/gravação e modos de exibição, usando o Entity Framework

Este modelo permite que você crie rapidamente uma interface de usuário de entrada de dados de trabalho. Ele gera o código que lida com uma variedade de requisitos e cenários, como procedimentos comuns:

- *Acesso a dados*. O código gerado lê e grava as entidades em um banco de dados. Ele funciona com a abordagem do Entity Framework Code First, se você escolher uma classe de contexto de dados existente ou se você permitir que o modelo gera um novo *DbContext* classe. Ele também funciona com a abordagem Model First ou Database First do Entity Framework se você escolher um existente *ObjectContext* classe.
- *Validação*. O código gerado usa associação de modelo ASP.NET MVC e recursos de metadados para que os envios de formulário são validados de acordo com as regras declaradas em sua classe de modelo. Isso inclui regras de validação internas, como o *necessária* e *StringLength* atributos e regras de validação personalizadas.
- *Relações um-para-muitos*. Se você definir relações de chave estrangeira de um-para-muitos entre suas classes de modelo, o código gerado produzirá listas suspensas para selecionar as entidades relacionadas. Por exemplo, você pode definir as seguintes classes de modelo segue as convenções de Code First do Entity Framework: 

    [!code-csharp[Main](mvc3-release-notes/samples/sample5.cs)]

    Quando você, em seguida, criar o scaffolding de um controlador para o *produto* classe, suas exibições permitirá que os usuários escolham uma *categoria* para cada objeto *produto* instância.

    Este modelo permite opções adicionais na *Adicionar controlador* caixa de diálogo. Para *classe de modelo*, você pode escolher qualquer classe de modelo em sua solução, que determina o tipo de dados que os usuários poderão criar ou editar:
- Se você quiser usar o Entity Framework Code First, você pode escolher qualquer classe de modelo.
- Se você estiver usando o Entity Framework Model First ou Database First do Entity Framework, certifique-se de escolher uma classe de entidade definida no modelo conceitual.

Para *classe de contexto de dados*, você pode fazer essas opções:

- Se você quiser usar o Code First e não têm nenhum contexto de dados existente da classe, escolha * * novo contexto de dados * *. Uma classe de contexto de dados, em seguida, será gerada para você.
- Se você quiser usar o Code First e ter uma classe de contexto de dados existente, escolha-o aqui. Ele será atualizado para manter a classe de modelo que você selecionou.
- Se você estiver usando Model First ou Database First, escolha sua classe de contexto de objeto aqui.

Para modos de exibição, escolha o mecanismo de exibição que você deseja usar, ou escolha Nenhum se você não quiser criar o scaffolding de todas as exibições.

Você pode selecionar opções avançadas especificar mais opções para os modos de exibição gerados. Por exemplo, você pode escolher o layout ou página mestra para usar.

<a id="tu-ImprovementsNewDialogBox"></a>
### <a name="improvements-to-the-aspnet-mvc-3-new-project-dialog-box"></a>Melhorias para o "ASP.NET MVC 3 novo projeto de" caixa de diálogo

A caixa de diálogo, que você pode usar para criar novos projetos do ASP.NET MVC 3 inclui vários aprimoramentos, como listado abaixo.

![](mvc3-release-notes/_static/image2.png)

#### <a name="new-intranet-project-template"></a>Novo modelo "Projeto Intranet"

Lista de modelos de projeto inclui um novo modelo de aplicativo de Intranet. Este modelo contém configurações para a criação de um aplicativo web usando a autenticação do Windows em vez da autenticação de formulários. Como um aplicativo de intranet requer algumas configurações de IIS não podem ser encapsuladas em um modelo de projeto, o modelo inclui um arquivo Leiame com instruções sobre como fazer com que o modelo de projeto funcionar no IIS. Documentação para o novo modelo de aplicativo de Intranet está disponível no site da MSDN na seguinte URL:

[https://msdn.microsoft.com/library/gg703322(VS.98).aspx](https://msdn.microsoft.com/library/gg703322(VS.98).aspx)

#### <a name="project-templates-are-now-html5-enabled"></a>Modelos de projeto agora estão HTML5 habilitado

A caixa de diálogo Novo projeto agora contém uma opção para adicionar recursos específicos do HTML5 para os modelos de projeto. Selecionando a opção faz com que os modos de exibição a ser gerado que contêm o novo HTML5 `<header>`, `<footer>`, e `<navigation>` elementos.

Observe que as versões anteriores de navegadores não dão suporte a marcas específicas do HTML5. Para resolver essa limitação, os modelos de projeto HTML5 incluem uma referência à biblioteca do Modernizr. (Consulte a próxima seção).

<a id="tu-Modernizr"></a>
### <a name="project-templates-now-include-modernizr-17"></a>Modelos de projeto agora incluem o Modernizr 1.7

O Modernizr é uma biblioteca JavaScript que habilita o suporte para CSS 3 e o HTML5 em navegadores que não ainda dão suporte a esses recursos. Essa biblioteca é incluída como um pacote do NuGet pré-instalado nos modelos para projetos do ASP.NET MVC 3. Para obter mais informações sobre o Modernizr, consulte [ http://www.modernizr.com/ ](http://www.modernizr.com/).

<a id="tu-UpdatedJQuery"></a>
### <a name="project-templates-include-updated-versions-of-jquery-jquery-ui-and-jquery-validation"></a>Modelos de projeto incluem versões atualizadas do jQuery, o jQuery UI e o jQuery validação

Os modelos de projeto agora incluem as seguintes versões dos scripts do jQuery:

- jQuery 1.5.1
- 1.8 de validação do jQuery
- jQuery UI 1.8.11

Essas bibliotecas são incluídas como previamente instalados pacotes do NuGet.

<a id="tu-EF"></a>
### <a name="project-templates-now-include-adonet-entity-framework-41-as-a-pre-installed-nuget-package"></a>Modelos de projeto agora incluem o ADO.NET Entity Framework 4.1 como um pacote do NuGet pré-instalados

O ADO.NET Entity Framework 4.1 inclui o recurso de Code First. Code First é um novo padrão de desenvolvimento para o ADO.NET Entity Framework que fornece uma alternativa para os padrões Database First e Model First existentes.

Código pela primeira vez se concentra em torno de definição de seu modelo usando classes POCO ("plain old CLR objects"), escritas em Visual Basic ou c#. Essas classes, em seguida, podem ser mapeadas para um banco de dados existente ou ser usadas para gerar um esquema de banco de dados. Configuração adicional pode ser fornecida usando *DataAnnotations* atributos ou usando APIs fluentes.

Documentação para usar o código Firstwith ASP.NET MVC está disponível no site do ASP.NET nas seguintes URLs:

[https://www.asp.net/mvc/tutorials/getting-started-with-mvc3-part1-cs](../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) [https://www.asp.net/entity-framework/tutorials/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)

<a id="tu-JavaScriptLibsNuget"></a>
### <a name="project-templates-include-javascript-libraries-as-pre-installed-nuget-packages"></a>Modelos de projeto incluem bibliotecas JavaScript como previamente instalados pacotes do NuGet

Quando você cria um novo projeto ASP.NET MVC 3, o projeto inclui os arquivos de JavaScript mencionados anteriormente (por exemplo, a biblioteca do Modernizr) ao instalá-los usando o NuGet em vez de adicionar diretamente os scripts para a pasta de Scripts no modelo de projeto conteúdo. Isso permite que você use o NuGet para atualizar os scripts para a versão mais recente quando são lançadas novas versões dos scripts.

Por exemplo, dada a frequência das novas versões do jQuery, a versão do jQuery incluído no modelo de projeto em algum momento será desatualizado. No entanto, como jQuery está incluído como um pacote NuGet instalado, você será notificado na caixa de diálogo do NuGet quando versões mais recentes do jQuery estão disponíveis.

Como jQuery inclui o número de versão no nome do arquivo, também atualizar o jQuery para a versão mais recente requer a atualização de `<script>` marca que faz referência ao arquivo de jQuery para usar o novo nome de arquivo. Outras bibliotecas de script incluídos não incluem o número de versão no nome do script, para que possam ser atualizadas com mais facilidade em suas versões mais recentes.

<a id="tu-KI"></a>
## <a name="known-issues"></a>Problemas Conhecidos

- Em alguns casos, a instalação poderá falhar com o erro mensagem "instalação falha com código de erro (0x80070643)". Para obter informações sobre como contornar esse problema, consulte [artigo da Base de Conhecimento 2531566](https://support.microsoft.com/kb/2531566).
- O scaffolding para adicionar um controlador não realizar o scaffolding de entidades que tiram proveito do suporte à herança de entidade dentro do Entity Framework. Por exemplo, dada uma base *pessoa* classe é herdada por uma *aluno* classe scaffolding a *aluno* classe resultará no código gerado não é compilado.
- Criando um novo projeto ASP.NET MVC 3 dentro de uma pasta de solução faz com que um *NullReferenceException* erro. A solução alternativa é criar o projeto do ASP.NET MVC 3 na raiz da solução e, em seguida, mova-o para a pasta da solução.
- IntelliSense para a sintaxe do Razor não funciona quando o ReSharper está instalado. Se você tiver o ReSharper instalado e quiser aproveitar o suporte do IntelliSense do Razor no ASP.NET MVC 3, consulte a entrada [Razor Intellisense e o ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) no blog de Hadi Hariri, que descreve maneiras de usá-los juntos hoje mesmo.
- Durante a instalação, a caixa de diálogo de aceitação do EULA exibe os termos de licença em uma janela que é menor do que o pretendido.
- Quando você estiver editando uma exibição do Razor (. cshtml ou. *vbhtml* arquivo), modos de exibição. O ASP.NET MVC 3 não inclui qualquer trechos de código para exibições do Razor... aspxselecting um trecho de código para ASP.NET MVC será mostrar trechos de código para
- Se você instala o ASP.NET MVC 3 para o Visual Web Developer Express em um computador em que o Visual Studio não está instalado e, em seguida, instala posteriormente o Visual Studio, você deverá reinstalar o ASP.NET MVC 3. O Visual Studio e Visual Web Developer Express compartilham componentes que são atualizados pelo instalador do ASP.NET MVC 3. O mesmo problema se aplica se você instalar o ASP.NET MVC 3 para o Visual Studio em um computador que não tenha o Visual Web Developer Express e, em seguida, instalar posteriormente o Visual Web Developer Express.

<a id="MVC3RTM"></a>
## <a name="changes-in-aspnet-mvc-3-rtm"></a>Alterações no RTM do ASP.NET MVC 3

Esta seção descreve as alterações e correções de bugs feitas na versão RTM do ASP.NET MVC 3, desde o lançamento do RC2.

<a id="RTM-1"></a>
### <a name="change-updated-the-version-of-jquery-ui-to-187"></a>Alteração: A versão atualizada do jQuery UI para 1.8.7

Os modelos de projeto do ASP.NET MVC para o Visual Studio foram atualizados para incluir a versão mais recente da biblioteca de interface do usuário do jQuery. Os modelos também incluem o conjunto mínimo de arquivos de recurso exigido pela interface do usuário, como o CSS associados e arquivos de imagem do jQuery.

<a id="RTM-2"></a>
### <a name="change-changed-the-default-modelmetadataprovider-back-to-dataannotationsmodelmetadataprovider"></a>Alteração: Alterado o padrão ModelMetadataProvider de volta para DataAnnotationsModelMetadataProvider

A versão RC2 do ASP.NET MVC 3 introduziu uma *CachedDataAnnotationsMetadataProvider* classe que a fornecida armazenando em cache sobre o existente *DataAnnotationsModelMetadataProvider* da classe como um melhoria no desempenho. No entanto, alguns erros foram relatados com essa implementação, portanto, a alteração foi revertida e movida para o projeto MVC Futures, que está disponível em [WebStack ASP.NET](https://github.com/aspnet/AspNetWebStack).

<a id="RTM-3"></a>
### <a name="fixed-pasting-part-of-a-razor-expression-that-contains-whitespace-results-in-it-being-reversed"></a>Corrigido: Colando parte de uma expressão Razor que contém os resultados de espaço em branco nele que está sendo revertido

Em versões de pré-lançamento do ASP.NET MVC 3, quando você cola uma parte de uma expressão Razor que contém espaço em branco em um arquivo Razor, a expressão resultante é invertida. Por exemplo, considere o seguinte bloco de código do Razor:

[!code-cshtml[Main](mvc3-release-notes/samples/sample6.cshtml)]

Se você selecionar o texto "primeiro param" no primeiro método e cole-o como um argumento para o segundo método, o resultado é da seguinte maneira:

[!code-cshtml[Main](mvc3-release-notes/samples/sample7.cshtml)]

O comportamento correto é que a operação de colagem deve resultar no seguinte:

[!code-cshtml[Main](mvc3-release-notes/samples/sample8.cshtml)]

Esse problema foi corrigido na versão RTM, para que a expressão corretamente é preservada durante a operação de colagem.

<a id="RTM-4"></a>
### <a name="fixed-renaming-a-razor-file-that-is-opened-in-the-editor-disables-syntax-colorization-and-intellisense"></a>Corrigido: Renomear um arquivo Razor que é aberto no editor de desabilita a colorização de sintaxe e IntelliSense

Renomear um arquivo Razor usando o Gerenciador de soluções enquanto o arquivo é aberto na janela do editor faz com que o realce de sintaxe e IntelliSense pare de funcionar para esse arquivo. Esse problema foi corrigido para que o realce e IntelliSense são mantidas após uma renomeação.

<a id="RTM-KI"></a>
## <a name="known-issues"></a>Problemas Conhecidos

- Se você fechar o Visual Studio 2010 SP1 Beta enquanto o NuGet Package Manager Console é aberto, o Visual Studio falha e tenta ser reiniciado. Isso será corrigido na versão RTM do Visual Studio 2010 SP1.
- O instalador do ASP.NET MVC 3 só é capaz de instalar uma versão inicial do Gerenciador de pacotes NuGet. Depois de ter instalado a versão inicial, o NuGet pode ser instalado e atualizados usando o Gerenciador de extensões do Visual Studio. Se você tiver o NuGet instalado, vá para a Galeria de extensão do Visual Studio para atualizar para a versão mais recente do NuGet.
- Criando um novo projeto ASP.NET MVC 3 dentro de uma pasta de solução faz com que um *NullReferenceException* erro. A solução alternativa é criar o projeto do ASP.NET MVC 3 na raiz da solução e, em seguida, mova-o para a pasta da solução.
- O instalador pode levar muito mais do que as versões anteriores do ASP.NET MVC para ser concluída. Isso ocorre porque ele atualiza os componentes do Visual Studio 2010.
- IntelliSense para a sintaxe do Razor não funciona quando o ReSharper está instalado. Se você tiver o ReSharper instalado e quiser aproveitar o suporte do IntelliSense do Razor no ASP.NET MVC 3, consulte a entrada [Razor Intellisense e o ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) no blog de Hadi Hariri, que descreve maneiras de usá-los juntos hoje mesmo.
- Exibições CCSHTML e VBHTML criadas com a versão Beta do ASP.NET MVC 3 não tem as ações de build definida corretamente, sabendo que elas exibir tipos são omitidos quando o projeto é publicado. O valor de ação de Build para esses arquivos deve ser definido como "Conteúdo". RTM do ASP.NET MVC 3 corrige esse problema para novos arquivos, mas não corrigir a configuração de arquivos existentes para um projeto criado com as versões de pré-lançamento.
- ![](mvc3-release-notes/_static/image3.png)
- Durante a instalação, a caixa de diálogo de aceitação do EULA exibe os termos de licença em uma janela que é menor do que o pretendido.
- Quando você estiver editando uma exibição do Razor (arquivo. cshtml), o item de menu Ir para o controlador no Visual Studio não estará disponível e não há nenhum trecho de código.
- Se você instala o ASP.NET MVC 3 para o Visual Web Developer Express em um computador em que o Visual Studio não está instalado e, em seguida, instala posteriormente o Visual Studio, você deverá reinstalar o ASP.NET MVC 3. O Visual Studio e Visual Web Developer Express compartilham componentes que são atualizados pelo instalador do ASP.NET MVC 3. O mesmo problema se aplica se você instalar o ASP.NET MVC 3 para o Visual Studio em um computador que não tenha o Visual Web Developer Express e, em seguida, instalar posteriormente o Visual Web Developer Express.

<a id="RTM-BC"></a>
## <a name="breaking-changes"></a>Alterações significativas

- Nas versões anteriores do ASP.NET MVC, a ação que os filtros são criar por solicitação, exceto em alguns casos. Esse comportamento nunca foi um comportamento garantido, mas meramente um detalhe de implementação e o contrato para filtros era considerá-las sem monitoração de estado. No ASP.NET MVC 3, os filtros são armazenados em cache mais agressiva. Portanto, qualquer filtro de ação personalizada que armazenar incorretamente o estado da instância pode estar quebrado.
- A ordem de execução para filtros de exceção foi alterado para filtros de exceção que têm o mesmo *ordem* valor. No ASP.NET MVC 2 e versões anteriores, filtros de exceção no controlador que têm o mesmo *ordem* valor conforme aqueles em um método de ação são executados antes dos filtros de exceção no método de ação. Isso normalmente seria o caso quando os filtros de exceção são aplicados sem um especificado *ordem* valor. No ASP.NET MVC 3, nessa ordem foi revertida para que o manipulador de exceção mais específico seja executado pela primeira vez. Como nas versões anteriores, se o *ordem* propriedade explicitamente é especificada, os filtros são executados na ordem especificada.
- Uma nova propriedade chamada *FileExtensions* foi adicionado para o *VirtualPathProviderViewEngine* classe base. Quando o ASP.NET procura uma exibição pelo caminho (não por nome), são consideradas apenas exibições com uma extensão de arquivo contido na lista especificada por essa nova propriedade. Isso é uma alteração significativa nos aplicativos em que um provedor de build personalizado está registrado para habilitar uma extensão de arquivo personalizada para modos de exibição de formulário da Web e o provedor faz referência a esses modos de exibição usando um caminho completo em vez de um nome. A solução é modificar o valor da *FileExtensions* propriedade para incluir a extensão de arquivo personalizado.
- Implementações de fábrica do controlador personalizado que implementam diretamente o *IControllerFactory* interface deve fornecer uma implementação da nova *GetControllerSessionBehavior* método foi adicionado à interface nesta versão. Em geral, é recomendável que você não implemente essa interface diretamente e em vez disso, derive sua classe de *DefaultControllerFactory*.

<a id="_Toc2"></a>
## <a name="changes-in-aspnet-mvc-3-rc2"></a>Alterações no ASP.NET MVC 3 RC2

Esta seção descreve as alterações (novos recursos e correções de bugs) feitas na versão RC2 do ASP.NET MVC 3, desde a versão RC.

<a id="_Toc2_1"></a>
### <a name="project-templates-changed-to-include-jquery-144-jquery-validation-17-and-jquery-ui-186"></a>Modelos de projeto alterados para incluir jQuery 1.4.4, 1.7 de validação do jQuery e o jQuery UI 1.8.6

Os modelos de projeto do ASP.NET MVC 3 agora incluem as versões mais recentes do jQuery, a validação do jQuery e o jQuery UI. jQuery UI é uma novidade para os modelos de projeto e fornece os widgets de interface do usuário útil. Para obter mais informações sobre o jQuery UI, visite sua home page: [ http://jqueryui.com/ ](http://jqueryui.com/).

<a id="_Toc2_2"></a>
### <a name="added-additionalmetadataattribute-class"></a>Classe de "AdditionalMetadataAttribute" adicionado

Você pode usar o *AdditionalMetadataAttribute* classe para preencher o *ModelMetadata.AdditionalValues* dicionário para uma propriedade de modelo.

Por exemplo, suponha que um modelo de exibição tem propriedades que devem ser exibidas somente para o administrador. Esse modelo pode ser anotado com o novo atributo usando AdminOnly como a chave e o verdadeiro como o valor, como no exemplo a seguir:

[!code-csharp[Main](mvc3-release-notes/samples/sample9.cs)]

Esses metadados é disponibilizado para qualquer exibição ou o editor de modelo quando um modelo de exibição de produto é renderizado. Ele cabe a você como desenvolvedor de aplicativos para interpretar as informações de metadados.

<a id="_Toc2_3"></a>
### <a name="improved-view-scaffolding"></a>Modo de exibição aprimorado Scaffolding

Os modelos T4 usados para exibições de scaffolding agora geram chamadas para métodos de auxiliares de modelo, como *EditorFor* em vez de auxiliares, como *TextBoxFor*. Essa alteração melhora o suporte para metadados com o modelo na forma de atributos de anotação de dados quando a caixa de diálogo Adicionar modo de exibição gera uma exibição.

O scaffolding de adicionar exibição também inclui detecção aprimorada e o uso de informações de chave primária no modelo, baseado em convenção. Por exemplo, a caixa de diálogo Adicionar modo de exibição usa essas informações para garantir que o valor de chave primária não é feito o scaffolding de um campo de formulário editável.

O padrão de editar e criar modelos de incluem referências a jQuery scripts necessários para a validação do cliente.

<a id="_Toc2_4"></a>
### <a name="added-htmlraw-method"></a>Método de HTML. RAW adicionado

Por padrão, o Razor mecanismo de exibição HTML codifica todos os valores. Por exemplo, o trecho de código a seguir codifica o HTML dentro a variável de saudação para que ele seja exibido na página como `<strong>Hello World!</strong>`.

[!code-cshtml[Main](mvc3-release-notes/samples/sample10.cshtml)]

O novo *HTML. RAW* método fornece uma maneira simples de exibição de HTML não codificado quando o conteúdo é conhecido por ser seguro. O exemplo a seguir exibe a mesma cadeia de caracteres, mas a cadeia de caracteres é renderizada como marcação:

[!code-cshtml[Main](mvc3-release-notes/samples/sample11.cshtml)]

<a id="_Toc2_5"></a>
### <a name="renamed-controllerviewmodel-property-and-the-view-property-to-viewbag"></a>Propriedade renomeado "Controller.ViewModel" e a propriedade de "Exibição" para "ViewBag"

Anteriormente, o *ViewModel* propriedade de *controlador* correspondessem à *exibição* propriedade de um modo de exibição. Essas propriedades fornecem uma maneira para acessar valores da *ViewDataDictionary* usando a sintaxe de acessador de propriedade dinâmico do objeto. Ambas as propriedades foram renomeadas para ser o mesmo para evitar confusão e ser mais consistente.

<a id="_Toc2_6"></a>
### <a name="renamed-controllersessionstateattribute-class-to-sessionstateattribute"></a>Classe de "ControllerSessionStateAttribute" renomeado para "SessionStateAttribute"

O *ControllerSessionStateAttribute* classe foi introduzido na versão RC do ASP.NET MVC 3. A propriedade foi renomeada para ser mais sucinta.

<a id="_Toc2_7"></a>
### <a name="renamed-remoteattribute-fields-property-to-additionalfields"></a>Propriedade de "Campos" RemoteAttribute renomeado para "AdditionalFields"

O *RemoteAttribute* da classe *campos* propriedade causou alguma confusão entre os usuários. Essa propriedade para a renomeação *AdditionalFields* esclarece sua intenção.

<a id="_Toc2_8"></a>
### <a name="renamed-skiprequestvalidationattribute-to-allowhtmlattribute"></a>Renomeado "SkipRequestValidationAttribute" para "AllowHtmlAttribute"

O *SkipRequestValidationAttribute* atributo foi renomeado para *AllowHtmlAttribute* para representar melhor seu uso pretendido.

<a id="_Toc2_9"></a>
### <a name="changed-htmlvalidationmessage-method-to-display-the-first-useful-error-message"></a>Método alterado "Html.ValidationMessage" para exibir a primeira mensagem de erro úteis

O *Html.ValidationMessage* método foi corrigido para mostrar a primeira mensagem de erro é útil em vez de simplesmente exibir o primeiro erro.

Durante a associação de modelo, o *ModelState* dicionário pode ser preenchido de várias fontes com mensagens de erro sobre a propriedade, incluindo o próprio modelo (se ele implementa *IValidatableObject* ), de atributos de validação aplicados à propriedade e de exceções geradas, enquanto a propriedade está sendo acessada.

Quando o *Html.ValidationMessage* método exibe uma mensagem de validação, ele ignora entradas de estado de modelo que incluem uma exceção, porque eles geralmente não são destinados para o usuário final. Em vez disso, o método procura a primeira mensagem de validação que não está associado uma exceção e exibe essa mensagem. Se nenhuma mensagem desse tipo for encontrada, o padrão será uma mensagem de erro genérica que está associada com a primeira exceção.

<a id="_Toc2_10"></a>
### <a name="fixed-model-declaration-to-not-add-whitespace-to-the-document"></a>Fixo @model declaração não adicionar espaço em branco ao documento

Em versões anteriores, o <em>@model</em> declaração na parte superior de uma exibição adicionada a uma linha em branco na saída HTML renderizado. Esse problema foi corrigido para que a declaração não introduz o espaço em branco.

<a id="_Toc2_11"></a>
### <a name="added-fileextensions-property-to-view-engines-to-support-engine-specific-file-names"></a>Propriedade "FileExtensions" adicionado aos mecanismos de exibição para dar suporte a nomes de arquivo específico do mecanismo

Um mecanismo de exibição pode retornar uma exibição usando um caminho de exibição explícita como no exemplo a seguir:

[!code-csharp[Main](mvc3-release-notes/samples/sample12.cs)]

O primeiro mecanismo de exibição sempre tenta renderizar a exibição. Por padrão, o mecanismo de exibição de formulários da Web é o primeiro mecanismo de exibição; porque o mecanismo de formulários da Web não pode renderizar um modo de exibição do Razor, ocorrerá um erro. Mecanismos de exibição agora tem um *FileExtensions* propriedade que é usada para especificar quais extensões de arquivo eles oferecem suporte. Essa propriedade é verificada quando o ASP.NET determina se um mecanismo de exibição pode processar um arquivo. Essa é uma alteração significativa e mais detalhes estão incluídos na [alterações significativas](#_Toc2_BC) seção deste documento.

<a id="_Toc2_12"></a>
### <a name="fixed-labelfor-helper-to-emit-the-correct-value-for-the-for-attribute"></a>Auxiliar de fixa "LabelFor" para emitir o valor correto para o atributo "For"

Foi corrigido um bug onde o *LabelFor* método renderizado um *para* atributo que corresponda a *entrada* do elemento *nome* atributo em vez disso de sua ID. Acordo com o W3C, o *para* atributo deve corresponder a *entrada* ID. do elemento

<a id="_Toc2_13"></a>
### <a name="fixed-renderaction-method-to-give-explicit-values-precedence-during-model-binding"></a>Método fixa "RenderAction" dar prioridade valores explícitos durante a associação de modelo

Em versões anteriores, os valores explícitos que foram passados para o *RenderAction* método estavam sendo ignorado em favor dos valores de formulário atuais durante a associação de modelo dentro de uma ação filha. A correção garante que os valores explícitos têm precedência durante a associação de modelo.

<a id="_Toc2_BC"></a>
## <a name="breaking-changes"></a>Alterações significativas

- Nas versões anteriores do ASP.NET MVC, os filtros de ação foram criados por solicitação, exceto em alguns casos. Esse comportamento nunca foi um comportamento garantido, mas meramente um detalhe de implementação e o contrato para filtros era considerá-las sem monitoração de estado. No ASP.NET MVC 3, os filtros são armazenados em cache mais agressiva. Portanto, qualquer filtro de ação personalizada que armazenar incorretamente o estado da instância pode estar quebrado.
- A ordem de execução para filtros de exceção foi alterado para filtros de exceção que têm o mesmo *ordem* valor. No ASP.NET MVC 2 e versões anteriores, filtros de exceção no controlador que tinha o mesmo *ordem* valor conforme aqueles em um método de ação foram executados antes dos filtros de exceção no método de ação. Isso normalmente seria o caso quando os filtros de exceção foram aplicados sem um especificado *ordem* valor. No ASP.NET MVC 3, nessa ordem foi revertida para que o manipulador de exceção mais específico seja executado pela primeira vez. Como nas versões anteriores, se o *ordem* propriedade explicitamente é especificada, os filtros são executados na ordem especificada.
- Uma nova propriedade chamada *FileExtensions* foi adicionado para o *VirtualPathProviderViewEngine* classe base. Quando o ASP.NET procura uma exibição pelo caminho (não por nome), são consideradas apenas exibições com uma extensão de arquivo contido na lista especificada por essa nova propriedade. Isso é uma alteração significativa nos aplicativos em que um provedor de build personalizado está registrado para habilitar uma extensão de arquivo personalizada para modos de exibição de formulário da Web e o provedor faz referência a esses modos de exibição usando um caminho completo em vez de um nome. A solução é modificar o valor da *FileExtensions* propriedade para incluir a extensão de arquivo personalizado.
- Implementações de fábrica do controlador personalizado que implementam diretamente o <em>IControllerFactory</em> interface deve fornecer uma implementação da nova <em>GetControllerSessionBehavior</em>  <em>método que foi adicionado à interface nesta versão</em>. Em geral, é recomendável que você não implemente essa interface diretamente e em vez disso, derive sua classe de <em>DefaultControllerFactory</em>.

<a id="_Toc2_KI"></a>
## <a name="known-issues"></a>Problemas Conhecidos

- O instalador do ASP.NET MVC 3 só é capaz de instalar uma versão inicial do Gerenciador de pacotes NuGet. Depois de ter instalado a versão inicial, o NuGet pode ser instalado e atualizados usando o Gerenciador de extensões do Visual Studio. Se você tiver o NuGet instalado, vá para a Galeria de extensão do Visual Studio para atualizar para a versão mais recente do NuGet.
- Criando um novo projeto ASP.NET MVC 3 dentro de uma pasta de solução faz com que um *NullReferenceException* erro. A solução alternativa é criar o projeto do ASP.NET MVC 3 na raiz da solução e, em seguida, mova-o para a pasta da solução.
- O instalador pode levar muito mais do que as versões anteriores do ASP.NET MVC para ser concluída. Isso ocorre porque ele atualiza os componentes do Visual Studio 2010.
- IntelliSense para a sintaxe do Razor não funciona quando o ReSharper está instalado. Se você tiver o ReSharper instalado e quiser aproveitar o suporte do IntelliSense do Razor no ASP.NET MVC 3 RC2, consulte a entrada [Razor Intellisense e o ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) no blog de Hadi Hariri, que descreve maneiras de usá-los juntos hoje mesmo.
- CSHTML e VBHTML exibições criadas com a versão Beta do ASP.NET MVC 3 não tem as ações de build definida corretamente, sabendo que elas exibir tipos são omitidos quando o projeto é publicado. O *ação de compilação* valor para esses arquivos devem ser definidos como conteúdo ". ASP.NET MVC 3 RC2 corrige esse problema para novos arquivos, mas não corrigir a configuração de arquivos existentes para um projeto criado com a versão Beta.![](mvc3-release-notes/_static/image4.png)
- Durante a instalação, a caixa de diálogo de aceitação do EULA exibe os termos de licença em uma janela que é menor do que o pretendido.
- Quando você estiver editando uma exibição do Razor (arquivo. cshtml), o item de menu Ir para o controlador no Visual Studio não estará disponível e não há nenhum trecho de código.
- Se você instala o ASP.NET MVC 3 para o Visual Web Developer Express em um computador em que o Visual Studio não está instalado e, em seguida, instala posteriormente o Visual Studio, você deverá reinstalar o ASP.NET MVC 3. O Visual Studio e Visual Web Developer Express compartilham componentes que são atualizados pelo instalador do ASP.NET MVC 3. O mesmo problema se aplica se você instalar o ASP.NET MVC 3 para o Visual Studio em um computador que não tenha o Visual Web Developer Express e, em seguida, instalar posteriormente o Visual Web Developer Express.
- Instalar o ASP.NET MVC 3 RC 2 não atualizar o NuGet, se você já tiver instalado. Para atualizar o NuGet, vá para o Gerenciador de extensões do Visual Studio e ele deve aparecer como uma atualização disponível. Você pode atualizar o NuGet para a versão mais recente a partir daí.

<a id="TOC_ASP_NET_3_RC"></a>
## <a name="aspnet-mvc-3-release-candidate"></a>Versão Release Candidate do ASP.NET MVC 3

ASP.NET MVC versão Release Candidate foi lançada em 9 de novembro de 2010.

<a id="_Toc276711785"></a>
## <a name="new-features-in-aspnet-mvc-3-rc"></a>Novos recursos no ASP.NET MVC 3 RC

Esta seção descreve os recursos que foram introduzidos na versão RC do ASP.NET MVC 3 desde a versão Beta.

<a id="_Toc276711786"></a>
### <a name="nuget-package-manager"></a>Gerenciador de pacotes NuGet

O ASP.NET MVC 3 inclui o NuGet Package Manager (anteriormente conhecido como NuPack), que é uma ferramenta de gerenciamento de pacote integrado para adicionar as bibliotecas e ferramentas para projetos do Visual Studio. Essa ferramenta automatiza as etapas que os desenvolvedores usam hoje para obter uma biblioteca em sua árvore de origem.

Você pode trabalhar com o NuGet como uma ferramenta de linha de comando, como uma janela de console integrado do Visual Studio 2010, no menu de contexto do Visual Studio e como um conjunto de cmdlets do PowerShell.

Para obter mais informações sobre o NuGet, leia as [documentação do Nuget](https://docs.microsoft.com/nuget/).

<a id="_Toc276711787"></a>
### <a name="improved-new-project-dialog-box"></a>Aprimorado "Novo projeto" caixa de diálogo

Quando você cria um novo projeto, a caixa de diálogo Novo projeto agora permite especificar o mecanismo de exibição, bem como um tipo de projeto do ASP.NET MVC.

![](mvc3-release-notes/_static/image5.png)

Suporte para modificar a lista de modelos e mecanismos listados na caixa de diálogo de exibição está incluído nesta versão.

Os modelos padrão são os seguintes:

Vazio. Contém um conjunto mínimo de arquivos para um projeto ASP.NET MVC, incluindo a estrutura de pastas padrão para projetos do ASP.NET MVC, um arquivo CSS que contém o padrão de que estilos do ASP.NET MVC e um diretório de Scripts que contém os arquivos de JavaScript padrão.

Aplicativo de Internet. Contém a funcionalidade de exemplo que demonstra como usar o provedor de associação com o ASP.NET MVC.

A lista de modelos de projeto que é exibida na caixa de diálogo é especificada no registro do Windows.

<a id="_Toc276711788"></a>
### <a name="sessionless-controllers"></a>Controladores sem sessão

O novo *ControllerSessionStateAttribute* fornece mais controle sobre o comportamento de estado de sessão para controladores, especificando um [System.Web.SessionState.SessionStateBehavior](https://msdn.microsoft.com/library/system.web.sessionstate.sessionstatebehavior.aspx) valor de enumeração.

O exemplo a seguir mostra como desativar o estado de sessão para todas as solicitações para um controlador.

[!code-csharp[Main](mvc3-release-notes/samples/sample13.cs)]

O exemplo a seguir mostra como definir o estado de sessão somente leitura para todas as solicitações para um controlador.

[!code-csharp[Main](mvc3-release-notes/samples/sample14.cs)]

<a id="_Toc276711789"></a>
### <a name="new-validation-attributes"></a>Novos atributos de validação

#### <a name="compareattribute"></a>CompareAttribute

O novo *CompareAttribute* atributo de validação permite que você compare os valores de duas propriedades diferentes de um modelo. No exemplo a seguir, o *ComparePassword* propriedade deve corresponder a *senha* campo para que seja válido.

[!code-csharp[Main](mvc3-release-notes/samples/sample15.cs)]

#### <a name="remoteattribute"></a>RemoteAttribute

O novo *RemoteAttribute* atributo de validação se beneficia de validador remoto a validação do plug-in do jQuery, que habilita a validação do lado do cliente chamar um método no servidor que executa a lógica de validação real.

No exemplo a seguir, o *nome de usuário* propriedade tem o *RemoteAttribute* aplicado. Ao editar essa propriedade em uma exibição de edição, a validação do cliente chamará uma ação chamada *UserNameAvailable* sobre o *UsersController* classe a fim de validar esse campo.

[!code-csharp[Main](mvc3-release-notes/samples/sample16.cs)]

O exemplo a seguir mostra o controlador correspondente.

[!code-csharp[Main](mvc3-release-notes/samples/sample17.cs)]

Por padrão, o nome da propriedade que o atributo é aplicado é enviado para o método de ação como um parâmetro de cadeia de caracteres de consulta.

<a id="_Toc276711790"></a>
### <a name="new-overloads-for-labelfor-and-labelformodel-methods"></a>Novas sobrecargas dos métodos de "LabelForModel" e "LabelFor"

Foram adicionadas novas sobrecargas para o *LabelFor* e *LabelForModel* métodos que permitem que você especificam o texto do rótulo. O exemplo a seguir mostra como usar essas sobrecargas.

[!code-cshtml[Main](mvc3-release-notes/samples/sample18.cshtml)]

<a id="_Toc276711791"></a>
### <a name="child-action-output-caching"></a>Ação filha o cache de saída

O *OutputCacheAttribute* dá suporte a cache de saída das ações de filho que são chamadas usando o *Html.RenderAction* ou *Html.Action* métodos auxiliares. O exemplo a seguir mostra uma exibição que chama outra ação.

[!code-cshtml[Main](mvc3-release-notes/samples/sample19.cshtml)]

O *GetDate* ação é anotada com a *OutputCacheAttribute*:

[!code-csharp[Main](mvc3-release-notes/samples/sample20.cs)]

Quando esse código é executado, o resultado da chamada para Html.Action("GetDate") é armazenado em cache por 100 segundos.

<a id="_Toc276711792"></a>
### <a name="add-view-dialog-box-improvements"></a>Melhorias da caixa de diálogo "Add View"

Quando você adiciona uma exibição fortemente tipada, a caixa de diálogo Adicionar modo de exibição agora filtra mais tipos não aplicáveis que em versões anteriores, como muitos tipos do .NET Framework core. Além disso, a lista agora é classificada pelo nome de classe e não pelo nome do tipo totalmente qualificado, o que torna mais fácil localizar tipos. Por exemplo, o nome do tipo agora é exibido como no exemplo a seguir:

Nome da classe (namespace)

Em versões anteriores, isso tiver sido exibido da seguinte maneira:

Namespace.ClassName

<a id="_Toc276711793"></a>
### <a name="granular-request-validation"></a>Validação de solicitação granular

O *exclua* propriedade de *ValidateInputAttribute* não existe mais. Em vez disso, para que a validação de solicitação ignorada para propriedades específicas de um modelo durante a associação de modelo, use a nova *SkipRequestValidationAttribute*.

Por exemplo, suponha que um método de ação é usado para editar uma postagem de blog:

[!code-csharp[Main](mvc3-release-notes/samples/sample21.cs)]

O exemplo a seguir mostra o modelo de exibição para uma postagem de blog.

[!code-csharp[Main](mvc3-release-notes/samples/sample22.cs)]

Quando um usuário envia uma marcação para a propriedade de descrição, a associação de modelo falhará devido à validação de solicitação. Para desabilitar a validação de solicitação durante a associação de modelo para a descrição de postagem de blog, aplicar a *SkipRequpestValidationAttribute* para a propriedade, conforme mostrado neste exemplo:.

[!code-csharp[Main](mvc3-release-notes/samples/sample23.cs)]

Como alternativa, para desativar a validação de solicitação para todas as propriedades do modelo, aplique *ValidateInputAttribute* com um valor de *falso* ao método de ação:

[!code-csharp[Main](mvc3-release-notes/samples/sample24.cs)]

<a id="_Toc276711794"></a>
## <a name="breaking-changes"></a>Alterações significativas

- A ordem de execução para filtros de exceção foi alterado para filtros de exceção que têm o mesmo *ordem* valor. No ASP.NET MVC 2 e versões anteriores, filtros de exceção no controlador que tinha o mesmo *ordem* como aqueles em um método de ação foram executados antes dos filtros de exceção no método de ação. Isso normalmente seria o caso quando os filtros de exceção foram aplicados sem um especificado *ordem* valor. No ASP.NET MVC 3, nessa ordem foi revertida para que o manipulador de exceção mais específico seja executado pela primeira vez. Como nas versões anteriores, se o *ordem* propriedade explicitamente é especificada, os filtros são executados na ordem especificada.
- Adicionada uma nova propriedade chamada *FileExtensions* para o *VirtualPathProviderViewEngine* classe base. Ao procurar por uma exibição pelo caminho (e não por nome), somente as exibições com uma extensão de arquivo contido na lista especificada por essa nova propriedade é considerada. Isso é uma alteração significativa para aqueles que registrar um provedor de build personalizadas para habilitar uma extensão de arquivo personalizado para modos de exibição de formulário da web e fazem referência a essas exibições, usando um caminho completo em vez de um nome. A solução é modificar o valor da *FileExtensions* propriedade para incluir a extensão de arquivo personalizado.

<a id="_Toc276711795"></a>
## <a name="known-issues"></a>Problemas Conhecidos

- O instalador pode levar muito mais do que as versões anteriores do ASP.NET MVC para ser concluída porque ele atualiza os componentes do Visual Studio 2010.
- O scaffolding de adicionar modo de exibição ao selecionar astrongly digitado exibir scaffolds propriedades somente gravação. Eles sempre devem ser ignorados por scaffolding. A caixa de diálogo Adicionar modo de exibição também aplica Scaffold nas propriedades somente leitura ao gerar uma exibição de "Editar" ou "Criar". Propriedades somente leitura só devem ser gerado automaticamente para as exibição e exibições de lista.
- Depuração não funciona quando o ASP.NET MVC 3 é instalada junto com o CTP do Async. O ASP.NET MVC 3 não pode ser instalada lado a lado com o CTP Async. Desinstale o CTP do Async para reparar a depuração. Para obter mais detalhes, leia [esta postagem de blog](http://drew-prog.blogspot.com/2010/11/how-to-uninstall-microsoft-aspnet-mvc-3.html) sobre a desinstalação de todas as partes do ASP.NET MVC 3 RC.
- Intellisense do Razor não funciona quando o Resharper está instalado. Se você tiver o ReSharper instalado e quiser aproveitar o suporte para intellisense do Razor no ASP.NET MVC 3 RC, leia [esta postagem de blog](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) da JetBrains que descreve maneiras de usá-los juntos hoje mesmo.
- CSHTML e VBHTML modos de exibição criados com o Beta do ASP.NET MVC 3 não tem as ações de build corretamente que omite-los da publicação. O *ação de compilação* para esses arquivos devem ser definidos como "Conteúdo". RC do ASP.NET MVC 3 corrige esse problema para novos arquivos, mas não corrigir a configuração de arquivos existentes para um projeto criado com a versão Beta.
- O instalador pode levar muito mais do que as versões anteriores do ASP.NET MVC para ser concluída porque ele atualiza os componentes do Visual Studio 2010.
- O scaffolding de adicionar modo de exibição ao selecionar "Editar" fortemente tipados scaffolds do modo de exibição propriedades somente leitura. Da mesma forma, as propriedades somente gravação são gerados automaticamente para modos de exibição de "Display".
- Durante a instalação, a caixa de diálogo de aceitação do EULA exibe os termos de licença em uma janela que é menor do que o pretendido.
- Instalar o Visual Studio Async CTP causa um conflito com o lançamento do Razor que é incluído como parte da instalação das ferramentas do ASP.NET MVC 3. Certifique-se de que você não tente instalar o Visual Studio Async CTP e a versão do Razor no mesmo computador.
- Quando você estiver editando uma exibição do Razor (arquivo. cshtml), o item de menu Ir para o controlador no Visual Studio não estará disponível e não há nenhum trecho de código.

<a id="TOC_ASP_NET_3_Beta"></a>
## <a name="aspnet-mvc-3-beta"></a>Versão Beta do ASP.NET MVC 3

ASP.NET MVC 3 Beta foi lançada em 6 de outubro de 2010. As observações a seguir são específicas para a versão Beta e estão sujeitos a quaisquer atualizações ou alterações mencionadas na seção de Release Candidate do ASP.NET MVC 3 acima.

## <a id="0.1__Toc274034215"></a>  Novo Featuresin ASP.NET MVC 3 Beta

<a id="0.1__Default_validation_system"></a>Esta seção descreve os recursos que foram introduzidos na versão Beta do ASP.NET MVC 3.

### <a id="0.1__Toc274034216"></a>  Gerenciador de pacotes NuGet

O ASP.NET MVC 3 inclui o Gerenciador de pacotes do NuGet, que é uma ferramenta de gerenciamento de pacote integrado para adição de bibliotecas e ferramentas para projetos do Visual Studio. Na maior parte, ele automatiza as etapas que os desenvolvedores usam hoje para obter uma biblioteca em sua árvore de origem.

Você pode trabalhar com o NuGet como uma ferramenta de linha de comando, como uma janela de console integrado do Visual Studio 2010, no menu de contexto do Visual Studio e como o conjunto de cmdlets do PowerShell.

Para obter mais informações sobre o NuGet, leia as [documentação do NuGet](https://docs.microsoft.com/nuget/).

### <a id="0.1__Toc274034217"></a>  Aprimorada a caixa de diálogo Novo projeto

Quando você cria um novo projeto, a caixa de diálogo Novo projeto agora permite especificar o mecanismo de exibição, bem como um tipo de projeto do ASP.NET MVC.

![](mvc3-release-notes/_static/image6.png)

Suporte para modificar a lista de modelos e exibição mecanismos listada na caixa de diálogo não está incluído nesta versão.

Os modelos padrão são os seguintes:

Vazio. Contém um conjunto mínimo de arquivos para um projeto ASP.NET MVC, incluindo a estrutura de pastas padrão para projetos do ASP.NET MVC, um pequeno arquivo CSS que contém o padrão de que estilos do ASP.NET MVC e um diretório de Scripts que contém os arquivos de JavaScript padrão.

Aplicativo de Internet. Contém a funcionalidade de exemplo que demonstra como usar o provedor de associação no ASP.NET MVC.

### <a id="0.1__Toc274034218"></a>  Maneira simplificada de especificar fortemente tipados modelos em exibições do Razor

A maneira de especificar o tipo de modelo para as exibições do Razor com rigidez de tipos foi simplificada usando o novo @model diretriz para modos de exibição CSHTML e @ModelType diretriz para exibições VBHTML. Em versões anteriores do ASP.NET MVC, você deve especificar que um modelo fortemente tipado para Razor exibições dessa maneira:

[!code-cshtml[Main](mvc3-release-notes/samples/sample25.cshtml)]

Nesta versão, você pode usar a sintaxe a seguir:

[!code-cshtml[Main](mvc3-release-notes/samples/sample26.cshtml)]

### <a id="0.1__Toc274034219"></a>  Suporte para novos métodos de auxiliares de páginas da Web do ASP.NET

A nova tecnologia de páginas da Web ASP.NET inclui um conjunto de métodos auxiliares que são úteis para adicionar funcionalidades mais comumente usadas em exibições e controladores. O ASP.NET MVC 3 oferece suporte a usar esses métodos auxiliares dentro de controladores e exibições (onde apropriado). Esses métodos estão contidos no assembly System. A tabela a seguir lista alguns dos métodos auxiliares páginas da Web ASP.NET.

| **Auxiliar** | **Descrição** |
| --- | --- |
| Gráfico | Processa um gráfico em um modo de exibição. Contém métodos como Chart.ToWebImage, Chart.Save e Chart.Write. |
| Criptografia | Usa algoritmos para criar corretamente de hash com salt e hash de senhas. |
| WebGrid | Renderiza uma coleção de objetos (normalmente, os dados de um banco de dados) como uma grade. Dá suporte à paginação e classificação. |
| WebImage | Renderiza uma imagem. |
| WebMail | Envia uma mensagem de email. |

Um tópico de referência rápida que lista os auxiliares e a sintaxe básica está disponível como parte da documentação de sintaxe do Razor do ASP.NET na seguinte URL:

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-api-reference](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)

### <a id="0.1__Toc274034220"></a>  Suporte à injeção de dependência adicional

Aproveitando o lançamento do ASP.NET MVC 3 versão prévia 1, a versão atual que inclui o suporte adicionado para dois novos serviços e quatro serviços existentes e suporte aprimorado para resolução de dependência e o Common Service Locator.

#### <a name="new-icontrolleractivator-interface-for-fine-grained-controller-instantiation"></a>Nova Interface IControllerActivator para instanciação do controlador refinado

A nova interface IControllerActivator fornece controle mais refinado sobre como os controladores são instanciados por meio da injeção de dependência. O exemplo a seguir mostra a interface:

[!code-csharp[Main](mvc3-release-notes/samples/sample27.cs)]

Compare isso à função de alocador de controlador. Um alocador de controlador é uma implementação da interface IControllerFactory, que é responsável tanto para localizar um tipo de controlador para instanciar uma instância desse tipo de controlador.

Ativadores de controlador serão responsáveis somente para instanciar uma instância de um tipo de controlador. Eles não realizam a pesquisa do tipo de controlador. Depois de localizar um tipo de controlador apropriada, alocadores de controlador devem delegar para uma instância de IControllerActivator para tratar da instanciação real do controlador.

A classe de DefaultControllerFactory tem um novo construtor que aceita uma instância de IControllerFactory. Isso permite que se aplicam a injeção de dependência para gerenciar esse aspecto da criação do controlador sem precisar substituir o comportamento de pesquisa do tipo de controlador padrão.

#### <a name="iservicelocator-interface-replaced-with-idependencyresolver"></a>Interface IServiceLocator substituídos por IDependencyResolver

Com base nos comentários da comunidade, a versão Beta do ASP.NET MVC 3 substituiu o uso da interface IServiceLocator com uma interface de IDependencyResolver enxutos específica às necessidades do ASP.NET MVC. O exemplo a seguir mostra a nova interface:

[!code-csharp[Main](mvc3-release-notes/samples/sample28.cs)]

Como parte dessa alteração, a classe ServiceLocator também foi substituída com a classe DependencyResolver. Registro de um resolvedor de dependência é semelhante às versões anteriores do ASP.NET MVC:

[!code-csharp[Main](mvc3-release-notes/samples/sample29.cs)]

Implementações dessa interface simplesmente devem delegar ao contêiner de injeção de dependência subjacente para fornecer o serviço registrado para o tipo solicitado.

Quando não houver nenhum serviço registrado do tipo solicitado, o ASP.NET MVC espera implementações dessa interface para retornar nulo dos GetService e para retornar uma coleção vazia de GetServices.

A nova classe DependencyResolver permite registrar as classes que implementam a nova interface IDependencyResolver ou a interface de localizador de serviço comum (IServiceLocator). Para obter mais informações sobre o Common Service Locator, consulte [CommonServiceLocator no GitHub](https://github.com/unitycontainer/commonservicelocator).

<a id="0.1__Breaking_Changes"></a>

#### <a name="new-iviewactivator-interface-for-fine-grained-view-page-instantiation"></a>Nova Interface IViewActivator para instanciação de página de exibição refinado

A nova interface IViewPageActivator fornece controle mais refinado sobre como exibir páginas são instanciadas por meio da injeção de dependência. Isso se aplica a instâncias de WebFormView e instâncias de RazorView. O exemplo a seguir mostra a nova interface:

[!code-csharp[Main](mvc3-release-notes/samples/sample30.cs)]

Agora, essas classes aceitam um argumento de construtor IViewPageActivator, que permite que você usar injeção de dependência para controlar como os tipos ViewPage, ViewUserControl e WebViewPage são instanciados.

#### <a name="new-dependency-resolver-support-for-existing-services"></a>Novo suporte de resolvedor de dependência para os serviços existentes

A nova versão inclui suporte à resolução de dependência para os seguintes serviços:

- Provedores de validação de modelo. As classes que implementam ModelValidatorProvider podem ser registradas no resolvedor de dependência e o sistema usará para dar suporte à validação do lado do cliente e do servidor.
- Provedor de metadados do modelo. Uma única classe que implementa ModelMetadataProvider pode ser registrada no resolvedor de dependência e o sistema usará para fornecer metadados para os sistemas de modelagem e validação.
- Provedores de valor. As classes que implementam ValueProviderFactory podem ser registradas no resolvedor de dependência e o sistema usará para criar provedores de valor que são consumidos pelo controlador e durante a associação de modelo.
- Associadores de modelo. As classes que implementam IModelBinderProvider podem ser registradas no resolvedor de dependência e o sistema usará para criar associadores de modelo que são consumidos pelo sistema de associação de modelo.

### <a id="0.1__Toc274034221"></a>  Novo suporte para o Ajax não invasiva baseada no jQuery

O ASP.NET MVC inclui métodos de auxiliares de Ajax como o seguinte:

- Ajax.ActionLink
- Ajax.RouteLink
- Ajax.BeginForm
- Ajax.BeginRouteForm

Esses métodos usam o JavaScript para invocar um método de ação no servidor em vez de usar um postback completo. Essa funcionalidade foi atualizada para tirar proveito do jQuery de forma não invasiva. Em vez de forma invasiva emitir scripts do cliente embutido, esses métodos auxiliares separam o comportamento da marcação emitindo atributos HTML5 usando o *dados ajax* prefixo. Em seguida, o comportamento é aplicado à marcação referenciando os arquivos apropriados do JavaScript. Certifique-se de que os arquivos JavaScript a seguir são referenciados:

- jquery-1.4.1.js
- jquery.unobtrusive.ajax.js

Esse recurso é habilitado por padrão no arquivo Web. config no ASP.NET MVC 3 novos modelos de projeto, mas é desabilitado por padrão para projetos existentes. Para obter mais informações, consulte [adicionado sinalizadores de todo o aplicativo para validação do cliente e o JavaScript discreto](#0.1_AddedApplicationWideFlagsForClientValida) mais adiante neste documento.

### <a id="0.1__Toc274034222"></a>  Novo suporte para jQuery Unobtrusive Validation

Por padrão, a versão Beta do ASP.NET MVC 3 usa validação do jQuery para executar a validação do lado do cliente de forma não invasiva. Para habilitar a validação não invasiva do cliente, fazer uma chamada com o seguinte de dentro de um modo de exibição:

[!code-csharp[Main](mvc3-release-notes/samples/sample31.cs)]

Isso requer que a propriedade de ViewContext.UnobtrusiveJavaScriptEnabled está definida como true, o que pode ser feito fazendo a chamada a seguir:

[!code-csharp[Main](mvc3-release-notes/samples/sample32.cs)]

Além disso, verifique se que os arquivos JavaScript a seguir são referenciados.

- jquery-1.4.1.js
- jquery.validate.js
- jquery.validate.unobtrusive.js

Esse recurso está ativado por padrão no arquivo Web. config no ASP.NET MVC 3 novos modelos de projeto, mas é desabilitado por padrão para projetos existentes. Para obter mais informações, consulte [novos sinalizadores de todo o aplicativo para validação do cliente e o JavaScript discreto](#0.1_AddedApplicationWideFlagsForClientValida) mais adiante neste documento.

<a id="0.1__Toc274034223"></a>

### <a id="0.1_AddedApplicationWideFlagsForClientValida"></a>  Novos sinalizadores de todo o aplicativo para validação do cliente e o JavaScript discreto

Você pode habilitar ou desabilitar a validação do cliente e o JavaScript discreto globalmente usando membros estáticos da classe HtmlHelper, como no exemplo a seguir:

[!code-csharp[Main](mvc3-release-notes/samples/sample33.cs)]

Os modelos de projeto padrão habilitam o JavaScript discreto por padrão. Você também pode habilitar ou desabilitar esses recursos no arquivo Web. config raiz do seu aplicativo usando as seguintes configurações:

[!code-xml[Main](mvc3-release-notes/samples/sample34.xml)]

Porque você pode habilitar esses recursos por padrão, novas sobrecargas foram introduzidas para a classe HtmlHelper que permitem que você substituir as configurações padrão, conforme mostrado nos exemplos a seguir:

[!code-csharp[Main](mvc3-release-notes/samples/sample35.cs)]

Para compatibilidade com versões anteriores, esses dois recursos são desabilitados por padrão.

### <a id="0.1__Toc274034224"></a>  Novo suporte para o código que é executado antes que a execução de modos de exibição

Agora você pode colocar um arquivo chamado \_viewstart. cshtml (ou \_viewstart.vbhtml) no diretório visualizações e adicione o código a ele que serão compartilhados entre vários modos de exibição no diretório e seus subdiretórios. Por exemplo, você pode colocar o código a seguir para o \_página viewstart. cshtml na pasta ~/Views:

[!code-cshtml[Main](mvc3-release-notes/samples/sample36.cshtml)]

Isso define a página de layout para cada exibição dentro da pasta de modos de exibição e todas as suas subpastas. Quando um modo de exibição está sendo processado, o código a \_arquivo viewstart. cshtml é executado antes de executar o código de exibição. O \_viewstart. cshtml código se aplica a cada exibição nessa pasta.

Por padrão, o código a \_arquivo viewstart. cshtml também se aplica a exibições em qualquer subpasta. No entanto, as subpastas individuais podem ter sua própria versão do \_viewstart. cshtml file; nesse caso, a versão local tem precedência. Por exemplo, para executar o código que é comum a todos os modos de exibição para o HomeController, coloque um \_arquivo viewstart. cshtml na pasta ~/Views/Home.

### <a id="0.1__Toc274034225"></a>  Novo suporte para a sintaxe do Razor VBHTML

A visualização anterior do ASP.NET MVC inclui suporte para modos de exibição usando a sintaxe do Razor com base em c#. Esses modos de exibição usam a extensão de arquivo. cshtml. Como parte de um trabalho em andamento para dar suporte ao Razor, o ASP.NET MVC 3 Beta introduz o suporte para a sintaxe do Razor no Visual Basic, que usa a extensão de arquivo. vbhtml.

Para obter uma introdução ao uso de sintaxe do Visual Basic em páginas VBHTML, consulte o tutorial na seguinte URL:

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-visual-basic](../web-pages/overview/getting-started/introducing-razor-syntax-vb.md)

### <a id="0.1__Toc274034226"></a>  Controle mais Granular sobre ValidateInputAttribute

ASP.NET MVC sempre incluiu a classe ValidateInputAttribute, que invoca a infraestrutura de validação de solicitação ASP.NET core para certificar-se de que a solicitação de entrada não contenha entrada potencialmente mal-intencionada. Por padrão, a validação de entrada está habilitada. É possível desabilitar a validação de solicitação usando o atributo ValidateInputAttribute, como no exemplo a seguir:

[!code-csharp[Main](mvc3-release-notes/samples/sample37.cs)]

No entanto, muitos aplicativos web têm campos de formulário individuais que precisam para permitir que o HTML, enquanto os campos restantes não. A classe ValidateInputAttribute agora permite especificar uma lista de campos que não devem ser incluídos na validação de solicitação.

Por exemplo, se você estiver desenvolvendo um mecanismo de blog, você talvez queira permitir marcação nos campos de corpo e o resumo. Esses campos podem ser representados por dois elementos de entrada, cada um com um atributo de nome correspondente ao nome da propriedade ("Body" e "Resumo"). Para desabilitar a solicitação de validação para esses campos somente, especifique os nomes (separada por vírgulas) na propriedade de exclusão da classe ValidateInput, como no exemplo a seguir:

[!code-csharp[Main](mvc3-release-notes/samples/sample38.cs)]

### <a id="0.1__Toc274034227"></a>  Os auxiliares de converter sublinhados hifens para nomes de atributo HTML especificados usando objetos anônimos

Métodos auxiliares permitem que você especifique pares de nome/valor de atributo usando um objeto anônimo, como no exemplo a seguir:

[!code-csharp[Main](mvc3-release-notes/samples/sample39.cs)]

Essa abordagem não permite que você use hifens no nome do atributo, pois um hífen não pode ser usado para um nome de propriedade no ASP.NET. No entanto, os hifens são importantes para os atributos personalizados do HTML5; Por exemplo, o HTML5 usa o prefixo "data-".

Ao mesmo tempo, os sublinhados não podem ser usados para nomes de atributo em HTML, mas são válidos em nomes de propriedade. Portanto, se você especificar atributos usando um objeto anônimo, e se os nomes de atributo incluam um caractere de sublinhado, métodos auxiliares converterá os sublinhados hifens. Por exemplo, a sintaxe de auxiliar a seguir usa um caractere de sublinhado:

[!code-csharp[Main](mvc3-release-notes/samples/sample40.cs)]

O exemplo anterior renderiza a marcação a seguir quando o auxiliar é executado:

[!code-html[Main](mvc3-release-notes/samples/sample41.html)]

## <a id="0.1__Toc274034228"></a>  Correções de bugs

O modelo de objeto padrão para os auxiliares de modelo EditorFor e DisplayFor agora dá suporte a ordenação especificada na propriedade DisplayAttribute.Order. (Nas versões anteriores, a configuração da ordem não foi usada.)

A validação do cliente agora dá suporte à validação de propriedades substituídas que têm atributos de validação aplicados.

JsonValueProviderFactory agora é registrado por padrão.

## <a id="0.1__Toc274034229"></a>  Alterações significativas

A ordem de execução para filtros de exceção foi alterado para filtros de exceção que têm o mesmo valor de ordem. No ASP.NET MVC 2 e versões anteriores, a exceção filtra no controlador com a mesma ordem como aqueles em um método de ação foram executados antes dos filtros de exceção no método de ação. Isso normalmente seria o caso quando os filtros de exceção foram aplicados sem um valor de ordem especificado. No ASP.NET MVC 3, nessa ordem foi revertida para que o manipulador de exceção mais específico seja executado pela primeira vez. Como nas versões anteriores, se a propriedade de ordem for especificada explicitamente, os filtros são executados na ordem especificada.

## <a id="0.1__Toc274034230"></a>  Problemas conhecidos

Durante a instalação, a caixa de diálogo de aceitação do EULA exibe os termos de licença em uma janela que é menor do que o pretendido.

Exibições do Razor não tem suporte ao IntelliSense nem o realce de sintaxe. Espera-se que o suporte para sintaxe do Razor no Visual Studio será incluído como parte de uma versão posterior.

Quando você estiver editando uma exibição do Razor (arquivo CSHTML), o <a id="0.1__Toc224729061"> </a> <a id="0.1__Toc238051347"> </a> item de menu Ir para o controlador no Visual Studio não estará disponível e não há nenhum trecho de código.

Ao usar o @model exibir de sintaxe para especificar um CSHTML com rigidez de tipos, atalhos de idioma específico para tipos não são reconhecidos. Por exemplo, @model int não funcionará, mas @model Int32 funcionará. A solução alternativa para esse bug é usar o nome do tipo real quando você especifica o tipo de modelo.

Ao usar o @model sintaxe para especificar um modo de exibição CSHTML com rigidez de tipos (ou @ModelType para especificar uma exibição fortemente tipada de VBHTML), não há suporte para tipos anuláveis e declarações de matriz. Por exemplo, @model int? não tem suporte. Em vez disso, use `@model Nullable<Int32>`. A sintaxe @model string [] também não tem suporte; em vez disso, use `@model IList<string>`.

Quando você atualiza um projeto ASP.NET MVC 2 para o ASP.NET MVC 3, certifique-se de adicionar o seguinte à seção appSettings do arquivo Web. config:

[!code-xml[Main](mvc3-release-notes/samples/sample42.xml)]

Há um problema conhecido que faz com que a autenticação de formulários para sempre redirecionar os usuários não autenticados para ~/Account/Login, ignorando a configuração de autenticação de formulários usada em Web. config. A solução alternativa é adicionar a seguinte configuração de aplicativo.

[!code-xml[Main](mvc3-release-notes/samples/sample43.xml)]

## <a id="0.1__Toc274034231"></a>  Isenção de responsabilidade

© 2011 Microsoft Corporation. Todos os direitos reservados. Este documento é fornecido "como-está." As informações e opiniões expressadas neste documento, incluindo URLs e outras referências a sites da Internet, podem ser alteradas sem aviso prévio. Você assume o risco de utilizá-las.

Este documento não lhe concede nenhum direito legal à nenhuma propriedade intelectual de nenhum produto da Microsoft. Você pode copiar e usar este documento para fins internos de referência.
