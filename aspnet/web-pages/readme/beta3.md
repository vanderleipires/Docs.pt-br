---
uid: web-pages/readme/beta3
title: Web Matrix e Leiame de versão Beta 3 do ASP.NET Web Pages (Razor) | Microsoft Docs
author: rick-anderson
description: Web Matrix e Leiame de versão Beta 3 do ASP.NET Web Pages (Razor)
ms.author: aspnetcontent
ms.date: 01/10/2011
ms.assetid: ffa3d5c9-91e5-4da3-b409-560b0c7fbbf0
msc.legacyurl: /web-pages/readme/beta3
msc.type: content
ms.openlocfilehash: 16b324e555b5450fc1e05c63e7e19985a2d02b89
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831826"
---
<a name="web-matrix-and-aspnet-web-pages-razor-beta-3-release-readme"></a>Web Matrix e Leiame de versão Beta 3 do ASP.NET Web Pages (Razor)
====================
> Web Matrix e Leiame de versão Beta 3 do ASP.NET Web Pages (Razor)

9 de novembro de 2010

## <a name="contents"></a>Conteúdo

- [Visão geral](#Overview)
- [Instalação](#Installation_Notes)
- [Novos recursos, alterações e problemas conhecidos na versão Beta 3](#Known_Issues)

    - [Problemas de instalação do WebMatrix](#Known_Issues_Installation)
    - [Páginas da Web do ASP.NET](#Known_Issues_ASPNET)
    - [SQL Server Compact](#Known_Issues_SQL_Server_Compact)
    - [Instalação de aplicativos](#Known_Issues_Installing_Applications)
    - [Publicação de aplicativos](#Known_Issues_Publishing_Applications)
    - [Outros problemas](#Known_Issues_Other_Issues)
- [Para obter mais informações](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>Visão geral

> Microsoft WebMatrix Beta é uma pilha de desenvolvimento da web gratuito que instala em minutos. Ele integra um servidor web com o banco de dados e estruturas para criar uma experiência única e integrada de programação. Você pode usar o WebMatrix Beta para simplificar a maneira de código, testar e publicar seu próprio site ASP.NET ou PHP, ou você pode usar a versão Beta do WebMatrix para iniciar um novo site usando aplicativos de software livre populares, como o Joomla, Umbraco, WordPress ou DotNetNuke. O WebMatrix Beta usa o mesmo servidor da web avançados, o mecanismo de banco de dados e o ambiente de estruturas que executará o seu site na internet, o que faz a transição do desenvolvimento para produção simples e direta.


<a id="Installation_Notes"></a>

## <a name="installation"></a>Instalação

> Para instalar o WebMatrix Beta 3, você deve usar [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638). Depois de instalar o Web Platform Installer, você pode usá-lo para instalar o WebMatrix Beta 3.
> 
> Se você tiver problemas durante a instalação, consulte [Solucionando problemas com o Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).


<a id="Installation_Notes0"></a>

## <a name="instructions-for-publishing-applications"></a>Instruções para a publicação de aplicativos

> Consulte [instruções passo a passo para a publicação de aplicativos](https://go.microsoft.com/fwlink/?LinkID=196149)


<a id="Known_Issues"></a>

## <a name="new-features-changes-andknown-issues"></a>Novos recursos, alterações, andKnown problemas

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-beta-3-installation"></a>Instalação do WebMatrix Beta 3

#### <a name="issue-webmatrix-beta-3-is-only-available-on-platforms-that-support-microsoft-net-framework-4"></a>Problema: O WebMatrix Beta 3 está disponível apenas em plataformas que dão suporte ao Microsoft .NET Framework 4

> O .NET Framework versão 4 é necessário para o WebMatrix Beta. Em alguns casos, o instalador do WebMatrix Beta serão permitem que você tente instalar em uma plataforma que não faz parte do conjunto de configurações com suporte. Em particular, Windows Vista sem a atualização do SP1 lhe permitirá começar a instalação da versão Beta do WebMatrix, mas o componente do .NET Framework 4 falhará e bloquear a instalação.
> 
> **Solução alternativa**  
> Instale em uma plataforma com suporte, que inclui:
> 
> - Windows 7
> - Windows Server 2008
> - Windows Server 2008 R2
> - Windows Vista SP1 ou posterior
> - Windows XP SP3
> - Windows Server 2003 SP2


#### <a name="issue-cannot-install-webmatrix-beta-3-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>Problema: Não é possível instalar o WebMatrix Beta 3, se o Microsoft Visual Studio 2008 é instalado sem o Microsoft Visual Studio 2008 SP1

> **Solução alternativa**  
> Instale [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) do Centro de Download da Microsoft.


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a>Problema: Alguns assemblies para o SQL Server Compact 4.0 não estão instalados no GAC

> Os assemblies gerenciados para o SQL Server Compact 4.0 não são colocados no cache de assembly global (GAC), quando você instala o SQL Server Compact 4.0 em um computador de 64 bits e o computador tem apenas o .NET Framework 3.5 SP1 Client Profile instalado. São os assemblies gerenciados que não estão instalados no GAC:
> 
> - *SqlServerCe* (provedor de ADO.NET)
> - *System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )
> 
> **Solução alternativa**  
> Desinstalar o SQL Server Compact 4.0. Baixe e instale a versão completa do .NET Framework 3.5 SP1 no seguinte local:  
>   
> [Microsoft .NET Framework 3.5 Service pack 1 (pacote completo)](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> Em seguida, reinstale o SQL Server Compact 4.0.


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a>Problema: Não é possível desinstalar o SQL Server Compact usando a linha de comando

> Desinstalação do SQL Server Compact usando as opções de linha de comando não funciona nesta versão.
> 
> **Solução alternativa**  
> Use *programas e recursos* no painel de controle do Windows para desinstalar o Microsoft SQL Server Compact 4.0.


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a>Páginas da Web do ASP.NET

Esta seção do documento descreve novos recursos, alterações e problemas conhecidos com a versão Beta 3 do ASP.NET Web Pages com sintaxe do Razor.

- [Novos recursos](#NewFeatures)
- [Alterações](#Changes)
- [Problemas](#Issues)

<a id="NewFeatures"></a>

#### <a name="new-features-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Novos recursos na versão Beta 3 páginas da Web ASP.NET com sintaxe do Razor

#### <a name="new-htmlraw-method-renders-unencoded-markup"></a>Novo: O método de "HTML. Raw" renderiza marcação sem codificação

> O novo `Html.Raw` método permite renderizar marcação HTML como marcação em vez de renderizar a saída codificada. (Por padrão, ASP.NET Razor codifica cadeias de caracteres antes de renderizá-los.) A sintaxe é:
> 
> `Html.Raw(value)`
> 
> O exemplo a seguir mostra como usar `Html.Raw`:
> 
> [!code-cshtml[Main](beta3/samples/sample1.cshtml)]


<a id="Changes"></a>

#### <a name="changes-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Alterações na versão Beta 3 páginas da Web ASP.NET com sintaxe do Razor

#### <a name="change-hrefattribute-method-removed"></a>Alterar: "HrefAttribute" método removido

> O `HrefAttribute` método da `WebPage` classe foi removida. Este auxiliar foi usado para codificar caracteres não seguros em URLs. Não é necessário porque o ASP.NET Razor automaticamente codifica cadeias de caracteres. (Use o novo `Html.Raw` método para renderizar cadeias de caracteres sem codificação.)


#### <a name="change-syntax-for-declarative-helper-helpers-changed"></a>Alteração: Sintaxe para declarativo "@helper" auxiliares alterados

> Na versão Beta 3, ASP.NET muda como ele analisa os auxiliares que são criados usando o `@helper` sintaxe. Em essência, o `@helper` sintaxe agora é analisado como um bloco de código em vez de como um bloco de marcação que pode incluir código. Portanto, o código dentro do auxiliar não precisa ser colocado entre `@{ }` blocos. Por outro lado, marcação dentro do auxiliar deve ser incluído explicitamente em elementos HTML ou em ASP.NET Razor `<text></text>` marcas.
> 
> Por exemplo, a seguinte `@helper` sintaxe funciona na versão Beta 3:
> 
> [!code-cshtml[Main](beta3/samples/sample2.cshtml)]
> 
> Na versão Beta 3, esse auxiliar deve ser alterada para se parecer com o exemplo a seguir:
> 
> [!code-cshtml[Main](beta3/samples/sample3.cshtml)]
> 
> Observe que o `@{ }` caracteres ao redor do código inicial no auxiliar não é mais usada. Isso ocorre porque o conteúdo dos auxiliares é tratado como um bloco de código por padrão. O auxiliar de marcação, que começa com a abertura de renderiza `<a>` marca. Se o auxiliar deve renderizar texto sem formatação ou marcas que não incluem uma marca de fechamento (por exemplo, `<meta>` marcas), o conteúdo a ser renderizado deve estar no `<text></text>` marcas.


#### <a name="change-webpagecontexthttpcontext-removed"></a>Alterações: "WebPageContext.HttpContext" removidas

> O `WebPageContext.HttpContext` propriedade foi removida. Use `HttpContext.Current` em seu lugar. (O `WebPageContext.HttpContext` propriedade simplesmente encapsulada isso.)


#### <a name="change-facebook-helper-moved-to-new-package"></a>Alteração: Auxiliar de "Facebook" movido para o novo pacote

> O `Facebook` auxiliar foi movido para o *Facebook.Helper* biblioteca, que inclui o `Facebook` auxiliar e funcionalidade adicional. Você deve instalar essa biblioteca como um pacote separado, conforme descrito em "Instalar auxiliares com o Gerenciador de pacotes" no tutorial [Introdução a páginas ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202889).


#### <a name="change-membership-role-and-security-types-moves-to-new-assembly"></a>Alteração: Tipos de associação, a função e a segurança move para o novo assembly

> Os seguintes tipos foram movidos para o `WebMatrix.WebData` assembly:
> 
> - `ExtendedMembershipProvider`
> - `SimpleMembershipProvider`
> - `SimpleRoleProvider`
> - `WebSecurity`


#### <a name="change-tagbuilder-class-moved-to-systemwebwebpagesdll-assembly"></a>Alteração: Classe de "TagBuilder" movido para o assembly System.Web.WebPages.dll

> O `TagBuilde` classe r foi movido para o assembly System.Web.WebPages.dll. Anteriormente, isso era em um assembly que fazia parte do ASP.NET MVC. Essa alteração significa que você não precisa instalar o ASP.NET MVC para usar o `TagBuilder` classe.
> 
> No entanto, a classe ainda está no `System.Web.Mvc` namespace. Para usar o `TagBuilder` classe (por exemplo, em um ASP.NET Razor auxiliar personalizado), você deve fazer referência ao namespace (por exemplo, adicionando `@using System.Web.Mvc` ao seu código).


#### <a name="change-request-validation-syntax-changed-validation-class-removed"></a>Alteração: Alterado; de sintaxe de validação de solicitação Classe de "Validação" removido

> Na versão Beta 3, para desabilitar a validação para um campo individual ou um conjunto de campos, você pode chamar o `Validation.Exclude` método, passando o nome ou os nomes dos campos a serem excluídos da validação. Uma nova sintaxe está disponível na versão Beta 3 para ignorar a validação. O `Validation` método usado na versão Beta 3 foi removido.
> 
> > [!NOTE]
> > Se você não desabilitar a validação de solicitação, se os usuários tentarem carregar uma marcação HTML (por exemplo, usando um editor de rich text em uma página), o site relatará um erro como *um valor Request. Form potencialmente perigoso foi detectado no cliente*e a entrada do usuário não é aceito. Se você desabilitar a validação de solicitação, você deve verificar manualmente a entrada do usuário para certificar-se de que ele não contém marcação potencialmente perigosa ou de script usando algo como o [Microsoft anti-Cross Site Scripting Library V4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).
> 
> 
> Para desabilitar a validação de solicitação automática, chame o `Request.Unvalidated` método, passando o nome do campo ou outro objeto de postagem que deseja ignorar a validação de solicitação para. Você pode usar esse método para ignorar a validação para todos os itens na `Form`, `QueryString`, `Cookies`, e `ServerVariables` coleções. Os exemplos a seguir mostram como usar o `Unvalidated` método:
> 
> [!code-csharp[Main](beta3/samples/sample4.cs)]


<a id="Issues"></a>

#### <a name="known-issues-for-aspnet-web-pages-with-razor-syntax"></a>Problemas conhecidos do ASP.NET Web Pages com sintaxe do Razor

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>Problema: Um comportamento inesperado ao usar uma tabela de usuário personalizada para a associação

> Para inicializar o provedor de associação para um site do ASP.NET Razor, você deve chamar o `WebSecurity.InitializeDatabaseConnection` método. (No WebMatrix, o modelo Site inicial inclui uma chamada para esse método na  *\_AppStart.cshtml* arquivo.) Se o `autoCreateTables` parâmetro desse método é definido como true (por padrão, ele é definido como true no modelo de Site inicial), e se um nome de tabela não reconhecido é passado para o método (o segundo parâmetro), o método não lança um erro. Em vez disso, ele automaticamente cria a tabela.
> 
> Isso pode ser um problema se você pretende usar uma tabela de usuário personalizada para a associação, mas passe o nome da tabela incorreta para o `WebSecurity.InitializeDatabaseConnection` método. Como o método por padrão gerará um erro se a tabela que você especificar não existir, e em vez disso, ele cria uma nova tabela, o aplicativo pode parecer estar funcionando. No entanto, o código do aplicativo que se baseia em sua tabela de usuário personalizada (e em campos nele), eventualmente, pode falhar com erros inesperados.
> 
> **Solução alternativa**  
> Certifique-se de que o nome passado a `InitializeDatabaseConnection` correspondências de método, o perfil do usuário no banco de dados de associação de tabela, ou certifique-se de que o `autoCreateTables` parâmetro é definido como false.


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>Erro de problema: "Falha ao gerar uma instância de usuário do SQL Server"

> Se um aplicativo Web do WebMatrix usa o SQL Server Express e está executando o IIS 7.5 no Windows 7 ou Windows Server 2008 R2, você poderá ver um erro que indica que o SQL Server não pode recuperar caminho do aplicativo local do usuário em tempo de execução.
> 
> **Solução alternativa** Certifique-se de que a conta do Windows que o aplicativo é executado (normalmente o serviço de rede) tem permissões de leitura/gravação para pastas raiz do aplicativo em subpastas, como *aplicativo\_dados*. Informações mais detalhadas estão disponíveis no artigo da Base de Conhecimento [problemas com a instância de usuário do SQL Server Express e o ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).


#### <a name="issue-in-visual-studio-namespaces-for-custom-assemblies-dlls-are-not-imported-automatically"></a>Problema: No Visual Studio, namespaces para assemblies personalizados (DLLs) não são importados automaticamente

> Se você usar assemblies personalizados em um projeto no Visual Studio, os namespaces declarados nesses assemblies não são importados automaticamente no tempo de design. Como resultado, as referências a tipos personalizados não podem ser reconhecidas em tempo de design e são marcadas como não reconhecido no Visual Studio (usando uma "linha ondulada"). Esse problema ocorre somente em tempo de design no Visual Studio; o aplicativo em si seja executado corretamente.
> 
> **Solução alternativa**  
> Incluir um `using` instrução (`imports` no Visual Basic) que faz referência as entidades que não são reconhecidas em tempo de design.


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>Problema: Visual Studio IntelliSense e o projeto de modelos disponíveis apenas no ASP.NET MVC versão 3

> Instalar páginas da Web ASP.NET não também instalará as ferramentas para Visual Studio como IntelliSense e modelos de projeto para aplicativos de páginas da Web ASP.NET.
> 
> **Solução alternativa** para usar o IntelliSense e modelos de projeto para aplicativos de páginas da Web ASP.NET no Visual Studio, instalar o ASP.NET MVC 3 RC por meio do Web Platform Installer ou o [instalador autônomo](https://go.microsoft.com/fwlink/?LinkID=191797).


#### <a name="issue-lthelpergt-class-cannot-be-found-error"></a>Problema: "&lt;auxiliar&gt; classe não pode ser encontrado" Erro

> Depois de atualizar a versão Beta 3, você poderá ver um erro que uma classe auxiliar (por exemplo, o `Facebook` classe) não pode ser encontrado. Começando na versão Beta 2 e continuando na versão Beta 3, os auxiliares foram movidos para pacotes que você deve instalar explicitamente. Sites existentes não são atualizados para incluir esses pacotes; Isso inclui sites na *\My Documents\IISExpress* ou *\My Sites da Web* pastas. Em particular, você verá esse erro se você usar o site padrão no *Meus Sites* (WebSite1), que inclui uma referência para o `Twitter` auxiliar.
> 
> **Solução alternativa**  
> Comente a chamadas para qualquer auxiliares no site, execute as  *\_Admin* da página e instalar o pacote ou pacotes que incluem os auxiliares que você deseja usar. Depois de instalar o pacote, você pode remover o comentário as linhas que fazem referência a auxiliares.


#### <a name="issue-deploying-beta-3-aspnet-razor-assemblies-to-the-bin-folder-might-not-work-on-hosting-sites"></a>Problema: Implantar assemblies do Beta 3 ASP.NET Razor para a pasta Bin pode não funcionar na hospedagem de sites

> Se você implantar um site de páginas da Web ASP.NET em um site de hospedagem, e se você implantar os assemblies do ASP.NET Razor Beta 3 para o site *Bin* pasta, você pode enfrentar erros, incluindo o seguinte:
> 
> `Could not load type 'Microsoft.Web.Infrastructure.DynamicModuleHelper.DynamicModuleUtility' from assembly 'Microsoft.Web.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
> 
> Isso pode acontecer se o provedor de hospedagem tiver instalado os assemblies do ASP.NET Web Pages Beta 1 no cache do servidor de aplicativo global (GAC). Assemblies no GAC têm precedência sobre os assemblies instalados localmente na *Bin* pasta.
> 
> **Solução alternativa** entre em contato com seu provedor de hospedagem para confirmar se os erros que você está vendo devido a um conflito entre versões do provedor de assemblies e o seu. Nesse caso, solicite que o provedor de hospedagem atualizem os assemblies no GAC do servidor.


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>Problema: Leitura de feeds ou outros dados externos por meio de um servidor proxy

> Se o servidor que executa o site estiver atrás de um servidor proxy, você talvez precise configurar informações de proxy nas *Web. config* arquivo para que seja possível ler informações provenientes de fora do site. Por exemplo, se você usar o `ReCaptcha` auxiliar, o auxiliar se comunica com o serviço reCAPTCHA, mas podem ser bloqueado pelo seu servidor proxy. Da mesma forma, os feeds que são usados no ASP.NET Web Pages, como o feed usado pelo Gerenciador de pacote, podem exigir configuração de proxy.
> 
> Se você tiver problemas em trabalhar com um serviço externo ou trabalhar com o feed de pacote, coloque os seguintes elementos raiz do seu aplicativo *Web. config* arquivo:
> 
> [!code-xml[Main](beta3/samples/sample5.xml)]
> 
> Para obter mais informações sobre como configurar um servidor proxy, consulte [ &lt;proxy&gt; (configurações de rede)](https://msdn.microsoft.com/library/sa91de1e.aspx) no site do MSDN.


#### <a name="issue-microsoftwebinfrastructuredll-cannot-be-loaded-error"></a>Problema: Erro "Não é possível carregar Microsoft.Web.Infrastructure.dll"

> Se você instalou a versão Beta 1 do ASP.NET Web Pages com sintaxe do Razor e, em seguida, instale a versão Beta 3, todos os assemblies apropriados estão instalados no GAC, exceto *Microsoft.Web.Infrastructure.dll*. Como consequência, quando você executa páginas Razor do ASP.NET, você verá um erro que indica que *Microsoft.Web.Infrastructure.dll* não pôde ser carregado.
> 
> Esse problema não ocorre se você tiver carregado a versão Beta 3 em um computador limpo.
> 
> **Solução alternativa**  
> No painel de controle, desinstale as páginas da Web ASP.NET. Em seguida, reinstale a versão Beta 3.


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>Problema: Desinstalar o .NET Framework versão 4 desabilita a ASP.NET Web Pages com sintaxe do Razor

> Se você desinstalar o .NET Framework versão 4 e, em seguida, reinstalá-lo, ASP.NET Web Pages com sintaxe do Razor estão desabilitados. As páginas com as *. cshtml* extensão não são executados corretamente. Páginas da Web ASP.NET registra um assembly na raiz da máquina *Web. config* arquivo e remover o .NET Framework remove esse arquivo. Reinstalar o .NET Framework instala uma nova versão do arquivo de configuração, mas não adiciona a referência para o assembly de páginas da Web ASP.NET.
> 
> **Solução alternativa** após reinstalar o .NET Framework, reinstale o ASP.NET Web Pages com sintaxe do Razor. Isso adiciona o elemento a seguir para o *Web. config* arquivo na raiz do computador, que normalmente é no seguinte local:  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> 
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](beta3/samples/sample6.xml)]


#### <a name="issue-applications-previously-deployed-with-aspnet-assemblies-in-the-bin-folder-experience-errors"></a>Problema: Os aplicativos implantados anteriormente com o ASP.NET assemblies na pasta Bin ocorrerem erros

> Durante a implantação, cópias dos assemblies páginas da Web ASP.NET (por exemplo, *Microsoft.WebPages.dll*) para o *Bin* pasta do site no servidor. (Isso pode ter acontecido automaticamente durante a implantação ou porque o desenvolvedor copiados explicitamente os assemblies.) No entanto, quando a versão Beta 3 está instalada, erros ocorre, como os erros que certos tipos não podem ser encontrados. Isso ocorre porque vários tipos de páginas da Web ASP.NET foram movidos para namespaces diferentes para a versão Beta 3.
> 
> **Solução alternativa**   
> Desmarque a *Bin* pasta do aplicativo implantado, copie os novos assemblies para a pasta (ou reimplantar o aplicativo) e, em seguida, reinicie o aplicativo.


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a>Problema: As URLs sem extensão não encontrar arquivos.cshtml/.vbhtml no IIS 7 ou IIS 7.5

> No IIS 7 ou IIS 7.5, solicitações com uma URL semelhante à seguinte não serão possível localizar as páginas que têm o *. cshtml* ou *. vbhtml* extensão:  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> O problema surge porque a regravação de URL não está habilitado por padrão para o IIS 7 ou IIS 7.5. O cenário mais provável é que você não vir o problema ao testar localmente usando o IIS Express, mas a experiência quando você implanta seu site em um site de hospedagem.
> 
> **Solução alternativa**
> 
> - Se você tiver controle sobre o computador do servidor, no computador do servidor instale a atualização descrita no [uma atualização está disponível que permite que certos manipuladores do IIS 7.0 ou IIS 7.5 para manipular as solicitações cujas URLs não terminam com um período](https://support.microsoft.com/kb/980368).
> - Se você não tem controle sobre o computador do servidor (por exemplo, você estiver implantando em um site de hospedagem), adicione o seguinte para o site *Web. config* arquivo:
> 
> 
> [!code-xml[Main](beta3/samples/sample7.xml)]


#### <a name="issue-using-web-application-project-or-aspnet-mvc-and-aspnet-web-pages-in-the-same-application"></a>Problema: Usando o projeto de aplicativo Web ou páginas ASP.NET MVC e Web ASP.NET no mesmo aplicativo

> Se você estivesse usando páginas da Web ASP.NET em um projeto de aplicativo Web ou um aplicativo ASP.NET MVC, você poderá ver um erro que *WebPageHttpApplication* não pode ser encontrado.
> 
> **Solução alternativa**  
> Se você receber esse erro, altere a classe base da qual deriva o aplicativo. No *global. asax* file, altere a linha a seguir:
> 
> [!code-csharp[Main](beta3/samples/sample8.cs)]
> 
> Para isso:
> 
> [!code-csharp[Main](beta3/samples/sample9.cs)]
> 
> Isso inverte a uma alteração que foi introduzida em vigor para a versão Beta 1 do ASP.NET Web Pages com sintaxe do Razor.


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>Problema: Implantando um aplicativo em um computador que não tenha o SQL Server Compact instalado

> Aplicativos que incluem bancos de dados do SQL Server Compact podem ser executados em um computador em que o SQL Server Compact não está instalado. Microsoft WebMatrix Beta 3 copia esses binários para você automaticamente e executa apropriado *Web. config* transformações de arquivo.
> 
> **Solução alternativa** se você precisar copiar esses arquivos e fazer a *Web. config* alterações de arquivo manualmente, faça o seguinte:
> 
> 1. Copie os assemblies do mecanismo de banco de dados para o *Bin* pasta (e as subpastas) do aplicativo no computador de destino: 
> 
>     - Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **to** *\Bin*
>     - Cópia *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\** **para** *\Bin\x86*
>     - Cópia *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\** **para** *\Bin\amd64*
> 2. Na pasta raiz do site, crie ou abra uma *Web. config* arquivo. (No WebMatrix Beta 3, esse tipo de arquivo estará disponível se você clicar **todos os** na **escolher um tipo de arquivo** caixa de diálogo.)
> 3. Adicione o seguinte elemento como um filho do **&lt;configuration&gt;** elemento (não dentro de **&lt;System. Web&gt;** elemento):
> 
> 
> [!code-xml[Main](beta3/samples/sample10.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>Problema: Os auxiliares de banco de dados e o WebGrid não funcionam no nível de confiança média no Visual Basic

> Se você estiver usando o Visual Basic (criando *. vbhtml* arquivos), o `Database` e `WebGrid` auxiliares não funcionará se o aplicativo está configurado para usar confiança média.
> 
> **Solução alternativa**  
> Defina temporariamente o aplicativo para usar a confiança total.

<a id="Known_Issues_SQL_Server_Compact"></a>
### <a name="sql-server-compact"></a>SQL Server Compact

#### <a name="issue-encrypt-property-is-not-recognized"></a>Problema: A propriedade "Encrypt" não é reconhecida

> SQL Server Compact 4.0 não reconhece o `Encrypt` propriedade do `SqlCeConnection` classe. Você não deve usar essa propriedade para criptografar arquivos de banco de dados. O `Encrypt` propriedade foi preterida na versão 3.5 compacto do SQL Server e foi mantida somente para compatibilidade com versões anteriores. 
> 
> **Solução alternativa**  
> Use o `Encryption Mode` propriedade do `SqlCeConnection` classe para criptografar arquivos de banco de dados do SQL Server Compact 4.0. O exemplo a seguir mostra como criar um banco de dados SQL Server Compact 4.0 criptografada usando o `Encryption Mode` propriedade:
> 
> [!code-csharp[Main](beta3/samples/sample11.cs)]
> 
> [!code-vb[Main](beta3/samples/sample12.vb)]
> 
> Para alterar o modo de criptografia de banco de dados SQL Server Compact 4.0 existente, faça o seguinte:
> 
> [!code-csharp[Main](beta3/samples/sample13.cs)]
> 
> [!code-vb[Main](beta3/samples/sample14.vb)]
> 
> Para criptografar um banco de dados do SQL Server Compact 4.0 não criptografado, faça o seguinte:
> 
> [!code-csharp[Main](beta3/samples/sample15.cs)]
> 
> [!code-vb[Main](beta3/samples/sample16.vb)]


#### <a name="issue-microsoft-visual-c-2008-runtime-libraries-are-required"></a>Problema: Bibliotecas de tempo de execução do Microsoft Visual C++ 2008 são necessárias

> O native DLLs do SQL Server Compact 4.0 é necessário o Microsoft Visual C++ 2008 bibliotecas de Runtime (x86, IA64 e x64), o Service Pack 1.
> 
> **Solução alternativa**  
> Instale o .NET Framework 3.5 SP1. Isso também instala o Visual C++ 2008 tempo de execução de bibliotecas do SP1. Você pode baixar as bibliotecas do seguinte local:   
>   
> [Atualização de segurança ATL do Microsoft Visual C++ 2008 Service Pack 1 Redistributable Package](https://go.microsoft.com/fwlink/?LinkId=194827)
> 
> [!NOTE]
> Observe que o .NET Framework 2.0, 3.0, a instalação ou 4 faz *não* instalar o SP1 de bibliotecas de tempo de execução do Visual C++ 2008.


#### <a name="issue-if-sql-server-compact-is-installed-prior-to-installing-net-framework-on-the-computer-its-provider-invariant-name-is-not-registered-in-the-net-framework-machineconfig-file"></a>Problema: Se o SQL Server Compact está instalado antes de instalar o .NET Framework no computador, seu nome invariável do provedor não está registrado no arquivo Machine. config do .NET Framework

> O SQL Server Compact pode ser instalado em um computador que não tenha o .NET Framework instalado, pois o SQL Server Compact requerem o .NET framework. Se nem o .NET Framework versão 3.5 nem 4 está instalado antes de instalar o SQL Server Compact, a instalação do SQL Server Compact não registra seu nome invariável do provedor na *Machine. config* arquivo. Qualquer aplicativo que se baseia na entrada do SQL Server Compact na *Machine. config* arquivo falhará. A entrada de registro de nome invariável na *Machine. config* se parece com o exemplo a seguir:
> 
> [!code-xml[Main](beta3/samples/sample17.xml)]
> 
> **Solução alternativa**  
> Desinstale o SQL Server Compact 4.0 CTP1. Baixe e instale as versões completas do .NET Framework do seguinte local:
> 
> [Microsoft .NET Framework 3.5 Service pack 1 (pacote completo)](https://go.microsoft.com/fwlink/?LinkId=194828)  
> [Versão do Microsoft .NET Framework 4.0 (pacote completo)](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en)
> 
> Em seguida, reinstalar [SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).


<a id="Known_Issues_Installing_Applications"></a>

### <a name="installing-applications"></a>Instalação de aplicativos

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>Problema: Instalação de um aplicativo pode levar muito tempo se a pasta Meus documentos do usuário é redirecionada para um compartilhamento de rede

> **Solução alternativa**  
> nenhuma. O aplicativo pode levar algum tempo para ser instalado, mas será instalado corretamente.


<a id="Known_Issues_Publishing_Applications"></a>

### <a name="publishing-applications"></a>Publicação de aplicativos

#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a>Problema: Os sites podem não funcionar após a publicação se o campo "URL de destino" não é prefixado com http:// ou https://

> No **configurações de publicação** caixa de diálogo, se a URL de destino não começa com `http://` ou `https://`, o site pode não funcionar após a implantação.
> 
> **Solução alternativa**  
> Certifique-se de que antes de publicar um site, a URL de destino na **configurações de publicação** caixa de diálogo começa com `http://` ou `https://`.


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a>Problema: Um banco de dados MySQL de publicação falha com o erro "Falha ao publicar o banco de dados. Isso pode acontecer se o banco de dados remoto não é possível executar o script."

> O erro pode ocorrer por vários motivos. Um motivo que você pode ver esse erro é se o script de banco de dados contém um caractere de aspas simples (') e o conjunto de caracteres de padrão de destino MySQL do banco de dados não está em UTF-8.
> 
> **Solução alternativa**  
> Defina o caractere padrão definido para o banco de dados remoto do MySQL como UTF-8.


<a id="Known_Issues_Other_Issues"></a>

### <a name="other-issues"></a>Outros Problemas

#### <a name="issue-searchfilter-does-not-work-in-reports-for-group-by-issue-type"></a>Problema: / Filtro de pesquisa não funciona em relatórios para agrupar por: tipo de problema

> Quando você executa um relatório para um site, se você inserir texto na *filtrar por URL* caixa e clique em *pesquisa*, nada acontece. Isso ocorre porque esse controle não está funcionando enquanto os *Group By* o estado do relatório é definido como *tipo de problema*, que é o padrão.
> 
> **Solução alternativa** no *Group By* guia da faixa de opções, clique em *URL* para agrupar as entradas por sua URL de origem. A caixa de texto e o botão para filtrar as entradas são funcionais enquanto nesse estado.


#### <a name="issue-wcf-applications-fail-to-run-with-iis-express"></a>Problema: Os aplicativos do WCF não funcionará com o IIS Express

> Navegando para um aplicativo WCF resulta em um erro semelhante ao seguinte:
> 
> *Não foi possível carregar arquivo ou assembly ' Administration, versão = 7.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35' ou uma de suas dependências. O sistema não pode localizar o arquivo especificado.*
> 
> Isso ocorre porque a versão do IIS Express Beta não dá suporte a WCF por padrão.
> 
> **Solução alternativa** usar qualquer uma das seguintes alternativas (solução alternativa 2 # requer o Microsoft Windows Vista ou superior):
> 
> 
> 1. Cópia de *Microsoft.Web.dll* e *Administration* assemblies do local de instalação do WebMatrix para o *bin* do WCF aplicativo. Por padrão, o WebMatrix é instalado na *o Microsoft WebMatrix* subpasta do sistema *arquivos de programas* pasta.
> 2. No Microsoft Windows Vista ou versões posteriores, crie um symlink para os assemblies na *bin* usando os seguintes comandos do diretório. (Essa abordagem tem a vantagem que ele não cria uma cópia dos assemblies.)
> 
>     [!code-console[Main](beta3/samples/sample18.cmd)]
> 3. Instale os dois assemblies no GAC. Em um prompt com privilégios elevados, execute os seguintes comandos:
> 
>     [!code-console[Main](beta3/samples/sample19.cmd)]


#### <a name="issue-webmatrix-beta-3-is-unable-to-perform-certain-tasks-that-require-elevation"></a>Problema: O WebMatrix Beta 3 é capaz de realizar certas tarefas que exigem elevação

> O WebMatrix Beta 3 é capaz de realizar certas tarefas que exigem elevação de privilégio, como a instalação de componentes adicionais nas seguintes situações:
> 
> - No Windows Vista ou Windows 7, você está conectado com uma conta que não tem privilégios administrativos e controle de conta de usuário (UAC) está desabilitado.
> - Você está usando o Microsoft Windows XP ou Microsoft Windows Server 2003.
> 
> **Solução alternativa**  
> A maioria das tarefas no WebMatrix Beta 3 não exigem permissão administrativa. Para aqueles que fazer, você pode executar a operação como um administrador ou siga estas etapas:
> 
> - No Windows Vista ou Windows 7, habilite o UAC.
> - No Windows XP, adicione o usuário ao grupo de segurança Administradores.


#### <a name="issue-site-from-web-gallery-is-disabled"></a>Problema: "Site da Galeria de Web" está desabilitada

> O **Site da Galeria de Web** opção será desabilitada se o Web Platform Installer 3.0 não está instalado.
> 
> **Solução alternativa**  
> Instalar o [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).


#### <a name="issue-on-windows-server-2003-iis-express-does-not-start-for-a-non-administrative-user"></a>Problema: No Windows Server 2003, o IIS Express não é iniciado para um usuário não administrativo

> No Windows Server 2003, quando você iniciar uma página ou iniciar o IIS Express, o IIS Express não é iniciado. Páginas da Web, um erro será exibido que indica se o aplicativo foi iniciado por um usuário não administrativo.
> 
> **Solução alternativa**  
> Inicie o WebMatrix Beta 3 como um usuário administrativo. Para obter mais detalhes, consulte o seguinte artigo da Base de Conhecimento:  
>   
> [Um aplicativo que é iniciado por um usuário não administrativo não pode ouvir o tráfego HTTP do computador no qual o aplicativo é executado no Windows Vista, Windows Server 2003 ou Windows XP.](https://support.microsoft.com/kb/939786)


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a>Problema: O Google Chrome não está disponível como uma opção de execução

> Google Chrome não é exibido na lista de navegadores em **executados** sobre o **Home** guia.
> 
> **Solução alternativa**  
> Algumas versões do Google Chrome não se registram corretamente com o recurso de programas padrão no Windows. Como alternativa, inicie o Google Chrome, clique o *personalizar e controle Google Chrome* menu, clique em *opções*e, em seguida, clique em *Make Google Chrome meu navegador padrão*.


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a>Problema: A caixa de diálogo "Foreign Key" não permite inserir uma chave primária

> O **Foreign Key** caixa de diálogo não permite que você insira o nome da chave primário da tabela de chave primária.
> 
> **Solução alternativa**  
> Isso é intencional. Você não precisará inserir o nome da chave primária da tabela de chave primária.


#### <a name="issue-the-relationships-button-is-disabled"></a>Problema: O botão "Relações" está desabilitado

> O **relacionamentos** botão sob o **tabela** guia o **bancos de dados** espaço de trabalho está desabilitado para bancos de dados do SQL Server Compact.
> 
> **Solução alternativa**  
> nenhuma. O SQL Server Compact não oferece suporte a relações entre tabelas.


#### <a name="issue-parameterized-sql-queries-throw-exceptions"></a>Problema: Consultas SQL parametrizadas lançam exceções

> No SQL Server Compact 4.0, se você não especificar um tipo de dados, como `SqlDbType` ou `DbType` para parâmetros em consultas parametrizadas, uma exceção é lançada quando a consulta é executada.
> 
> **Solução alternativa**  
> Definir explicitamente o tipo de dados para parâmetros, como `SqlDbType` ou `DbType`. Isso é importante no caso de tipos de dados BLOB (`image` e `ntext`). Use o código semelhante ao seguinte:
> 
> [!code-sql[Main](beta3/samples/sample20.sql)]
> 
> [!code-vb[Main](beta3/samples/sample21.vb)]


<a id="More_Info"></a>

## <a name="for-more-information"></a>Para obter mais informações

Para obter mais informações sobre o WebMatrix Beta 3, consulte os seguintes sites:

- [IIS.net](http://iis.net/)
- [ASP.NET](https://asp.net/webmatrix)
- [Microsoft.com/web](https://www.microsoft.com/web)

* * *

© 2010 Microsoft Corporation. Todos os direitos reservados. [Termos de uso](https://msdn.microsoft.cos/cc300389.aspx).
