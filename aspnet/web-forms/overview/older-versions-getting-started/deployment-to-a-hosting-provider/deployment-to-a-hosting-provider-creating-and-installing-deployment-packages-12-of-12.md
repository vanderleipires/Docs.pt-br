---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
title: 'Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio ou Visual Web Developer: solução de problemas (12 de 12) | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um ASP.NET projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando o Visual Stu...
ms.author: aspnetcontent
ms.date: 11/17/2011
ms.assetid: 3fc23eed-921d-4d46-a610-a2d156e4bd03
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
msc.type: authoredcontent
ms.openlocfilehash: d6175d46c6df3c9a4d9bc98492a4458bec45221c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811591"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-troubleshooting-12-of-12"></a>Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio ou Visual Web Developer: solução de problemas (12 de 12)
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto inicial](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta série de tutoriais mostra como implantar (publicar) um ASP.NET projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando o Visual Studio 2012 RC ou Visual Studio Express 2012 RC para Web. Você também pode usar o Visual Studio 2010 se você instalar a atualização de publicação na Web. Para obter uma introdução à série, consulte [o primeiro tutorial na série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para obter um tutorial que mostra os recursos de implantação introduzidos após a versão RC do Visual Studio 2012, mostra como implantar as edições do SQL Server que não seja o SQL Server Compact e mostra como implantar o Windows Azure Web Sites, consulte [implantação da Web do ASP.NET usando o Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


Esta página descreve alguns problemas comuns que podem surgir quando você implanta um aplicativo web ASP.NET usando o Visual Studio. Para cada um deles, um ou mais possíveis causas e soluções correspondentes são fornecidas.

## <a name="server-error-in--application---current-custom-error-settings-prevent-details-of-the-error-from-being-viewed-remotely"></a>Erro de servidor no aplicativo - '/' configurações atuais de erro personalizada impedirem detalhes do erro que está sendo visualizado remotamente

### <a name="scenario"></a>Cenário

Depois de implantar um site em um host remoto, você receberá uma mensagem de erro que menciona a configuração de customErrors no arquivo Web. config, mas não indica qual foi a causa real do erro:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample1.cmd)]

### <a name="possible-cause-and-solution"></a>Possível causa e solução

Por padrão, o ASP.NET mostra informações detalhadas do erro somente quando seu aplicativo web está em execução no computador local. Geralmente, você não deseja exibir informações detalhadas do erro quando seu aplicativo web está disponível publicamente na Internet, porque os hackers podem ser capazes de usar essas informações para encontrar vulnerabilidades no aplicativo. No entanto, quando você estiver implantando um site ou em um site, às vezes, algo pode dar errado e você precisa obter a mensagem de erro real.

Para habilitar o aplicativo para exibir mensagens de erro detalhadas quando ele é executado no host remoto, edite o arquivo Web. config para definir `customErrors` modo off, reimplantar o aplicativo e executar o aplicativo novamente:

1. Se o arquivo Web. config do aplicativo tem um `customErrors` elemento na `system.web` elemento, altere o `mode` atributo como "desativado". Caso contrário, adicione uma `customErrors` elemento na `system.web` elemento com o `mode` atributo definido como "desativado", conforme mostrado no exemplo a seguir:

    [!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample2.xml?highlight=3)]
2. Implante o aplicativo.
3. Execute o aplicativo e repita a tudo o que você fez anteriormente que causou o erro ocorrer. Agora você pode ver o que é a mensagem de erro real.
4. Quando você tiver resolvido o erro, restaurar original `customErrors` definindo e reimplantar o aplicativo.

## <a name="access-is-denied-in-a-web-page-that-uses-sql-server-compact"></a>Acesso negado em uma página da Web que usa SQL Server Compact

### <a name="scenario"></a>Cenário

Quando você implantar um site que usa o SQL Server Compact e executa uma página no site implantado que acessa o banco de dados, você verá a seguinte mensagem de erro:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample3.cmd)]

### <a name="possible-cause-and-solution"></a>Possível causa e solução

A conta de serviço de rede no servidor precisa ser capaz de ler o serviço do SQL Compact binários nativos que estão na *bin\amd64* ou *bin\x86* pasta, mas ele não tem permissões de leitura para essas pastas. Conjunto de permissão de leitura para o serviço de rede sobre o *bin* pasta, certificando-se de estender as permissões para as subpastas.

