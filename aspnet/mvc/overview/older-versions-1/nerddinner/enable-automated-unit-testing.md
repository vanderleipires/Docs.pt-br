---
uid: mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
title: Habilitar automatizada de teste de unidade | Microsoft Docs
author: microsoft
description: Etapa 12 mostra como desenvolver um conjunto de testes de unidade automatizados que verifique nossa funcionalidade NerdDinner e que fornece a confiança para fazer alterações...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: a19ff2ce-3f7e-4358-9a51-a1403da9c63e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
msc.type: authoredcontent
ms.openlocfilehash: fede08be7e06327c6d04fa5d36f7dd818d79b380
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875624"
---
<a name="enable-automated-unit-testing"></a>Habilitar o teste de unidade automatizado
====================
por [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Esta é a etapa 12 do livremente [aplicativo "NerdDinner"](introducing-the-nerddinner-tutorial.md) que aborda-por meio de como criar um pequeno, mas concluir, o aplicativo web usando o ASP.NET MVC 1.
> 
> Etapa 12 mostra como desenvolver um conjunto de testes de unidade automatizados que verifique nossa funcionalidade NerdDinner e que fornece a confiança para fazer alterações e aprimoramentos para o aplicativo no futuro.
> 
> Se você estiver usando o ASP.NET MVC 3, recomendamos que você siga o [obtendo iniciado com MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [repositório de música MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriais.


## <a name="nerddinner-step-12-unit-testing"></a>NerdDinner etapa 12: Teste de unidade

Vamos desenvolver um conjunto de testes de unidade automatizados que verifique nossa funcionalidade NerdDinner e que fornece a confiança para fazer alterações e aprimoramentos para o aplicativo no futuro.

### <a name="why-unit-test"></a>Por que a unidade de teste?

Na unidade em um dia de trabalho tiver flash repentino de inspirei sobre um aplicativo que você está trabalhando. Você percebe que há uma alteração que você pode implementar que tornam o aplicativo significativamente melhor. Pode ser uma refatoração que limpa o código, adiciona um novo recurso ou corrige um bug.

A pergunta que confronts você quando você chegar ao seu computador é: "como seguro é tornar essa melhoria?" Se a alteração tem efeitos colaterais ou quebras de algo? A alteração pode ser simples e só levar alguns minutos para implementar, mas e se levar horas para testar manualmente todos os cenários de aplicativo? E se você se esquecer de aborda um cenário e um aplicativo danificado entra em produção? Está fazendo essa melhoria realmente valha a pena?

Testes de unidade automatizados podem fornecer uma rede de segurança que permite aprimorar continuamente seus aplicativos e evitar sendo medo do código que você está trabalhando. Tendo automatizada testes que verifiquem rapidamente a funcionalidade permite que você com confiança – o código e ajudá-lo a fazer melhorias pode caso contrário, não ter sensação confortável fazendo. Eles também ajudam a criar soluções que são mais passível de manutenção e têm um tempo de vida mais - que leva a uma maior retorno sobre o investimento.

A estrutura do ASP.NET MVC torna fácil e natural para a funcionalidade do aplicativo de teste de unidade. Ele também permite que um fluxo de trabalho de teste controlado por TDD (desenvolvimento) que permite o desenvolvimento baseado em teste primeiro.

### <a name="nerddinnertests-project"></a>NerdDinner.Tests Project

Quando criamos o nosso aplicativo NerdDinner no início deste tutorial, vamos solicitados com uma caixa de diálogo perguntando se quiséssemos criar um projeto de teste de unidade para ir junto com o projeto de aplicativo:

![](enable-automated-unit-testing/_static/image1.png)

Mantivemos "Sim, crie um projeto de teste de unidade" botão de opção selecionado – que resultou em um projeto de "NerdDinner.Tests" que está sendo adicionado à nossa solução:

![](enable-automated-unit-testing/_static/image2.png)

O projeto NerdDinner.Tests referencia o assembly de projeto de aplicativo NerdDinner e permite adicionar facilmente testes automatizados que verificar a funcionalidade do aplicativo.

### <a name="creating-unit-tests-for-our-dinner-model-class"></a>Criando testes de unidade para nossa classe de modelo refeição

Vamos adicionar alguns testes ao nosso projeto NerdDinner.Tests que verifique se a classe refeição criadas quando criamos nossa camada de modelo.

Vamos começar criando uma nova pasta em nosso projeto de teste chamado "Modelos" onde podemos colocar nossos testes relacionados ao modelo. Vamos, em seguida, clique com botão direito na pasta e escolha o **Add -&gt;novo teste** comando de menu. Isso abrirá a caixa de diálogo "Adicionar novo teste".

Vamos escolher criar um teste de unidade"" e nomeie-a como "DinnerTest.cs":

![](enable-automated-unit-testing/_static/image3.png)

Quando, clique no botão "okey" Visual Studio irá adicionar (e abrir) um arquivo DinnerTest.cs ao projeto:

![](enable-automated-unit-testing/_static/image4.png)

O modelo de teste de unidade de Visual Studio padrão tem uma série de códigos de placa clichê dentro dele que localizar um pouco confuso. Vamos limpá-la para conter apenas o código a seguir:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample1.cs)]

O atributo [TestClass] na classe DinnerTest acima identifica como uma classe que contém testes, bem como a inicialização de teste opcional e código de desmontagem. Podemos definir testes dentro dele com a adição de métodos públicos que têm um atributo [TestMethod] neles.

Abaixo estão o primeiro dos dois testes, adicionaremos que exercitam nossa classe refeição. O primeiro teste verifica se o nosso refeição é inválida se uma refeição novo é criada sem todas as propriedades que estão sendo definidas corretamente. O segundo teste verifica se o nosso refeição é válida quando uma refeição tem todas as propriedades definidas com valores válidos:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample2.cs)]

