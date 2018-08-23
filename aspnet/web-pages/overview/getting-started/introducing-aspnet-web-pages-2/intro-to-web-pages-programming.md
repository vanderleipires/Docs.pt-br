---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
title: Introdução ao ASP.NET Web Pages - Noções básicas da programação | Microsoft Docs
author: tfitzmac
description: "Este tutorial fornece uma visão geral de como ao programa em páginas da Web do ASP.NET com sintaxe do Razor. O que você vai aprender: A sintaxe 'Razor' básica que você usa para pr..."
ms.author: riande
ms.date: 06/17/2015
ms.assetid: 7526ed45-a97d-4e8a-8301-01324ef0eff9
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
msc.type: authoredcontent
ms.openlocfilehash: 120246fab3e71afeef2e2b7c4388f7c294e6b703
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825197"
---
<a name="introducing-aspnet-web-pages---programming-basics"></a>Introdução ao ASP.NET Web Pages - Noções básicas de programação
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial fornece uma visão geral de como ao programa em páginas da Web do ASP.NET com sintaxe do Razor.
> 
> O que você aprenderá:
> 
> - A sintaxe "Razor" básica que você pode usar para a programação em páginas da Web do ASP.NET.
> - Alguns básicas c#, que é a linguagem de programação que você usará.
> - Alguns conceitos fundamentais da programação para páginas da Web.
> - Como instalar pacotes (componentes que contêm código pré-compilado) para usar com seu site.
> - Como usar *auxiliares* para executar tarefas comuns de programação.
>   
> 
> Recursos/tecnologias abordadas:
> 
> - O NuGet e o Gerenciador de pacotes.
> - O `Gravatar` auxiliar.


Este tutorial é basicamente um exercício de apresentar a você a sintaxe de programação que você usará para páginas da Web do ASP.NET. Você saberá mais sobre *sintaxe do Razor* e linguagem de programação de código que é escrito em c#. Você tem uma ideia dessa sintaxe no tutorial anterior; Neste tutorial, explicaremos a sintaxe mais.

Prometemos que este tutorial envolve mais de programação que você verá um tutorial único e que é o tutorial único que é *apenas* sobre programação. Os tutoriais restantes neste conjunto, você realmente cria páginas que fazem coisas interessantes.

Você também aprenderá *auxiliares*. Um auxiliar é um componente — uma parte de cima empacotados de código — que podem ser adicionados a uma página. O auxiliar realiza o trabalho para você que, caso contrário, pode ser entediante ou complexa para ser feito manualmente.

## <a name="creating-a-page-to-play-with-razor"></a>Criando uma página para brincar com Razor

Nesta seção você irá brincar um pouco com Razor para que você possa obter uma ideia do que a sintaxe básica.

Se ele ainda não estiver em execução, inicie o WebMatrix. Você usará o site que você criou no tutorial anterior ([obtendo iniciado com páginas da Web](https://go.microsoft.com/fwlink/?LinkId=251578)). Para reabri-la, clique em **Meus Sites** e escolha **WebPageMovies**:

![Tela de início do WebMatrix mostrando as opções de Abrir Site e os Meus Sites realçado](intro-to-web-pages-programming/_static/image1.png)

Selecione o **arquivos** espaço de trabalho.

Na faixa de opções, clique em **New** para criar uma página. Selecione **CSHTML** e nomeie a nova página *TestRazor.cshtml*.

Clique em **OK**.

Copie o seguinte para o arquivo, substituindo completamente o que há já.

> [!NOTE]
> Quando você copiar o código ou marcação de exemplos para uma página, o recuo e o alinhamento podem não ser o mesmo do tutorial. Recuo e alinhamento não afetam como o código é executado, no entanto.


[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample1.cshtml)]

## <a name="examining-the-example-page"></a>Examinando a página de exemplo

A maior parte do que você vê é HTML comum. No entanto, na parte superior, há esse bloco de código:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample2.cshtml)]

