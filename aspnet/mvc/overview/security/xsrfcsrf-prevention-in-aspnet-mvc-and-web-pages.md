---
uid: mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
title: Prevenção de XSRF/CSRF no ASP.NET MVC e páginas da Web | Microsoft Docs
author: Rick-Anderson
description: Falsificação de solicitação intersite forjada (também conhecida como XSRF ou CSRF) é um ataque contra aplicativos web hospedados no qual um site mal-intencionado pode influenciar o interacti...
ms.author: riande
ms.date: 03/14/2013
ms.assetid: aadc5fa4-8215-4fc7-afd5-bcd2ef879728
msc.legacyurl: /mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
msc.type: authoredcontent
ms.openlocfilehash: 5db661cccc58d1101f95091b069ab5cbfe78a378
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577932"
---
<a name="xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages"></a>Prevenção de XSRF/CSRF no ASP.NET MVC e páginas da Web
====================
por [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Solicitação intersite forjada (também conhecida como XSRF ou CSRF) é um ataque contra aplicativos web hospedados no qual um site mal-intencionado pode influenciar a interação entre um navegador cliente e um site confiável para esse navegador. Esses ataques são possibilitados porque os navegadores da web enviarão tokens de autenticação automaticamente com cada solicitação para um site da web. O exemplo canônico é um cookie de autenticação, como ASP. Tíquete de autenticação de formulários do NET. No entanto, sites da web que usam qualquer mecanismo de autenticação persistente (como a autenticação do Windows, Basic e assim por diante) podem ser alvos desses ataques.
> 
> Um ataque XSRF é diferente de um ataque de phishing. Ataques de phishing exigem a interação da vítima. Em um ataque de phishing, um site mal-intencionado imita o site de destino e a vítima é levada a fornecer informações confidenciais ao invasor. Em um ataque XSRF, geralmente, não há nenhuma interação necessário da vítima. Em vez disso, o invasor conta com o navegador envie automaticamente todos os cookies relevantes para o site de destino.
> 
> Para obter mais informações, consulte o [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page)(OWASP) [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)).


## <a name="anatomy-of-an-attack"></a>Anatomia de um ataque

Para percorrer um ataque XSRF, considere um usuário que deseja executar algumas transações bancárias online. Primeiro, esse usuário visita WoodgroveBank.com e os logs em, no ponto em que o cabeçalho de resposta conterá seu cookie de autenticação:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample1.cmd)]

Como o cookie de autenticação é um cookie de sessão, ele será removido automaticamente pelo navegador quando o processo de navegador é fechado. No entanto, até esse momento, o navegador automaticamente incluirá o cookie com cada solicitação para WoodgroveBank.com. Agora, o usuário deseja transferir US $1000 para outra conta, portanto, ela preenche um formulário no site de serviços bancários, e o navegador faz a solicitação para o servidor:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample2.cmd)]

Como essa operação tem um efeito colateral (ele inicia uma transação monetária), o site de serviços bancários tiver optado por exigirem um HTTP POST para iniciar essa operação. O servidor lê o token de autenticação da solicitação, pesquise o número da conta do usuário atual, verifica se fundos suficientes existe e, em seguida, inicia a transação para a conta de destino.

Dela bancárias online concluída, o usuário navega para fora do site de serviços bancários e visita outros locais na web. Um desses sites – fabrikam.com – inclui a seguinte marcação em uma página inserida em uma &lt;iframe&gt;:

[!code-html[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample3.html)]

Quais, em seguida, faz com que o navegador fazer essa solicitação:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample4.cmd)]

O invasor está explorando o fato de que o usuário ainda pode ter um token de autenticação válido para o site de destino, e ela está usando um pequeno trecho de Javascript para fazer com que o navegador fizesse um HTTP POST para o site de destino automaticamente. Se o token de autenticação ainda for válido, o site de serviços bancários iniciará uma transferência de US $250 para a conta de invasores.

### <a name="ineffective-mitigations"></a>Atenuações ineficazes

