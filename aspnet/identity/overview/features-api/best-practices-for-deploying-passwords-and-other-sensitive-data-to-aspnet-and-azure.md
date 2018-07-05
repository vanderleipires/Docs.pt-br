---
uid: identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
title: Práticas recomendadas para implantar senhas e outros dados confidenciais no ASP.NET e o serviço de aplicativo do Azure | Microsoft Docs
author: Rick-Anderson
description: Este tutorial mostra como o seu código com segurança pode armazenar e acessar informações seguras. O ponto mais importante é que você nunca deve armazenar senhas ou outros sen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2015
ms.topic: article
ms.assetid: 97902c66-cb61-4d11-be52-73f962f2db0a
ms.technology: ''
msc.legacyurl: /identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
msc.type: authoredcontent
ms.openlocfilehash: 8434c439cef7e30ddd45b78bb0bca5e4daeceaff
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371003"
---
<a name="best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure-app-service"></a>Práticas recomendadas para implantar senhas e outros dados confidenciais no ASP.NET e o serviço de aplicativo do Azure
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial mostra como o seu código com segurança pode armazenar e acessar informações seguras. O ponto mais importante é que você nunca deve armazenar senhas ou outros dados confidenciais no código-fonte, e você não deve usar os segredos de produção no modo de desenvolvimento e teste.
> 
> O código de exemplo é um aplicativo de console simples do trabalho Web e um aplicativo ASP.NET MVC que precisa acessar um banco de dados conexão cadeia de caracteres senha, Twilio, Google e SendGrid as chaves seguras.
> 
> No local configurações e o PHP também é mencionado.


- [Trabalhando com senhas no ambiente de desenvolvimento](#pwd)
- [Trabalhando com cadeias de caracteres de conexão no ambiente de desenvolvimento](#con)
- [Aplicativos de console do WebJobs](#wj)
- [Implantação de segredos no Azure](#da)
- [Notas para o local e o PHP](#not)
- [Recursos adicionais](#addRes)

<a id="pwd"></a>
## <a name="working-with-passwords-in-the-development-environment"></a>Trabalhando com senhas no ambiente de desenvolvimento

Os tutoriais frequentemente mostram dados confidenciais no código-fonte, espera-se com uma limitação que você nunca deve armazenar dados confidenciais no código-fonte. Por exemplo, minha [aplicativo ASP.NET MVC 5 com SMS e email 2FA](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md) tutorial mostra o seguinte na *Web. config* arquivo:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample1.xml)]

O *Web. config* arquivo é o código-fonte, para que esses segredos nunca devem ser armazenados nesse arquivo. Felizmente, o `<appSettings>` elemento tem um `file` atributo que permite que você especifique um arquivo externo que contém definições de configuração de aplicativo confidenciais. Você pode mover todos os seus segredos para um arquivo externo, desde que o arquivo externo não é verificado em sua árvore de origem. Por exemplo, na marcação a seguir, o arquivo *AppSettingsSecrets.config* contém todos os segredos de aplicativo:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample2.xml)]

A marcação no arquivo externo (*AppSettingsSecrets.config* neste exemplo), é a mesma marcação encontrada na *Web. config* arquivo:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample3.xml)]

O tempo de execução do ASP.NET mescla o conteúdo do arquivo externo com a marcação na &lt;appSettings&gt; elemento. O tempo de execução ignora o atributo de arquivo se o arquivo especificado não pode ser encontrado.

