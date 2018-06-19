---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
title: Introdução a páginas da Web ASP.NET - Noções básicas de programação | Microsoft Docs
author: tfitzmac
description: "Este tutorial fornece uma visão geral de como programa em páginas da Web do ASP.NET com sintaxe do Razor. Você aprenderá: A sintaxe 'Razor' básica que você usa para pr..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/17/2015
ms.topic: article
ms.assetid: 7526ed45-a97d-4e8a-8301-01324ef0eff9
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
msc.type: authoredcontent
ms.openlocfilehash: 60115dd06a27bf856427953de29e993194afb991
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33839280"
---
<a name="introducing-aspnet-web-pages---programming-basics"></a>Introdução a páginas da Web ASP.NET - Noções básicas de programação
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial fornece uma visão geral de como programa em páginas da Web do ASP.NET com sintaxe do Razor.
> 
> O que você aprenderá:
> 
> - A sintaxe "Razor" básica que você usa para programação em páginas da Web do ASP.NET.
> - Alguns básica c#, que é a linguagem de programação que você usará.
> - Alguns conceitos de programação fundamentais para páginas da Web.
> - Como instalar pacotes (componentes que contêm código pré-compilada) para usar com seu site.
> - Como usar *auxiliares* para executar tarefas comuns de programação.
>   
> 
> Recursos/tecnologias abordadas:
> 
> - NuGet e o Gerenciador de pacotes.
> - O `Gravatar` auxiliar.


Este tutorial é basicamente um exercício em apresentando a sintaxe de programação que você usará para páginas da Web do ASP.NET. Você saberá mais sobre *sintaxe Razor* e linguagem de programação de código escrito em c#. Você viu rapidamente dessa sintaxe no tutorial anterior; Neste tutorial, explicaremos a sintaxe mais.

Prometemos que este tutorial envolve mais de programação que você verá um tutorial único e que é o tutorial único que é *somente* sobre programação. Os tutoriais restantes neste conjunto, você criará realmente páginas que fazer coisas interessantes.

Você também aprenderá sobre *auxiliares*. Um auxiliar é um componente — uma parte de cima empacotados de código, que você pode adicionar a uma página. O auxiliar executará o trabalho para você que pode ser entediante ou complexa para ser feito manualmente.

## <a name="creating-a-page-to-play-with-razor"></a>Criando uma página para experimentar Razor

Nesta seção você vai experimentar um pouco Razor para que você pode obter uma noção da sintaxe básica.

Se ele ainda não estiver em execução, inicie o WebMatrix. Você usará o site que você criou no tutorial anterior ([obtendo iniciado com páginas da Web](https://go.microsoft.com/fwlink/?LinkId=251578)). Para reabri-la, clique em **Meus Sites** e escolha **WebPageMovies**:

![Tela de início do WebMatrix mostrando as opções de Abrir Site e Meus Sites realçado](intro-to-web-pages-programming/_static/image1.png)

Selecione o **arquivos** espaço de trabalho.

Na faixa de opções, clique em **novo** para criar uma página. Selecione **CSHTML** e nomeie a nova página *TestRazor.cshtml*.

Clique em **OK**.

Copie o seguinte no arquivo, substituindo completamente o que há já.

> [!NOTE]
> Quando você copiar o código ou marcação de exemplos para uma página, o recuo e o alinhamento podem não ser o mesmo que o tutorial. Recuo e alinhamento não afetam como o código é executado, embora.


[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample1.cshtml)]

## <a name="examining-the-example-page"></a>Examinando a página de exemplo

Mais do que você vê é HTML comum. No entanto, na parte superior, há este bloco de código:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample2.cshtml)]

Observe os itens a seguir sobre este bloco de código:

