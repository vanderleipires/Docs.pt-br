---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
title: "Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio ou Visual Web Developer: solução de problemas (12 de 12) | Microsoft Docs"
author: tdykstra
description: "Esta série de tutoriais mostra como implantar um ASP.NET (publicar) projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando Visual Stu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 3fc23eed-921d-4d46-a610-a2d156e4bd03
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
msc.type: authoredcontent
ms.openlocfilehash: d8c4931a1d26af49ee61c896897fa6ddf12fccea
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-troubleshooting-12-of-12"></a>Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio ou Visual Web Developer: solução de problemas (12 de 12)
====================
Por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto Starter](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta série de tutoriais mostra como implantar um ASP.NET (publicar) projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando o Visual Studio 2012 RC ou Visual Studio Express 2012 RC para Web. Se você instalar a atualização de publicação na Web, você também pode usar o Visual Studio 2010. Para obter uma introdução à série, consulte [primeiro tutorial na série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para obter um tutorial que mostra os recursos de implantação introduzidos após a versão RC do Visual Studio 2012, mostra como implantar as edições do SQL Server diferente do SQL Server Compact e mostra como implantar o Windows Azure Web Sites, consulte [implantação da Web do ASP.NET usando o Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


Esta página descreve alguns problemas comuns que podem surgir quando você implantar um aplicativo web ASP.NET usando o Visual Studio. Para cada um, um ou mais possíveis causas e soluções correspondentes são fornecidas.

## <a name="server-error-in--application---current-custom-error-settings-prevent-details-of-the-error-from-being-viewed-remotely"></a>Erro de servidor no aplicativo - '/' erro personalizado configurações atuais impedem que detalhes do erro que está sendo exibido remotamente

### <a name="scenario"></a>Cenário

Depois de implantar um site para um host remoto, você receberá uma mensagem de erro que menciona a configuração de customErrors no arquivo Web. config, mas não indica qual foi a causa real do erro:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample1.cmd)]

### <a name="possible-cause-and-solution"></a>Possível causa e solução

Por padrão, o ASP.NET mostra informações de erro detalhadas somente quando o aplicativo da web está em execução no computador local. Em geral, você não deseja exibir informações detalhadas de erro quando seu aplicativo web está disponível publicamente na Internet, porque os hackers poderá usar essas informações para localizar vulnerabilidades no aplicativo. No entanto, quando você estiver implantando um site ou em um site, às vezes, algo dê errado e você precisa obter a mensagem de erro real.

Para ativar o aplicativo exibir mensagens de erro detalhadas quando ele é executado no host remoto, edite o arquivo Web. config para definir `customErrors` modo off, reimplante o aplicativo e executar o aplicativo novamente:

1. Se o arquivo Web. config do aplicativo tiver um `customErrors` elemento o `system.web` elemento, alterar o `mode` atributo como "off". Caso contrário, adicione um `customErrors` elemento o `system.web` elemento com o `mode` atributo definido como "off", conforme mostrado no exemplo a seguir:

    [!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample2.xml?highlight=3)]
2. Implante o aplicativo.
3. Executar o aplicativo e repita o que você fez anteriormente que causou o erro ocorrer. Agora você pode ver o que é a mensagem de erro real.
4. Quando você tiver resolvido o erro, restaurar original `customErrors` configuração e reimplantar o aplicativo.

## <a name="access-is-denied-in-a-web-page-that-uses-sql-server-compact"></a>Acesso negado em uma página da Web que usa SQL Server Compact

### <a name="scenario"></a>Cenário

Quando você implanta um site que usa o SQL Server Compact e executar uma página no site implantado que acessa o banco de dados, você verá a seguinte mensagem de erro:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample3.cmd)]

### <a name="possible-cause-and-solution"></a>Possível causa e solução

A conta de serviço de rede no servidor precisa ser capaz de ler o serviço do SQL Compact binários nativos que estão no *bin\amd64* ou *bin\x86* pasta, mas ele não tem permissões de leitura para essas pastas. Conjunto de permissão de leitura para o serviço de rede no *bin* pasta, certificando-se de estender as permissões para as subpastas.

## <a name="cannot-read-configuration-file-due-to-insufficient-permissions"></a>Não é possível ler o arquivo de configuração devido a permissões insuficientes

