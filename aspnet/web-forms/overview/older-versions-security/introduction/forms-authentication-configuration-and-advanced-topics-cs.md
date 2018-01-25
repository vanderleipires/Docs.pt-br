---
uid: web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-cs
title: "Tópicos avançados (c#) e configuração de autenticação de formulários | Microsoft Docs"
author: rick-anderson
description: "Neste tutorial, examine as várias configurações de autenticação de formulários e ver como modificá-las por meio do elemento de formulários. Isso envolvem uma detalhadas..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/14/2008
ms.topic: article
ms.assetid: b9c29865-a34e-48bb-92c0-c443a72cb860
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-cs
msc.type: authoredcontent
ms.openlocfilehash: c57722965b510ac4f5cf0c06c7c01c8cea26384f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="forms-authentication-configuration-and-advanced-topics-c"></a>Configuração de autenticação de formulários e tópicos avançados (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_03_CS.zip) ou [baixar PDF](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial03_AuthAdvanced_cs.pdf)

> Neste tutorial, examine as várias configurações de autenticação de formulários e ver como modificá-las por meio do elemento de formulários. Isso envolvem uma visão detalhada de como personalizar o valor de tempo limite do tíquete de autenticação de formulários, usando uma página de logon com uma URL personalizada (como SignIn.aspx em vez de login. aspx) e permissões de autenticação de formulários cookieless.


## <a name="introduction"></a>Introdução

No [tutorial anterior](an-overview-of-forms-authentication-cs.md) examinamos as etapas necessárias para implementar a autenticação de formulários em um aplicativo ASP.NET, especificando definições de configuração no Web. config para criar um registro na página para exibir diferentes conteúdo para usuários anônimos e autenticados. Lembre-se de que configuramos o site para usar a autenticação de formulários, definindo o atributo de modo do &lt;autenticação&gt; elemento formulários. O &lt;autenticação&gt; elemento opcionalmente pode incluir um &lt;formulários&gt; elemento filho, por meio do qual uma variedade de configurações de autenticação de formulários pode ser especificada.

Neste tutorial, examine as várias configurações de autenticação de formulários e ver como modificá-los por meio de &lt;formulários&gt; elemento. Isso envolvem uma visão detalhada de como personalizar o valor de tempo limite do tíquete de autenticação de formulários, usando uma página de logon com uma URL personalizada (como SignIn.aspx em vez de login. aspx) e permissões de autenticação de formulários cookieless. Também vamos examinar a composição de tíquete de autenticação de formulários mais detalhadamente e consulte as precauções que ASP.NET leva para garantir que os dados do tíquete são seguros de inspeção e violação. Por fim, examinaremos como armazenar dados de usuário adicionais no tíquete de autenticação de formulários e modele esses dados por meio de um objeto de entidade personalizado.

## <a name="step-1-examining-the-ltformsgt-configuration-settings"></a>Etapa 1: Examinando o &lt;formulários&gt; definições de configuração