- O caractere @ diz ao ASP.NET que se trata de código Razor, não em HTML. ASP.NET tratará tudo após o caractere de código até que ele seja executado novamente em alguns HTML @. (Nesse caso, que o &lt;! DOCTYPE&gt; elemento.
- As chaves ({e}) incluir um bloco de código Razor, se o código tiver mais de uma linha. As chaves informam ASP.NET onde o código para esse bloco inicia e termina.
- O / / caracteres marquem um comentário — ou seja, uma parte do código que não irá executar.
- Cada instrução deve terminar com um ponto e vírgula (;). (Não os comentários, embora.)
- Você pode armazenar valores em *variáveis*, que você criar (*declarar*) com o var de palavra-chave. Quando você cria uma variável, você dê a ele um nome, que pode incluir letras, números e sublinhado (\_). Nomes de variável não podem começar com um número e não é possível usar o nome de uma palavra-chave programação (como var).
- Você pode colocar cadeias de caracteres (como "ASP.NET" e "Páginas da Web") entre aspas. (Eles devem ser as aspas duplas). Números não estão entre aspas.
- Espaço em branco fora de aspas não importa. Quebras de linha geralmente não importam; a exceção é que não é possível dividir uma cadeia de caracteres entre aspas em linhas. Não importam o recuo e alinhamento.

Algo que não seja óbvio deste exemplo é que todo o código diferencia maiusculas de minúsculas. Isso significa que a variável TheSum é uma variável diferente que as variáveis que pode ser chamada theSum ou thesum. Da mesma forma, var é uma palavra-chave, mas Var não é.

### <a name="objects-and-properties-and-methods"></a>Objetos e propriedades e métodos

Em seguida, há a expressão Now. Em termos simples, DateTime é um *objeto*. Um objeto é uma coisa que você pode programar com — uma página, uma caixa de texto, um arquivo, uma imagem, uma solicitação da web, uma mensagem de email, um registro de cliente, etc. Objetos têm um ou mais *propriedades* que descrevem suas características. Um objeto de caixa de texto tem uma propriedade de texto (entre outros), um objeto de solicitação tem uma propriedade de Url (e outros), uma mensagem de email tem uma propriedade From e uma propriedade para, e assim por diante. Objetos também têm *métodos* que são "verbos" podem realizar. Você estará trabalhando com objetos muito.

Como você pode ver no exemplo, data e hora é um objeto que permite que você programa datas e horas. Ele tem uma propriedade chamada agora que retorna a data e hora atuais.

### <a name="using-code-to-render-markup-in-the-page"></a>Usar código para renderizar marcação na página

No corpo da página, observe o seguinte:

[!code-html[Main](intro-to-web-pages-programming/samples/sample3.html)]

Novamente, o caractere @ diz ao ASP.NET que segue é o código, não em HTML. Na marcação você pode adicionar seguido por uma expressão de código e ASP.NET processará o valor de direito essa expressão nesse momento. No exemplo, @a processará tudo o que é o valor da variável nomeada, @product renderiza que estiver no produto variável nomeado e assim por diante.

Você não está limitado a variáveis, embora. Em alguns casos aqui, o caractere @ precede uma expressão:

- @(um\*b) renderiza o produto de tudo o que é nas variáveis de um e b. (O \* significa de operador de multiplicação.)
- @(tecnologia + "" + produto) renderiza os valores na tecnologia de variáveis e produto depois concatená-las e adicionar um espaço entre. O operador (+) para concatenar cadeias de caracteres é o mesmo que o operador para adicionar números. ASP.NET geralmente pode informar se você estiver trabalhando com números ou cadeias de caracteres e faz o certo com o operador +.
- @Request.Url renderiza a propriedade Url do objeto de solicitação. O objeto de solicitação contém informações sobre a solicitação atual do navegador e, claro, a propriedade Url contém a URL da solicitação atual.

O exemplo também foi projetado para mostrar que você pode trabalhar de maneiras diferentes. Você pode fazer cálculos no bloco de código na parte superior, colocar os resultados em uma variável e processará a variável na marcação. Ou você pode fazer cálculos em um direito de expressão na marcação. A abordagem usada depende que você está fazendo e, até certo ponto, de sua preferência.

### <a name="seeing-the-code-in-action"></a>Ver o código em ação

Clique no nome do arquivo e, em seguida, escolha **iniciar no navegador**. Consulte a página no navegador com todos os valores e expressões resolvidas na página.

![Página 'TestRazor' em execução no navegador](intro-to-web-pages-programming/_static/image2.png)

Examine o código-fonte no navegador.

![Origem da página 'Teste Razor' no navegador](intro-to-web-pages-programming/_static/image3.png)

Como você espera de sua experiência no tutorial anterior, nenhum código Razor está na página. Que são os valores de exibição real. Quando você executa uma página, você está, na verdade, fazendo uma solicitação ao servidor da web é criado para o WebMatrix. Quando a solicitação é recebida, o ASP.NET resolve todos os valores e expressões e renderiza os valores para a página. Em seguida, envia a página para o navegador.

> [!TIP] 
> 
> **Razor e C#.**
> 
> Até agora dissemos que você está trabalhando com a sintaxe do Razor. Isso é verdadeiro, mas não é a história completa. Linguagem de programação real que você está usando é chamada *c#*. C# foi criado pela Microsoft, há uma década e se tornou uma das linguagens de programação principais para a criação de aplicativos do Windows. Todas as regras que você viu sobre como nomear uma variável e como criar instruções e assim por diante são, na verdade, todas as regras da linguagem c#.
> 
> Razor se refere mais especificamente a um pequeno conjunto de convenções para como inserir esse código em uma página. Por exemplo, a convenção de usando para marcar o código na página e @ {} para inserir um bloco de código é o aspecto de Razor de uma página. Auxiliares também são considerados parte do Razor. A sintaxe do Razor é usada em mais locais do que apenas em páginas da Web ASP.NET. (Por exemplo, ele é usado em modos de exibição de ASP.NET MVC.)
> 
> Mencionamos isso porque se você procurar informações sobre programação páginas da Web ASP.NET, você encontrará muitas referências para o Razor. No entanto, muito essas referências não se aplicam ao qual você está fazendo e, portanto, pode ser confuso. E, na verdade, muitas das suas perguntas programação realmente serão sobre o trabalho com c# ou trabalhar com o ASP.NET. Então se você observar especificamente para obter informações sobre Razor, talvez você não encontre as respostas que necessárias.


## <a name="adding-some-conditional-logic"></a>Adicionando alguma lógica condicional

Um dos excelentes recursos sobre o uso de código em uma página é que você pode alterar o que acontece com base em várias condições. Nesta parte do tutorial, você irá brincar com algumas maneiras de alterar o que é exibido na página.

O exemplo será simples e um pouco contrived para que possamos nos concentrar na lógica condicional. A página que você criará fará isso:

- Mostra texto diferente na página, dependendo se ela for a primeira vez que a página é exibida ou se você clicou em um botão para enviar a página. Que será o primeiro teste condicional.
- Exibe a mensagem somente se um determinado valor for transmitido na cadeia de caracteres de consulta da URL (http://...?show=true). Que será o segundo teste condicional.

No WebMatrix, crie uma página e nomeie- *TestRazorPart2.cshtml*. (Na faixa de opções, clique em **novo**, escolha **CSHTML**, nomeie o arquivo e, em seguida, clique em **Okey**.)

Substitua o conteúdo dessa página com o seguinte:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample4.cshtml)]

