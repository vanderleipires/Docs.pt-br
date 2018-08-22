---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
title: Noções básicas sobre a autenticação do AJAX ASP.NET e serviços de aplicativo de perfil | Microsoft Docs
author: scottcate
description: O serviço de autenticação permite que os usuários forneçam credenciais para receber um cookie de autenticação e é o serviço de gateway para permitir que o usuário personalizada...
ms.author: riande
ms.date: 03/14/2008
ms.assetid: 6ab4efb6-aab6-45ac-ad2c-bdec5848ef9e
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
msc.type: authoredcontent
ms.openlocfilehash: d722130e625a9f867923280fce0ef35f19bfeb9d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830031"
---
<a name="understanding-aspnet-ajax-authentication-and-profile-application-services"></a>Noções básicas sobre a autenticação do AJAX ASP.NET e serviços de aplicativo de perfil
====================
por [Scott Cate](https://github.com/scottcate)

[Baixar PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial03_MSAjax_ASP.NET_Services_cs.pdf)

> O serviço de autenticação permite que os usuários forneçam credenciais para receber um cookie de autenticação e o serviço de gateway para permitir que os perfis de usuário personalizada que é fornecido pelo ASP.NET. O uso do serviço de autenticação do ASP.NET AJAX é compatível com a autenticação de formulários do ASP.NET padrão, para que aplicativos que atualmente usam autenticação de formulários (por exemplo, com o logon de controle) não quebrará atualizando para o serviço de autenticação do AJAX.


## <a name="introduction"></a>Introdução

Como parte do .NET Framework 3.5, a Microsoft está fornecendo uma atualização no ambiente dimensionável; não só está disponível um novo ambiente de desenvolvimento, mas os novos recursos de consulta integrada à linguagem (LINQ) e outros aprimoramentos de linguagem serão disponibilizados em breve. Além disso, alguns recursos familiares do outros conjuntos de ferramentas, especialmente as extensões do AJAX ASP.NET, estão sendo incluídos como membros de primeira classe da biblioteca de classe Base do .NET Framework. Essas extensões permitem vários novos recursos de cliente avançados, inclusive a renderização parcial de páginas sem exigir uma atualização de página inteira, a capacidade de acessar serviços Web por meio de script de cliente (incluindo a API de criação de perfil do ASP.NET) e uma extensa API do lado do cliente projetado para espelhar a muitos dos esquemas de controle vistos no conjunto de controle de servidor ASP.NET.

Este white paper aborda a implementação e o uso de criação de perfil do ASP.NET e serviços de autenticação de formulários como eles são expostos por extensões de AJAX do Microsoft ASP.NET AJAX ExtensionsThe fazer autenticação de formulários incrivelmente fácil o suporte, como ela (bem como o serviço de criação de perfil) é exposto por meio de um script de proxy de serviço Web. As extensões AJAX também dão suporte a autenticação personalizada por meio da classe AuthenticationServiceManager.

Este white paper se baseia na versão Beta 2 do Visual Studio 2008 e o .NET Framework 3.5. Este white paper também pressupõe que você estará trabalhando com o Visual Studio 2008 Beta 2, não Visual Web Developer Express e fornecerá instruções passo a passo de acordo com a interface do usuário do Visual Studio. Alguns exemplos de código podem utilizar modelos de projeto não está disponíveis no Visual Web Developer Express.

## <a name="profiles-and-authentication"></a>*Autenticação e perfis*

Os perfis do Microsoft ASP.NET e serviços de autenticação são fornecidos pelo sistema de autenticação de formulários ASP.NET e são componentes padrão do ASP.NET. As extensões do AJAX ASP.NET fornecem acesso de script para esses serviços por meio de proxies de script, através de um modelo bastante simples sob o namespace sys da biblioteca cliente AJAX.

O serviço de autenticação permite que os usuários forneçam credenciais para receber um cookie de autenticação e o serviço de gateway para permitir que os perfis de usuário personalizada que é fornecido pelo ASP.NET. O uso do serviço de autenticação do ASP.NET AJAX é compatível com a autenticação de formulários do ASP.NET padrão, para que aplicativos que atualmente usam autenticação de formulários (por exemplo, com o logon de controle) não quebrará atualizando para o serviço de autenticação do AJAX.

O serviço de perfil permite a integração automática e o armazenamento de dados do usuário com base na associação, conforme fornecido pelo serviço de autenticação. Os dados armazenados são especificados pelo arquivo Web. config, e os vários provedores de serviço de criação de perfil lidar com o gerenciamento de dados. Assim como acontece com o serviço de autenticação, o serviço de perfil do AJAX é compatível com o serviço de perfil do ASP.NET padrão, para que as páginas no momento, incorporando recursos do serviço de perfil do ASP.NET não devem ser quebradas, incluindo suporte a AJAX.

Incorporar a autenticação do ASP.NET e os próprios serviços de criação de perfil em um aplicativo está fora do escopo deste white paper. Para obter mais informações sobre o tópico, consulte Biblioteca MSDN Gerenciando usuários usando associação no artigo de referência [ https://msdn.microsoft.com/library/tw292whz.aspx ](https://msdn.microsoft.com/library/tw292whz.aspx). ASP.NET também inclui um utilitário para configurar automaticamente a associação com o SQL Server, que é o provedor de serviço de autenticação padrão para a associação ASP.NET. Para obter mais informações, consulte o artigo da ferramenta de registro do ASP.NET SQL Server (Aspnet\_regsql.exe) no [ https://msdn.microsoft.com/library/ms229862(vs.80).aspx ](https://msdn.microsoft.com/library/ms229862(vs.80).aspx).

## <a name="using-the-aspnet-ajax-authentication-service"></a>*Usando o serviço de autenticação do ASP.NET AJAX*

O serviço de autenticação do ASP.NET AJAX deve ser habilitado no arquivo Web. config:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample1.xml)]

