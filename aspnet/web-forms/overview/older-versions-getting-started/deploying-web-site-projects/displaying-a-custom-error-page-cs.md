---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-cs
title: "Exibindo uma página de erro personalizada (c#) | Microsoft Docs"
author: rick-anderson
description: "O que o usuário ver quando ocorre um erro de tempo de execução em um aplicativo web ASP.NET? A resposta depende de como o site &lt;customErrors&gt; configuração..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: cb061642-faf3-41b2-9372-69e13444d458
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 8d68dedfc1f606cc6f0381bcbdb3f65c1ea3b2e5
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
<a name="displaying-a-custom-error-page-c"></a>Exibindo uma página de erro personalizada (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_11_CS.zip) ou [baixar PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial11_CustomErrors_cs.pdf)

> O que o usuário ver quando ocorre um erro de tempo de execução em um aplicativo web ASP.NET? A resposta depende de como o site &lt;customErrors&gt; configuração. Por padrão, os usuários veem uma tela de amarela má aparência que exaltam que ocorreu um erro de tempo de execução. Este tutorial mostra como personalizar essas configurações para exibir um estética-forma de página de erro personalizada que corresponda a aparência do seu site.


## <a name="introduction"></a>Introdução

Em um mundo perfeito, não deve haver nenhum erro de tempo de execução. Programadores deve escrever o código ao fim sem nenhum um bug e com a validação de entrada do usuário robusto e externos recursos, como servidores de email e servidores de banco de dados nunca deve ir offline. É claro que, na realidade, erros são inevitáveis. As classes do .NET Framework sinalizam um erro ao gerar uma exceção. Por exemplo, o método Open do objeto chamando uma SqlConnection estabelece uma conexão ao banco de dados especificado por uma cadeia de caracteres de conexão. No entanto, se o banco de dados está inativo ou se as credenciais na cadeia de conexão são inválidas, em seguida, o método Open lança um `SqlException`. Exceções podem ser manipuladas pelo uso de `try/catch/finally` blocos. Se código dentro de um `try` bloco lançar uma exceção, o controle é transferido para o bloco catch apropriado em que o desenvolvedor pode tentar recuperar do erro. Se não há nenhum bloco catch correspondente, ou se o código que lançou a exceção não estiver em um bloco try, a exceção percolates a pilha de chamadas search de `try/catch/finally` blocos.