Você observará acima que nossos nomes de teste são muito explícitas (e um pouco detalhado). Estamos fazendo isso porque estamos pode acabar criando centenas ou milhares de testes de pequena e queremos facilitar a determinar rapidamente a intenção e o comportamento de cada um deles (especialmente quando estamos procurando em uma lista de falhas em um executor de teste). Os nomes de teste devem ser chamados após a funcionalidade que estão sendo testado. Acima estamos usando um "substantivo\_devem\_verbo" padrão de nomenclatura.

Podemos estruturação os testes usando o teste padrão – o que significa "Arrange, Act, Assert" "AAA":

- Organizar: Configurar a unidade que está sendo testada
- ACT: Utilizar a unidade de teste e capturar resultados
- Assert: Verificar o comportamento

Quando escrevemos testes que desejamos para evitar que os testes individuais fazer muito. Em vez disso, cada teste deve verificar somente um único conceito (que torna muito mais fácil de identificar a causa de falhas). Uma boa diretriz é tentar ter apenas uma única instrução para cada teste de asserção. Se você tiver mais de uma instrução em um método de teste de asserção, certifique-se de que eles são todos os que está sendo usados para testar o mesmo conceito. Em caso de dúvida, verifique outro teste.

### <a name="running-tests"></a>Executando testes

Visual Studio 2008 Professional (e edições superiores) incluem um executor de testes internos que pode ser usado para executar projetos do Visual Studio de teste de unidade dentro do IDE. Podemos selecionar a **teste -&gt;execução&gt;todos os testes na solução** menu comando (ou tipo Ctrl R, A) para executar todos os testes de unidade. Ou como alternativa, pode posicionar nosso cursor dentro de um método de teste ou classe de teste específicos e usar o **teste -&gt;execução&gt;testes no contexto atual** comando de menu (ou tipo Ctrl R, T) para executar um subconjunto dos testes de unidade.

Vamos posicionar nosso cursor dentro da classe DinnerTest e digite "Ctrl R, T" para executar os dois testes que acabamos de definir. Quando fazemos isso uma janela "Resultados de teste" serão exibido dentro do Visual Studio e você verá os resultados de nosso teste listados nele:

![](enable-automated-unit-testing/_static/image5.png)

*Observação: A janela de resultados de teste do VS não mostra a coluna de nome de classe por padrão. Você pode adicionar isso clicando duas vezes dentro da janela de resultados de teste e usando o comando de menu Adicionar/remover colunas.*

Nossos dois testes levaram apenas uma fração de segundo para executar – e como você pode ver ambos passados. Agora podemos ir e aumentá-las com a criação de testes extras que verifique se a validação de regra específica, bem como para cobrem os dois métodos auxiliares - IsUserHost() e IsUserRegisterd() – que adicionamos à classe refeição. Ter todos esses testes em vigor para a classe refeição tornará muito mais fácil e segura adicionar novas regras de negócio e validações no futuro a ela. Podemos adicionar nossa nova lógica de regra a uma refeição e, em seguida, em segundos, verifique se que ela não foi quebrada qualquer um dos nossos funcionalidade lógica anterior.