Observe o seguinte sobre esse bloco de código:

- O caractere @ informa ao ASP.NET que se trata de código do Razor, não o HTML. ASP.NET irá tratar tudo após o caractere como código até que ele seja executado novamente em algum HTML @. (Nesse caso, que o &lt;! DOCTYPE&gt; elemento.
- As chaves ({e}) incluir um bloco de código do Razor, se o código tem mais de uma linha. As chaves informar ao ASP.NET, onde o código para esse bloco começa e termina.
- O / / caracteres marcam um comentário — ou seja, uma parte do código que não será executada.
- Cada instrução deve terminar com um ponto e vírgula (;). (Não comentários, no entanto.)
- Você pode armazenar valores em *variáveis*, que você criar (*declarar*) com o var de palavra-chave. Quando você cria uma variável, você dê a ele um nome, que pode incluir letras, números e sublinhado (\_). Nomes de variável não podem começar com um número e não é possível usar o nome de uma palavra-chave programação (como var).
- Cadeias de caracteres (como "ASP.NET" e "Páginas da Web") você coloque entre aspas. (Eles devem ser as aspas duplas). Números não estão entre aspas.
- Espaço em branco fora de aspas não importa. Quebras de linha principalmente não importam; a exceção é que você não pode dividir uma cadeia de caracteres entre aspas em linhas. Não importam o recuo e alinhamento.

Algo que não seja óbvio neste exemplo é que todo o código diferencia maiusculas de minúsculas. Isso significa que a soma da variável é uma variável diferente que variáveis que pode ser chamada soma ou soma. Da mesma forma, o var é uma palavra-chave, mas Var não é.

### <a name="objects-and-properties-and-methods"></a>Objetos e propriedades e métodos

Em seguida, há a expressão de DateTime. Now. Em termos simples, a data e hora é uma *objeto*. Um objeto é uma coisa que você pode programar com — uma página, uma caixa de texto, um arquivo, uma imagem, uma solicitação da web, uma mensagem de email, um registro de cliente, etc. Objetos têm uma ou mais *propriedades* que descrevem suas características. Um objeto de caixa de texto tem uma propriedade de texto (entre outros), um objeto de solicitação tem uma propriedade de Url (e outros), uma mensagem de email tem uma propriedade e uma propriedade para, e assim por diante. Objetos também têm *métodos* que são "verbos" podem executar. Você estará trabalhando com objetos muito.

Como você pode ver no exemplo, a data e hora é um objeto que permite que você programa datas e horas. Ele tem uma propriedade chamada agora que retorna a data e hora atuais.

### <a name="using-code-to-render-markup-in-the-page"></a>Usando o código para renderizar a marcação na página

No corpo da página, observe o seguinte:

[!code-html[Main](intro-to-web-pages-programming/samples/sample3.html)]

Novamente, o caractere @ informa ao ASP.NET que o que segue é o código, não o HTML. Na marcação você pode adicionar seguido por uma expressão de código, e ASP.NET irá renderizar o valor de certo expressão nesse momento. No exemplo, @a renderizará tudo o que é ao valor da variável nomeada a, @product renderiza tudo o que está na variável nomeado produto e assim por diante.

Você não está limitado a variáveis, no entanto. Em algumas instâncias aqui, o caractere @ precede uma expressão:

- @(um\*b) renderiza o produto de tudo o que está nas variáveis de um e b. (O \* significa de operador de multiplicação.)
- @(tecnologia + "" + produto) renderiza os valores na tecnologia de variáveis e produto depois concatená-las e adicionar um espaço entre os dois. O operador (+) para concatenar cadeias de caracteres é o mesmo que o operador para adicionar números. ASP.NET normalmente podem indicar se você estiver trabalhando com números ou cadeias de caracteres e faz a coisa certa com o operador +.
- @Request.Url renderiza a propriedade Url do objeto de solicitação. O objeto de solicitação contém informações sobre a solicitação atual do navegador e, claro, a propriedade Url contém a URL da solicitação atual.

O exemplo também foi projetado para mostrar a você o que você pode fazer o trabalho de maneiras diferentes. Você pode fazer cálculos no bloco de código na parte superior, coloca os resultados em uma variável e, em seguida, renderizar a variável na marcação. Ou você pode fazer cálculos em um direito de expressão na marcação. A abordagem usada depende do que você está fazendo e, até certo ponto, em sua própria preferência.

### <a name="seeing-the-code-in-action"></a>Ver o código em ação

Clique no nome do arquivo e, em seguida, escolha **iniciar no navegador**. Você verá a página no navegador com todos os valores e expressões resolvidas na página.

![Página de 'TestRazor' em execução no navegador](intro-to-web-pages-programming/_static/image2.png)

Examinar o código-fonte no navegador.

![Origem da página Razor de teste no navegador](intro-to-web-pages-programming/_static/image3.png)

Como você espera de sua experiência no tutorial anterior, nenhum código Razor estão na página. Tudo que você vê são os valores de exibição real. Quando você executa uma página, você está realmente fazendo uma solicitação ao servidor web que está incorporado ao WebMatrix. Quando a solicitação é recebida, o ASP.NET resolve todos os valores e expressões e processa seus valores na página. Ele então envia a página no navegador.

> [!TIP] 
> 
> **Razor e C#.**
> 
> Até agora, dissemos que você está trabalhando com sintaxe do Razor. Isso é verdadeiro, mas não é a história completa. A linguagem de programação real que você está usando é chamada *c#*. C# foi criado pela Microsoft, há uma década e se tornou uma das linguagens de programação principais para a criação de aplicativos do Windows. Todas as regras que você já viu sobre como nomear uma variável e como criar instruções e assim por diante são, na verdade, todas as regras da linguagem c#.
> 
> Razor se refere mais especificamente a um pequeno conjunto de convenções para como inserir esse código em uma página. Por exemplo, a convenção de usar para marcar o código na página e usando @ {} para inserir um bloco de código é o aspecto de uma página de Razor. Os auxiliares também são considerados parte do Razor. Sintaxe do Razor é usado em mais locais do que apenas em páginas da Web ASP.NET. (Por exemplo, ele é usado nos modos de exibição de MVC do ASP.NET.)
> 
> Mencionamos isso porque se você olhar para obter informações sobre programação páginas da Web ASP.NET, você encontrará muitas referências ao Razor. No entanto, muitos essas referências não se aplicam ao que você está fazendo e, portanto, pode ser confuso. E, na verdade, muitas das suas dúvidas sobre a programação realmente serão sobre o trabalho com c# ou trabalhar com o ASP.NET. Portanto, se você examinar especificamente para obter informações sobre o Razor, talvez não localize as respostas que precisa.


## <a name="adding-some-conditional-logic"></a>Adicionar alguma lógica condicional

Um dos excelentes recursos sobre como usar o código em uma página é que você pode alterar o que acontece com base em várias condições. Nesta parte do tutorial, você irá brincar com algumas maneiras de alterar o que é exibido na página.

O exemplo será simples e um pouco artificial para que possamos nos concentrar na lógica condicional. A página que você vai criar fará isso:

- Mostra texto diferente na página, dependendo se ela for a primeira vez que a página é exibida ou se você clicar em um botão para enviar a página. Esse será o primeiro teste condicional.
- Exiba a mensagem somente se um determinado valor é passado na cadeia de caracteres de consulta da URL (http://...?show=true). Esse será o segundo teste condicional.

No WebMatrix, crie uma página e denomine *TestRazorPart2.cshtml*. (Na faixa de opções, clique em **New**, escolha **CSHTML**, nomeie o arquivo e, em seguida, clique em **Okey**.)

Substitua o conteúdo dessa página com o seguinte:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample4.cshtml)]

