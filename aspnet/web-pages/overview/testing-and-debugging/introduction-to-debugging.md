---
uid: web-pages/overview/testing-and-debugging/introduction-to-debugging
title: Introdução à depuração da Web do ASP.NET páginas Sites (Razor) | Microsoft Docs
author: tfitzmac
description: A depuração é o processo de localizar e corrigir erros em suas páginas de código. Este capítulo mostra algumas ferramentas e técnicas que você pode usar para depurar e analyz...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 68de4326-7611-4b9b-b5f6-79b7adc3069f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/testing-and-debugging/introduction-to-debugging
msc.type: authoredcontent
ms.openlocfilehash: c28d63acda6e585f4aa64f294049c1790faac850
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="introduction-to-debugging-aspnet-web-pages-razor-sites"></a>Introdução à depuração da Web do ASP.NET páginas Sites (Razor)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo explica as várias maneiras de depurar páginas em um site de páginas da Web do ASP.NET (Razor). A depuração é o processo de localizar e corrigir erros em suas páginas de código.
> 
> **O que você aprenderá:** 
> 
> - Como exibir informações que ajudam a analisar e depurar páginas.
> - Como usar a depuração de ferramentas no Visual Studio.
>   
> 
> Estes são os recursos ASP.NET introduzidos no artigo:
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
> Este tutorial também funciona com 2 de páginas da Web do ASP.NET. Você pode usar o WebMatrix 3, mas não há suporte para o depurador integrado.