Observe como usar um nome de teste descritivo facilita a compreender rapidamente o que cada teste é verificar. É recomendável usar o **ferramentas -&gt;opções** comando de menu, abrindo o teste ferramentas -&gt;tela de configuração de execução de teste e verificação de "clicando duas vezes em um resultado de teste de unidade com falha ou inconclusivo exibe caixa de seleção do ponto de falha no teste". Isso permitirá que você clique duas vezes em uma falha na janela de resultados de teste e pular imediatamente para a falha de declaração.

### <a name="creating-dinnerscontroller-unit-tests"></a>Criar testes de unidade DinnersController

Agora vamos criar alguns testes de unidade que verifique a nossa funcionalidade DinnersController. Vamos começar clicando na pasta "Controladores" em nosso projeto de teste e, em seguida, escolha o **Add -&gt;novo teste** comando de menu. Vamos criar um teste de unidade"" e nomeie-a como "DinnersControllerTest.cs".

Vamos criar dois métodos de teste que verifique se o método de ação Details() sobre o DinnersController. O primeiro verificará se uma exibição é retornada quando uma refeição existente é solicitada. O segundo verificará se um modo de exibição de "NotFound" é retornado quando é solicitada uma refeição inexistente:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample3.cs)]

O código acima compila limpo. Quando executamos os testes, no entanto, ambas falharem:

![](enable-automated-unit-testing/_static/image6.png)

Se observarmos as mensagens de erro, veremos que o motivo da falharam de testes foi porque nossa classe DinnersRepository não pôde se conectar a um banco de dados. Nosso aplicativo NerdDinner está usando uma cadeia de conexão para um arquivo local do SQL Server Express que reside sob o \App\_diretório de dados do projeto de aplicativo NerdDinner. Como nosso projeto NerdDinner.Tests compila e executa-o em um diretório diferente, em seguida, o projeto de aplicativo, o local do caminho relativo da nossa cadeia de conexão está incorreto.

Estamos *foi* corrigir esse problema, copie o arquivo de banco de dados SQL Express ao nosso projeto de teste e, em seguida, adicionar uma cadeia de conexão de teste apropriado para ele no App. config de nosso projeto de teste. Isso teria os testes acima desbloqueado e em execução.

Código usando um banco de dados real, o teste de unidade, traz inúmeros desafios. Especificamente:

- Ela reduz significativamente o tempo de execução de testes de unidade. Quanto mais tempo que leva para executar testes, menor será a probabilidade são executá-los com frequência. Idealmente, você deseja os testes de unidade para poder ser executado em segundos – e tê-lo algo você como naturalmente como compilar o projeto.
- Ele complica a instalação e lógica de limpeza em testes. Você deseja que cada teste de unidade ser isolado e independente de outras pessoas (com sem efeitos colaterais ou dependências). Ao trabalhar em um banco de dados real, que você precisa estar atento ao estado e redefini-lo entre testes.

Vamos examinar um padrão de design chamado "injeção de dependência" que pode nos ajudar a resolver esses problemas e evitar a necessidade de usar um banco de dados real com nossos testes.

### <a name="dependency-injection"></a>Injeção de dependência

Agora DinnersController é "acoplado" para a classe DinnerRepository. "Acoplamento" refere-se a uma situação em que uma classe explicitamente depende de outra classe para funcionar:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample4.cs)]

Como a classe DinnerRepository requer acesso a um banco de dados, a dependência firmemente acoplada a classe DinnersController tem nas extremidades DinnerRepository a necessidade de ter um banco de dados para que os métodos de ação DinnersController a ser testada.

Podemos contornar isso empregando um padrão de design chamado "injeção de dependência –" que é uma abordagem onde dependências (como repositório classes que fornecem acesso a dados) não são mais implicitamente são criadas dentro de classes que usá-los. Em vez disso, as dependências podem ser passadas explicitamente para a classe que usa-los usando os argumentos de construtor. Se as dependências são definidas usando as interfaces, em seguida, temos a flexibilidade para passar em implementações de dependência "falso" para cenários de teste de unidade. Isso nos permite criar implementações de dependência específicas de teste que realmente não exigem acesso a um banco de dados.

Para ver isso em ação, vamos implementar injeção de dependência com nosso DinnersController.

#### <a name="extracting-an-idinnerrepository-interface"></a>Extraindo uma interface IDinnerRepository