O bloco de código na parte superior inicializa uma variável chamada de mensagem com algum texto. No corpo da página, o conteúdo da variável message é exibido dentro de um &lt;p&gt; elemento. A marcação também contém um &lt;entrada&gt; elemento para criar um **enviar** botão.

Execute a página para ver como ele funciona agora. Por enquanto, ele é basicamente uma página estática, mesmo se você clicar na **enviar** botão.

Volte para o WebMatrix. Dentro do bloco de código, adicione o seguinte código realçado *depois de* a linha que inicializa a mensagem:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample5.cshtml?highlight=4-6)]

### <a name="the-if---block"></a>Se o bloco de {}

O que você acabou de adicionar foi um se condição. No código, a condição if tem uma estrutura como este:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample6.cs)]

A condição de teste está entre parênteses. Ele deve ser um valor ou uma expressão que retorna true ou false. Se a condição for verdadeira, o ASP.NET executa a instrução ou instruções que estão dentro das chaves. (Esses são os *, em seguida,* fazem parte do *se-então* lógica.) Se a condição for falsa, o bloco de código será ignorado.

Aqui estão alguns exemplos de condições que você pode testar em um se instrução:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample7.cs)]

Você pode testar variáveis com base nos valores ou contra expressões usando um <em>operador lógico</em> ou <em>operador de comparação</em>: igual a (= =), maior que (&gt;), menor que (&lt;), maior que ou igual a (&gt;=) e menor ou igual a (&lt;=). A! = operador significa não é igual a — por exemplo, se (um! = 0) significa <em>se</em> <em>um</em><em>não for igual a 0</em>.