## <a name="cannot-read-configuration-file-due-to-insufficient-permissions"></a>Não é possível ler o arquivo de configuração devido a permissões insuficientes

### <a name="scenario"></a>Cenário

Quando você clica em Visual Studio botão Publicar para implantar um aplicativo para o IIS no computador local, publicação falhar e o **saída** janela mostra uma mensagem de erro semelhante a esta:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample4.cmd)]

### <a name="possible-cause-and-solution"></a>Possível causa e solução

Para usar um clique de publicação para o IIS no computador local, você deve estar executando o Visual Studio com permissões de administrador. Feche o Visual Studio e reiniciá-lo com permissões de administrador.

## <a name="could-not-connect-to-the-destination-computer--using-the-specified-process"></a>Não foi possível conectar ao computador de destino... Usando o processo especificado

### <a name="scenario"></a>Cenário

Quando você clica em do Visual Studio botão Publicar para implantar um aplicativo de publicação falhará e o **saída** janela mostra uma mensagem de erro semelhante a esta:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample5.cmd)]

### <a name="possible-cause-and-solution"></a>Possível causa e solução

Um servidor proxy é interromper a comunicação com o servidor de destino. No painel de controle do Windows ou no Internet Explorer, selecione **opções da Internet** e selecione o **conexões** guia. No **propriedades da Internet** caixa de diálogo, clique em **configurações de LAN**. No **configurações de rede Local (LAN)** caixa de diálogo, desmarque a **detectar automaticamente as configurações** caixa de seleção. Em seguida, clique no botão Publicar novamente.

Se o problema persistir, contate o administrador do sistema para determinar o que pode ser feito com as configurações de proxy ou firewall. O problema ocorre porque a implantação da Web usa uma porta não padrão para a implantação de serviço de gerenciamento da Web (8172); para outras conexões, a implantação da Web usa a porta 80. Quando você estiver implantando em um provedor de hospedagem de terceiros, você normalmente está usando o serviço de gerenciamento da Web.

## <a name="default-net-40-application-pool-does-not-exist"></a>Pool de aplicativos 4.0 do .NET padrão não existe

### <a name="scenario"></a>Cenário

Quando você implanta um aplicativo que requer o .NET Framework 4, você verá a seguinte mensagem de erro:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample6.cmd)]

### <a name="possible-cause-and-solution"></a>Possível causa e solução

ASP.NET 4 não está instalado no IIS. Se o servidor que você está implantando é seu computador de desenvolvimento e com o Visual Studio 2010 instalado, o ASP.NET 4 está instalado no computador, mas pode não estar instalado no IIS. No servidor que você está implantando, abra um prompt de comando elevado e instale o ASP.NET 4 no IIS executando os comandos a seguir:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample7.cmd)]

Talvez você também precise definir manualmente a versão do .NET Framework do pool de aplicativos padrão. Para obter mais informações, consulte o [implantando no IIS como um ambiente de teste](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) tutorial.

## <a name="format-of-the-initialization-string-does-not-conform-to-specification-starting-at-index-0"></a>Formato da cadeia de caracteres de inicialização não se adequa à especificação começando no índice 0.

### <a name="scenario"></a>Cenário

Depois de implantar um aplicativo usando um único clique publica, quando você executa uma página que acessa o banco de dados você receber a seguinte mensagem de erro:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample8.cmd)]

### <a name="possible-cause-and-solution"></a>Possível causa e solução

Abra o *Web. config* arquivo no site implantado e verifique se os valores de cadeia de caracteres de conexão começam com `$(ReplacableToken_`, conforme mostrado no exemplo a seguir:

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample9.xml)]

Se as cadeias de caracteres de conexão se parecer com este exemplo, edite o arquivo de projeto e adicione as seguintes propriedades para o `PropertyGroup` elemento que está para todas as configurações de compilação:

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample10.xml)]

Em seguida, reimplante o aplicativo.

## <a name="http-500-internal-server-error"></a>Erro de servidor interno 500 HTTP

### <a name="scenario"></a>Cenário

Quando você executa o site implantado, você verá a seguinte mensagem de erro sem informações específicas que indica a causa do erro:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample11.cmd)]

### <a name="possible-cause-and-solution"></a>Possível causa e solução