A primeira etapa será criar uma nova interface IDinnerRepository que encapsula o contrato de repositório que nosso controladores necessárias para recuperar e atualizar jantares.

Podemos definir essa interface de contrato manualmente clicando duas vezes na pasta \Models e, em seguida, escolhendo o **Add -&gt;Novo Item** comando de menu e criar uma nova interface chamada IDinnerRepository.cs.

Também podemos usar a refatoração automaticamente ferramentas integrado ao Visual Studio Professional (e edições superiores) para extração e criar uma interface para nós de nossa classe DinnerRepository existente. Para extrair essa interface usando o VS, simplesmente posicione o cursor no editor de texto na classe DinnerRepository e, em seguida, clique com botão direito e escolha o **refatorar -&gt;extrair Interface** comando de menu:

![](enable-automated-unit-testing/_static/image7.png)

Isso iniciará a caixa de diálogo "Extrair Interface" e nos solicitar o nome da interface para criar. Ele será o padrão para IDinnerRepository e selecionar automaticamente todos os métodos públicos da classe DinnerRepository existente para adicionar à interface:

![](enable-automated-unit-testing/_static/image8.png)

Quando é clicar no botão "okey", o Visual Studio adicionará uma nova interface IDinnerRepository para nosso aplicativo:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample5.cs)]

E nossa classe DinnerRepository existente será atualizado para que ele implementa a interface:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample6.cs)]

#### <a name="updating-dinnerscontroller-to-support-constructor-injection"></a>Atualizando DinnersController para dar suporte a injeção de construtor

Agora vamos atualizar a classe DinnersController para usar a nova interface.

No momento DinnersController é inserido no código, de modo que seu campo "dinnerRepository" é sempre uma classe DinnerRepository:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample7.cs)]

Vamos alterá-lo para que o campo "dinnerRepository" é do tipo IDinnerRepository em vez de DinnerRepository. Em seguida, adicionaremos dois construtores DinnersController públicas. Um dos construtores permite que um IDinnerRepository a ser passado como um argumento. A outra é um construtor padrão que usa nossa implementação DinnerRepository existente:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample8.cs)]

Como o ASP.NET MVC por padrão cria classes do controlador usando construtores padrão, nosso DinnersController em tempo de execução continuará a usar a classe DinnerRepository para executar o acesso a dados.

Agora podemos atualizar nossos testes de unidade, no entanto, para passar por uma implementação do repositório de refeição "falso" usando o construtor de parâmetro. Esse repositório refeição "falso" não requer acesso a um banco de dados real e em vez disso, usará dados de exemplo na memória.

#### <a name="creating-the-fakedinnerrepository-class"></a>Criando a classe FakeDinnerRepository

Vamos criar uma classe FakeDinnerRepository.

Vamos começar criando um diretório "Fakes" em nosso projeto NerdDinner.Tests e, em seguida, adicione uma nova classe de FakeDinnerRepository a ele (com o botão direito na pasta e escolha **Add -&gt;nova classe**):

![](enable-automated-unit-testing/_static/image9.png)

Vamos atualizar o código de forma que a classe FakeDinnerRepository implementa a interface IDinnerRepository. Podemos, em seguida, clique com botão direito nela e escolha o comando de menu de contexto "Implementar interface IDinnerRepository":

![](enable-automated-unit-testing/_static/image10.png)

Isso fará com que o Visual Studio adicionar todos os membros de interface IDinnerRepository automaticamente a nossa classe FakeDinnerRepository com implementações padrão de "stub":

[!code-csharp[Main](enable-automated-unit-testing/samples/sample9.cs)]

Em seguida, podemos atualizar a implementação do FakeDinnerRepository trabalham com uma lista de memória&lt;refeição&gt; coleção passados a ele como um argumento de construtor:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample10.cs)]

Agora temos uma implementação IDinnerRepository falsa que não requer um banco de dados e, em vez disso, pode ser executado fora de uma lista de memória de objetos de uma refeição.

#### <a name="using-the-fakedinnerrepository-with-unit-tests"></a>Usando o FakeDinnerRepository com testes de unidade

Vamos voltar para os testes de unidade DinnersController que falharam anteriormente porque o banco de dados não estava disponível. Podemos atualizar os métodos de teste para usar um FakeDinnerRepository preenchido com dados de uma refeição do exemplo na memória para o DinnersController usando o código a seguir:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample11.cs)]