É um aspecto importante da solução de problemas de erros e problemas no seu código para evitá-los em primeiro lugar. Você pode fazer isso colocando seções do seu código que podem causar erros em `try/catch` blocos. Para obter mais informações, consulte a seção sobre manipulação de erros em [Introdução ao ASP.NET Web programação usando a sintaxe do Razor](https://go.microsoft.com/fwlink/?LinkId=202890).

O `ServerInfo` auxiliar é uma ferramenta de diagnóstico que fornece uma visão geral das informações sobre o ambiente de servidor web que hospeda a página. Ele também mostra informações de solicitação HTTP enviada quando a página de solicitações de um navegador. O `ServerInfo` auxiliar exibe a identidade do usuário atual, o tipo de navegador que fez a solicitação, e assim por diante. Esse tipo de informação pode ajudar a solucionar problemas comuns.

1. Criar uma nova página da web denominado *ServerInfo.cshtml*.
2. No final da página, imediatamente antes do fechamento `</body>` marca, adicione `@ServerInfo.GetHtml()`:

    [!code-cshtml[Main](introduction-to-debugging/samples/sample1.cshtml)]

    Você pode adicionar o `ServerInfo` código em qualquer lugar na página. Mas, adicioná-lo no final manterá sua saída separada de seus outros conteúdos da página, que torna mais fácil de ler.

    > [!NOTE] 
    > 
    > **Importante** você deve remover qualquer código de diagnóstico de suas páginas da web antes de mover páginas da web em um servidor de produção. Isso se aplica ao `ServerInfo` auxiliar, bem como as outras técnicas de diagnóstico neste artigo que envolvem a inclusão de código para uma página. Você não deseja os visitantes do site para obter informações sobre o nome do servidor, os nomes de usuário, os caminhos no seu servidor e detalhes semelhantes, pois esse tipo de informação pode ser útil para pessoas mal-intencionadas.
3. Salve a página e executá-lo em um navegador.

    ![Depuração-1](introduction-to-debugging/_static/image1.jpg)

    O `ServerInfo` auxiliar exibe quatro tabelas de informações na página:

   - Configuração do servidor. Esta seção fornece informações sobre o servidor de hospedagem da web, incluindo o nome do computador, a versão do ASP.NET que você está executando, o nome de domínio e a hora do servidor.
   - Variáveis de servidor do ASP.NET. Esta seção fornece detalhes sobre os diversos detalhes de protocolo HTTP (chamados variáveis HTTP) e os valores que fazem parte de cada solicitação de página da web.
   - Informações de tempo de execução HTTP. Esta seção fornece detalhes sobre o que a versão do Microsoft .NET Framework que sua página da web está em execução no, o caminho, detalhes sobre o cache e assim por diante. (Como você aprendeu em [Introdução ao ASP.NET Web programação usando a sintaxe do Razor](https://go.microsoft.com/fwlink/?LinkId=202890), páginas da Web ASP.NET usando o Razor sintaxe baseiam-se na tecnologia ASP.NET da Microsoft web server, que também é criado em uma ampla de software biblioteca de desenvolvimento chamado o .NET Framework.)
   - Variáveis de ambiente. Esta seção fornece uma lista de todas as variáveis de ambiente local e seus valores no servidor web.

     Uma descrição completa de todas as informações de solicitação e o servidor está além do escopo deste artigo, mas você pode ver que o `ServerInfo` auxiliar retorna várias informações de diagnóstico. Para obter mais informações sobre os valores que `ServerInfo` retorna, consulte [variáveis de ambiente reconhecido](https://technet.microsoft.com/library/dd560744(WS.10).aspx) no site da Microsoft TechNet e [variáveis de servidor IIS](https://msdn.microsoft.com/library/ms524602(VS.90).aspx) no site do MSDN.

## <a name="embedding-output-expressions-to-display-page-values"></a>Expressões de saída inserido para exibir valores de página

É outra maneira de ver o que está acontecendo em seu código inserir expressões de saída na página. Como você sabe, você pode extrair o valor de uma variável diretamente adicionando algo como `@myVariable` ou `@(subTotal * 12)` para a página. Para depuração, você pode colocar essas expressões de saída em pontos estratégicos no seu código. Isso permite que você veja o valor de chave variáveis ou o resultado de cálculos quando a página é executada. Quando você terminar de depuração, você pode remover as expressões ou comente-os. Este procedimento ilustra uma maneira comum de usar expressões inseridas para ajudar a depurar uma página.

1. Criar uma nova página do WebMatrix chamado *OutputExpression.cshtml*.
2. Substitua a página de conteúdo com o seguinte:

    [!code-html[Main](introduction-to-debugging/samples/sample2.html)]

    O exemplo usa um `switch` instrução para verificar o valor da `weekday` variável e, em seguida, exibir uma mensagem de saída diferentes dependendo de qual dia da semana é. No exemplo, o `if` bloco entre o primeiro bloco de código arbitrariamente altera o dia da semana com a adição de um dia para o valor de dia da semana atual. Este é um erro introduzido para fins de ilustração.
3. Salve a página e executá-lo em um navegador.

    A página exibe a mensagem para o errado dia da semana. O dia da semana, na verdade, é, você verá a mensagem para um dia depois. Embora nesse caso você saiba por que a mensagem está desativado (porque o código deliberadamente define o valor de dia incorreta), na realidade, muitas vezes é difícil saber futuro incorreto no código. Para depurar, você precisa saber o que acontece ao valor de objetos de chave e variáveis, como `weekday`.
4. Adicionar expressões de saída inserindo `@weekday` conforme mostrado nos dois locais indicados por comentários no código. Essas expressões de saída exibirá os valores da variável nesse ponto na execução de código.

    [!code-csharp[Main](introduction-to-debugging/samples/sample3.cs?highlight=2-3,15-16)]
5. Salve e execute a página em um navegador.

    A página exibe o dia da semana real pela primeira vez, em seguida, atualizado dia da semana que resulta da adição de um dia e, em seguida, a mensagem resultante do `switch` instrução. A saída das duas expressões de variável (`@weekday`) não têm espaços entre os dias porque você não adicionou qualquer HTML `<p>` marcas para a saída; as expressões são apenas para teste.

    ![Debugging-2](introduction-to-debugging/_static/image2.jpg)

    Agora você pode ver onde está o erro. Quando você exibe primeiro os `weekday` variável no código, ele mostra o dia correto. Quando você exibi-lo na segunda vez, após o `if` bloco no código, o dia é desativada por um. Para que você saiba que algo ocorreu entre o primeiro e segundo o aparecimento da variável de dia da semana. Se esse fosse um bug real, esse tipo de abordagem seria ajudá-lo a restringir o local do código que está causando o problema.
6. Corrija o código na página removendo as expressões de dois saída adicionado e removendo o código que altera o dia da semana. O restante, concluído o bloco de código parece com o exemplo a seguir:

    [!code-cshtml[Main](introduction-to-debugging/samples/sample4.cshtml)]
7. Execute a página em um navegador. Neste momento você ver a mensagem correta exibida para o dia atual da semana.

## <a name="using-the-objectinfo-helper-to-display-object-values"></a>Usando o auxiliar ObjectInfo para exibir valores de objeto

O `ObjectInfo` auxiliar exibe o tipo e o valor de cada objeto que você passa para ele. Você pode usá-lo para exibir o valor de variáveis e objetos em seu código (como feito com expressões de saída no exemplo anterior), além de poder ver dados informações sobre o objeto de tipo.

1. Abra o arquivo chamado *OutputExpression.cshtml* que você criou anteriormente.
2. Substitua todo o código na página com o bloco de código a seguir:

    [!code-html[Main](introduction-to-debugging/samples/sample5.html)]
3. Salve e execute a página em um navegador.

    ![4 de depuração](introduction-to-debugging/_static/image3.jpg)

    Neste exemplo, o `ObjectInfo` auxiliar exibe dois itens:

   - O tipo. Para a primeira variável, o tipo é `DayOfWeek`. Para a segunda variável, o tipo é `String`.
   - O valor. Nesse caso, porque você já pode exibir o valor da variável de saudação na página, o valor é exibido novamente quando você passar a variável `ObjectInfo`.

     Para objetos mais complexos, o `ObjectInfo` auxiliar pode exibir mais informações &#8212; basicamente, ele pode exibir os tipos e valores de todas as propriedades de um objeto.

## <a name="using-debugging-tools-in-visual-studio"></a>Usando ferramentas de depuração no Visual Studio

Para uma experiência de depuração mais abrangente, use o Visual Studio 2013 ou livre [Visual Studio Express 2013 para Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express). Com o Visual Studio, você pode definir um ponto de interrupção no seu código na linha que deseja inspecionar.

![Conjunto de ponto de interrupção](introduction-to-debugging/_static/image1.png)

Quando você testar o site da web, o código de execução é interrompida no ponto de interrupção.

![alcançar o ponto de interrupção](introduction-to-debugging/_static/image2.png)

Você pode examinar os valores atuais das variáveis e percorrer a código linha por linha.

![Consulte os valores](introduction-to-debugging/_static/image3.png)

Para obter informações sobre como usar o depurador integrado no Visual Studio para depurar páginas ASP.NET Razor, consulte [Programming ASP.NET Web Pages (Razor) com o Visual Studio](https://go.microsoft.com/fwlink/?LinkId=205854).

## <a name="additional-resources"></a>Recursos adicionais

- [Programação de páginas da Web do ASP.NET (Razor) usando o Visual Studio](https://go.microsoft.com/fwlink/?LinkId=205854)
- [Variáveis de servidor IIS](https://msdn.microsoft.com/library/ms524602(VS.90).aspx) (MSDN)
- [Reconhecido variáveis de ambiente](https://technet.microsoft.com/library/dd560744(WS.10).aspx) (TechNet)