Há muitas causas de 500 erros, mas uma causa possível se você estiver seguindo esses tutoriais é que você coloque um elemento XML no lugar errado em um dos arquivos de transformação de XML. Por exemplo, você obteria esse erro se você colocar a transformação que insere um `<location>` sob o elemento `<system.web>` em vez de diretamente em `<configuration>`. Nesse caso, a solução é corrigir o arquivo de transformação de XML e reimplantar.

## <a name="http-50021-internal-server-error"></a>Erro interno do servidor 500.21 de HTTP

### <a name="scenario"></a>Cenário

Quando você executa o site implantado, você verá a seguinte mensagem de erro:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample12.cmd)]

### <a name="possible-cause-and-solution"></a>Possível causa e solução

O site, você já implantou o ASP.NET 4, mas o ASP.NET 4 não está registrado no IIS no servidor de destinos. No servidor de abrir um prompt de comando elevado e registrar o ASP.NET 4 executando os comandos a seguir:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample13.cmd)]

Talvez você também precise definir manualmente a versão do .NET Framework do pool de aplicativos padrão. Para obter mais informações, consulte o [implantando no IIS como um ambiente de teste](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) tutorial.

## <a name="login-failed-opening-sql-server-express-database-in-appdata"></a>Abrindo o SQL Server Express Database no aplicativo falha no logon\_dados

### <a name="scenario"></a>Cenário

Atualizado o *Web. config* arquivo de cadeia de conexão para apontar para um banco de dados do SQL Server Express como um *. mdf* do arquivo no seu *aplicativo\_dados* pasta e o primeiro executar o aplicativo, que você verá a seguinte mensagem de erro de tempo:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample14.cmd)]

### <a name="possible-cause-and-solution"></a>Possível causa e solução

O nome da *. mdf* arquivo não pode corresponder ao nome de qualquer banco de dados SQL Server Express que tem um dia existiu no seu computador, mesmo se você tiver excluído o *. mdf* arquivo do banco de dados existente. Altere o nome da *. mdf* arquivo para um nome que nunca tenha sido usado como um nome de banco de dados e altere o *Web. config* arquivo para usar o novo nome. Como alternativa, você pode usar [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) excluir previamente existentes do SQL Server Express bancos de dados.

## <a name="model-compatibility-cannot-be-checked"></a>Compatibilidade de modelo não pode ser verificada

### <a name="scenario"></a>Cenário

Atualizado o *Web. config* arquivo de cadeia de conexão para apontar para um novo banco de dados do SQL Server Express e a primeira vez que você executar o aplicativo, você verá a seguinte mensagem de erro:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample15.cmd)]

### <a name="possible-cause-and-solution"></a>Possível causa e solução

Se o nome do banco de dados que você colocar o arquivo Web. config já foi usado antes no seu computador, um banco de dados talvez já existam com algumas tabelas nele. Selecione um novo nome não foi usado em seu computador antes e alterar o *Web. config* arquivo para apontar para usar esse novo nome de banco de dados. Como alternativa, você pode usar [SQL Server Express Utility](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990) ou [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) para excluir o banco de dados existente.

## <a name="sql-error-when-a-script-attempts-to-create-users-or-roles"></a>Erro ao detectar tentativas de um Script criar usuários ou funções do SQL

### <a name="scenario"></a>Cenário

Você está usando a implantação de banco de dados configurada na **empacotar/publicar SQL** guia, os scripts SQL que são executados durante a implantação incluem comandos Create User ou Create Role e falha de execução de script quando esses comandos são executados. Você poderá ver que mais detalhadas de mensagens, como o seguinte:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample16.cmd)]

Se esse erro ocorre quando você tiver configurado a implantação de banco de dados na **Publicar Web** assistente em vez de **empacotar/publicar SQL** guia, crie um thread no [configuração e Implantação](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) fórum e a solução serão adicionado a esta página de solução de problemas.

### <a name="possible-cause-and-solution"></a>Possível causa e solução

A conta de usuário que você está usando para executar uma implantação não tem permissão para criar usuários ou funções. Por exemplo, a empresa de hospedagem podem ser atribuídos a `db_datareader`, `db_datawriter`, e `db_ddladmin` funções à conta de usuário que configura para você. Esses são suficientes para criar a maioria dos objetos de banco de dados, mas não para a criação de usuários ou funções. É uma maneira de evitar o erro excluindo usuários e funções de implantação de banco de dados. Você pode fazer isso editando o `PreSource` elemento para o banco de dados automaticamente gerada pelo script para que ele inclui os seguintes atributos:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample17.cmd)]

