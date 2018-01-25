---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-cs
title: "Protegendo cadeias de caracteres de Conexão e outras informações de configuração (c#) | Microsoft Docs"
author: rick-anderson
description: "Normalmente, um aplicativo ASP.NET armazena informações de configuração em um arquivo Web. config. Algumas dessas informações são confidenciais e garante a proteção. Por def..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: ad8dd396-30f7-4abe-ac02-a0b84422e5be
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-cs
msc.type: authoredcontent
ms.openlocfilehash: e3782e3d4acc2db0e744128dad64fdfae1e8766d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="protecting-connection-strings-and-other-configuration-information-c"></a>Protegendo cadeias de caracteres de Conexão e outras informações de configuração (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_73_CS.zip) ou [baixar PDF](protecting-connection-strings-and-other-configuration-information-cs/_static/datatutorial73cs1.pdf)

> Normalmente, um aplicativo ASP.NET armazena informações de configuração em um arquivo Web. config. Algumas dessas informações são confidenciais e garante a proteção. Por padrão este arquivo não será servido para um visitante de site da Web, mas um administrador ou um hacker pode obter acesso ao sistema de arquivos do servidor Web e exibir o conteúdo do arquivo. Neste tutorial, saiba que o ASP.NET 2.0 permite que nós proteger informações confidenciais criptografando seções do arquivo Web. config.


## <a name="introduction"></a>Introdução

Informações de configuração para aplicativos ASP.NET geralmente são armazenadas em um arquivo XML denominado `Web.config`. Ao longo destes tutoriais atualizamos o `Web.config` algumas vezes. Ao criar o `Northwind` conjunto de dados tipado no [primeiro tutorial](../introduction/creating-a-data-access-layer-cs.md), por exemplo, informações de cadeia de caracteres de conexão foi adicionadas automaticamente ao `Web.config` no `<connectionStrings>` seção. Posteriormente, o [páginas mestras e navegação de Site](../introduction/master-pages-and-site-navigation-cs.md) tutorial, estamos atualizado manualmente `Web.config`, adicionando um `<pages>` elemento que indica que todas as páginas ASP.NET em nosso projeto devem usar o `DataWebControls` tema.

Como `Web.config` pode conter dados confidenciais, como cadeias de caracteres de conexão, é importante que o conteúdo de `Web.config` ficar oculto dos visualizadores não autorizados e seguro. Por padrão, qualquer HTTP solicitações para um arquivo com o `.config` extensão é tratada pelo mecanismo ASP.NET, que retorna o *esse tipo de página não é atendido* mensagem mostrada na Figura 1. Isso significa que os visitantes não é possível exibir seu `Web.config` s o conteúdo do arquivo simplesmente digitando http://www.YourServer.com/Web.config na sua barra de endereços do navegador s.


