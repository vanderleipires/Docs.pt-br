---
uid: mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
title: Prevenção de XSRF/CSRF no ASP.NET MVC e páginas da Web | Microsoft Docs
author: Rick-Anderson
description: Falsificação de solicitação entre sites (também conhecido como XSRF ou CSRF) é um ataque contra aplicativos web hospedados no qual um site mal-intencionado pode influenciar o interacti...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2013
ms.topic: article
ms.assetid: aadc5fa4-8215-4fc7-afd5-bcd2ef879728
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
msc.type: authoredcontent
ms.openlocfilehash: 6cf30daa7ed966b11405cec715c5bc803b567249
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/10/2018
---
<a name="xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages"></a>Prevenção de XSRF/CSRF no ASP.NET MVC e páginas da Web
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Solicitação intersite forjada (também conhecido como XSRF ou CSRF) é um ataque contra aplicativos web hospedados no qual um site mal-intencionado pode influenciar a interação entre o navegador do cliente e um site confiada pelo navegador. Esses ataques são possibilitados como navegadores da web enviará tokens de autenticação automaticamente com todas as solicitações para um site da web. O exemplo canônico é um cookie de autenticação, como ASP. Tíquete de autenticação de formulários do NET. No entanto, os sites da web que use qualquer mecanismo de autenticação persistente (como a autenticação do Windows, Basic e assim por diante) podem ser afetados por esses ataques.
> 
> Um ataque XSRF é diferente de um ataque de phishing. Ataques de phishing requerem interação da vítima. Em um ataque de phishing, um site mal-intencionado imitarão o site de destino e a vítima é levada a fornecer informações confidenciais ao invasor. Em um ataque de XSRF, geralmente há nenhuma interação necessárias da vítima. Em vez disso, o invasor depende do navegador automaticamente enviar todos os cookies relevantes para o site de destino.
> 
> Para obter mais informações, consulte o [Abrir projeto de segurança do aplicativo Web](https://www.owasp.org/index.php/Main_Page)(OWASP) [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)).


## <a name="anatomy-of-an-attack"></a>Anatomia de um ataque

Para percorrer um ataque XSRF, considere um usuário que deseja executar algumas transações bancárias online. Este usuário acessa primeiro WoodgroveBank.com e os logs em, no ponto em que o cabeçalho de resposta conterá o cookie de autenticação:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample1.cmd)]

Como o cookie de autenticação é um cookie de sessão, ele será automaticamente excluído pelo navegador quando o processo do navegador. No entanto, até esse momento, o navegador automaticamente incluirá o cookie com cada solicitação para WoodgroveBank.com. Agora, o usuário deseja transferir $1000 para outra conta, para que ela preenche um formulário no site de serviços bancários, e o navegador faz a solicitação para o servidor:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample2.cmd)]

Como essa operação não tem um efeito colateral (ele inicia uma transação monetária), o site de serviços bancários escolheu solicitar um HTTP POST para iniciar essa operação. O servidor lê o token de autenticação da solicitação, pesquise o número da conta do usuário atual, verifica se os fundos existe e, em seguida, inicia a transação para a conta de destino.

Ela bancárias online concluída, o usuário navega para fora do site de serviços bancários e outros locais de visitas na web. Um desses sites – fabrikam.com – inclui a seguinte marcação em uma página inserida em uma &lt;iframe&gt;:

[!code-html[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample3.html)]

Que faz com que o navegador fazer essa solicitação:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample4.cmd)]

O invasor está explorando o fato de que o usuário ainda poderá ter um token de autenticação válido para o site de destino, e ela está usando um pequeno trecho de Javascript para fazer com que o navegador tornar um HTTP POST para o site de destino automaticamente. Se o token de autenticação ainda é válido, o site de serviços bancários iniciará uma transferência de US $250 para a conta de invasores.

### <a name="ineffective-mitigations"></a>Atenuantes ineficazes