Para obter informações sobre como editar o `PreSource` elemento no arquivo de projeto, consulte [como: Editar configurações de implantação no arquivo de projeto](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx). Se os usuários ou funções em seu banco de dados de desenvolvimento precisarem estar no banco de dados de destino, entre em contato com seu provedor de hospedagem para obter assistência.

## <a name="sql-server-timeout-error-when-running-custom-scripts-during-deployment"></a>Erro de tempo limite do SQL Server durante a execução de Scripts personalizados durante a implantação

### <a name="scenario"></a>Cenário

Você especificou os scripts SQL personalizados para ser executado durante a implantação, e quando a implantação da Web é executado-los, eles o tempo limite.

### <a name="possible-cause-and-solution"></a>Possível causa e solução

Execução de vários scripts que têm modos de transação diferentes pode causar erros de tempo limite. Por padrão, scripts gerados automaticamente executados em uma transação, mas não scripts personalizados. Se você selecionar o **efetuar Pull de dados e/ou esquema de banco de dados existente** opção a **empacotar/publicar SQL** guia, e se você adicionar um script SQL personalizado, você deve alterar as configurações de transação em alguns scripts, de modo que todos os scripts usam as mesmas configurações de transação. Para obter mais informações, consulte [como: implantar um banco de dados com um projeto de aplicativo Web](https://msdn.microsoft.com/library/dd465343.aspx).

Se você tiver definido configurações de transação, para que todos os mesmos, mas ainda recebe esse erro, uma possível solução alternativa é executar os scripts separadamente. No **Scripts de banco de dados** grade na **empacotar/publicar** guia SQL, desmarque o **Include** caixa de seleção para o script que faz com que o erro de tempo limite, em seguida, publicar o projeto. Em seguida, vá novamente na **Scripts de banco de dados** grade, selecione esse script **Include** caixa de seleção e, em seguida, desmarque a **Include** caixas de seleção para os outros scripts. Em seguida, publique o projeto novamente. Neste momento, quando você publica, somente as execuções de script personalizado selecionado.

## <a name="stream-data-of-site-manifest-is-not-yet-available"></a>Dados do Stream do manifesto do Site ainda não estão disponíveis

### <a name="scenario"></a>Cenário

Quando você estiver instalando um pacote usando o *Deploy. cmd* do arquivo com o `t` opção (teste), você verá a seguinte mensagem de erro:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample18.cmd)]

### <a name="possible-cause-and-solution"></a>Possível causa e solução

A mensagem de erro significa que o comando não pode produzir um relatório de teste. No entanto, talvez a executar o comando se você usar o `y` opção (instalação real). A mensagem só indica que há um problema com a execução do comando no modo de teste.

## <a name="this-application-requires-managedruntimeversion-v40"></a>Este aplicativo requer ManagedRuntimeVersion v4.0

### <a name="scenario"></a>Cenário

Quando você tenta implantar, você verá a seguinte mensagem de erro:

 Erro: Os dados de fluxo de ' sitemanifest/dbFullSql [@path= 'C:\TEMP\AdventureWorksGrant.sql']/sqlScript' ainda não está disponível. O pool de aplicativos que você está tentando usar tem a propriedade 'managedRuntimeVersion' definido como 'v2.0'. Este aplicativo requer 'v4.0'. 

### <a name="possible-cause-and-solution"></a>Possível causa e solução

ASP.NET 4 não está instalado no IIS. Se o servidor que você está implantando é seu computador de desenvolvimento e com o Visual Studio 2010 instalado, o ASP.NET 4 está instalado no computador, mas pode não estar instalado no IIS. No servidor que você está implantando, abra um prompt de comando elevado e instale o ASP.NET 4 no IIS executando os comandos a seguir:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample19.cmd)]

## <a name="unable-to-cast-microsoftwebdeploymentdeploymentprovideroptions"></a>Não é possível converter Microsoft.Web.Deployment.DeploymentProviderOptions

### <a name="scenario"></a>Cenário

Quando você estiver implantando um pacote, você verá a seguinte mensagem de erro:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample20.cmd)]

### <a name="possible-cause-and-solution"></a>Possível causa e solução