### <a name="scenario"></a>Cenário

Quando você clica em Visual Studio botão Publicar para implantar um aplicativo para o IIS no computador local, publicação falhar e o **saída** janela mostra uma mensagem de erro semelhante a este:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample4.cmd)]

### <a name="possible-cause-and-solution"></a>Possível causa e solução

Para usar um clique de publicação ao IIS no computador local, você deve estar executando o Visual Studio com permissões de administrador. Feche o Visual Studio e reiniciá-lo com permissões de administrador.

## <a name="could-not-connect-to-the-destination-computer--using-the-specified-process"></a>Não foi possível conectar ao computador de destino... Usando o processo especificado

### <a name="scenario"></a>Cenário

Quando você clica em Visual Studio botão Publicar para implantar um aplicativo, publicação falhar e o **saída** janela mostra uma mensagem de erro semelhante a este:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample5.cmd)]

### <a name="possible-cause-and-solution"></a>Possível causa e solução

Um servidor proxy é interromper a comunicação com o servidor de destino. No painel de controle do Windows ou no Internet Explorer, selecione **opções da Internet** e selecione o **conexões** guia. No **propriedades da Internet** caixa de diálogo, clique em **configurações da LAN**. No **configurações de rede Local (LAN)** caixa de diálogo, desmarque o **detectar automaticamente as configurações** caixa de seleção. Em seguida, clique no botão Publicar novamente.

Se o problema persistir, contate o administrador do sistema para determinar o que pode ser feito com as configurações de proxy ou firewall. O problema ocorre porque a implantação da Web usa uma porta não padrão para a implantação de serviço de gerenciamento da Web (8172); para outras conexões, a implantação da Web usa a porta 80. Quando você estiver implantando em um provedor de hospedagem de terceiros, normalmente você está usando o serviço de gerenciamento da Web.

## <a name="default-net-40-application-pool-does-not-exist"></a>Pool de aplicativos 4.0 do .NET padrão não existe

### <a name="scenario"></a>Cenário

Quando você implanta um aplicativo que requer o .NET Framework 4, você verá a seguinte mensagem de erro:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample6.cmd)]

### <a name="possible-cause-and-solution"></a>Possível causa e solução

O ASP.NET 4 não está instalado no IIS. Se o servidor que você está implantando é seu computador de desenvolvimento e com o Visual Studio 2010 instalado, o ASP.NET 4 está instalado no computador, mas não pode ser instalado no IIS. No servidor que você está implantando, abra um prompt de comando elevado e instale o ASP.NET 4 no IIS, executando os seguintes comandos:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample7.cmd)]

Você também precisará definir manualmente a versão do .NET Framework do pool de aplicativos padrão. Para obter mais informações, consulte o [implantando para o IIS como um ambiente de teste](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) tutorial.

## <a name="format-of-the-initialization-string-does-not-conform-to-specification-starting-at-index-0"></a>Formato da cadeia de caracteres de inicialização não está de acordo com a especificação iniciada no índice 0.

### <a name="scenario"></a>Cenário

Depois de implantar um aplicativo usando um clique publica, quando você executa uma página que acessa o banco de dados você receber a seguinte mensagem de erro:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample8.cmd)]

### <a name="possible-cause-and-solution"></a>Possível causa e solução

Abra o *Web. config* arquivo no site implantado e verifique se os valores de cadeia de caracteres de conexão começam com `$(ReplacableToken_`, conforme mostrado no exemplo a seguir:

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample9.xml)]

Se as cadeias de caracteres de conexão é parecido com este exemplo, edite o arquivo de projeto e adicione a propriedade a seguir para o `PropertyGroup` elemento é para todas as configurações de compilação:

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample10.xml)]

Em seguida, reimplante o aplicativo.

## <a name="http-500-internal-server-error"></a>HTTP 500 Internal Server Error

### <a name="scenario"></a>Cenário

Quando você executar o site implantado, você verá a seguinte mensagem de erro sem informações específicas que indica a causa do erro:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample11.cmd)]

### <a name="possible-cause-and-solution"></a>Possível causa e solução

Há muitas causas de erros de 500, mas uma possível causa se você estiver seguindo um destes tutoriais é que você coloque um elemento XML no local errado em um dos arquivos de transformação de XML. Por exemplo, você deve receber esse erro se você colocar a transformação que insere um `<location>` elemento sob `<system.web>` em vez de diretamente em `<configuration>`. Nesse caso é a solução corrigir o arquivo de transformação XML e reimplantar.

