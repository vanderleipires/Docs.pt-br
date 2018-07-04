---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-cs
title: Protegendo cadeias de caracteres de Conexão e outras informações de configuração (c#) | Microsoft Docs
author: rick-anderson
description: Normalmente, um aplicativo ASP.NET armazena informações de configuração em um arquivo Web. config. Algumas dessas informações são confidenciais e garante a proteção. Por def....
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: ad8dd396-30f7-4abe-ac02-a0b84422e5be
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-cs
msc.type: authoredcontent
ms.openlocfilehash: e5ce4459a1b62fe851ce8a4cae50aed798f0d508
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378969"
---
<a name="protecting-connection-strings-and-other-configuration-information-c"></a>Protegendo cadeias de caracteres de Conexão e outras informações de configuração (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_73_CS.zip) ou [baixar PDF](protecting-connection-strings-and-other-configuration-information-cs/_static/datatutorial73cs1.pdf)

> Normalmente, um aplicativo ASP.NET armazena informações de configuração em um arquivo Web. config. Algumas dessas informações são confidenciais e garante a proteção. Por padrão esse arquivo não será servido para um visitante de site da Web, mas um administrador ou um hacker pode obter acesso ao sistema de arquivos do servidor Web e exibir o conteúdo do arquivo. Neste tutorial, saiba que o ASP.NET 2.0 permite-na proteger informações confidenciais, criptografando seções do arquivo Web. config.


## <a name="introduction"></a>Introdução

Informações de configuração para aplicativos ASP.NET é geralmente são armazenadas em um arquivo XML denominado `Web.config`. Ao longo destes tutoriais, nós atualizamos o `Web.config` inúmeras vezes. Ao criar o `Northwind` digitado o conjunto de dados na [primeiro tutorial](../introduction/creating-a-data-access-layer-cs.md), por exemplo, informações de cadeia de caracteres de conexão foi adicionadas automaticamente ao `Web.config` no `<connectionStrings>` seção. Posteriormente, o [páginas mestras e navegação no Site](../introduction/master-pages-and-site-navigation-cs.md) tutorial, estamos atualizado manualmente `Web.config`, adicionar um `<pages>` elemento que indica que todas as páginas do ASP.NET em nosso projeto devem usar o `DataWebControls` tema.

Uma vez que `Web.config` pode conter dados confidenciais, como cadeias de caracteres de conexão, é importante que o conteúdo de `Web.config` sejam mantidos seguros e ocultos dos visualizadores não autorizados. Por padrão, qualquer HTTP de solicitação para um arquivo com o `.config` extensão é tratada pelo mecanismo ASP.NET, que retorna o *esse tipo de página não é atendido* mensagem mostrada na Figura 1. Isso significa que os visitantes não poderá ver sua `Web.config` s o conteúdo do arquivo, basta inserir http://www.YourServer.com/Web.config na sua barra de endereços do navegador s.


[![Visitar o Web. config por meio de um navegador retorna esse tipo de página não é atendido mensagem](protecting-connection-strings-and-other-configuration-information-cs/_static/image2.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image1.png)

**Figura 1**: visitando `Web.config` por meio de um navegador retorna esse tipo de página não é atendida mensagem ([clique para exibir a imagem em tamanho normal](protecting-connection-strings-and-other-configuration-information-cs/_static/image3.png))


Mas e se um invasor é capaz de encontrar alguns outra exploração que permite a ela exibir seu `Web.config` s conteúdo do arquivo? O que um invasor pode fazer com essas informações e quais etapas podem ser tomadas para proteger ainda mais as informações confidenciais dentro `Web.config`? Felizmente, a maioria das seções no `Web.config` não contêm informações confidenciais. Que mal um invasor pode cometem se souberem o nome do tema usado por suas páginas ASP.NET padrão?

Determinados `Web.config` seções, no entanto, contêm informações confidenciais que podem incluir cadeias de caracteres de conexão, nomes de usuário, senhas, nomes de servidor, chaves de criptografia e assim por diante. Essas informações geralmente são encontradas no seguinte `Web.config` seções:

- `<appSettings>`
- `<connectionStrings>`
- `<identity>`
- `<sessionState>`

Neste tutorial vamos examinar técnicas para proteger essas informações de configuração confidenciais. Como veremos, o .NET Framework versão 2.0 inclui um sistema de configurações protegidas que torna muito simples de seções de configuração selecionada de forma programática com criptografia e descriptografia.

> [!NOTE]
> Esse tutorial conclui com uma olhada em recomendações s da Microsoft para se conectar a um banco de dados de um aplicativo ASP.NET. Além de criptografar suas cadeias de caracteres de conexão, você pode ajudar a proteger seu sistema, garantindo que você está se conectando ao banco de dados de forma segura.


## <a name="step-1-exploring-aspnet-20-s-protected-configuration-options"></a>Etapa 1: Explorando o ASP.NET 2.0 s protegidas as opções de configuração

O ASP.NET 2.0 inclui um sistema de configuração protegida para criptografar e descriptografar as informações de configuração. Isso inclui métodos no .NET Framework que pode ser usado para criptografar ou descriptografar informações de configuração de forma programática. O sistema de configuração protegida usa o [modelo de provedor](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), que permite aos desenvolvedores escolher qual implementação de criptografia é usada.

O .NET Framework vem com dois provedores de configuração protegida:

- [`RSAProtectedConfigurationProvider`](https://msdn.microsoft.com/library/system.configuration.rsaprotectedconfigurationprovider.aspx) -usa assimétrica [algoritmo RSA](http://en.wikipedia.org/wiki/Rsa) para criptografia e descriptografia.
- [`DPAPIProtectedConfigurationProvider`](https://msdn.microsoft.com/system.configuration.dpapiprotectedconfigurationprovider.aspx) -usa o Windows [API de proteção de dados (DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx) para criptografia e descriptografia.

Uma vez que o sistema de configuração protegida implementa o padrão de design de provedor, é possível criar seu próprio provedor de configuração protegida e conectá-lo ao seu aplicativo. Ver [implementando um provedor de configuração protegida](https://msdn.microsoft.com/library/wfc2t3az(VS.80).aspx) para obter mais informações sobre esse processo.

Os provedores de RSA e DPAPI usam chaves para suas rotinas de criptografia e descriptografia, e essas chaves podem ser armazenadas no nível da máquina ou usuário. Informações criptografadas de chaves de nível de máquina são ideais para cenários em que o aplicativo web é executado em seu próprio servidor dedicado ou se houver vários aplicativos em um servidor que precisam compartilhar. As chaves de nível de usuário são uma opção mais segura em ambientes de hospedagem compartilhadas, onde outros aplicativos no mesmo servidor não devem ser capazes de descriptografar suas seções de configuração do aplicativo s protegidos.

Neste tutorial nossos exemplos usará o provedor DPAPI e chaves de nível de máquina. Especificamente, examinaremos criptografando as `<connectionStrings>` seção `Web.config`, embora o sistema de configuração protegida pode ser usado para criptografar a maioria dos qualquer `Web.config` seção. Para obter informações sobre o uso de chaves de nível de usuário ou usando o provedor RSA, consulte os recursos na seção Leituras adicionais no final deste tutorial.

> [!NOTE]
> O `RSAProtectedConfigurationProvider` e `DPAPIProtectedConfigurationProvider` provedores são registrados na `machine.config` arquivo com os nomes de provedor `RsaProtectedConfigurationProvider` e `DataProtectionConfigurationProvider`, respectivamente. Ao criptografar ou descriptografar informações de configuração, precisamos fornecer o nome de provedor apropriado (`RsaProtectedConfigurationProvider` ou `DataProtectionConfigurationProvider`) em vez do nome do tipo real (`RSAProtectedConfigurationProvider` e `DPAPIProtectedConfigurationProvider`). Você pode encontrar o `machine.config` arquivo o `$WINDOWS$\Microsoft.NET\Framework\version\CONFIG` pasta.


## <a name="step-2-programmatically-encrypting-and-decrypting-configuration-sections"></a>Etapa 2: Programaticamente criptografar e descriptografar as seções de configuração

Com algumas linhas de código podemos criptografar ou descriptografar uma seção de configuração específico usando um provedor especificado. O código, como veremos daqui a pouco, simplesmente precisa referenciar programaticamente a seção de configuração apropriado, chame seu `ProtectSection` ou `UnprotectSection` método e, em seguida, chame o `Save` método para manter as alterações. Além disso, o .NET Framework inclui um utilitário de linha de comando útil que pode criptografar e descriptografar as informações de configuração. Vamos explorar esse utilitário de linha de comando na etapa 3.

Para ilustrar por meio de programação de proteção de informações de configuração, let s criar uma página ASP.NET que inclui botões para criptografar e descriptografar o `<connectionStrings>` seção `Web.config`.

Comece abrindo o `EncryptingConfigSections.aspx` página o `AdvancedDAL` pasta. Arraste um controle de caixa de texto da caixa de ferramentas para o Designer, definindo sua `ID` propriedade para `WebConfigContents`, sua `TextMode` propriedade a ser `MultiLine`e sua `Width` e `Rows` propriedades a 95% e 15, respectivamente. Esse controle de caixa de texto exibirá o conteúdo de `Web.config` que nos permite ver rapidamente se o conteúdo é criptografado ou não. É claro que, em um aplicativo real seriam nunca deseja exibir o conteúdo de `Web.config`.

Sob a caixa de texto, adicione dois controles de botão chamados `EncryptConnStrings` e `DecryptConnStrings`. Defina suas propriedades de texto como cadeias de caracteres de Conexão criptografar e descriptografar cadeias de caracteres de Conexão.

Neste ponto, sua tela deve ser semelhante da Figura 2.


[![Adicione uma caixa de texto e dois controles de Web de botão à página](protecting-connection-strings-and-other-configuration-information-cs/_static/image5.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image4.png)

**Figura 2**: Adicione uma caixa de texto e dois controles de Web de botão à página ([clique para exibir a imagem em tamanho normal](protecting-connection-strings-and-other-configuration-information-cs/_static/image6.png))


Em seguida, precisamos escrever um código que carrega e exibe o conteúdo da `Web.config` no `WebConfigContents` carregado da caixa de texto quando a página é o primeira. Adicione o seguinte código para a classe de code-behind de s de página. Este código adiciona um método chamado `DisplayWebConfig` e chama-o do `Page_Load` manipulador de eventos quando `Page.IsPostBack` é `false`:


[!code-csharp[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample1.cs)]

O `DisplayWebConfig` usa o [ `File` classe](https://msdn.microsoft.com/library/system.io.file.aspx) para abrir o aplicativo s `Web.config` arquivo, o [ `StreamReader` classe](https://msdn.microsoft.com/library/system.io.streamreader.aspx) para ler seu conteúdo em uma cadeia de caracteres e o [ `Path` classe](https://msdn.microsoft.com/library/system.io.path.aspx) para gerar o caminho físico para o `Web.config` arquivo. Essas três classes são todos encontrados na [ `System.IO` namespace](https://msdn.microsoft.com/library/system.io.aspx). Consequentemente, você precisará adicionar um `using` `System.IO` instrução na parte superior da classe code-behind ou, Alternativamente, prefixo de nomes com essas classes `System.IO.` .

Em seguida, precisamos adicionar manipuladores de eventos para os dois controles de botão `Click` eventos e adicione o código necessário para criptografar e descriptografar o `<connectionStrings>` seção usando uma chave de nível de máquina com o provedor DPAPI. No Designer, clique duas vezes em cada um dos botões para adicionar um `Click` manipulador de eventos no code-behind de classe e, em seguida, adicione o seguinte código:


[!code-csharp[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample2.cs)]

O código usado nos manipuladores de eventos de dois é praticamente idêntico. Ambos comecem Obtendo informações sobre o aplicativo atual `Web.config` de arquivos por meio de [ `WebConfigurationManager` classe](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.aspx) s [ `OpenWebConfiguration` método](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.openwebconfiguration.aspx). Esse método retorna o arquivo de configuração da web para o caminho virtual especificado. Em seguida, o `Web.config` s de arquivo `<connectionStrings>` seção é acessada por meio de [ `Configuration` classe](https://msdn.microsoft.com/library/system.configuration.configuration.aspx) s [ `GetSection(sectionName)` método](https://msdn.microsoft.com/library/system.configuration.configuration.getsection.aspx), que retorna um [ `ConfigurationSection` ](https://msdn.microsoft.com/library/system.configuration.configurationsection.aspx) objeto.

O `ConfigurationSection` objeto inclui um [ `SectionInformation` propriedade](https://msdn.microsoft.com/library/system.configuration.configurationsection.sectioninformation.aspx) que fornece informações adicionais e funcionalidades em relação à seção de configuração. Como o código acima mostra, podemos determinar se a seção de configuração é criptografada, verificando o `SectionInformation` propriedade s `IsProtected` propriedade. Além disso, a seção pode ser criptografada ou descriptografada por meio de `SectionInformation` propriedade s `ProtectSection(provider)` e `UnprotectSection` métodos.

O `ProtectSection(provider)` método aceita como entrada uma cadeia de caracteres especificando o nome do provedor de configuração protegida para usar ao criptografar. No `EncryptConnString` manipulador de eventos do botão s passamos DataProtectionConfigurationProvider no `ProtectSection(provider)` método para que o provedor do DPAPI é usado. O `UnprotectSection` método pode determinar o provedor que foi usado para criptografar a seção de configuração e, portanto, não requer parâmetros de entrada.

Depois de chamar o `ProtectSection(provider)` ou `UnprotectSection` método, você deve chamar o `Configuration` objeto s [ `Save` método](https://msdn.microsoft.com/library/system.configuration.configuration.save.aspx) para manter as alterações. Depois que as informações de configuração foram criptografadas ou descriptografadas e as alterações salvadas, chamamos `DisplayWebConfig` para carregar o atualizado `Web.config` conteúdo no controle de caixa de texto.

Depois que você inseriu o código acima, testá-lo visitando o `EncryptingConfigSections.aspx` página por meio de um navegador. Inicialmente, você verá uma página que lista o conteúdo do `Web.config` com o `<connectionStrings>` seção exibida em texto sem formatação (veja a Figura 3).


[![Adicione uma caixa de texto e dois controles de Web de botão à página](protecting-connection-strings-and-other-configuration-information-cs/_static/image8.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image7.png)

**Figura 3**: Adicione uma caixa de texto e dois controles de Web de botão à página ([clique para exibir a imagem em tamanho normal](protecting-connection-strings-and-other-configuration-information-cs/_static/image9.png))


Agora, clique no botão criptografar cadeias de caracteres de Conexão. Se a validação de solicitação estiver habilitada, a marcação postada a partir de `WebConfigContents` TextBox produzirá uma `HttpRequestValidationException`, que exibe a mensagem, potencialmente perigosa `Request.Form` valor foi detectado no cliente. Validação de solicitação, que é habilitada por padrão no ASP.NET 2.0, proíbe postbacks que incluem sem HTML codificado e foi projetada para ajudar a evitar ataques de injeção de script. Essa verificação pode ser desabilitada na página - ou nível do aplicativo. Para desativá-lo para essa página, defina as `ValidateRequest` definir como `false` no `@Page` diretiva. O `@Page` diretiva for encontrada na parte superior da marcação declarativa de s de página.


[!code-aspx[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample3.aspx)]

Para obter mais informações sobre a validação de solicitação, sua finalidade, como desabilitá-lo na página – e nível do aplicativo, como bem como para o HTML codificar marcação, consulte [solicitação de validação – impedindo ataques de Script](../../../../whitepapers/request-validation.md).

Depois de desabilitar a validação de solicitação para a página, tente clicar no botão criptografar cadeias de caracteres de Conexão novamente. No postback, o arquivo de configuração será acessado e sua `<connectionStrings>` seção criptografada usando o provedor DPAPI. A caixa de texto, em seguida, é atualizada para exibir o novo `Web.config` conteúdo. Como mostra a Figura 4, o `<connectionStrings>` informações agora são criptografadas.


[![Clicar a criptografar Conexão cadeias de caracteres de botão criptografa o &lt;connectionString&gt; seção](protecting-connection-strings-and-other-configuration-information-cs/_static/image11.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image10.png)

**Figura 4**: clicando o criptografar Conexão cadeias de caracteres de botão criptografa o `<connectionString>` seção ([clique para exibir a imagem em tamanho normal](protecting-connection-strings-and-other-configuration-information-cs/_static/image12.png))


O criptografado `<connectionStrings>` seção gerada no meu computador segue, embora o conteúdo a `<CipherData>` elemento tiver sido removido para fins de brevidade:


[!code-xml[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample4.xml)]

> [!NOTE]
> O `<connectionStrings>` elemento Especifica o provedor usado para executar a criptografia (`DataProtectionConfigurationProvider`). Essas informações são usadas pelo `UnprotectSection` método quando se clica no botão de descriptografar cadeias de caracteres de Conexão.


Quando as informações de cadeia de caracteres de conexão são acessadas de `Web.config` - o código que escrevemos, de um controle SqlDataSource, ou o código gerado automaticamente da TableAdapters em nossos conjuntos de dados tipados - é automaticamente descriptografada. Em resumo, não precisamos adicionar qualquer código extra ou lógica para descriptografar o criptografado `<connectionString>` seção. Para demonstrar isso, visite um dos tutoriais anteriores neste momento, como o tutorial simples para exibição da seção de relatórios básicos (`~/BasicReporting/SimpleDisplay.aspx`). Como mostra a Figura 5, o tutorial funciona exatamente como o que desejávamos, indicando que as informações de cadeia de caracteres de conexão criptografada está sendo descriptografadas automaticamente pela página ASP.NET.


[![A camada de acesso a dados descriptografa automaticamente as informações de cadeia de caracteres de Conexão](protecting-connection-strings-and-other-configuration-information-cs/_static/image14.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image13.png)

**Figura 5**: A camada de acesso a dados descriptografa automaticamente as informações de cadeia de caracteres de Conexão ([clique para exibir a imagem em tamanho normal](protecting-connection-strings-and-other-configuration-information-cs/_static/image15.png))


Para reverter o `<connectionStrings>` seção novamente em sua representação de texto sem formatação, clique no botão de descriptografar cadeias de caracteres de Conexão. Em um postback, você deverá ver as cadeias de caracteres de conexão no `Web.config` em texto sem formatação. Neste ponto, sua tela deve ser como ele fez ao primeiro ao visitar essa página (consulte Figura 3).

## <a name="step-3-encrypting-configuration-sections-usingaspnetregiisexe"></a>Etapa 3: Criptografar seções de configuração usando`aspnet_regiis.exe`

O .NET Framework inclui uma variedade de ferramentas de linha de comando no `$WINDOWS$\Microsoft.NET\Framework\version\` pasta. No [dependências de Cache de SQL usando](../caching-data/using-sql-cache-dependencies-cs.md) tutorial, por exemplo, analisamos usando o `aspnet_regsql.exe` ferramenta de linha de comando para adicionar a infraestrutura necessária para as dependências de cache SQL. Outra ferramenta de linha de comando útil nessa pasta é a [ferramenta de registro do ASP.NET IIS (`aspnet_regiis.exe`)](https://msdn.microsoft.com/library/k6h9cz8h(VS.80).aspx). Como o nome sugere, a ferramenta de registro de IIS do ASP.NET é principalmente usada para registrar um aplicativo ASP.NET 2.0 com o servidor de Web de nível profissional do Microsoft s, o IIS. Além de seus recursos relacionados ao IIS, a ferramenta de registro do ASP.NET IIS pode ser usada para criptografar ou descriptografar as seções de configuração especificado no `Web.config`.

A instrução a seguir mostra a sintaxe geral usada para criptografar uma seção de configuração com o `aspnet_regiis.exe` ferramenta de linha de comando:


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample5.cmd)]

*seção* é a seção de configuração para criptografar (como connectionStrings), o *físico\_diretório* é o caminho físico completo para o diretório de raiz do aplicativo s da web, e *provedor*  é o nome do provedor de configuração protegida para usar (como DataProtectionConfigurationProvider). Como alternativa, se o aplicativo web está registrado no IIS, você pode inserir o caminho virtual em vez do caminho físico usando a seguinte sintaxe:


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample6.cmd)]

O seguinte `aspnet_regiis.exe` exemplo criptografa o `<connectionStrings>` seção usando o provedor com a DPAPI com uma chave de nível de máquina:


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample7.cmd)]

Da mesma forma, o `aspnet_regiis.exe` ferramenta de linha de comando pode ser usada para descriptografar as seções de configuração. Em vez de usar o `-pef` alternar, use `-pdf` (ou instead of `-pe`, use `-pd`). Além disso, observe que o nome do provedor não é necessário ao descriptografar.


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample8.cmd)]

> [!NOTE]
> Como estamos usando o provedor DPAPI, que usa chaves específicas do computador, você deve executar `aspnet_regiis.exe` no mesmo computador do qual as páginas da web estão sendo atendidas. Por exemplo, se você executar este programa de linha de comando do seu computador de desenvolvimento local e, em seguida, carregar o arquivo Web. config criptografado para o servidor de produção, o servidor de produção não será capaz de descriptografar as informações de cadeia de caracteres de conexão desde que foi criptografado usando chaves específicas para sua máquina de desenvolvimento. O provedor RSA não tem essa limitação como é possível exportar as chaves RSA para outro computador.


## <a name="understanding-database-authentication-options"></a>Noções básicas sobre opções de autenticação de banco de dados

Antes de qualquer aplicativo pode emitir `SELECT`, `INSERT`, `UPDATE`, ou `DELETE` consultas para um banco de dados do Microsoft SQL Server, o banco de dados primeiro devem identificar o solicitante. Esse processo é conhecido como *autenticação* e do SQL Server fornece dois métodos de autenticação:

- **Autenticação do Windows** -o processo sob a qual o aplicativo está em execução é usado para se comunicar com o banco de dados. Ao executar um aplicativo ASP.NET por meio do Visual Studio 2005 s ASP.NET Development Server, o aplicativo ASP.NET assume a identidade do usuário conectado no momento. Para aplicativos do ASP.NET no Microsoft Internet Information Server (IIS), aplicativos ASP.NET normalmente assumir a identidade do `domainName``\MachineName` ou `domainName``\NETWORK SERVICE`, embora isso pode ser personalizado.
- **Autenticação do SQL** -um valores de ID e a senha do usuário são fornecidos como credenciais para autenticação. Com a autenticação do SQL, a ID de usuário e senha são fornecidos na cadeia de conexão.

Autenticação do Windows é preferencial em relação à autenticação do SQL porque é mais seguro. Com a autenticação do Windows a cadeia de caracteres de conexão está livre de um nome de usuário e uma senha e se o servidor web e o servidor de banco de dados residirem em dois computadores diferentes, as credenciais não são enviadas pela rede em texto sem formatação. Com a autenticação do SQL, no entanto, as credenciais de autenticação são embutidos na cadeia de conexão e são transmitidas do servidor web para o servidor de banco de dados em texto sem formatação.

Esses tutoriais usou a autenticação do Windows. Você pode dizer qual modo de autenticação está sendo usado inspecionando a cadeia de caracteres de conexão. A cadeia de conexão no `Web.config` para nossos tutoriais foi:

`Data Source=.\SQLEXPRESS; AttachDbFilename=|DataDirectory|\NORTHWND.MDF; Integrated Security=True; User Instance=True`

A segurança integrada = True e a falta de um nome de usuário e senha indicam que a autenticação do Windows está sendo usada. Em alguma conexão cadeias de caracteres de Conexão confiável termo = Yes ou a segurança integrada = SSPI é usado em vez da segurança integrada = True, mas todos os três indicam o uso da autenticação do Windows.

O exemplo a seguir mostra uma cadeia de caracteres de conexão que usa a autenticação do SQL. Observe que as credenciais inseridas na cadeia de conexão:

`Server=serverName; Database=Northwind; uid=userID; pwd=password`

Imagine que um invasor é capaz de exibir o aplicativo s `Web.config` arquivo. Se você usar autenticação do SQL para se conectar a um banco de dados que seja acessível pela Internet, o invasor pode usar essa cadeia de caracteres de conexão para se conectar ao seu banco de dados SQL Management Studio ou de páginas do ASP.NET no seu próprio site. Para ajudar a reduzir essa ameaça, criptografa as informações de cadeia de caracteres de conexão do `Web.config` usando o sistema de configuração protegida.

> [!NOTE]
> Para obter mais informações sobre os diferentes tipos de autenticação disponíveis no SQL Server, consulte [Building Secure ASP.NET Applications: autenticação, autorização e comunicação segura](https://msdn.microsoft.com/library/aa302392.aspx). Para obter ainda mais conexão cadeia de caracteres exemplos que ilustram as diferenças entre a sintaxe de autenticação do Windows e SQL, consulte [ConnectionStrings.com](http://www.connectionstrings.com/).


## <a name="summary"></a>Resumo

Por padrão, os arquivos com um `.config` extensão em um aplicativo ASP.NET não pode ser acessado por meio de um navegador. Esses tipos de arquivos não são retornados porque eles podem conter informações confidenciais, como cadeias de conexão de banco de dados, os nomes de usuário e senhas e assim por diante. O sistema de configuração protegida no .NET 2.0 ajuda a proteger ainda mais as informações confidenciais, permitindo que as seções de configuração especificado a ser criptografado. Há dois provedores de configuração protegida internos: um que usa o algoritmo RSA e outro que usa o Windows Data Protection DPAPI (API).

Neste tutorial vimos como criptografar e descriptografar as definições de configuração usando o provedor DPAPI. Isso pode ser feito por meio de programação, como vimos na etapa 2, bem como por meio de `aspnet_regiis.exe` ferramenta de linha de comando, que foi abordada na etapa 3. Para obter mais informações sobre o uso de chaves de nível de usuário ou usando o provedor RSA em vez disso, consulte os recursos na seção leitura adicional.

Boa programação!

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Aplicativos ASP.NET seguros de construção: Autenticação, autorização e comunicação segura](https://msdn.microsoft.com/library/aa302392.aspx)
- [Criptografando informações de configuração no ASP.NET 2.0 Applications](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [Criptografando `Web.config` valores no ASP.NET 2.0](https://weblogs.asp.net/scottgu/archive/2006/01/09/434893.aspx)
- [Como: Criptografar seções de configuração no ASP.NET 2.0 usando DPAPI](https://msdn.microsoft.com/library/ms998280.aspx)
- [Como: Criptografar seções de configuração no ASP.NET 2.0 usando o RSA](https://msdn.microsoft.com/library/ms998283.aspx)
- [A API de configuração no .NET 2.0](http://www.odetocode.com/Articles/418.aspx)
- [Proteção de dados do Windows](https://msdn.microsoft.com/library/ms995355.aspx)

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores de avanço para este tutorial foram Teresa Murphy e Randy Schmidt. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs.md)
> [Próximo](debugging-stored-procedures-cs.md)