O sistema de autenticação de formulários do ASP.NET oferece uma série de configurações que podem ser personalizados em uma base por aplicativo. Isso inclui configurações como: o tempo de vida de autenticação de formulários de tíquete; que tipo de proteção é aplicado a permissão; em qual autenticação cookieless condições, as permissões são usadas; o caminho para a página de logon. e outras informações. Para modificar os valores padrão, adicione um [ &lt;formulários&gt; elemento](https://msdn.microsoft.com/library/1d3t3c61.aspx) como um filho de [ &lt;autenticação&gt; elemento](https://msdn.microsoft.com/library/532aee0e.aspx), especificando as propriedades valores para personalizar como os atributos XML da seguinte forma:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample1.xml)]

A tabela 1 resume as propriedades que podem ser personalizadas por meio de &lt;formulários&gt; elemento. Como o Web. config é um arquivo XML, os nomes de atributo na coluna esquerda diferenciam maiusculas de minúsculas.

| **Atributo** | **Descrição** |
| --- | --- |
| cookieless | Esse atributo especifica em quais condições o tíquete de autenticação é armazenado em um cookie versus sendo inserido na URL. Valores permitidos são: UseCookies; UseUri; Detecção automática; e UseDeviceProfile (o padrão). Etapa 2 examina essa configuração em mais detalhes. |
| defaultUrl | Indica a URL que os usuários são redirecionados para depois de entrar na página de logon, se não houver nenhum valor RedirectUrl especificado na querystring. O valor padrão é default. aspx. |
| domínio | Ao usar permissões de autenticação baseada em cookie, essa configuração especifica o valor de domínio do cookie. O valor padrão é uma cadeia de caracteres vazia, o que faz com que o navegador para usar o domínio do qual ele foi emitido (como www.yourdomain.com). Nesse caso, o cookie será **não** ser enviada ao fazer solicitações para sub-domínios, como admin.yourdomain.com. Se você deseja que o cookie a ser passado para todos os subdomínios que você precisará personalizar o atributo de domínio definido como seudomínio. |
| enableCrossAppRedirects | Um valor booliano que indica se os usuários autenticados serão lembrados quando redirecionado para URLs em outros aplicativos web no mesmo servidor. O padrão é falso. |
| loginUrl | A URL da página de logon. O valor padrão é login.aspx. |
| name | Ao usar os tíquetes de autenticação baseada em cookie, o nome do cookie. O padrão é. ASPXAUTH. |
| path | Ao usar permissões de autenticação baseada em cookie, essa configuração especifica o atributo de caminho do cookie. O atributo de caminho permite que um desenvolvedor limitar o escopo de um cookie para uma hierarquia de diretórios específica. O valor padrão é /, que informa ao navegador para enviar o cookie de tíquete de autenticação a qualquer solicitação feita no domínio. |
| proteção | Indica as técnicas usadas para proteger o tíquete de autenticação de formulários. Os valores permitidos são: todos (padrão); Criptografia; Nenhum; e a validação. Essas configurações são discutidas em detalhes na etapa 3. |
| requireSSL | Um valor booliano que indica se uma conexão SSL é necessária para transmitir o cookie de autenticação. O valor padrão é false. |
| slidingExpiration | Um valor booliano que indica que se o tempo de limite do cookie de autenticação é redefinido cada vez que o usuário acessa o site durante uma única sessão. O valor padrão é true. A política de tempo limite de tíquete de autenticação é discutida mais detalhadamente a especificação seção do valor de tempo limite de tíquete. |
| Tempo limite | Especifica o tempo, em minutos, após o qual o cookie de tíquete de autenticação expira. O valor padrão é 30. A política de tempo limite de tíquete de autenticação é discutida mais detalhadamente a especificação seção do valor de tempo limite de tíquete. |

**Tabela 1**: um resumo do &lt;formulários&gt; atributos do elemento

No ASP.NET 2.0 e posteriores, o padrão valores de autenticação de formulários são codificados na classe FormsAuthenticationConfiguration no .NET Framework. Todas as modificações devem ser aplicadas em uma base por aplicativo no arquivo Web. config. Isso é diferente do ASP.NET 1. x, onde os valores de autenticação de formulários padrão foram armazenados no arquivo Machine. config (e, portanto, podem ser modificados por meio da edição de Machine. config). Enquanto no tópico do ASP.NET 1. x, vale a pena mencionar que um número das configurações de sistema de autenticação de formulários têm valores padrão diferentes no ASP.NET 2.0 e além do que no ASP.NET 1. x. Se você estiver migrando seu aplicativo de um ambiente do ASP.NET 1. x, é importante estar atento essas diferenças. Consulte [o &lt;formulários&gt; documentação técnica do elemento](https://msdn.microsoft.com/library/1d3t3c61.aspx) para obter uma lista das diferenças.

> [!NOTE]
> Várias configurações de autenticação de formulários, como o tempo limite, o domínio e o caminho, especificam os detalhes para o cookie de tíquete de autenticação de formulários resultante. Para obter mais informações sobre cookies, como elas funcionam e as várias propriedades, leia [este tutorial Cookies](http://www.quirksmode.org/js/cookies.html).


### <a name="specifying-the-tickets-timeout-value"></a>Especificando o valor de tempo limite de tíquete

O tíquete de autenticação de formulários é um token que representa uma identidade. Com permissões de autenticação baseada em cookie, esse token é mantido na forma de um cookie e enviado para o servidor web em cada solicitação. A posse do token, em essência, declara, estou *nome de usuário*, posso ter feito logon e é usado para que a identidade do usuário pode ser lembrada em visitas à página.

O tíquete de autenticação de formulários não só inclui a identidade do usuário, mas também contém informações para ajudar a garantir a integridade e a segurança do token. Após todos os nós não desejamos um usuário mal-intencionado para poder criar um token falsificado ou modificar um token legal de alguma forma underhanded.

Um esse bit das informações incluídas na permissão é um *expiração*, que é a data e hora que o tíquete não é mais válido. Cada vez que o FormsAuthenticationModule inspeciona um tíquete de autenticação, ele garante a expiração do tíquete ainda não tiver passado. Em caso afirmativo, ele ignora o tíquete e identifica o usuário como anônimo. Essa proteção ajuda a proteger contra ataques de repetição. Sem uma expiração, se um hacker conseguiu obter suas mãos no tíquete de autenticação válido do usuário - talvez tenham acesso físico ao seu computador e meio de seus cookies, eles podem enviar uma solicitação para o servidor com esse tíquete de autenticação roubado e obter a entrada. Enquanto a expiração não impede que esse cenário, limita a janela durante o qual esse ataque pode ser bem-sucedida.

> [!NOTE]
> Etapa 3 técnicas adicionais de detalhes usadas pelo sistema de autenticação de formulários para proteger o tíquete de autenticação.


Ao criar o tíquete de autenticação, o sistema de autenticação de formulários determina sua expiração consultando a configuração de tempo limite. Conforme observado na tabela 1, o tempo limite de configuração padrão é 30 minutos, o que significa que, quando o tíquete de autenticação de formulários é criado sua expiração é definida como uma data e hora 30 minutos no futuro.

A expiração define um tempo absoluto no futuro quando expira o tíquete de autenticação de formulários. Mas normalmente os desenvolvedores desejam implementar uma expiração deslizante, que é redefinida toda vez que o usuário revisitar o site. Esse comportamento é determinado pelas configurações de slidingExpiration. Se definido como true (o padrão), cada vez que o FormsAuthenticationModule autentica um usuário, atualiza expiração do tíquete. Se definido como false, a expiração não é atualizado em cada solicitação, causando o tíquete para expirar exatamente tempo limite número de minutos passados quando a permissão foi inicialmente criado.

> [!NOTE]
> A expiração armazenada no tíquete de autenticação é uma data absoluta e o valor de tempo, como em 2 de agosto de 2008 11:34:00. Além disso, a data e hora estão em relação a hora do local do servidor web. Essa decisão de design pode ter alguns efeitos colaterais da interessantes em torno do horário de verão (DST), que é quando os relógios dos Estados Unidos são movidos em frente uma hora (supondo que o servidor web é hospedado em um local em que o horário de verão é observado). Considere o que aconteceria para um site ASP.NET com uma expiração de 30 minutos próximo a hora de início do horário de verão (que é às 2:00 AM). Imagine que um visitante faz logon site de em 11 de março de 2008 à 1H00: 55. Isso poderia gerar um tíquete de autenticação de formulários que expira no dia 11 de março de 2008, às 2h: 25: 00 (30 minutos no futuro). No entanto, quando chegar às 2H, o relógio salta para 3:00 AM devido ao horário de verão. Quando o usuário carregar uma nova página seis minutos depois de entrar (às 3:01 AM), o FormsAuthenticationModule observa que o tíquete expirou e redireciona o usuário para a página de logon. Para obter uma discussão mais completa sobre esse e outros Singularidades de tempo limite de tíquete de autenticação, bem como soluções alternativas, escolha uma cópia do Stefan Schackow *Professional ASP.NET 2.0 segurança, associação e gerenciamento de função* (ISBN: 978-0-7645-9698-8).


A Figura 1 ilustra o fluxo de trabalho quando slidingExpiration é definido como false e o tempo limite é definido como 30. Observe que o tíquete de autenticação gerado no logon contém a data de validade, e esse valor não é atualizado em solicitações subsequentes. Se o FormsAuthenticationModule localiza o tíquete expirou, ele descarta e trata a solicitação como anônimo.


[![Uma representação gráfica dos slidingExpiration do tíquete de autenticação de formulários de expiração quando for falsa](forms-authentication-configuration-and-advanced-topics-cs/_static/image2.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image1.png)

**Figura 01**: representação gráfica de um dos slidingExpiration do tíquete de autenticação de formulários de expiração quando for falsa ([clique para exibir a imagem em tamanho normal](forms-authentication-configuration-and-advanced-topics-cs/_static/image3.png))


A Figura 2 mostra o fluxo de trabalho quando slidingExpiration é definido como true e o tempo limite é definido como 30. Quando é recebida uma solicitação autenticada (com um tíquete não expiradas) sua expiração é atualizada para o tempo limite o número de minutos no futuro.


[![Uma representação gráfica do tíquete de autenticação de formulários quando slidingExpiration é true](forms-authentication-configuration-and-advanced-topics-cs/_static/image5.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image4.png)

**Figura 02**: representação gráfica de um do tíquete de autenticação de formulários quando slidingExpiration é true ([clique para exibir a imagem em tamanho normal](forms-authentication-configuration-and-advanced-topics-cs/_static/image6.png))


Ao usar permissões de autenticação baseada em cookie (o padrão), esta discussão se torna um pouco mais confusa porque cookies também podem ter seus próprios expirações especificadas. Expiração de um cookie (ou não) instrui o navegador quando o cookie deve ser destruído. Se o cookie não tiver uma expiração, ele é destruído quando o navegador é desligado. Se houver uma expiração, no entanto, o cookie permanecerá armazenado no computador do usuário até a data e horário de expiração especificado tiver passado. Quando um cookie é destruído pelo navegador, ele não será mais enviado ao servidor web. Portanto, a destruição de um cookie é análoga ao usuário log fora do site.

> [!NOTE]
> Obviamente, é possível que um usuário proativamente remover qualquer cookies armazenados em seu computador. No Internet Explorer 7, você poderia acesse ferramentas, opções e clique no botão Excluir na seção de histórico de navegação. A partir daí, clique no botão de cookies de exclusão.


O sistema de autenticação de formulários cria cookies baseada em sessão ou expiração dependendo do valor passado para o *persistCookie* parâmetro. Lembre-se que os FormsAuthentication métodos da classe GetAuthCookie, SetAuthCookie e RedirectFromLoginPage levar dois parâmetros de entrada: *username* e *persistCookie*. A página de logon que são criados no tutorial anterior incluído um CheckBox lembrar-me, que determinar se um cookie persistente foi criado. Cookies persistentes são baseados em expiração; cookies não são persistentes são baseadas em sessão.

Os tempo limite e slidingExpiration conceitos já discutidos aplicam o mesmo para ambos os cookies com base em sessão e expiração. Há apenas uma diferença mínima em execução: ao usar cookies de expiração com slidingTimeout definido como true, expiração do cookie só é atualizada quando mais de metade do tempo especificado tiver decorrido.

Vamos atualizar políticas de tempo limite de tíquete de autenticação do nosso site para que tíquetes do tempo limite depois de uma hora (60 minutos), usando uma expiração deslizante. Para efetuar essa alteração, atualize o arquivo Web. config, adicionando um &lt;formulários&gt; elemento para o &lt;autenticação&gt; elemento com a seguinte marcação:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample2.xml)]

### <a name="using-an-login-page-url-other-than-loginaspx"></a>Usando uma URL de página de logon diferente aspx

Desde o FormsAuthenticationModule redireciona automaticamente os usuários não autorizados a página de logon, é necessário saber a URL da página de logon. Essa URL é especificado pelo atributo loginUrl no &lt;formulários&gt; elemento e o padrão é login.aspx. Se você estiver portando em um site existente, você talvez já tenha uma página de logon com uma URL diferente, que já o indicador e indexado por mecanismos de pesquisa. Em vez de renomear a página de logon existente para login.aspx e recentes links e marcadores de usuários, em vez disso, você pode modificar o atributo loginUrl para apontar para a página de logon.

Por exemplo, se a página de logon foi nomeada SignIn.aspx e foi localizada no diretório de usuários, pode apontar a configuração loginUrl para ~/Users/SignIn.aspx da seguinte forma:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample3.xml)]

Como nosso aplicativo atual já tem uma página de logon chamada aspx, não é necessário especificar um valor personalizado no &lt;formulários&gt; elemento.

## <a name="step-2-using-cookieless-forms-authentication-tickets"></a>Etapa 2: Uso de tíquetes de autenticação de formulários Cookieless

Por padrão, o sistema de autenticação de formulários determina se armazenar suas permissões de autenticação na coleção de cookies ou inseri-los na URL com base no agente do usuário visitando o site. Todos os principais navegadores de desktop como Internet Explorer, Firefox, Opera e Safari, cookies de suporte, mas não todos os dispositivos móveis.

A política de cookie usada pelo sistema de autenticação de formulários depende na configuração de cookieless o &lt;formulários&gt; elemento, que pode ser atribuído um destes quatro valores:

- UseCookies - Especifica que os tíquetes de autenticação baseada em cookie sempre serão usados.
- UseUri - indica que os tíquetes de autenticação baseada em cookie nunca serão usados.
- Detecção automática - se o perfil do dispositivo não dá suporte a cookies, tíquetes de autenticação baseada em cookie não são usados; Se o perfil do dispositivo der suporte a cookies, um mecanismo de sondagem é usado para determinar se os cookies estão habilitados.
- UseDeviceProfile - padrão. usa os tíquetes de autenticação baseada em cookie somente se o perfil do dispositivo der suporte a cookies. Nenhum mecanismo de sondagem é usado.

As configurações de detecção automática e UseDeviceProfile dependem um *perfil do dispositivo* no averiguar se deseja usar tíquetes de autenticação baseada em cookie ou não em cookies. O ASP.NET mantém um banco de dados de vários dispositivos e seus recursos, como se eles dão suporte a cookies, qual versão do JavaScript dão suporte e assim por diante. Cada vez que um dispositivo solicita uma página da web de um servidor da web envia ao longo de um *agente do usuário* cabeçalho HTTP, que identifica o tipo de dispositivo. ASP.NET corresponde automaticamente a cadeia de caracteres de agente do usuário fornecido com o perfil correspondente especificado no banco de dados.

> [!NOTE]
> Este banco de dados de recursos do dispositivo é armazenado em um número de arquivos XML que seguem o [esquema de arquivo de definição de navegador](https://msdn.microsoft.com/library/ms228122.aspx). Os arquivos de perfil de dispositivo padrão estão localizados em % WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG\Browsers. Você também pode adicionar arquivos personalizados para o aplicativo do seu aplicativo\_pasta navegadores. Para obter mais informações, consulte [como: detectar tipos de navegador em páginas da Web do ASP.NET](https://msdn.microsoft.com/library/3yekbd5b.aspx).


Como a configuração padrão é UseDeviceProfile, tíquetes de autenticação de formulários cookieless serão usados quando o site for visitado por um dispositivo cujo perfil informa que não dá suporte a cookies.

### <a name="encoding-the-authentication-ticket-in-the-url"></a>O tíquete de autenticação na URL de codificação

Os cookies são um meio natural para incluir informações do navegador em cada solicitação para um site específico, por isso, as configurações de autenticação de formulários padrão usam cookies se o dispositivo visitando oferece suporte a eles. Se não há suporte para cookies, um meio alternativo para passar o tíquete de autenticação do cliente para o servidor deve ser empregado. É uma solução comum usada em ambientes sem cookies codificar os dados do cookie na URL.

É a melhor maneira de ver como essas informações podem ser incorporadas na URL de forçar o site para usar os tíquetes de autenticação sem cookies. Isso pode ser feito definindo o parâmetro de configuração cookieless para UseUri:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample4.xml)]