Você está tentando implantar do Gerenciador do IIS usando a IU da Web implantar 1.1 em um servidor que tem o Web Deploy 2.0 instalado. Se você estiver usando a ferramenta de administração remota do IIS para implantar com a importação de um pacote, a seleção de **novos recursos disponíveis** caixa de diálogo ao estabelecer a conexão. (Essa caixa de diálogo pode ser exibida somente uma vez quando a conexão é estabelecido pela primeira vez. Para limpar a conexão e começar novamente, feche o Gerenciador do IIS e iniciá-lo novamente, inserindo `inetmgr /reset` no prompt de comando.) Se um dos recursos listados **interface do usuário da Web implantar**e ele tem um número de versão inferior a 8, o servidor que você está implantando pode ter versões 1.1 e 2.0 da implantação da Web instalado. Para implantar de um cliente que tenha o 2.0 instalado, o servidor deve ter apenas Web Deploy 2.0 instalado. Você terá de entrar em contato com seu provedor de hospedagem para resolver esse problema.

## <a name="unable-to-load-the-native-components-of-sql-server-compact"></a>Não é possível carregar os componentes nativos do SQL Server Compact

### <a name="scenario"></a>Cenário

Quando você executa o site implantado, você verá a seguinte mensagem de erro:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample21.cmd)]

### <a name="possible-cause-and-solution"></a>Possível causa e solução

Não tem um site implantado *amd64* e *x86* subpastas com os assemblies nativos na caixa de diálogo *bin* pasta. Em um computador que tem o SQL Server Compact instalado, os assemblies nativos estão localizados em *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private*. É a melhor maneira de obter os arquivos corretos nas pastas corretas em um projeto do Visual Studio instalar o pacote do NuGet SqlServerCompact. Instalação do pacote adiciona um script de pós-compilação para copiar os assemblies nativos no *amd64* e *x86*. No entanto, em ordem para que eles sejam implantados, você precisa incluí-lo manualmente no projeto. Para obter mais informações, consulte o [Implantando o SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) tutorial.

## <a name="path-is-not-valid-error-after-deploying-an-entity-framework-code-first-application"></a>Erro "O caminho não é válido" depois de implantar um aplicativo do Entity Framework Code First

### <a name="scenario"></a>Cenário

Implantar um aplicativo que usa o Entity Framework Code First Migrations e um DBMS, como o SQL Server Compact que armazena seu banco de dados em um arquivo no aplicativo\_pasta de dados. Você tem as migrações Code First configurado para criar o banco de dados após sua primeira implantação. Quando você executar o aplicativo você receberá uma mensagem de erro semelhante ao seguinte exemplo:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample22.cmd)]

### <a name="possible-cause-and-solution"></a>Possível causa e solução

Primeiro código está tentando criar o banco de dados, mas o aplicativo\_pasta de dados não existe. Ou você não tem todos os arquivos *App\_dados* pasta quando você implantou ou selecionadas **excluir aplicativo\_dados** no **pacote/Publicar Web** guia do **propriedades do projeto** janela. O processo de implantação não criará uma pasta no servidor se não houver nenhum arquivo na pasta a ser copiado para o servidor. Se você já tiver o banco de dados configurado no site, o processo de implantação excluirá os arquivos e o *App\_dados* pasta em si se você selecionou **remover arquivos adicionais no destino** em o perfil de publicação. Para resolver o problema, coloque um arquivo de espaço reservado como um arquivo. txt na *App\_dados* pasta, verifique se você não tiver **excluir aplicativo\_dados** selecionado e reimplantar. 

## <a name="com-object-that-has-been-separated-from-its-underlying-rcw-cannot-be-used"></a>"O objeto COM que foi separado do seu RCW subjacente não pode ser usado."

### <a name="scenario"></a>Cenário

Foram com êxito usando um clique a publicação para implantar seu aplicativo e, em seguida, você começar a receber este erro:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample23.cmd)]

### <a name="possible-cause-and-solution"></a>Possível causa e solução

Fechar e reiniciar o Visual Studio geralmente é tudo o que é necessária para resolver esse erro.

## <a name="deployment-fails-because-user-credentials-used-for-publishing-dont-have-setacl-authority"></a>Implantação falhar porque o usuário as credenciais usadas para setACL publicação não tem autoridade

### <a name="scenario"></a>Cenário