É interessante observar que, no cenário acima, o fato de que WoodgroveBank.com estava sendo acessado por meio de SSL e tinha um cookie de autenticação somente SSL foi suficiente para impedir que o ataque. O invasor é capaz de especificar o [esquema de URI](http://en.wikipedia.org/wiki/URI_scheme) (https) em seu &lt;formulário&gt; elemento e o navegador continuará a enviar cookies não expirados para o site de destino, desde que esses cookies são consistentes com o URI esquema de destino pretendido.

Alguém poderia argumentar que o usuário deve simplesmente visita os sites não confiáveis, como visitar somente sites confiáveis é ajuda a permaneçam seguros online. Há alguns verdade para isso, mas infelizmente essa orientação nem sempre é prática. Talvez o usuário "confia" o site de notícias locais ConsolidatedMessenger. ConsolidatedMessenger.com e vai para a visita do site em vez disso, mas esse site tem uma vulnerabilidade XSS, que permite que um invasor inserir o mesmo trecho de código que estava em execução no fabrikam.com.

Você pode verificar que as solicitações de entrada tem um [cabeçalho Referer](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14) que referenciam seu domínio. Isso interromperá as solicitações enviadas involuntariamente de um domínio de terceiros. No entanto, algumas pessoas desabilitar o cabeçalho de referência do seu navegador por motivos de privacidade e os invasores podem falsificar, às vezes, esse cabeçalho se a vítima tem determinados software inseguro instalado. Verificando a [cabeçalho Referer](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14) não é considerado uma abordagem segura para impedir ataques XSRF.

## <a name="web-stack-runtime-xsrf-mitigations"></a>Atenuações de XSRF de tempo de execução de pilha da Web

O tempo de execução do ASP.NET Web Stack usa uma variante do [padrão de token do sincronizador](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)_Prevention_Cheat_Sheet#General_Recommendation:_Synchronizer_Token_Pattern) se defender contra ataques de XSRF. A forma geral do padrão de token de sincronizador é que dois tokens de anti-XSRF sejam enviadas para o servidor com cada HTTP POST (além do token de autenticação): um token como um cookie e o outro como um valor de formulário. Os valores do token gerados pelo tempo de execução do ASP.NET não são determinísticas ou previsível por um invasor. Quando os tokens são enviados, o servidor permitirá que a solicitação avance somente se ambos os tokens passam em uma verificação de comparação.

A verificação de solicitação XSRF *token de sessão* é armazenado como um cookie HTTP e contém as seguintes informações na sua carga:

- Um token de segurança, consistindo de um identificador aleatório de 128 bits.   
 A imagem a seguir mostra o token de sessão de verificação do XSRF solicitação exibido com as ferramentas de desenvolvedor F12 do Internet Explorer: (Observe que essa é a implementação atual e está sujeita, provavelmente alterar.)

![](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/_static/image1.png)

O *token Pole* é armazenado como um `<input type="hidden" />` e contém as seguintes informações na sua carga:

- Nome do usuário conectado (se autenticado).
- Quaisquer dados adicionais fornecidos por um [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx).

Os conteúdos dos tokens anti-XSRF são criptografados e assinados, para que você não pode exibir o nome de usuário ao usar as ferramentas para examinar os tokens. Quando o aplicativo web está direcionando o ASP.NET 4.0, os serviços de criptografia são fornecidos pelo [Encode](https://msdn.microsoft.com/library/system.web.security.machinekey.encode.aspx) rotina. Quando o aplicativo web está direcionando a ASP.NET 4.5 ou superior, criptografia de serviço é fornecido pelo [machineKey](https://msdn.microsoft.com/library/system.web.security.machinekey.protect(v=vs.110)) rotina, que oferece melhor desempenho, extensibilidade e segurança. Consulte que o blog a seguir posta para obter mais detalhes:

- [Aprimoramentos de criptografia no ASP.NET 4.5, pt. 1](https://blogs.msdn.com/b/webdev/archive/2012/10/22/cryptographic-improvements-in-asp-net-4-5-pt-1.aspx)
- [Aprimoramentos de criptografia no ASP.NET 4.5, pt. 2](https://blogs.msdn.com/b/webdev/archive/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2.aspx)
- [Aprimoramentos de criptografia no ASP.NET 4.5, pt. 3](https://blogs.msdn.com/b/webdev/archive/2012/10/24/cryptographic-improvements-in-asp-net-4-5-pt-3.aspx)

## <a name="generating-the-tokens"></a>Geração de tokens

Para gerar os tokens de anti-XSRF, chame o [ @Html.AntiForgeryToken ](https://msdn.microsoft.com/library/dd470175.aspx) método de uma exibição do MVC ou @AntiForgery.GetHtml() de uma página Razor. O tempo de execução, em seguida, executará as seguintes etapas:

1. Se a solicitação HTTP atual já contém um token de sessão anti-XSRF (o cookie de anti-XSRF \_ \_RequestVerificationToken), o token de segurança é extraído dele. Se a solicitação HTTP não contém um token de sessão anti-XSRF ou falha na extração do token de segurança, um novo token ANTI-XSRF aleatório será gerado.
2. Um token de campo de anti-XSRF é gerado usando o token de segurança da etapa (1) acima e a identidade do usuário atual conectado. (Para obter mais informações sobre como determinar a identidade do usuário, consulte o **[cenários com suporte especial](#_Scenarios_with_special)** seção abaixo.) Além disso, se um [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/jj158328(v=vs.111).aspx) é configurado, o tempo de execução chamará seu [GetAdditionalData](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.getadditionaldata(v=vs.111).aspx) método e incluir a cadeia de caracteres retornada no token de campo. (Consulte a **[configuração e extensibilidade](#_Configuration_and_extensibility)** seção para obter mais informações.)
3. Se um novo token ANTI-XSRF foi gerado na etapa (1), um novo token de sessão será criado para contê-lo e será adicionado à coleção de cookies HTTP de saída. O token de campo da etapa (2) será encapsulado em um `<input type="hidden" />` elemento e essa marcação HTML será o valor de retorno `Html.AntiForgeryToken()` ou `AntiForgery.GetHtml()`.

## <a name="validating-the-tokens"></a>Validando tokens

Para validar os tokens de entrada anti-XSRF, o desenvolvedor inclui um [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(VS.108).aspx) atributo em sua ação do MVC ou controlador ou chamadas she `@AntiForgery.Validate()` da sua página do Razor. O tempo de execução executará as seguintes etapas:

1. O token da sessão e o token de campo são lidos e o token anti-XSRF extraídos de cada um. Os tokens de anti-XSRF devem ser idênticos por etapa (2) na rotina de geração.
2. Se o usuário atual é autenticado, seu nome de usuário é comparado com o nome de usuário armazenado no token de campo. Os nomes de usuário devem corresponder.
3. Se um [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) estiver configurado, a tempo de execução chama seu *ValidateAdditionalData* método. O método deve retornar o valor booliano *verdadeira*.

Se a validação for bem-sucedida, a solicitação tem permissão para continuar. Se a validação falhar, o framework gerará uma *HttpAntiForgeryException*.

## <a name="failure-conditions"></a>Condições de falha

Começando com o Runtime do ASP.NET Web Stack v2, qualquer *HttpAntiForgeryException* que é lançada durante a validação conterá informações detalhadas sobre o que deu errado. As condições de falha definidos atualmente são:

- O token de sessão ou token de formulário não está presente na solicitação.
- O token de sessão ou token de formulário é ilegível. A causa mais provável disso é um farm que executam versões incompatíveis do Runtime do ASP.NET Web Stack ou em um farm no qual o &lt;machineKey&gt; elemento no Web. config é diferente entre as máquinas. Você pode usar uma ferramenta como o Fiddler para forçar essa exceção violando qualquer token anti-XSRF.
- O token de sessão e o token de campo foram trocadas.
- O token de sessão e o token de campo contêm tokens de segurança incompatíveis.
- O nome de usuário inserido dentro do token de campo não coincide com nome de usuário fez logon do usuário atual.
- O *[IAntiForgeryAdditionalDataProvider.ValidateAdditionalData](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.validateadditionaldata(v=vs.111).aspx)* retornados pelo método *false*.

As instalações de anti-XSRF também podem executar uma verificação adicional durante geração de token ou de validação e falhas durante essas verificações podem resultar em exceções sendo geradas. Consulte a [WIF / ACS baseado em declarações de autenticação](#_WIF_ACS) e **[configuração e extensibilidade](#_Configuration_and_extensibility)** seções para obter mais informações.

<a id="_Scenarios_with_special"></a>

## <a name="scenarios-with-special-support"></a>Cenários com suporte especial

### <a name="anonymous-authentication"></a>Autenticação anônima

O sistema de anti-XSRF contém suporte especial para usuários anônimos, onde o "anônimo" é definido como um usuário em que o *IIdentity.IsAuthenticated* propriedade retorna *falso*. Os cenários incluem fornecendo proteção XSRF à página de logon (antes do usuário é autenticado) e esquemas de autenticação personalizada, em que o aplicativo usa um mecanismo diferente de *IIdentity* para identificar os usuários.

Para dar suporte a esses cenários, lembre-se de que os tokens de sessão e campo são unidos por um token de segurança, que é um identificador opaco 128 bits gerada aleatoriamente. Esse token de segurança é usado para controlar a sessão de um usuário individual conforme ela navega o site, para que ela efetivamente serve ao propósito de um identificador anônimo. Uma cadeia de caracteres vazia é usada no lugar do nome de usuário para as rotinas de geração e validação descritas acima.

<a id="_WIF_ACS"></a>

### <a name="wif--acs--claims-based-authentication"></a>O WIF / ACS baseado em declarações de autenticação

Normalmente, o *IIdentity* classes internas para o .NET Framework tem a propriedade que *IIdentity.Name* é suficiente para identificar exclusivamente um usuário específico dentro de um aplicativo específico. Por exemplo, *FormsIdentity.Name* retorna o nome de usuário armazenado no banco de dados em associação (que é exclusivo para todos os aplicativos, dependendo do banco de dados), *WindowsIdentity.Name* retorna o identidade de domínio qualificado do usuário e assim por diante. Esses sistemas fornecem não apenas a autenticação; Eles também *identificar* usuários para um aplicativo.

Autenticação baseada em declarações, por outro lado, não necessariamente exige a identificação de um usuário específico. Em vez disso, o *ClaimsPrincipal* e *ClaimsIdentity* tipos são associados um conjunto de *declaração* instâncias, em que as declarações individuais podem ser "é de 18 anos de idade" ou " é um administrador"para qualquer outra coisa. Uma vez que o usuário necessariamente não tiver sido identificado, o tempo de execução não é possível usar o *ClaimsIdentity.Name* a propriedade como um identificador exclusivo para esse usuário específico. A equipe viu exemplos do mundo real em que *ClaimsIdentity.Name* retorna *nulo*, retorna um nome amigável (exibição) ou, caso contrário, retornará uma cadeia de caracteres que não é adequada para uso como um identificador exclusivo para o usuário.

Muitas das implantações que usam autenticação baseada em declarações estão usando [Azure Access Control Service](https://msdn.microsoft.com/library/windowsazure/gg429786.aspx) (ACS) em particular. ACS permite que o desenvolvedor configure individuais *provedores de identidade* (como o ADFS, provedor Account da Microsoft, provedores do OpenID como Yahoo!, etc.), e retornam os provedores de identidade *nomear identificadores*. Esses identificadores de nome podem conter informações pessoalmente identificáveis (PII), como um endereço de email, ou eles poderiam ser mantidos em anonimato, como uma privada PPID (identificador pessoal). Independentemente disso, a tupla (provedor de identidade, identificador de nome) suficientemente serve como um token de rastreamento apropriado para um determinado usuário enquanto ela está navegando no site, portanto, o tempo de execução do ASP.NET Web Stack pode usar a tupla no lugar do nome de usuário durante a geração e validação de tokens de campo de anti-XSRF. Os URIs específico para o provedor de identidade e o identificador de nome são:

- `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`
- `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier`

(consulte este [página de documentação do ACS](https://msdn.microsoft.com/library/windowsazure/gg185971.aspx) para obter mais informações.)

Ao gerar ou validar um token, o tempo de execução do ASP.NET Web Stack em tempo de execução tentará associação aos tipos:

- `Microsoft.IdentityModel.Claims.IClaimsIdentity, Microsoft.IdentityModel, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35` (Para o SDK do WIF.)
- `System.Security.Claims.ClaimsIdentity` (Para .NET 4.5).

Se esses tipos existem e se o usuário atual *IIIIdentity* subclasses ou implementa um desses tipos, o recurso de anti-XSRF usará (provedor de identidade, identificador de nome) tupla no lugar ao gerar o nome de usuário e Validando os tokens. Se nenhuma tupla tal estiver presente, a solicitação falhará com um erro que descreve para o desenvolvedor como configurar o sistema de anti-XSRF para entender o mecanismo de determinado autenticação baseada em declarações em uso. Consulte a **[configuração e extensibilidade](#_Configuration_and_extensibility)** seção para obter mais informações.

### <a name="oauth--openid-authentication"></a>OAuth / OpenID autenticação

Finalmente, o recurso de anti-XSRF possui suporte especial para aplicativos que usam a autenticação OAuth ou OpenID. Esse suporte é baseada em heurística: se o atual *IIdentity.Name* começa com http:// ou https://, em seguida, as comparações de nome de usuário serão feitas usando uma comparação Ordinal em vez de OrdinalIgnoreCase comparador padrão.

<a id="_Configuration_and_extensibility"></a>

## <a name="configuration-and-extensibility"></a>Configuração e extensibilidade

Ocasionalmente, os desenvolvedores talvez queiram controle mais rigoroso sobre a geração de anti-XSRF e os comportamentos de validação. Por exemplo, talvez o comportamento padrão dos auxiliares MVC e páginas da Web adicionando automaticamente os cookies HTTP para a resposta é indesejável, e o desenvolvedor pode desejar manter os tokens em outro lugar. Existem duas APIs para ajudá-lo com isso:

`AntiForgery.GetTokens(string oldCookieToken, out string newCookieToken, out string formToken);`  
`AntiForgery.Validate(string cookieToken, string formToken);`

O *GetTokens* método utiliza como entrada um XSRF solicitação verificação sessão token existente (que pode ser nulo) e produz como saída um novo token de sessão de verificação para solicitação XSRF e o token de campo. Os tokens são cadeias de caracteres opacas simplesmente sem decoração; o *formToken* valor por exemplo não será colado em um &lt;entrada&gt; marca. O *newCookieToken* valor pode ser nulo; nesse caso, o *oldCookieToken* valor ainda é válido e nenhum novo cookie de resposta precisa ser definido. O chamador de *GetTokens* é responsável por manter os cookies de resposta necessários ou gerar qualquer marcação necessário; a *GetTokens* método em si não alterará a resposta como um efeito colateral. O *validar* método usa a sessão de entrada e o campo de tokens e executa a lógica de validação mencionados anteriormente sobre eles.

### <a name="antiforgeryconfig"></a>AntiForgeryConfig

O desenvolvedor pode configurar o sistema de anti-XSRF do aplicativo\_iniciar. A configuração é através de programação. As propriedades de estática *AntiForgeryConfig* tipo são descritos abaixo. A maioria dos usuários usando declarações vai querer definir a propriedade UniqueClaimTypeIdentifier.

| **Property** | **Descrição** |
| --- | --- |
| **AdditionalDataProvider** | Uma [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) que fornece dados adicionais durante a geração de tokens e consome dados adicionais durante a validação de token. O valor padrão é *nulo*. Para obter mais informações, consulte o [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) seção. |
| **CookieName** | Uma cadeia de caracteres que fornece o nome do cookie HTTP que é usado para armazenar o token de sessão anti-XSRF. Se esse valor não for definido, um nome será automaticamente gerado com base no caminho de virtual implantada do aplicativo. O valor padrão é *nulo*. |
| **RequireSsl** | Um booliano que determina se os tokens de anti-XSRF são necessários para serem enviadas por um canal protegido por SSL. Se esse valor for *verdadeira*, nenhum cookie gerado automaticamente terá o sinalizador "secure" definida e as APIs de anti-XSRF lançará se for chamado de dentro de uma solicitação que não é enviada via SSL. O valor padrão é *false*. |
| **SuppressIdentityHeuristicChecks** | Um booliano que determina se o sistema de anti-XSRF deve desativar o suporte para identidades baseadas em declarações. Se esse valor for *verdadeira*, o sistema assumirá que *IIdentity.Name* é adequado para uso como um identificador exclusivo por usuário e não tentará casos especiais *IClaimsIdentity*ou *ClClaimsIdentity* conforme descrito na [WIF / ACS baseado em declarações de autenticação](#_WIF_ACS) seção. O valor padrão é `false`. |
| **UniqueClaimTypeIdentifier** | Uma cadeia de caracteres que indica qual declaração de tipo é adequada para uso como um identificador exclusivo por usuário. Se esse valor está definida e o atual *IIdentity* baseada em declarações, o sistema tentará extrair uma declaração do tipo especificada por *UniqueClaimTypeIdentifier*, e o valor correspondente será usado em vez do nome do usuário ao gerar o token de campo. Se o tipo de declaração não for encontrado, o sistema falhará na solicitação. O valor padrão é *nulo*, que indica que o sistema deve usar (provedor de identidade, identificador de nome) conforme descrita anteriormente no lugar do nome de usuário do usuário de tupla. |

<a id="_IAntiForgeryAdditionalDataProvider"></a>

### <a name="iantiforgeryadditionaldataprovider"></a>IAntiForgeryAdditionalDataProvider

O *[IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)* tipo permite aos desenvolvedores estender o comportamento do sistema anti-XSRF por dados adicionais de ciclo completo em cada token. O *GetAdditionalData* método é chamado sempre que um token de campo é gerado e o valor de retorno é incorporado dentro do token gerado. Um implementador poderia retornar um carimbo de hora, um nonce ou qualquer outro valor que ela deseja deste método.

Da mesma forma, o *ValidateAdditionalData* método é chamado sempre que um campo token é validado e a cadeia de caracteres de "dados adicionais" que foi inserida dentro do token é passada para o método. A rotina de validação pode implementar um tempo limite (Verificando a hora atual em relação a hora em que foi armazenada quando o token foi criado), um nonce verificação de rotina ou qualquer outro desejadas lógica.

## <a name="design-decisions-and-security-considerations"></a>Decisões de design e considerações de segurança

O token de segurança que vincula os tokens de sessão e o campo tecnicamente só é necessário ao tentar proteger os usuários não autenticados / anônimos contra ataques de XSRF. Quando o usuário é autenticado, o token de autenticação em si (supostamente enviada na forma de um cookie) pode ser usado como um par de metade de um sincronizador de token. No entanto, há cenários válidos para proteger as páginas de logon atingidas por usuários não autenticados e a lógica de anti-XSRF foi feita mais simples, sempre gerando e validar o token de segurança, mesmo para usuários autenticados. Ele também fornece alguma proteção adicional que um token do campo for comprometido por um invasor, como configuração ou o token de sessão seria outro obstáculo para o invasor superar a adivinhação.

Os desenvolvedores devem usar cuidado quando vários aplicativos são hospedados em um único domínio. Por exemplo, embora *example1.cloudapp.net* e *example2.cloudapp.net* são hosts diferentes, há uma relação de confiança implícita entre todos os hosts sob o  *\*. cloudapp.net* domínio. Essa relação de confiança implícita [permite que os hosts potencialmente não confiáveis afetam uns dos outros cookies](http://stackoverflow.com/questions/9636857/how-can-asp-net-or-asp-net-mvc-be-protected-from-related-domain-cookie-attacks) (as políticas de mesma origem que regem as solicitações AJAX não necessariamente se aplicam aos cookies HTTP). O tempo de execução do ASP.NET Web Stack fornece alguns atenuação em que o nome de usuário está incorporada ou o token de campo, portanto, mesmo que um subdomínio mal-intencionado seja capaz de substituir um token de sessão será não é possível gerar um token de campo válido para o usuário. No entanto, quando hospedados em um ambiente desse tipo as rotinas internas anti-XSRF ainda não podem se defender contra sequestro de sessão ou logon XSRF.

As rotinas de anti-XSRF no momento, não se defender contra [clickjacking](https://www.owasp.org/index.php/Clickjacking). Aplicativos que desejam se defendem contra sequestro de clique podem fazer isso facilmente, enviando um X-Frame-Options: cabeçalho SAMEORIGIN com cada resposta. Esse cabeçalho é compatível com todos os navegadores recentes. Para obter mais informações, consulte o [blog do IE](https://blogs.msdn.com/b/ieinternals/archive/2010/03/30/combating-clickjacking-with-x-frame-options.aspx), o [blog do SDL](https://blogs.msdn.com/b/sdl/archive/2009/02/05/clickjacking-defense-in-ie8.aspx), e [OWASP](https://www.owasp.org/index.php/Clickjacking). O tempo de execução do ASP.NET Web Stack pode em algum marca de versão futura do MVC e auxiliares de anti-XSRF páginas da Web defina automaticamente esse cabeçalho para que aplicativos sejam protegidos automaticamente contra esse ataque.

Os desenvolvedores da Web devem continuar a garantir que seu site não é vulnerável a ataques de XSS. Ataques de XSS são muito eficientes, e uma exploração bem-sucedida também interrompe as defesas de tempo de execução do ASP.NET Web Stack contra ataques de XSRF.

## <a name="acknowledgment"></a>Confirmação

[@LeviBroderick](https://twitter.com/LeviBroderick), que escreveu a grande parte do código de segurança do ASP.NET a maior parte dessas informações.