O serviço de autenticação requer autenticação de formulários do ASP.NET para ser habilitado e os cookies estejam habilitados no navegador do cliente (um script não é possível habilitar a uma sessão sem cookies, pois sessões sem cookie exigem parâmetros de URL).

Depois que o serviço de autenticação do AJAX é habilitado e configurado, o script de cliente imediatamente pode tirar proveito do objeto AuthenticationService. Basicamente, script de cliente vai querer aproveitar o `login` método e `isLoggedIn` propriedade. Existem várias propriedades para fornecer padrões para o método de logon, que pode aceitar um grande número de parâmetros.

*Membros de AuthenticationService*

*método de logon:*

O método login () inicia uma solicitação para autenticar as credenciais do usuário. Esse método é assíncrono e não bloqueia a execução.

*Parâmetros:*

| **Nome do parâmetro** | **Significado** |
| --- | --- |
| userName | Necessário. O nome de usuário para autenticar. |
| password | Opcional (o padrão é nulo). A senha do usuário. |
| isPersistent | Opcional (o padrão é false). Se o cookie de autenticação do usuário deve persistir entre as sessões. Se for false, o usuário será fazer logoff quando o navegador é fechado ou a sessão expirar. |
| URL de redirecionamento | Opcional (o padrão é nulo). A URL para redirecionar o navegador após a autenticação bem-sucedida. Se esse parâmetro for nulo ou uma cadeia de caracteres vazia, nenhum redirecionamento ocorrerá. |
| customInfo | Opcional (o padrão é nulo). Esse parâmetro não está sendo utilizado e é reservado para uso futuro. |
| loginCompletedCallback | Opcional (o padrão é nulo). A função ser chamada quando o logon for concluída com êxito. Se for especificado, esse parâmetro substitui a propriedade defaultLoginCompleted. |
| failedCallback | Opcional (o padrão é nulo). A função ser chamada quando o logon falhou. Se for especificado, esse parâmetro substitui a propriedade defaultFailedCallback. |
| userContext | Opcional (o padrão é nulo). Dados de contexto de usuário personalizada que devem ser passados para as funções de retorno de chamada. |

*Valor de retorno:*

Essa função não inclui um valor de retorno. No entanto, um número de comportamentos está incluído após a conclusão de uma chamada para essa função:

- A página atual será atualizada ou ser alterada se o `redirectUrl` parâmetro não era nulo nem uma cadeia de caracteres vazia.
- No entanto, se o parâmetro era nulo ou uma cadeia de caracteres vazia, o `loginCompletedCallback` parâmetro, ou `defaultLoginCompletedCallback` propriedade é chamada.
- Se a chamada para o serviço web falhar, o `failedCallback` parâmetro de `defaultFailedCallback` propriedade é chamada.

*método logoff:*

O método logout() remove o cookie de credenciais e faz logoff do usuário atual do aplicativo web.

*Parâmetros:*

| **Nome do parâmetro** | **Significado** |
| --- | --- |
| URL de redirecionamento | Opcional (o padrão é nulo). A URL para redirecionar o navegador após a autenticação bem-sucedida. Se esse parâmetro for nulo ou uma cadeia de caracteres vazia, nenhum redirecionamento ocorrerá. |
| logoutCompletedCallback | Opcional (o padrão é nulo). A função ser chamada quando o logout foi concluída com êxito. Se for especificado, esse parâmetro substitui a propriedade defaultLogoutCompleted. |
| failedCallback | Opcional (o padrão é nulo). A função ser chamada quando o logon falhou. Se for especificado, esse parâmetro substitui a propriedade defaultFailedCallback. |
| userContext | Opcional (o padrão é nulo). Dados de contexto de usuário personalizada que devem ser passados para as funções de retorno de chamada. |

*Valor de retorno:*

Essa função não inclui um valor de retorno. No entanto, um número de comportamentos está incluído após a conclusão de uma chamada para essa função:

- A página atual será atualizada ou ser alterada se o `redirectUrl` parâmetro não era nulo nem uma cadeia de caracteres vazia.
- No entanto, se o parâmetro era nulo ou uma cadeia de caracteres vazia, o `logoutCompletedCallback` parâmetro, ou `defaultLogoutCompletedCallback` propriedade é chamada.
- Se a chamada para o serviço web falhar, o `failedCallback` parâmetro de `defaultFailedCallback` propriedade é chamada.

*propriedade defaultFailedCallback (get, set):*

Esta propriedade especifica uma função que deve ser chamada se ocorrer uma falha na comunicação com o serviço web. Ele deve receber um delegado (ou uma referência de função).

A referência de função especificada por essa propriedade deve ter a seguinte assinatura:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample2.js)]

*Parâmetros:*

| **Nome do parâmetro** | **Significado** |
| --- | --- |
| erro | Especifica as informações de erro. |
| userContext | Especifica as informações de contexto de usuário fornecidas quando a função de logon ou logoff foi chamada. |
| methodName | O nome do método de chamada. |

*propriedade defaultLoginCompletedCallback (get, set):*

Esta propriedade especifica uma função que deve ser chamada quando a chamada de serviço da web de logon foi concluída. Ele deve receber um delegado (ou uma referência de função).

A referência de função especificada por essa propriedade deve ter a seguinte assinatura:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample3.js)]

*Parâmetros:*

| **Nome do parâmetro** | **Significado** |
| --- | --- |
| validCredentials | Especifica se o usuário forneceu credenciais válidas. `true` Se o usuário fez logon com êxito em; Caso contrário, `false`. |
| userContext | Especifica as informações de contexto de usuário fornecidas quando a função de logon foi chamada. |
| methodName | O nome do método de chamada. |

*propriedade defaultLogoutCompletedCallback (get, set):*

Esta propriedade especifica uma função que deve ser chamada quando a chamada de serviço da web de logout foi concluída. Ele deve receber um delegado (ou uma referência de função).

A referência de função especificada por essa propriedade deve ter a seguinte assinatura:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample4.js)]

*Parâmetros:*

| **Nome do parâmetro** | **Significado** |
| --- | --- |
| resultado | Esse parâmetro sempre será `null`; ela é reservada para uso futuro. |
| userContext | Especifica as informações de contexto de usuário fornecidas quando a função de logon foi chamada. |
| methodName | O nome do método de chamada. |

*Propriedade isLoggedIn (get):*