Publicação falhará com um erro indicando que você não tiver autoridade para definir permissões de pasta (a conta de usuário que você está usando não tem autoridade setACL).

### <a name="possible-cause-and-solution"></a>Possível causa e solução

Por padrão, o Visual Studio define permissões de leitura na pasta raiz do site e permissões de gravação no aplicativo\_pasta de dados. Se você souber que as permissões padrão em pastas do site estão corretas e não precisam ser definidas, você desabilitar esse comportamento adicionando **&lt;destino IncludeSetACLProviderOn&gt;falso&lt;/ IncludeSetACLProviderOnDestination&gt;** para o arquivo de perfil de publicação (para afetar um único perfil) ou ao arquivo wpp.targets (para afetar todos os perfis). Para obter informações sobre como editar esses arquivos, consulte [como: Editar configurações de implantação nos arquivos de perfil (. pubxml)](https://msdn.microsoft.com/library/ff398069.aspx). 

## <a name="access-denied-errors-when-the-application-tries-to-write-to-an-application-folder"></a>Erros de acesso negado quando o aplicativo tenta gravar em uma pasta de aplicativo

### <a name="scenario"></a>Cenário

Os erros de aplicativo quando ele tenta criar ou editar um arquivo em uma das pastas de aplicativo, porque ele não tem autoridade de gravação para essa pasta.

### <a name="possible-cause-and-solution"></a>Possível causa e solução

Por padrão, o Visual Studio define permissões de leitura na pasta raiz do site e permissões de gravação no aplicativo\_pasta de dados. Se seu aplicativo precisa de acesso de gravação em uma subpasta, você pode definir permissões para essa pasta, conforme mostrado na [definindo permissões de pasta](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md) e [implantando no ambiente de produção](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutoriais. Se seu aplicativo precisa de acesso de gravação para a pasta raiz do site, você precisa impedir que a configuração de acesso somente leitura na pasta raiz, adicionando **&lt;destino IncludeSetACLProviderOn&gt;falso&lt;/ IncludeSetACLProviderOnDestination&gt;** para o arquivo de perfil de publicação (para afetar um único perfil) ou ao arquivo wpp.targets (para afetar todos os perfis). Para obter informações sobre como editar esses arquivos, consulte [como: Editar configurações de implantação nos arquivos de perfil (. pubxml)](https://msdn.microsoft.com/library/ff398069.aspx). <a id="aspnet45error"></a>

## <a name="configuration-error---targetframework-attribute-references-a-version-that-is-later-than-the-installed-version-of-the-net-framework"></a>Erro de configuração - targetFramework atributo faz referência a uma versão posterior à versão instalada do .NET Framework

### <a name="scenario"></a>Cenário

Você publicou com êxito um projeto web que tem como alvo o ASP.NET 4.5, mas quando você executa o aplicativo (com o `customErrors` modo definido como "desativado" no arquivo Web. config) você obterá o seguinte erro:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample24.cmd)]

A caixa de erro de código-fonte da página de erro realça a linha seguinte da Web. config como a causa do erro:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample25.cmd)]

### <a name="possible-cause-and-solution"></a>Possível causa e solução

O servidor não oferece suporte para ASP.NET 4.5. Entre em contato com o provedor de hospedagem para determinar quando e se o suporte para ASP.NET 4.5 pode ser adicionado. Se atualizar o servidor não for uma opção, você deve implantar um projeto da web que tem como alvo o ASP.NET 4 ou anterior em vez disso. Se você implantar um projeto de web anterior ou o ASP.NET 4 para o mesmo destino, selecione a **remover arquivos adicionais no destino** caixa de seleção a **configurações** guia do **publicar na Web**assistente. Se você não selecionar **remover arquivos adicionais no destino**, você continuará para a página de erro de configuração.

O projeto **propriedades** windows inclui uma lista de lista suspensa de framework de destino, mas você não pode resolver esse problema alterando apenas do **.NET Framework 4.5** para **do.NETFramework4**. Se você alterar a estrutura de destino para uma versão anterior do framework, o projeto ainda terão referências aos assemblies do framework versão posterior e não será executado. Você precisa alterar essas referências manualmente ou criar um novo projeto que tem como alvo o .NET Framework 4 ou anterior. Para obter mais informações, consulte [.NET Framework Targeting para Sites da Web](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx).

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
