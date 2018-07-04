---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-vb
title: Exibindo uma página de erro personalizada (VB) | Microsoft Docs
author: rick-anderson
description: O que o usuário vê quando ocorre um erro de tempo de execução em um aplicativo web ASP.NET? A resposta depende de como o site &lt;customErrors&gt; configuração...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: 14873c5d-81a9-455b-bd71-30fb555583e7
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 732aad689b5d427ceb50b0062e7c00de0b7fb401
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362610"
---
<a name="displaying-a-custom-error-page-vb"></a>Exibindo uma página de erro personalizada (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_11_VB.zip) ou [baixar PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial11_CustomErrors_vb.pdf)

> O que o usuário vê quando ocorre um erro de tempo de execução em um aplicativo web ASP.NET? A resposta depende de como o site &lt;customErrors&gt; configuração. Por padrão, os usuários recebem uma tela amarela emaranhada exaltam que ocorreu um erro de tempo de execução. Este tutorial mostra como personalizar essas configurações para a página de erro personalizada de um estética-forma de exibição que corresponde à aparência do seu site.


## <a name="introduction"></a>Introdução

Em um mundo perfeito não haveria nenhum erro de tempo de execução. Programadores deve escrever o código ao fim sem nenhum um bug e com validação de entrada do usuário eficientes e externo recursos como servidores de banco de dados e servidores de email nunca deve ir offline. É claro que, na realidade, erros são inevitáveis. As classes no .NET Framework sinalizam um erro lançando uma exceção. Por exemplo, o método Open do objeto chamando um SqlConnection estabelece uma conexão ao banco de dados especificado por uma cadeia de caracteres de conexão. No entanto, se o banco de dados está inativo ou se as credenciais na cadeia de conexão são inválidas, em seguida, o método Open lança um `SqlException`. Exceções podem ser manipuladas pelo uso de `Try/Catch/Finally` blocos. Se código dentro de um `Try` bloco lançar uma exceção, o controle é transferido para o bloco catch adequado em que o desenvolvedor pode tentar se recuperar do erro. Se não houver nenhum bloco catch correspondente ou se o código que lançou a exceção não estiver em um bloco try, a exceção percolates a pilha de chamadas no search de `Try/Catch/Finally` blocos.

