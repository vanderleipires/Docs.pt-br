---
uid: web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-vb
title: Configuração de autenticação formulários e tópicos avançados (VB) | Microsoft Docs
author: rick-anderson
description: Neste tutorial examinaremos as várias configurações de autenticação de formulários e veja como modificá-los por meio do elemento de formulários. Isso envolvem uma detalhada...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/14/2008
ms.topic: article
ms.assetid: 829d2f56-5c48-445b-b826-3418a450c788
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-vb
msc.type: authoredcontent
ms.openlocfilehash: 493cb81271ea1c0439f7b499c5b48e659d3589b5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390855"
---
<a name="forms-authentication-configuration-and-advanced-topics-vb"></a>Configuração de autenticação de formulários e tópicos avançados (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_03_VB.zip) ou [baixar PDF](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial03_AuthAdvanced_vb.pdf)

> Neste tutorial examinaremos as várias configurações de autenticação de formulários e veja como modificá-los por meio do elemento de formulários. Isso envolvem uma visão detalhada de como personalizar o valor de tempo limite do tíquete de autenticação de formulários, usando uma página de logon com uma URL personalizada (como SignIn.aspx em vez de login. aspx) e tíquetes de autenticação de formulários sem cookies.


## <a name="introduction"></a>Introdução

No [tutorial anterior](an-overview-of-forms-authentication-vb.md) analisamos as etapas necessárias para implementar a autenticação de formulários em um aplicativo ASP.NET, da especificação de definições de configuração no Web. config para a criação de um log na página para exibir diferentes conteúdo para usuários anônimos e autenticados. Lembre-se de que podemos configurar o site para usar autenticação de formulários, definindo o atributo do modo dos &lt;autenticação&gt; elemento aos formulários. O &lt;autenticação&gt; elemento pode incluir opcionalmente um &lt;formulários&gt; elemento filho, por meio do qual uma variedade de configurações de autenticação de formulários pode ser especificada.

Neste tutorial examinaremos as várias configurações de autenticação de formulários e veja como modificá-los por meio de &lt;formulários&gt; elemento. Isso envolvem uma visão detalhada de como personalizar o valor de tempo limite do tíquete de autenticação de formulários, usando uma página de logon com uma URL personalizada (como SignIn.aspx em vez de login. aspx) e tíquetes de autenticação de formulários sem cookies. Também examinaremos a composição de tíquete de autenticação de formulários mais de perto e ver as precauções que ASP.NET usa para garantir que os dados de tíquete sejam protegidos contra inspeção e violação. Por fim, vamos examinar como modelar esses dados por meio de um objeto de entidade personalizado e de como armazenar dados de usuário adicionais no tíquete de autenticação de formulários.

## <a name="step-1-examining-the-ltformsgt-configuration-settings"></a>Etapa 1: Examinando os &lt;formulários&gt; definições de configuração