> [!NOTE]
> Verifique se que você notar que o operador de comparação igual a (= =) não é o mesmo =. O = operador é usado apenas para atribuir valores (var um = 2). Se você combinar esses operadores de, você obterá um erro ou você obterá alguns resultados estranhos.


Para testar se algo for verdade, a sintaxe completa é if(IsDone == true). Mas você também pode usar o atalho if(IsDone). Se não houver nenhum operador de comparação, o ASP.NET pressupõe que você está testando para true.

O operador! operador por si só significa um NOT lógico. Por exemplo, a condição if (! IsPost) significa *se IsPost não é verdade*.

Você pode combinar condições usando um AND lógico (&amp; &amp; operador) ou OR lógico (| | operador). Por exemplo, a última da se as condições na significa que exemplos anteriores *se FileProcessingIsDone não for definido como true displayMessage AND é definido como false*.

### <a name="the-else-block"></a>O bloco else

Uma coisa final sobre se blocos: um se o bloco pode ser seguido por um bloco else. Um bloco else é útil é que você precisa executar um código diferente quando a condição for falsa. Aqui está um exemplo simples:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample8.cs)]

Você verá alguns exemplos nos próximos tutoriais desta série em que é útil usar um bloco else.

### <a name="testing-whether-the-request-is-a-submit-post"></a>Testando se a solicitação é um envio (post)

Há mais, mas vamos voltar ao exemplo a, que tem a condição if(IsPost) {...}. IsPost é, na verdade, uma propriedade da página atual. Na primeira vez que a página é solicitada, IsPost retorna false. No entanto, se você clicar em um botão ou caso contrário, enviar a página — ou seja, postá-lo — IsPost retorna true. Portanto, IsPost permite determinar se você estiver lidando com envio de um formulário. (Em termos de verbos HTTP, se a solicitação é uma operação GET, IsPost retornará false. Se a solicitação é uma operação POST, IsPost retorna true.) Um tutorial posterior, você trabalhará com formulários de entrada, em que esse teste é particularmente útil.

Execute a página. Como esta é a primeira vez que são solicitados a página, consulte "Esta é a primeira vez que você solicitou a página". Essa cadeia de caracteres é o valor que você inicializou a variável de mensagem. Há um teste if(IsPost), mas que retorna false no momento, portanto, o código dentro do if bloquear não será executado.

