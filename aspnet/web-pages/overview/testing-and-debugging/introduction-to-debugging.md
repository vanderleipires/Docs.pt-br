---
uid: web-pages/overview/testing-and-debugging/introduction-to-debugging
title: Introdução à depuração da Web do ASP.NET (Razor) Sites de páginas | Microsoft Docs
author: tfitzmac
description: Depuração é o processo de localizar e corrigir erros em suas páginas de código. Este capítulo mostra algumas ferramentas e técnicas que você pode usar para depurar e analisar...
ms.author: aspnetcontent
ms.date: 02/20/2014
ms.assetid: 68de4326-7611-4b9b-b5f6-79b7adc3069f
msc.legacyurl: /web-pages/overview/testing-and-debugging/introduction-to-debugging
msc.type: authoredcontent
ms.openlocfilehash: b8a492b065902fa10d3e4c5cccd50e63ea356709
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823797"
---
<a name="introduction-to-debugging-aspnet-web-pages-razor-sites"></a>Introdução à depuração da Web do ASP.NET (Razor) Sites de páginas
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo explica as várias maneiras de depurar páginas em um site de páginas da Web do ASP.NET (Razor). Depuração é o processo de localizar e corrigir erros em suas páginas de código.
> 
> **O que você aprenderá:** 
> 
> - Como exibir informações que ajudam a analisar e depurar páginas.
> - Como usar a depuração de ferramentas no Visual Studio.
>   
> 
> Estes são os recursos do ASP.NET introduzidos no artigo:
> 
> - O `ServerInfo` auxiliar.
> - `ObjectInfo` auxiliar.
>   
> 
> ## <a name="software-versions"></a>Versões de software
> 
> 
> - Páginas da Web do ASP.NET (Razor) 3
> - Visual Studio 2013
>   
> 
> Este tutorial também funciona com ASP.NET Web Pages 2. Você pode usar o WebMatrix 3, mas não há suporte para o depurador integrado.