Essa propriedade obtém o estado atual de autenticação do usuário; ele é definido pelo objeto ScriptManager durante uma solicitação de página.

Essa propriedade retornará `true` se o usuário está conectado no momento; caso contrário, ele retornará `false`.

*propriedade de caminho (get, set):*

Programaticamente, essa propriedade determina o local do serviço de autenticação da web. Ele pode ser usado para substituir o provedor de autenticação padrão, bem como uma definidas declarativamente na propriedade do caminho do nó do filho do controle ScriptManager AuthenticationService (para obter mais informações, consulte usando um provedor de serviço de autenticação personalizado tópico abaixo).

Observe que não altera o local do serviço de autenticação padrão. No entanto, ASP.NET AJAX permite que você especifique o local de um serviço web que fornece a mesma interface de classe que o proxy de serviço de autenticação do ASP.NET AJAX.

Observe também que essa propriedade não deve ser definida como um valor que direciona a solicitação de script fora do site atual. Como o aplicativo atual não deve receber as credenciais de autenticação, seria inútil. Além disso, o AJAX de tecnologia subjacente não deve lançar solicitações entre sites e pode gerar uma exceção de segurança no navegador do cliente.

Esta propriedade é um `String` objeto que representa o caminho para o serviço de autenticação da web.

*propriedade de tempo limite (get, set):*

Essa propriedade determina o período de tempo de espera para o serviço de autenticação antes de assumir que a solicitação de logon falhou. Se o tempo limite expirar enquanto aguarda uma chamada ser concluído, o retorno de chamada com falha na solicitação será chamado e a chamada não será concluída.

Esta propriedade é um `Number` objeto que representa o número de milissegundos para aguardar os resultados do serviço de autenticação.

*Exemplo de código: Registro em log para o serviço de autenticação*

A marcação a seguir é um exemplo de página ASP.NET com uma chamada de script simples para os métodos de logon e logoff da classe AuthenticationService.

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample5.aspx)]

## <a name="accessing-aspnet-profiling-data-via-ajax"></a>Acessando a criação de perfil dados por meio do AJAX ASP.NET

O serviço de criação de perfil do ASP.NET também é exposta por meio de extensões AJAX do ASP.NET. Como o serviço de criação de perfil do ASP.NET fornece uma API rica e granular por para armazenar e recuperar dados do usuário, isso pode ser uma ferramenta excelente produtividade.

O serviço de perfil deve ser habilitado em Web. config; não é por padrão. Para fazer isso, certifique-se de que o `profileService` elemento filho tiver habilitado = Web. config especificado em true e, se você especificou as propriedades que podem ser lidos ou gravadas da seguinte maneira:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample6.xml)]

O serviço de perfil também deve ser configurado. Embora a configuração do serviço de criação de perfil está fora do escopo deste white paper, vale a pena observar que os grupos, conforme definido nas configurações de perfil poderá ser acessados como subpropriedades do nome do grupo. Por exemplo, com a seguinte seção de perfil especificada:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample7.xml)]

Script de cliente seja capaz de acessar o nome, Address.Line1, Address.Line2, Address.City, Address.State, Address.Zip e BackgroundColor como propriedades do campo de propriedades da classe ProfileService.

Depois que o serviço de criação de perfil do AJAX é configurado, ele estará imediatamente disponível nas páginas; No entanto, ele precisa ser carregado uma vez antes de usar.

*ProfileService membros*

*campo de propriedades:*

O campo de propriedades expõe todos os dados de perfil configurado como propriedades filho que podem ser referenciadas pela convenção de nome de operador de ponto. As propriedades que são filhos dos grupos de propriedade são chamadas de GroupName.PropertyName. Na configuração de perfil de exemplo apresentada acima para obter o estado do usuário, você pode usar o seguinte identificador:

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample8.cs)]

*método Load:*

Carrega uma lista selecionada ou todas as propriedades do servidor.

*Parâmetros:*

