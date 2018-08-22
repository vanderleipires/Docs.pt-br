---
uid: web-pages/readme/overview
title: Leiame do WebMatrix | Microsoft Docs
author: rick-anderson
description: O WebMatrix e o Leiame do ASP.NET Web Pages (Razor) versão 1.0
ms.author: riande
ms.date: 01/06/2011
ms.assetid: 36c5beeb-45a7-48a0-9c30-f82cdf5c5f5f
msc.legacyurl: /web-pages/readme
msc.type: content
ms.openlocfilehash: aa852e7bbd93622154d59e0d0a13ffa680812df2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834518"
---
<a name="webmatrix-readme"></a>Leiame do WebMatrix
====================
13 de janeiro de 2011

## <a name="contents"></a>Conteúdo

> [!NOTE]
> Este arquivo Leiame aplica-se a 1.0 versão do WebMatrix.


- [Visão geral](#Overview)
- [Instalação](#Installation_Notes)
- [Como publicar aplicativos](#InstructionsForPublishingApplications)
- [As alterações e questões](#ChangesAndIssues)

    - [Instalação do WebMatrix 1.0](#Known_Issues_Installation)
    - [Páginas da Web do ASP.NET](#Known_Issues_ASPNET)
    - [WebMatrix](#Known_Issues_WebMatrix)
    - [IIS Express](#Known_Issues_IISExpress)
    - [SQL Server Compact](#Known_Issues_SQLServerCompact)
    - [Instalação de aplicativos](#Known_Issues_Installing_Applications)
    - [Publicação de aplicativos](#Known_Issues_Publishing_Applications)
- [Para obter mais informações](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>Visão geral

> Microsoft WebMatrix 1.0 é uma pilha de desenvolvimento da web gratuito que instala em minutos. Ele integra um servidor web com o banco de dados e estruturas para criar uma experiência única e integrada de programação. Você pode usar o WebMatrix para simplificar a maneira de código, testar e publicar seu próprio site ASP.NET ou PHP, ou você pode usar o WebMatrix para iniciar um novo site usando aplicativos de software livre populares, como o Joomla, Umbraco, WordPress ou DotNetNuke. O WebMatrix usa o mesmo servidor da web avançados, o mecanismo de banco de dados e o ambiente de estruturas que executará o seu site na internet, o que faz a transição do desenvolvimento para produção simples e direta.


<a id="Installation_Notes"></a>

## <a name="installation"></a>Instalação

> Para instalar o WebMatrix 1.0, você deve primeiro instalar o [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638). Depois de instalar o Web Platform Installer, você pode usá-lo para instalar o WebMatrix.
> 
> Se você tiver problemas durante a instalação, consulte [Solucionando problemas com o Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).


<a id="InstructionsForPublishingApplications"></a>
## <a name="how-to-publish-applications"></a>Como publicar aplicativos

> Consulte [instruções passo a passo para a publicação de aplicativos](https://go.microsoft.com/fwlink/?LinkID=196149)


<a id="ChangesAndIssues"></a>

## <a name="changes-and-issues"></a>As alterações e questões

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-10-installation-issues"></a>Problemas de instalação 1.0 do WebMatrix

#### <a name="issue-webmatrix-10-is-available-only-on-platforms-that-support-microsoft-net-framework-4"></a>Problema: O WebMatrix 1.0 está disponível somente nas plataformas que dão suporte ao Microsoft .NET Framework 4

> O .NET Framework versão 4 é necessário para o WebMatrix. Em alguns casos, o instalador do WebMatrix 1.0 será permitem que você tente instalar em uma plataforma que não faz parte do conjunto de configurações com suporte. Em particular, Windows Vista sem a atualização do SP1 lhe permitirá começar a instalação do WebMatrix, mas o componente do .NET Framework 4 falhará e bloquear a instalação.
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


#### <a name="issue-cannot-install-webmatrix-10-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>Problema: Não é possível instalar o WebMatrix 1.0, se o Microsoft Visual Studio 2008 é instalado sem o Microsoft Visual Studio 2008 SP1

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

Esta seção do documento descreve novos recursos, alterações e problemas conhecidos com a 1.0 versão do ASP.NET Web Pages com sintaxe do Razor.

- [Novos recursos](#NewFeatures)
- [Alterações](#Changes)
- [Problemas](#Issues)

#### <a id="NewFeatures"></a>  Novos recursos

#### <a name="new-configuration-setting-added-to-disable-the-package-manager"></a>Novo: A definição de configuração adicionada para desabilitar o Gerenciador de pacotes

> Uma nova `asp:AdminManagerEnabled` chave está disponível para o `<appSettings>` elemento o *Web. config* arquivo, que permite que você desabilite completamente o Gerenciador de pacotes. O valor padrão para esse elemento for true, o que significa que, se ele não está incluído na *Web. config* arquivo, o Gerenciador de pacotes está habilitado. Para desabilitar o Gerenciador de pacotes, adicione o seguinte elemento para o *Web. config* arquivo na raiz do site:
> 
> [!code-xml[Main](overview/samples/sample1.xml)]


#### <a id="Changes"></a>  Alterações

#### <a name="change-webpagesadminfoldervirtualpath-key-renamed-to-aspadminfoldervirtualpath"></a>Alterar: chave de "webPages:AdminFolderVirtualPath" renomeado para "asp: AdminFolderVirtualPath"

> O `webPages:AdminFolderVirtualPath` chave que pode ser adicionado ao *Web. config* arquivo para especificar o local do Gerenciador de pacotes foi renomeado para usar os `asp:` namespace em vez do `webPages` namespace. Se você tiver usado esse elemento, você deverá renomeá-la no arquivo de configuração.


#### <a id="Issues"></a>  Problemas conhecidos

#### <a name="issue-passwords-for-membership-users-no-longer-recognized"></a>Problema: Senhas para usuários de associação não reconhecidos

> O algoritmo para criar e armazenar as senhas de participação (logon) foi alterado para ser mais seguro. Como resultado, as senhas armazenadas para membros (usuários) criados em versões Beta do ASP.NET Razor não serão reconhecidas. 
> 
> **Solução alternativa** se o site ainda não foi colocado em produção, remova os registros de usuário do banco de dados de associação. Se o banco de dados em tempo real, por meio de programação regenere as senhas existentes no banco de dados de associação.


#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>Problema: Um comportamento inesperado ao usar uma tabela de usuário personalizada para a associação

> Para inicializar o provedor de associação para um site do ASP.NET Razor, você deve chamar o `WebSecurity.InitializeDatabaseConnection` método. (No WebMatrix, o modelo Site inicial inclui uma chamada para esse método na  *\_AppStart.cshtml* arquivo.) Se o `autoCreateTables` parâmetro desse método é definido como true (por padrão, ele é definido como true no modelo de Site inicial), e se um nome de tabela não reconhecido é passado para o método (o segundo parâmetro), o método não lança um erro. Em vez disso, ele automaticamente cria a tabela.
> 
> Isso pode ser um problema se você pretende usar uma tabela de usuário personalizada para a associação, mas passe o nome da tabela incorreta para o `WebSecurity.InitializeDatabaseConnection` método. Como o método por padrão gerará um erro se a tabela que você especificar não existir, e em vez disso, ele cria uma nova tabela, o aplicativo pode parecer estar funcionando. No entanto, o código do aplicativo que se baseia em sua tabela de usuário personalizada (e em campos nele), eventualmente, pode falhar com erros inesperados.
> 
> **Solução alternativa**  
> Certifique-se de que o nome passado a `InitializeDatabaseConnection` correspondências de método, o perfil do usuário no banco de dados de associação de tabela, ou certifique-se de que o `autoCreateTables` parâmetro é definido como false.


#### <a name="issue-error-message-the-admin-module-requires-access-to-appdata"></a>Problema: Mensagem de erro "o módulo de administração requer acesso ao ~/App\_dados"

> Em algumas circunstâncias, tentando criar usuários ou trabalhar com o sistema de associação do ASP.NET pode fazer com que a página para exibir o erro *o módulo de administração requer acesso ao ~/App\_dados*. Isso ocorre se a conta que o IIS ou IIS Express é executado em não tem permissões para criar e gravar o *App\_dados* pasta sob a raiz do site. 
> 
> **Solução alternativa** criar manualmente uma *App\_dados* pasta para o site. Em seguida, certifique-se de que a conta do Windows que o aplicativo é executado (normalmente o serviço de rede) tem permissões de leitura/gravação para pastas raiz do aplicativo em subpastas, como aplicativo\_dados. Informações mais detalhadas estão disponíveis no artigo da Base de Conhecimento [problemas com a instância de usuário do SQL Server Express e o ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>Erro de problema: "Falha ao gerar uma instância de usuário do SQL Server"

> Se um aplicativo Web do WebMatrix usa o SQL Server Express e está executando o IIS 7.5 no Windows 7 ou Windows Server 2008 R2, você poderá ver um erro que indica que o SQL Server não pode recuperar caminho do aplicativo local do usuário em tempo de execução.
> 
> **Solução alternativa** Certifique-se de que a conta do Windows que o aplicativo é executado (normalmente o serviço de rede) tem permissões de leitura/gravação para pastas raiz do aplicativo em subpastas, como *aplicativo\_dados*. Informações mais detalhadas estão disponíveis no artigo da Base de Conhecimento [problemas com a instância de usuário do SQL Server Express e o ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).


#### <a name="issue-files-that-contains-package-manager-resources-or-package-manager-passwords-are-servable-under-iis-60-and-earlier"></a>Problema: Os arquivos que contém recursos do Gerenciador de pacotes ou as senhas do Gerenciador de pacote são fornecíveis no IIS 6.0 e versões anteriores

> Se você implantar um aplicativo ASP.NET Web Pages (Razor) que foi criado usando a versão RC2, e se o aplicativo contém um *Password* ou *packagesources.txt* arquivo sob */App\_ Administrador dedados/*, IIS 6.0 servirá o arquivo se solicitado, potencialmente expor as senhas para sua instância do Gerenciador de pacote. 
> 
> **Solução alternativa** renomear os *Password* ou *packagesources.txt* arquivo *password.config* ou *packagesources.config*. Por padrão, o IIS 6.0 não servirá os arquivos que têm o *. config* extensão. (No IIS 7, nenhum arquivo o *App\_dados* pasta são atendidos, portanto você não precisa renomear os arquivos.)


#### <a name="issue-uninstalling-packages-installed-using-the-beta-3-release-does-not-completely-remove-package-components"></a>Problema: Desinstalar os pacotes instalados usando a versão Beta 3 não remover completamente os componentes de pacote

> Se você instalou um pacote usando o Gerenciador de pacotes na versão Beta 3 e, em seguida, tente desinstalá-lo usando a versão atual, o pacote não foi completamente desinstalado. Usando o Gerenciador de pacotes **Uninstall** botão remove alguns componentes, mas deixa o código da biblioteca do pacote e não atualiza o *Package* arquivo.
> 
> **Solução alternativa**   
> Execute estas etapas:  
> 1. Excluir o *App\_Data\packages* pasta. Isso remove todos os pacotes.   
> 2. Excluir o *Packages. config* arquivo na raiz do site.


#### <a name="issue-in-visual-studio-invoking-the-web-based-package-manager-takes-the-application-offline"></a>Problema: No Visual Studio, invocar o Gerenciador de pacotes baseado na web leva para o aplicativo offline

> Se você estiver trabalhando no Visual Studio (não o WebMatrix) e usar o  *\_admin* funcionalidade para iniciar o Gerenciador de pacotes, o Visual Studio coloca o aplicativo offline e posta os *aplicativo\_ offline.htm* para a raiz do site, que interrompe a capacidade de usar o Gerenciador de pacotes.
> 
> [!NOTE]
> Embora você geralmente veria esse comportamento ao usar a interface do Gerenciador de pacotes baseado na web, o mesmo comportamento ocorre se você adiciona, remove ou modificar em todos os arquivos a *App\_dados* pasta.
> 
> **Solução alternativa**   
> Para trabalhar com pacotes no Visual Studio, use a extensão do NuGet em vez do Gerenciador de pacotes baseado na web. Para obter informações, consulte o [documentação do NuGet](https://docs.microsoft.com/nuget/). Se você estiver trabalhando com outros arquivos na *App\_dados* pasta, considere manter os arquivos em outro lugar para evitar esse problema. Se não for prático, exclua o *app\_offline.htm* arquivo manualmente ou esperar até que o site fica online automaticamente (por padrão, após 30 segundos).


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>Problema: Visual Studio IntelliSense e o projeto de modelos disponíveis apenas no ASP.NET MVC versão 3

> Instalar páginas da Web ASP.NET não também instalará as ferramentas para Visual Studio como IntelliSense e modelos de projeto para aplicativos de páginas da Web ASP.NET.
> 
> **Solução alternativa** para usar o IntelliSense e modelos de projeto para aplicativos de páginas da Web ASP.NET no Visual Studio, instalar o ASP.NET MVC 3 RC por meio do Web Platform Installer ou o [instalador autônomo](https://go.microsoft.com/fwlink/?LinkID=191797).


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>Problema: Leitura de feeds ou outros dados externos por meio de um servidor proxy

> Se o servidor que executa o site estiver atrás de um servidor proxy, você talvez precise configurar informações de proxy nas *Web. config* arquivo para que seja possível ler informações provenientes de fora do site. Por exemplo, se você usar o `ReCaptcha` auxiliar, o auxiliar se comunica com o serviço reCAPTCHA, mas podem ser bloqueado pelo seu servidor proxy. Da mesma forma, os feeds que são usados no ASP.NET Web Pages, como o feed usado pelo Gerenciador de pacote, podem exigir configuração de proxy.
> 
> Se você tiver problemas em trabalhar com um serviço externo ou trabalhar com o feed de pacote, coloque os seguintes elementos raiz do seu aplicativo *Web. config* arquivo:
> 
> [!code-xml[Main](overview/samples/sample2.xml)]
> 
> Para obter mais informações sobre como configurar um servidor proxy, consulte [ &lt;proxy&gt; (configurações de rede)](https://msdn.microsoft.com/library/sa91de1e.aspx) no site do MSDN.


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>Problema: Desinstalar o .NET Framework versão 4 desabilita a ASP.NET Web Pages com sintaxe do Razor

> Se você desinstalar o .NET Framework versão 4 e, em seguida, reinstalá-lo, ASP.NET Web Pages com sintaxe do Razor estão desabilitados. As páginas com as *. cshtml* extensão não são executados corretamente. Páginas da Web ASP.NET registra um assembly na raiz da máquina *Web. config* arquivo e remover o .NET Framework remove esse arquivo. Reinstalar o .NET Framework instala uma nova versão do arquivo de configuração, mas não adiciona a referência para o assembly de páginas da Web ASP.NET.
> 
> **Solução alternativa** após reinstalar o .NET Framework, reinstale o ASP.NET Web Pages com sintaxe do Razor. Isso adiciona o elemento a seguir para o *Web. config* arquivo na raiz do computador, que normalmente é no seguinte local:  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](overview/samples/sample3.xml)]


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
>     [!code-xml[Main](overview/samples/sample4.xml)]


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>Problema: Implantando um aplicativo em um computador que não tenha o SQL Server Compact instalado

> Aplicativos que incluem bancos de dados do SQL Server Compact podem ser executados em um computador em que o SQL Server Compact não está instalado. Microsoft WebMatrix 1.0 copia esses binários para você automaticamente e executa apropriado *Web. config* transformações de arquivo.
> 
> **Solução alternativa** se você precisar copiar esses arquivos e fazer a *Web. config* alterações de arquivo manualmente, faça o seguinte:
> 
> 1. Copie os assemblies do mecanismo de banco de dados para o *Bin* pasta (e as subpastas) do aplicativo no computador de destino:  
> 
>    - Cópia *C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll*   
>        **para** *\Bin.*
>    - Cópia <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\</em><strong><em>para</em></strong>\Bin\x86*
>    - Cópia <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\</em>* <strong>para</strong><em>\Bin\amd64</em>
> 
> 2. Na pasta raiz do site, crie ou abra uma *Web. config* arquivo. (No WebMatrix 1.0, esse tipo de arquivo está disponível se você clicar **todos os** na **escolher um tipo de arquivo** caixa de diálogo.)
> 3. Adicione o seguinte elemento como um filho de `<configuration>` elemento (não dentro a `<system.web>` elemento):
> 
>     [!code-xml[Main](overview/samples/sample5.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>Problema: "Database" e "WebGrid" auxiliares não funcionam no nível de confiança média no Visual Basic

> Se você estiver usando o Visual Basic (criando *. vbhtml* arquivos), o `Database` e `WebGrid` auxiliares não funcionará se o aplicativo está configurado para usar confiança média.
> 
> **Solução alternativa**  
> Se você usar o Visual Studio 2010, você pode resolver esse problema ao instalar a versão de Service Pack 1. Até que a versão final da versão do SP1 está disponível, você pode baixar a versão Beta do SP1 a partir de [Microsoft Visual Studio 2010 Service Pack 1 Beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) página no Microsoft Download Center.   
>   
> Se isso não é prático, ou se você não usar o Visual Studio 2010, você pode temporariamente, defina o aplicativo para usar a confiança total.


#### <a name="issue-applicationpart-resources-are-externally-accessible"></a>Problema: "ApplicationPart" recursos sejam acessíveis externamente

> Se um assembly contém objetos que deriva de `ApplicationPart` classe, que os recursos do assembly são expostos pelo `ResourceRouteHandler` classe. Por exemplo, considere a seguinte URL:  
>   
> `~/r.ashx/System.Web.WebPages.Administration/Resources/AdminResources.resources`  
>   
> Essa solicitação baixa todas as cadeias de caracteres de recurso nas *System.Web.WebPages.Administration.dll* assembly. Todos os recursos incorporados (mesmo aqueles que não se destina a ser atendido como conteúdo estático) são baixados. Se os recursos incorporados contiverem informações confidenciais, isso pode representar um risco à segurança. 
> 
> **Solução alternativa**   
> Se você criar uma **ApplicationPart** do objeto, certifique-se de que os recursos incorporados associados a esse **ApplicationPart** assembly do objeto não contêm informações confidenciais.


<a id="Known_Issues_WebMatrix"></a>

### <a name="webmatrix"></a>WebMatrix

> [!NOTE]
> Para obter informações sobre problemas de instalação para o WebMatrix, consulte [problemas de instalação do WebMatrix](#Known_Issues_Installation) anteriormente neste documento.


Esta seção do documento descreve problemas conhecidos para o ambiente de desenvolvimento do WebMatrix.

#### <a name="issue-changes-in-the-username-or-password-of-a-database-connection-string-in-a-webconfig-file-are-not-reflected-in-the-databases-workspace"></a>Problema: As alterações no nome de usuário ou senha de uma cadeia de caracteres de conexão de banco de dados em um arquivo Web. config não são refletidas no espaço de trabalho de bancos de dados

> **Solução alternativa**  
> 
> 1. No *Web. config* file, altere o nome do banco de dados na cadeia de conexão (por exemplo, adicione "1" a ele).
> 2. Salvar a *Web. config* arquivo.
> 3. Clique em **bancos de dados** e atualizar.
> 4. Altere o nome do banco de dados na cadeia de conexão a *Web. config* arquivo de volta para o nome do banco de dados original.
> 5. Salvar a *Web. config* arquivo.
> 6. Clique em **bancos de dados** e atualizar.


#### <a name="issue-folders-created-by-webmatrix-cannot-be-deleted"></a>Problema: Pastas criadas pelo WebMatrix não podem ser excluídas

> Se o WebMatrix está em execução usando permissões elevadas (ou seja, você iniciou o WebMatrix usando o **executar como administrador** opção no Windows), as pastas criadas pelo WebMatrix não podem ser excluídas usando o Windows Explorer.
> 
> **Solução alternativa**  
> Execute o Explorer do Windows usando permissões elevadas. Siga estas etapas:  
> 
> 1. No Windows, clique em **iniciar**.
> 2. Insira "Windows Explorer" e a entrada para o botão direito do mouse **Windows Explorer**.
> 3. Clique em **executar como administrador**. Em seguida, você pode excluir as pastas.


#### <a name="issue-webmatrix-10-is-unable-to-perform-certain-tasks-that-require-elevation"></a>Problema: O WebMatrix 1.0 não consegue realizar certas tarefas que exigem elevação

> O WebMatrix 1.0 não consegue realizar certas tarefas que exigem elevação de privilégio, como a instalação de componentes adicionais nas seguintes situações:
> 
> - No Windows Vista ou Windows 7, você está conectado com uma conta que não tem privilégios administrativos e controle de conta de usuário (UAC) está desabilitado.
> - Você está usando o Microsoft Windows XP ou Microsoft Windows Server 2003.
> 
> **Solução alternativa**  
> A maioria das tarefas no WebMatrix 1.0 não exigem permissão administrativa. Para aqueles que fazer, você pode executar a operação como um administrador ou siga estas etapas:
> 
> - No Windows Vista ou Windows 7, habilite o UAC.
> - No Windows XP, adicione o usuário ao grupo de segurança Administradores.


#### <a name="issue-site-from-web-gallery-is-disabled"></a>Problema: "Site da Galeria de Web" está desabilitada

> O **Site da Galeria de Web** opção será desabilitada se o Web Platform Installer 3.0 não está instalado.
> 
> **Solução alternativa**  
> Instalar o [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).


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


#### <a name="issue-intellisense-is-not-available-in-webmatrix-for-razor-syntax-c-or-visual-basic"></a>Problema: O IntelliSense não está disponível no WebMatrix para Razor sintaxe, c# ou Visual Basic

> Há suporte para o IntelliSense para HTML e CSS no WebMatrix. No entanto, ele não está disponível para outros idiomas. 
> 
> **Solução alternativa**   
> nenhuma.


#### <a name="issue-intellisense-for-html-and-css-suggests-elements-that-are-not-contextually-appropriate"></a>Problema: O IntelliSense para HTML e CSS sugere elementos que não são contextualmente apropriados

> IntelliSense para marcação no WebMatrix dá suporte a HTML usando o [esquema XHTML 1.0 Transitional](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) e CSS usando o [esquema CSS 2.1](http://www.w3.org/TR/CSS2/). Como o IntelliSense se baseia nesses esquemas específicos, determinadas marcas, atributos ou propriedades talvez sugeridas que não são apropriados para a definição de estilo ou de página atual. Para HTML, ele também pode levar a sugestões inesperados no conteúdo que pode ser interpretada como XHTML malformada (por exemplo, quando as marcas não estão fechadas). Esse problema pode ser mais perceptível se o ponto de inserção está dentro de uma marca incompleta; Nesse caso, IntelliSense pode sugerir novos marcas de abertura ou oferecer outras sugestões incorretos. 
> 
> **Solução alternativa**   
> Para HTML, certifique-se de que você está trabalhando em uma página XHTML bem formada e completa. Para o CSS, não há nenhuma solução alternativa.


#### <a name="issue-intellisense-is-not-invoked-while-you-type"></a>Problema: O IntelliSense não é chamado enquanto você digita

> Às vezes, IntelliSense não pode ser invocado como HTML ou CSS está sendo inserido no editor. Em particular, isso pode acontecer quando o ponto de inserção está diretamente ao lado do outro elemento ou no final de um arquivo. 
> 
> **Solução alternativa**   
> Certifique-se de que há espaço em branco em torno do ponto de inserção e o ponto de inserção não está no final de um arquivo. Você também pode invocar IntelliSense manualmente, pressionando Ctrl + espaço.


#### <a name="issue-no-ui-is-available-for-disabling-intellisense"></a>Problema: Nenhuma interface do usuário está disponível para desabilitar o IntelliSense

> O WebMatrix 1.0 não fornece nenhuma interface do usuário ou gesto para desativar o IntelliSense. 
> 
> **Solução alternativa**   
> Inicie o WebMatrix usando o comando a seguir, que inclui uma opção que desativa o IntelliSense:  
>   
> `WebMatrix.exe #ExecuteCommand# EditorIntelliSense off`


<a id="Known_Issues_IISExpress"></a>
### <a name="iis-express"></a>IIS Express

O IIS Express tem seu próprio arquivo Leiame, que está disponível na seguinte URL:

[https://go.microsoft.com/fwlink/?LinkID=207675&amp; clcid = 0x409](https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409)

<a id="Known_Issues_SQLServerCompact"></a>

### <a name="sql-server-compact"></a>SQL Server Compact

O SQL Server Compact tem seu próprio arquivo Leiame, que está disponível na seguinte URL:

[https://go.microsoft.com/fwlink/?LinkID=208545](https://go.microsoft.com/fwlink/?LinkID=208545&amp;clcid=0x409)

Para obter informações sobre problemas que envolvem a instalação do SQL Server Compact como parte do WebMatrix, consulte [problemas de instalação do WebMatrix](#Known_Issues_Installation) anteriormente neste documento.

### <a id="Known_Issues_Installing_Applications"></a>  Instalação de aplicativos

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>Problema: Instalação de um aplicativo pode levar muito tempo se a pasta Meus documentos do usuário é redirecionada para um compartilhamento de rede

> **Solução alternativa**  
> nenhuma. O aplicativo pode levar algum tempo para ser instalado, mas será instalado corretamente.


### <a id="Known_Issues_Publishing_Applications"></a>  Publicação de aplicativos

#### <a name="issue-required-permissions-cannot-be-acquired-error-when-publishing-a-sql-compact-database"></a>Problema: "Required não não possível adquirir as permissões" Erro ao publicar um banco de dados do SQL Compact

> O WebMatrix não suporta totalmente implantar binários do suporte para SQL Server Compact em um servidor que executa o .NET Framework versão 3.5 com uma configuração de confiança média.
> 
> **Solução alternativa**  
> É a solução preferencial instalar o .NET Framework 4 no servidor. Como alternativa, faça o seguinte:
> 
> 1. Adicione os seguintes elementos para o `SecurityClasses` seção *Web\_MediumTrust.config* arquivo:
> 
>     [!code-html[Main](overview/samples/sample6.html)]
> 2. Criar um novo conjunto de permissões na *Web\_MediumTrust.config* arquivo com as permissões necessárias a seguir:
> 
>     [!code-html[Main](overview/samples/sample7.html)]
> 3. Aplicar o conjunto de permissões para o SQL Server Compact, colocando os seguintes elementos *Web\_MediumTrust.config* arquivo:
> 
>     [!code-html[Main](overview/samples/sample8.html)]


#### <a name="issue-gallery-and-phpbb-web-applications-display-a-service-is-unavailable-error-after-publishing"></a>Problema: Aplicativos da web da galeria e PhpBB exibem um erro "O serviço não está disponível" após a publicação

> Em algumas circunstâncias, a publicação de um aplicativo faz com que um erro "serviço indisponível".
> 
> **Solução alternativa**  
> No WebMatrix, adicionar uma barra invertida (\) ao final do nome do servidor do **configurações de publicação** janela e, em seguida, publicar o aplicativo novamente.


#### <a name="issue-moodle-website-layout-and-links-are-broken-after-publishing"></a>Problema: Layout de site do Moodle e links são desfeitos após a publicação

> Depois de publicar um aplicativo Moodle, o aplicativo não funciona corretamente.
> 
> **Solução alternativa**  
> No WebMatrix, adicione uma barra (/) ao final do **nome do Site** campo o **configurações de publicação** janela e, em seguida, publicar o aplicativo novamente.


#### <a name="issue-publishing-nopcommerce-fails-with-a-database-error"></a>Problema: Publicação nopCommerce falha com um erro de banco de dados

> Publicação nopCommerce falhar e relata um erro de banco de dados, como "inserir o nop\_tabela de log falhou."
> 
> **Solução alternativa**  
> 
> 1. No WebMatrix, clique em **executar** para iniciar o nopCommerce localmente.
> 2. Faça logon na página de administração.
> 3. Clique o **sistema** menu.
> 4. Clique o **Log** opção.
> 5. Clique o **Limpar Log** botão.
> 6. Publique o nopCommerce novamente.


#### <a name="issue-silverstripe-cms-displays-a-http-500-php-fcgi-error-when-you-download-a-published-site"></a>Problema: O Silverstripe CMS exibe um "Erro HTTP 500 PHP FCGI" quando você baixa um site publicado

> **Solução alternativa**  
> Depois de clicar em **site publicado do Download**, ignore `silverstripe-cache/manifest_main` na **Publicar visualização**. Esse arquivo é usado para fins de cache e é específico para cada computador.


#### <a name="issue-subtext-displays-server-error-in--application-when-you-download-a-published-site"></a>Problema: O subtexto exibe "Erro de servidor no aplicativo '/'" quando você baixa um site publicado

> **Solução alternativa**  
> Abra o site *Web. config* de arquivo e substitua a ID de usuário e senha na cadeia de conexão de banco de dados com as credenciais de administrador do SQL Server (as credenciais de "sa").
> 
> Como alternativa, siga estas etapas para conceder a conta de usuário que você efetuou logon com `db_owner` permissões:
> 
> 1. Instale o SQL Server Management Studio usando o Web Platform Installer.
> 2. Conectar-se à instância local do SQL Server Express (por padrão, `.\SQLEXPRESS`).
> 3. Clique em **bancos de dados** &gt; *[localSubtextDatabase]* &gt; **segurança** &gt; **usuários** &gt; *[localSubtextUser*] (o padrão é `subtextuser`], mouse e clique em **propriedades**.
> 4. Selecione **db\_proprietário** na seção de associação de função.


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


#### <a name="issue-some-links-are-not-visible-in-dotnetnuke-after-publishing-or-downloading-the-site"></a>Problema: Alguns links não são visíveis no DotNetNuke depois de publicar ou baixando o site

> Se você publica ou baixar um site DotNetNuke, você talvez precise limpar o cache para obter novos links sejam exibidos no site.
> 
> **Solução alternativa**
> 
> 1. Faça logon como "Host".
> 2. Vá para o menu de host e selecione **configurações de Host**.
> 3. Role para baixo e, sob **configurações avançadas**, expanda **configurações de desempenho**.
> 4. Clique o **Limpar Cache** link para as páginas.
> 5. Vá até a parte inferior da página e reiniciar o aplicativo.


#### <a name="issue-some-links-in-atomsite-are-broken-after-you-download-a-published-site"></a>Problema: Alguns links AtomSite são desfeitos após o download de um site publicado

> **Solução alternativa**  
> No *service.config* arquivo, *users.config* arquivo e todos os *. XML* arquivos, substitua a cadeia de caracteres de URL (por exemplo, `http://myhost.com/atomsite`) com um local (por exemplo, `http://localhost:1239`).


#### <a name="issue-mysql-based-applications-like-wordpress-fail-to-publish-and-report-a-database-error"></a>Problema: Aplicativos baseados em MySQL como WordPress falharem publicar e relatar um erro de banco de dados

> Por padrão, o WebMatrix instala MySQL com o conjunto de caracteres UTF-8. Se você instalar o MySQL por conta própria, e o conjunto de caracteres não é UTF-8 (por exemplo, é Latin1), o processo de publicação para bancos de dados poderá falhar.
> 
> **Solução alternativa**
> 
> 1. Altere o conjunto de caracteres para MySQL para UTF-8. (Para obter detalhes, consulte [servidor do conjunto de caracteres e agrupamento](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) no site do MySQL.)
> 2. Reinstale o aplicativo.
> 3. Republique o aplicativo.


#### <a name="issue-download-published-site-fails-for-applications-that-have-browser-based-setup"></a>Problema: "Download publicados site" falhar para aplicativos que têm a instalação baseada em navegador

> Alguns aplicativos (por exemplo, o Kentico CMS) exigem que você iniciá-los no navegador para executar a instalação após a instalação, como a criação de um banco de dados. Se você publicar um aplicativo como este, sem concluir a instalação baseada em navegador, a tentativa de baixar o mesmo site de um servidor remoto falhará.
> 
> **Solução alternativa**  
> Conclua a instalação baseada em navegador antes de publicar o site.


#### <a name="issue-download-published-site-fails-with-a-database-error-for-dotnetnuke-and-kooboo-cms"></a>Problema: "Download publicados site" falhará com um erro de banco de dados para o DotNetNuke e Kooboo CMS

> Se você tentar baixar um aplicativo de um servidor e ter credenciais de administrador na cadeia de conexão de banco de dados na **configurações de publicação** caixa de diálogo, você poderá ver o seguinte erro no log de publicação:
> 
> [!code-console[Main](overview/samples/sample9.cmd)]
> 
> **Solução alternativa**  
> Se for prático, republique o site (ou publicá-lo) usando as credenciais de não administrador para o banco de dados.


<a id="More_Info"></a>

## <a name="for-more-information"></a>Para obter mais informações

Para obter mais informações sobre o WebMatrix 1.0, consulte os seguintes sites:

- [IIS.net](http://iis.net/)
- [ASP.NET](https://asp.net/webmatrix)
- [Microsoft.com/web](https://www.microsoft.com/web)

© 2011 Microsoft Corporation. Todos os direitos reservados. [Termos de uso](https://msdn.microsoft.cos/cc300389.aspx).