O bloco de código na parte superior inicializa uma variável denominada mensagem com algum texto. No corpo da página, o conteúdo da variável de mensagem é exibido dentro de um &lt;p&gt; elemento. A marcação também contém um &lt;entrada&gt; elemento para criar um **enviar** botão.

Execute a página para ver como ele funciona. Por enquanto, é basicamente uma página estática, mesmo se você clicar no **enviar** botão.

Volte para o WebMatrix. Dentro do bloco de código, adicione o seguinte código realçado *depois* a linha que inicializa a mensagem:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample5.cshtml?highlight=4-6)]

### <a name="the-if---block"></a>Se o bloco de {}

O que você acabou de adicionar foi um se condição. No código, o se a condição com uma estrutura semelhante isso:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample6.cs)]

A condição de teste está entre parênteses. Ele deve ser um valor ou uma expressão que retorna true ou false. Se a condição for true, o ASP.NET executa a instrução ou instruções que estão dentro dos colchetes. (Esses são os *, em seguida,* parte do *se-então* lógica.) Se a condição for falsa, o bloco de código será ignorado.

Aqui estão alguns exemplos de condições que você pode testar um se instrução:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample7.cs)]

Você pode testar variáveis com base nos valores ou em expressões, usando um <em>operador lógico</em> ou <em>operador de comparação</em>: igual a (= =), maior que (&gt;), menor que (&lt;), maior que ou igual a (&gt;=) e menor ou igual a (&lt;=). O! = operador significa não é igual a — por exemplo, se (um! = 0) significa <em>se</em> <em>um</em><em>não é igual a 0</em>.