Depois de fazer essa alteração, visite o site por meio de um navegador. Ao visitar como um usuário anônimo, as URLs parecerá exatamente como antes. Por exemplo, ao visitar a página Default.aspx barra de endereços do navegador mostra a URL a seguir:

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/default.aspx`

No entanto, após o logon, o tíquete de autenticação de formulários é incorporado na URL. Por exemplo, depois de visitar a página de logon e registro em log como Sam, eu estou retornado para a página Default.aspx, mas a URL neste momento é:

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/default.aspx`

O tíquete de autenticação de formulários foi incorporado na URL. A cadeia de caracteres (F (jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv rfezq0gKadKX0YPZCkA2) representa as informações do tíquete de autenticação codificada em hexadecimal e são os mesmos dados que geralmente são armazenados em um cookie.

Para tíquetes de autenticação cookieless funcione, o sistema deve codificar todas as URLs na página para incluir os dados de tíquete de autenticação, caso contrário, o tíquete de autenticação serão perdido quando o usuário clica em um link. Felizmente, essa lógica de incorporação é executada automaticamente. Para demonstrar essa funcionalidade, abra a página Default.aspx e adicionar um controle de hiperlink, definindo suas propriedades de texto e NavigateUrl para testar Link e SomePage.aspx, respectivamente. Não importa que existe realmente não uma página em nosso projeto chamado SomePage.aspx.

Salvar as alterações em Default.aspx e, em seguida, visite-o por meio de um navegador. Faça logon no site para que o tíquete de autenticação de formulários é incorporado na URL. Em seguida, de Default.aspx, clique no link de Link de teste. O que aconteceu? Se não houver nenhuma página chamada SomePage.aspx, em seguida, um erro 404 ocorreu, mas isso não é o que é importante aqui. Em vez disso, concentre-se na barra de endereço do seu navegador. Observe que ele inclui o tíquete de autenticação de formulários na URL!

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/SomePage.aspx`

O SomePage.aspx de URL no link foi automaticamente convertido em uma URL que incluído o tíquete de autenticação - não tivemos que gravar uma linha de código! O tíquete de autenticação de formulário será inserido automaticamente na URL para todos os hiperlinks não começar com http:// ou /. Não importa se o hiperlink é exibido em uma chamada para Response. Redirect, em um controle de hiperlink ou em um elemento âncora HTML (ou seja, &lt;um href = "..."&gt;... &lt;/a&gt;). A URL não é algo como http://www.someserver.com/SomePage.aspx ou /SomePage.aspx, desde que o tíquete de autenticação de formulários será inserido para nós.

> [!NOTE]
> Os tíquetes de autenticação de formulários cookieless aderem às mesmas políticas de tempo limite como tíquetes de autenticação baseada em cookie. No entanto, os tíquetes de autenticação sem cookies são mais propensas a ataques de repetição, desde que o tíquete de autenticação é inserido diretamente na URL. Imagine um usuário que visita um site, faz logon e cola a URL em um email para um colega. Se o colega clicar nesse link antes de atingir a expiração, eles são registrados que o usuário que enviou o email!


## <a name="step-3-securing-the-authentication-ticket"></a>Etapa 3: Protegendo o tíquete de autenticação

O tíquete de autenticação de formulários é transmitido pela rede em um cookie ou inseridas diretamente na URL. Além das informações de identidade, o tíquete de autenticação também pode incluir dados de usuário (como veremos na etapa 4). Consequentemente, é importante que os dados do tíquete são criptografados das pessoas curiosas e (ainda mais importante) que o sistema de autenticação de formulários pode garantir que o tíquete não foi violado.

Para garantir a privacidade dos dados de tíquete, o sistema de autenticação de formulários pode criptografar os dados de tíquete. Falha ao criptografar os dados do tíquete envia informações potencialmente confidenciais eletronicamente em texto sem formatação.

Para garantir a autenticidade do tíquete, o sistema de autenticação de formulários deve *validar* o tíquete. A validação é o ato de garantir que um determinado tipo de dados não tiver sido modificado e é realizado por meio de um  *[(MAC) de código de autenticação de mensagem](http://en.wikipedia.org/wiki/Message_authentication_code)*. Em resumo, o MAC é uma pequeno pedaço de informações que identifiquem os dados que precisam ser validada (nesse caso, o tíquete). Se os dados representados por MAC são modificados, em seguida, o MAC e os dados não corresponde a. Além disso, é computacionalmente difícil para um hacker modificar os dados tanto gerar seu próprio MAC para corresponder com os dados modificados.

Ao criar (ou modificar) um tíquete, o sistema de autenticação de formulários cria um MAC e a anexa ao dados do tíquete. Quando chega uma solicitação subsequente, o sistema de autenticação de formulários compara os dados de MAC e tíquete para validar a autenticidade dos dados de tíquete. A Figura 3 ilustra o fluxo de trabalho graficamente.


[![Autenticidade do tíquete é garantida por meio de um MAC](forms-authentication-configuration-and-advanced-topics-cs/_static/image8.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image7.png)

**Figura 03**: autenticidade o tíquete é garantida por meio de um MAC ([clique para exibir a imagem em tamanho normal](forms-authentication-configuration-and-advanced-topics-cs/_static/image9.png))


Quais medidas de segurança são aplicadas para o tíquete de autenticação depende da configuração de proteção no &lt;formulários&gt; elemento. A configuração de proteção pode ser atribuída a um dos três valores a seguir:

- Todos os - o tíquete é criptografado e assinada digitalmente (o padrão).
- Criptografia - criptografia de somente é aplicada - nenhuma MAC é gerado.
- Nenhum - o tíquete é criptografado nem assinado digitalmente.
- Validação - um MAC é gerada, mas os dados do tíquete são enviados eletronicamente em texto sem formatação.

A Microsoft recomenda expressamente usando a configuração de todos os.

### <a name="setting-the-validation-and-decryption-keys"></a>Configurando a validação e chaves de descriptografia

A criptografia e algoritmos usados pelo sistema de autenticação de formulários para criptografar e validar o tíquete de autenticação de hash são personalizáveis por meio de [ &lt;machineKey&gt; elemento](https://msdn.microsoft.com/library/w8h3skw9.aspx) no Web. config. A tabela 2 descreve o &lt;machineKey&gt; atributos do elemento e seus valores possíveis.

| **Atributo** | **Descrição** |
| --- | --- |
| descriptografia | Indica o algoritmo usado para criptografia. Esse atributo pode ter um dos quatro valores a seguir: - Autopadrão. Determina o algoritmo com base no comprimento do atributo decryptionKey. -AES - usa a [padrão de criptografia avançada (AES)](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) algoritmo. -DES - usa a [padrão de criptografia de dados (DES)](http://en.wikipedia.org/wiki/Data_Encryption_Standard) esse algoritmo é considerado em termos computacionais fraco e não deve ser usado. -3DES - usa a [Triple DES](http://en.wikipedia.org/wiki/Triple_DES) algoritmo, o que funciona aplicando o algoritmo DES triplo. |
| decryptionKey | A chave secreta usada pelo algoritmo de criptografia. Esse valor deve ser uma cadeia de caracteres hexadecimal do tamanho apropriado (com base no valor na descriptografia), gerar automaticamente ou um valor concatenado, IsolateApps. Adicionar IsolateApps instrui o ASP.NET para usar um valor exclusivo para cada aplicativo. O padrão é gerar automaticamente, IsolateApps. |
| validação | Indica o algoritmo usado para validação. Esse atributo pode ter um dos quatro valores a seguir: - AES - usa o algoritmo de criptografia AES (Advanced Standard). -MD5 - usa a [Message Digest 5 (MD5)](http://en.wikipedia.org/wiki/MD5) algoritmo. -SHA1 - usa a [SHA1](http://en.wikipedia.org/wiki/Sha1) algoritmo (o padrão). -3DES - usa o algoritmo DES triplo. |
| validationKey | A chave secreta usada pelo algoritmo de validação. Esse valor deve ser uma cadeia de caracteres hexadecimal do tamanho apropriado (com base no valor de validação), gerar automaticamente ou um valor concatenado, IsolateApps. Adicionar IsolateApps instrui o ASP.NET para usar um valor exclusivo para cada aplicativo. O padrão é gerar automaticamente, IsolateApps. |

**Tabela 2**: O &lt;machineKey&gt; atributos do elemento

Uma discussão completa sobre essas opções de criptografia e validação e os profissionais e os contras de vários algoritmos, estão além do escopo deste tutorial. Para um profundo examinar esses problemas, incluindo orientação sobre quais algoritmos de criptografia e validação de usar, quais comprimentos de chave a ser usado e a melhor maneira de gerar essas chaves, consulte *Professional ASP.NET 2.0 segurança, associação e gerenciamento de função* .

Por padrão, as chaves usadas para criptografia e validação são geradas automaticamente para cada aplicativo, e essas chaves são armazenadas na autoridade de segurança Local (LSA). Em resumo, as configurações padrão garantem que as chaves exclusivas em um servidor de web, servidor web e pelo aplicativo. Consequentemente, esse comportamento padrão não funcionará para os dois seguintes cenários:

- **Web Farms** - em um [farm da web](http://en.wikipedia.org/wiki/Web_farm) cenário, um único aplicativo web é hospedado em vários servidores web para fins de redundância e escalabilidade. Cada solicitação de entrada é enviada a um servidor no farm, que significa que durante a vida útil de uma sessão de usuário, servidores diferentes podem ser usados para manipular seus várias solicitações. Consequentemente, cada servidor deve usar as mesmas chaves de criptografia e validação de forma que o tíquete de autenticação de formulários é criado, criptografado e validado em um servidor pode ser descriptografado e validado em um servidor diferente no farm.
- **Entre o aplicativo de compartilhamento de tíquete** -um único servidor web pode hospedar vários aplicativos ASP.NET. Se você precisar para esses aplicativos diferentes compartilhar um tíquete de autenticação de formulários simples, é essencial que as chaves de criptografia e validação de correspondam.

Ao trabalhar em uma web farm configuração ou o compartilhamento de tíquetes de autenticação em todos os aplicativos no mesmo servidor, você precisará configurar o &lt;machineKey&gt; elemento nos aplicativos afetados para que seus decryptionKey e valores de validationKey correspondem.

Enquanto a nenhum dos cenários acima se aplica ao nosso aplicativo de exemplo, podemos ainda especificar valores de validationKey e decryptionKey explícita e definir os algoritmos a ser usado. Adicionar um &lt;machineKey&gt; configuração para o arquivo Web. config:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample5.xml)]

Para obter mais informações consulte [como: configurar MachineKey no ASP.NET 2.0](https://msdn.microsoft.com/library/ms998288.aspx).

> [!NOTE]
> Os valores decryptionKey e validationKey foram tirados da [Steve Gibson](http://www.grc.com/stevegibson.htm)do [página da web de senhas perfeito](https://www.grc.com/passwords.htm), que gera a 64 caracteres hexadecimais aleatórios em cada página visita. Para reduzir a probabilidade de que essas chaves fazer sua maneira em seus aplicativos de produção, você é incentivado a substituir as chaves acima por gerado aleatoriamente da página de senhas perfeita.


## <a name="step-4-storing-additional-user-data-in-the-ticket"></a>Etapa 4: Armazenando dados de usuário adicionais no tíquete

Muitos aplicativos web exibem informações sobre ou exibição da página de base no usuário conectado no momento. Por exemplo, uma página da web pode mostrar o nome do usuário e a data em que ela último logon no canto superior de cada página. O tíquete de autenticação de formulários armazena o nome do usuário conectado no momento, mas quando qualquer outra informação é necessária, a página deve ir para o repositório do usuário - normalmente um banco de dados - para pesquisar as informações não são armazenadas no tíquete de autenticação.

Com um pouco de código, pode armazenar informações de usuário adicionais no tíquete de autenticação de formulários. Esses dados podem ser expressos por meio de [classe FormsAuthenticationTicket](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx)do [UserData propriedade](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.userdata.aspx). Isso é um local útil para colocar pequenas quantidades de informações sobre o usuário que normalmente é necessário. O valor especificado em UserData propriedade é incluído como parte do cookie de tíquete de autenticação e, como os outros campos de tíquete, é criptografada e validada com base na configuração do sistema de autenticação de formulários. Por padrão, os dados do usuário é uma cadeia de caracteres vazia.

Para armazenar dados de usuário no tíquete de autenticação, é preciso escrever um pouco de código na página de logon que captura as informações específicas do usuário e a armazena na permissão. Como dados do usuário é uma propriedade do tipo cadeia de caracteres, os dados armazenados nela devem ser serializados corretamente como uma cadeia de caracteres. Por exemplo, imagine que nosso repositório do usuário incluído data de cada usuário de nascimento e o nome do seu empregador, e queríamos armazenar esses dois valores no tíquete de autenticação. Podemos pode serializar esses valores em uma cadeia de caracteres concatenando data do usuário de cadeia de caracteres do aniversário com uma barra vertical (|), seguido do nome do empregador. Para um usuário nascido em 15 de agosto de 1974 que trabalha para a Northwind Traders, atribuiremos a propriedade de dados do usuário a cadeia de caracteres: 1974-08-15 | A Northwind Traders.

Sempre que é necessário acessar os dados armazenados na permissão, podemos fazer isso captando FormsAuthenticationTicket da solicitação atual e desserializar a propriedade de dados do usuário. No caso da data de nascimento e empregador exemplo de nome, podemos seria dividir a cadeia de caracteres de dados do usuário em dois subcadeias de caracteres com base no delimitador (|).


[![Informações adicionais do usuário podem ser armazenadas no tíquete de autenticação](forms-authentication-configuration-and-advanced-topics-cs/_static/image11.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image10.png)

**Figura 04**: adicionais usuário podem ser armazenadas no tíquete de autenticação ([clique para exibir a imagem em tamanho normal](forms-authentication-configuration-and-advanced-topics-cs/_static/image12.png))


### <a name="writing-information-to-userdata"></a>Gravando informações para dados do usuário

Infelizmente, não é tão simples, como se pode esperar adicionar informações específicas do usuário para um tíquete de autenticação de formulários. A propriedade de dados do usuário da classe FormsAuthenticationTicket é somente leitura e só pode ser especificada com o construtor da classe FormsAuthenticationTicket. Ao especificar a propriedade de dados do usuário no construtor, podemos também precisará fornecer o tíquete de outros valores: o nome de usuário, a data de emissão, expiração e assim por diante. Quando criamos a página de logon no tutorial anterior, isso foi manipulado para que possamos pela classe FormsAuthentication. Ao adicionar dados do usuário para o FormsAuthenticationTicket, precisamos escrever código para replicar muito da funcionalidade já fornecida pela classe FormsAuthentication.

Vamos explorar o código necessário para trabalhar com dados do usuário atualizando a página aspx para registrar informações adicionais sobre o usuário para o tíquete de autenticação. Suponhamos que nosso repositório do usuário contém informações sobre o usuário trabalha para a empresa e seu título e que queremos capturar essas informações no tíquete de autenticação. Atualize o manipulador de eventos da página aspx LoginButton para que o código é semelhante ao seguinte:

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample6.cs)]

Vamos percorrer esse código uma linha por vez. O método começa com a definição de quatro matrizes de cadeia de caracteres: os usuários, senhas, companyName e titleAtCompany. Essas matrizes contêm os nomes de usuário, senhas, nomes de empresa e títulos para as contas de usuário no sistema, de que há três: Scott Jisun e Sam. Em um aplicativo real, esses valores deve ser consultados de repositório do usuário, não embutido em código-fonte da página.

No tutorial anterior, se as credenciais fornecidas eram válidas, simplesmente chamamos RedirectFromLoginPage (UserName.Text, RememberMe.Checked), que executar as seguintes etapas:

1. Criado o tíquete de autenticação de formulários
2. Gravou o tíquete de armazenamento apropriado. Para tíquetes de autenticação baseada em cookies, coleção de cookies do navegador é usada; para tíquetes de autenticação sem cookies, os dados do tíquete são serializados em URL
3. Redirecionado ao usuário para a página apropriada

Essas etapas são replicadas no código acima. Primeiro, a cadeia de caracteres que será armazenado na propriedade UserData é formada pela combinação do nome da empresa e o título, delimitando os dois valores com um caractere de pipe (|).

string userDataString = string.Concat(companyName[i], "|", titleAtCompany[i]);

Em seguida, FormsAuthentication.GetAuthCookie método é chamado, o que cria o tíquete de autenticação, criptografa e valida-lo de acordo com as definições de configuração e coloca-o em um objeto HttpCookie.

HttpCookie authCookie = FormsAuthentication.GetAuthCookie (UserName.Text, RememberMe.Checked);

Para trabalhar com o FormAuthenticationTicket inserido no cookie, é preciso chamar a classe de FormAuthentication [descriptografar método](https://msdn.microsoft.com/library/system.web.security.formsauthentication.decrypt.aspx), passando o valor do cookie.

FormsAuthenticationTicket ticket = FormsAuthentication.Decrypt(authCookie.Value);

Em seguida, criamos um *novo* FormsAuthenticationTicket instância com base em valores do FormsAuthenticationTicket existente. No entanto, esse novo tíquete inclui as informações específicas do usuário (userDataString).

FormsAuthenticationTicket newTicket = novo FormsAuthenticationTicket (permissão. Versão da permissão. Nome da permissão. IssueDate, tíquete. Expiração da permissão. IsPersistent, userDataString);

Nós, em seguida, criptografar (e validar) a nova instância FormsAuthenticationTicket chamando o [criptografar método](https://msdn.microsoft.com/library/system.web.security.formsauthentication.encrypt.aspx)e coloque esses dados criptografados (e validados) de volta em authCookie.

authCookie.Value = FormsAuthentication.Encrypt(newTicket);

Finalmente, authCookie é adicionado à coleção Response.Cookies e o método GetRedirectUrl é chamado para determinar a página apropriada para enviar ao usuário.

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample7.cs)]

Todo esse código é necessária porque a propriedade de dados do usuário é somente leitura e a classe FormsAuthentication fornece os métodos para especificar informações de dados do usuário em seus métodos GetAuthCookie, SetAuthCookie ou RedirectFromLoginPage.

> [!NOTE]
> O código que acabamos de examinar armazena informações específicas do usuário em um tíquete de autenticação baseada em cookies. As classes responsáveis para serializar o tíquete de autenticação de formulários para a URL são internas para o .NET Framework. História, você não pode armazenar dados de usuário em um tíquete de autenticação de formulários cookieless.


### <a name="accessing-the-userdata-information"></a>Acessando as informações de dados do usuário

Neste ponto nome da empresa e o título de cada usuário é armazenada na propriedade de dados do usuário do tíquete de autenticação de formulários ao fazer logon. Essa informação pode ser acessada no tíquete de autenticação em qualquer página sem exigir uma viagem para o repositório do usuário. Para ilustrar como essas informações podem ser recuperadas da propriedade UserData, vamos atualizar Default.aspx para que sua mensagem de boas-vinda inclui não apenas o nome do usuário, mas também a empresa funcionam para e seu título.

Atualmente, Default.aspx contém um painel AuthenticatedMessagePanel com um controle de rótulo denominado WelcomeBackMessage. Este painel é exibido apenas aos usuários autenticados. Atualize o código em página de Default.aspx\_carregar o manipulador de eventos para que ele se parece com o seguinte:

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample8.cs)]

Se Request.IsAuthenticated for true, a propriedade de texto do WelcomeBackMessage primeiro é definida como bem-vindo novamente, *nome de usuário*. Em seguida, a propriedade Identity é convertida em um objeto de FormsIdentity para que podemos acessar FormsAuthenticationTicket subjacente. Assim que tivermos FormsAuthenticationTicket, podemos desserializar a propriedade de dados do usuário para o nome da empresa e o título. Isso é feito ao dividir a cadeia de caracteres no caractere de pipe. O nome da empresa e o título são exibidos no rótulo WelcomeBackMessage.

A Figura 5 mostra uma captura de tela dessa exibição em ação. Registrar-se como Scott exibe uma mensagem de backup bem-vinda que inclui a empresa e o título de Scott.


[![Título e empresa atualmente registrados do usuário são exibidos](forms-authentication-configuration-and-advanced-topics-cs/_static/image14.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image13.png)

**Figura 05**: O atualmente registrados do usuário da empresa e título são exibidas ([clique para exibir a imagem em tamanho normal](forms-authentication-configuration-and-advanced-topics-cs/_static/image15.png))


> [!NOTE]
> Propriedade de dados do usuário do tíquete de autenticação serve como um cache para o repositório do usuário. Como qualquer cache, ele precisa ser atualizado quando os dados subjacentes são modificados. Por exemplo, se houver uma página da web do qual os usuários podem atualizar seu perfil, os campos em cache na propriedade UserData devem atualizados para refletir as alterações feitas pelo usuário.


## <a name="step-5-using-a-custom-principal"></a>Etapa 5: Usando uma entidade de segurança personalizada

Em cada solicitação de entrada FormsAuthenticationModule tenta autenticar o usuário. Se houver um tíquete de autenticação não expiradas, FormsAuthenticationModule atribui a propriedade HttpContext para um novo objeto GenericPrincipal. Esse objeto GenericPrincipal tem uma identidade de tipo FormsIdentity, que inclui uma referência para o tíquete de autenticação de formulários. A classe GenericPrincipal contém bare funcionalidade mínima necessária para uma classe que implementa IPrincipal - tem apenas uma propriedade de identidade e um método IsInRole.

O objeto principal tem duas responsabilidades: para indicar as funções que o usuário pertence e fornecer informações de identidade. Isso é feito por meio IsInRole da interface IPrincipal (*roleName*) método e propriedade de identidade, respectivamente. A classe GenericPrincipal permite uma matriz de cadeia de caracteres de nomes de função para ser especificadas por meio de seu construtor; seu IsInRole (*roleName*) método simplesmente verifica para ver se transmitido em *roleName* existe dentro da matriz de cadeia de caracteres. Quando o FormsAuthenticationModule cria o GenericPrincipal, ele passa em uma matriz de cadeia de caracteres vazia para o construtor do GenericPrincipal. Consequentemente, todas as chamadas de IsInRole sempre retornará false.

A classe GenericPrincipal atende às necessidades para a maioria dos cenários de autenticação baseada em formulários em que as funções não são usadas. Nas situações em que a manipulação de função padrão é insuficiente ou quando você precisa associar a um objeto de IIdentity personalizado com o usuário, você pode criar um objeto IPrincipal personalizado durante o fluxo de trabalho de autenticação e atribuí-la à propriedade HttpContext.

> [!NOTE]
> Como veremos no futuro tutoriais, quando ASP. Estrutura de funções do NET é habilitada cria um objeto principal personalizado do tipo [RolePrincipal](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx) e substitui o objeto de GenericPrincipal de criado para autenticação de formulários. Ele faz isso para personalizar o IsInRole método principal para interagir com a API da estrutura de funções.


Como podemos ter não preocupada nós com funções ainda, o único motivo que temos para criar uma entidade de segurança personalizada neste momento seria associar um objeto de IIdentity personalizado para a entidade de segurança. Na etapa 4, analisamos armazenar informações adicionais do usuário na propriedade de dados do usuário do tíquete de autenticação, em particular, o nome do usuário da empresa e seu título. No entanto, as informações de dados do usuário só são acessível por meio do tíquete de autenticação e, em seguida, somente como uma cadeia de caracteres serializada, que significa que sempre que você deseja exibir as informações de usuário armazenadas na permissão é necessário analisar a propriedade de dados do usuário.

Podemos melhorar a experiência do desenvolvedor, criando uma classe que implementa IIdentity e inclui propriedades CompanyName e título. Dessa forma, um desenvolvedor pode acessar o nome da empresa do usuário conectado no momento e título diretamente por meio das propriedades de título e CompanyName sem necessário saber como analisar a propriedade de dados do usuário.

### <a name="creating-the-custom-identity-and-principal-classes"></a>Criando a identidade personalizada e Classes de entidade

Para este tutorial, vamos criar os objetos principal e identidade personalizados no aplicativo\_pasta de código. Comece adicionando um aplicativo\_pasta ao seu projeto de código - com o botão direito no nome do projeto no Gerenciador de soluções, selecione a opção de adicionar pasta ASP.NET e escolha o aplicativo\_código. O aplicativo\_pasta de código é uma pasta ASP.NET especial que contém a classe arquivos específica para o site.

> [!NOTE]
> O aplicativo\_pasta de código só deve ser usada ao gerenciar seu projeto por meio do modelo de projeto de site. Se você estiver usando o [modelo de projeto de aplicativo Web](https://msdn.microsoft.com/asp.net/Aa336618.aspx), crie uma pasta padrão e adicionar as classes para que. Por exemplo, você pode adicionar uma nova pasta chamada Classes e coloque seu código existe.


Em seguida, adicione dois novos arquivos de classe para o aplicativo\_pasta de código, um CustomIdentity.cs nomeado e outro chamado CustomPrincipal.cs.


[![Adicione as Classes de CustomPrincipal e CustomIdentity ao seu projeto](forms-authentication-configuration-and-advanced-topics-cs/_static/image17.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image16.png)

**Figura 06**: adicionar as Classes de CustomPrincipal e CustomIdentity ao seu projeto ([clique para exibir a imagem em tamanho normal](forms-authentication-configuration-and-advanced-topics-cs/_static/image18.png))


A classe CustomIdentity é responsável por implementar a interface de IIdentity, que define as propriedades AuthenticationType IsAuthenticated e nome. Além dessas propriedades necessárias, estamos interessados na exposição a subjacente tíquete de autenticação de formulários, bem como propriedades de nome da empresa e o título do usuário. Digite o seguinte código à classe CustomIdentity.

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample9.cs)]

Observe que a classe inclui uma variável de membro FormsAuthenticationTicket (\_tíquete) e que essas informações de permissão devem ser fornecidas por meio do construtor. Esses dados de tíquete são usados para retornar o nome da identidade; a propriedade de dados do usuário é analisada para retornar os valores para as propriedades do título e CompanyName.

Em seguida, crie a classe CustomPrincipal. Como não estamos preocupados com funções neste momento, o construtor da classe CustomPrincipal aceita apenas um objeto de CustomIdentity; o método IsInRole sempre retorna false.

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample10.cs)]

### <a name="assigning-a-customprincipal-object-to-the-incoming-requests-security-context"></a>A atribuição de um objeto CustomPrincipal ao contexto de segurança de solicitação de entrada

Agora temos uma classe que estende a especificação de IIdentity padrão para incluir propriedades CompanyName e título, bem como uma classe de entidade de segurança personalizada que usa a identidade personalizada. Estamos prontos para a etapa no pipeline do ASP.NET e atribuir nosso objeto principal personalizado para o contexto de segurança de solicitação de entrada.

O pipeline do ASP.NET leva uma solicitação de entrada e a processa através de várias etapas. Em cada etapa, é gerado um evento específico, tornando possível para os desenvolvedores aproveitar o pipeline do ASP.NET e modificar a solicitação em determinados pontos do ciclo de vida. FormsAuthenticationModule, por exemplo, aguarda até que o ASP.NET gerar o [evento AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx), no ponto em que ele inspeciona a solicitação de entrada para um tíquete de autenticação. Se for encontrado um tíquete de autenticação, um objeto GenericPrincipal é criado e atribuído à propriedade HttpContext.

Após o evento AuthenticateRequest, o pipeline do ASP.NET gera o [PostAuthenticateRequest evento](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx), que é onde podemos pode substituir o objeto GenericPrincipal criado pelo FormsAuthenticationModule com uma instância do nosso Objeto CustomPrincipal. Figura 7 mostra o fluxo de trabalho.


[![O GenericPrincipal é substituído por um CustomPrincipal no evento PostAuthenticationRequest](forms-authentication-configuration-and-advanced-topics-cs/_static/image20.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image19.png)

**Figura 07**: O GenericPrincipal é substituído por um CustomPrincipal no evento PostAuthenticationRequest ([clique para exibir a imagem em tamanho normal](forms-authentication-configuration-and-advanced-topics-cs/_static/image21.png))


Para executar código em resposta a um evento de pipeline do ASP.NET, podemos pode criar o manipulador de eventos apropriado em global. asax ou criar nosso próprio módulo de HTTP. Para este tutorial vamos criar o manipulador de eventos em global. asax. Comece adicionando global. asax para seu site. Clique com botão direito no nome do projeto no Gerenciador de soluções e adicionar um item de tipo de classe de aplicativo Global chamado global. asax.


[![Adicionar um arquivo global. asax ao seu site](forms-authentication-configuration-and-advanced-topics-cs/_static/image23.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image22.png)

**Figura 08**: adicionar um arquivo global. asax ao seu site ([clique para exibir a imagem em tamanho normal](forms-authentication-configuration-and-advanced-topics-cs/_static/image24.png))


O modelo de global. asax padrão inclui manipuladores de eventos para um número de eventos de pipeline do ASP.NET, incluindo o início, fim e [evento de erro](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx), entre outros. Fique à vontade para remover esses manipuladores de eventos, como não precisamos delas para este aplicativo. O evento que estamos interessados em é PostAuthenticateRequest. Atualize o arquivo global. asax para que sua marcação é semelhante ao seguinte:

[!code-aspx[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample11.aspx)]

O aplicativo\_OnPostAuthenticateRequest método executa cada vez que o tempo de execução do ASP.NET gera o evento de PostAuthenticateRequest, o que acontece uma vez em cada solicitação de página de entrada. Inicia o manipulador de eventos para verificar se o usuário é autenticado e foi autenticado através da autenticação de formulários. Nesse caso, um novo objeto CustomIdentity é criado e transmitido tíquete de autenticação da solicitação atual em seu construtor. Um objeto CustomPrincipal é criado e o objeto de CustomIdentity recém-criada passado no construtor. Por fim, o contexto de segurança da solicitação atual é atribuído ao objeto CustomPrincipal recém-criado.

Observe que a última etapa - associando o objeto CustomPrincipal o contexto de segurança da solicitação - atribui a entidade de segurança para duas propriedades: HttpContext e thread. CurrentPrincipal. Essas duas atribuições são necessárias devido ao modo como os contextos de segurança são tratados no ASP.NET. O .NET Framework associa um contexto de segurança a cada thread em execução; Essa informação está disponível como um objeto IPrincipal por meio de [objeto Thread](https://msdn.microsoft.com/library/system.threading.thread.aspx)do [propriedade CurrentPrincipal](https://msdn.microsoft.com/library/system.threading.thread.currentcontext.aspx). O que é um confuso é que o ASP.NET tem suas próprias informações de contexto de segurança (HttpContext).

Em determinados cenários, a propriedade de thread. CurrentPrincipal é examinada para determinar o contexto de segurança; em outros cenários, HttpContext é usado. Por exemplo, não há recursos de segurança no .NET que permitem aos desenvolvedores declarativamente estado que os usuários ou funções podem instanciar uma classe ou chamar métodos específicos (consulte [adicionando regras de autorização para os negócios e camadas de dados usando PrincipalPermissionAttributes](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)). Nos bastidores, essas técnicas declarativas determinam o contexto de segurança por meio da propriedade de thread. CurrentPrincipal.

Em outros cenários, a propriedade HttpContext é usada. Por exemplo, no tutorial anterior, nós usamos essa propriedade para exibir o nome do usuário conectado no momento. Claramente, em seguida, é essencial que as informações de contexto de segurança nas propriedades do thread. CurrentPrincipal e HttpContext correspondem a.

O tempo de execução do ASP.NET é sincronizado automaticamente esses valores de propriedade para nós. No entanto, a sincronização ocorre após o evento AuthenticateRequest, mas *antes de* o evento PostAuthenticateRequest. Consequentemente, ao adicionar uma entidade de segurança personalizada no evento PostAuthenticateRequest, precisamos ter certeza de atribuir manualmente o thread. CurrentPrincipal senão CurrentPrincipal e HttpContext será fora de sincronia. Consulte [Context.User vs. Thread. CurrentPrincipal](http://leastprivilege.com/2005/11/23/context-user-vs-thread-currentprincipal/) para uma discussão mais detalhada sobre esse problema.

### <a name="accessing-the-companyname-and-title-properties"></a>Acessando o CompanyName e propriedades do título

Sempre que uma solicitação chega e é enviada para o mecanismo do ASP.NET, o aplicativo\_OnPostAuthenticateRequest o manipulador de eventos em global. asax será acionado. Se a solicitação foi autenticada com êxito pelo FormsAuthenticationModule, o manipulador de eventos criará um novo objeto CustomPrincipal com um objeto CustomIdentity com base no tíquete de autenticação de formulários. Com essa lógica em vigor, é extremamente simples acessar informações sobre o nome da empresa e o título do usuário conectado no momento.

Retornar à página\_manipulador de eventos de carga em Default.aspx, onde na etapa 4, escrevemos código para recuperar o tíquete de autenticação de formulário e analisar a propriedade de dados do usuário para exibir o nome da empresa e o título do usuário. Com os objetos CustomPrincipal e CustomIdentity em uso agora, não é necessário para analisar os valores de propriedade de dados do usuário do tíquete. Em vez disso, simplesmente obter uma referência ao objeto CustomIdentity e usar suas propriedades CompanyName e título:

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample12.cs)]

## <a name="summary"></a>Resumo

Neste tutorial, examinamos como personalizar as configurações do sistema de autenticação de formulários por meio do Web. config. Examinamos como a expiração do tíquete de autenticação é tratada e como as garantias de criptografia e validação são usadas para proteger o tíquete de inspeção e a modificação. Por fim, discutimos usando a propriedade de dados do usuário do tíquete de autenticação para armazenar informações adicionais do usuário na permissão em si e como usar objetos de entidade de segurança e identidade personalizados para expor essas informações de maneira mais amigável para desenvolvedores.

Este tutorial conclui nossa análise de autenticação de formulários do ASP.NET. O tutorial próximo inicia nossa jornada na estrutura de associação.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Autenticação de formulários dissecando](http://aspnet.4guysfromrolla.com/articles/072005-1.aspx)
- [Explicado: Autenticação de formulários do ASP.NET 2.0](https://msdn.microsoft.com/library/aa480476.aspx)
- [Como Proteger a autenticação de formulários do ASP.NET 2.0](https://msdn.microsoft.com/library/ms998310.aspx)
- [Professional ASP.NET 2.0 segurança, associação e gerenciamento de função](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [Protegendo controles de logon](https://msdn.microsoft.com/library/ms178346.aspx)
- [O &lt;autenticação&gt; elemento](https://msdn.microsoft.com/library/532aee0e.aspx)
- [O &lt;formulários&gt; elemento para &lt;autenticação&gt;](https://msdn.microsoft.com/library/1d3t3c61.aspx)
- [O &lt;machineKey&gt; elemento](https://msdn.microsoft.com/library/w8h3skw9.aspx)
- [Noções básicas sobre o tíquete de autenticação de formulários e o Cookie](https://support.microsoft.com/kb/910443)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Treinamento em vídeo sobre tópicos contidos neste tutorial

- [Como alterar as propriedades de autenticação de formulários](../../../videos/authentication/how-to-change-the-forms-authentication-properties.md)
- [A autenticação de Cookie sem configuração e uso em um aplicativo ASP.NET](../../../videos/authentication/how-to-setup-and-use-cookie-less-authentication-in-an-aspnet-application.md)
- [Realocação de logon de formulários do ASP](../../../videos/authentication/asp-forms-login-relocation.md)
- [Configuração de chave personalizada de logon de formulários](../../../videos/authentication/forms-login-custom-key-configuration.md)
- [Adicionar dados personalizados ao método de autenticação](../../../videos/authentication/add-custom-data-to-the-authentication-method.md)
- [Usar objetos de entidade de segurança personalizada](../../../videos/authentication/use-custom-principal-objects.md)

### <a name="about-the-author"></a>Sobre o autor

Scott Mitchell, autor de vários livros sobre ASP/ASP.NET e fundador da 4GuysFromRolla. com, trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é  *[Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott pode ser contatado pelo [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) ou em seu blog [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisor levar para este tutorial foi Alicja Maziarz. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com).

>[!div class="step-by-step"]
[Anterior](an-overview-of-forms-authentication-cs.md)
[Próximo](security-basics-and-asp-net-support-vb.md)