> [!WARNING]
> Security - não adicione suas *. config de segredos* arquivo ao seu projeto ou verificá-lo no controle de origem. Por padrão, o Visual Studio define o `Build Action` para `Content`, que significa que o arquivo é implantado. Para obter mais informações, consulte [por que não todos os arquivos na pasta Meu projeto são implantados?](https://msdn.microsoft.com/library/ee942158(v=vs.110).aspx#can_i_exclude_specific_files_or_folders_from_deployment) Embora você possa usar qualquer extensão para o *. config de segredos* arquivo, é melhor para mantê-lo *. config*, como arquivos de configuração não são realizados pelo IIS. Observe também que o *AppSettingsSecrets.config* arquivo é de dois níveis de diretório acima da *Web. config* arquivo, para que ele está completamente fora do diretório da solução. Movendo o arquivo fora do diretório da solução, &quot;adicionar git \* &quot; não adicioná-lo ao seu repositório.


<a id="con"></a>
## <a name="working-with-connection-strings-in-the-development-environment"></a>Trabalhando com cadeias de caracteres de conexão no ambiente de desenvolvimento

O Visual Studio cria novos projetos do ASP.NET que usam [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx). LocalDB foi criado especificamente para o ambiente de desenvolvimento. Ele não requer uma senha, portanto você não precisa fazer nada para impedir que os segredos que está sendo verificado em seu código-fonte. Algumas equipes de desenvolvimento usam as versões completas do SQL Server (ou outro DBMS) que exigem uma senha.

Você pode usar o `configSource` atributo para substituir todo o `<connectionStrings>` marcação. Ao contrário do `<appSettings>` `file` atributo que mescla a marcação, o `configSource` atributo substitui a marcação. A marcação a seguir mostra a `configSource` atributo o *Web. config* arquivo:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample4.xml?highlight=1)]

> [!NOTE]
> Se você usar o `configSource` atributo conforme mostrado acima para mover suas cadeias de caracteres de conexão para um arquivo externo e tiver o Visual Studio a criar um novo site, ele não será capaz de detectar você estiver usando um banco de dados, e você não terá a opção de configurar o banco de dados quando você pu Publicar no Azure do Visual Studio. Se você estiver usando o `configSource` atributo, você pode usar o PowerShell para criar e implantar seu site da web e banco de dados, ou você pode criar o site da web e o banco de dados no portal antes de publicar. O [New-AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) script criará um novo site da web e o banco de dados.


> [!WARNING]
> Segurança – ao contrário o *AppSettingsSecrets.config* arquivo, o arquivo de cadeias de caracteres de conexão externa deve estar no mesmo diretório raiz *Web. config* arquivo, portanto, você precisará tomar precauções para garantir que você não verificá-lo no seu repositório de origem.


> [!NOTE]
> **Aviso de segurança no arquivo de segredos:** uma prática recomendada é não usar segredos de produção em desenvolvimento e teste. Uso de senhas de produção em teste ou desenvolvimento vazamentos desses segredos.


<a id="wj"></a>
## <a name="webjobs-console-apps"></a>Aplicativos de console do WebJobs

O *App. config* arquivo usado por um aplicativo de console não dá suporte a caminhos relativos, mas oferece suporte a caminhos absolutos. Você pode usar um caminho absoluto para mover seus segredos fora do diretório do projeto. A marcação a seguir mostra os segredos na *C:\secrets\AppSettingsSecrets.config* arquivo e dados não confidenciais na *App. config* arquivo.

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample5.xml?highlight=2)]

<a id="da"></a>
## <a name="deploying-secrets-to-azure"></a>Implantação de segredos no Azure