> [!NOTE]
> Verifique se que você notar que o operador de comparação igual a (= =) não é o mesmo =. O = operador é usado apenas para atribuir valores (var um = 2). Se você combinar esses operadores de, ou você receberá um erro ou você terá alguns resultados estranhos.


Para testar se algo é true, a sintaxe completa é if(IsDone == true). Mas você também pode usar o atalho if(IsDone). Se não houver nenhum operador de comparação, o ASP.NET pressupõe que você está testando para true.

O operador! operador por si só significa um NOT lógico. Por exemplo, a condição if (! IsPost) significa *se IsPost não for verdadeiro*.

Você pode combinar condições usando um AND lógico (&amp; &amp; operador) ou OR lógico (| | operador). Por exemplo, se a última condições no significa exemplos anteriores *se FileProcessingIsDone não está definido como true displayMessage AND é definida como false*.

### <a name="the-else-block"></a>O bloco else

Uma coisa final sobre se blocos: um se o bloco pode ser seguido por um bloco else. Um bloco else é útil é que você precisa executar código diferente quando a condição for falsa. Aqui está um exemplo simples:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample8.cs)]

Você verá alguns exemplos em tutoriais subsequentes dessa série onde é útil usar um bloco else.

### <a name="testing-whether-the-request-is-a-submit-post"></a>Teste se a solicitação é um envio (post)

Há mais, mas vamos voltar ao exemplo, que tem o if(IsPost) condição {…}. IsPost é realmente uma propriedade da página atual. Na primeira vez que a página é solicitada, IsPost retorna false. No entanto, se você clicar em um botão ou caso contrário, enviar a página — ou seja, lançar — IsPost retorna true. Portanto IsPost permite determinar se você está lidando com um envio de formulário. (Em termos de verbos HTTP, se a solicitação é uma operação GET, IsPost retornará false. Se a solicitação é uma operação POST, IsPost retorna true.) Um tutorial posterior, você trabalhará com formulários de entrada, em que esse teste é particularmente útil.

Execute a página. Como esta é a primeira vez que você solicitados a página, consulte "Esta é a primeira vez que você solicitou a página". Essa cadeia de caracteres é o valor que você inicializar a variável de mensagem. Há um teste de if(IsPost), mas que retorna false no momento, portanto o código dentro de se bloquear não é executado.

Clique o **enviar** botão. A página é solicitada novamente. Como antes, a variável de mensagem é definida como "Esta é a primeira vez...". Mas, neste momento, if(IsPost) o teste retornar true, para que o código dentro do se execução do bloco. O código altera o valor da variável de mensagem para um valor diferente, que é o que é renderizado na marcação.

Agora, adicione um se a condição na marcação. Abaixo do &lt;p&gt; elemento que contém o **enviar** botão, adicione a seguinte marcação:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample9.cshtml)]

Você está adicionando o código dentro da marcação, para que seja necessário iniciar com @. Há um se teste semelhante ao que você adicionou anteriormente para cima no bloco de código. Dentro das chaves, porém, você está adicionando HTML comum — pelo menos, é comum até chegar ao @DateTime.Now. Este é outro pouco de código Razor, novamente você precisará adicionar na frente dele.

O ponto aqui é que você pode adicionar se condições em ambos o código de bloco na parte superior e na marcação. Se você usar um se condição no corpo da página, as linhas dentro do bloco pode ser marcação ou código. Nesse caso, e como ocorre sempre que você misturar marcação e código, você deve usar para tornar claro para o ASP.NET onde está o código.

Execute a página e clique em **enviar**. Desta vez não verá apenas uma mensagem diferente quando você enviar ("Agora você enviou..."), mas você verá uma nova mensagem que lista a data e hora.

![Página 2 do Razor de teste em execução no navegador com carimbo de hora mostrando após enviar](intro-to-web-pages-programming/_static/image4.png)

### <a name="testing-the-value-of-a-query-string"></a>Testar o valor de uma cadeia de caracteres de consulta

Um teste mais. Neste momento, você adicionará um se bloco que testa um valor denominado show que pode ser passado na cadeia de caracteres de consulta. (Esta aparência: `http://localhost:43097/TestRazorPart2.cshtml?show=true`) você alterará a página para que a mensagem você tiver sido exibindo ("Esta é a primeira vez...", etc.) é exibida somente se o valor de mostrar é true.

Na parte inferior (mas interna), o bloco de código na parte superior da página, adicione o seguinte:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample10.cs)]