## <a name="http-50021-internal-server-error"></a>Erro de servidor interno 500.21 HTTP

### <a name="scenario"></a>Cenário

Quando você executar o site implantado, você verá a seguinte mensagem de erro:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample12.cmd)]

### <a name="possible-cause-and-solution"></a>Possível causa e solução

O site de você ter implantado o ASP.NET 4, mas o ASP.NET 4 não está registrado no IIS no servidor de destino. No servidor de abrir um prompt de comando com privilégios elevados e registrar o ASP.NET 4, executando os seguintes comandos:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample13.cmd)]

Você também precisará definir manualmente a versão do .NET Framework do pool de aplicativos padrão. Para obter mais informações, consulte o [implantando para o IIS como um ambiente de teste](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) tutorial.

## <a name="login-failed-opening-sql-server-express-database-in-appdata"></a>Falha de logon abertura SQL Server Express banco de dados no aplicativo\_dados

### <a name="scenario"></a>Cenário

Atualizado o *Web. config* arquivo de cadeia de caracteres de conexão para apontar para um banco de dados do SQL Server Express como um *. mdf* do arquivo em seu *aplicativo\_dados* pasta e o primeiro tempo de que executar o aplicativo, que você verá a seguinte mensagem de erro:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample14.cmd)]

### <a name="possible-cause-and-solution"></a>Possível causa e solução

O nome do *. mdf* arquivo não pode corresponder ao nome de qualquer banco de dados SQL Server Express que já existe em seu computador, mesmo se você tiver excluído o *. mdf* arquivo do banco de dados já existente. Alterar o nome do *. mdf* arquivo para um nome que nunca foi usado como um nome de banco de dados e altere o *Web. config* arquivo para usar o novo nome. Como alternativa, você pode usar [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) excluir preexistentes SQL Server Express bancos de dados.

## <a name="model-compatibility-cannot-be-checked"></a>Compatibilidade de modelo não pode ser verificada

### <a name="scenario"></a>Cenário

Atualizado o *Web. config* arquivo de cadeia de caracteres de conexão para apontar para um novo banco de dados do SQL Server Express e, na primeira vez que você executar o aplicativo você verá a seguinte mensagem de erro:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample15.cmd)]

### <a name="possible-cause-and-solution"></a>Possível causa e solução

Se o nome do banco de dados é colocado no arquivo Web. config já foi usado antes em seu computador, um banco de dados pode já existir com algumas tabelas. Selecione um novo nome não foi usado em seu computador antes e alterar o *Web. config* arquivo para apontar para usar esse novo nome de banco de dados. Como alternativa, você pode usar [SQL Server Express Utility](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990) ou [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) para excluir o banco de dados existente.

## <a name="sql-error-when-a-script-attempts-to-create-users-or-roles"></a>Quando um Script tenta criar usuários ou funções de erro do SQL

### <a name="scenario"></a>Cenário

Você está usando a implantação de banco de dados configurada no **pacote/publicar SQL** guia, scripts SQL que são executados durante a implantação incluem comandos Create User ou Create Role e haverá falha na execução do script quando esses comandos são executados. Você poderá ver que mais detalhadas mensagens, como o seguinte:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample16.cmd)]

Se esse erro ocorre quando você tiver configurado uma implantação de banco de dados no **Publicar Web** assistente em vez de **pacote/publicar SQL** guia, criar um thread no [configuração e Implantação](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) fórum e a solução serão adicionado a esta página de solução de problemas.

### <a name="possible-cause-and-solution"></a>Possível causa e solução

A conta de usuário que você está usando para executar a implantação não tem permissão para criar usuários ou funções. Por exemplo, a empresa de hospedagem pode ser atribuídos a `db_datareader`, `db_datawriter`, e `db_ddladmin` funções para a conta de usuário que configura para você. Esses são suficientes para criar a maioria dos objetos de banco de dados, mas não para a criação de usuários ou funções. É uma maneira de evitar o erro excluindo usuários e funções da implantação de banco de dados. Você pode fazer isso editando o `PreSource` elemento para o banco de dados automaticamente gerada do script para que ele inclui os seguintes atributos:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample17.cmd)]

