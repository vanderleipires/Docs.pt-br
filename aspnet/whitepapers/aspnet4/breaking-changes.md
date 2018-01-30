---
uid: whitepapers/aspnet4/breaking-changes
title: "ASP.NET 4 últimas alterações | Microsoft Docs"
author: rick-anderson
description: "Este documento descreve as alterações que foram feitas para a versão do .NET Framework versão 4 que pode afetar os aplicativos que foram criados usando..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: d601c540-f86b-4feb-890c-20c806b3da6c
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet4/breaking-changes
msc.type: content
ms.openlocfilehash: d68723b52ae1ee80142fb1aca3b0b10de34332d1
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
<a name="aspnet-4-breaking-changes"></a>Alterações de quebra 4 do ASP.NET
====================
> Este documento descreve as alterações que foram feitas para a versão do .NET Framework versão 4 que pode afetar os aplicativos que foram criados com versões anteriores, incluindo as versões do ASP.NET 4 Beta 1 e Beta 2.
> 
> [Baixe o white paper](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_Breaking_Changes.pdf)


<a id="0.1__Toc256768952"></a><a id="0.1__Toc256770056"></a>

## <a name="contents"></a>Conteúdo

[Configuração de ControlRenderingCompatibilityVersion no arquivo Web. config](#0.1__Toc256770141 "_Toc256770141")  
[ClientIDMode Changes](#0.1__Toc256770142 "_Toc256770142")  
[Aspas simples de codificar HtmlEncode e UrlEncode agora](#0.1__Toc256770143 "_Toc256770143")  
[Página do ASP.NET (. aspx) analisador é Stricter](#0.1__Toc256770144 "_Toc256770144")  
[Arquivos de definição de navegador atualizados](#0.1__Toc256770145 "_Toc256770145")  
[System.Web.Mobile.dll removida do arquivo de configuração Web raiz](#0.1__Toc256770146 "_Toc256770146")  
[Validação de solicitação ASP.NET](#0.1__Toc256770147 "_Toc256770147")  
[Algoritmo de hash padrão agora é HMACSHA256](#0.1__Toc256770148 "_Toc256770148")  
[Erros de configuração relacionados a nova configuração do ASP.NET 4 raiz](#0.1__Toc256770149 "_Toc256770149")  
[Aplicativos do ASP.NET 4 filho falham ao iniciar quando em ASP.NET 2.0 ou o ASP.NET 3.5 aplicativos](#0.1__Toc256770150 "_Toc256770150")  
[Falha de Sites da Web ASP.NET 4 iniciar em computadores em que o SharePoint está instalado](#0.1__Toc256770151 "_Toc256770151")  
[A propriedade HttpRequest.FilePath não inclui mais valores PathInfo](#0.1__Toc256770152 "_Toc256770152")  
[O ASP.NET 2.0 aplicativos poderá gerar erros de HttpException que fazem referência a eurl.axd](#0.1__Toc256770153 "_Toc256770153")  
[Manipuladores de eventos não podem não ser gerados em um documento padrão no IIS 7.5 ou IIS 7 modo integrado](#0.1__Toc256770154 "_Toc256770154")  
[Muda para a implementação de CAS (segurança) de acesso de código ASP.NET](#0.1__Toc256770155 "_Toc256770155")  
[MembershipUser e outros tipos no Namespace de System.Web.Security foram movidos](#0.1__Toc256770156 "_Toc256770156")  
[Alterações de cache para variar de saída \* cabeçalho HTTP](#0.1__Toc256770157 "_Toc256770157")  
[Tipos de System.Web.Security Passport são obsoleto](#0.1__Toc256770158 "_Toc256770158")  
[A propriedade MenuItem.PopOutImageUrl falhar ao processar uma imagem no ASP.NET 4](#0.1__Toc256770159 "_Toc256770159")  
[Menu.StaticPopOutImageUrl e Menu.DynamicPopOutImageUrl Falha ao renderizar imagens quando caminhos conter barras invertidas](#0.1__Toc256770160 "_Toc256770160")  
[Disclaimer](#0.1__Toc256770161 "_Toc256770161")

<a id="0.1__ControlRenderingCompatibilityVersio"></a><a id="0.1__Toc245724853"></a><a id="0.1__Toc255587630"></a><a id="0.1__Toc256770141"></a>

## <a name="controlrenderingcompatibilityversion-setting-in-the-webconfig-file"></a>Configuração de ControlRenderingCompatibilityVersion no arquivo Web. config

Controles do ASP.NET foram modificados na versão 4 do .NET Framework para permitir que você especificar mais precisamente como eles processem marcação. Em versões anteriores do .NET Framework, alguns controles emitido marcação que não era possível desabilitar. Por padrão, esse tipo de marcação do ASP.NET 4 não é gerado.

Se você usar o Visual Studio 2010 para atualizar seu aplicativo do ASP.NET 2.0 ou ASP.NET 3.5, a ferramenta adicionará automaticamente uma configuração para o `Web.config` arquivo que preserva a renderização de herança. No entanto, se você atualizar um aplicativo alterando o pool de aplicativos no IIS para direcionar o .NET Framework 4, o ASP.NET usará o novo modo de renderização por padrão. Para desabilitar o novo modo de renderização, adicione a seguinte configuração no `Web.config` arquivo:

[!code-xml[Main](breaking-changes/samples/sample1.xml)]

As alterações de processamento principal que traz o novo comportamento são as seguintes:

- O **imagem** e **ImageButton** controles não processam um `border="0"` atributo.
- O **BaseValidator** controles de classe e validação de não processará mais texto vermelho por padrão.
- O **HtmlForm** controle não processa um **nome** atributo.
- O **tabela** controlar não processa um `border="0"` atributo.
- Controles que não são projetados para a entrada do usuário (por exemplo, o **rótulo** controle) não processará mais o `disabled="disabled"` atributo se seus **habilitado** está definida como **false**(ou se eles herdam essa configuração de um controle de contêiner).

<a id="0.1__Toc245724854"></a><a id="0.1__Toc255587631"></a><a id="0.1__Toc256770142"></a>

## <a name="clientidmode-changes"></a>Alterações de ClientIDMode

O **ClientIDMode** configuração no ASP.NET 4 permite especificar como o ASP.NET gera o **id** atributo para elementos HTML. Nas versões anteriores do ASP.NET, o comportamento padrão era equivalente para o **AutoID** de **ClientIDMode**. No entanto, a configuração padrão é agora **previsível**.

Se você usar o Visual Studio 2010 para atualizar seu aplicativo do ASP.NET 2.0 ou ASP.NET 3.5, a ferramenta adicionará automaticamente uma configuração para o `Web.config` arquivo que preserva o comportamento de versões anteriores do .NET Framework. No entanto, se você atualizar um aplicativo alterando o pool de aplicativos no IIS para direcionar o .NET Framework 4, o ASP.NET usará o novo modo por padrão. Para desabilitar o novo modo de ID de cliente, adicione a seguinte configuração no `Web.config` arquivo:

[!code-xml[Main](breaking-changes/samples/sample2.xml)]

<a id="0.1__Toc245724855"></a><a id="0.1__Toc255587632"></a><a id="0.1__Toc256770143"></a>

## <a name="htmlencode-and-urlencode-now-encode-single-quotation-marks"></a>Aspas simples de codificar HtmlEncode e UrlEncode agora

No ASP.NET 4, o **HtmlEncode** e **UrlEncode** métodos do **HttpUtility** e **HttpServerUtility** classes foram atualizadas para codifica o caractere de aspas simples (') da seguinte maneira:

- O **HtmlEncode** método codifica instâncias a aspa simples como.
- O **UrlEncode** método codifica instâncias a aspa simples como % 27.

<a id="0.1__Toc255587633"></a><a id="0.1__Toc256770144"></a><a id="0.1__Toc245724856"></a>

## <a name="aspnet-page-aspx-parser-is-stricter"></a>Página do ASP.NET (. aspx) analisador é Stricter

O analisador de página para páginas ASP.NET (`.aspx` arquivos) e controles de usuário (`.ascx` arquivos) mais rígida no ASP.NET 4 e rejeitará mais instâncias de marcação inválida. Por exemplo, os dois trechos de código a seguir seriam analisar com êxito em versões anteriores do ASP.NET, mas agora gerarão erros de analisador no ASP.NET 4.

[!code-aspx[Main](breaking-changes/samples/sample3.aspx)]

Observe o ponto e vírgula inválido no final de **HiddenField** marca.

[!code-aspx[Main](breaking-changes/samples/sample4.aspx)]

Observe o fechado **estilo** atributo que é executado para o **CssClass** atributo.

<a id="0.1__Toc255587634"></a><a id="0.1__Toc256770145"></a>

## <a name="browser-definition-files-updated"></a>Arquivos de definição de navegador atualizados

Os arquivos de definição do navegador foram atualizados para incluir informações sobre dispositivos e navegadores novos e atualizados. Navegadores e dispositivos mais antigos como o Netscape Navigator foram removidos e navegadores e dispositivos mais novos como o Google Chrome e Apple iPhone foram adicionados.

Se seu aplicativo contiver definições do navegador personalizadas herdadas de uma das definições do navegador que foram removidas, você verá um erro. Por exemplo, se o `App_Browsers` pasta contém uma definição de navegador que herda da definição de navegador IE2, você receberá a seguinte mensagem de erro de configuração:

- O elemento de navegador ou gateway com a ID 'IE2' não pode ser encontrado.

> [!NOTE]
> O **HttpBrowserCapabilities** objeto (que é exposto da página **Request. browser** propriedade) é orientada pelos arquivos de definições de navegador. Portanto, as informações retornadas por acessar uma propriedade do objeto no ASP.NET 4 podem ser diferentes das informações retornadas em uma versão anterior do ASP.NET.


Você pode reverter para os arquivos de definição de navegador antigo, copiando os arquivos de definição de navegador de pasta a seguir:

[!code-console[Main](breaking-changes/samples/sample5.cmd)]

Copie os arquivos para o correspondente `\CONFIG\Browsers` pasta para ASP.NET 4. Depois de copiar os arquivos, execute o Aspnet\_regbrowsers.exe ferramenta de linha de comando.

<a id="0.1__Toc255587635"></a><a id="0.1__Toc256770146"></a>

## <a name="systemwebmobiledll-removed-from-root-web-configuration-file"></a>System.Web.Mobile.dll removida do arquivo de configuração Web raiz

Nas versões anteriores do ASP.NET, uma referência ao assembly System.Web.Mobile.dll foi incluída na raiz `Web.config` arquivo o **assemblies** seção em. Para melhorar o desempenho, a referência a esse assembly foi removida.

O assembly System.Web.Mobile.dll está incluído no ASP.NET 4, mas ela foi preterida. Se você quiser usar tipos do assembly System.Web.Mobile.dll, adicione uma referência a esse assembly para a raiz `Web.config` arquivo ou aplicativo `Web.config` arquivo. Por exemplo, se você quiser usar algum dos controles móveis ASP.NET (preteridos), você deve adicionar uma referência ao assembly System.Web.Mobile.dll para o `Web.config` arquivo.

<a id="0.1__Toc245724857"></a><a id="0.1__Toc255587636"></a><a id="0.1__Toc256770147"></a>

## <a name="aspnet-request-validation"></a>Validação de solicitação do ASP.NET

O recurso de validação de solicitação do ASP.NET fornece um certo nível de proteção padrão contra ataques de script entre sites (XSS). Em versões anteriores do ASP.NET, a validação de solicitação foi habilitada por padrão. No entanto, ela é aplicada apenas a páginas ASP.NET (`.aspx` arquivos e seus arquivos de classe) e somente quando essas páginas estiverem sendo executados.

No ASP.NET 4, por padrão, a validação de solicitação está habilitada para todas as solicitações, porque ela está habilitada antes do **BeginRequest** fase de uma solicitação HTTP. Como resultado, a validação de solicitação se aplica às solicitações de todos os recursos do ASP.NET, não apenas as solicitações de páginas. aspx. Isso inclui solicitações, como chamadas de serviço Web e de manipuladores HTTP personalizados. Validação de solicitação também estará ativa quando o módulo HTTP personalizado está lendo o conteúdo de uma solicitação HTTP.

Como resultado, os erros de validação de solicitação agora podem ocorrer para solicitações que anteriormente não disparar erros. Para reverter para o comportamento do recurso de validação de solicitação do ASP.NET 2.0, adicione a seguinte configuração no `Web.config` arquivo:

[!code-xml[Main](breaking-changes/samples/sample6.xml)]

No entanto, recomendamos que você analise os erros de validação de solicitação para determinar se os manipuladores existentes, módulos ou outros códigos personalizados potencialmente acessa entradas HTTP não seguras que podem ser XSS vetores de ataque.

<a id="0.1__Toc245724858"></a><a id="0.1__Toc255587637"></a><a id="0.1__Toc256770148"></a>

## <a name="default-hashing-algorithm-is-now-hmacsha256"></a>Algoritmo de hash padrão agora é HMACSHA256

O ASP.NET usa algoritmos de hash e criptografia para ajudar a proteger dados como cookies de autenticação de formulários e estado de exibição. Por padrão, o ASP.NET 4 agora usa o algoritmo de HMACSHA256 para operações de hash em cookies e estado de exibição. Versões anteriores do ASP.NET usado o algoritmo HMACSHA1 mais antigo.

Os aplicativos podem ser afetados se você executar misto ASP.NET 2.0/ASP.NET 4 ambientes onde os dados, como os cookies de autenticação de formulários devem trabalhar across.NET Framework versões. Para configurar um aplicativo ASP.NET 4 para usar o algoritmo HMACSHA1 mais antigo, adicione a seguinte configuração no `Web.config` arquivo:

[!code-xml[Main](breaking-changes/samples/sample7.xml)]

<a id="0.1__Toc245724859"></a><a id="0.1__Toc255587638"></a><a id="0.1__Toc256770149"></a>

## <a name="configuration-errors-related-to-new-aspnet-4-root-configuration"></a>Erros de configuração relacionados a nova configuração do ASP.NET 4 raiz

Os arquivos de configuração raiz (o `machine.config` de arquivo e a raiz `Web.config` arquivo) para o .NET Framework 4 (e, portanto, ASP.NET 4) foram atualizadas para incluir a maioria das informações de configuração clichê que foi encontradas no ASP.NET 3.5 a aplicativo `Web.config` arquivos. Devido à complexidade dos sistemas de configuração IIS 7 e IIS 7.5 gerenciados, executar aplicativos ASP.NET 3.5 no ASP.NET 4 e no IIS 7 e 7.5 do IIS pode resultar em ASP.NET ou IIS erros de configuração.

É recomendável que você atualize aplicativos ASP.NET 3.5 para ASP.NET 4, usando as ferramentas de atualização de projeto no Visual Studio 2010, se possível. Visual Studio 2010 modifica automaticamente o aplicativo de ASP.NET 3.5 `Web.config` arquivo para conter as configurações apropriadas para ASP.NET 4.

No entanto, esse é um cenário com suporte para executar aplicativos ASP.NET 3.5 usando o .NET Framework 4 sem recompilação. Nesse caso, talvez você precise modificar manualmente o aplicativo `Web.config` arquivo antes de executar o aplicativo sob o .NET Framework 4 e 7 do IIS ou IIS 7.5.

As duas próximas seções descrevem as alterações que talvez você precise fazer para diferentes combinações de software.

**Windows Vista SP1 ou Windows Server 2008 SP1, em que nem o hotfix KB958854 nem SP2 estão instalados.** Nessa configuração, o sistema de configuração do IIS 7 incorretamente mescla configuração de um aplicativo gerenciado, comparando o nível do aplicativo `Web.config` arquivo para o ASP.NET 2.0 `machine.config` arquivos. Devido a isso, nível do aplicativo `Web.config` arquivos do .NET Framework 3.5 ou posterior devem ter uma **Extensions** definição de seção de configuração (o elemento) para não causar uma falha de validação do IIS 7.

No entanto, nível de aplicativo modificado manualmente `Web.config` entradas do arquivo que não coincidem com precisão as definições de seção de configuração clichê originais que foram introduzidas com o Visual Studio 2008 fará com que erros de configuração do ASP.NET. (Entradas de configuração padrão que são geradas pelo Visual Studio 2008 funcionam corretamente.) Um problema comum é que modificado manualmente `Web.config` arquivos omitir o **allowDefinition** e **requirePermission** atributos de configuração que são encontrados na seção de configuração de vários definições. Isso causa uma incompatibilidade entre a seção de configuração abreviado no nível do aplicativo `Web.config` arquivos e a definição completa no ASP.NET 4 `machine.config` arquivo. Como resultado, em tempo de execução, o sistema de configuração do ASP.NET 4 gera um erro de configuração.

**Windows Vista SP2, Windows Server 2008 SP2, Windows 7, Windows Server 2008 R2 e também o Windows Vista SP1 e Windows Server 2008 SP1 em que o hotfix KB958854 está instalado.**

Nesse cenário, o sistema de configuração nativo do IIS 7 e 7.5 do IIS retorna um erro de configuração porque ela executa uma comparação de texto no **tipo** atributo está definido para um manipulador de seção de configuração gerenciada. Como todos os `Web.config` os arquivos que são gerados pelo Visual Studio 2008 e o Visual Studio 2008 SP1 têm "3.5" na cadeia de caracteres de tipo para o **Extensions** (e relacionados) manipuladores de seção de configuração e porque o ASP.NET 4 `machine.config` arquivo tem "4.0" **tipo** atributo para os mesmo manipuladores de seção de configuração, os aplicativos que são gerados no Visual Studio 2008 ou o Visual Studio 2008 SP1 sempre falharem na validação de configuração no IIS 7 e IIS 7.5.

<a id="0.1__Toc251910248"></a>

### <a name="resolving-these-issues"></a>Resolver esses problemas

A solução alternativa para o primeiro cenário é atualizar o nível do aplicativo `Web.config` arquivo, incluindo o texto de configuração clichê de um `Web.config` arquivo que foi gerado automaticamente pelo Visual Studio 2008.

Uma solução alternativa para o primeiro cenário é para instalar o Service Pack 2 para o Vista ou Windows Server 2008 em seu computador ou instale o hotfix KB958854 ([https://support.microsoft.com/kb/958854](https://support.microsoft.com/kb/958854)) para corrigir incorreto comportamento da mesclagem de configuração do sistema de configuração do IIS. No entanto, depois de executar qualquer uma dessas ações, seu aplicativo provavelmente encontrará um erro de configuração devido ao problema descrito para o segundo cenário.

A solução alternativa para o segundo cenário é exclua ou comente a todos os **Extensions** definições da seção de configuração e a seção de configuração de grupo de definições de nível de aplicativo `Web.config` arquivo. Essas definições geralmente estão na parte superior do nível do aplicativo `Web.config` de arquivos e pode ser identificado pelo **configSections** elemento e seus filhos.

Para ambos os cenários, é recomendável que você exclua manualmente o **System. CodeDom** seção, embora isso não seja necessário.

<a id="0.1__Toc252995490"></a><a id="0.1__Toc255587639"></a><a id="0.1__Toc256770150"></a><a id="0.1__Toc245724860"></a>

## <a name="aspnet-4-child-applications-fail-to-start-when-under-aspnet-20-or-aspnet-35-applications"></a>Aplicativos do ASP.NET 4 filho falham ao iniciar quando em ASP.NET 2.0 ou o ASP.NET 3.5 aplicativos

Os aplicativos ASP.NET 4 configurados como filhos de aplicativos que executam versões anteriores do ASP.NET talvez falhem ao iniciar devido a erros de configuração ou de compilação. O exemplo a seguir mostra uma estrutura de diretórios para um aplicativo afetado.

`/parentwebapp`(configurado para usar o ASP.NET 2.0 ou ASP.NET 3.5)  
`/childwebapp`(configurado para usar o ASP.NET 4)

O aplicativo no `childwebapp` pasta falhará ao iniciar no IIS 7 ou IIS 7.5 e relatará um erro de configuração. O texto de erro inclui uma mensagem semelhante à seguinte:

- `The requested page cannot be accessed because the related configuration data for the page is invalid.`
  

- `The configuration section 'configSections' cannot be read because it is missing a section declaration.`

No IIS 6, o aplicativo no `childwebapp` pasta também poderão ser iniciados, mas ele relatará um erro diferente. Por exemplo, o texto de erro pode indicar o seguinte:

- `The value for the 'compilerVersion' attribute in the provider options must be 'v4.0' or later if you are compiling for version 4.0 or later of the .NET Framework. To compile this Web application for version 3.5 or earlier of the .NET Framework, remove the 'targetFramework' attribute from the element of the Web.config file`

Essas situações ocorrer porque as informações de configuração do aplicativo-pai no `parentwebapp` pasta faz parte da hierarquia de informações de configuração que determina as configurações de configuração mesclada final que são usadas por web filho aplicativo no `childwebapp` pasta. Dependendo se o aplicativo Web do ASP.NET 4 está em execução no IIS 7 (ou IIS 7.5) ou no IIS 6, o sistema de configuração do IIS ou o sistema de compilação do ASP.NET 4 retornará um erro.

As etapas que devem ser seguidas para resolver esse problema e obter o filho aplicativo ASP.NET 4 para trabalhar dependem se o aplicativo ASP.NET 4 é executado no IIS 6 ou IIS 7 (ou IIS 7.5).

### <a name="step-1-iis-7-or-iis-75-only"></a>Etapa 1 (IIS 7 ou IIS 7.5 apenas)

Essa etapa é necessária somente em sistemas operacionais que executam o IIS 7 ou IIS 7.5, que inclui o Windows Vista, Windows Server 2008, Windows 7 e Windows Server 2008 R2.

Mover o **configSections** definição no `Web.config` arquivo do aplicativo pai (o aplicativo que executa o ASP.NET 2.0 ou ASP.NET 3.5) para a raiz `Web.config` arquivo para o.NET Framework 2.0. O sistema de configuração nativa do IIS 7 e IIS 7.5 verifica o **configSections** elemento quando ele mescla a hierarquia de arquivos de configuração. Movendo o **configSections** definição a partir do aplicativo Web pai `Web.config` arquivo para a raiz `Web.config` arquivo oculta efetivamente o elemento do processo de mesclagem de configuração que ocorre para o filho ASP.NET 4 aplicativo.

Em um sistema operacional de 32 bits ou 32 bits para pools de aplicativos, a raiz `Web.config` arquivo para o ASP.NET 2.0 e o ASP.NET 3.5 normalmente está localizado na seguinte pasta:

`C:\Windows\Microsoft.NET\Framework\v2.0.50727\CONFIG`

Em um sistema operacional de 64 bits ou 64 bits para pools de aplicativos, a raiz `Web.config` arquivo para o ASP.NET 2.0 e o ASP.NET 3.5 normalmente está localizado na seguinte pasta:

`C:\Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG`

Se você executar aplicativos da Web de 32 bits e 64 bits em um computador de 64 bits, você deve mover o **configSections** elemento para cima na raiz `Web.config` arquivos de 32 bits e os sistemas de 64 bits.

Quando você colocar o **configSections** elemento na raiz `Web.config` de arquivos, cole a seção imediatamente após o **configuração** elemento. O exemplo a seguir mostra que a parte superior da raiz `Web.config` arquivo deve ter aparência quando você terminar de mover os elementos.

> [!NOTE]
> No exemplo a seguir, as linhas foram quebradas para facilitar a leitura.


[!code-xml[Main](breaking-changes/samples/sample8.xml)]

### <a name="step-2-all-versions-of-iis"></a>Etapa 2 (todas as versões do IIS)

Esta etapa é necessária se o aplicativo Web ASP.NET 4 filho está em execução no IIS 6 ou IIS 7 (ou IIS 7.5).

No `Web.config` adicionar de arquivo do aplicativo Web que está executando o ASP.NET 2 ou ASP.NET 3.5 pai um **local** marca especifica explicitamente (para sistemas de configuração do IIS e ASP.NET) que as entradas de configuração se aplicam ao aplicativo Web pai. O exemplo a seguir mostra a sintaxe do **local** elemento a ser adicionado:

[!code-xml[Main](breaking-changes/samples/sample9.xml)]

A exemplo a seguir mostra como o **local** marca é usada para encapsular todas as seções de configuração, começando com o **appSettings** seção e terminando com **System. webServer**seção.

[!code-xml[Main](breaking-changes/samples/sample10.xml)]

Quando você tiver concluído as etapas 1 e 2, aplicativos da Web do ASP.NET 4 filho serão iniciado sem erros.

<a id="0.1__Toc252995491"></a><a id="0.1__Toc255587640"></a><a id="0.1__Toc256770151"></a>

## <a name="aspnet-4-web-sites-fail-to-start-on-computers-where-sharepoint-is-installed"></a>Falha de Sites da Web ASP.NET 4 iniciar em computadores em que o SharePoint está instalado

Servidores Web que executam o SharePoint tem um `Web.config` arquivo implantado na raiz de um site do SharePoint (por exemplo, `c:\inetpub\wwwroot\web.config` para Site padrão). Neste `Web.config` arquivo, do SharePoint define uma confiança parcial personalizada nível nomeado WSS\_mínimo.

Se você tentar executar um site da Web do ASP.NET 4 que é implantado como um filho desse tipo de site do SharePoint, você verá o seguinte erro:

`Could not find permission set named 'ASP.NET'.`

Esse erro ocorre porque a infraestrutura de segurança (ACS) do acesso de código ASP.NET 4 procura por um conjunto de permissões nomeado ASP.NET. No entanto, o parcial confiar em arquivo de configuração que é referenciado pelo WSS\_mínimo não contém todos os conjuntos de permissão com esse nome.

Atualmente não há uma versão do SharePoint disponível que é compatível com o ASP.NET. Como resultado, você não deve tentar executar um site da Web do ASP.NET 4 como um site filho abaixo do SharePoint Web sites.

<a id="0.1__Toc255587641"></a><a id="0.1__Toc256770152"></a>

## <a name="the-httprequestfilepath-property-no-longer-includes-pathinfo-values"></a>A propriedade HttpRequest.FilePath não inclui mais valores PathInfo

Versões anteriores do ASP.NET incluído um **PathInfo** valor no valor retornado diversos relacionados ao caminho das propriedades do arquivo, incluindo **HttpRequest.FilePath**,  **HttpRequest.AppRelativeCurrentExecutionFilePath**, e **HttpRequest.CurrentExecutionFilePath**. O ASP.NET 4 já não inclui o **PathInfo** valor nos valores de retorno dessas propriedades. Em vez disso, o **PathInfo** informações estão disponíveis em **HttpRequest.PathInfo**. Por exemplo, imagine o fragmento da URL a seguir:

`/testapp/Action.mvc/SomeAction`

Em versões anteriores do ASP.NET, **HttpRequest** propriedades têm os seguintes valores:

**HttpRequest.FilePath**: `/testapp/Action.mvc/SomeAction`

**HttpRequest.PathInfo**: (vazio)

No ASP.NET 4, **HttpRequest** propriedades em vez disso, tem os seguintes valores:

**HttpRequest.FilePath**: `/testapp/Action.mvc`

**HttpRequest.PathInfo**: `SomeAction`

<a id="0.1__Toc252995493"></a><a id="0.1__Toc255587642"></a><a id="0.1__Toc256770153"></a><a id="0.1__Toc245724861"></a>

## <a name="aspnet-20-applications-might-generate-httpexception-errors-that-reference-eurlaxd"></a>O ASP.NET 2.0 aplicativos poderá gerar erros de HttpException que fazem referência a eurl.axd

Após a habilitação do ASP.NET 4 no IIS 6, os aplicativos ASP.NET 2.0 que são executados no IIS 6 (no Windows Server 2003 ou Windows Server 2003 R2) poderá gerar erros, como o seguinte:

`System.Web.HttpException: Path '/[yourApplicationRoot]/eurl.axd/[Value]' was not found.`

Esse erro ocorre porque quando ASP.NET detecta que um site da Web está configurado para usar o ASP.NET 4, um componente nativo de 4 ASP.NET passa uma URL sem extensão para a parte gerenciada do ASP.NET para processamento adicional. No entanto, se os diretórios virtuais que estão abaixo de um site da Web do ASP.NET 4 são configurados para usar o ASP.NET 2.0, processar a URL sem extensão nos resultados modo em uma URL modificada que contém a cadeia de caracteres "eurl.axd". Essa URL modificada é enviado para o aplicativo ASP.NET 2.0. O ASP.NET 2.0 não reconhece o formato de "eurl.axd". Portanto, o ASP.NET 2.0 tenta localizar um arquivo chamado `eurl.axd` e executá-lo. Como esse arquivo não existir, a solicitação falhará com um **HttpException** exceção.

Você pode contornar esse problema usando uma das opções a seguir.

### <a name="option-1"></a>Opção 1

Se o ASP.NET 4 não é necessária para executar o site da Web, remapear o site para usar o ASP.NET 2.0.

### <a name="option-2"></a>Opção 2

Se o ASP.NET 4 é necessário para executar o site da Web, mova quaisquer diretórios virtuais do ASP.NET 2.0 de filho para um site diferente, é mapeado para o ASP.NET 2.0.

### <a name="option-3"></a>Opção 3

Se não for viável para remapear o site da Web para ASP.NET 2.0 ou para alterar o local de um diretório virtual, desabilite explicitamente o URL sem extensão de processamento no ASP.NET 4. Use o procedimento a seguir:

1. No registro do Windows, abra o seguinte nó:

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ASP.NET\4.0.30319.0`

1. Criar um novo **DWORD** valor denominado **EnableExtensionlessUrls**.
2. Definir **EnableExtensionlessUrls** como 0. Isso desativa o comportamento da URL sem extensão.
3. Salve o valor do registro e feche o editor do registro.
4. Execute o **iisreset** ferramenta de linha de comando, que faz com que o IIS ler o novo valor de registro.

> [!NOTE]
> Configuração **EnableExtensionlessUrls** como 1 habilita o comportamento da URL sem extensão. Essa é a configuração padrão se nenhum valor for especificado.


<a id="0.1__Toc252995494"></a><a id="0.1__Toc255587643"></a><a id="0.1__Toc256770154"></a><a id="0.1__Toc245724862"></a>

## <a name="event-handlers-might-not-be-not-raised-in-a-default-document-in-iis-7-or-iis-75-integrated-mode"></a>Manipuladores de eventos não podem não ser gerados em um documento padrão no IIS 7.5 ou IIS 7 modo integrado

O ASP.NET 4 inclui modificações alterarem como o **ação** atributo do HTML **formulário** elemento é processado quando uma URL sem extensão resolve para um documento padrão. Um exemplo de uma URL sem extensão resolver para um documento padrão seria [http://contoso.com/](http://contoso.com/), resultando em uma solicitação para [http://contoso.com/Default.aspx](http://contoso.com/Default.aspx).

Agora, o ASP.NET 4 processa o HTML **formulário** do elemento **ação** valor do atributo como uma cadeia de caracteres vazia quando uma solicitação é feita em uma URL sem extensão que tem um documento padrão mapeado para ele. Por exemplo, nas versões anteriores do ASP.NET, uma solicitação para [http://contoso.com](http://contoso.com) resultaria em uma solicitação para `Default.aspx`. Nesse documento, a abertura **formulário** marca deve ser renderizada como no exemplo a seguir:

`<form action="Default.aspx" />`

No ASP.NET 4, uma solicitação para [http://contoso.com](http://contoso.com) também resulta em uma solicitação para `Default.aspx`. No entanto, o ASP.NET processa a abertura de HTML agora **formulário** marca como no exemplo a seguir:

`<form action="" />`

Essa diferença em como a **ação** atributo é processado pode causar alterações sutis no como uma postagem de formulário é processada pelo IIS e ASP.NET. Quando o **ação** atributo é uma cadeia de caracteres vazia, o IIS **DefaultDocumentModule** objeto criará uma solicitação filho para `Default.aspx`. Na maioria das condições, essa solicitação filho é transparente para o código do aplicativo e o `Default.aspx` página normalmente é executada.

No entanto, uma eventual interação entre código gerenciado e o modo integrado do IIS 7 ou IIS 7.5 pode fazer páginas .aspx gerenciadas pararem de funcionar adequadamente durante a solicitação filho. Se as condições a seguir ocorrer, a solicitação filho para uma `Default.aspx` documento resultará em erro ou um comportamento inesperado:

1. Uma página. aspx é enviada para o navegador com o **formulário** do elemento **ação** atributo definido como "".
2. O formulário é enviado para o ASP.NET.
3. Um módulo gerenciado pelo HTTP lê alguma parte do corpo da entidade. Por exemplo, um módulo lê **Request** ou **params**. Isso faz o corpo da entidade da solicitação POST ser lido na memória gerenciada. Como resultado, o corpo da entidade não está mais disponível para quaisquer módulos de código nativo em execução no modo integrado do IIS 7 ou do IIS 7.5.
4. O IIS **DefaultDocumentModule** objeto eventualmente é executado e cria uma solicitação filho para o `Default.aspx` documento. No entanto, como o corpo da entidade já foi lido por um trecho de código gerenciado, não há nenhum corpo de entidade disponível para enviar a solicitação filho.
5. Quando o pipeline HTTP é executada para a solicitação filho, o manipulador para `.aspx` arquivos é executada durante a fase de handler-execute.
6. Porque não há nenhum corpo de entidade, há nenhuma variável de formulário e nenhum estado de exibição e, portanto, nenhuma informação está disponível para o manipulador de página. aspx determinar qual evento (se houver) deve ser gerado. Como resultado, nenhum manipulador de eventos de postback para a página .aspx afetada é executado.

Você pode contornar esse comportamento das seguintes maneiras:

- Identificar o módulo HTTP que está acessando o corpo da entidade da solicitação durante solicitações de documentos padrão e determinar se ele pode ser configurado para ser executado somente para solicitações gerenciadas. No modo integrado para IIS 7 e IIS 7.5, módulos HTTP podem ser marcados para ser executado somente para solicitações gerenciadas, adicionando o seguinte atributo para o módulo **webServer/modules** entrada:

- `precondition="managedHandler"`

- Este desabilita a configuração do módulo para solicitações que o IIS 7 e IIS 7.5 determinam como não sendo gerenciados solicitações. Para solicitações de documento padrão, a primeira solicitação é uma URL sem extensão. Portanto, IIS não execute quaisquer módulos gerenciados que são marcados com uma pré-condição de manipulador gerenciado durante o processamento da solicitação inicial. Como resultado, os módulos gerenciados não lerá acidentalmente o corpo da entidade e, portanto, o corpo da entidade ainda está disponível e é passado para a solicitação filho e o documento padrão.

- Se os módulos HTTP problemáticos precisam executar para todas as solicitações (para arquivos estáticos, para URLs sem extensão resolvem para o **DefaultDocumentModule** objeto gerenciado de solicitações, etc.), modifique as páginas. aspx afetados por explicitamente Definindo o **ação** propriedade da página **System.Web.UI.HtmlControls.HtmlForm** controle para uma cadeia de caracteres não vazia. Por exemplo, se o documento padrão é `Default.aspx`, modifique o código da página para definir explicitamente o **HtmlForm** do controle **ação** propriedade para "Default.aspx".

<a id="0.1__Toc255587644"></a><a id="0.1__Toc256770155"></a>

## <a name="changes-to-the-aspnet-code-access-security-cas-implementation"></a>Alterações na implementação de CAS (segurança) de acesso de código ASP.NET

O ASP.NET 2.0, e pela extensão de recursos do ASP.NET que foram adicionados em 3.5, use o .NET Framework 1.1 e o modelo de segurança (ACS) 2.0 código acesso. No entanto, a implementação da CAS no ASP.NET 4 foi substancialmente revisada. Como resultado, os aplicativos ASP.NET de confiança parcial que dependem de código confiável em execução no cache de assembly global (GAC) podem falhar com diversas exceções de segurança. Aplicativos de confiança parcial que dependem de grandes modificações para a política de CAS do computador também podem falhar com exceções de segurança.

Você pode reverter a aplicativos ASP.NET 4 de confiança parcial para o comportamento do ASP.NET 1.1 e 2.0 usando o novo **legacyCasModel** atributo o **confiança** elemento de configuração, conforme mostrado no exemplo a seguir :

`<trust level= "Medium" legacyCasModel="true" />`

Quando você reverter para o modelo CAS herdado, os seguintes comportamentos de autoridades de certificação antigos estão habilitados:

- Política de CAS de máquina é cumprida.
- Vários conjuntos de permissões diferentes em um único domínio de aplicativo são permitidos.
- Declarações de permissão explícita não são necessárias para assemblies no GAC que são invocados quando apenas ASP.NET ou outro código do .NET Framework estiver na pilha.

Um cenário não pode ser revertido no .NET Framework 4: aplicativos de confiança parcial não Web não podem chamar determinadas APIs em System.Web.dll e System.Web.Extensions.dll. Nas versões anteriores do .NET Framework, era possível para aplicativos de confiança parcial da Web não deve ser concedido explicitamente **AspNetHostingPermission** permissões. Esses aplicativos podem usar **System.Web.HttpUtility**, digita o **System.Web.ClientServices.\***  namespaces e tipos relacionam a associação, funções e perfis. Não há suporte para chamar esses tipos de aplicativos de confiança parcial não Web no .NET Framework 4.

> [!NOTE]
> O **HtmlEncode** e **HtmlDecode** funcionalidade do **System.Web.HttpUtility** classe foi movida para o .NET Framework 4 novo  **System.Net.WebUtility** classe. Se o que era a única funcionalidade ASP.NET que estava sendo usada, modifique o código do aplicativo para usar o novo **WebUtility** classe em vez disso.


Este é um resumo de alto nível das alterações na implementação de autoridades de certificação padrão no ASP.NET 4:

- Domínios do aplicativo ASP.NET agora são domínios de aplicativo homogênea. Somente conjuntos de conceder confiança total e de confiança parcial estão disponíveis em um domínio de aplicativo.
- Conjuntos de concessões de confiança parcial do ASP.NET são independentes de qualquer política de CAS de nível empresarial, nível de máquina ou nível de usuário.
- Assemblies ASP.NET fornecida no 3.5 e 3.5 SP1 foram convertidos para usar o modelo de transparência do .NET Framework 4.
- Uso do ASP.NET **AspNetHostingPermission** atributo foi reduzido significativamente. A maioria das instâncias deste atributo foram removidas das APIs públicas do ASP.NET.
- Assemblies compilados dinamicamente que são criados pelos provedores de compilação do ASP.NET foram atualizados para marcar explicitamente assemblies como transparente.
- Agora, todos os assemblies do ASP.NET são marcados de forma que o atributo APTCA é respeitado apenas em ambientes de hospedagem na Web. Parcialmente confiáveis ambientes de hospedagem não Web como o ClickOnce não poderá chamar assemblies do ASP.NET.

Para obter mais informações sobre o novo modelo de segurança de acesso de código ASP.NET 4, consulte [usando a segurança do acesso do código em aplicativos ASP.NET](https://msdn.microsoft.com/library/dd984947%28VS.100%29.aspx) no site do MSDN.

<a id="0.1__Toc256770156"></a><a id="0.1__Toc245724863"></a><a id="0.1__Toc252995496"></a><a id="0.1__Toc255587645"></a><a id="0.1__Toc245724864"></a>

## <a name="membershipuser-and-other-types-in-the-systemwebsecurity-namespace-have-been-moved"></a>MembershipUser e outros tipos no Namespace de System.Web.Security foram movidos

Alguns tipos que são usados na associação do ASP.NET foram movidos do `System.Web.dll` para o novo assembly System.Web.ApplicationServices.dll. Os tipos foram movidos para resolver as dependências de camadas de arquitetura entre os tipos no cliente e nos SKUs do .NET Framework estendido.

Projetos de site não tem problemas como resultado de mover esses tipos, como System.Web.ApplicationServices.dll foi adicionada à lista de assemblies referenciados que é usada por padrão, o sistema de compilação do ASP.NET. Se você atualizar um projeto de site da Web que foi criado usando uma versão anterior do ASP.NET para ASP.NET 4 abrindo-o no Visual Studio 2010, o projeto será compilado sem erros.

Da mesma forma, se você atualizar um projeto de aplicativo Web que foi criado em uma versão anterior do ASP.NET para ASP.NET 4 abrindo-o no Visual Studio 2010, o processo de atualização adiciona uma referência a System.Web.ApplicationServices.dll ao projeto. Portanto, atualizado da Web a projetos de aplicativo também serão compilado sem erros.

Arquivos compilados de (binários) que foram criados com versões anteriores do ASP.NET também serão executadas sem erros no ASP.NET 4, mesmo que os tipos de associação foram movidos para um assembly diferente. Tipo de informações de encaminhamento foi adicionado à versão do ASP.NET 4 `System.Web.dll` que encaminha automaticamente as referências de tempo de execução para esses tipos para o novo local para os tipos.

No entanto, as bibliotecas de classes que usam tipos de associação específico e que tenham sido atualizados de versões anteriores do ASP.NET falhará ao compilar quando usado em um projeto do ASP.NET 4. Por exemplo, um projeto de biblioteca de classe pode falhar compilar e relatar um erro como o seguinte:

- `The type 'System.Web.Security.MembershipUser' is defined in an assembly that is not referenced. You must add a reference to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
  

- `The type name 'MembershipUser' could not be found. This type has been forwarded to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'. Consider adding a reference to that assembly.`

Você pode contornar esse problema adicionando uma referência em seu projeto de biblioteca de classe para System.Web.ApplicationServices.dll.

A lista a seguir mostra o *System.Web.Security* tipos que foram movidos de `System.Web.dll` para System.Web.ApplicationServices.dll:

- *System.Web.Security.MembershipCreateStatus*
- *System.Web.Security.Membership.CreateUserException*
- *System.Web.Security.MembershipPasswordException*
- *System.Web.Security.MembershipPasswordFormat*
- *System.Web.Security.MembershipProvider*
- *System.Web.Security.MembershipProviderCollection*
- *System.Web.Security.MembershipUser*
- *System.Web.Security.MembershipUserCollection*
- *System.Web.Security.MembershipValidatePasswordEventHandler*
- *System.Web.Security.ValidatePasswordEventArgs*
- *System.Web.Security.RoleProvider*
- <a id="0.1_a"></a>*System.Web.Configuration.MembershipPasswordCompatibilityMode*

<a id="0.1__Toc256770157"></a>

## <a name="output-caching-changes-to-vary--http-header"></a>Alterações de cache para variar de saída \* cabeçalho HTTP

No ASP.NET 1.0, um erro causado páginas armazenadas em cache que especificou `Location="ServerAndClient"` como uma configuração de cache de saída – para emitir um `Vary:*` cabeçalho HTTP na resposta. O efeito disso era informar os navegadores de clientes a nunca armazenar a página em cache localmente.

No ASP.NET 1.1, o **System.Web.HttpCachePolicy.SetOmitVaryStar** método foi adicionado, que você pode chamar para suprimir o `Vary:*` cabeçalho. Esse método foi escolhido porque o cabeçalho HTTP emitido a alteração foi considerada uma quebra potencialmente alteração no momento. No entanto, os desenvolvedores confusas pelo comportamento no ASP.NET e relatórios de bugs sugerem que os desenvolvedores têm reconhecimento de existente **SetOmitVaryStar** comportamento.

No ASP.NET 4, a decisão foi feita para corrigir o problema raiz. O `Vary:*` cabeçalho HTTP não é emitido de respostas que especifique a seguinte diretiva:

`<%@OutputCache Location="ServerAndClient" %>`

Como resultado, **SetOmitVaryStar** não é mais necessário para suprimir o `Vary:*` cabeçalho.

Em aplicativos que especifiquem `Location="ServerAndClient"` no **@ OutputCache** diretiva em uma página, você verá o comportamento indicado pelo nome do **local** valor do atributo – que é, as páginas serão armazenável em cache no navegador sem a necessidade de chamar o **SetOmitVaryStar** método.

Se as páginas em seu aplicativo devem emitir `Vary:*`, chame o **AppendHeader** método, como no exemplo a seguir:

`HttpResponse.AppendHeader("Vary","*");`

Como alternativa, você pode alterar o valor de cache de saída **local** atributo "Server".

<a id="0.1__Toc255587646"></a><a id="0.1__Toc256770158"></a>

## <a name="systemwebsecurity-types-for-passport-are-obsolete"></a>Tipos de System.Web.Security Passport são obsoleto

O suporte de Passport embutido no ASP.NET 2.0 foi obsoletos e não há suporte para alguns anos devido a alterações no Passport (agora LiveID). Como resultado, os cinco tipos relacionados ao Passport no **System.Web.Security** agora são marcados com o **ObsoleteAttribute** atributo.

<a id="0.1__The_MenuItem.PopOutImageUrl_Propert"></a><a id="0.1__Toc256770159"></a>

## <a name="the-menuitempopoutimageurl-property-fails-to-render-an-image-in-aspnet-4"></a>A propriedade MenuItem.PopOutImageUrl falhar ao processar uma imagem no ASP.NET 4

No ASP.NET 3.5, o *MenuItem.PopOutImageUrl* propriedade permite que você especifique a URL de uma imagem que é exibida em um item de menu para indicar que o item de menu tem um submenu dinâmico. O exemplo a seguir mostra como especificar essa propriedade na marcação no ASP.NET 3.5.

[!code-aspx[Main](breaking-changes/samples/sample11.aspx)]

Como resultado de uma alteração de design no ASP.NET 4, nenhuma saída é renderizada para o *PopOutImageUrl* se a propriedade é definida para o *MenuItem* classe. Em vez disso, você deve especificar uma URL de imagem diretamente no *Menu* controlar usando o *StaticPopOutImageUrl* propriedade ou o *DynamicPopOutImageUrl* propriedade. Quando você trabalha com um menu estático, o *Menu.StaticPopOutImageUrl* propriedade especifica a URL para uma imagem que é exibida para indicar que o item de menu estático tem um submenu, conforme mostrado no exemplo a seguir:

[!code-aspx[Main](breaking-changes/samples/sample12.aspx)]

Se você estiver trabalhando com um menu dinâmico, você usa o *Menu.DynamicPopOutImageUrl* propriedade para especificar a URL para uma imagem que indica que um item de menu dinâmico tem um submenu. O exemplo a seguir é semelhante à anterior, mas mostra como definir o *DynamicPopOutImageUrl* propriedade para um menu dinâmico.

[!code-aspx[Main](breaking-changes/samples/sample13.aspx)]

Se o *Menu.DynamicPopOutImageUrl* propriedade não está definida e o *Menu.DynamicEnableDefaultPopOutImage* está definida como *false*, nenhuma imagem é exibida. Da mesma forma, se o *StaticPopOutImageUrl* propriedade não está definida e o *StaticEnableDefaultPopOutImage* está definida como *false*, nenhuma imagem é exibida.

Quando você definir os caminhos para essas propriedades, use uma barra invertida (/) em vez de uma barra invertida (\). Para obter mais informações, consulte [Menu.StaticPopOutImageUrl e Menu.DynamicPopOutImageUrl Falha ao renderizar imagens quando caminhos conter barras invertidas](#0.1__Menu.StaticPopOutImageUrl_and_Menu. "_Menu.StaticPopOutImageUrl_and_Menu.") em outros lugares neste documento.

<a id="0.1__Menu.StaticPopOutImageUrl_and_Menu."></a><a id="0.1__Toc256770160"></a>

## <a name="menustaticpopoutimageurl-and-menudynamicpopoutimageurl-fail-to-render-images-when-paths-contain-backslashes"></a>Menu.StaticPopOutImageUrl e Menu.DynamicPopOutImageUrl falhar renderizar imagens quando caminhos conter barras invertidas

No ASP.NET 4, as imagens que você especifica usando o *Menu.StaticPopOutImageUrl* e *Menu.DynamicPopOutImageUrl* propriedades não será processada se o caminho contiver backlashes (\). Essa é uma alteração de versões anteriores do ASP.NET.

O exemplo a seguir de *Menu* controlar marcação mostra o *StaticPopOutImageUrl* propriedade definida usando um caminho que contém uma barra invertida. No ASP.NET 4, a imagem especificada na propriedade não será renderizado.

[!code-aspx[Main](breaking-changes/samples/sample14.aspx)]

Para corrigir esse problema, altere os valores de caminho que são especificados no *StaticPopOutImageUrl* e *DynamicPopOutImageUrl* propriedades usar barras (/). O exemplo a seguir mostra essa alteração:

[!code-aspx[Main](breaking-changes/samples/sample15.aspx)]

Observe que os aplicativos que foram migrados de versões anteriores do ASP.NET para ASP.NET 4 também podem ser afetado porque o *MenuItem.PopOutImageUrl* propriedade foi alterada. Para obter mais informações, consulte [o MenuItem.PopOutImageUrl propriedade falhar ao processar uma imagem no ASP.NET 4](#0.1__The_MenuItem.PopOutImageUrl_Propert "_The_MenuItem.PopOutImageUrl_Propert") em outro lugar neste documento.

<a id="0.1__Toc224729061"></a><a id="0.1__Toc255587647"></a><a id="0.1__Toc256770161"></a>

## <a name="disclaimer"></a>Aviso de isenção de responsabilidade

Este é um documento preliminar e pode ser alterado substancialmente antes da versão comercial final do software descrito aqui.

As informações contidas neste documento representam a visão atual da Microsoft Corporation sobre os problemas discutidos a partir da data da publicação. Como a Microsoft deve responder às condições do mercado em constante mudança, este documento não deve ser interpretado como um compromisso por parte da Microsoft, e a Microsoft não pode garantir a exatidão de nenhuma informação apresentada após a data da publicação.

Este whitepaper é apenas para fins informativos. A MICROSOFT NÃO OFERECE GARANTIA, EXPRESSA, IMPLÍCITA OU ESTATUTÁRIA, DAS INFORMAÇÕES CONTIDAS NESTE DOCUMENTO.

Estar em conformidade com todas as leis de direitos autorais aplicáveis é responsabilidade do usuário. Sem limitar os direitos autorais, nenhuma parte deste documento pode ser reproduzida, armazenada ou inserida em um sistema de recuperação, ou transmitida de qualquer forma, por qualquer meio (eletrônico, mecânico, fotocópia, gravação ou outro) ou para qualquer fim, sem a permissão expressa, por escrito, da Microsoft Corporation.

A Microsoft pode ter patentes, solicitações de patente, marcas comerciais, direitos autorais ou outros direitos de propriedade intelectual que abrangem o conteúdo deste documento. A não ser que especificado expressamente em um contrato de licença da Microsoft, o fornecimento deste documento não confere a você nenhum direito sobre as supracitadas patentes, marcas comerciais, direitos autorais ou outra propriedade intelectual.

Salvo indicação em contrário, os exemplos de empresas organizações, produtos, nomes de domínio, endereços de email, logotipos, pessoas, lugares e eventos aqui descritos são fictícios e nenhuma associação com qualquer empresa, organização, produto, nome de domínio, email endereço, logotipo, pessoa, lugar ou evento é intencional ou deve ser inferido.

© 2010 Microsoft Corporation. Todos os direitos reservados.

Microsoft e Windows são marcas registradas ou comerciais da Microsoft Corporation nos Estados Unidos e/ou em outros países.

Os nomes de empresas reais e produtos mencionados aqui podem ser marcas comerciais de seus respectivos proprietários.