E agora durante a execução desses testes ambos passam:

![](enable-automated-unit-testing/_static/image11.png)

O melhor de tudo, eles ocupam apenas uma fração de segundo para executar e não exigem qualquer lógica de instalação/limpeza complicadas. Podemos agora teste de unidade todo nosso código de método de ação DinnersController (incluindo listagem, paginação, detalhes, criar, atualizar e excluir) sem jamais precisar se conectar a um banco de dados real.

| **Lado tópico: Estruturas de injeção de dependência** |
| --- |
| Executar a injeção de dependência manual (como estamos acima) funciona bem, mas se tornar mais difícil de manter como o número de dependências e aumenta a componentes em um aplicativo. Existem várias estruturas de injeção de dependência para .NET que pode ajudar a fornecer mais flexibilidade de gerenciamento de dependência. Essas estruturas, também chamadas de contêineres de "Inversão de controle" (IoC), fornecem mecanismos que permitem que um nível adicional de suporte de configuração para especificar e passar as dependências para objetos em tempo de execução (geralmente usando injeção de construtor ). Alguns da injeção de dependência de sistemas operacionais mais populares / estruturas IOC no .NET incluem: AutoFac, Ninject, Spring.NET, StructureMap e Windsor. ASP.NET MVC expõe APIs que permitem aos desenvolvedores participar a resolução e a instanciação de controladores e que permite que a injeção de dependência de extensibilidade / estruturas IoC corretamente seja integrado no processo. Usando uma estrutura DI/IOC também permitiria a remover o construtor padrão de nosso DinnersController – o que seria remover completamente o acoplamento entre ele e o DinnerRepositorys. Nós não irá usar uma injeção de dependência / estrutura IOC com nosso aplicativo NerdDinner. Mas isso é algo que é possível considerar no futuro se os recursos e a base de código NerdDinner cresceram. |

### <a name="creating-edit-action-unit-tests"></a>Criar testes de unidade de ação de edição

Agora vamos criar alguns testes de unidade para verificam a funcionalidade de edição do DinnersController. Vamos começar Testando a versão de HTTP GET do nosso Editar ação:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample12.cs)]

Vamos criar um teste que verifica se uma exibição com o apoio de um objeto DinnerFormViewModel é renderizada quando for solicitada uma refeição válida:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample13.cs)]

Durante a execução de teste, no entanto, descobriremos que falha porque uma exceção de referência nula é gerada quando o método de edição acessa a propriedade User.Identity.Name ao executar a verificação de Dinner.IsHostedBy().

O objeto de usuário na classe base do controlador encapsula os detalhes sobre o usuário que fez logon e é populado pelo ASP.NET MVC quando ele cria o controlador em tempo de execução. Como estamos testando DinnersController fora de um ambiente de servidor web, o objeto de usuário não está definido (portanto, a exceção de referência nula).

### <a name="mocking-the-useridentityname-property"></a>Simulação a propriedade User.Identity.Name

Estruturas fictícias facilitar os testes, permitindo a criação dinamicamente falsas versões dos objetos dependentes que dão suporte a nossos testes. Por exemplo, podemos usar uma estrutura fictícia em nosso teste da ação de edição para criar dinamicamente um objeto de usuário que nosso DinnersController pode usar para pesquisar um nome de usuário simulada. Isso evitará uma referência nula de ser lançada quando executamos nosso teste.

Há muitos .NET que podem ser usadas com o ASP.NET MVC estruturas fictícias (você pode ver uma lista deles: [ http://www.mockframeworks.com/ ](http://www.mockframeworks.com/)). Para testar o nosso aplicativo NerdDinner, usaremos um código-fonte aberto fictícias framework chamado "Moq", que pode ser baixado gratuitamente do [ http://www.mockframeworks.com/moq ](http://www.mockframeworks.com/moq).

Após o download, vamos adicionar uma referência em nosso projeto NerdDinner.Tests ao assembly Moq.dll:

![](enable-automated-unit-testing/_static/image12.png)

Em seguida, adicionaremos um método auxiliar de "CreateDinnersControllerAs(username)" à nossa classe de teste que aceita um nome de usuário como um parâmetro e que, em seguida, a propriedade User.Identity.Name na instância DinnersController "mocks":

[!code-csharp[Main](enable-automated-unit-testing/samples/sample14.cs)]

Acima estamos usando Moq para criar um objeto de simulação que simula a um objeto de ControllerContext (que é que o ASP.NET MVC passa para classes do controlador para expor os objetos de tempo de execução como usuário, a solicitação, a resposta e a sessão). Estamos ligando o método "SetupGet" na simulação para indicar que a propriedade HttpContext.User.Identity.Name em ControllerContext deve retornar a cadeia de caracteres de nome de usuário que são passados para o método auxiliar.

É possível simular qualquer número de métodos e propriedades de ControllerContext. Para ilustrar isso também ter adicionado uma chamada SetupGet() para a propriedade Request.IsAuthenticated (que, na verdade, não é necessária para os testes abaixo – mas que ajuda a ilustrar como você pode simular propriedades de solicitação). Quando terminar de nós atribuímos uma instância de simulação ControllerContext ao DinnersController nosso método auxiliar retorna.

Agora podemos escrever testes de unidade que usam esse método auxiliar para testar cenários de edição que envolvem usuários diferentes:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample15.cs)]