Para obter informações sobre como editar o `PreSource` elemento no arquivo de projeto, consulte [como: Editar configurações de implantação no arquivo de projeto](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx). Se os usuários ou funções em seu banco de dados de desenvolvimento precisam ser no banco de dados de destino, entre em contato com seu provedor de hospedagem para obter assistência.

## <a name="sql-server-timeout-error-when-running-custom-scripts-during-deployment"></a>Erro de tempo limite do SQL Server durante a execução de Scripts personalizados durante a implantação

### <a name="scenario"></a>Cenário

Você especificou os scripts SQL personalizados para ser executado durante a implantação, e quando a implantação da Web executa, o tempo limite.

### <a name="possible-cause-and-solution"></a>Possível causa e solução

Executar vários scripts que têm transações diferentes modos pode causar erros de tempo limite. Por padrão, executar scripts gerados automaticamente em uma transação, mas não scripts personalizados. Se você selecionar o **extrair dados e/ou esquema de banco de dados existente** opção o **pacote/publicar SQL** guia, e se você adicionar um script SQL personalizado, você deve alterar as configurações de transação em alguns scripts para que todos os scripts usam as mesmas configurações de transação. Para obter mais informações, consulte [como: implantar um banco de dados com um projeto de aplicativo Web](https://msdn.microsoft.com/library/dd465343.aspx).

Se você configurou as configurações de transação para que todos são os mesmos, mas ainda receber esse erro, uma solução alternativa é executar os scripts separadamente. No **Scripts de banco de dados** grade no **pacote/publicar** guia SQL, desmarque o **incluir** caixa de seleção para o script que faz com que o erro de tempo limite, em seguida, publicar o projeto. Em seguida, vá para o **Scripts de banco de dados** grade, selecione esse script **incluir** caixa de seleção e, em seguida, desmarque o **incluir** caixas de seleção para os outros scripts. Em seguida, publica o projeto novamente. Neste momento, quando você publica, o script selecionado personalizado é executado.

## <a name="stream-data-of-site-manifest-is-not-yet-available"></a>Dados de fluxo do manifesto de Site ainda não estão disponíveis

### <a name="scenario"></a>Cenário

Quando você estiver instalando um pacote usando o *Deploy* arquivo com o `t` opção (teste), você verá a seguinte mensagem de erro:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample18.cmd)]

### <a name="possible-cause-and-solution"></a>Possível causa e solução

A mensagem de erro significa que o comando não pode produzir um relatório de teste. No entanto, o comando pode ser executado se você usar o `y` opção (instalação). A mensagem só indica que há um problema com a execução do comando no modo de teste.

## <a name="this-application-requires-managedruntimeversion-v40"></a>Este aplicativo requer v ManagedRuntimeVersion 4.0

### <a name="scenario"></a>Cenário

Quando você tenta implantar, você verá a seguinte mensagem de erro:

 Erro: Os dados de fluxo de ' sitemanifest/dbFullSql [@path= 'C:\TEMP\AdventureWorksGrant.sql']/sqlScript' ainda não está disponível. O pool de aplicativos que você está tentando usar tem a propriedade 'managedRuntimeVersion' definido como 'v 2.0'. Este aplicativo requer 'v 4.0'. 

### <a name="possible-cause-and-solution"></a>Possível causa e solução

O ASP.NET 4 não está instalado no IIS. Se o servidor que você está implantando é seu computador de desenvolvimento e com o Visual Studio 2010 instalado, o ASP.NET 4 está instalado no computador, mas não pode ser instalado no IIS. No servidor que você está implantando, abra um prompt de comando elevado e instale o ASP.NET 4 no IIS, executando os seguintes comandos:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample19.cmd)]

## <a name="unable-to-cast-microsoftwebdeploymentdeploymentprovideroptions"></a>Não é possível converter Microsoft.Web.Deployment.DeploymentProviderOptions

### <a name="scenario"></a>Cenário

Quando você estiver implantando um pacote, você verá a seguinte mensagem de erro:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample20.cmd)]

### <a name="possible-cause-and-solution"></a>Possível causa e solução