Quando você implanta seu aplicativo web do Azure, o *AppSettingsSecrets.config* arquivo não será implantado (ou seja, o que você deseja). Você pode ir para o [Portal de gerenciamento](https://azure.microsoft.com/services/management-portal/) e defini-los manualmente, para fazer isso:

1. Vá para [ https://portal.azure.com ](https://portal.azure.com)e entre com suas credenciais do Azure.
2. Clique em **navegue &gt; aplicativos Web**, em seguida, clique no nome do seu aplicativo web.
3. Clique em **todas as configurações &gt; configurações do aplicativo**.

O **configurações do aplicativo** e **cadeia de caracteres de conexão** valores substituem as mesmas configurações no *Web. config* arquivo. Em nosso exemplo, podemos não tiver implantado essas configurações para o Azure, mas se essas chaves estivessem na *Web. config* arquivo, as configurações mostradas no portal teria precedência.

Uma prática recomendada é seguir um [fluxo de trabalho DevOps](../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything.md) e use [do Azure PowerShell](https://azure.microsoft.com/documentation/articles/install-configure-powershell/) (ou outra estrutura, como [Chef](http://www.opscode.com/chef/) ou [Puppet](http://puppetlabs.com/puppet/what-is-puppet)) para Automatize a definir esses valores no Azure. Usa o seguinte script do PowerShell [Export-CliXml](http://www.powershellcookbook.com/recipe/PukO/securely-store-credentials-on-disk) para exportar os segredos criptografados em disco:

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample6.ps1)]

No script acima, 'Name' é o nome da chave secreta, tais como '&quot;FB\_AppSecret&quot; ou "TwitterSecret". Você pode exibir o arquivo ".credential" criado pelo script no seu navegador. O trecho a seguir testa cada um dos arquivos de credencial e define os segredos do aplicativo web nomeados:

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample7.ps1)]

> [!WARNING]
> Security - não incluem senhas ou outros segredos no script do PowerShell, fazendo derrotas caso a finalidade do uso de um script do PowerShell para implantar dados confidenciais. O [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) cmdlet fornece um mecanismo seguro para obter uma senha. Usando um prompt de interface do usuário pode impedir o vazamento de uma senha.


### <a name="deploying-db-connection-strings"></a>Implantação de cadeias de caracteres de conexão de banco de dados

Cadeias de caracteres de conexão de banco de dados são tratadas da mesma forma para as configurações do aplicativo. Se você implantar seu aplicativo web do Visual Studio, a cadeia de caracteres de conexão será configurada para você. Você pode verificar isso no portal. É a maneira recomendada para definir a cadeia de conexão com o PowerShell. Para obter um exemplo de um script do PowerShell a cria um site e um banco de dados e define a cadeia de caracteres de conexão no site de download [New-AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) da [biblioteca de Script do Azure](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure).

<a id="not"></a>
## <a name="notes-for-php"></a>Notas do PHP

Desde os pares chave-valor para ambos **configurações do aplicativo** e **cadeias de caracteres de conexão** são armazenados em variáveis de ambiente no serviço de aplicativo do Azure, os desenvolvedores que usam qualquer can de estruturas (como PHP) do aplicativo web com facilidade recupere esses valores. Consulte de Stefan Schackow [Sites do Windows Azure: como cadeias de caracteres de aplicativo e o trabalho de cadeias de caracteres de Conexão](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/) postagem de blog que mostra um trecho de código PHP para ler as configurações do aplicativo e cadeias de caracteres de conexão.

## <a name="notes-for-on-premises-servers"></a>Observações para os servidores no local

Se você estiver implantando em servidores da web no local, você pode ajudar a proteger segredos por [criptografar seções de configuração dos arquivos de configuração](https://msdn.microsoft.com/library/ff647398.aspx). Como alternativa, você pode usar a mesma abordagem recomendada para sites do Azure: Manter configurações de desenvolvimento em arquivos de configuração e usar valores de variáveis de ambiente para as configurações de produção. Nesse caso, no entanto, você precisa escrever o código do aplicativo para a funcionalidade que é automático nos sites do Azure: recuperar as configurações de variáveis de ambiente e usar esses valores no lugar do arquivo de configuração ou usar o arquivo de configuração quando variáveis de ambiente não foram encontradas.

<a id="addRes"></a>
## <a name="additional-resources"></a>Recursos adicionais

Para obter um exemplo de um PowerShell script que cria um aplicativo web + banco de dados, define a cadeia de caracteres de conexão + configurações do aplicativo, download [New-AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) da [biblioteca de Script do Azure](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure). 

Consulte de Stefan Schackow [Windows Azure Web Sites: como cadeias de caracteres de aplicativo e Conexão cadeias de caracteres de trabalho](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/)


Agradecemos Barry Dorrans ( [ @blowdart ](https://twitter.com/blowdart) ) e Carlos Farre revisão.