A código completo bloco agora a aparência o exemplo a seguir. (Lembre-se de que, quando você copiar o código para a página, o recuo pode ser diferente. Mas que não afeta como o código é executado.)

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample11.cshtml)]

O novo código no bloco inicializa uma variável chamada showMessage como false. Em seguida, faz um se o teste para procurar um valor na cadeia de caracteres de consulta. Ao solicitar a página pela primeira vez, ele tem uma URL como este:

`http://localhost:43097/TestRazorPart2.cshtml`

O código determina se a URL contém uma variável chamada show na cadeia de consulta, como a esta versão da URL:

`http://localhost:43097/TestRazorPart2.cshtml`? Mostrar = true

O próprio teste examina a propriedade QueryString do objeto de solicitação. Se a cadeia de caracteres de consulta contém um item denominado show, e se esse item é definido como true, o se bloco é executado e define a variável showMessage como true.

Há um truque aqui, como você pode ver. Como o nome diz, a cadeia de caracteres de consulta é uma cadeia de caracteres. No entanto, você só pode testar para true e false se o valor que você está testando é um valor booliano de (true/false). Antes de você testar o valor da variável Mostrar na cadeia de caracteres de consulta, você precisa convertê-lo em um valor booleano. É o que faz o método AsBool — ela usa uma cadeia de caracteres como entrada e o converte em um valor booleano. Obviamente, se a cadeia de caracteres for "true", o método AsBool converte esse valor como true. Se o valor da cadeia de caracteres é qualquer outra coisa, AsBool retorna false.

> [!TIP] 
> 
> **Tipos de dados e os métodos de As()**
> 
> Somente dissemos até que, quando você cria uma variável, você use o var de palavra-chave. Não é todo o texto. Para manipular valores — para adicionar os números, ou concatenar cadeias de caracteres, ou comparar as datas ou de teste para verdadeiro/falso — c# tiver de funcionar com uma representação interna apropriada do valor. C# pode *geralmente* descobrir o que essa representação deve ser (ou seja, o que *tipo* os dados) com base no que você está fazendo com que os valores. No entanto, ele não pode fazer isso. Caso contrário, você precisa ajudar explicitamente indicando como c# devem representar os dados. O método AsBool faz isso — informa c# que um valor de cadeia de caracteres "true" ou "false" deve ser tratado como um valor booliano. Existem métodos semelhantes para representar cadeias de caracteres em outros tipos, como AsInt (considerar como um número inteiro), AsDateTime (considerar como uma data/hora), AsFloat (considerar como um número de ponto flutuante) e assim por diante. Quando você usá-los como métodos (), se c# não pode representar o valor de cadeia de caracteres como solicitado, você verá um erro.


Na marcação da página, remova ou comente esse elemento (aqui é mostrado comentado):

[!code-html[Main](intro-to-web-pages-programming/samples/sample12.html)]

Direita onde você removeu ou comentada que o texto, adicione o seguinte:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample13.cshtml)]

O se o teste diz que se a variável showMessage for true, renderizar um &lt;p&gt; elemento com o valor da variável de mensagem.

### <a name="summary-of-your-conditional-logic"></a>Resumo de sua lógica condicional

Caso você não tiver certeza de que você acabou de fazer, aqui está um resumo.

- A variável de mensagem é inicializada com uma cadeia de caracteres padrão ("Esta é a primeira vez...").
- Se a solicitação de página é o resultado de um envio (post), o valor da mensagem é alterado para "Agora você enviou..."
- A variável showMessage é inicializada como false.
- Se a cadeia de caracteres de consulta contém? Mostrar = true, a variável showMessage é definida como true.
- Na marcação, se showMessage for true, uma &lt;p&gt; elemento é processado que mostra o valor da mensagem. (Se showMessage for false, nada será renderizado nesse ponto na marcação.)
- Na marcação, se a solicitação for uma postagem, uma &lt;p&gt; elemento é processado que exibe a data e hora.

Execute a página. Não há nenhuma mensagem, pois showMessage é false, portanto na marcação o teste de if(showMessage) retorna false.

Clique em **Enviar**. Você ver a data e hora, mas ainda assim nenhuma mensagem.

Em seu navegador, vá para a caixa de URL e adicione o seguinte ao final da URL:? Mostrar = true e, em seguida, pressione Enter.