O sistema de autenticação de formulários no ASP.NET oferece uma série de definições de configuração que podem ser personalizados em uma base por aplicativo. Isso inclui configurações como: o tempo de vida de autenticação de formulários do tíquete; que tipo de proteção é aplicado ao tíquete; em qual autenticação sem cookies de condições, as permissões são usadas; o caminho para a página de logon. e outras informações. Para modificar os valores padrão, adicione uma [ &lt;formulários&gt; elemento](https://msdn.microsoft.com/library/1d3t3c61.aspx) como um filho a [ &lt;autenticação&gt; elemento](https://msdn.microsoft.com/library/532aee0e.aspx), especificando essas propriedade valores que você deseja personalizar como atributos XML desta forma:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample1.xml)]

A tabela 1 resume as propriedades que podem ser personalizadas por meio de &lt;formulários&gt; elemento. Como o Web. config é um arquivo XML, os nomes de atributo na coluna esquerda diferenciam maiusculas de minúsculas.


| <strong>Atributo</strong> |                                                                                                                                                                                                                                     <strong>Descrição</strong>                                                                                                                                                                                                                                      |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         sem cookies         |                                                                                                                Esse atributo especifica em quais condições o tíquete de autenticação é armazenado em um cookie versus sendo inserida na URL. Valores permitidos são: UseCookies; UseUri; Detecção automática; e UseDeviceProfile (o padrão). Etapa 2 examina essa configuração em mais detalhes.                                                                                                                |
|         defaultUrl         |                                                                                                                                                         Indica a URL que os usuários são redirecionados para depois de entrar na página de logon, se não houver nenhum valor de URL de redirecionamento especificado na cadeia de consulta. O valor padrão é default. aspx.                                                                                                                                                         |
|           domínio           | Ao usar os tíquetes de autenticação baseada em cookies, essa configuração especifica o valor de domínio do cookie s. O valor padrão é uma cadeia de caracteres vazia, o que faz com que o navegador para usar o domínio do qual ele foi emitido (como www.yourdomain.com). Nesse caso, o cookie será <strong>não</strong> ser enviada quando a tomada de solicitações para subdomínios, como admin.yourdomain.com. Se você quiser que o cookie a ser passado para todos os subdomínios, você precisará personalizar o atributo de domínio, configurando-a como seudomínio.com. |
|  enableCrossAppRedirects   |                                                                                                                                                                   Um valor booliano que indica se os usuários autenticados devem ser lembrados quando redirecionados para URLs em outros aplicativos da web no mesmo servidor. O padrão é falso.                                                                                                                                                                   |
|          loginUrl          |                                                                                                                                                                                                                      A URL da página de logon. O valor padrão é login.aspx.                                                                                                                                                                                                                      |
|            name            |                                                                                                                                                                                                   Ao usar os tíquetes de autenticação baseada em cookies, o nome do cookie. O padrão é. ASPXAUTH.                                                                                                                                                                                                   |
|            path            |                                                                             Ao usar os tíquetes de autenticação baseada em cookies, essa configuração especifica o atributo de caminho do cookie s. O atributo de caminho permite que um desenvolvedor limitar o escopo de um cookie para uma hierarquia de diretório específico. O valor padrão é /, que informa ao navegador para enviar o cookie do tíquete de autenticação para qualquer solicitação feita ao domínio.                                                                              |
|         proteção         |                                                                                                                                            Indica quais técnicas são usadas para proteger o tíquete de autenticação de formulários. Os valores permitidos são: todos (padrão); Criptografia; Nenhum; e a validação. Essas configurações são discutidas em detalhes na etapa 3.                                                                                                                                            |
|         requireSSL         |                                                                                                                                                                                Um valor booliano que indica se uma conexão SSL é necessária para transmitir o cookie de autenticação. O valor padrão é false.                                                                                                                                                                                |
|     slidingExpiration      |                                                                                                 Um valor booliano que indica que se o tempo limite s do cookie de autenticação é redefinido cada vez que o usuário visita o site durante uma única sessão. O valor padrão é true. A política de tempo limite do tíquete de autenticação é discutida mais detalhadamente o especificando o tíquete de seção do valor de tempo limite de s.                                                                                                 |
|          tempo limite           |                                                                                                                               Especifica o tempo, em minutos, após o qual o cookie do tíquete de autenticação expira. O valor padrão é 30. A política de tempo limite do tíquete de autenticação é discutida mais detalhadamente o especificando o tíquete de seção do valor de tempo limite de s.                                                                                                                               |

**Tabela 1**: um resumo do &lt;formulários&gt; atributos do elemento

No ASP.NET 2.0 e posteriores, o padrão a valores de autenticação de formulários são embutidos em código na classe FormsAuthenticationConfiguration no .NET Framework. Devem ser aplicadas a todas as modificações em uma base por aplicativo no arquivo Web. config. Isso é diferente do ASP.NET 1. x, onde os valores de autenticação de formulários padrão foram armazenados no arquivo Machine. config (e, portanto, pode ser modificados por meio da edição Machine. config). Enquanto o tópico do ASP.NET 1.x, vale a pena mencionar que um número das configurações de sistema de autenticação de formulários têm valores padrão diferentes no ASP.NET 2.0 e além dele, que no ASP.NET 1. x. Se você estiver migrando seu aplicativo em um ambiente do ASP.NET 1. x, é importante estar ciente dessas diferenças. Consultar [as &lt;formulários&gt; documentação técnica do elemento](https://msdn.microsoft.com/library/1d3t3c61.aspx) para obter uma lista das diferenças.

> [!NOTE]
> Várias configurações de autenticação de formulários, como o tempo limite, o domínio e o caminho, especificam os detalhes para o cookie de tíquete de autenticação de formulários resultante. Para obter mais informações sobre cookies, como elas funcionam e suas várias propriedades, leia [este tutorial de Cookies](http://www.quirksmode.org/js/cookies.html).


### <a name="specifying-the-tickets-timeout-value"></a>Especificando o valor de tempo limite do tíquete

O tíquete de autenticação de formulários é um token que representa uma identidade. Com permissões de autenticação baseada em cookies, esse token é mantido na forma de um cookie e enviado para o servidor web em cada solicitação. A posse do token, em essência, declara, eu sou *nome de usuário*, eu já fez logon e é usado para que a identidade do usuário pode ser lembrada em visitas à página.

O tíquete de autenticação de formulários não apenas inclui a identidade do usuário, mas também contém informações para ajudar a garantir a integridade e a segurança do token. Afinal, não queremos que um usuário perigoso para poder criar um token falsificado ou modificar um token legítimas de alguma forma underhanded.

Um tal bit de informações incluídas no tíquete é um *expiração*, que é a data e hora que o tíquete não é mais válido. Cada vez que o FormsAuthenticationModule inspeciona um tíquete de autenticação, ele garante a expiração do tíquete ainda não tiver passado. Em caso positivo, ele desconsidera o tíquete e identifica o usuário como sendo anônimo. Essa proteção ajuda a proteger contra ataques de repetição. Sem uma expiração, se um hacker conseguiu obter suas mãos no tíquete de autenticação válido de um usuário - talvez obtendo acesso físico ao seu computador e torcendo por meio de seus cookies, eles poderiam enviar uma solicitação para o servidor com esse tíquete de autenticação roubados e obter a entrada. Embora a expiração não impede que esse cenário, isso limita a janela durante o qual um ataque desse tipo pode ser bem-sucedida.

> [!NOTE]
> Etapa 3 técnicas adicionais de detalhes usadas pelo sistema de autenticação de formulários para proteger o tíquete de autenticação.


Ao criar o tíquete de autenticação, o sistema de autenticação de formulários determina sua expiração consultando a configuração de tempo limite. Conforme observado na tabela 1, o tempo limite configurando padrões para 30 minutos, o que significa que, quando o tíquete de autenticação de formulários é criado sua expiração é definida como uma data e hora, 30 minutos no futuro.

A expiração define um tempo absoluto no futuro em que o tíquete de autenticação de formulários expira. Mas, normalmente, os desenvolvedores querem implementar uma expiração deslizante, que é redefinida toda vez que o usuário revisitar o site. Esse comportamento é determinado pelas configurações slidingExpiration. Se definido como true (padrão), cada vez que o FormsAuthenticationModule autentica um usuário, ele de atualizações de expiração do tíquete. Se definido como false, a expiração não é atualizado em cada solicitação, causando assim o tíquete expirar exatamente tempo limite número de minutos passados quando o tíquete foi inicialmente criado.

> [!NOTE]
> A expiração armazenada no tíquete de autenticação é uma data absoluta e o valor de tempo, como em 2 de agosto de 2008 11:34 AM. Além disso, a data e hora são em relação à hora do local do servidor web. Essa decisão de design pode ter alguns efeitos colaterais de interessantes em torno do horário de verão (DST), que é quando os relógios dos Estados Unidos são movidos com antecedência (supondo que o servidor web está hospedado em uma localidade em que o horário de verão é observada) de uma hora. Considere o que aconteceria para um site ASP.NET com uma expiração de 30 minutos próximos ao horário em que horário de verão começa (que é às 2 horas). Imagine que um visitante se inscreve para o site em 11 de março de 2008 à 1h: 55. Isso geraria um tíquete de autenticação de formulários expira no dia 11 de março de 2008 às: 25 2H (30 minutos no futuro). No entanto, depois de chegar às 2H, o relógio salta para 3:00 AM devido ao horário de verão. Quando o usuário carregar uma nova página de seis minutos depois de entrar (em 3:01 AM), o FormsAuthenticationModule observa que o tíquete expirou e redireciona o usuário para a página de logon. Para obter uma discussão mais completa sobre esse e outros Singularidades do tempo limite de tíquete de autenticação, bem como soluções alternativas, escolher um exemplar de Stefan Schackow *Professional ASP.NET 2.0 segurança, associação e gerenciamento de função* (ISBN: 978-0-7645-9698-8).


Figura 1 ilustra o fluxo de trabalho quando slidingExpiration é definido como false e o tempo limite é definido como 30. Observe que o tíquete de autenticação gerado no logon contém a data de validade, e esse valor não é atualizado em solicitações subsequentes. Se o FormsAuthenticationModule localiza o tíquete expirou, ele descarta e trata a solicitação como anônimo.


[![Uma representação gráfica dos slidingExpiration do tíquete de autenticação de formulários de expiração quando for false](forms-authentication-configuration-and-advanced-topics-vb/_static/image2.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image1.png)

**Figura 01**: representação gráfica de um dos slidingExpiration do tíquete de autenticação de formulários de expiração quando for false ([clique para exibir a imagem em tamanho normal](forms-authentication-configuration-and-advanced-topics-vb/_static/image3.png))


A Figura 2 mostra o fluxo de trabalho quando slidingExpiration é definido como true e o tempo limite é definido como 30. Quando uma solicitação autenticada é recebida (com um tíquete não expirados) sua expiração é atualizada para o tempo limite número de minutos no futuro.


[![Uma representação gráfica do tíquete de autenticação de formulários quando slidingExpiration é true](forms-authentication-configuration-and-advanced-topics-vb/_static/image5.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image4.png)

**Figura 02**: representação gráfica de um do tíquete de autenticação de formulários quando slidingExpiration é true ([clique para exibir a imagem em tamanho normal](forms-authentication-configuration-and-advanced-topics-vb/_static/image6.png))


Ao usar os tíquetes de autenticação baseada em cookie (o padrão), esta discussão se torna um pouco mais confusa porque cookies também podem ter seus próprios expirações especificadas. Expiração de um cookie (ou falta de preparação) instrui o navegador quando o cookie deve ser destruído. Se o cookie não tiver uma expiração, ele é destruído quando o navegador é fechado. Se houver uma expiração, no entanto, o cookie permanecerá armazenado no computador do usuário até a data e especificado na expiração do tempo. Quando um cookie for destruído pelo navegador, ele não é enviado ao servidor web. Portanto, a destruição de um cookie é análoga ao usuário fazer logoff do site.

> [!NOTE]
> É claro, é possível que um usuário proativamente remover quaisquer cookies armazenados em seu computador. No Internet Explorer 7, você teria vá para ferramentas, opções e clique no botão Excluir na seção de histórico de navegação. A partir daí, clique no botão Excluir cookies.


O sistema de autenticação de formulários cria cookies com base em sessão ou com base em expiração, dependendo do valor passado para o *persistCookie* parâmetro. Lembre-se que os métodos da classe FormsAuthentication de GetAuthCookie, SetAuthCookie e RedirectFromLoginPage levar dois parâmetros de entrada: *nome de usuário* e *persistCookie*. A página de logon que criamos no tutorial anterior incluído um CheckBox lembrar-me, que determinado se um cookie persistente foi criado. Cookies persistentes são baseados em expiração; cookies não persistentes são baseadas em sessão.

Os tempo limite e slidingExpiration conceitos já discutidos aplicam o mesmo para ambos os cookies com base em sessão e expiração. Há apenas uma diferença mínima em execução: ao usar cookies com base em expiração com slidingTimeout definido como true, expiração do cookie só é atualizada quando mais de metade do tempo especificado tiver decorrido.

Vamos atualizar as políticas de tempo limite de tíquete de autenticação do nosso site para que tíquetes do tempo limite após uma hora (60 minutos), usando uma expiração deslizante. Para efetuar essa alteração, atualize o arquivo Web. config, adicionando um &lt;formulários&gt; elemento para o &lt;autenticação&gt; elemento com a seguinte marcação:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample2.xml)]

### <a name="using-an-login-page-url-other-than-loginaspx"></a>Usando uma URL da página de logon diferente de login. aspx

Uma vez que o FormsAuthenticationModule redireciona automaticamente os usuários não autorizados para a página de logon, ele precisa saber a URL da página de logon. Essa URL é especificada pelo atributo loginUrl na &lt;formulários&gt; elemento e o padrão é login. aspx. Se você estiver portando ao longo de um site existente, você talvez já tenha uma página de logon com uma URL diferente, um que já foi marcada e indexada por mecanismos de pesquisa. Em vez de renomear sua página de logon existentes para login. aspx e quebrar links e marcadores de usuários, você pode modificar em vez disso, o atributo loginUrl para apontar para a página de logon.

Por exemplo, se sua página de logon foi nomeada SignIn.aspx e foi localizada no diretório de usuários, você pode apontar a definição de configuração loginUrl para ~/Users/SignIn.aspx da seguinte forma:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample3.xml)]

Como nosso aplicativo atual já tem uma página de logon chamada Login. aspx, não é necessário especificar um valor personalizado na &lt;formulários&gt; elemento.

## <a name="step-2-using-cookieless-forms-authentication-tickets"></a>Etapa 2: Usando os tíquetes de autenticação de formulários sem cookies

Por padrão, o sistema de autenticação de formulários determina se deseja armazenar seus tíquetes de autenticação na coleção de cookies ou inseri-las na URL com base no agente do usuário visitar o site. Todos os navegadores da área de trabalho como cookies de suporte do Internet Explorer, Firefox, Opera e Safari, mas não todos os dispositivos móveis.

A política de cookie usada pelo sistema de autenticação de formulários depende da configuração sem cookies na &lt;formulários&gt; elemento, que pode ser atribuído um dos quatro valores:

- UseCookies - Especifica que os tíquetes de autenticação baseada em cookie sempre serão usados.
- UseUri - indica que os tíquetes de autenticação baseada em cookie nunca serão usados.
- Detecção automática - se o perfil de dispositivo não dá suporte a cookies, tíquetes de autenticação baseada em cookies não são usados; Se o perfil de dispositivo dá suporte a cookies, um mecanismo de investigação é usado para determinar se os cookies estão habilitados.
- UseDeviceProfile - padrão. usa os tíquetes de autenticação baseada em cookie somente se o perfil de dispositivo dá suporte a cookies. Nenhum mecanismo de investigação é usado.

As configurações de detecção automática e UseDeviceProfile dependem de um *perfil de dispositivo* no atestar se é necessário usar tíquetes de autenticação sem cookies ou baseada em cookie. O ASP.NET mantém um banco de dados de vários dispositivos e seus recursos, como se eles dão suporte a cookies, qual versão do JavaScript dar suporte a eles e assim por diante. Cada vez que um dispositivo solicita uma página da web de um servidor web que ele envia ao longo de um *agente do usuário* cabeçalho HTTP que identifica o tipo de dispositivo. ASP.NET corresponde automaticamente a cadeia de caracteres de agente do usuário fornecido com o perfil correspondente especificado no seu banco de dados.

> [!NOTE]
> Este banco de dados de recursos do dispositivo é armazenado em um número de arquivos XML que seguem a [esquema de arquivo de definição do navegador](https://msdn.microsoft.com/library/ms228122.aspx). Os arquivos de perfil de dispositivo padrão estão localizados em % WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG\Browsers. Você também pode adicionar arquivos personalizados ao aplicativo do seu aplicativo\_pasta navegadores. Para obter mais informações, consulte [How To: detectar tipos de navegador em páginas da Web do ASP.NET](https://msdn.microsoft.com/library/3yekbd5b.aspx).


Como a configuração padrão é UseDeviceProfile, tíquetes de autenticação de formulários sem cookies serão usadas quando o site é visitado por um dispositivo cujo perfil informa que ele não dá suporte a cookies.

### <a name="encoding-the-authentication-ticket-in-the-url"></a>O tíquete de autenticação na URL de codificação

Os cookies são uma mídia natural para incluir informações do navegador em cada solicitação para um site específico, por isso, as configurações de autenticação de formulários padrão usam cookies se o dispositivo visitando ofereça suporte a eles. Se os cookies não são suportados, deve-se empregar um meio alternativo para transmitir o tíquete de autenticação do cliente ao servidor. É uma solução comum usada em ambientes sem cookies codificar os dados do cookie na URL.

A melhor maneira de ver como essas informações podem estar incorporadas a URL é forçar o site para usar os tíquetes de autenticação sem cookies. Isso pode ser feito definindo o parâmetro de configuração sem cookies para UseUri:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample4.xml)]

