---
uid: whitepapers/overview
title: White papers | Microsoft Docs
author: rick-anderson
description: Nesta página você encontrará white papers para ajudá-lo a instalar e configurar o ASP.NET e para ajudar a escrever aplicativos do ASP.NET seguros, rápidos e flexíveis.
ms.author: riande
ms.date: 11/15/2011
ms.assetid: d5e79470-01f2-4d65-8077-11c3e10a6784
msc.legacyurl: /whitepapers
msc.type: content
ms.openlocfilehash: 5017efc4d141afba206aaca5a8b5e6bab996ebbf
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830499"
---
<a name="whitepapers"></a>Whitepapers
====================
> Nesta página você encontrará white papers para ajudá-lo a instalar e configurar o ASP.NET e para ajudar a escrever aplicativos do ASP.NET seguros, rápidos e flexíveis.
> 
> - [ASP.NET 4](#aspnet4)
> - [White papers de segurança do ASP.NET](#security)
> - [Instalação e white papers do programa de instalação](#setup)
> - [White papers do SQL Server](#sql)
> - [White papers de geral](#general)


<a id="aspnet4"></a>
## <a name="aspnet-4"></a>ASP.NET 4

Informações relacionadas ao ASP.NET 4 e Visual Studio 2010.

[Notas de versão do ASP.NET MVC 4](mvc4-beta-release-notes.md "mvc4 notas de versão")

Este documento descreve novos recursos e aprimoramentos introduzidos na visualização do desenvolvedor do ASP.NET MVC 4 para Visual Studio 2010, bem como notas de instalação e problemas conhecidos.

[Notas de versão do ASP.NET MVC 3](mvc3-release-notes.md "mvc3 notas de versão")

Este documento descreve novos recursos e aprimoramentos introduzidos no ASP.NET MVC 3, bem como notas de instalação e problemas conhecidos.

[ASP.NET 4 e visão geral de desenvolvimento Visual Studio 2010 Web](aspnet4/index.md "aspnet4")

Muitas alterações interessantes para o ASP.NET estão sendo disponibilizados no .NET Framework versão 4. Este documento fornece uma visão geral de muitos dos novos recursos que estão incluídos na próxima versão.

[Alterações do ASP.NET 4 Beta 2 quebra](aspnet4/breaking-changes.md "alterações significativas")

Este documento descreve as alterações que foram feitas para a versão do .NET Framework 4 Beta 2 versão (ou seja, a versão Beta 2 do ASP.NET 4) que pode afetar aplicativos que foram criados usando versões anteriores, incluindo a versão Beta 1 do ASP.NET 4.

[O que há de novo no ASP.NET MVC 2](what-is-new-in-aspnet-mvc.md "o que há de novo no ASP.NET mvc")

Este documento descreve novos recursos e aprimoramentos introduzidos no ASP.NET MVC 2.

[Atualizando um aplicativo ASP.NET MVC 1.0 para o ASP.NET MVC 2](aspnet-mvc2-upgrade-notes.md "aspnet-mvc2 – notas de atualização")

ASP.NET MVC 2 pode ser instalado lado a lado com o ASP.NET MVC 1.0 no mesmo servidor. Isso fornece flexibilidade de desenvolvedores de aplicativo na escolha quando atualizar um aplicativo ASP.NET MVC 1.0 para o ASP.NET MVC 2. Este descreve documento ambas as como atualizar manualmente e com um assistente no Visual...

<a id="security"></a>
## <a name="aspnet-security-whitepapers"></a>White papers de segurança do ASP.NET

A segurança é um aspecto importante de aplicativos da internet, e esses white papers discutem como projetar e implementar aplicativos ASP.NET seguros.

[O ASP.NET 2.0 de instrumentar aplicativos para segurança](https://msdn.microsoft.com/library/ms998325.aspx)

Este artigo "como" mostra como usar eventos de monitoramento de integridade personalizadas para instrumentar seu aplicativo ASP.NET para acompanhar eventos relacionados à segurança e operações. Versão do ASP.NET 2.0 fornece integridade de monitoramento, que inclui a instrumentação para muitos padrão...

[Executar uma revisão de implantação de segurança para o ASP.NET 2.0](https://msdn.microsoft.com/library/ms998367.aspx)

Este "como" mostra de que como executar uma revisão de segurança de implantação para um aplicativo ASP.NET 2.0 identificar possíveis vulnerabilidades de segurança introduzidas por configurações inadequadas. A maior parte do processo de revisão envolve a tomada de...

[Uso do ADAM para funções no ASP.NET 2.0](https://msdn.microsoft.com/library/ms998331.aspx)

Este tópico como fazer mostra como você pode desenvolver um site ASP.NET que usa o aplicativo de modo de ADAM (Active Directory) para armazenar as funções do ASP.NET. Ele mostra como configurar o ADAM e o repositório de políticas do Gerenciador de autorização (AzMan), como criar novas funções e...

[Usar o Gerenciador de autorização (AzMan) com o ASP.NET 2.0](https://msdn.microsoft.com/library/ms998336.aspx)

Este tópico como fazer mostra como usar o Gerenciador de autorização (AzMan) em conjunto com o Gerenciador de funções API para gerenciar funções, verificar a associação de função de usuário e autorizar funções do ASP.NET para executar operações específicas em relação a um armazenamento de diretivas de AzMan. O como...

[Usar a associação do ASP.NET 2.0](https://msdn.microsoft.com/library/ms998347.aspx)

Este "como" mostra de que forma usar o recurso de associação em aplicativos do ASP.NET versão 2.0. Ele mostra como usar dois provedores de associação diferentes: o ActiveDirectoryMembershipProvider e o SqlMembershipProvider. O recurso de associação...

[Usar o Gerenciador de funções no ASP.NET 2.0](https://msdn.microsoft.com/library/ms998314.aspx)

Este artigo "como" mostra como usar o Gerenciador de funções do ASP.NET 2.0. O Gerenciador de funções facilita a tarefa de funções de gerenciamento e execução de autorização baseado em função em seu aplicativo. Ele mostra como configurar os vários provedores de função para uso com o seu...

[Usar a autenticação do Windows no ASP.NET 2.0](https://msdn.microsoft.com/library/ms998358.aspx)

Este artigo "como" mostra como configurar e usar a autenticação do Windows em um aplicativo Web ASP.NET. Autenticação do Windows é a abordagem preferida, sempre que os usuários fazem parte do seu domínio do Windows. Essa abordagem permite que você use um repositório de identidades existente...

[Executar uma revisão de código de segurança para código gerenciado (atividade de linha de base)](https://msdn.microsoft.com/library/ms998364.aspx)

Este "como" mostra de que como realizar revisões de código de segurança. Este módulo apresenta as etapas envolvidas na atividade e técnicas para analisar os resultados. Use este como com "lista de perguntas de segurança: (.NET Framework 2.0) de código gerenciado"...

[Executar uma revisão de implantação de segurança para o ASP.NET 2.0](https://msdn.microsoft.com/library/ms998367.aspx)

Este "como" mostra de que como executar uma revisão de segurança de implantação para um aplicativo ASP.NET 2.0 identificar possíveis vulnerabilidades de segurança introduzidas por configurações inadequadas. A maior parte do processo de revisão envolve a tomada de...

[Implementar a delegação Kerberos para Windows 2000](https://msdn.microsoft.com/library/aa302400.aspx)

A delegação de Kerberos permite que você fluxo uma identidade autenticada em várias camadas físicas de um aplicativo para dar suporte à autorização e autenticação de downstream. Como este mostra a você as etapas de configuração necessárias para fazer isso funcionar.

[Use a representação e delegação no ASP.NET 2.0](https://msdn.microsoft.com/library/ms998351.aspx)

Este tópico como fazer mostra como e quando você deve usar a representação em aplicativos ASP.NET 2.0. Por padrão, representação está desativada e você pode acessar recursos usando a identidade do processo do aplicativo Web do ASP.NET. No entanto, você pode usar...

[Criar um modelo de risco para um aplicativo Web em tempo de Design](https://msdn.microsoft.com/library/ms978527.aspx)

Este "como" descreve de que uma abordagem para a criação de um modelo de risco para um aplicativo Web. A atividade de modelagem de ameaças ajudam você a seu design de segurança de modelo para que você pode expor vulnerabilidades e possíveis falhas de design de segurança antes de você investir...

### <a name="forms-authentication"></a>Autenticação de formulários

[Proteger a autenticação de formulários no ASP.NET 2.0](https://msdn.microsoft.com/library/ms998310.aspx)

Este artigo "como" mostra como configurar e usar a autenticação de formulários com aplicativos ASP.NET 2.0 com segurança. Principais fatores a serem considerados incluem corretamente protegendo o tíquete de autenticação e protegendo o repositório de identidades de usuário e o acesso ao repositório. ...

[Usar autenticação de formulários com o Active Directory no ASP.NET 2.0](https://msdn.microsoft.com/library/ms998360.aspx)

Este "como" mostra de que como usar a autenticação de formulários com o serviço de diretório do Microsoft® Active Directory® usando o ActiveDirectoryMembershipProvider. Como mostra como configurar o provedor e criar e autenticar usuários...

[Usar autenticação de formulários com o Active Directory em vários domínios no ASP.NET 2.0](https://msdn.microsoft.com/library/ms998345.aspx)

Este "como" mostra de que como usar a autenticação de formulários com o serviço de diretório do Microsoft® Active Directory® usando o ActiveDirectoryMembershipProvider. Como mostra como configurar o provedor e criar e autenticar usuários...

[Usar autenticação de formulários com o SQL Server no ASP.NET 2.0](https://msdn.microsoft.com/library/ms998317.aspx)

Este tópico como fazer mostra como você pode usar a autenticação de formulários com o provedor de associação do SQL Server. Autenticação de formulários com o SQL Server é mais aplicável em situações em que os usuários do seu aplicativo não fazem parte do seu domínio do Windows, e como resultado,...

[Criar objetos GenericPrincipal usando a autenticação de formulários no ASP.NET 1.1](https://msdn.microsoft.com/library/aa302399.aspx)

Este artigo "como" mostra como criar e manipular objetos GenericPrincipal e FormsIdentity ao usar a autenticação de formulários.

[Usar autenticação de formulários com o Active Directory no ASP.NET 1.1](https://msdn.microsoft.com/library/aa302397.aspx)

Este artigo mostra como implementar a autenticação de formulários em relação a um armazenamento de credenciais do Active Directory.

[Usar autenticação de formulários com o SQL Server no ASP.NET 1.1](https://msdn.microsoft.com/library/aa302398.aspx)

Este artigo "como" mostra como implementar a autenticação de formulários em relação a um repositório de credenciais do SQL Server. Ele também mostra como armazenar resumos de senha no banco de dados.

### <a name="user-input-data-validation"></a>Validação de dados de entrada do usuário

[Solicitação de validação – impedindo ataques de Script](request-validation.md "validação de solicitação")

Este documento descreve o recurso de validação de solicitação do ASP.NET, onde, por padrão, o aplicativo será impedido de processamento de conteúdo HTML não codificado enviado ao servidor. Esse recurso de validação de solicitação pode ser desabilitado quando o aplicativo foi...

[Evitar scripts entre sites no ASP.NET](https://msdn.microsoft.com/library/ms998274.aspx)

Este "como" mostra de que como você pode ajudar a proteger seus aplicativos ASP.NET contra ataques de script entre sites por meio de técnicas de validação de entrada apropriada e pela codificação de saída. Ele também descreve alguns dos outros mecanismos de proteção que você pode usar em...

[Proteger contra injeção de SQL no ASP.NET](https://msdn.microsoft.com/library/ms998271.aspx)

Este "como" mostra de que um número de maneiras para ajudar a proteger seu aplicativo ASP.NET contra ataques de injeção de SQL. Injeção de SQL pode ocorrer quando um aplicativo usa a entrada para construir instruções SQL dinâmicas ou quando ele usa procedimentos armazenados para conectar-se para o...

[Usar expressões regulares para restringir entradas em ASP.NET](https://msdn.microsoft.com/library/ms998267.aspx)

Este "como" mostra de que como você pode usar expressões regulares em aplicativos ASP.NET para restringir entradas não confiáveis. Expressões regulares são uma boa maneira de validar os campos de texto, como nomes, endereços, números de telefone e outras informações do usuário. Você pode usar...

### <a name="code-access-security"></a>Segurança de acesso do código

[Use a segurança de acesso do código no ASP.NET 2.0](https://msdn.microsoft.com/library/ms998326.aspx)

Este tópico como fazer mostra como selecionar um nível de confiança apropriado para seu aplicativo e, quando necessário, como criar um personalizado ASP.NET código acesso arquivo diretiva de segurança para definir um personalizado nível de confiança. Você pode usar a confiança de segurança de acesso de código diferente...

[Criar uma permissão de criptografia personalizada](https://msdn.microsoft.com/library/aa302362.aspx)

Este "como" descreve de que como criar uma permissão de segurança de acesso do código personalizado para controlar o acesso programático à funcionalidade de criptografia não gerenciada que fornece Win32® Data Protection API (DPAPI). Use essa permissão personalizada com o wrapper gerenciado do DPAPI...

[Use a política de segurança de acesso do código para restringir um Assembly](https://msdn.microsoft.com/library/aa302361.aspx)

Um administrador pode configurar a política de segurança de acesso do código para restringir as operações de código do .NET Framework (assemblies). Este tópico como fazer, você configurar política de segurança de acesso do código para restringir a capacidade de um assembly para executar a e/s de arquivo e restringir...

### <a name="communications-security"></a>Segurança de comunicações

[Configurar SSL em um servidor Web](https://msdn.microsoft.com/library/aa302411.aspx)

Um servidor Web deve ser configurado para SSL para dar suporte a conexões https de aplicativos cliente. Este "como" mostra de que forma configurar SSL em um servidor Web.

[Configurar certificados de cliente](https://msdn.microsoft.com/library/aa302412.aspx)

IIS dá suporte à autenticação de certificado de cliente. Este artigo "como" mostra como configurar um aplicativo Web para exigir certificados de cliente. Ele também mostra como instalar um certificado em um computador cliente e usá-lo ao chamar o aplicativo Web.

[Usar IPSec na filtragem de portas e autenticação](https://msdn.microsoft.com/library/aa302366.aspx)

Internet Protocol security (IPSec) é um protocolo, não um serviço, que fornece serviços de autenticação para tráfego de rede baseado em IP, integridade e criptografia. Como o IPSec fornece proteção de servidor para servidor, você pode usar IPSec para combater ameaças internas...

[Usar IPSec para oferecer comunicação segura entre dois servidores](https://msdn.microsoft.com/library/aa302413.aspx)

O IPSec é uma tecnologia fornecida pelo Windows 2000 que permite que você crie canais criptografados entre dois servidores. IPSec pode ser usado para filtrar o tráfego de IP e para autenticar servidores. Este "como" mostra de que forma configurar IPSec para fornecer seguro (criptografado)...

[Use o SSL para proteger a comunicação com o SQL Server](https://msdn.microsoft.com/library/aa302414.aspx)

Costuma ser fundamental para aplicativos que são capazes de proteger os dados passados de e para um servidor de banco de dados do SQL Server. Com o SQL Server, você pode usar o SSL para criar um canal criptografado. Este artigo "como" mostra como instalar um certificado no servidor de banco de dados,...

[Chamar um Web Service usando certificados de cliente do ASP.NET 1.1](https://msdn.microsoft.com/library/aa302408.aspx)

Este "como" descreve de que como você pode passar um certificado de cliente para um serviço Web para autenticação de um aplicativo Web ASP.NET ou de um aplicativo do Windows Forms. Você pode instalar o certificado do cliente no repositório do computador local ou no armazenamento do usuário. If...

[Chamar um serviço Web usando o SSL do ASP.NET 1.1](https://msdn.microsoft.com/library/aa302409.aspx)

A criptografia Secure Sockets Layer (SSL) pode ser usada para garantir a integridade e a confidencialidade das mensagens transmitidas para e de um serviço Web. Este "como" mostra de que como usar SSL com os serviços Web.

### <a name="cryptography"></a>Criptografia

[Criar uma biblioteca DPAPI no .NET 1.1](https://msdn.microsoft.com/library/aa302402.aspx)

Este artigo "como" mostra como criar uma biblioteca de classes gerenciadas que expõe a funcionalidade DPAPI a aplicativos que desejam criptografar dados, por exemplo, cadeias de conexão de banco de dados e as credenciais da conta.

[Criar uma biblioteca de criptografia no .NET 1.1](https://msdn.microsoft.com/library/aa302405.aspx)

Este artigo "como" mostra como criar uma biblioteca de classes gerenciada para oferecer funcionalidade de criptografia para aplicativos. Ele permite que um aplicativo escolher o algoritmo de criptografia. Algoritmos com suporte incluem DES, Triple DES, RC2 e Rijndael.

[Uma cadeia de caracteres de Conexão criptografada no registro no ASP.NET 1.1 da Store](https://msdn.microsoft.com/library/aa302406.aspx)

Aplicativos podem optar por armazenar os dados criptografados, como cadeias de caracteres de conexão e credenciais de conta no registro do Windows. Este artigo "como" mostra como armazenar e recuperar cadeias de caracteres criptografadas no registro.

[Usar DPAPI (Store máquina) no ASP.NET 1.1](https://msdn.microsoft.com/library/aa302403.aspx)

Este "como" mostra de que forma usar DPAPI de um aplicativo Web ASP.NET ou Web service para criptografar dados confidenciais.

[Usar DPAPI (Store do usuário) no ASP.NET 1.1 com os serviços corporativos](https://msdn.microsoft.com/library/aa302404.aspx)

Este "como" mostra de que forma usar DPAPI de um aplicativo Web ASP.NET ou serviço para criptografar dados confidenciais. Este tópico como fazer usa a DPAPI com o armazenamento de usuário, que requer o uso de um fora do processo de componente de serviços corporativos.

<a id="setup"></a>
## <a name="installation-and-setup-whitepapers"></a>Instalação e white papers do programa de instalação

Esses white papers fornecem instruções passo a passo para instalar e configurar o ASP.NET no servidor.

[Criar uma conta de serviço para um ASP.NET 2.0 aplicativo](https://msdn.microsoft.com/library/ms998297.aspx)

Este artigo "como" mostra como criar e configurar uma conta de serviço personalizado de com menos privilégios para executar um aplicativo Web ASP.NET. Por padrão, um aplicativo ASP.NET no Microsoft Windows Server 2003 e o IIS 6.0 é executado usando o serviço de rede interno...

[Melhorar a segurança ao hospedar vários aplicativos em ASP.NET 2.0](https://msdn.microsoft.com/library/aa480478.aspx)

Este como mostra como você pode isolar vários aplicativos uns dos outros e de recursos compartilhados do sistema em uma Web ambiente de hospedagem. O ambiente de hospedagem pode ser um servidor Web fornecido pelo provedor de um serviço de Internet (ISP) que hospeda várias...

[Usar confiança média no ASP.NET 2.0](https://msdn.microsoft.com/library/ms998341.aspx)

Este artigo "como" mostra como configurar aplicativos Web do ASP.NET para ser executado em confiança média. Se você hospedar vários aplicativos no mesmo servidor, você pode usar a segurança de acesso do código e o nível de confiança médio para fornecer isolamento de aplicativos. Configurando...

[Usar a conta de serviço de rede para acessar recursos no ASP.NET 2.0](https://msdn.microsoft.com/library/ms998320.aspx)

Este tópico como fazer mostra como você pode usar a conta NT AUTHORITY\Network Service da máquina para acessar o local e recursos de rede. Por padrão no Windows Server 2003, os aplicativos ASP.NET são executados usando a identidade desta conta. Ele é uma menos privilegiada...

[Usar a transição de protocolo e delegação restrita no ASP.NET 2.0](https://msdn.microsoft.com/library/ms998355.aspx)

Este artigo "como" mostra como configurar e usar a transição de protocolo e delegação restrita para permitir que seu aplicativo ASP.NET para acessar os recursos de rede ao representar o chamador original. Sistema de operacional do Microsoft® Windows® 2000...

[Execução do ASP.NET lado a lado do .NET Framework 1.0 e 1.1](side-by-side-with-10.md "lado a lado com 1.0")

Este white paper descreve como instalar o .NET 1.0 e 1.1 do .NET em seu computador, permitindo que um aplicativo Web ASP.NET seja executado em qualquer versão do framework.

[Acesso negado do ASP.NET a diretórios do IIS](denied-access-to-iis-directories.md "negado acesso a diretórios do iis")

Este white paper descreve o que você deve fazer uma solicitação para seu aplicativo ASP.NET retorna o erro "acesso negado ao *Nomedodiretório* directory. Falha ao iniciar o monitoramento de alterações de diretório".

[Executando o ASP.NET 1.1 com o IIS 6.0](aspnet-and-iis6.md "aspnet e iis6")

Embora o Windows Server 2003 inclui o IIS 6.0 e ASP.NET 1.1, esses componentes são desabilitados por padrão. Este white paper descreve como habilitar o IIS 6.0 e ASP.NET 1.1 e recomenda várias definições de configuração para obter o melhor...

[Correção para o erro de ''servidor aplicativo não está disponível após a aplicação de atualização de segurança para o IE](ms03-32-issue.md "ms03-32-problema")

Este documento descreve o patch que corrige um problema com a atualização de segurança MS03-32 para o Internet Explorer que afeta os aplicativos do ASP.NET 1.0 em execução no Windows XP Professional.

[Criar uma conta personalizada para executar o ASP.NET 1.1](https://msdn.microsoft.com/library/aa302396.aspx)

Aplicativos Web ASP.NET normalmente executar usando a conta ASPNET interna. Às vezes, você talvez queira usar uma conta personalizada em vez disso. Este artigo mostra como criar uma conta local com menos privilégios para executar aplicativos da Web do ASP.NET. ...

### <a name="configuration"></a>Configuração

[Configurar a chave do computador no ASP.NET 2.0](https://msdn.microsoft.com/library/ms998288.aspx)

Esse manual explica a &lt;machineKey&gt; elemento no arquivo Web. config e mostra como configurar o &lt;machineKey&gt; elemento a verificação de adulteração de controle e a criptografia de ViewState, a autenticação de formulários permissões e os cookies de função. ViewState é assinado...

[Criptografar seções de configuração no ASP.NET 2.0 usando DPAPI](https://msdn.microsoft.com/library/ms998280.aspx)

Este tópico como fazer mostra como usar o provedor de configuração protegida DPAPI (interface) e o Aspnet de programação de aplicativo de proteção de dados do Windows\_ferramenta regiis.exe para criptografar seções de arquivos de configuração. Você pode usar o Aspnet\_ferramenta regiis.exe para...

[Criptografar seções de configuração no ASP.NET 2.0 usando o RSA](https://msdn.microsoft.com/library/ms998283.aspx)

Este tópico como fazer mostra como usar o provedor de configuração protegida do RSA e o Aspnet\_ferramenta regiis.exe para criptografar seções de arquivos de configuração. Você pode usar Aspnet\_ferramenta regiis.exe para criptografar dados confidenciais, como cadeias de caracteres de conexão, mantidos na...

[Use a representação e delegação no ASP.NET 2.0](https://msdn.microsoft.com/library/ms998351.aspx)

Este tópico como fazer mostra como e quando você deve usar a representação em aplicativos ASP.NET 2.0. Por padrão, representação está desativada e você pode acessar recursos usando a identidade do processo do aplicativo Web do ASP.NET. No entanto, você pode usar...

<a id="sql"></a>
## <a name="sql-server-whitepapers"></a>White papers do SQL Server

Enquanto o ASP.NET funciona com uma variedade de bancos de dados, estes white papers examinar especificamente a conexão de aplicativos do ASP.NET para o SQL Server.

[Conectar-se ao SQL Server usando a autenticação do SQL no ASP.NET 2.0](https://msdn.microsoft.com/library/ms998300.aspx)

Este artigo "como" mostra como se conectar a um aplicativo ASP.NET com segurança para Microsoft® SQL Server™ quando a autenticação de acesso de banco de dados usa a autenticação do SQL native. A autenticação do Windows é a maneira recomendada para se conectar ao SQL Server porque você...

[Conectar-se ao SQL Server usando a autenticação do Windows no ASP.NET 2.0](https://msdn.microsoft.com/library/ms998292.aspx)

Este tópico como fazer mostra como se conectar ao SQL Server 2000 usando uma conta de serviço do Windows de um aplicativo do ASP.NET versão 2.0. Você deve usar a autenticação do Windows em vez da autenticação do SQL sempre que possível, porque você evitar armazenar credenciais em...

[Use o SSL para proteger a comunicação com o SQL Server 2000](https://msdn.microsoft.com/library/aa302414.aspx)

Costuma ser fundamental para aplicativos que são capazes de proteger os dados passados de e para um servidor de banco de dados do SQL Server. Com o SQL Server, você pode usar o SSL para criar um canal criptografado. Este artigo "como" mostra como instalar um certificado no servidor de banco de dados,...

<a id="general"></a>
## <a name="general-whitepapers"></a>White papers de geral

O seguinte estudo de caso descreve o processo que foi usado para migrar sites de comunidade do .NET da Microsoft de um ambiente de hospedagem tradicional para o Microsoft Azure.

[Migrando da Microsoft ASP.NET e sites de comunidade do IIS.NET para o Microsoft Azure](https://go.microsoft.com/fwlink/?LinkId=400656)

Esses white papers abrangem uma variedade de tópicos relacionados a ASP.NET.

[Usar o monitoramento de integridade no ASP.NET 2.0](https://msdn.microsoft.com/library/ms998306.aspx)

Este artigo "como" mostra como usar o monitoramento para instrumentar seu aplicativo para um evento personalizado de integridade. Para criar uma evento de monitoramento de integridade personalizada, você pode criar uma classe que deriva de System.Web.Management.WebBaseEvent, configure a &lt;healthMonitoring&gt; ...

[Implementar o gerenciamento de Patch](https://msdn.microsoft.com/library/aa302364.aspx)

Este tópico como fazer explica o gerenciamento de patches, incluindo como manter um único ou vários servidores atualizados. Software adicional não é necessário, exceto para as ferramentas disponíveis para download da Microsoft. Operações e a política de segurança devem adotar um gerenciamento de patches...

[Controles de servidor ASP.NET para o Silverlight no Silverlight 3 do SDK](https://go.microsoft.com/fwlink/?LinkId=153377)

Os controles de servidor ASP.NET para o Silverlight ("controles do Silverlight do ASP.NET"), que são os controles do Media Player do ASP.NET e Silverlight, foram removidos do SDK do Silverlight para o Silverlight versão 3. Este documento fornece orientação para desenvolvedores que trabalharam com esses ASP.NET...

[Criando aplicativos da Web de alto desempenho](https://devexpress.com/act)

Saiba como usar os novos recursos na biblioteca do Ajax ASP.NET para criar aplicativos de Web alto desempenho