| **Nome do parâmetro** | **Significado** |
| --- | --- |
| propertyNames | Opcional (o padrão é nulo). As propriedades a ser carregada do servidor. |
| loadCompletedCallback | Opcional (o padrão é nulo). A função ser chamada durante o carregamento foi concluído. |
| failedCallback | Opcional (o padrão é nulo). A função ser chamada se ocorrer um erro. |
| userContext | Opcional (o padrão é nulo). Informações de contexto a ser passado para a função de retorno de chamada. |

A função de carregamento não tem um valor de retorno. Se a chamada foi concluída com êxito, ele irá chamar qualquer uma de `loadCompletedCallback` parâmetro ou `defaultLoadCompletedCallback` propriedade. Se a chamada falhou ou o tempo limite expirou, ou o `failedCallback` parâmetro ou `defaultFailedCallback` propriedade será chamada.

Se o `propertyNames` parâmetro não for fornecido, todas as propriedades configuradas de leitura são recuperadas do servidor.

*Salve método:*

O método Save () salva a lista de propriedades especificada (ou todas as propriedades) para o perfil do usuário ASP.NET.

*Parâmetros:*

| **Nome do parâmetro** | **Significado** |
| --- | --- |
| propertyNames | Opcional (o padrão é nulo). As propriedades a ser salvo no servidor. |
| saveCompletedCallback | Opcional (o padrão é nulo). A função ser chamada quando o salvamento foi concluída. |
| failedCallback | Opcional (o padrão é nulo). A função ser chamada se ocorrer um erro. |
| userContext | Opcional (o padrão é nulo). Informações de contexto a ser passado para a função de retorno de chamada. |

Salvar função não tem um valor de retorno. Se a chamada for concluída com êxito, ele irá chamar o `saveCompletedCallback` parâmetro ou `defaultSaveCompletedCallback` propriedade. Se a chamada falhou ou o tempo limite expirou, ou o `failedCallback` ou `defaultFailedCallback` propriedade será chamada.

Se o `propertyNames` parâmetro for nulo, todas as propriedades de perfil serão enviadas para o servidor e o servidor decidirá quais propriedades podem ser salvos e quais não podem.

*propriedade defaultFailedCallback (get, set):*

Esta propriedade especifica uma função que deve ser chamada se ocorrer uma falha na comunicação com o serviço web. Ele deve receber um delegado (ou uma referência de função).

A referência de função especificada por essa propriedade deve ter a seguinte assinatura:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample9.js)]

*Parâmetros:*

| **Nome do parâmetro** | **Significado** |
| --- | --- |
| Erro | Especifica as informações de erro. |
| userContext | Especifica as informações de contexto de usuário fornecidas quando a carga ou a função de salvamento foi chamado. |
| methodName | O nome do método de chamada. |

*propriedade defaultSaveCompleted (get, set):*

Esta propriedade especifica uma função que deve ser chamada após a conclusão de salvar os dados de perfil do usuário. Ele deve receber um delegado (ou uma referência de função).

A referência de função especificada por essa propriedade deve ter a seguinte assinatura:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample10.js)]

*Parâmetros:*

| **Nome do parâmetro** | **Significado** |
| --- | --- |
| numPropsSaved | Especifica o número de propriedades que foram salvos. |
| userContext | Especifica as informações de contexto de usuário fornecidas quando a carga ou a função de salvamento foi chamado. |
| methodName | O nome do método de chamada. |

*propriedade defaultLoadCompleted (get, set):*

Esta propriedade especifica uma função que deve ser chamada após a conclusão do carregamento dos dados de perfil do usuário. Ele deve receber um delegado (ou uma referência de função).

A referência de função especificada por essa propriedade deve ter a seguinte assinatura:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample11.js)]

*Parâmetros:*

| **Nome do parâmetro** | **Significado** |
| --- | --- |
| numPropsLoaded | Especifica o número de propriedades carregado. |
| userContext | Especifica as informações de contexto de usuário fornecidas quando a carga ou a função de salvamento foi chamado. |
| methodName | O nome do método de chamada. |

*propriedade de caminho (get, set):*

Programaticamente, essa propriedade determina o local do serviço da web de perfil. Ele pode ser usado para substituir o provedor de serviço de perfil de padrão, bem como uma definidas declarativamente na propriedade do caminho do nó do filho do controle ScriptManager ProfileService.