É interessante observar que, no cenário anterior, o fato que WoodgroveBank.com estava sendo acessado via SSL e teve um cookie de autenticação somente SSL foi suficiente para impedir o ataque. O invasor for capaz de especificar o [esquema URI](http://en.wikipedia.org/wiki/URI_scheme) (https) em sua &lt;formulário&gt; elemento e o navegador continuará a enviar cookies não expirados ao site de destino, desde que esses cookies são consistentes com o URI esquema de destino pretendido.

Alguém poderia argumentar que o usuário simplesmente não visite sites não confiáveis, como visitar somente sites confiáveis é ajuda a permaneçam seguros online. Há um fundo de verdade isso, mas infelizmente esse aviso nem sempre é prático. Talvez o usuário "confia" do site de notícias locais ConsolidatedMessenger. ConsolidatedMessenger.com e vai para a visita do site em vez disso, mas esse site tem uma vulnerabilidade XSS que permite que um invasor inserir o mesmo trecho de código que estava em execução no fabrikam.com.

Você pode verificar se as solicitações de entrada tem um [cabeçalho de referenciador](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14) fazendo referência a seu domínio. Isso interromperá a solicitações inadvertidamente enviadas de um domínio de terceiro. No entanto, algumas pessoas desabilitar o cabeçalho de referenciador do seu navegador por motivos de privacidade e os invasores às vezes podem falsificar esse cabeçalho se vítima tem determinados inseguro software instalado. Verificando o [cabeçalho de referenciador](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14) não é considerado uma abordagem segura para evitar ataques XSRF.

## <a name="web-stack-runtime-xsrf-mitigations"></a>Reduções de pilha em tempo de execução XSRF da Web

O tempo de execução de pilha do ASP.NET Web usa uma variante do [padrão de token sincronizador](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)_Prevention_Cheat_Sheet#General_Recommendation:_Synchronizer_Token_Pattern) para proteção contra ataques XSRF. A forma geral do padrão de token sincronizador é que os dois tokens de anti-XSRF sejam enviados para o servidor com cada HTTP POST (além do token de autenticação): um token como um cookie e o outro como um valor de formulário. Os valores do token gerados pelo tempo de execução do ASP.NET não são determinísticas ou previsível por um invasor. Quando os tokens são enviados, o servidor permitirá que a solicitação avance somente se ambos os tokens passam em uma verificação de comparação.

A verificação de solicitação XSRF *o token de sessão* é armazenado como um cookie HTTP e contém as seguintes informações na sua carga:

- Um token de segurança, consiste em um identificador de 128 bits aleatório.   
 A imagem a seguir mostra o token de sessão de verificação do XSRF solicitação exibido com as ferramentas de desenvolvedor F12 do Internet Explorer: (Observe que esta é a implementação atual e é um assunto, provavelmente, será alterado.)

![](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/_static/image1.png)

O *token de campo* é armazenado como um `<input type="hidden" />` e contém as seguintes informações na sua carga:

- Nome do usuário conectado (se autenticado).
- Dados adicionais fornecidos por um [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx).

As cargas dos tokens de anti-XSRF estão criptografadas e assinadas, para que você não pode exibir o nome de usuário ao usar as ferramentas para examinar os tokens. Quando o aplicativo web está direcionando o ASP.NET 4.0, os serviços de criptografia são fornecidos pelo [MachineKey.Encode](https://msdn.microsoft.com/library/system.web.security.machinekey.encode.aspx) rotina. Quando o aplicativo web está voltado para ASP.NET 4.5 ou superior de serviços criptográficos fornecidos pelo [Protect](https://msdn.microsoft.com/library/system.web.security.machinekey.protect(v=vs.110)) rotina, que oferece melhor desempenho, segurança e extensibilidade. Consulte que postagens de blog a seguir para obter mais detalhes:

- [Aprimoramentos de criptografia no ASP.NET 4.5, pt. 1](https://blogs.msdn.com/b/webdev/archive/2012/10/22/cryptographic-improvements-in-asp-net-4-5-pt-1.aspx)
- [Aprimoramentos de criptografia no ASP.NET 4.5, pt. 2](https://blogs.msdn.com/b/webdev/archive/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2.aspx)
- [Aprimoramentos de criptografia no ASP.NET 4.5, pt. 3](https://blogs.msdn.com/b/webdev/archive/2012/10/24/cryptographic-improvements-in-asp-net-4-5-pt-3.aspx)

## <a name="generating-the-tokens"></a>Geração de tokens

Para gerar tokens de anti-XSRF, chame o [ @Html.AntiForgeryToken ](https://msdn.microsoft.com/library/dd470175.aspx) método de um modo de exibição do MVC ou @AntiForgery.GetHtml() de uma página do Razor. O tempo de execução, em seguida, executará as seguintes etapas:

1. Se a solicitação HTTP atual já contém um token de sessão de anti-XSRF (o cookie de anti-XSRF \_ \_RequestVerificationToken), o token de segurança é extraído dele. Se a solicitação HTTP não contém um token de sessão de anti-XSRF ou se uma falha na extração do token de segurança, um novo token anti-XSRF aleatória será gerado.
2. Um token de campo de anti-XSRF é gerado usando o token de segurança da etapa (1) acima e a identidade do usuário atual fez logon. (Para obter mais informações sobre como determinar a identidade do usuário, consulte o **[cenários com suporte especial](#_Scenarios_with_special)** seção abaixo.) Além disso, se um [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/jj158328(v=vs.111).aspx) é configurado, o tempo de execução chamará o [GetAdditionalData](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.getadditionaldata(v=vs.111).aspx) método e incluir a cadeia de caracteres retornada no token de campo. (Consulte o **[configuração e extensibilidade](#_Configuration_and_extensibility)** para obter mais informações.)
3. Se um novo token anti-XSRF foi gerado na etapa (1), um token de nova sessão será criado para contê-lo e será adicionado à coleção de cookies HTTP de saída. O token de campo da etapa (2) será encapsulado em um `<input type="hidden" />` elemento e essa marcação HTML será o valor de retorno `Html.AntiForgeryToken()` ou `AntiForgery.GetHtml()`.

## <a name="validating-the-tokens"></a>Valida os tokens

Para validar os tokens de anti-XSRF a entrada, o desenvolvedor inclui um [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(VS.108).aspx) atributo em sua ação do MVC ou controlador ou chamadas she `@AntiForgery.Validate()` da sua página de Razor. O tempo de execução executará as seguintes etapas:

1. O token de sessão de entrada e o token de campo são lidos e o token anti-XSRF extraídos de cada um. Os tokens de anti-XSRF devem ser idênticos por etapa (2) na rotina de geração.
2. Se o usuário atual é autenticado, seu nome de usuário é comparado com o nome de usuário armazenado no token de campo. Os nomes de usuário devem corresponder.
3. Se um [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) estiver configurado, o tempo de execução chama seu *ValidateAdditionalData* método. O método deve retornar o valor booliano *true*.

Se a validação for bem-sucedida, a solicitação pode continuar. Se a validação falhar, o framework lançará um *HttpAntiForgeryException*.

## <a name="failure-conditions"></a>Condições de falha

Começando com o ASP.NET Web pilha Runtime v2, qualquer *HttpAntiForgeryException* que é lançada durante a validação conterá informações detalhadas sobre o que deu errado. As condições de falha definidos atualmente são:

- O token de sessão ou um token de formulário não está presente na solicitação.
- O token de sessão ou um token de formulário é ilegível. A causa mais provável disso é um farm executando versões incompatíveis do Runtime de pilha da Web ASP.NET ou um farm onde o &lt;machineKey&gt; elemento no Web. config é diferente entre as máquinas. Você pode usar uma ferramenta como o Fiddler para forçar essa exceção viole o token anti-XSRF.
- O token de sessão e o token de campo foram trocadas.
- O token de sessão e o token de campo contém tokens de segurança incompatível.
- O nome de usuário inserido dentro do token de campo não coincide com nome de usuário fez logon do usuário atual.
- O *[IAntiForgeryAdditionalDataProvider.ValidateAdditionalData](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.validateadditionaldata(v=vs.111).aspx)* retornados pelo método *false*.

As instalações de anti-XSRF também podem executar a verificação adicional durante a geração de token ou validação e falhas durante essas verificações podem resultar em exceções que está sendo geradas. Consulte o [WIF / ACS / baseada em declarações autenticação](#_WIF_ACS) e **[configuração e extensibilidade](#_Configuration_and_extensibility)** seções para obter mais informações.

<a id="_Scenarios_with_special"></a>

## <a name="scenarios-with-special-support"></a>Cenários com suporte especial

### <a name="anonymous-authentication"></a>Autenticação anônima

O sistema de anti-XSRF contém suporte especial para usuários anônimos, onde o "anônimo" é definido como um usuário onde o *IIdentity.IsAuthenticated* propriedade retorna *false*. Os cenários incluem fornecendo proteção XSRF à página de logon (antes que o usuário é autenticado) e esquemas de autenticação personalizada em que o aplicativo usa um mecanismo diferente de *IIdentity* para identificar os usuários.

Para dar suporte a esses cenários, lembre-se de que os tokens de sessão e de campo são unidos por um token de segurança, que é um identificador opaco 128 bits gerada aleatoriamente. Esse token de segurança é usado para controlar a sessão de um usuário individual conforme ela navega site, portanto ele efetivamente tem a finalidade de um identificador anônimo. Uma cadeia de caracteres vazia é usada no lugar do nome de usuário para as rotinas de geração e validação descritas acima.

<a id="_WIF_ACS"></a>

### <a name="wif--acs--claims-based-authentication"></a>O WIF / ACS / baseado em declarações de autenticação

Normalmente, o *IIdentity* classes internas para o .NET Framework tem a propriedade que *IIdentity.Name* são suficientes para identificar exclusivamente um usuário específico dentro de um aplicativo específico. Por exemplo, *FormsIdentity.Name* retorna o nome de usuário armazenado no banco de dados em associação (que é exclusivo para todos os aplicativos, dependendo do banco de dados), *WindowsIdentity.Name* retorna o domínio qualificado a identidade do usuário e assim por diante. Esses sistemas fornecem não somente a autenticação; Eles também *identificar* os usuários para um aplicativo.

Autenticação baseada em declarações, por outro lado, não necessariamente exigem a identificação de um usuário específico. Em vez disso, o *ClaimsPrincipal* e *ClaimsIdentity* tipos são associados um conjunto de *declaração* instâncias, onde as declarações individuais podem ser "é de 18 anos de idade" ou " é um administrador"para qualquer outra coisa. Como o usuário não necessariamente foi identificado, o tempo de execução não pode usar o *ClaimsIdentity.Name* a propriedade como um identificador exclusivo para esse usuário específico. A equipe viu exemplos do mundo real onde *ClaimsIdentity.Name* retorna *nulo*, retorna um nome amigável (exibição) ou, caso contrário, retorna uma cadeia de caracteres que não é apropriada para uso como um identificador exclusivo para o usuário.

Muitas implantações que usam a autenticação baseada em declarações estão usando [Azure Access Control Service](https://msdn.microsoft.com/library/windowsazure/gg429786.aspx) (ACS) em particular. ACS permite que o desenvolvedor configure individuais *provedores de identidade* (como o AD FS, o provedor do Microsoft Account, provedores de OpenID como Yahoo!, etc.), e os provedores de identidade retornam *nome identificadores*. Esses identificadores de nome podem conter informações pessoalmente identificáveis (PII) como um endereço de email, ou eles podem ser anônimas como um privada PPID (identificador pessoal). Independentemente, a tupla (provedor de identidade, identificador de nome) suficientemente serve como um token de controle apropriado para um determinado usuário enquanto ela estiver navegando no site, então o tempo de execução de pilha do ASP.NET Web pode usar a tupla no lugar do nome de usuário durante a geração e Validando tokens de campo de anti-XSRF. Os URIs específico para o provedor de identidade e o identificador do nome são:

- `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`
- `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier`

(consulte este [página do documento de ACS](https://msdn.microsoft.com/library/windowsazure/gg185971.aspx) para obter mais informações.)

Ao gerar ou validação de um token, o tempo de execução do ASP.NET Web pilha em tempo de execução tentará associação para os tipos:

- `Microsoft.IdentityModel.Claims.IClaimsIdentity, Microsoft.IdentityModel, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35` (Para o SDK do WIF.)
- `System.Security.Claims.ClaimsIdentity` (Para .NET 4.5).

Se esses tipos existem e se o usuário atual *IIIIdentity* subclasses ou implementa um desses tipos, o recurso de anti-XSRF usará (provedor de identidade, identificador de nome) tupla no lugar ao gerar o nome de usuário e valida os tokens. Se nenhuma tupla tal estiver presente, a solicitação falhará com um erro que descreve ao desenvolvedor de como configurar o sistema de anti-XSRF para entender o mecanismo de autenticação específica baseada em declarações em uso. Consulte o **[configuração e extensibilidade](#_Configuration_and_extensibility)** para obter mais informações.

### <a name="oauth--openid-authentication"></a>OAuth / OpenID autenticação

Por fim, o recurso de anti-XSRF possui suporte especial para aplicativos que usam autenticação OAuth ou OpenID. Esse suporte é baseado em heurística: se o atual *IIdentity.Name* começa com http:// ou https://, em seguida, as comparações de nome de usuário serão feitas usando uma comparação Ordinal em vez do comparador de OrdinalIgnoreCase padrão.

<a id="_Configuration_and_extensibility"></a>

## <a name="configuration-and-extensibility"></a>Configuração e extensibilidade

Ocasionalmente, os desenvolvedores podem desejar maior controle sobre a geração de anti-XSRF e comportamentos de validação. Por exemplo, talvez o comportamento padrão dos auxiliares MVC e páginas da Web adicionando automaticamente os cookies HTTP para a resposta é indesejável, e o desenvolvedor pode desejar manter os tokens em outro lugar. Existem duas APIs para ajudá-lo com isso:

`AntiForgery.GetTokens(string oldCookieToken, out string newCookieToken, out string formToken);`  
`AntiForgery.Validate(string cookieToken, string formToken);`

O *GetTokens* método utiliza como entrada um existente XSRF verificação sessão token de solicitação (que pode ser nulo) e gera como saída um novo token de sessão de verificação para solicitação XSRF e um token de campo. Os tokens são cadeias de caracteres opacas simplesmente sem decoração; o *formToken* valor para a instância não será ajustado em um &lt;entrada&gt; marca. O *newCookieToken* valor pode ser null; nesse caso, o *oldCookieToken* valor ainda é válido e não precisa ser definido nenhum novo cookie de resposta. O chamador de *GetTokens* é responsável por manter os cookies de resposta necessários ou gerar qualquer marcação necessário; o *GetTokens* próprio método não alterará a resposta como um efeito colateral. O *validar* método usa a sessão de entrada e o campo de tokens e executa a lógica de validação mencionados acima sobre eles.

### <a name="antiforgeryconfig"></a>AntiForgeryConfig

O desenvolvedor pode configurar o sistema de anti-XSRF do aplicativo\_iniciar. A configuração é através de programação. As propriedades de estático *AntiForgeryConfig* tipo são descritos abaixo. A maioria dos usuários usando declarações deve definir a propriedade UniqueClaimTypeIdentifier.

| **Property** | **Descrição** |
| --- | --- |
| **AdditionalDataProvider** | Um [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) que fornece dados adicionais durante a geração de token e consome dados adicionais durante a validação de token. O valor padrão é *nulo*. Para obter mais informações, consulte o [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) seção. |
| **CookieName** | Uma cadeia de caracteres que fornece o nome do cookie HTTP que é usado para armazenar o token de sessão de anti-XSRF. Se esse valor não for definido, um nome será gerado automaticamente com base no caminho virtual implantado do aplicativo. O valor padrão é *nulo*. |
| **RequireSsl** | Um valor booleano que determina se os tokens de anti-XSRF são necessários para serem enviados por um canal protegido por SSL. Se esse valor for *true*, os cookies automaticamente gerado terá o sinalizador "secure" definida e as APIs de anti-XSRF lançará se chamado de dentro de uma solicitação que não é enviada por meio de SSL. O valor padrão é *false*. |
| **SuppressIdentityHeuristicChecks** | Um valor booleano que determina se o sistema de anti-XSRF deve desativar o suporte a identidades baseadas em declarações. Se esse valor for *true*, o sistema assumirá que *IIdentity.Name* é adequado para uso como um identificador exclusivo por usuário e não tentará casos especiais *IClaimsIdentity*ou *ClClaimsIdentity* conforme descrito no [WIF / ACS / baseada em declarações autenticação](#_WIF_ACS) seção. O valor padrão é `false`. |
| **UniqueClaimTypeIdentifier** | Uma cadeia de caracteres que indica qual declaração de tipo é adequada para uso como um identificador exclusivo por usuário. Se esse valor é o conjunto e atual *IIdentity* é baseada em declarações, o sistema tentará extrair uma declaração do tipo especificada pelo *UniqueClaimTypeIdentifier*, e o valor correspondente será usado no lugar do nome de usuário ao gerar o token de campo. Se o tipo de declaração não for encontrado, o sistema falhará na solicitação. O valor padrão é *nulo*, que indica que o sistema deve usar (provedor de identidade, identificador de nome) tupla conforme descrita anteriormente no lugar do nome do usuário. |

<a id="_IAntiForgeryAdditionalDataProvider"></a>

### <a name="iantiforgeryadditionaldataprovider"></a>IAntiForgeryAdditionalDataProvider

O *[IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)* tipo permite aos desenvolvedores estender o comportamento do sistema anti-XSRF por dados adicionais do ciclo em cada token. O *GetAdditionalData* método é chamado sempre que um token de campo é gerado e o valor de retorno é inserido no token gerado. Um implementador pode retornar um carimbo de hora, um valor de uso único ou qualquer outro valor que ela deseja deste método.

Da mesma forma, o *ValidateAdditionalData* método é chamado sempre que um token de campo for validado, e a cadeia de caracteres "dados adicionais" foi incorporada o token é passada para o método. A rotina de validação pode implementar um tempo limite (Verificando a hora atual em relação a hora em que foi armazenada quando o token foi criado), um verificação de rotina ou qualquer outro de nonce desejado lógica.

## <a name="design-decisions-and-security-considerations"></a>Decisões de design e considerações de segurança

O token de segurança que vincula os tokens de sessão e o campo tecnicamente só é necessário ao tentar proteger os usuários não autenticados / anônimos contra ataques XSRF. Quando o usuário é autenticado, o token de autenticação em si (supostamente enviada na forma de um cookie) pode ser usado como um par de metade de um sincronizador token. No entanto, há cenários válidos para proteger as páginas de logon de ocorrências por usuários não autenticados e a lógica de anti-XSRF foi simplificada por sempre gerar e validar o token de segurança, mesmo para os usuários autenticados. Ela também fornece proteção adicional que um token de campo for comprometido por um invasor, como configuração ou adivinhar que o token de sessão seria outro obstáculo para superar o invasor.

Os desenvolvedores devem usar cuidado quando vários aplicativos são hospedados em um único domínio. Por exemplo, embora *example1.cloudapp.net* e *example2.cloudapp.net* são hosts diferentes, há uma relação de confiança implícita entre todos os hosts sob o  *\*. cloudapp.net* domínio. Essa relação de confiança implícita [permite que os hosts potencialmente não confiáveis afetam uns dos outros cookies](http://stackoverflow.com/questions/9636857/how-can-asp-net-or-asp-net-mvc-be-protected-from-related-domain-cookie-attacks) (as políticas de mesma origem que governam as solicitações do AJAX não necessariamente se aplicam a cookies HTTP). O tempo de execução de pilha do ASP.NET Web fornece algumas redução em que o nome de usuário é incorporada ao token de campo, assim mesmo se um subdomínio mal-intencionado é capaz de substituir um token de sessão, será possível gerar um token de campo válido para o usuário. No entanto, quando hospedados em um ambiente desse tipo as rotinas de anti-XSRF internas ainda não é possível proteger contra sequestro de sessão ou logon XSRF.

As rotinas de anti-XSRF atualmente não proteger contra [clickjacking](https://www.owasp.org/index.php/Clickjacking). Aplicativos que deseja proteger-se contra clickjacking podem facilmente fazer isso enviando um X-Frame-Options: cabeçalho SAMEORIGIN com cada resposta. Esse cabeçalho é compatível com todos os navegadores recentes. Para obter mais informações, consulte o [IE blog](https://blogs.msdn.com/b/ieinternals/archive/2010/03/30/combating-clickjacking-with-x-frame-options.aspx), o [blog do SDL](https://blogs.msdn.com/b/sdl/archive/2009/02/05/clickjacking-defense-in-ie8.aspx), e [OWASP](https://www.owasp.org/index.php/Clickjacking). O tempo de execução do ASP.NET Web pilha pode em alguns Verifique versão futura do MVC e auxiliares de anti-XSRF páginas da Web automaticamente defina esse cabeçalho para que os aplicativos são protegidos automaticamente contra esse ataque.

Os desenvolvedores da Web devem continuar a garantir que seu site não é vulnerável a ataques XSS. Ataques XSS são muito eficientes e uma exploração bem-sucedida também interrompe as defesas de tempo de execução do ASP.NET Web pilha contra ataques XSRF.

## <a name="acknowledgment"></a>Confirmação

[@LeviBroderick](https://twitter.com/LeviBroderick), quem criou a boa parte do código de segurança do ASP.NET a maior parte dessas informações.