E agora ao executar os testes que passam:

![](enable-automated-unit-testing/_static/image13.png)

### <a name="testing-updatemodel-scenarios"></a>Cenários de teste UpdateModel()

Criamos testes que abrangem a versão HTTP GET da ação de edição. Agora vamos criar alguns testes que verificam a versão HTTP POST da ação de edição:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample16.cs)]

O cenário de teste novos interessante para nosso suporte com esse método de ação é o uso do método auxiliar UpdateModel() na classe base do controlador. Estamos usando esse método auxiliar para associar valores de postagem de formulário para a instância de objeto refeição.

Abaixo estão dois testes que demonstra como podermos fornecer formulário postado valores para o método auxiliar UpdateModel() usar. Vamos fazer isso criando e populando um objeto FormCollection e, em seguida, atribuí-la à propriedade "ValueProvider" no controlador.

O primeiro teste verifica em uma gravação com êxito o navegador é redirecionado para a ação de detalhes. O segundo teste verifica quando uma entrada inválida é lançada a ação exibe novamente a exibição de edição novamente com uma mensagem de erro.


[!code-csharp[Main](enable-automated-unit-testing/samples/sample17.cs)]

### <a name="testing-wrap-up"></a>Teste de encerramento

Falamos sobre os conceitos básicos envolvidos em classes de controlador de teste de unidade. Podemos usar essas técnicas para criar facilmente centenas de testes simples que verificam o comportamento de nosso aplicativo.

Como o nosso controlador e os testes de modelo não exigem um banco de dados real, eles são extremamente rápido e fácil de ser executada. Poderemos executar centenas de testes automatizados em segundos, e imediatamente obter feedback se uma alteração que são feitas rompeu algo. Isso ajuda a fornecer a confiança para melhorar, refatorar e refinar o nosso aplicativo continuamente.

Abordamos teste como o último tópico neste capítulo – mas não como o teste é algo que você deve fazer ao final de um processo de desenvolvimento! Ao contrário, você deve escrever testes automatizados mais cedo possível no processo de desenvolvimento. Fazer permite que você obtenha comentários imediatos à medida que desenvolve, ajuda você a ponderação pensar sobre cenários de caso de uso do aplicativo e orienta você para projetar seu aplicativo com limpa dispondo em camadas e acoplamento em mente.

Um capítulo posterior no catálogo de discutir desenvolvimento controlado por testes (TDD) e como usá-lo com o ASP.NET MVC. TDD é uma prática de codificação iterativa onde você primeiro escrever os testes que satisfazem o código resultante. Com TDD começar cada recurso, criando um teste que verifica a funcionalidade que você está prestes a implementar. Gravar a unidade de teste primeiro ajuda a garantir que você compreenda claramente o recurso e como ele deve para funcionar. Somente depois que o teste é gravado (e verificar se ele falhar) você e implementar a funcionalidade real que verifica o teste. Porque você já tenha tempo pensando sobre o caso de uso de como o recurso deve para funcionar, você terá uma melhor compreensão dos requisitos e a melhor maneira de implementá-los. Quando você terminar com a implementação, você pode executar novamente o teste – e obter feedback imediato e se o recurso funciona corretamente. Abordaremos TDD mais no capítulo 10.

### <a name="next-step"></a>Próxima etapa

Alguns conclusão final comentários.

> [!div class="step-by-step"]
> [Anterior](use-ajax-to-implement-mapping-scenarios.md)
> [Próximo](nerddinner-wrap-up.md)