Se a exceção bolhas todo o caminho até o tempo de execução do ASP.NET sem que está sendo tratado, o [ `HttpApplication` classe](https://msdn.microsoft.com/library/system.web.httpapplication.aspx)do [ `Error` evento](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx) é gerado e configurado *página de erro*  é exibido. Por padrão, o ASP.NET exibe uma página de erro que normalmente é referida como o [amarelo tela de morte](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow) (YSOD). Há duas versões do YSOD: um mostra os detalhes da exceção, um rastreamento de pilha e outras informações úteis para os desenvolvedores a depurar o aplicativo (consulte **Figura 1**); o outro simplesmente informando que houve um erro de tempo de execução (consulte  **Figura 2**).

Os detalhes da exceção YSOD é bastante útil para desenvolvedores para depurar o aplicativo, mas mostrar um YSOD aos usuários finais é pegajosas e não profissional. Em vez disso, os usuários finais devem ser tomados para uma página de erro que mantém a aparência do site com texto mais amigável que descreve a situação. A boa notícia é que é muito fácil criar esse tipo de página de erro personalizada. Este tutorial começa com uma olhada em ASP. Páginas de erro diferente do NET. Em seguida, mostra como configurar o aplicativo web para mostrar aos usuários uma página de erro personalizada caso ocorra um erro.

### <a name="examining-the-three-types-of-error-pages"></a>Examinando os três tipos de páginas de erro

Quando uma exceção não tratada ocorre em um aplicativo ASP.NET, um dos três tipos de páginas de erro é exibida:

- A página de erro de exceção detalhes amarelo tela de morte,
- A página de erro de tempo de execução erro amarelo tela de morte, ou
- Uma página de erro personalizada

Os desenvolvedores de página de erro são mais familiarizado com é a YSOD de detalhes da exceção. Por padrão, essa página é exibida aos usuários que estão visitando localmente e, portanto, é a página que você vê quando ocorre um erro quando o site de teste no ambiente de desenvolvimento. Como o nome sugere, o YSOD de detalhes da exceção fornece detalhes sobre a exceção - o tipo, a mensagem e o rastreamento de pilha. Além disso, se a exceção foi gerada pelo código na classe de code-behind da página do ASP.NET e se o aplicativo está configurado para depuração, o YSOD de detalhes da exceção também mostrará esta linha de código (e algumas linhas de código acima e abaixo).

**Figura 1** mostra a página YSOD de detalhes da exceção. Anote a URL na janela de endereço do navegador: `http://localhost:62275/Genre.aspx?ID=foo`. Lembre-se de que o `Genre.aspx` página lista as revisões de catálogo em um gênero específico. Isso requer que `GenreId` valor (uma `uniqueidentifier`) ser passado querystring; por exemplo, a URL apropriada para exibir as revisões de ficção é `Genre.aspx?ID=7683ab5d-4589-4f03-a139-1c26044d0146`. Se não`uniqueidentifier` valor é passado na querystring (por exemplo, "foo") por meio de uma exceção será lançada.

> [!NOTE]
> Para reproduzir esse erro no aplicativo da web de demonstração disponível para download é possível ou visite `Genre.aspx?ID=foo` diretamente ou clique no link "Gerar um erro de tempo de execução" no `Default.aspx`.


Observe as informações de exceção apresentadas **Figura 1**. A mensagem de exceção "Falha na conversão ao converter de uma cadeia de caracteres para uniqueidentifier" está presente na parte superior da página. O tipo da exceção, `System.Data.SqlClient.SqlException`, estiver listado, também. Também é o rastreamento de pilha.

[![](displaying-a-custom-error-page-cs/_static/image2.png)](displaying-a-custom-error-page-cs/_static/image1.png)

**Figura 1**: os detalhes da exceção YSOD inclui informações sobre a exceção  
 ([Clique para exibir a imagem em tamanho normal](displaying-a-custom-error-page-cs/_static/image3.png))

O outro tipo de YSOD é YSOD de erro de tempo de execução e é mostrado na **Figura 2**. O tempo de execução erro YSOD informa o visitante que ocorreu um erro de tempo de execução, mas ele não inclui todas as informações sobre a exceção foi lançada. (No entanto, ele, fornecem instruções sobre como fazer os detalhes do erro podem ser exibidos, modificando o `Web.config` arquivo, que faz parte do que o torna tal YSOD uma aparência profissional.)

Por padrão, o YSOD de erro de tempo de execução é exibido aos usuários visitar remotamente (por meio de http://www.yoursite.com), conforme evidenciado pela URL na barra de endereços do navegador do **Figura 2**: `http://httpruntime.web703.discountasp.net/Genre.aspx?ID=foo`. As duas telas diferentes YSOD existem porque os desenvolvedores estão interessados em saber os detalhes do erro, mas essas informações não devem ser mostradas em um site em tempo real como ele pode revelar possíveis vulnerabilidades de segurança ou outras informações confidenciais para qualquer pessoa que acessa sua site.

> [!NOTE]
> Se você estiver acompanhando e estiver usando DiscountASP.NET como host da web, você pode perceber que o YSOD de erro de tempo de execução não exibirá ao visitar o site. Isso ocorre porque DiscountASP.NET tem seus servidores configurados para mostrar o YSOD detalhes de exceção por padrão. A boa notícia é que você pode substituir esse comportamento padrão, adicionando um `<customErrors>` seção ao seu `Web.config` arquivo. A seção "Configurando qual erro página é exibido" examina o `<customErrors>` seção em detalhes.


[![](displaying-a-custom-error-page-cs/_static/image5.png)](displaying-a-custom-error-page-cs/_static/image4.png)

**Figura 2**: O erro de tempo de execução YSOD não inclui quaisquer detalhes de erro  
 ([Clique para exibir a imagem em tamanho normal](displaying-a-custom-error-page-cs/_static/image6.png))

O terceiro tipo de página de erro é a página de erro personalizada, que é uma página da web que você criar. O benefício de uma página de erro personalizada é que você tenha controle total sobre as informações que são exibidas para o usuário junto com aparência a página; a página de erro personalizada pode usar a mesma página mestra e estilos como as outras páginas. A seção "Usando uma página de erro personalizada" orienta a criação de uma página de erro personalizada e configurá-lo para exibir no caso de uma exceção sem tratamento. **Figura 3** oferece uma prévia desta página de erro personalizada. Como você pode ver, a aparência da página de erro é muito mais profissionais que ambos os amarelo telas de morte mostrado nas figuras 1 e 2.

[![](displaying-a-custom-error-page-cs/_static/image8.png)](displaying-a-custom-error-page-cs/_static/image7.png)

**Figura 3**: uma página de erro personalizado oferece uma aparência mais personalizada  
 ([Clique para exibir a imagem em tamanho normal](displaying-a-custom-error-page-cs/_static/image9.png))

Reserve um tempo para inspecionar a barra de endereços do navegador do **Figura 3**. Observe que a barra de endereço mostra a URL da página de erro personalizada (`/ErrorPages/Oops.aspx`). Em números de 1 e 2 a telas de morte amarelo é mostrada na mesma página que o erro foi originado (`Genre.aspx`). A página de erro personalizada é passada a URL da página onde ocorreu o erro por meio de `aspxerrorpath` parâmetro querystring.

## <a name="configuring-which-error-page-is-displayed"></a>Configurar a página de erro que é exibido

Qual das três páginas de erro é exibida se baseia em duas variáveis:

- As informações de configuração de `<customErrors>` seção, e
- Se o usuário está visitando o site local ou remotamente.

O [ `<customErrors>` seção](https://msdn.microsoft.com/library/h0hfz6fc.aspx) na `Web.config` tem dois atributos que afetam a página de erro que é mostrada: `defaultRedirect` e `mode`. O atributo `defaultRedirect` é opcional. Se fornecido, ele especifica a URL da página de erro personalizada e indica que a página de erro personalizada deve ser mostrada em vez de YSOD de erro de tempo de execução. O `mode` atributo é necessário e aceita um dos três valores: `On`, `Off`, ou `RemoteOnly`. Esses valores tem o seguinte comportamento:

- `On`-indica que a página de erro personalizada ou YSOD de erro de tempo de execução é exibido para todos os visitantes, independentemente de estarem locais ou remotos.
- `Off`-Especifica que o YSOD de detalhes de exceção é exibida para todos os visitantes, independentemente de estarem locais ou remotos.
- `RemoteOnly`-indica que a página de erro personalizada ou YSOD de erro de tempo de execução é mostrado aos visitantes remotos, e o YSOD de detalhes da exceção para os visitantes do locais.

A menos que você especifique o contrário, o ASP.NET age como se você definiu o atributo de modo `RemoteOnly` e não tiver especificado um `defaultRedirect` valor. Em outras palavras, o comportamento padrão é que o YSOD de detalhes da exceção é exibida para os visitantes locais enquanto o YSOD de erro de tempo de execução é exibido aos visitantes remotos. Você pode substituir esse comportamento padrão, adicionando um `<customErrors>` seção do aplicativo web`Web.config file.`

## <a name="using-a-custom-error-page"></a>Usando uma página de erro personalizada

Cada aplicativo web deve ter uma página de erro personalizada. Ele fornece uma alternativa mais profissionais para o YSOD de erro de tempo de execução, é fácil criar e configurar o aplicativo para usar a página de erro personalizada leva apenas alguns minutos. A primeira etapa é criar a página de erro personalizada. Adicionei uma nova pasta para o aplicativo de revisões de livros denominado `ErrorPages` e adicionados para que uma nova página ASP.NET chamado `Oops.aspx`. Tem a página usar a mesma página mestra como o restante das páginas em seu site para que ela herda automaticamente a mesma aparência.

[![](displaying-a-custom-error-page-cs/_static/image11.png)](displaying-a-custom-error-page-cs/_static/image10.png)

**Figura 4**: criar uma página de erro personalizada

Em seguida, dedique alguns minutos, criação de conteúdo para a página de erro. Criei uma página de erro personalizada simples com uma mensagem indicando que houve um erro inesperado e um link de volta para a home page do site.

[![](displaying-a-custom-error-page-cs/_static/image13.png)](displaying-a-custom-error-page-cs/_static/image12.png)

**Figura 5**: criar a página de erro personalizada  
 ([Clique para exibir a imagem em tamanho normal](displaying-a-custom-error-page-cs/_static/image14.png))

Com a página de erro concluída, configure o aplicativo web para usar a página de erro personalizada em vez de YSOD de erro de tempo de execução. Isso é feito especificando a URL da página de erro no `<customErrors>` da seção `defaultRedirect` atributo. Adicione a seguinte marcação para o seu aplicativo `Web.config` arquivo:

[!code-xml[Main](displaying-a-custom-error-page-cs/samples/sample1.xml)]

A marcação acima configura o aplicativo para mostrar o YSOD de detalhes da exceção para os usuários que visitam localmente, enquanto estiver usando a página de erro personalizada Oops.aspx para os usuários que visitam remotamente. Para ver isso em ação, implante seu site para o ambiente de produção e, em seguida, visite a página de Genre.aspx no site ao vivo com um valor de querystring inválido. Você deve ver a página de erro personalizado (consulte **Figura 3**).

Para verificar a página de erro personalizada só é exibida aos usuários remotos, visite o `Genre.aspx` página com uma querystring inválida do ambiente de desenvolvimento. Você ainda verá o YSOD detalhes de exceção (consulte **Figura 1**). O `RemoteOnly` configuração garante que os usuários que visitam o site no ambiente de produção veem a página de erro personalizada enquanto os desenvolvedores trabalhando localmente continuarem a ver os detalhes da exceção.

## <a name="notifying-developers-and-logging-error-details"></a>Notificar os desenvolvedores e registro em log os detalhes do erro

Erros que ocorrem no ambiente de desenvolvimento foram causados pelo desenvolvedor colocada em seu computador. Ela é mostrada informações da exceção no YSOD de detalhes de exceção, e que conhece as etapas que ela estava executando quando ocorreu o erro. Mas, quando ocorre um erro em produção, o desenvolvedor não tem conhecimento que ocorreu um erro, a menos que o usuário final visitando o site leva tempo para relatar o erro. E, mesmo que o usuário sai do sua forma de alertar a equipe de desenvolvimento que ocorreu um erro, sem saber o tipo de exceção, mensagem e rastreamento de pilha podem ser difíceis de diagnosticar a causa do erro, sem falar corrigi-lo.

Por esses motivos, é fundamental que qualquer erro no ambiente de produção é registrado para um repositório permanente (como um banco de dados) e que os desenvolvedores são alertados sobre este erro. A página de erro personalizada pode parecer um bom lugar para fazer esse registro e a notificação. Infelizmente, a página de erro personalizado não tem acesso para os detalhes do erro e, portanto, não pode ser usada para registrar essas informações. A boa notícia é que há várias maneiras de interceptar os detalhes do erro e para acessá-los, e os próximos três tutoriais explorar esse tópico em mais detalhes.

## <a name="using-different-custom-error-pages-for-different-http-error-statuses"></a>Usando as páginas de erro personalizado diferente para o status de erro diferente de HTTP

Quando uma exceção é lançada por uma página ASP.NET e não é tratada, a exceção percolates até o tempo de execução do ASP.NET, que exibe a página de erro configurado. Se uma solicitação é o mecanismo do ASP.NET, mas não pode ser processada por algum motivo, talvez o arquivo solicitado não foi encontrado ou ler permissões foram desabilitadas para o arquivo - em seguida, o mecanismo do ASP.NET gera uma `HttpException`. Essa exceção, como as exceções geradas em páginas do ASP.NET, bolhas até o tempo de execução, fazendo com que a página de erro apropriado a ser exibido.

Para o aplicativo web em produção, isso significa é que, se um usuário solicita uma página que não é encontrada, eles verão a página de erro personalizada. **Figura 6** mostra um exemplo. Porque a solicitação é para uma página inexistente (`NoSuchPage.aspx`), uma `HttpException` é gerada e é exibida a página de erro personalizada (Observe a referência ao `NoSuchPage.aspx` no `aspxerrorpath` parâmetro querystring).

[![](displaying-a-custom-error-page-cs/_static/image16.png)](displaying-a-custom-error-page-cs/_static/image15.png)

**Figura 6**: O tempo de execução do ASP.NET exibe o configurado erro na resposta de página a uma solicitação inválida ([clique para exibir a imagem em tamanho normal](displaying-a-custom-error-page-cs/_static/image17.png))

Por padrão, todos os tipos de erros de fazem a mesma página de erro personalizada a ser exibida. No entanto, você pode especificar uma página de erro personalizado diferente para um determinado HTTP status código usando `<error>` elementos filhos dentro de `<customErrors>` seção. Por exemplo, para que uma página de erro diferentes exibida no caso de uma página não encontrada o erro, que tem um código de status HTTP de 404, atualize o `<customErrors>` seção para incluir a seguinte marcação:

[!code-xml[Main](displaying-a-custom-error-page-cs/samples/sample2.xml)]

Com essa alteração no local, sempre que um usuário visitar remotamente solicita um recurso do ASP.NET não existe, ele será redirecionado para a `404.aspx` página de erro personalizada em vez de `Oops.aspx`. Como **Figura 7** ilustra, o `404.aspx` página pode incluir uma mensagem mais específica que a página de erro personalizada geral.

> [!NOTE]
> Check-out [páginas de erro 404, uma vez mais](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/) para obter orientação sobre como criar páginas de erro 404 efetivo.


[![](displaying-a-custom-error-page-cs/_static/image19.png)](displaying-a-custom-error-page-cs/_static/image18.png)**Figura 7**: A página de erro 404 personalizada exibirá uma mensagem mais direcionada que`Oops.aspx`  
 ([Clique para exibir a imagem em tamanho normal](displaying-a-custom-error-page-cs/_static/image20.png)) 

Porque você sabe que o `404.aspx` página apenas é atingida quando o usuário faz uma solicitação para uma página que não foi encontrada, você pode aprimorar a esta página de erro personalizada para incluir funcionalidade para ajudar o usuário esse tipo de erro específico de endereço. Por exemplo, você poderia criar uma tabela de banco de dados que mapeia conhecida URLs incorretas para URLs BOM e, em seguida, o `404.aspx` página de erro personalizada executar uma consulta na tabela e sugere páginas que o usuário pode estar tentando acessar.

> [!NOTE]
> A página de erro personalizado é exibida somente quando é feita uma solicitação para um recurso tratado pelo mecanismo de ASP.NET. Conforme abordado no [principais diferenças entre o IIS e o ASP.NET Development Server](core-differences-between-iis-and-the-asp-net-development-server-cs.md) tutorial, o servidor web pode lidar com determinadas solicitações em si. Por padrão, o IIS web solicitações de processos do servidor de conteúdo estático, como imagens e arquivos HTML sem invocar o mecanismo do ASP.NET. Consequentemente, se o usuário solicita um arquivo de imagem existente não receberão novamente mensagem de erro 404 padrão do IIS, em vez de ASP. Página de erro configurado do NET.


## <a name="summary"></a>Resumo

Quando ocorre uma exceção sem tratamento em um aplicativo ASP.NET, o usuário é mostrado uma das três páginas de erro: a exceção detalhes amarelo tela de morte; o erro de tempo de execução amarelo tela de morte; ou uma página de erro personalizada. A página de erro é exibida depende do aplicativo `<customErrors>` configuração e se o usuário está visitando localmente ou remotamente. O comportamento padrão é mostrar a YSOD de detalhes de exceção para os visitantes do locais e o YSOD de erro de tempo de execução para os visitantes remotos.

Enquanto o YSOD de erro de tempo de execução oculta informações potencialmente confidenciais e erro do usuário visitar o site, ele quebras de aparência do seu site e faz com que seu aplicativo pareça com erros. Uma abordagem melhor é usar uma página de erro personalizada, que envolve a criação e criar a página de erro personalizada e especificando a URL no `<customErrors>` da seção `defaultRedirect` atributo. Você pode ter várias páginas de erro personalizadas para os diferentes status de erro HTTP.

A página de erro personalizada é a primeira etapa em uma estratégia para um site de produção de tratamento de erros abrangente. O desenvolvedor do erro de alerta e registro em log os detalhes também são etapas importantes. Os próximos três tutoriais exploram técnicas para notificação de erro e registro em log.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Páginas de erro, mais uma vez](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/)
- [Diretrizes de design para exceções](https://msdn.microsoft.com/library/ms229014.aspx)
- [Páginas de erro amigável](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [Manipulando e lançando exceções](https://msdn.microsoft.com/library/5b2yeyab.aspx)
- [Corretamente usando páginas de erro personalizadas no ASP.NET](http://professionalaspnet.com/archive/2007/09/30/Properly-Using-Custom-Error-Pages-in-ASP.NET.aspx)

>[!div class="step-by-step"]
[Anterior](strategies-for-database-development-and-deployment-cs.md)
[Próximo](processing-unhandled-exceptions-cs.md)
