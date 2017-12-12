---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: "Habilitar solicitações entre origens em ASP.NET Web API 2 | Microsoft Docs"
author: MikeWasson
description: Mostra como dar suporte a compartilhamento de recursos entre origens (CORS) na API da Web do ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/15/2014
ms.topic: article
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 453ad29ff4f10f9660f3aa8bab358519b4cfd48b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="enabling-cross-origin-requests-in-aspnet-web-api-2"></a>Habilitar solicitações entre origens em ASP.NET Web API 2
====================
por [Mike Wasson](https://github.com/MikeWasson)

> Segurança do navegador impede que uma página da web fazer solicitações do AJAX para outro domínio. Essa restrição é chamada de *política de mesma origem*e impede que um site mal-intencionado lendo dados confidenciais de outro site. No entanto, às vezes, convém permitir que outros sites chamar a API da web.
> 
> [Entre o compartilhamento de recursos de origem](http://www.w3.org/TR/cors/) (CORS) é um padrão de W3C que permite que um servidor atenuar a política de mesma origem. Usando CORS, um servidor pode permitir explicitamente algumas solicitações entre origens durante a rejeição de outras pessoas. CORS é mais segura e mais flexível que técnicas anteriores como [JSONP](http://en.wikipedia.org/wiki/JSONP). Este tutorial mostra como habilitar o CORS em seu aplicativo de API da Web.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - [Visual Studio 2013 Atualização 2](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - 2.2 da API da Web


<a id="intro"></a>
## <a name="introduction"></a>Introdução

Este tutorial demonstra o que suporte de CORS na API da Web do ASP.NET. Vamos começar criando dois projetos ASP.NET – uma chamada "serviço Web", que hospeda o controlador de API da Web, e outro chamado "WebClient", que chama o serviço Web. Como os dois aplicativos são hospedados em domínios diferentes, uma solicitação AJAX do WebClient para serviço Web é uma solicitação de entre origens.

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a>O que é "Origem mesmo"?

Duas URLs têm a mesma origem se eles tiverem portas, hosts e esquemas idênticas. ([RFC 6454](http://tools.ietf.org/html/rfc6454))

Essas duas URLs têm a mesma origem:

- `http://example.com/foo.html`
- `http://example.com/bar.html`

Essas URLs têm diferentes origens que anterior dois:

- `http://example.net`-Domínio diferente
- `http://example.com:9000/foo.html`-Porta diferente
- `https://example.com/foo.html`-Esquema diferente
- `http://www.example.com/foo.html`-Subdomínio diferente

> [!NOTE]
> Internet Explorer não considera a porta ao comparar as origens.


<a id="create-webapi-project"></a>
## <a name="create-the-webservice-project"></a>Criar o projeto de serviço Web

> [!NOTE]
> Esta seção pressupõe que você já sabe como criar projetos Web API. Se não, consulte [guia de Introdução ao ASP.NET Web API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).


Inicie o Visual Studio e crie um novo **aplicativo Web ASP.NET** projeto. Selecione o **vazio** modelo de projeto. Em "Adicionar pastas e referências de núcleo", selecione o **API da Web** caixa de seleção. Opcionalmente, selecione a opção "Host na nuvem" para implantar o aplicativo para o Microsoft Azure. A Microsoft oferece gratuito da web de hospedagem para até 10 sites em uma [conta de avaliação do Azure gratuita](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

[![](enabling-cross-origin-requests-in-web-api/_static/image3.png)](enabling-cross-origin-requests-in-web-api/_static/image2.png)

Adicionar um controlador de API da Web chamado `TestController` com o código a seguir:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

Você pode executar o aplicativo localmente ou implantar no Azure. (Para as capturas de tela neste tutorial, eu implantado em aplicativos de Web do serviço de aplicativo do Azure.) Para verificar se a API da web está funcionando, navegue até `http://hostname/api/test/`, onde *hostname* é o domínio onde você implantou o aplicativo. Você deve ver o texto de resposta, &quot;obter: mensagem de teste&quot;.

![](enabling-cross-origin-requests-in-web-api/_static/image4.png)

<a id="create-client"></a>
## <a name="create-the-webclient-project"></a>Criar o projeto de cliente

Crie outro projeto de aplicativo Web do ASP.NET e selecione o **MVC** modelo de projeto. Opcionalmente, selecione **alterar autenticação** > **sem autenticação**. Autenticação não é necessário para este tutorial.

[![](enabling-cross-origin-requests-in-web-api/_static/image6.png)](enabling-cross-origin-requests-in-web-api/_static/image5.png)

No Solution Explorer, abra o arquivo Views/Home/Index.cshtml. Substitua o código neste arquivo com o seguinte:

[!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

Para o *serviceUrl* variável, use o URI do aplicativo de serviço Web. Agora execute o aplicativo WebClient localmente ou publicá-lo em outro site.

Clicar no botão "Experimente" envia uma solicitação AJAX para o aplicativo de serviço Web, usando o método HTTP listado na caixa suspensa (GET, POST ou PUT). Isso nos permite examinar as solicitações entre origens diferentes. Agora, o aplicativo de serviço Web não dá suporte para CORS, portanto, se você clicar no botão, você receberá um erro.

![](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> Se você observar o tráfego HTTP em uma ferramenta como [Fiddler](http://www.telerik.com/fiddler), você verá que o navegador envia a solicitação de obtenção e a solicitação for bem-sucedida, mas a chamada AJAX retornará um erro. É importante entender a política de mesma origem não impede que o navegador de *enviar* a solicitação. Em vez disso, impede que o aplicativo vendo o *resposta*.


![](enabling-cross-origin-requests-in-web-api/_static/image8.png)

<a id="enable-cors"></a>
## <a name="enable-cors"></a>Habilitar o CORS

Agora vamos habilitar CORS no aplicativo do serviço Web. Primeiro, adicione o pacote NuGet de CORS. No Visual Studio, do **ferramentas** menu, selecione **Gerenciador de biblioteca de pacote**, em seguida, selecione **Package Manager Console**. Na janela do Console do Gerenciador de pacotes, digite o seguinte comando:

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

Esse comando instala o pacote mais recente e atualiza todas as dependências, inclusive as bibliotecas de API da Web principal. Usuário-sinalizador de versão para o destino de uma versão específica. O pacote CORS requer Web API 2.0 ou posterior.

Abra o arquivo de aplicativo\_Start/WebApiConfig.cs. Adicione o seguinte código para o **WebApiConfig.Register** método.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

Em seguida, adicione o **[EnableCors]** de atributo para o `TestController` classe:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

Para o *origens* parâmetro, use o URI em que você implantou o aplicativo cliente. Isso permite que solicitações entre origens de WebClient, ao mesmo tempo em que ainda não permitir todas as outras solicitações entre domínios. Posteriormente, irá descrever os parâmetros para **[EnableCors]** mais detalhadamente.

Não inclua uma barra invertida no final de *origens* URL.

Reimplante o aplicativo de serviço Web atualizado. Você não precisa atualizar WebClient. Agora a solicitação AJAX do WebClient deve ser bem-sucedida. Os métodos GET, PUT e POST todos são permitidos.

![](enabling-cross-origin-requests-in-web-api/_static/image9.png)

<a id="how-it-works"></a>
## <a name="how-cors-works"></a>Como funciona o CORS

Esta seção descreve o que acontece em uma solicitação CORS, o nível das mensagens HTTP. É importante compreender o funcionamento de CORS, para que você possa configurar o **[EnableCors]** atributo corretamente e solucionar problemas se as coisas não funcionam conforme o esperado.

A especificação CORS apresenta vários novos cabeçalhos HTTP que habilitam solicitações entre origens. Se um navegador dá suporte a CORS, ele define esses cabeçalhos automaticamente para solicitações entre origens; Você não precisa fazer nada especial em seu código JavaScript.

Aqui está um exemplo de uma solicitação entre origens. O cabeçalho de "Origem" fornece o domínio do site que está fazendo a solicitação.

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

Se o servidor permite que a solicitação, ele define o cabeçalho Access-Control-Allow-Origin. O valor desse cabeçalho corresponde o cabeçalho de origem, tanto é o valor de curinga "\*", que significa que qualquer origem é permitida.

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

Se a resposta não incluir o cabeçalho Access-Control-Allow-Origin, a solicitação AJAX falhará. Especificamente, o navegador não permite a solicitação. Mesmo se o servidor retornará uma resposta bem-sucedida, o navegador não disponibiliza a resposta para o aplicativo cliente.

**Solicitações de simulação**

Para algumas solicitações CORS, o navegador envia uma solicitação de adicional, chamada uma "solicitação de simulação," antes de enviar a solicitação real do recurso.

O navegador pode ignorar a solicitação de simulação se as seguintes condições forem verdadeiras:

- O método de solicitação é GET, HEAD ou POST, *e*
- O aplicativo não definir os cabeçalhos de solicitação diferente de idioma do conteúdo Accept, Accept-Language, Content-Type ou última--ID do evento, *e*
- O cabeçalho Content-Type (se definido) é um dos seguintes: 

    - Application/x-www-form-urlencoded
    - com diversas partes/dados de formulário
    - texto/sem formatação

A regra sobre cabeçalhos de solicitação se aplica aos cabeçalhos que o aplicativo define chamando **setRequestHeader** no **XMLHttpRequest** objeto. (A especificação CORS chama esses cabeçalhos de solicitação"autor".) A regra não se aplica aos cabeçalhos de *navegador* pode definir como o agente do usuário, o Host ou o comprimento do conteúdo.

Aqui está um exemplo de uma solicitação de simulação:

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

A solicitação de simulação usa o método HTTP OPTIONS. Ele inclui dois cabeçalhos especiais:

- Access-Control-Request-Method: O método HTTP que será usado para a solicitação real.
- Access-Control-Request-Headers: Uma lista de cabeçalhos de solicitação que o *aplicativo* definido na solicitação atual. (Novamente, isso não inclui os cabeçalhos que define o navegador.)

Aqui está um exemplo de resposta, supondo que o servidor permite que a solicitação:

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

A resposta inclui um cabeçalho de acesso--permitir-métodos de controle que lista os métodos permitidos e, opcionalmente, um cabeçalho Access-Control-permitir-Headers, que lista os cabeçalhos permitidos. Se a solicitação de simulação for bem-sucedida, o navegador envia a solicitação real, conforme descrito anteriormente.

<a id="scope"></a>
## <a name="scope-rules-for-enablecors"></a>Regras de escopo para [EnableCors]

Você pode habilitar o CORS por ação, por controlador ou globalmente para todos os controladores de API da Web em seu aplicativo.

**Cada ação**

Para habilitar o CORS para uma única ação, defina o **[EnableCors]** atributo no método de ação. O exemplo a seguir habilita o CORS para o `GetItem` método apenas.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

**Por controlador**

Se você definir **[EnableCors]** na classe de controlador, ela se aplica a todas as ações no controlador. Para desabilitar CORS para uma ação, adicione o **[DisableCors]** de atributo para a ação. O exemplo a seguir habilita o CORS para cada método, exceto `PutItem`.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

**Globalmente**

Para habilitar o CORS para todos os controladores de API da Web em seu aplicativo, passe um **EnableCorsAttribute** de instância para o **EnableCors** método:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

Se você definir o atributo em mais de um escopo, a ordem de precedência é:

1. Ação
2. Controlador
3. Global

<a id="allowed-origins"></a>
## <a name="set-the-allowed-origins"></a>Definir as origens permitidas

O *origens* parâmetro o **[EnableCors]** atributo especifica quais origens são permitidas para acessar o recurso. O valor é uma lista separada por vírgulas das origens permitidas.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

Você também pode usar o valor de curinga "\*" para permitir solicitações de qualquer origem.

Considere cuidadosamente antes de permitir solicitações de qualquer origem. Isso significa que literalmente qualquer site pode fazer chamadas AJAX para sua API da web.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

<a id="allowed-methods"></a>
## <a name="set-the-allowed-http-methods"></a>Definir os métodos HTTP permitidos

O *métodos* parâmetro o **[EnableCors]** atributo especifica quais métodos HTTP têm permissão para acessar o recurso. Para permitir que todos os métodos, use o valor de curinga "\*". O exemplo a seguir permite que somente solicitações GET e POST.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

<a id="allowed-request-headers"></a>
## <a name="set-the-allowed-request-headers"></a>Definir os cabeçalhos de solicitação permitido

Anteriormente descrito como uma solicitação de simulação pode incluir um cabeçalho Access-Control-Request-Headers, listando os cabeçalhos HTTP definidos pelo aplicativo (os chamados "author cabeçalhos de solicitação"). O *cabeçalhos* parâmetro o **[EnableCors]** atributo especifica quais cabeçalhos de solicitação de autor são permitidos. Para permitir que todos os cabeçalhos, defina *cabeçalhos* para "\*". A lista branca de cabeçalhos específicos, defina *cabeçalhos* para uma lista separada por vírgulas dos cabeçalhos permitidos:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

No entanto, os navegadores não são totalmente consistentes em como eles definidos Access-Control-Request-Headers. Por exemplo, o Chrome atualmente inclui "origem"; enquanto o FireFox não incluir cabeçalhos padrão como "Aceitar", mesmo quando o aplicativo define-los no script.

Se você definir *cabeçalhos* para algo diferente de "\*", você deve incluir pelo menos "aceitar", "content-type" e "origem", além de quaisquer cabeçalhos personalizados que você deseja dar suporte.

<a id="allowed-response-headers"></a>
## <a name="set-the-allowed-response-headers"></a>Definir os cabeçalhos de resposta permitidos

Por padrão, o navegador não expõe todos os cabeçalhos de resposta para o aplicativo. Os cabeçalhos de resposta que estão disponíveis por padrão são:

- Controle de cache
- Idioma do conteúdo
- Tipo de conteúdo
- Expirar
- Última modificação
- Pragma

A especificação CORS chama esses [cabeçalhos de resposta simples](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header). Para disponibilizar outros cabeçalhos para o aplicativo, defina o *exposedHeaders* parâmetro **[EnableCors]**.

No exemplo a seguir, o controlador `Get` método define um cabeçalho personalizado chamado 'X-personalizado-cabeçalho'. Por padrão, o navegador não irá expor esse cabeçalho em uma solicitação entre origens. Para disponibilizar o cabeçalho, inclua 'X-personalizado-cabeçalho' no *exposedHeaders*.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

<a id="credentials"></a>
## <a name="passing-credentials-in-cross-origin-requests"></a>Passagem de credenciais nas solicitações entre origens

Credenciais requerem tratamento especial em uma solicitação CORS. Por padrão, o navegador não envia credenciais com uma solicitação entre origens. As credenciais incluem cookies, bem como os esquemas de autenticação HTTP. Para enviar as credenciais com uma solicitação entre origens, o cliente deve definir **XMLHttpRequest.withCredentials** como true.

Usando **XMLHttpRequest** diretamente:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

Em jQuery:

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

Além disso, o servidor deve permitir que as credenciais. Para permitir que as credenciais de entre origens na API da Web, defina o **SupportsCredentials** propriedade como true se o **[EnableCors]** atributo:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

Se essa propriedade for true, a resposta HTTP incluirá um cabeçalho Access-controle-Allow-Credentials. Esse cabeçalho informa ao navegador que o servidor permite que as credenciais para uma solicitação entre origens.

Se o navegador envia as credenciais, mas a resposta não incluir um cabeçalho Access-controle-Allow-Credentials válido, o navegador não irá expor a resposta para o aplicativo e haverá falha na solicitação AJAX.

Tenha muito cuidado sobre configuração **SupportsCredentials** como true, pois isso significa que um site da Web em outro domínio pode enviar credenciais do usuário conectado à API da Web em nome do usuário, sem o conhecimento do usuário. A especificação CORS estados também essa configuração *origens* para &quot; \* &quot; não é válido se **SupportsCredentials** é verdadeiro.

<a id="cors-policy-providers"></a>
## <a name="custom-cors-policy-providers"></a>Provedores de política CORS personalizado

O **[EnableCors]** atributo implementa o **ICorsPolicyProvider** interface. Você pode fornecer sua própria implementação, criando uma classe que deriva de **atributo** e implementa **ICorsProlicyProvider**.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

Agora você pode aplicar o atributo de qualquer lugar que você colocaria **[EnableCors]**.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

Por exemplo, um provedor personalizado de política CORS pode ler as configurações de um arquivo de configuração.

Como alternativa ao uso de atributos, você pode registrar um **ICorsPolicyProviderFactory** objeto cria **ICorsPolicyProvider** objetos.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

Para definir o **ICorsPolicyProviderFactory**, chame o **SetCorsPolicyProviderFactory** método de extensão na inicialização, da seguinte maneira:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

<a id="browser-support"></a>
## <a name="browser-support"></a>Suporte a navegador

O pacote de Web API CORS é uma tecnologia de servidor. O navegador do usuário também precisa dar suporte a CORS. Felizmente, as versões atuais de todos os principais navegadores incluem [suporte para CORS](http://caniuse.com/cors).

Internet Explorer 8 e Internet Explorer 9 têm suporte parcial para CORS, usando o objeto herdado do XDomainRequest em vez de XMLHttpRequest. Para obter mais informações, consulte [XDomainRequest - restrições, limitações e soluções alternativas](https://blogs.msdn.com/b/ieinternals/archive/2010/05/13/xdomainrequest-restrictions-limitations-and-workarounds.aspx).