Clique o **enviar** botão. A página é solicitada novamente. Como antes, a variável de mensagem é definida como "Esta é a primeira vez...". Mas, desta vez, if(IsPost) o teste retorna true, para que a execução do bloco de código dentro do if. O código altera o valor da variável a mensagem para um valor diferente, que é o que é processado na marcação.

Agora, adicione um se a condição na marcação. Abaixo de &lt;p&gt; elemento que contém o **enviar** botão, adicione a seguinte marcação:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample9.cshtml)]

Você está adicionando o código dentro da marcação, portanto, você precisa para começar @. Em seguida, há um se teste semelhante ao que você adicionou anteriormente para cima no bloco de código. Dentro de chaves, no entanto, você está adicionando HTML comum — pelo menos, é comum até chegar ao @DateTime.Now. Isso é outro pouco de código do Razor, então, novamente você precisa adicionar na frente dele.

O ponto aqui é que você pode adicionar se as condições em ambos o bloco de código na parte superior e na marcação. Se você usar um se condição no corpo da página, as linhas dentro do bloco pode ser a marcação ou código. Nesse caso, e assim como ocorre sempre que você misture marcação e código, você precisa usar para deixar claro para o ASP.NET onde está o código.

Execute a página e clique em **enviar**. Neste momento você não apenas ver uma mensagem diferente quando você enviar ("Agora você enviou..."), mas você verá uma nova mensagem que lista a data e hora.

![Página 2 do Razor de teste em execução no navegador com carimbo de hora mostrando após enviar](intro-to-web-pages-programming/_static/image4.png)

### <a name="testing-the-value-of-a-query-string"></a>Testar o valor de uma cadeia de caracteres de consulta

Mais um teste. Neste momento, você adicionará um se o bloco que testa um valor chamado mostram que pode ser passado na cadeia de caracteres de consulta. (Como este: `http://localhost:43097/TestRazorPart2.cshtml?show=true`) você alterará a página para que a mensagem de você ter sido exibindo ("Esta é a primeira vez...", etc.) é exibida somente se o valor de apresentação é true.

Na parte inferior (mas interna), o bloco de código na parte superior da página, adicione o seguinte:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample10.cs)]

A código completo bloco agora, veja como o exemplo a seguir. (Lembre-se de que, quando você copia o código na sua página, o recuo pode parecer diferente. Mas que não afeta como o código é executado.)

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample11.cshtml)]

O novo código no bloco inicializa uma variável chamada showMessage como false. Ele faz um se o teste para procurar um valor na cadeia de caracteres de consulta. Quando você primeiro solicita a página, ele tem uma URL como esta:

`http://localhost:43097/TestRazorPart2.cshtml`

O código determina se a URL contém uma variável chamada show na cadeia de consulta, como a essa versão da URL:

`http://localhost:43097/TestRazorPart2.cshtml`? Mostrar = true

O próprio teste examina a propriedade QueryString do objeto Request. Se a cadeia de caracteres de consulta contém um item denominado show, e se esse item é definido como true, o se bloco é executado e define a variável showMessage como true.

Há um truque aqui, como você pode ver. Como o nome diz, a cadeia de caracteres de consulta é uma cadeia de caracteres. No entanto, você só pode testar para true e false se o valor que você está testando for um valor booliano de (true/false). Antes de testar o valor da variável Mostrar na cadeia de caracteres de consulta, você precisa convertê-lo em um valor booliano. É o que faz o método AsBool — ele usa uma cadeia de caracteres como entrada e o converte em um valor booliano. Claramente, se a cadeia de caracteres for "true", o método AsBool converte esse valor como true. Se o valor da cadeia de caracteres é qualquer outra coisa, AsBool retorna false.