É um aspecto importante da solução de problemas de erros e problemas em seu código para evitá-los em primeiro lugar. Você pode fazer isso colocando seções do seu código que podem causar erros em `try/catch` blocos. Para obter mais informações, consulte a seção sobre como tratar erros em [Introdução ao ASP.NET Web de programação usando a sintaxe do Razor](https://go.microsoft.com/fwlink/?LinkId=202890).

O `ServerInfo` auxiliar é uma ferramenta de diagnóstico que lhe dá uma visão geral das informações sobre o ambiente de servidor web que hospeda sua página. Ele também mostra informações de solicitação HTTP são enviadas quando um navegador solicita a página. O `ServerInfo` auxiliar exibe a identidade do usuário atual, o tipo de navegador que fez a solicitação, e assim por diante. Esse tipo de informação pode ajudar você a solucionar problemas comuns.

1. Criar uma nova página da web denominado *ServerInfo.cshtml*.
2. No final da página, logo antes do fechamento `</body>` marca, adicione `@ServerInfo.GetHtml()`:

    [!code-cshtml[Main](introduction-to-debugging/samples/sample1.cshtml)]

    Você pode adicionar o `ServerInfo` código em qualquer lugar na página. Mas, adicioná-lo no final será manter sua saída separados de seus outros conteúdos da página, que torna mais fácil de ler.

    > [!NOTE] 
    > 
    > **Importante** você deverá remover qualquer código de diagnóstico de suas páginas da web antes de mover as páginas da web para um servidor de produção. Isso se aplica ao `ServerInfo` auxiliar, bem como as outras técnicas de diagnóstico neste artigo que envolvem a adição de código para uma página. Você não deseja os visitantes do site para obter informações sobre seu nome do servidor, os nomes de usuário, os caminhos no seu servidor e detalhes semelhantes, pois esse tipo de informação pode ser útil para pessoas com más intenções.
3. Salve a página e executá-lo em um navegador.

    ![Depuração de-1](introduction-to-debugging/_static/image1.jpg)

    O `ServerInfo` auxiliar exibe quatro tabelas de informações na página:

   - Configuração do servidor. Esta seção fornece informações sobre o servidor de hospedagem da web, incluindo o nome do computador, a versão do ASP.NET que você está executando, o nome de domínio e o tempo de servidor.
   - Variáveis de servidor ASP.NET. Esta seção fornece detalhes sobre os diversos detalhes de protocolo HTTP (chamados variáveis HTTP) e valores que fazem parte de cada solicitação de página da web.
   - Informações de tempo de execução de HTTP. Esta seção fornece detalhes sobre o que a versão do Microsoft .NET Framework que sua página da web está em execução no, o caminho, detalhes sobre o cache e assim por diante. (Como você aprendeu na [Introdução ao ASP.NET Web de programação usando a sintaxe do Razor](https://go.microsoft.com/fwlink/?LinkId=202890), páginas Web ASP.NET usando o Razor sintaxe baseiam-se na tecnologia ASP.NET da Microsoft web server, que também é desenvolvido em um amplo biblioteca de desenvolvimento chamado .NET Framework).
   - Variáveis de ambiente. Esta seção fornece uma lista de todas as variáveis de ambiente local e seus valores no servidor web.

     Uma descrição completa de todas as informações de servidor e a solicitação está além do escopo deste artigo, mas você pode ver que o `ServerInfo` auxiliar retorna várias informações de diagnóstico. Para obter mais informações sobre os valores que `ServerInfo` retorna, consulte [variáveis de ambiente reconhecidas](https://technet.microsoft.com/library/dd560744(WS.10).aspx) no site da Microsoft TechNet e [variáveis de servidor IIS](https://msdn.microsoft.com/library/ms524602(VS.90).aspx) no site do MSDN.

## <a name="embedding-output-expressions-to-display-page-values"></a>Inserir expressões de saída para exibir valores de página

Outra maneira de ver o que está acontecendo em seu código é inserir expressões de saída na página. Como você sabe, você pode exibir diretamente o valor de uma variável com a adição de algo como `@myVariable` ou `@(subTotal * 12)` para a página. Para depuração, você pode colocar essas expressões de saída em pontos estratégicos em seu código. Isso permite que você veja o valor de chave variáveis ou o resultado de cálculos, quando a página é executada. Quando você terminar de depuração, você pode remover as expressões ou comente-os. Este procedimento ilustra uma maneira comum de usar expressões inseridas para ajudar a depurar uma página.

1. Criar uma nova página do WebMatrix chamado *OutputExpression.cshtml*.
2. Substitua a página de conteúdo com o seguinte:

    [!code-html[Main](introduction-to-debugging/samples/sample2.html)]

    O exemplo usa uma `switch` instrução para verificar o valor da `weekday` variável e, em seguida, exibir uma mensagem de saída diferente, dependendo de qual dia da semana é. No exemplo, o `if` bloco entre o primeiro bloco de código altera arbitrariamente o dia da semana com a adição de um dia para o valor de dia da semana atual. Esse é um erro introduzido para fins de ilustração.
3. Salve a página e executá-lo em um navegador.

    A página exibe a mensagem para o errado dia da semana. Qualquer dia da semana, na verdade, é, você verá a mensagem para um dia depois. Embora, nesse caso, você sabe por que a mensagem está desativado (porque o código deliberadamente define o valor de dia incorreta), na realidade, muitas vezes é difícil saber onde as coisas estão indo erradas no código. Para depurar, você precisa descobrir o que está acontecendo com o valor de variáveis e objetos de chave, como `weekday`.
4. Adicionar expressões de saída inserindo `@weekday` conforme mostrado nos dois lugares indicados por comentários no código. Essas expressões de saída exibirá os valores da variável nesse ponto na execução do código.

    [!code-csharp[Main](introduction-to-debugging/samples/sample3.cs?highlight=2-3,15-16)]
5. Salve e execute a página em um navegador.

    A página exibe o dia da semana real pela primeira vez, em seguida, atualizado dia da semana que resulta da adição de um dia e, em seguida, a mensagem resultante do `switch` instrução. A saída de duas expressões variáveis (`@weekday`) não tem espaços entre os dias porque você não adicionou qualquer HTML `<p>` marcas para a saída; as expressões são apenas para teste.

    ![Depuração-2](introduction-to-debugging/_static/image2.jpg)

    Agora você pode ver onde está o erro. Quando você exibe primeiro a `weekday` variável no código, ele mostra o dia. Quando você exibi-lo na segunda vez, após o `if` blocos no código, o dia está desativado por um. Para que você saiba que algo aconteceu entre o primeiro e segundo o aparecimento da variável de dia da semana. Se esse fosse um bug real, esse tipo de abordagem seria ajudá-lo a restringir o local do código que está causando o problema.
6. Corrija o código na página removendo as expressões de saída de dois que você adicionou e, em seguida, remover o código que altera o dia da semana. O restante, concluído o bloco de código é semelhante ao exemplo a seguir:

    [!code-cshtml[Main](introduction-to-debugging/samples/sample4.cshtml)]
7. Execute a página em um navegador. Neste momento você ver a mensagem correta exibida para o real dia da semana.

## <a name="using-the-objectinfo-helper-to-display-object-values"></a>Usando o auxiliar de ObjectInfo para exibir valores de objeto

O `ObjectInfo` auxiliar exibe o tipo e o valor de cada objeto que você passa para ele. Você pode usá-lo para exibir o valor de variáveis e objetos em seu código (como fiz com expressões de saída no exemplo anterior), além de poder ver dados informações sobre o objeto de tipo.

1. Abra o arquivo chamado *OutputExpression.cshtml* que você criou anteriormente.
2. Substitua todo o código na página com o seguinte bloco de código:

    [!code-html[Main](introduction-to-debugging/samples/sample5.html)]
3. Salve e execute a página em um navegador.

    ![4 de depuração](introduction-to-debugging/_static/image3.jpg)

    Neste exemplo, o `ObjectInfo` auxiliar exibe dois itens:

   - O tipo. Para a primeira variável, o tipo é `DayOfWeek`. Para a segunda variável, o tipo é `String`.
   - O valor. Nesse caso, porque você já pode exibir o valor da variável de saudação na página, o valor é exibido novamente quando você passa a variável para `ObjectInfo`.

     Para obter objetos mais complexos, o `ObjectInfo` auxiliar pode exibir mais informações sobre &#8212; basicamente, ele pode exibir os tipos e valores de todas as propriedades de um objeto.

## <a name="using-debugging-tools-in-visual-studio"></a>Usando as ferramentas de depuração no Visual Studio

Para uma experiência de depuração mais abrangente, use o Visual Studio 2013 ou o software gratuito [Visual Studio Express 2013 para Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express). Com o Visual Studio, você pode definir um ponto de interrupção em seu código na linha que você deseja inspecionar.

![Definir ponto de interrupção](introduction-to-debugging/_static/image1.png)

Quando você testar o site da web, o código de execução é interrompida no ponto de interrupção.

![alcançar o ponto de interrupção](introduction-to-debugging/_static/image2.png)

Você pode examinar os valores atuais das variáveis e percorrer a código linha por linha.

![ver valores](introduction-to-debugging/_static/image3.png)

Para obter informações sobre como usar o depurador integrado no Visual Studio para depurar páginas Razor do ASP.NET, consulte [Programming ASP.NET Web Pages (Razor) usando o Visual Studio](https://go.microsoft.com/fwlink/?LinkId=205854).

## <a name="additional-resources"></a>Recursos adicionais

- [Programação de páginas da Web ASP.NET (Razor) usando o Visual Studio](https://go.microsoft.com/fwlink/?LinkId=205854)
- [Variáveis de servidor IIS](https://msdn.microsoft.com/library/ms524602(VS.90).aspx) (MSDN)
- [Reconhecido variáveis de ambiente](https://technet.microsoft.com/library/dd560744(WS.10).aspx) (TechNet)