Você está tentando implantar do Gerenciador do IIS usando a IU da Web implantar 1.1 para um servidor que tem o Web Deploy 2.0 instalado. Se você estiver usando a ferramenta de administração remota do IIS para implantar com a importação de um pacote, verifique o **novos recursos disponíveis** caixa de diálogo quando você estabelecer a conexão. (Essa caixa de diálogo pode ser mostrada apenas uma vez quando a conexão é estabelecida pela primeira vez. Para limpar a conexão e recomeçar, feche o Gerenciador do IIS e iniciá-lo novamente inserindo `inetmgr /reset` no prompt de comando.) Se um dos recursos listados é **Web implantar UI**e ele tem um número de versão inferior a 8, o servidor que você está implantando pode ter versões 1.1 e 2.0 de implantação da Web instalado. Para implantar de um cliente que tenha 2.0 instalado, o servidor deve ter apenas Web Deploy 2.0 instalado. Você precisará entrar em contato com seu provedor de hospedagem para resolver esse problema.

## <a name="unable-to-load-the-native-components-of-sql-server-compact"></a>Não é possível carregar os componentes nativo do SQL Server Compact

### <a name="scenario"></a>Cenário

Quando você executar o site implantado, você verá a seguinte mensagem de erro:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample21.cmd)]

### <a name="possible-cause-and-solution"></a>Possível causa e solução

O site implantado não tem *amd64* e *x86* subpastas com os assemblies nativo neles sob o aplicativo *bin* pasta. Em um computador que tenha o SQL Server Compact instalado, os módulos nativos estão localizados em *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private*. É a melhor maneira de obter os arquivos corretos nas pastas corretas em um projeto do Visual Studio instalar o pacote do NuGet SqlServerCompact. Instalação do pacote adiciona um script de pós-compilação para copiar os assemblies nativo em *amd64* e *x86*. No entanto, em ordem para essas ser implantado, você precisa inclui-las manualmente no projeto. Para obter mais informações, consulte o [Implantando o SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) tutorial.

## <a name="path-is-not-valid-error-after-deploying-an-entity-framework-code-first-application"></a>Erro "O caminho não é válido" depois de implantar um aplicativo Entity Framework Code First

### <a name="scenario"></a>Cenário

Implantar um aplicativo que usa o Entity Framework Code First Migrations e um DBMS, como o SQL Server Compact que armazena seu banco de dados em um arquivo no aplicativo\_pasta de dados. Você tem as migrações do Code First configurado para criar o banco de dados após a primeira implantação. Quando você executar o aplicativo você receberá uma mensagem de erro semelhante ao seguinte exemplo:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample22.cmd)]

### <a name="possible-cause-and-solution"></a>Possível causa e solução

Primeiro código está tentando criar o banco de dados, mas o aplicativo\_pasta de dados não existe. Ou você não tem todos os arquivos *aplicativo\_dados* pasta quando você implantou ou selecionado **excluir aplicativo\_dados** no **pacote/Publicar Web** guia o **propriedades do projeto** janela. O processo de implantação não criará uma pasta no servidor se não existem arquivos na pasta a ser copiado para o servidor. Se você já tiver o banco de dados configurado no site, o processo de implantação excluirá os arquivos e o *aplicativo\_dados* pasta propriamente dita se você selecionou **remover arquivos adicionais no destino** em o perfil de publicação. Para resolver o problema, coloque um arquivo de espaço reservado, como um arquivo. txt a *aplicativo\_dados* pasta, verifique se você não tem **excluir aplicativo\_dados** selecionado e reimplantar. 

## <a name="com-object-that-has-been-separated-from-its-underlying-rcw-cannot-be-used"></a>"O objeto COM que foi separado de seu RCW subjacente não pode ser usado."

### <a name="scenario"></a>Cenário

Foram com êxito usando um clique de publicação para implantar seu aplicativo e, em seguida, iniciar recebendo essa mensagem de erro:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample23.cmd)]

### <a name="possible-cause-and-solution"></a>Possível causa e solução

Fechar e reiniciar o Visual Studio geralmente é tudo o que é necessário para resolver esse erro.

## <a name="deployment-fails-because-user-credentials-used-for-publishing-dont-have-setacl-authority"></a>Implantação falhar porque o usuário as credenciais usadas para setACL de publicação não tem autoridade

### <a name="scenario"></a>Cenário