> [!TIP] 
> 
> **Tipos de dados e métodos As()**
> 
> Já dissemos apenas até o momento que, quando você cria uma variável, você use o var de palavra-chave. Isso não é a história inteira, no entanto. Para manipular valores — para adicionar números, ou concatenar cadeias de caracteres, ou comparar datas ou de teste para verdadeiro/falso — c# tem que trabalhar com uma representação interna apropriada do valor. C# pode *geralmente* descobrir o que essa representação deve ser (ou seja, o que *tipo* os dados são) com base no que você está fazendo com os valores. Agora em seguida, no entanto, ele não pode fazer isso. Caso contrário, você precisa ajudar explicitamente indicando como c# devem representar os dados. O método AsBool faz isso — ele diz ao c# que um valor de cadeia de caracteres "true" ou "false" deve ser tratado como um valor booliano. Existem métodos semelhantes para representar cadeias de caracteres como outros tipos Além disso, como AsInt (trate como um número inteiro), AsDateTime (trate como uma data/hora), AsFloat (trate como um número de ponto flutuante) e assim por diante. Quando você usá-los como métodos (), se o c# não pode representar o valor de cadeia de caracteres conforme solicitado, você verá um erro.


Na marcação da página, remova ou comente a esse elemento (aqui é mostrado comentado):

[!code-html[Main](intro-to-web-pages-programming/samples/sample12.html)]

À direita no qual você removido ou comentada que o texto, adicione o seguinte:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample13.cshtml)]

O se o teste afirma que renderizam se a variável showMessage for true, uma &lt;p&gt; elemento com o valor da variável message.

### <a name="summary-of-your-conditional-logic"></a>Resumo de sua lógica condicional

Caso você não está totalmente certo sobre o que você acabou de fazer, aqui está um resumo.

- A variável de mensagem é inicializada para uma cadeia de caracteres padrão ("Isso é a primeira vez...").
- Se a solicitação de página é o resultado de um envio (post), o valor da mensagem é alterado para "Agora você enviou..."
- A variável showMessage é inicializada como false.
- Se a cadeia de caracteres de consulta contém? Mostrar = true, a variável showMessage é definida como true.
- Na marcação, se showMessage for true, uma &lt;p&gt; elemento é processado que mostra o valor da mensagem. (Se showMessage for false, nada será renderizado nesse ponto na marcação.)
- Na marcação, se a solicitação for uma postagem, uma &lt;p&gt; elemento é processado que exibe a data e hora.

Execute a página. Não há nenhuma mensagem, porque showMessage é false, portanto, na marcação, o teste de if(showMessage) retorna false.

Clique em **Enviar**. Você ver a data e hora, mas ainda assim nenhuma mensagem.

Em seu navegador, vá para a caixa de URL e adicione o seguinte ao final da URL:? Mostrar = true e, em seguida, pressione Enter.

![Página 2 do Razor de teste no navegador mostrando a cadeia de caracteres de consulta](intro-to-web-pages-programming/_static/image5.png)

A página é exibida novamente. (Porque você alterou a URL, isso é uma nova solicitação, não um envio.) Clique em **enviar** novamente. A mensagem é exibida novamente, pois é a data e hora.

![Página 2 do Razor de teste após enviar quando há uma cadeia de caracteres de consulta](intro-to-web-pages-programming/_static/image6.png)

Na URL, alterar? Mostrar = true para? mostrar = false e pressione Enter. Envie novamente a página. A página é para como você iniciou — nenhuma mensagem.

Conforme observado anteriormente, a lógica desse exemplo é um pouco artificial. No entanto, se vai surgem em muitas das suas páginas, e levará um ou mais dos formulários que você viu aqui.

## <a name="installing-a-helper-displaying-a-gravatar-image"></a>Instalação de um auxiliar (exibindo uma imagem Gravatar)