Observe que não altera o local do serviço de perfil padrão. No entanto, ASP.NET AJAX permite que você especifique o local de um serviço web que fornece a mesma interface de classe que o proxy de serviço de autenticação do ASP.NET AJAX.

Observe também que essa propriedade não deve ser definida como um valor que direciona a solicitação de script fora do site atual. O AJAX de tecnologia subjacente não deve lançar solicitações entre sites e pode gerar uma exceção de segurança no navegador do cliente.

Esta propriedade é um `String` objeto que representa o caminho para o serviço web de perfil.

*propriedade de tempo limite (get, set):*

Essa propriedade determina o período de tempo para aguardar o serviço de perfil antes de assumir a carga ou salvar a solicitação falhou. Se o tempo limite expirar enquanto aguarda uma chamada ser concluído, o retorno de chamada com falha na solicitação será chamado e a chamada não será concluída.

Esta propriedade é um `Number` objeto que representa o número de milissegundos para aguardar os resultados do serviço de perfil.

*Exemplo de código: Carregando dados de perfil no carregamento da página*

O código a seguir verifica se um usuário é autenticado e nesse caso, carregará cor de plano de fundo preferencial do usuário como a página.

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample12.js)]

## <a name="using-a-custom-authentication-service-provider"></a>*Usando um provedor de serviços de autenticação personalizada*

Extensões AJAX do ASP.NET permitem que você crie um provedor de serviços de autenticação de script personalizado ao expor sua funcionalidade por meio de um serviço da web personalizado. Para ser usado, o serviço web deve expor dois métodos, `Login` e `Logout`; e esses métodos devem ser especificados com as mesmas assinaturas de método como um serviço da web de autenticação do ASP.NET AJAX padrão.

Depois de criar o serviço da web personalizado, você precisará especificar o caminho para ele, seja declarativamente em sua página, por meio de programação em código, ou por meio de script de cliente.

*Para definir o caminho declarativamente:*

Para definir o caminho de forma declarativa, incluem AuthenticationService filho do objeto ScriptManager na sua página ASP.NET:

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample13.aspx)]

*Para definir o caminho no código:*

Para definir o caminho por meio de programação, especifique o caminho pela instância do seu Gerenciador de scripts:

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample14.cs)]

*Para definir o caminho no script:*

Para definir o caminho programaticamente no script, utilize o `path` propriedade da classe AuthenticationService:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample15.js)]

*Sample Web Service de autenticação personalizada*

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample16.aspx)]

## <a name="summary"></a>Resumo

Serviços do ASP.NET – especificamente, os serviços de criação de perfil, a associação e autenticação - são facilmente expostos ao JavaScript no navegador do cliente. Isso permite que os desenvolvedores integrem perfeitamente, seu código do lado do cliente com o mecanismo de autenticação sem depender de controles como UpdatePanels para fazer o trabalho pesado. Dados de perfil podem ser protegidos do cliente, utilizando as definições de configuração da web; Nenhum dado está disponível por padrão, e os desenvolvedores devem participar de propriedades de perfil.

Além disso, ao criar implementações de serviço web simplificada com assinaturas de método equivalente, os desenvolvedores podem criar provedores de script personalizado para esses serviços ASP.NET intrínseco. Suporte para essas técnicas simplifica o desenvolvimento de aplicativos cliente avançados, além de fornecer aos desenvolvedores uma ampla gama de flexibilidade para atender às necessidades específicas.

## <a name="bio"></a>*Biografia*

Scott Cate tem trabalhado com tecnologias Web Microsoft desde 1997 e é o presidente da myKB.com ([www.myKB.com](http://www.myKB.com)) onde ele é especialista na escrita de ASP.NET com base em aplicativos com foco em soluções de Software da Base de dados de Conhecimento. Scott pode ser contatado através do email [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) ou em seu blog em [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Anterior](understanding-asp-net-ajax-updatepanel-triggers.md)
> [Próximo](understanding-asp-net-ajax-localization.md)