![Página 2 do Razor de teste no navegador, mostrando a cadeia de caracteres de consulta](intro-to-web-pages-programming/_static/image5.png)

A página é exibida novamente. (Como você alterou a URL, essa é uma nova solicitação, não um envio.) Clique em **enviar** novamente. A mensagem é exibida novamente, como a data e hora.

![Página 2 do Razor de teste após enviar quando há uma cadeia de caracteres de consulta](intro-to-web-pages-programming/_static/image6.png)

Alterar a URL? Mostrar = true para? mostrar = false e pressione Enter. Envie novamente a página. A página é novamente como iniciou — nenhuma mensagem.

Conforme observado anteriormente, a lógica deste exemplo é um pouco contrived. No entanto, se vai aparecer em muitas de suas páginas e levará um ou mais dos formulários visto aqui.

## <a name="installing-a-helper-displaying-a-gravatar-image"></a>Instalando um auxiliar (exibir uma imagem Gravatar)

Algumas tarefas que as pessoas desejam em páginas da web geralmente exigem muito código ou exigem conhecimento extra. Exemplos: exibir um gráfico de dados. colocar um Facebook "botão curtir" em uma página. Enviar email de seu site. recortando ou redimensionamento de imagens; usando PayPal para o seu site. Para facilitar a esses tipos de coisas, o ASP.NET Web Pages permite que você use *auxiliares*. Auxiliares são componentes que você instalar um site e que permitem executar tarefas comuns usando apenas algumas linhas de código Razor.

Páginas da Web do ASP.NET tem alguns auxiliares internos. No entanto, muitos auxiliares estão disponíveis em pacotes (Suplementos) que são fornecidos usando o NuGet package manager. O NuGet permite que você selecione um pacote para instalar e, em seguida, ele cuida de todos os detalhes da instalação.

Nesta parte do tutorial, você instalará um auxiliar que permite que você exiba uma imagem Gravatar ("avatar globalmente reconhecida"). Você aprenderá duas coisas. Uma é como localizar e instalar um auxiliar. Você também aprenderá como um auxiliar torna mais fácil de fazer algo que seriam necessários para fazer usando muito código que você teria de escrever por conta própria.