Se a exceção propaga-se até o tempo de execução do ASP.NET sem que está sendo manipulado, o [ `HttpApplication` classe](https://msdn.microsoft.com/library/system.web.httpapplication.aspx)do [ `Error` evento](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx) é gerado e configurado *página de erro*  é exibida. Por padrão, o ASP.NET exibe uma página de erro é carinhosamente conhecida como o [tela amarela da morte](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow) (YSOD). Há duas versões do YSOD: um mostra os detalhes da exceção, um rastreamento de pilha e outras informações úteis para desenvolvedores de depuração do aplicativo (consulte **Figura 1**); a outra simplesmente informa que houve um erro de tempo de execução (veja  **Figura 2**).

Os detalhes da exceção YSOD é muito útil para desenvolvedores de depuração do aplicativo, mas mostrar um YSOD aos usuários finais é pegajosas e não profissional etc. Em vez disso, os usuários finais devem ser tomados para uma página de erro que mantém a aparência do site com mais amigável prosa descrevendo a situação. A boa notícia é que a criação de uma página de erro personalizado como essa é muito fácil. Este tutorial começa com uma olhada no ASP. Páginas de erro diferente da rede. Ele, em seguida, mostra como configurar o aplicativo web para mostrar aos usuários uma página de erro personalizada diante de um erro.

### <a name="examining-the-three-types-of-error-pages"></a>Examinando os três tipos de páginas de erro

Quando uma exceção sem tratamento ocorre em um aplicativo ASP.NET, um dos três tipos de páginas de erro é exibida:

- A página de erro de exceção detalhes amarelo tela da morte,
- A página de erro de tempo de execução erro amarelo tela da morte, ou
- Uma página de erro personalizada

Os desenvolvedores de página de erro são mais familiarizados com é o YSOD de detalhes da exceção. Por padrão, essa página será exibida para os usuários que visitam localmente e, portanto, é a página que você vê quando ocorre um erro ao testar o site no ambiente de desenvolvimento. Como o nome sugere, o YSOD de detalhes de exceção fornece detalhes sobre a exceção – o tipo, a mensagem e o rastreamento de pilha. Além disso, se a exceção foi gerada pelo código na classe de code-behind da página do ASP.NET e se o aplicativo está configurado para depuração, em seguida, a exceção detalhes YSOD também mostrará esta linha de código (e algumas linhas de código acima e abaixo dele).

**Figura 1** mostra a página YSOD de detalhes da exceção. Anote a URL na janela de endereço do navegador: `http://localhost:62275/Genre.aspx?ID=foo`. Lembre-se de que o `Genre.aspx` página lista as resenhas de livros em um gênero específico. Ele requer que `GenreId` valor (uma `uniqueidentifier`) ser passado a querystring; por exemplo, é a URL apropriada para exibir as revisões de ficção `Genre.aspx?ID=7683ab5d-4589-4f03-a139-1c26044d0146`. Se um não -`uniqueidentifier` valor é passado por meio da cadeia de consulta (por exemplo, "foo") uma exceção é gerada.

> [!NOTE]
> Para reproduzir este erro no aplicativo web de demonstração disponível para download você pode visitar `Genre.aspx?ID=foo` diretamente ou clique no link "Gerar um erro de tempo de execução" `Default.aspx`.


Observe as informações de exceção apresentadas **Figura 1**. A mensagem de exceção "Falha na conversão ao converter de uma cadeia de caracteres para uniqueidentifier" está presente na parte superior da página. O tipo da exceção, `System.Data.SqlClient.SqlException`, estiver listado, também. Há também o rastreamento de pilha.

[![](displaying-a-custom-error-page-vb/_static/image2.png)](displaying-a-custom-error-page-vb/_static/image1.png)

**Figura 1**: os detalhes da exceção YSOD inclui informações sobre a exceção  
 ([Clique para exibir a imagem em tamanho normal](displaying-a-custom-error-page-vb/_static/image3.png))

O outro tipo de YSOD é o YSOD de erro de tempo de execução e é mostrado na **Figura 2**. O tempo de execução erro YSOD informa o visitante que ocorreu um erro de tempo de execução, mas ele não inclui todas as informações sobre a exceção que foi gerada. (No entanto, ele, fornecem instruções sobre como fazer os detalhes do erro visível, modificando o `Web.config` arquivo, que é parte daquilo que torna esse um YSOD com aparência profissional.)

Por padrão, o YSOD de erro de tempo de execução é mostrado aos usuários visitar remotamente (por meio http://www.yoursite.com), conforme evidenciado pela URL na barra de endereços do navegador do **Figura2**: `http://httpruntime.web703.discountasp.net/Genre.aspx?ID=foo`. As duas telas YSOD diferentes existem porque os desenvolvedores estão interessados em saber os detalhes do erro, mas tais informações não devem ser exibidas em um site ao vivo como ele pode revelar possíveis vulnerabilidades de segurança ou outras informações confidenciais para qualquer pessoa que visitar seu site.

> [!NOTE]
> Se você estiver acompanhando e estiver usando DiscountASP.NET como host da web, você pode perceber que o YSOD de erro de tempo de execução não exibirá ao visitar o site ao vivo. Isso ocorre porque DiscountASP.NET tem seus servidores configurados para mostrar o YSOD de detalhes de exceção por padrão. A boa notícia é que você pode substituir esse comportamento padrão, adicionando um `<customErrors>` seção ao seu `Web.config` arquivo. A seção "Configurando qual erro página é exibido" examina o `<customErrors>` seção em detalhes.


[![](displaying-a-custom-error-page-vb/_static/image5.png)](displaying-a-custom-error-page-vb/_static/image4.png)

**Figura 2**: O erro de tempo de execução YSOD não inclui quaisquer detalhes de erro  
 ([Clique para exibir a imagem em tamanho normal](displaying-a-custom-error-page-vb/_static/image6.png))

O terceiro tipo de página de erro é a página de erro personalizada, que é uma página da web que você criar. O benefício de uma página de erro personalizada é que você tenha controle total sobre as informações que são exibidas ao usuário, juntamente com a aparência e funcionamento da página; a página de erro personalizada pode usar os estilos e a mesma página mestra como suas outras páginas. A seção "Usando uma página de erro personalizada" orienta a criação de uma página de erro personalizado e configurá-lo para exibir no caso de uma exceção sem tratamento. **Figura 3** oferece um prévia pico desta página de erro personalizada. Como você pode ver, a aparência da página de erro é muito mais com aparência profissional de qualquer uma do amarelo telas da morte mostrado nas figuras 1 e 2.

[![](displaying-a-custom-error-page-vb/_static/image8.png)](displaying-a-custom-error-page-vb/_static/image7.png)

**Figura 3**: uma página de erro personalizado oferece uma aparência mais personalizada  
 ([Clique para exibir a imagem em tamanho normal](displaying-a-custom-error-page-vb/_static/image9.png))

Reserve um tempo para inspecionar a barra de endereços do navegador do **Figura 3**. Observe que a barra de endereços mostra a URL da página de erro personalizada (`/ErrorPages/Oops.aspx`). Nas figuras 1 e 2 a telas de morte amarelo é mostrada na mesma página que o erro foi originado (`Genre.aspx`). A página de erro personalizada é passada a URL da página onde ocorreu o erro por meio de `aspxerrorpath` parâmetro querystring.

## <a name="configuring-which-error-page-is-displayed"></a>Configurar a página de erro que é exibida.

Qual das três páginas de erro possíveis é exibido se baseia em duas variáveis:

- As informações de configuração no `<customErrors>` seção, e
- Se o usuário está visitando o site local ou remotamente.

O [ `<customErrors>` seção](https://msdn.microsoft.com/library/h0hfz6fc.aspx) na `Web.config` tem dois atributos que afetam a qual página de erro é mostrada: `defaultRedirect` e `mode`. O atributo `defaultRedirect` é opcional. Se fornecido, ele especifica a URL da página de erro personalizada e indica que a página de erro personalizadas deve ser mostrada, em vez do YSOD de erro de tempo de execução. O `mode` atributo é obrigatório e aceita um dos três valores: `On`, `Off`, ou `RemoteOnly`. Esses valores têm o seguinte comportamento:

- `On` -indica que a página de erro personalizada ou YSOD de erro de tempo de execução é exibido para todos os visitantes, independentemente de estarem locais ou remotos.
- `Off` -Especifica que o YSOD de detalhes de exceção é exibida para todos os visitantes, independentemente de estarem locais ou remotos.
- `RemoteOnly` -indica que a página de erro personalizada ou YSOD de erro de tempo de execução é exibido aos visitantes remotos, enquanto o YSOD de detalhes de exceção é mostrado aos visitantes do locais.

A menos que você especifique de outra forma, o ASP.NET age como se você tiver definido o atributo mode como `RemoteOnly` e não tivesse especificado uma `defaultRedirect` valor. Em outras palavras, o comportamento padrão é que o YSOD de detalhes de exceção é exibida aos visitantes locais enquanto o YSOD de erro de tempo de execução é exibido aos visitantes remotos. Você pode substituir esse comportamento padrão, adicionando um `<customErrors>` seção ao seu aplicativo de web `Web.config file.`

## <a name="using-a-custom-error-page"></a>Usando uma página de erro personalizada

Cada aplicativo web deve ter uma página de erro personalizada. Ele fornece uma alternativa mais com aparência profissional para o YSOD de erro de tempo de execução, é fácil criar e configurar o aplicativo para usar a página de erro personalizada leva apenas alguns minutos. A primeira etapa é criar a página de erro personalizada. Eu adicionei uma nova pasta para o aplicativo de resenhas de livros denominado `ErrorPages` e adicionado para que uma nova página ASP.NET chamado `Oops.aspx`. Tem a página usar a mesma página mestra como o restante das páginas do seu site para que ela herda automaticamente a mesma aparência.

[![](displaying-a-custom-error-page-vb/_static/image11.png)](displaying-a-custom-error-page-vb/_static/image10.png)

**Figura 4**: criar uma página de erro personalizada

Em seguida, reserve alguns minutos, criando o conteúdo para a página de erro. Eu criei uma página de erro personalizada simples com uma mensagem indicando que houve um erro inesperado e um link de volta para a home page do site.

[![](displaying-a-custom-error-page-vb/_static/image13.png)](displaying-a-custom-error-page-vb/_static/image12.png)

**Figura 5**: projeta sua página de erro personalizada  
 ([Clique para exibir a imagem em tamanho normal](displaying-a-custom-error-page-vb/_static/image14.png))

Com a página de erro concluída, configure o aplicativo web para usar a página de erro personalizada no lugar de YSOD de erro de tempo de execução. Isso é feito especificando a URL da página de erro no `<customErrors>` da seção `defaultRedirect` atributo. Adicione a seguinte marcação ao seu aplicativo `Web.config` arquivo:

[!code-xml[Main](displaying-a-custom-error-page-vb/samples/sample1.xml)]

A marcação acima configura o aplicativo para mostrar o YSOD de detalhes da exceção para usuários que visitam localmente, enquanto estiver usando a página de erro personalizada Oops.aspx para esses usuários visitar remotamente. Para ver isso em ação, implante seu site ao ambiente de produção e, em seguida, visite a página de Genre.aspx no site ativo com um valor de cadeia de consulta inválido. Você deve ver a página de erro personalizada (consulte volta **Figura 3**).

Para verificar que a página de erro personalizada só é exibida a usuários remotos, visite o `Genre.aspx` página com uma querystring inválida do ambiente de desenvolvimento. Você ainda verá o YSOD de detalhes de exceção (consultar novamente **Figura 1**). O `RemoteOnly` configuração garante que os usuários que visitam o site no ambiente de produção veem a página de erro personalizado, enquanto os desenvolvedores que trabalham localmente continuam a ver os detalhes da exceção.

## <a name="notifying-developers-and-logging-error-details"></a>Notificar os desenvolvedores e registro em log os detalhes do erro

Erros que ocorrem no ambiente de desenvolvimento foram causados pelo desenvolvedor sentado em seu computador. Ela é mostrada informações da exceção no YSOD de detalhes de exceção, e ela sabe as etapas que ela estava executando quando ocorreu o erro. Mas, quando ocorre um erro em produção, o desenvolvedor não tem conhecimento que ocorreu um erro, a menos que o usuário final visitando o site leva o tempo para relatar o erro. E mesmo que o usuário sai do seu caminho para alertar a equipe de desenvolvimento que ocorreu um erro, sem saber o tipo de exceção, a mensagem e o rastreamento de pilha que pode ser difícil de diagnosticar a causa do erro, sem falar corrigi-lo.

Por esses motivos, é imprescindível que qualquer erro no ambiente de produção está conectado a um repositório permanente (por exemplo, um banco de dados) e que os desenvolvedores são alertados desse erro. A página de erro personalizada pode parecer um bom lugar para fazer esse registro em log e notificação. Infelizmente, a página de erro personalizado não tem acesso para os detalhes do erro e, portanto, não pode ser usada para registrar essas informações. A boa notícia é que há várias maneiras de interceptar os detalhes do erro e registrá-los, e os próximos três tutoriais exploram esse tópico em mais detalhes.

## <a name="using-different-custom-error-pages-for-different-http-error-statuses"></a>Usando páginas de erro personalizadas diferentes para o status de erro diferente de HTTP

Quando uma exceção é gerada por uma página ASP.NET e não for tratada, a exceção percolates até o tempo de execução do ASP.NET, que exibe a página de erro configurado. Se uma solicitação é fornecido para o mecanismo do ASP.NET, mas não pode ser processada por algum motivo, talvez o arquivo solicitado não for encontrado ou ler permissões foram desabilitadas para o arquivo - em seguida, o mecanismo do ASP.NET gerará um `HttpException`. Essa exceção, como as exceções geradas das páginas ASP.NET, propaga-se até o tempo de execução, fazendo com que a página de erro apropriado a ser exibido.

Para o aplicativo web em produção, isso significa é que, se um usuário solicita uma página que não for encontrada, em seguida, eles verão a página de erro personalizada. **Figura 6** mostra um exemplo. Porque a solicitação é para uma página inexistente (`NoSuchPage.aspx`), uma `HttpException` será lançada e a página de erro personalizada é exibida (Observe a referência ao `NoSuchPage.aspx` no `aspxerrorpath` parâmetro querystring).

[![](displaying-a-custom-error-page-vb/_static/image16.png)](displaying-a-custom-error-page-vb/_static/image15.png)**Figura 6**: O tempo de execução do ASP.NET exibe a página de erro configurado em resposta a uma solicitação inválida  
 ([Clique para exibir a imagem em tamanho normal](displaying-a-custom-error-page-vb/_static/image17.png)) 

Por padrão, todos os tipos de erros fazem com que a mesma página de erro personalizada a ser exibido. No entanto, você pode especificar uma página de erro personalizado diferente para um específico HTTP status de código usando `<error>` elementos filhos dentro do `<customErrors>` seção. Por exemplo, para ter uma página de erro diferentes exibida em caso de uma página não encontrada o erro, que tem um código de status HTTP de 404, atualize o `<customErrors>` seção para incluir a seguinte marcação:

[!code-xml[Main](displaying-a-custom-error-page-vb/samples/sample2.xml)]

Com essa alteração em vigor, sempre que um usuário visitar remotamente solicita um recurso do ASP.NET que não existe, ele será redirecionado para o `404.aspx` página de erro personalizada em vez de `Oops.aspx`. Como **Figura7** ilustra, o `404.aspx` página pode incluir uma mensagem mais específica que a página de erro personalizado geral.

> [!NOTE]
> Fazer check-out [páginas de erro 404, uma vez mais](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/) para obter orientação sobre como criar páginas efetiva de erro 404.


[![](displaying-a-custom-error-page-vb/_static/image19.png)](displaying-a-custom-error-page-vb/_static/image18.png)**Figura 7**: A página de erro 404 personalizada exibe uma mensagem mais direcionada que `Oops.aspx`  
 ([Clique para exibir a imagem em tamanho normal](displaying-a-custom-error-page-vb/_static/image20.png)) 

Porque você sabe que o `404.aspx` página só é atingida quando o usuário faz uma solicitação para uma página que não foi encontrada, você pode aprimorar a esta página de erro personalizada para incluir a funcionalidade para ajudar o usuário esse tipo de erro específico de endereços. Por exemplo, você poderia criar uma tabela de banco de dados que mapeia conhecida URLs ruins a BOM URLs e, em seguida, ter o `404.aspx` página de erro personalizada executar uma consulta em relação a que tabela e sugerir páginas que o usuário pode estar tentando acessar.

> [!NOTE]
> A página de erro personalizada é exibida somente quando uma solicitação é feita a um recurso tratado pelo mecanismo de ASP.NET. Como discutimos na [principais diferenças entre o IIS e o ASP.NET Development Server](core-differences-between-iis-and-the-asp-net-development-server-vb.md) tutorial, o servidor web pode manipular determinadas solicitações em si. Por padrão, o IIS web solicitações de processos do servidor de conteúdo estático como imagens e arquivos HTML sem invocar o mecanismo do ASP.NET. Consequentemente, se o usuário solicita um arquivo de imagem inexistente eles obterá mensagem de erro 404 padrão do IIS em vez de ASP. Página de erro configurada da rede.


## <a name="summary"></a>Resumo

Quando ocorre uma exceção sem tratamento em um aplicativo ASP.NET, o usuário verá uma das três páginas de erro: a exceção detalhes amarelo tela da morte; o erro de tempo de execução amarelo tela; ou uma página de erro personalizada. A página de erro é exibida depende do aplicativo `<customErrors>` configuração e se o usuário está visitando localmente ou remotamente. O comportamento padrão é mostrar a YSOD de detalhes de exceção para os visitantes do locais e o YSOD de erro de tempo de execução aos visitantes remotos.

Enquanto o YSOD de erro de tempo de execução oculta informações potencialmente confidenciais de erro do usuário visitar o site, ele interrompe da aparência do seu site e faz com que seu aplicativo pareça cheio de bugs. Uma abordagem melhor é usar uma página de erro personalizada, que envolve criar e projetar a página de erro personalizada e especificando a URL na `<customErrors>` da seção `defaultRedirect` atributo. Você pode até ter várias páginas de erro personalizadas para diferentes status de erro HTTP.

A página de erro personalizada é a primeira etapa em uma estratégia para um site de produção de tratamento de erros abrangente. Alertando o desenvolvedor do erro e seus detalhes de log também são etapas importantes. Os próximos três tutoriais exploram técnicas para notificação de erro e registro em log.

Boa programação!

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Páginas de erro, mais uma vez](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/)
- [Diretrizes de design para exceções](https://msdn.microsoft.com/library/ms229014.aspx)
- [Páginas de erro amigável](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [Tratando e gerando exceções](https://msdn.microsoft.com/library/5b2yeyab.aspx)
- [Corretamente usando páginas de erro personalizada no ASP.NET](http://professionalaspnet.com/archive/2007/09/30/Properly-Using-Custom-Error-Pages-in-ASP.NET.aspx)

> [!div class="step-by-step"]
> [Anterior](strategies-for-database-development-and-deployment-vb.md)
> [Próximo](processing-unhandled-exceptions-vb.md)