Algumas tarefas que as pessoas frequentemente desejam fazer em páginas da web exigem um monte de código ou exigem conhecimento extra. Exemplos: exibindo um gráfico para dados; colocando um Facebook "botão curtir" em uma página. envio de email do seu site; recortando ou redimensionamento de imagens; usando PayPal para o seu site. Para que seja fácil fazer esses tipos de coisas, o ASP.NET Web Pages permite que você use *auxiliares*. Os auxiliares são componentes que você instala para um site e que permitem que você realize tarefas comuns usando apenas algumas linhas de código do Razor.

Páginas da Web do ASP.NET tem alguns auxiliares internos. No entanto, muitos auxiliares estão disponíveis em pacotes (Suplementos) que são fornecidos usando o Gerenciador de pacotes do NuGet. O NuGet permite que você selecione um pacote para instalar e, em seguida, ele cuida de todos os detalhes da instalação.

Nesta parte do tutorial, você instalará um auxiliar que permite que você exiba uma imagem Gravatar ("avatar globalmente reconhecida"). Você saberá mais duas coisas. Uma é como localizar e instalar um auxiliar. Você também aprenderá como um auxiliar facilita fazer alguma coisa, que caso contrário, você precisaria fazer usando um monte de código, que você precisaria escrever por conta própria.