[![Visitar o Web. config por meio de um navegador retorna desse tipo de página não é atendido mensagem](protecting-connection-strings-and-other-configuration-information-cs/_static/image2.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image1.png)

**Figura 1**: visitando `Web.config` por meio de um navegador retorna desse tipo de página não é atendida mensagem ([clique para exibir a imagem em tamanho normal](protecting-connection-strings-and-other-configuration-information-cs/_static/image3.png))


Mas e se um invasor for capaz de encontrar alguns outros exploração que permite a ela exibir seu `Web.config` s conteúdo do arquivo? O que um invasor pode fazer com essas informações e quais etapas podem ser tomadas para proteger ainda mais as informações confidenciais em `Web.config`? Felizmente, a maioria das seções `Web.config` não contêm informações confidenciais. Que mal um invasor pode cometem se souberem o nome do tema usado por suas páginas ASP.NET padrão?

Determinados `Web.config` seções, no entanto, contêm informações confidenciais que podem incluir cadeias de caracteres de conexão, nomes de usuário, senhas, nomes de servidor, chaves de criptografia e assim por diante. Essas informações normalmente são encontradas no seguinte `Web.config` seções:

- `<appSettings>`
- `<connectionStrings>`
- `<identity>`
- `<sessionState>`

Este tutorial examinaremos técnicas para proteger essas informações de configuração confidenciais. Como veremos, o .NET Framework versão 2.0 inclui um sistema de configurações protegidas que torna muito fácil de programaticamente criptografando e descriptografando seções de configuração selecionada.

> [!NOTE]
> Este tutorial conclui examinando as recomendações s da Microsoft para se conectar a um banco de dados de um aplicativo ASP.NET. Além de criptografar cadeias de caracteres de conexão, você pode ajudar a proteger seu sistema, garantindo que você está se conectando ao banco de dados de forma segura.


## <a name="step-1-exploring-aspnet-20-s-protected-configuration-options"></a>Etapa 1: Explorar o ASP.NET 2.0 s protegido opções de configuração

ASP.NET 2.0 inclui um sistema de configuração protegida para criptografar e descriptografar as informações de configuração. Isso inclui métodos do .NET Framework que pode ser usado para criptografar ou descriptografar informações de configuração de programaticamente. O sistema de configuração protegida utiliza o [modelo de provedor](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), que permite aos desenvolvedores escolher qual implementação de criptografia é usada.

O .NET Framework vem com dois provedores de configuração protegida:

- [`RSAProtectedConfigurationProvider`](https://msdn.microsoft.com/library/system.configuration.rsaprotectedconfigurationprovider.aspx)-usa assimétrica [algoritmo RSA](http://en.wikipedia.org/wiki/Rsa) para criptografia e descriptografia.
- [`DPAPIProtectedConfigurationProvider`](https://msdn.microsoft.com/system.configuration.dpapiprotectedconfigurationprovider.aspx)-usa o Windows [API de proteção de dados (DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx) para criptografia e descriptografia.

Como o sistema de configuração protegida implementa o padrão de design de provedor, é possível criar seu próprio provedor de configuração protegida e conectá-lo ao seu aplicativo. Consulte [implementando um provedor de configuração protegida](https://msdn.microsoft.com/library/wfc2t3az(VS.80).aspx) para obter mais informações sobre esse processo.

Os provedores de RSA e DPAPI usam chaves para suas rotinas de criptografia e descriptografia, e essas chaves podem ser armazenadas no computador ou usuário-nível de. Informações criptografadas de chaves de nível de máquina são ideais para cenários em que o aplicativo web é executado em seu próprio servidor dedicado ou se houver vários aplicativos em um servidor que precisam compartilhar. Chaves de nível de usuário são uma opção mais segura em ambientes de hospedagem compartilhados, onde outros aplicativos no mesmo servidor não devem ser capazes de descriptografar seções de configuração de s protegido por seu aplicativo.

Neste tutorial nossos exemplos usará o provedor DPAPI e chaves de nível de máquina. Especificamente, examinaremos criptografando o `<connectionStrings>` seção `Web.config`, embora o sistema de configuração protegida pode ser usado para criptografar a maioria dos qualquer `Web.config` seção. Para obter informações sobre o uso de chaves de nível de usuário ou usando o provedor RSA, consulte os recursos na seção Leituras adicionais no final deste tutorial.

> [!NOTE]
> O `RSAProtectedConfigurationProvider` e `DPAPIProtectedConfigurationProvider` provedores são registrados no `machine.config` arquivo com os nomes de provedor `RsaProtectedConfigurationProvider` e `DataProtectionConfigurationProvider`, respectivamente. Ao criptografar ou descriptografar informações de configuração, será necessário fornecer o nome de provedor apropriado (`RsaProtectedConfigurationProvider` ou `DataProtectionConfigurationProvider`) em vez do nome do tipo real (`RSAProtectedConfigurationProvider` e `DPAPIProtectedConfigurationProvider`). Você pode encontrar o `machine.config` arquivo o `$WINDOWS$\Microsoft.NET\Framework\version\CONFIG` pasta.


## <a name="step-2-programmatically-encrypting-and-decrypting-configuration-sections"></a>Etapa 2: Programaticamente criptografando e descriptografando seções de configuração

Com algumas linhas de código é criptografar ou descriptografar uma seção de configuração específico usando um provedor especificado. O código, como veremos em breve, simplesmente precisa programaticamente, consulte a seção de configuração apropriado, chame seu `ProtectSection` ou `UnprotectSection` método e, em seguida, chame o `Save` método para manter as alterações. Além disso, o .NET Framework inclui um utilitário de linha de comando útil que pode criptografar e descriptografar as informações de configuração. Iremos explorar esse utilitário de linha de comando na etapa 3.

Para ilustrar as informações de configuração de proteção por meio de programação, permitem s criar uma página ASP.NET que inclui botões para criptografar e descriptografar o `<connectionStrings>` seção `Web.config`.

Comece abrindo o `EncryptingConfigSections.aspx` página o `AdvancedDAL` pasta. Arraste um controle de caixa de texto da caixa de ferramentas para o Designer, definindo seu `ID` propriedade `WebConfigContents`, sua `TextMode` propriedade `MultiLine`e seu `Width` e `Rows` propriedades 95% e 15, respectivamente. Esse controle de caixa de texto exibirá o conteúdo de `Web.config` que permite ver rapidamente se o conteúdo é criptografado ou não. É claro que, em um aplicativo real você nunca deseja exibir o conteúdo de `Web.config`.

Sob a caixa de texto, adicione dois controles de botão chamados `EncryptConnStrings` e `DecryptConnStrings`. Defina suas propriedades de texto para cadeias de caracteres de Conexão de criptografar e descriptografar cadeias de caracteres de Conexão.

Neste ponto, sua tela deve ser semelhante a Figura 2.


[![Adicione uma caixa de texto e dois controles de Web do botão à página](protecting-connection-strings-and-other-configuration-information-cs/_static/image5.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image4.png)

**Figura 2**: adicionar uma caixa de texto e dois controles de Web do botão à página ([clique para exibir a imagem em tamanho normal](protecting-connection-strings-and-other-configuration-information-cs/_static/image6.png))


Em seguida, é preciso escrever código que carrega e exibe o conteúdo de `Web.config` no `WebConfigContents` carregado da caixa de texto quando a página é primeiro. Adicione o seguinte código para a classe de code-behind de s de página. Esse código adiciona um método chamado `DisplayWebConfig` e chamá-lo a partir de `Page_Load` manipulador de eventos quando `Page.IsPostBack` é `false`:


[!code-csharp[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample1.cs)]

O `DisplayWebConfig` método usa o [ `File` classe](https://msdn.microsoft.com/library/system.io.file.aspx) para abrir o aplicativo s `Web.config` arquivo, o [ `StreamReader` classe](https://msdn.microsoft.com/library/system.io.streamreader.aspx) para ler seu conteúdo em uma cadeia de caracteres e o [ `Path` classe](https://msdn.microsoft.com/library/system.io.path.aspx) para gerar o caminho físico para o `Web.config` arquivo. Todos esses três classes são encontradas no [ `System.IO` namespace](https://msdn.microsoft.com/library/system.io.aspx). Consequentemente, você precisará adicionar um `using` `System.IO` à parte superior da classe code-behind ou, Alternativamente, esses nomes de classe de prefixo `System.IO.` .

Em seguida, precisamos adicionar manipuladores de eventos para os dois controles de botão `Click` eventos e adicione o código necessário para criptografar e descriptografar o `<connectionStrings>` seção usando uma chave de nível de máquina com o provedor DPAPI. No Designer, clique duas vezes em cada um dos botões para adicionar um `Click` manipulador de eventos no code-behind de classe e, em seguida, adicione o seguinte código:


[!code-csharp[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample2.cs)]

O código usado nos manipuladores de eventos de dois é quase idêntico. Ambos começar Obtendo informações sobre o aplicativo atual `Web.config` de arquivos por meio de [ `WebConfigurationManager` classe](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.aspx) s [ `OpenWebConfiguration` método](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.openwebconfiguration.aspx). Esse método retorna o arquivo de configuração da web para o caminho virtual especificado. Em seguida, o `Web.config` arquivo s `<connectionStrings>` seção é acessada por meio de [ `Configuration` classe](https://msdn.microsoft.com/library/system.configuration.configuration.aspx) s [ `GetSection(sectionName)` método](https://msdn.microsoft.com/library/system.configuration.configuration.getsection.aspx), que retorna um [ `ConfigurationSection` ](https://msdn.microsoft.com/library/system.configuration.configurationsection.aspx) objeto.

O `ConfigurationSection` objeto inclui um [ `SectionInformation` propriedade](https://msdn.microsoft.com/library/system.configuration.configurationsection.sectioninformation.aspx) que fornece informações adicionais e funcionalidades em relação a seção de configuração. Como o código acima mostra, é possível determinar se a seção de configuração é criptografada, verificando o `SectionInformation` propriedade s `IsProtected` propriedade. Além disso, a seção pode ser criptografada ou descriptografada por meio de `SectionInformation` propriedade s `ProtectSection(provider)` e `UnprotectSection` métodos.

O `ProtectSection(provider)` método o aceita como entrada uma cadeia de caracteres especificando o nome do provedor de configuração protegida para usar ao criptografar. No `EncryptConnString` manipulador de eventos do botão s passamos DataProtectionConfigurationProvider para o `ProtectSection(provider)` método para que o provedor DPAPI é usado. O `UnprotectSection` método pode determinar o provedor que foi usado para criptografar a seção de configuração e, portanto, não requer parâmetros de entrada.

Depois de chamar o `ProtectSection(provider)` ou `UnprotectSection` método, você deve chamar o `Configuration` objeto s [ `Save` método](https://msdn.microsoft.com/library/system.configuration.configuration.save.aspx) para manter as alterações. Depois que as informações de configuração foi criptografadas ou descriptografadas e as alterações salvadas, podemos chamar `DisplayWebConfig` para carregar o atualizado `Web.config` conteúdo no controle de caixa de texto.

Depois que você inseriu o código acima, testá-lo visitando o `EncryptingConfigSections.aspx` página através de um navegador. Inicialmente, você verá uma página que lista o conteúdo de `Web.config` com o `<connectionStrings>` seção exibida em texto sem formatação (consulte a Figura 3).


[![Adicione uma caixa de texto e dois controles de Web do botão à página](protecting-connection-strings-and-other-configuration-information-cs/_static/image8.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image7.png)

**Figura 3**: adicionar uma caixa de texto e dois controles de Web do botão à página ([clique para exibir a imagem em tamanho normal](protecting-connection-strings-and-other-configuration-information-cs/_static/image9.png))


Agora, clique no botão de criptografar cadeias de caracteres de Conexão. Se a validação de solicitação estiver habilitada, a marcação postada a partir de `WebConfigContents` TextBox produzirá um `HttpRequestValidationException`, que exibe a mensagem, potencialmente perigosa `Request.Form` valor foi detectado no cliente. Validação de solicitação, que é habilitada por padrão no ASP.NET 2.0, proíbe postbacks que incluem o HTML não codificado e foi projetada para ajudar a evitar ataques de injeção de script. Essa verificação pode ser desabilitada no nível de página como ou aplicativo. Para desativá-lo para essa página, defina o `ValidateRequest` definindo como `false` no `@Page` diretiva. O `@Page` diretiva for encontrada na parte superior da marcação declarativa s de página.


[!code-aspx[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample3.aspx)]

Para obter mais informações sobre a validação de solicitação, sua finalidade, como desativá-lo no nível da página - e aplicativo-, como bem como para HTML codificar marcação, consulte [validação de solicitação - impedindo ataques de Script](../../../../whitepapers/request-validation.md).

Depois de desabilitar a validação de solicitação para a página, tente clicar novamente no botão criptografar cadeias de caracteres de Conexão. Em um postback, o arquivo de configuração será acessado e sua `<connectionStrings>` seção criptografada usando o provedor DPAPI. A caixa de texto, em seguida, é atualizada para exibir o novo `Web.config` conteúdo. Como mostra a Figura 4, o `<connectionStrings>` agora as informações são criptografadas.


[![Clicando na criptografar Conexão cadeias de caracteres botão criptografa o &lt;connectionString&gt; seção](protecting-connection-strings-and-other-configuration-information-cs/_static/image11.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image10.png)

**Figura 4**: clicar a criptografar Conexão cadeias de caracteres botão criptografa o `<connectionString>` seção ([clique para exibir a imagem em tamanho normal](protecting-connection-strings-and-other-configuration-information-cs/_static/image12.png))


Criptografada `<connectionStrings>` segue seção gerada no meu computador, embora o conteúdo a `<CipherData>` elemento tiver sido removido para resumir:


[!code-xml[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample4.xml)]

> [!NOTE]
> O `<connectionStrings>` elemento Especifica o provedor usado para realizar a criptografia (`DataProtectionConfigurationProvider`). Essas informações são usadas pelo `UnprotectSection` método quando clicar no botão de descriptografar as cadeias de caracteres de Conexão.


Quando as informações de cadeia de caracteres de conexão são acessadas de `Web.config` - o código que escrevemos, de um controle SqlDataSource, ou o código gerado automaticamente de TableAdapters em nosso DataSets tipados - é automaticamente descriptografada. Em suma, não precisamos adicionar qualquer código extra ou lógica para descriptografar o criptografados `<connectionString>` seção. Para demonstrar isso, consulte um dos tutoriais anteriores neste momento, como o tutorial de exibição simples da seção de relatórios básicos (`~/BasicReporting/SimpleDisplay.aspx`). Como mostra a Figura 5, o tutorial funciona exatamente como seria esperado, indicando que as informações de cadeia de caracteres de conexão criptografada está sendo descriptografadas automaticamente pela página ASP.NET.


[![Camada de acesso a dados descriptografa automaticamente as informações de cadeia de caracteres de Conexão](protecting-connection-strings-and-other-configuration-information-cs/_static/image14.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image13.png)

**Figura 5**: camada de acesso a dados descriptografa automaticamente as informações de cadeia de caracteres de Conexão ([clique para exibir a imagem em tamanho normal](protecting-connection-strings-and-other-configuration-information-cs/_static/image15.png))


Para reverter o `<connectionStrings>` seção sua representação de texto sem formatação, clique no botão de descriptografar as cadeias de caracteres de Conexão. Em um postback, você deve ver as cadeias de conexão `Web.config` em texto sem formatação. Neste ponto, sua tela deve ser parecido com quando visitar primeiro nessa página (consulte a Figura 3).

## <a name="step-3-encrypting-configuration-sections-usingaspnetregiisexe"></a>Etapa 3: Criptografar seções de configuração usando`aspnet_regiis.exe`

O .NET Framework inclui uma variedade de ferramentas de linha de comando do `$WINDOWS$\Microsoft.NET\Framework\version\` pasta. No [dependências de Cache de SQL usando](../caching-data/using-sql-cache-dependencies-cs.md) tutorial, por exemplo, analisamos usando o `aspnet_regsql.exe` ferramenta de linha de comando para adicionar a infraestrutura necessária para dependências de cache SQL. Outra ferramenta de linha de comando útil nessa pasta é o [ferramenta de registro de IIS do ASP.NET (`aspnet_regiis.exe`)](https://msdn.microsoft.com/library/k6h9cz8h(VS.80).aspx). Como o nome sugere, a ferramenta de registro do ASP.NET IIS é usada principalmente para registrar um aplicativo ASP.NET 2.0 no servidor de Web do Microsoft s nível profissional, IIS. Além de seus recursos relacionados ao IIS, a ferramenta de registro do ASP.NET IIS também pode ser usada para criptografar ou descriptografar seções de configuração especificado no `Web.config`.

A instrução a seguir mostra a sintaxe geral usada para criptografar uma seção de configuração com o `aspnet_regiis.exe` ferramenta de linha de comando:


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample5.cmd)]

*seção* é a seção de configuração para criptografar (como connectionStrings), o *físico\_diretório* é o caminho completo, físico para o diretório raiz de aplicativo s de web, e *provedor*  é o nome do provedor de configuração protegida para usar (como DataProtectionConfigurationProvider). Como alternativa, se o aplicativo web estiver registrado no IIS, você pode inserir o caminho virtual em vez do caminho físico usando a seguinte sintaxe:


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample6.cmd)]

O seguinte `aspnet_regiis.exe` exemplo criptografa o `<connectionStrings>` seção usando o provedor DPAPI com uma chave de nível de máquina:


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample7.cmd)]

Da mesma forma, o `aspnet_regiis.exe` ferramenta de linha de comando pode ser usada para descriptografar as seções de configuração. Em vez de usar o `-pef` alternar, use `-pdf` (ou, em vez de `-pe`, use `-pd`). Além disso, observe que o nome do provedor não é necessário ao descriptografar.


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample8.cmd)]

> [!NOTE]
> Como estamos usando o provedor DPAPI, que usa chaves específicas do computador, você deve executar `aspnet_regiis.exe` no mesmo computador do qual as páginas da web estão sendo atendidas. Por exemplo, se você executar este programa de linha de comando de sua máquina de desenvolvimento local e, em seguida, carregar o arquivo Web. config criptografado para o servidor de produção, o servidor de produção não será capaz de descriptografar as informações de cadeia de caracteres de conexão desde que foi criptografado usando chaves específicas para sua máquina de desenvolvimento. O provedor RSA não têm esta limitação porque é possível exportar as chaves RSA para outro computador.


## <a name="understanding-database-authentication-options"></a>Noções básicas sobre opções de autenticação de banco de dados

Antes de qualquer aplicativo pode emitir `SELECT`, `INSERT`, `UPDATE`, ou `DELETE` consultas para um banco de dados do Microsoft SQL Server, o banco de dados primeiro devem identificar o solicitante. Esse processo é conhecido como *autenticação* e SQL Server fornece dois métodos de autenticação:

- **Autenticação do Windows** -o processo sob a qual o aplicativo está em execução é usado para se comunicar com o banco de dados. Ao executar um aplicativo ASP.NET no Visual Studio 2005 s ASP.NET Development Server, o aplicativo ASP.NET assume a identidade do usuário conectado no momento. Para aplicativos ASP.NET no Microsoft Internet Information Server (IIS), aplicativos ASP.NET normalmente assumir a identidade de `domainName``\MachineName` ou `domainName``\NETWORK SERVICE`, embora isso pode ser personalizado.
- **Autenticação do SQL** -um valores de ID e a senha do usuário são fornecidos como credenciais para autenticação. Com a autenticação do SQL, a ID de usuário e senha são fornecidos na cadeia de conexão.

Autenticação do Windows é preferencial em relação à autenticação do SQL porque é mais seguro. Com a autenticação do Windows a cadeia de caracteres de conexão é livre de um nome de usuário e uma senha e se o servidor web e o servidor de banco de dados residirem em dois computadores diferentes, as credenciais não são enviadas pela rede em texto sem formatação. Com a autenticação do SQL, no entanto, as credenciais de autenticação são codificados na cadeia de conexão e são transmitidas do servidor web para o servidor de banco de dados em texto sem formatação.

Esses tutoriais usam a autenticação do Windows. Você pode determinar qual modo de autenticação está sendo usado inspecionando a cadeia de caracteres de conexão. A cadeia de caracteres de conexão em `Web.config` para nossos tutoriais foi:

`Data Source=.\SQLEXPRESS; AttachDbFilename=|DataDirectory|\NORTHWND.MDF; Integrated Security=True; User Instance=True`

A segurança integrada = True e a falta de um nome de usuário e senha indicam que a autenticação do Windows está sendo usada. Em alguns conexão cadeias de caracteres o termo Conexão confiável = Yes ou segurança integrada = SSPI é usado em vez da segurança integrada = True, mas todos os três indicam o uso da autenticação do Windows.

O exemplo a seguir mostra uma cadeia de caracteres de conexão que usa a autenticação do SQL. Observe que as credenciais inseridas na cadeia de caracteres de conexão:

`Server=serverName; Database=Northwind; uid=userID; pwd=password`

Imagine que um invasor for capaz de exibir o aplicativo s `Web.config` arquivo. Se você usar autenticação do SQL para se conectar a um banco de dados que seja acessível pela Internet, o invasor pode usar essa cadeia de caracteres de conexão para se conectar ao banco de dados SQL Management Studio ou em páginas do ASP.NET em seu próprio site. Para ajudar a reduzir essa ameaça, criptografar as informações de cadeia de caracteres de conexão em `Web.config` usando o sistema de configuração protegida.

> [!NOTE]
> Para obter mais informações sobre os diferentes tipos de autenticação disponíveis no SQL Server, consulte [Building Secure ASP.NET Applications: autenticação, autorização e comunicação segura](https://msdn.microsoft.com/library/aa302392.aspx). Para obter conexão cadeia exemplos que ilustram as diferenças entre a sintaxe de autenticação do Windows e do SQL, consulte [ConnectionStrings.com](http://www.connectionstrings.com/).


## <a name="summary"></a>Resumo

Por padrão, arquivos com um `.config` extensão em um aplicativo ASP.NET não pode ser acessado por meio de um navegador. Esses tipos de arquivos não são retornados porque eles podem conter informações confidenciais, como cadeias de conexão de banco de dados, nomes de usuário e senhas e assim por diante. O sistema de configuração protegida no .NET 2.0 ajuda a proteger ainda mais as informações confidenciais, permitindo que as seções de configuração especificada sejam criptografados. Há dois provedores de configuração protegida interno: um que usa o algoritmo RSA e outro que usa a API de proteção de dados Windows (DPAPI).

Neste tutorial vimos como criptografar e descriptografar as definições de configuração usando o provedor DPAPI. Isso pode ser feito por meio de programação, conforme vimos na etapa 2, bem como por meio de `aspnet_regiis.exe` ferramenta de linha de comando, que foi abordada na etapa 3. Para obter mais informações sobre o uso de chaves de nível de usuário ou usando o provedor RSA em vez disso, consulte os recursos na seção de leitura adicional.

Boa programação!

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Criando aplicativo do ASP.NET seguros: Autenticação, autorização e comunicação segura](https://msdn.microsoft.com/library/aa302392.aspx)
- [Criptografando informações de configuração no ASP.NET 2.0 aplicativos](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [Criptografar `Web.config` valores no ASP.NET 2.0](https://weblogs.asp.net/scottgu/archive/2006/01/09/434893.aspx)
- [Como: Criptografar seções de configuração no ASP.NET 2.0 usando DPAPI](https://msdn.microsoft.com/library/ms998280.aspx)
- [Como: Criptografar seções de configuração no ASP.NET 2.0 usando o RSA](https://msdn.microsoft.com/library/ms998283.aspx)
- [A API de configuração no .NET 2.0](http://www.odetocode.com/Articles/418.aspx)
- [Proteção de dados do Windows](https://msdn.microsoft.com/library/ms995355.aspx)

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisores levar para este tutorial foram Teresa Murphy e Randy Schmidt. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs.md)
[Próximo](debugging-stored-procedures-cs.md)