Você pode registrar seu próprio Gravatar no site Gravatar no [ http://www.gravatar.com/ ](http://www.gravatar.com/), mas não é essencial para criar uma conta de Gravatar para executar esta parte do tutorial.

No WebMatrix, clique o **NuGet** botão.

![Caixa de diálogo do NuGet galeria no WebMatrix](intro-to-web-pages-programming/_static/image7.png)

Isso inicia o NuGet package manager e exibe os pacotes disponíveis. (Nem todos os pacotes são auxiliares; alguns adicionam funcionalidades ao WebMatrix em si, alguns modelos adicionais, em assim por diante.) Você pode receber uma mensagem de erro sobre incompatibilidade de versão. Você pode ignorar essa mensagem de erro clicando **Okey** e continuar com este tutorial.

![Caixa de diálogo do NuGet galeria no WebMatrix](intro-to-web-pages-programming/_static/image8.png)

Na caixa de pesquisa, digite "auxiliares do asp.net". NuGet mostra os pacotes que correspondem aos termos de pesquisa.

![Galeria no WebMatrix, mostrando os pacotes do NuGet](intro-to-web-pages-programming/_static/image9.png)

O ASP.NET Web Helpers Library contém código para simplificar muitas tarefas comuns, incluindo o uso de imagens de Gravatar. Selecione o **ASP.NET Web Helpers Library** do pacote e, em seguida, clique em **instalar** para iniciar o instalador. Selecione **Sim** quando for perguntado se deseja instalar o pacote e aceite os termos para concluir a instalação.

É isso. NuGet baixa e instala tudo, inclusive todos os componentes adicionais que podem ser necessários (*dependências*).

Se por algum motivo, você precisa desinstalar um auxiliar, o processo é muito semelhante. Clique o **NuGet** , clique no **instalado** guia e selecione o pacote que deseja desinstalar.

## <a name="using-a-helper-in-a-page"></a>Usando um auxiliar em uma página

Agora, você usará o auxiliar que você acabou de instalar. O processo para adicionar um auxiliar a uma página é semelhante para a maioria dos auxiliares.

No WebMatrix, crie uma página e nomeie- *GravatarTest.cshml*. (Você está criando uma página especial para testar o auxiliar, mas você pode usar os auxiliares em qualquer página no seu site).

Dentro de &lt;corpo&gt; elemento, adicionar um &lt;div&gt; elemento. Dentro de &lt;div&gt; elemento, digite:

@Gravatar.

O caractere @ é o mesmo caractere que você usou para marcar código Razor. **Gravatar** é o objeto auxiliar que você está trabalhando.

Assim que você digita o ponto (.), o WebMatrix exibe uma lista de *métodos* (funções) que disponibiliza o auxiliar Gravatar:

![Lista de auxiliar suspensa IntelliSense Gravatar](intro-to-web-pages-programming/_static/image10.png)

Esse recurso é conhecido como *IntelliSense*. Ele ajuda a código fornecendo opções apropriada ao contexto. IntelliSense funciona com HTML, CSS, código ASP.NET, JavaScript e outras linguagens com suporte no WebMatrix. É outro recurso que facilita a desenvolver páginas da web no WebMatrix.

Pressionar G no teclado e consulte IntelliSense localiza o método GetHtml. Pressione Tab. IntelliSense insere o método selecionado (GetHtml) para você. Digite um parêntese de abertura e observe o parêntese de fechamento é adicionado automaticamente. Digite seu endereço de email entre aspas entre dois parênteses. Se você tiver uma conta de Gravatar, sua imagem de perfil será retornada. Se você não tiver uma conta de Gravatar, uma imagem padrão será retornada. Quando terminar, a linha tem esta aparência:

[!code-css[Main](intro-to-web-pages-programming/samples/sample14.css)]

Agora, exiba a página em um navegador. A imagem ou a imagem padrão é exibida, dependendo se você tiver uma conta de Gravatar.

![Gravatar](intro-to-web-pages-programming/_static/image11.png) ![imagem padrão](intro-to-web-pages-programming/_static/image12.png)

Para obter uma ideia do que o auxiliar está fazendo para você, exibir a origem da página no navegador. Junto com o HTML que você tinha em sua página, você verá um elemento de imagem que inclui um identificador. Este é o código que o auxiliar processado para a página no local em que você tinha @Gravatar.GetHtml. O auxiliar obteve as informações fornecidas e o código que se comunica diretamente com Gravatar para voltar a imagem correta para a conta fornecida gerado.

O método GetHtml também permite que você personalize a imagem, fornecendo os outros parâmetros. O código a seguir mostra como solicitar uma imagem tem uma largura e altura de 40 pixels e usa uma imagem padrão especificado chamada **wavatar** se a conta especificada não existe.

[!code-javascript[Main](intro-to-web-pages-programming/samples/sample15.js)]

Esse código produz algo parecido com o seguinte resultado (a imagem padrão aleatoriamente pode variar).

![](intro-to-web-pages-programming/_static/image13.png)

## <a name="coming-up-next"></a>Próximo

Para manter este breve tutorial, tivemos que se concentrar em apenas alguns conceitos básicos. Naturalmente, há um *muito* mais Razor e c#. Você aprenderá mais que você percorrer esses tutoriais. Se você estiver interessado em saber mais sobre os aspectos de programação do Razor e c# no momento, você pode ler uma introdução mais detalhada aqui: [Introdução ao ASP.NET Web programação usando a sintaxe do Razor](https://go.microsoft.com/fwlink/?LinkID=202890).

O seguinte tutorial apresenta a trabalhar com um banco de dados. Neste tutorial, você começará a criar o aplicativo de exemplo que permite que você listar seus filmes favoritos.

## <a name="complete-listing-for-testrazor-page"></a>Lista completa de página TestRazor

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample16.cshtml)]

## <a name="complete-listing-for-testrazorpart2-page"></a>Lista completa de página TestRazorPart2

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample17.cshtml)]

## <a name="complete-listing-for-gravatartest-page"></a>Lista completa de página GravatarTest

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample18.cshtml)]

## <a name="additional-resources"></a>Recursos adicionais

- [Introdução à programação da Web do ASP.NET usando a sintaxe do Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Auxiliar do Twitter](../../ui-layouts-and-themes/twitter-helper.md)

> [!div class="step-by-step"]
> [Anterior](getting-started.md)
> [Próximo](displaying-data.md)