Você pode registrar seu próprios Gravatar no site no Gravatar [ http://www.gravatar.com/ ](http://www.gravatar.com/), mas não é essencial para criar uma conta de Gravatar para executar essa parte do tutorial.

No WebMatrix, clique o **NuGet** botão.

![Caixa de diálogo de galeria do NuGet no WebMatrix](intro-to-web-pages-programming/_static/image7.png)

Isso inicia o Gerenciador de pacotes do NuGet e exibe os pacotes disponíveis. (Nem todos os pacotes são auxiliares; alguns adicionam funcionalidades ao WebMatrix em si, algumas são modelos adicionais e assim por diante.) Você pode receber uma mensagem de erro sobre a incompatibilidade de versão. Você pode ignorar essa mensagem de erro clicando **Okey** e continuar com este tutorial.

![Caixa de diálogo de galeria do NuGet no WebMatrix](intro-to-web-pages-programming/_static/image8.png)

Na caixa de pesquisa, digite "auxiliares do asp.net". O NuGet mostra os pacotes que correspondem aos termos de pesquisa.

![Galeria no WebMatrix, mostrando os pacotes do NuGet](intro-to-web-pages-programming/_static/image9.png)

O ASP.NET Web Helpers Library contém código para simplificar muitas tarefas comuns, incluindo o uso de imagens do Gravatar. Selecione o **ASP.NET Web Helpers Library** de pacote e, em seguida, clique em **instalar** para iniciar o instalador. Selecione **Sim** quando perguntado se você deseja instalar o pacote e aceite os termos para concluir a instalação.

Isso é tudo. O NuGet baixa e instala tudo, incluindo todos os componentes adicionais que podem ser necessários (*dependências*).

Se por algum motivo você precisar desinstalar um auxiliar, o processo é muito semelhante. Clique o **NuGet** , clique no **instalado** guia e, em seguida, selecione o pacote que você deseja desinstalar.

## <a name="using-a-helper-in-a-page"></a>Usando um auxiliar em uma página

Agora, você usará o auxiliar que você acabou de instalar. O processo para adicionar um auxiliar a uma página é semelhante para a maioria dos auxiliares.

No WebMatrix, crie uma página e denomine *GravatarTest.cshml*. (Você está criando uma página especial para o auxiliar de teste, mas você pode usar os auxiliares em qualquer página no seu site).

Dentro de &lt;corpo&gt; elemento, adicione uma &lt;div&gt; elemento. Dentro de &lt;div&gt; elemento, digite isto:

@Gravatar.

O caractere @ é o mesmo caractere que você usou para marcar o código do Razor. **Gravatar** é o objeto auxiliar que você está trabalhando.

Assim que você digita o ponto (.), o WebMatrix exibe uma lista dos *métodos* (funções) que disponibiliza o auxiliar Gravatar:

![Lista do Gravatar auxiliar suspensa do IntelliSense](intro-to-web-pages-programming/_static/image10.png)

Esse recurso é conhecido como *IntelliSense*. Ele ajuda a codificar, oferecendo opções de contexto apropriado. O IntelliSense funciona com HTML, CSS, código ASP.NET, JavaScript e outras linguagens que têm suporte no WebMatrix. Ele é outro recurso que torna mais fácil desenvolver páginas da web no WebMatrix.

Pressionar G no teclado e você verá que o IntelliSense localiza o método GetHtml. Pressione Tab. IntelliSense insere o método selecionado (GetHtml) para você. Digite um parêntese de abertura e observe o parêntese de fechamento é adicionado automaticamente. Digite seu endereço de email entre aspas entre dois parênteses. Se você tiver uma conta do Gravatar, sua imagem de perfil será retornada. Se você não tiver uma conta do Gravatar, uma imagem padrão será retornada. Quando terminar, a linha terá esta aparência:

[!code-css[Main](intro-to-web-pages-programming/samples/sample14.css)]

Agora, exiba a página em um navegador. A imagem ou a imagem padrão é exibida, dependendo se você tiver uma conta de Gravatar.

![Gravatar](intro-to-web-pages-programming/_static/image11.png) ![imagem padrão](intro-to-web-pages-programming/_static/image12.png)

Para obter uma ideia do que o auxiliar está fazendo para você, exiba a origem da página no navegador. Junto com o HTML que você tinha em sua página, você deve ver um elemento de imagem que inclui um identificador. Isso é o código que o auxiliar processado para a página no lugar em que você tinha @Gravatar.GetHtml. O auxiliar levou as informações fornecidas e gerou o código que se comunica diretamente com Gravatar para retornar a imagem correta para a conta fornecida.

O método GetHtml também permite que você personalize a imagem, fornecendo os outros parâmetros. O código a seguir mostra como solicitar uma imagem tem uma largura e altura de 40 pixels e usa uma imagem padrão especificada chamada **wavatar** se a conta especificada não existe.

[!code-javascript[Main](intro-to-web-pages-programming/samples/sample15.js)]

Esse código produz algo parecido com o seguinte resultado (a imagem padrão aleatoriamente irá variar).

![](intro-to-web-pages-programming/_static/image13.png)

## <a name="coming-up-next"></a>Próximo

Para manter esse tutorial curto, tivemos que se concentrar em apenas algumas noções básicas. Naturalmente, há uma *muito* mais Razor e c#. Você aprenderá mais ao percorrer esses tutoriais. Se você estiver interessado em aprender mais sobre os aspectos de programação do Razor e c# no momento, você pode ler uma introdução mais abrangente aqui: [Introdução ao ASP.NET Web de programação usando a sintaxe do Razor](https://go.microsoft.com/fwlink/?LinkID=202890).

O próximo tutorial apresenta a você a trabalhar com um banco de dados. Neste tutorial, você começará criando o aplicativo de exemplo que permite que você Liste seus filmes favoritos.

## <a name="complete-listing-for-testrazor-page"></a>Listagem completa para a página TestRazor

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample16.cshtml)]

## <a name="complete-listing-for-testrazorpart2-page"></a>Listagem completa para a página TestRazorPart2

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample17.cshtml)]

## <a name="complete-listing-for-gravatartest-page"></a>Listagem completa para a página GravatarTest

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample18.cshtml)]

## <a name="additional-resources"></a>Recursos adicionais

- [Introdução à programação Web do ASP.NET usando a sintaxe Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Auxiliar do Twitter](../../ui-layouts-and-themes/twitter-helper.md)

> [!div class="step-by-step"]
> [Anterior](getting-started.md)
> [Próximo](displaying-data.md)