Depois de fazer essa alteração, visite o site por meio de um navegador. Ao visitar como um usuário anônimo, as URLs serão semelhantes, exatamente como antes. Por exemplo, ao visitar a página Default. aspx barra de endereços do meu navegador mostra a seguinte URL:

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/default.aspx`

No entanto, após fazer logon, o tíquete de autenticação de formulários é incorporado na URL. Por exemplo, depois de visitar a página de logon e registro em log como Sam, eu estou retornado para a página Default. aspx, mas a URL neste momento é:

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/default.aspx`

O tíquete de autenticação de formulários foi incorporado na URL. A cadeia de caracteres (F (jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv rfezq0gKadKX0YPZCkA2) representa as informações de tíquete de autenticação codificada em hexadecimal e são os mesmos dados que normalmente são armazenados em um cookie.

Em ordem para tíquetes de autenticação sem cookies funcione, o sistema deverá codificar todas as URLs na página para incluir os dados de tíquete de autenticação, caso contrário, o tíquete de autenticação serão perdido quando o usuário clica em um link. Felizmente, essa lógica de inserção é executada automaticamente. Para demonstrar essa funcionalidade, abra a página Default. aspx e adicione um controle de hiperlink, definindo suas propriedades de texto e NavigateUrl para testar Link e SomePage.aspx, respectivamente. Não importa que existe realmente não uma página em nosso projeto chamado SomePage.aspx.

Salvar as alterações em Default. aspx e, em seguida, visite-o por meio de um navegador. Faça logon no site para que o tíquete de autenticação de formulários é inserido na URL. Em seguida, de Default. aspx, clique no link de link "teste". O que aconteceu? Se nenhuma página denominada SomePage.aspx existir, em seguida, um erro 404 ocorreu, mas que não é o que é importante aqui. Em vez disso, concentre-se na barra de endereços do seu navegador. Observe que ele inclui o tíquete de autenticação de formulários na URL!

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/SomePage.aspx`

O SomePage.aspx de URL no link foi convertida automaticamente em uma URL que incluído o tíquete de autenticação - não temos de escrever uma linha de código! O tíquete de autenticação de formulário será inserido automaticamente na URL para todos os hiperlinks não começar com http:// ou /. Não importa se o hiperlink é exibido em uma chamada de Response. Redirect, em um controle de hiperlink ou em um elemento âncora HTML (ou seja, &lt;href = "..."&gt;... &lt;/a&gt;). Desde que a URL não é algo como http://www.someserver.com/SomePage.aspx ou /SomePage.aspx, formulários de tíquete de autenticação será inserido para nós.

> [!NOTE]
> Tíquetes de autenticação de formulários sem cookies seguem as mesmas políticas de tempo limite como tíquetes de autenticação baseada em cookie. No entanto, os tíquetes de autenticação sem cookies são mais propensos a ataques de repetição, pois o tíquete de autenticação é inserido diretamente na URL. Imagine que um usuário que visita um site, faz logon no e, em seguida, cola a URL em um email a um colega. Se o colega clicar nesse link antes que a expiração é atingida, eles serão estar conectados como o usuário que enviou o email!


## <a name="step-3-securing-the-authentication-ticket"></a>Etapa 3: Protegendo o tíquete de autenticação

O tíquete de autenticação de formulários é transmitido durante a transmissão em um cookie ou inserido diretamente na URL. Além das informações de identidade, o tíquete de autenticação também pode incluir dados de usuário (conforme veremos na etapa 4). Consequentemente, é importante que os dados do tíquete são criptografados das pessoas curiosas e (ainda mais importante) que o sistema de autenticação de formulários pode garantir que o tíquete não foi violado.

Para garantir a privacidade dos dados de tíquete, o sistema de autenticação de formulários pode criptografar os dados de tíquete. Falha ao criptografar os dados de tíquete envia informações potencialmente confidenciais durante a transmissão em texto sem formatação.

Para garantir a autenticidade de um tíquete, o sistema de autenticação de formulários deve *validar* o tíquete. A validação é o ato de garantir que um determinado tipo de dados não tiver sido modificado e é feito por meio de um  *[(MAC) de código de autenticação de mensagem](http://en.wikipedia.org/wiki/Message_authentication_code)*. Em resumo, o MAC é uma pequena parte de informações que identificam os dados que precisa ser validada (nesse caso, o tíquete). Se os dados representados pelo MAC for modificados, em seguida, o MAC e os dados não serão correspondentes para cima. Além disso, é computacionalmente difícil um hacker tanto modificar os dados e gerar seu próprio MAC para corresponder com os dados modificados.

Ao criar (ou modificação) um tíquete, o sistema de autenticação de formulários cria um MAC e o anexa aos dados do tíquete. Quando chega uma solicitação subsequente, o sistema de autenticação de formulários compara os dados de MAC e tíquete para validar a autenticidade dos dados de tíquete. Figura 3 ilustra esse fluxo de trabalho graficamente.


[![Autenticidade do tíquete é garantida por meio de um MAC](forms-authentication-configuration-and-advanced-topics-vb/_static/image8.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image7.png)

**Figura 03**: autenticidade do The Ticket é garantida por meio de um MAC ([clique para exibir a imagem em tamanho normal](forms-authentication-configuration-and-advanced-topics-vb/_static/image9.png))


Quais medidas de segurança são aplicadas para o tíquete de autenticação depende da configuração de proteção na &lt;formulários&gt; elemento. A configuração de proteção pode ser atribuída a um dos seguintes três valores:

- All - o tíquete é criptografado e assinada digitalmente (o padrão).
- Criptografia – criptografia só é aplicada – nenhum MAC é gerado.
- Nenhum - o tíquete não é criptografado, nem assinado digitalmente.
- Validação - um MAC é gerada, mas os dados de tíquete são enviados eletronicamente em texto sem formatação.

Microsoft recomenda o uso de todas as configurações.

### <a name="setting-the-validation-and-decryption-keys"></a>Definindo as chaves de descriptografia e validação

A criptografia e algoritmos usados pelo sistema de autenticação de formulários para criptografar e validar o tíquete de autenticação de hash são personalizáveis por meio de [ &lt;machineKey&gt; elemento](https://msdn.microsoft.com/library/w8h3skw9.aspx) no Web. config. A tabela 2 descreve o &lt;machineKey&gt; atributos do elemento e seus valores possíveis.

| **Atributo** | **Descrição** |
| --- | --- |
| descriptografia | Indica o algoritmo usado para criptografia. Esse atributo pode ter um dos quatro valores a seguir: - Auto - o padrão; Determina o algoritmo com base no comprimento do atributo decryptionKey. -AES - usa a [criptografia AES (padrão avançado)](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) algoritmo. -DES - usa a [padrão de criptografia de dados (DES)](http://en.wikipedia.org/wiki/Data_Encryption_Standard) esse algoritmo é considerado computacionalmente fraco e não deve ser usado. -3DES - usa a [Triple DES](http://en.wikipedia.org/wiki/Triple_DES) algoritmo, que funciona aplicando o algoritmo DES triplo. |
| decryptionKey | A chave secreta usada pelo algoritmo de criptografia. Esse valor deve ser uma cadeia de caracteres hexadecimal do tamanho apropriado (com base no valor na descriptografia), gerar automaticamente ou qualquer valor concatenado com, IsolateApps. Adicionar IsolateApps instrui o ASP.NET para usar um valor exclusivo para cada aplicativo. O padrão é AutoGenerate, IsolateApps. |
| validação | Indica o algoritmo usado para validação. Esse atributo pode ter um dos quatro valores a seguir: - AES - usa o algoritmo de criptografia AES (padrão avançado). -MD5 - usa a [Message Digest 5 (MD5)](http://en.wikipedia.org/wiki/MD5) algoritmo. -SHA1 - usa a [SHA1](http://en.wikipedia.org/wiki/Sha1) algoritmo (o padrão). -3DES - usa o algoritmo DES triplo. |
| validationKey | A chave secreta usada pelo algoritmo de validação. Esse valor deve ser uma cadeia de caracteres hexadecimal do tamanho apropriado (com base no valor de validação), gerar automaticamente ou qualquer valor concatenado com, IsolateApps. Adicionar IsolateApps instrui o ASP.NET para usar um valor exclusivo para cada aplicativo. O padrão é AutoGenerate, IsolateApps. |

**Tabela 2**: O &lt;machineKey&gt; atributos do elemento

Uma discussão completa sobre essas opções de criptografia e validação e os prós e contras de vários algoritmos, estão além do escopo deste tutorial. Para um profundo examinar esses problemas, incluindo diretrizes sobre quais algoritmos de criptografia e validação de usar, quais comprimentos de chave a ser usado e a melhor maneira de gerar essas chaves, consulte *Professional ASP.NET 2.0 segurança, associação e gerenciamento de função* .

Por padrão, as chaves usadas para criptografia e validação são geradas automaticamente para cada aplicativo, e essas chaves são armazenadas na autoridade de segurança Local (LSA). Em resumo, as configurações padrão garantem que as chaves exclusivas em um servidor da web pelo servidor web e aplicativo por aplicativo. Consequentemente, esse comportamento padrão não funcionará para os dois seguintes cenários:

- **Web Farms** - em uma [farm da web](http://en.wikipedia.org/wiki/Web_farm) cenário, um único aplicativo web está hospedado em vários servidores web para fins de redundância e escalabilidade. Cada solicitação de entrada é enviada a um servidor no farm, que significa que ao longo do tempo de vida de uma sessão do usuário, servidores diferentes podem ser usadas para lidar com suas várias solicitações. Consequentemente, cada servidor deve usar as mesmas chaves de criptografia e validação para que o tíquete de autenticação de formulários criados, criptografado e validado em um servidor pode ser descriptografado e validado em outro servidor no farm.
- **Entre o aplicativo de compartilhamento de tíquete** -um único servidor web pode hospedar vários aplicativos ASP.NET. Se você precisar para esses aplicativos diferentes compartilhar um tíquete de autenticação de formulários único, é imperativo que suas chaves de criptografia e validação de correspondam.

Ao trabalhar em um farm da web definindo ou compartilhamento de tíquetes de autenticação entre aplicativos no mesmo servidor, você precisará configurar o &lt;machineKey&gt; elemento em aplicativos afetados para que seus decryptionKey e valores de validationKey correspondem.

Embora nenhum dos cenários acima aplica-se ao nosso aplicativo de exemplo, podemos ainda especificar valores de validationKey e decryptionKey explícita e defina os algoritmos a ser usado. Adicionar um &lt;machineKey&gt; configuração ao arquivo Web. config:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample5.xml)]

Para obter mais informações, check-out [How To: Configure MachineKey no ASP.NET 2.0](https://msdn.microsoft.com/library/ms998288.aspx).

> [!NOTE]
> Os valores de validationKey decryptionKey foram tirados [Steve Gibson](http://www.grc.com/stevegibson.htm)do [página de web de senhas perfeito](https://www.grc.com/passwords.htm), que gera a 64 caracteres hexadecimais aleatórios em cada visita de página. Para reduzir a probabilidade de que essas chaves até seus aplicativos de produção, você é incentivado a substituir as chaves acima por aqueles gerados aleatoriamente da página de senhas perfeito.


## <a name="step-4-storing-additional-user-data-in-the-ticket"></a>Etapa 4: Armazenando dados de usuário adicionais no tíquete

Muitos aplicativos da web exibem informações sobre ou exibição da página de base no usuário conectado no momento. Por exemplo, uma página da web pode mostrar o nome do usuário e a data em que ela último logon na parte superior de cada página. O tíquete de autenticação de formulários armazena o nome de usuário do usuário conectado no momento, mas quando qualquer outra informação é necessária, a página deve ir para o repositório do usuário - normalmente um banco de dados - para pesquisar as informações não armazenadas em do tíquete de autenticação.

Com um pouco de código podemos pode armazenar informações adicionais do usuário em que o tíquete de autenticação de formulários. Esses dados podem ser expressos por meio de [classe FormsAuthenticationTicket](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx)do [UserData propriedade](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.userdata.aspx). Isso é um local útil para colocar pequenas quantidades de informações sobre o usuário que normalmente é necessária. O valor especificado em UserData propriedade é incluído como parte do cookie do tíquete de autenticação e, como os outros campos de tíquete é criptografada e validada com base na configuração do sistema de autenticação de formulários. Por padrão, os dados do usuário é uma cadeia de caracteres vazia.

Para armazenar dados de usuário em que o tíquete de autenticação, precisamos escrever um pouco de código na página de logon que captura as informações específicas do usuário e o armazena no tíquete. Como dados do usuário é uma propriedade do tipo cadeia de caracteres, os dados armazenados nele devem ser serializados corretamente como uma cadeia de caracteres. Por exemplo, imagine que nosso repositório do usuário incluído data de cada usuário de nascimento e o nome do seu empregador, e queremos armazenar esses valores de dois propriedade no tíquete de autenticação. Podemos pode serializar esses valores em uma cadeia de caracteres concatenando a data do usuário da cadeia de caracteres do nascimento com uma barra vertical (|), seguido pelo nome do empregador. Para um usuário nascido em 15 de agosto de 1974 que funciona para a Northwind Traders, atribuiríamos a propriedade UserData a cadeia de caracteres: 1974-08-15 | A Northwind Traders.

Sempre que é necessário acessar os dados armazenados no tíquete, podemos fazer isso captando FormsAuthenticationTicket da solicitação atual e desserializar a propriedade de dados do usuário. No caso a data de nascimento e empregador exemplo de nome, podemos seria dividir a cadeia de caracteres de dados do usuário em duas subcadeias de caracteres com base no delimitador (|).


[![Informações adicionais do usuário podem ser armazenadas no tíquete de autenticação](forms-authentication-configuration-and-advanced-topics-vb/_static/image11.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image10.png)

**Figura 04**: adicionais usuário informações podem ser armazenados no tíquete de autenticação ([clique para exibir a imagem em tamanho normal](forms-authentication-configuration-and-advanced-topics-vb/_static/image12.png))


### <a name="writing-information-to-userdata"></a>Gravando informações UserData

Infelizmente, não é tão simples como se pode esperar a adição de informações específicas do usuário para um tíquete de autenticação de formulários. A propriedade de dados do usuário da classe FormsAuthenticationTicket é somente leitura e só pode ser especificada por meio do construtor de classe FormsAuthenticationTicket. Ao especificar a propriedade de dados do usuário no construtor, também precisamos fornecer o tíquete de outros valores: o nome de usuário, a data de emissão, a expiração e assim por diante. Quando criamos a página de logon no tutorial anterior, isso foi tudo tratado para que possamos pela classe FormsAuthentication. Ao adicionar UserData a FormsAuthenticationTicket, precisamos escrever código para replicar grande parte da funcionalidade já fornecida pela classe FormsAuthentication.

Vamos explorar o código necessário para trabalhar com dados do usuário atualizando a página de login. aspx para registrar informações adicionais sobre o usuário para o tíquete de autenticação. Suponhamos que o nosso repositório do usuário contém informações sobre o usuário trabalha para a empresa e seu título e que queremos capturar essas informações no tíquete de autenticação. Atualize o manipulador de eventos de clique LoginButton da página de login. aspx para que o código é semelhante ao seguinte:

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample6.vb)]

Vamos percorrer esse código uma linha por vez. O método começa definindo quatro matrizes de cadeia de caracteres: os usuários, senhas, companyName e titleAtCompany. Essas matrizes contêm os nomes de usuário, senhas, nomes de empresa e títulos para as contas de usuário no sistema, de que há três: Scott, Jisun e Sam. Em um aplicativo real, esses valores seriam consultados de repositório do usuário, não embutido no código-fonte da página.

No tutorial anterior, se as credenciais fornecidas eram válidas, simplesmente chamamos FormsAuthentication. RedirectFromLoginPage (UserName.Text, RememberMe.Checked), que executou as seguintes etapas:

1. Criado o tíquete de autenticação de formulários
2. Gravou o tíquete de armazenamento apropriado. Para tíquetes de autenticação baseada em cookies, a coleção de cookies do navegador é usada; para tíquetes de autenticação sem cookies, os dados de tíquete são serializados em URL
3. Redirecionado ao usuário para a página apropriada

Essas etapas são replicadas no código acima. Em primeiro lugar, a cadeia de caracteres, eventualmente, armazenaremos na propriedade UserData é formada pela combinação do nome da empresa e o título, que delimita os dois valores com um caractere de pipe (|).

Dim userDataString As String = String.Concat(companyName(i), "|", titleAtCompany(i))

Em seguida, FormsAuthentication.GetAuthCookie o método é invocado, que cria o tíquete de autenticação, criptografa e valida-lo de acordo com as definições de configuração e coloca-o em um objeto HttpCookie.

Dim authCookie como HttpCookie = FormsAuthentication.GetAuthCookie (UserName.Text, RememberMe.Checked)

Para trabalhar com o FormAuthenticationTicket incorporada o cookie, precisamos chamar a classe de FormAuthentication [descriptografar método](https://msdn.microsoft.com/library/system.web.security.formsauthentication.decrypt.aspx), passando o valor do cookie.

Dim tíquete como FormsAuthenticationTicket = FormsAuthentication.Decrypt(authCookie.Value)

Em seguida, criamos uma *novo* FormsAuthenticationTicket instância com base nos valores do FormsAuthenticationTicket existente. No entanto, esse novo tíquete inclui informações específicas do usuário (userDataString).

Dim newTicket como FormsAuthenticationTicket = FormsAuthenticationTicket(ticket. novo Versão, o tíquete. Nome, o tíquete. IssueDate, de tíquete. Expiração, o tíquete. IsPersistent, userDataString)

Nós, em seguida, criptografar (e validar) a nova instância de FormsAuthenticationTicket chamando o [método de criptografia](https://msdn.microsoft.com/library/system.web.security.formsauthentication.encrypt.aspx)e colocar esses dados criptografados (e validados) de volta no authCookie.

authCookie.Value = FormsAuthentication.Encrypt(newTicket)

Por fim, authCookie é adicionado à coleção Response.Cookies e o método GetRedirectUrl é chamado para determinar a página apropriada para enviar ao usuário.

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample7.vb)]

Todo esse código é necessária porque a propriedade de dados do usuário é somente leitura e a classe FormsAuthentication não fornece nenhum método para especificar informações de dados do usuário em seus métodos GetAuthCookie, SetAuthCookie ou RedirectFromLoginPage.

> [!NOTE]
> O código que acabamos de examinar armazena informações específicas do usuário em um tíquete de autenticação baseada em cookies. As classes responsáveis pela serialização o tíquete de autenticação de formulários para a URL são internas ao .NET Framework. Encurtar a história, é possível armazenar dados de usuário em um tíquete de autenticação de formulários sem cookies.


### <a name="accessing-the-userdata-information"></a>Acessar as informações de dados do usuário

Neste ponto nome da empresa e o título de cada usuário é armazenada na propriedade de dados do usuário do tíquete de autenticação de formulários quando ele fizer logon. Essas informações podem ser acessadas no tíquete de autenticação em qualquer página sem a necessidade de uma viagem para o repositório do usuário. Para ilustrar como essas informações podem ser recuperadas da propriedade UserData, vamos atualizar default. aspx para que sua mensagem de boas-vinda inclui não apenas o nome do usuário, mas também a empresa trabalham e seu título.

Atualmente, default. aspx contém um painel AuthenticatedMessagePanel com um controle de rótulo denominado WelcomeBackMessage. Este painel é exibido somente para usuários autenticados. Atualizar o código em página da default. aspx\_carregar o manipulador de eventos para que ele é semelhante ao seguinte:

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample8.vb)]

Se Request.IsAuthenticated for True, então a propriedade de texto do WelcomeBackMessage é definida primeiro para bem-vindo, *nome de usuário*. Em seguida, a propriedade Identity é convertida para um objeto FormsIdentity, de modo que podemos acessar o FormsAuthenticationTicket subjacente. Assim que tivermos o FormsAuthenticationTicket, podemos Desserialize a propriedade de dados do usuário para o nome da empresa e o título. Isso é feito ao dividir a cadeia de caracteres de caractere de barra vertical. O nome da empresa e o título são exibidos no rótulo WelcomeBackMessage.

Figura 5 mostra uma captura de tela nessa exibição em ação. Fazer logon como Scott exibe uma mensagem de back-boas-vinda que inclui a empresa e o título de Scott.


[![Título e empresa atualmente registradas do usuário são exibidos](forms-authentication-configuration-and-advanced-topics-vb/_static/image14.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image13.png)

**Figura 05**: empresa e o título a atualmente registrados do usuário são exibidas ([clique para exibir a imagem em tamanho normal](forms-authentication-configuration-and-advanced-topics-vb/_static/image15.png))


> [!NOTE]
> Propriedade de dados do usuário do tíquete de autenticação serve como um cache para o repositório do usuário. Como qualquer cache, ele precisa ser atualizada quando os dados subjacentes são modificados. Por exemplo, se houver uma página da web do qual os usuários podem atualizar seu perfil, os campos armazenados em cache na propriedade UserData devem atualizados para refletir as alterações feitas pelo usuário.


## <a name="step-5-using-a-custom-principal"></a>Etapa 5: Usando uma entidade de segurança personalizada

Em cada solicitação de entrada FormsAuthenticationModule tenta autenticar o usuário. Se houver um tíquete de autenticação não expiradas, o FormsAuthenticationModule atribui a propriedade HttpContext para um novo objeto GenericPrincipal. Esse objeto GenericPrincipal tem uma identidade de tipo FormsIdentity, que inclui uma referência para o tíquete de autenticação de formulários. A classe GenericPrincipal contém bare funcionalidade mínima necessária para uma classe que implementa IPrincipal - tem apenas uma propriedade de identidade e um método IsInRole.

O objeto de entidade tem duas responsabilidades: para indicar as funções que o usuário pertence e para fornecer informações de identidade. Isso é feito por meio IsInRole da interface IPrincipal (*roleName*) método e propriedade de identidade, respectivamente. A classe GenericPrincipal permite uma matriz de cadeia de caracteres de nomes de função para ser especificados através de seu construtor; seu IsInRole (*roleName*) método simplesmente verifica para ver se passado na *roleName* existe dentro da matriz de cadeia de caracteres. Quando o FormsAuthenticationModule cria o GenericPrincipal, ele passa em uma matriz de cadeia de caracteres vazia para o construtor do GenericPrincipal. Consequentemente, todas as chamadas de IsInRole sempre retornará False.

A classe GenericPrincipal atende às necessidades para a maioria dos cenários de autenticação baseada em formulários em que as funções não são usadas. Nas situações em que o tratamento de função padrão é insuficiente ou quando você precisa associar um objeto de IIdentity personalizado com o usuário, você pode criar um objeto IPrincipal personalizado durante o fluxo de trabalho de autenticação e atribuí-lo para a propriedade HttpContext.

> [!NOTE]
> Como veremos no futuro tutoriais, ao ASP. Framework de funções do .NET está habilitado ele cria um objeto de entidade personalizado do tipo [RolePrincipal](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx) e substitui o objeto de GenericPrincipal formulários criados para autenticação. Ela faz isso para personalizar o IsInRole método principal para interagir com a API da estrutura de funções.


Uma vez que podemos ter não entende sozinhos com funções ainda, o único motivo pelo qual que temos para criar uma entidade personalizada nesse momento seria associar um objeto de IIdentity personalizado para a entidade de segurança. Na etapa 4, examinamos armazenar informações adicionais do usuário na propriedade de dados do usuário do tíquete de autenticação, em particular, o nome do usuário da empresa e seu título. No entanto, as informações de dados do usuário só estão acessível por meio do tíquete de autenticação e, em seguida, apenas como uma cadeia de caracteres serializada, que significa que sempre que desejamos exibir as informações de usuário armazenadas no tíquete, precisamos analisar a propriedade de dados do usuário.

Podemos melhorar a experiência do desenvolvedor, criando uma classe que implementa IIdentity e inclui propriedades CompanyName e título. Dessa forma, um desenvolvedor pode acessar o nome da empresa do usuário conectado no momento e título diretamente por meio das propriedades de título e o CompanyName sem precisava saber como analisar a propriedade de dados do usuário.

### <a name="creating-the-custom-identity-and-principal-classes"></a>Criando a identidade personalizada e Classes de entidade de segurança

Para este tutorial, vamos criar os objetos personalizados de entidade de segurança e identidade no aplicativo\_pasta de código. Comece adicionando um aplicativo\_pasta ao seu projeto de código – clique com botão direito no nome do projeto no Gerenciador de soluções, selecione a opção de adicionar pasta ASP.NET e escolha aplicativo\_código. O aplicativo\_pasta de código é uma pasta especial do ASP.NET que contém arquivos de classe específico para o site.

> [!NOTE]
> O aplicativo\_pasta de código deve ser usada somente durante o gerenciamento de seu projeto por meio do modelo de projeto de site. Se você estiver usando o [modelo de projeto de aplicativo Web](https://msdn.microsoft.com/asp.net/Aa336618.aspx), criar uma pasta padrão e adicionar as classes a ele. Por exemplo, você poderia adicionar uma nova pasta chamada Classes e colocar seu código lá.


Em seguida, adicione dois novos arquivos de classe para o aplicativo\_pasta de código, um CustomIdentity.vb nomeado e outro chamado CustomPrincipal.vb.


[![Adicione as Classes de CustomPrincipal e CustomIdentity ao seu projeto](forms-authentication-configuration-and-advanced-topics-vb/_static/image17.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image16.png)

**Figura 06**: adicionar as Classes de CustomPrincipal e CustomIdentity ao seu projeto ([clique para exibir a imagem em tamanho normal](forms-authentication-configuration-and-advanced-topics-vb/_static/image18.png))


A classe CustomIdentity é responsável por implementar a interface IIdentity, que define as propriedades AuthenticationType, IsAuthenticated e nome. Além dessas propriedades necessárias, estamos interessados em expor o subjacente tíquete de autenticação de formulários, bem como as propriedades para o nome da empresa e o título do usuário. Insira o código a seguir para a classe CustomIdentity.

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample9.vb)]

Observe que a classe inclui uma variável de membro FormsAuthenticationTicket (\_tíquete) e que essas informações de permissão devem ser fornecidas por meio do construtor. Esses dados de tíquete são usados para retornar o nome da identidade; sua propriedade UserData é analisada para retornar os valores para as propriedades CompanyName e título.

Em seguida, crie a classe CustomPrincipal. Como não estamos preocupados com as funções nesse momento, o construtor da classe CustomPrincipal aceita apenas um objeto de CustomIdentity; o método IsInRole sempre retorna False.

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample10.vb)]

### <a name="assigning-a-customprincipal-object-to-the-incoming-requests-security-context"></a>A atribuição de um objeto CustomPrincipal ao contexto de segurança de solicitação de entrada

Agora temos uma classe que estende a especificação de IIdentity padrão para incluir propriedades CompanyName e título, bem como uma classe de entidade personalizada que usa a identidade personalizada. Estamos prontos para a etapa no pipeline do ASP.NET e atribuir nosso objeto de entidade personalizado para o contexto de segurança de solicitação de entrada.

O pipeline do ASP.NET leva uma solicitação de entrada e processa-o por meio de uma série de etapas. Em cada etapa, um determinado evento é gerado, o que possibilita aos desenvolvedores aproveitar o pipeline do ASP.NET e modificar a solicitação em determinados pontos do ciclo de vida. O FormsAuthenticationModule, por exemplo, espera por ASP.NET gerar a [evento AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx), no ponto em que ele inspeciona a solicitação de entrada para um tíquete de autenticação. Se for encontrado um tíquete de autenticação, um objeto GenericPrincipal é criado e atribuído à propriedade HttpContext.

Após o evento AuthenticateRequest, o pipeline do ASP.NET gera o [PostAuthenticateRequest evento](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx), que é onde podemos pode substituir o objeto GenericPrincipal criado pelo FormsAuthenticationModule com uma instância do nosso Objeto CustomPrincipal. Figura 7 ilustra esse fluxo de trabalho.


[![O GenericPrincipal é substituído por um CustomPrincipal no evento PostAuthenticationRequest](forms-authentication-configuration-and-advanced-topics-vb/_static/image20.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image19.png)

**Figura 07**: O GenericPrincipal é substituído por um CustomPrincipal no evento PostAuthenticationRequest ([clique para exibir a imagem em tamanho normal](forms-authentication-configuration-and-advanced-topics-vb/_static/image21.png))


Para executar código em resposta a um evento de pipeline do ASP.NET, podemos pode criar o manipulador de eventos apropriado no global. asax ou criar nosso próprio módulo de HTTP. Para este tutorial vamos criar o manipulador de eventos no global. asax. Comece adicionando global. asax ao seu site. Clique com botão direito no nome do projeto no Gerenciador de soluções e adicione um item de tipo de classe de aplicativo Global chamado global. asax.


[![Adicionar um arquivo global asax ao seu site](forms-authentication-configuration-and-advanced-topics-vb/_static/image23.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image22.png)

**Figura 08**: adicionar um arquivo global asax ao seu site ([clique para exibir a imagem em tamanho normal](forms-authentication-configuration-and-advanced-topics-vb/_static/image24.png))


O modelo padrão do global. asax inclui manipuladores de eventos para um número de eventos do pipeline ASP.NET, incluindo o início, fim e [evento de erro](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx), entre outros. Fique à vontade remover esses manipuladores de eventos, como não precisamos delas para este aplicativo. O evento que estamos interessados em é PostAuthenticateRequest. Atualize o arquivo global. asax para que sua marcação fique semelhante ao seguinte:

[!code-aspx[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample11.aspx)]

O aplicativo\_OnPostAuthenticateRequest método executado cada vez que o tempo de execução do ASP.NET gera o evento de PostAuthenticateRequest, que ocorre uma vez em cada solicitação de página de entrada. Inicia o manipulador de eventos para verificar se o usuário é autenticado e foi autenticado por meio da autenticação de formulários. Nesse caso, um novo objeto CustomIdentity é criado e passado o tíquete de autenticação da solicitação atual em seu construtor. Depois disso, um objeto CustomPrincipal é criado e recebe o objeto de CustomIdentity recém-criada em seu construtor. Por fim, o contexto de segurança da solicitação atual é atribuído ao objeto CustomPrincipal recém-criado.

Observe que a última etapa – associando o objeto CustomPrincipal o contexto de segurança da solicitação - atribui a entidade de segurança a duas propriedades: HttpContext e thread. CurrentPrincipal. Essas duas atribuições são necessárias devido à maneira contextos de segurança são tratados no ASP.NET. O .NET Framework associa um contexto de segurança a cada thread em execução; Essas informações estão disponíveis como um objeto IPrincipal por meio de [objeto de Thread](https://msdn.microsoft.com/library/system.threading.thread.aspx)do [propriedade CurrentPrincipal](https://msdn.microsoft.com/library/system.threading.thread.currentcontext.aspx). O que é uma confusão é que o ASP.NET tem suas próprias informações de contexto de segurança (HttpContext).

Em determinados cenários, a propriedade de thread. CurrentPrincipal é examinada para determinar o contexto de segurança; em outros cenários, HttpContext é usado. Por exemplo, há recursos de segurança do .NET que permitem aos desenvolvedores declarativamente de estado que os usuários ou funções podem instanciar uma classe ou invocar métodos específicos (consulte [adicionando regras de autorização para os negócios e camadas de dados usando PrincipalPermissionAttributes](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)). Nos bastidores, essas técnicas declarativas determinam o contexto de segurança por meio da propriedade de thread. CurrentPrincipal.

Em outros cenários, a propriedade HttpContext é usada. Por exemplo, no tutorial anterior, nós usamos essa propriedade para exibir o nome de usuário do usuário conectado no momento. Claramente, em seguida, é imperativo que as informações de contexto de segurança nas propriedades do thread. CurrentPrincipal e HttpContext correspondem.

O tempo de execução do ASP.NET sincroniza automaticamente esses valores de propriedade para nós. No entanto, esta sincronização ocorre após o evento AuthenticateRequest, mas *antes de* o evento PostAuthenticateRequest. Consequentemente, ao adicionar uma entidade personalizada no evento PostAuthenticateRequest precisamos ter certeza de atribuir manualmente o thread. CurrentPrincipal ou então um thread. CurrentPrincipal e HttpContext estará fora de sincronia. Consulte [Context.User vs. Thread. CurrentPrincipal](http://leastprivilege.com/2005/11/23/context-user-vs-thread-currentprincipal/) para uma discussão mais detalhada sobre esse problema.

### <a name="accessing-the-companyname-and-title-properties"></a>Acessando o CompanyName e propriedades do título

Sempre que uma solicitação chega e é expedida para o mecanismo do ASP.NET, o aplicativo\_OnPostAuthenticateRequest o manipulador de eventos no global. asax será acionado. Se a solicitação foi autenticada com êxito pelo FormsAuthenticationModule, o manipulador de eventos criará um novo objeto CustomPrincipal com um objeto CustomIdentity com base no tíquete de autenticação de formulários. Com essa lógica em vigor, o acesso a informações sobre o nome da empresa e o título do usuário conectado no momento é incrivelmente simples.

Retornar à página\_manipulador de eventos de carga em Default. aspx, onde na etapa 4, escrevemos código para recuperar o tíquete de autenticação de formulário e analisar a propriedade de dados do usuário para exibir o nome da empresa e o título do usuário. Com os objetos CustomPrincipal e CustomIdentity em uso agora, não é necessário para analisar os valores de propriedade de dados do usuário do tíquete. Em vez disso, basta obter uma referência ao objeto CustomIdentity e usar suas propriedades CompanyName e título:

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample12.vb)]

## <a name="summary"></a>Resumo

Neste tutorial, examinamos como personalizar configurações do sistema de autenticação de formulários por meio do Web. config. Analisamos como a expiração do tíquete de autenticação é tratada e como as garantias de criptografia e validação são usadas para proteger o tíquete de inspeção e a modificação. Por fim, discutimos usando a propriedade de dados do usuário do tíquete de autenticação para armazenar informações de usuário adicionais na permissão em si e como usar objetos personalizados de entidade de segurança e identidade para expor essas informações de maneira mais amigável para desenvolvedores.

Esse tutorial conclui nossa análise de autenticação de formulários no ASP.NET. O próximo tutorial inicia nossa jornada para a estrutura de associação.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Dissecando autenticação de formulários](http://aspnet.4guysfromrolla.com/articles/072005-1.aspx)
- [Explicado: Autenticação de formulários no ASP.NET 2.0](https://msdn.microsoft.com/library/aa480476.aspx)
- [Como Proteger a autenticação de formulários no ASP.NET 2.0](https://msdn.microsoft.com/library/ms998310.aspx)
- [Professional ASP.NET 2.0 segurança, associação e gerenciamento de função](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698 do MSSQLSERVER-8)
- [Protegendo os controles de logon](https://msdn.microsoft.com/library/ms178346.aspx)
- [O &lt;autenticação&gt; elemento](https://msdn.microsoft.com/library/532aee0e.aspx)
- [O &lt;formulários&gt; elemento para &lt;autenticação&gt;](https://msdn.microsoft.com/library/1d3t3c61.aspx)
- [O &lt;machineKey&gt; elemento](https://msdn.microsoft.com/library/w8h3skw9.aspx)
- [Noções básicas sobre o tíquete de autenticação de formulários e o Cookie](https://support.microsoft.com/kb/910443)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Treinamento em vídeo sobre os tópicos contidos neste tutorial

- [Como alterar as propriedades de autenticação de formulários](../../../videos/authentication/how-to-change-the-forms-authentication-properties.md)
- [Como configurar e usar autenticação sem cookies em um aplicativo ASP.NET](../../../videos/authentication/how-to-setup-and-use-cookie-less-authentication-in-an-aspnet-application.md)
- [Realocação de logon de formulários do ASP](../../../videos/authentication/asp-forms-login-relocation.md)
- [Configuração de chave personalizada de logon de formulários](../../../videos/authentication/forms-login-custom-key-configuration.md)
- [Adicionar dados personalizados ao método de autenticação](../../../videos/authentication/add-custom-data-to-the-authentication-method.md)
- [Usar objetos de entidade de segurança personalizada](../../../videos/authentication/use-custom-principal-objects.md)

### <a name="about-the-author"></a>Sobre o autor

Scott Mitchell, autor de vários livros sobre ASP/ASP.NET e fundador da 4GuysFromRolla.com, trabalha com tecnologias Web Microsoft desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é  *[Sams Teach por conta própria ASP.NET 2.0 em 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott pode ser contatado pelo [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) ou por meio de seu blog em [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Revisor de avanço para este tutorial foi Alicja Maziarz. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Anterior](an-overview-of-forms-authentication-vb.md)