Publicação falhará com um erro indicando que você não tem autoridade para definir permissões de pasta (a conta de usuário que você está usando não tem autoridade setACL).

### <a name="possible-cause-and-solution"></a>Possível causa e solução

Por padrão, o Visual Studio define permissões de leitura na pasta raiz do site e permissões de gravação no aplicativo\_pasta de dados. Se você souber que as permissões padrão em pastas do site estão corretas e não precisam ser definidas, você desabilitar esse comportamento adicionando  **&lt;IncludeSetACLProviderOn destino&gt;False&lt;/ IncludeSetACLProviderOnDestination&gt;**  para o arquivo de perfil de publicação (para afetar um único perfil) ou para o arquivo wpp.targets (para afetar todos os perfis). Para obter informações sobre como editar esses arquivos, consulte [como: Editar configurações de implantação no arquivo de perfil (. pubxml)](https://msdn.microsoft.com/library/ff398069.aspx). 

## <a name="access-denied-errors-when-the-application-tries-to-write-to-an-application-folder"></a>Erros de acesso negado quando o aplicativo tenta gravar em uma pasta de aplicativo

### <a name="scenario"></a>Cenário

Os erros de aplicativo quando ele tenta criar ou editar um arquivo em uma das pastas de aplicativo, porque ele não tem autoridade de gravação para essa pasta.

### <a name="possible-cause-and-solution"></a>Possível causa e solução

Por padrão, o Visual Studio define permissões de leitura na pasta raiz do site e permissões de gravação no aplicativo\_pasta de dados. Se seu aplicativo precisa de acesso de gravação para uma subpasta, você pode definir permissões para essa pasta, conforme mostrado no [definindo permissões de pasta](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md) e [implantando no ambiente de produção](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutoriais. Se seu aplicativo precisa de acesso de gravação para a pasta raiz do site, você precisa impedir que a configuração de acesso somente leitura na pasta raiz, adicionando  **&lt;IncludeSetACLProviderOn destino&gt;False&lt;/ IncludeSetACLProviderOnDestination&gt;**  para o arquivo de perfil de publicação (para afetar um único perfil) ou para o arquivo wpp.targets (para afetar todos os perfis). Para obter informações sobre como editar esses arquivos, consulte [como: Editar configurações de implantação no arquivo de perfil (. pubxml)](https://msdn.microsoft.com/library/ff398069.aspx). <a id="aspnet45error"></a>

## <a name="configuration-error---targetframework-attribute-references-a-version-that-is-later-than-the-installed-version-of-the-net-framework"></a>Erro de configuração - atributo targetFramework faz referência a uma versão mais recente do que a versão instalada do .NET Framework

### <a name="scenario"></a>Cenário

Você publicou com êxito um projeto web que tem como alvo o ASP.NET 4.5, mas quando você executar o aplicativo (com o `customErrors` modo definido como "off" no arquivo Web. config) você obterá o seguinte erro:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample24.cmd)]

A caixa de erro de origem da página de erro realça a linha seguinte da Web. config como a causa do erro:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample25.cmd)]

### <a name="possible-cause-and-solution"></a>Possível causa e solução

O servidor não suporta o ASP.NET 4.5. Entre em contato com o provedor de hospedagem para determinar quando e se o suporte para o ASP.NET 4.5 pode ser adicionado. Se atualizar o servidor não for uma opção, você precisa implantar um projeto da web que tem como alvo o ASP.NET 4 ou anterior em vez disso. Se você implantar um ASP.NET 4 ou anterior web para o mesmo destino, selecione o **remover arquivos adicionais no destino** caixa de seleção de **configurações** guia do **Publicar Web**assistente. Se você não selecionar **remover arquivos adicionais no destino**, você continuará para a página de erro de configuração.

O projeto **propriedades** windows inclui uma lista de lista suspensa do framework de destino, mas você não pode resolver esse problema alterando apenas do **.NET Framework 4.5** para **do.NETFramework4**. Se você alterar a estrutura de destino para uma versão anterior do framework, o projeto ainda terão referências aos assemblies da versão posterior do framework e não será executado. Você precisa alterar essas referências manualmente ou criar um novo projeto que tem como alvo o .NET Framework 4 ou anterior. Para obter mais informações, consulte [.NET Framework direcionamento para Sites da Web](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx).

>[!div class="step-by-step"]
[Anterior](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
