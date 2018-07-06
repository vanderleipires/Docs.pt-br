---
uid: mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
title: Habilitar o teste de unidade automatizado | Microsoft Docs
author: microsoft
description: Etapa 12 mostra como desenvolver um conjunto de testes de unidade automatizados que verifique nossa funcionalidade NerdDinner, e que nos fornecerá a confiança necessária para fazer alterações...
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: a19ff2ce-3f7e-4358-9a51-a1403da9c63e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
msc.type: authoredcontent
ms.openlocfilehash: 2247dc2e6d22cc0d5ddba97dfe6c7d2d1b0e49be
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819400"
---
<a name="enable-automated-unit-testing"></a>Habilitar testes de unidade automatizados
====================
por [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Esta é a etapa 12 do grátis [aplicativo "NerdDinner"](introducing-the-nerddinner-tutorial.md) que orienta-through como criar um pequeno, mas concluir, o aplicativo web usando ASP.NET MVC 1.
> 
> Etapa 12 mostra como desenvolver um conjunto de testes de unidade automatizados que verifique nossa funcionalidade NerdDinner, e que nos fornecerá a confiança para fazer alterações e aprimoramentos para o aplicativo no futuro.
> 
> Se você estiver usando o ASP.NET MVC 3, recomendamos que você siga a [obtendo iniciado com o MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [Store de música do MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriais.


## <a name="nerddinner-step-12-unit-testing"></a>NerdDinner etapa 12: Teste de unidade

Vamos desenvolver um conjunto de testes de unidade automatizados que verifique nossa funcionalidade NerdDinner, e que nos fornecerá a confiança para fazer alterações e aprimoramentos para o aplicativo no futuro.

### <a name="why-unit-test"></a>Por que teste de unidade?

Na unidade em uma manhã de trabalho, você tem um flash repentino de inspiração sobre um aplicativo que você está trabalhando. Você percebe que há uma alteração que você pode implementar o que tornará significativamente melhor o aplicativo. Pode ser uma refatoração que limpa o código, adiciona um novo recurso ou corrige um bug.

A pergunta que confronts você quando você chega ao seu computador é – "o nível de segurança é tornar essa melhoria?" E se a fazer a alteração tem efeitos colaterais ou quebras de algo? A alteração pode ser simples e levar apenas alguns minutos para implementar, mas e se levar horas para testar manualmente todos os cenários de aplicativo? E se você se esqueça de abordar um cenário e um aplicativo danificado entra em produção? Está fazendo essa melhoria realmente vale todo o esforço?

Testes de unidade automatizados podem fornecer uma rede de segurança que permite aprimorar continuamente seus aplicativos e evitar sendo teme que o código que você está trabalhando. Ter testes automatizados que verificar rapidamente a funcionalidade permite que você desenvolva com confiança – e capacitá-lo a fazer melhorias que você talvez caso contrário, não têm sentido à vontade fazendo. Eles também ajudam a criar soluções de manutenção mais fácil e têm um tempo de vida maior - que leva a um maior retorno do investimento.

O ASP.NET MVC Framework torna fácil e natural para a funcionalidade do aplicativo de teste de unidade. Ele também permite que um fluxo de trabalho de teste controlado por TDD (desenvolvimento) que permite o desenvolvimento de test-first com base.

### <a name="nerddinnertests-project"></a>Projeto NerdDinner.Tests

Quando criamos nosso aplicativo NerdDinner no início deste tutorial, fomos solicitados com uma caixa de diálogo perguntando se queríamos criar um projeto de teste de unidade para acompanharem o projeto de aplicativo:

![](enable-automated-unit-testing/_static/image1.png)

Mantivemos o "Sim, criar um projeto de teste de unidade" botão de opção selecionado – que resultou em um projeto de "NerdDinner.Tests" que está sendo adicionado à nossa solução:

![](enable-automated-unit-testing/_static/image2.png)

O projeto NerdDinner.Tests faz referência ao assembly do projeto de aplicativo NerdDinner e nos permite adicionar facilmente os testes automatizados a ele que verificam a funcionalidade do aplicativo.

### <a name="creating-unit-tests-for-our-dinner-model-class"></a>Criando testes de unidade para nossa classe de modelo de jantar

Vamos adicionar alguns testes ao nosso projeto NerdDinner.Tests que verificam se a classe Dinner que criamos quando criamos nossa camada de modelo.

Vamos começar criando uma nova pasta dentro do nosso projeto de teste chamado "Modelos" em que vamos colocar nossos testes relacionados ao modelo. Vamos, em seguida, clique com botão direito na pasta e escolha o **Add -&gt;novo teste** comando de menu. Isso abrirá a caixa de diálogo "Add New Test".

Vamos escolher criar um teste de unidade"" e nomeie-a como "DinnerTest.cs":

![](enable-automated-unit-testing/_static/image3.png)

Quando clicamos no botão "okey" Visual Studio irá adicionar (e abrir) um arquivo DinnerTest.cs ao projeto:

![](enable-automated-unit-testing/_static/image4.png)

O modelo de teste de unidade do Visual Studio padrão tem um monte de código clichê clichê dentro dele que considero um pouco confuso. Vamos limpá-lo para conter apenas o código a seguir:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample1.cs)]

O atributo [TestClass] na classe DinnerTest acima identifica como uma classe que contém testes, bem como inicialização de teste opcional e código de desmontagem. Podemos definir testes dentro dele com a adição de métodos públicos que têm um atributo [TestMethod] neles.

Abaixo estão o primeiro dos dois testes, adicionaremos que exercitam nossa classe Dinner. O primeiro teste verifica que nossos jantar é inválido, se um jantar novo é criado sem todas as propriedades que estão sendo definidas corretamente. O segundo teste verifica que nossos jantar é válido quando um jantar tem todas as propriedades definidas com valores válidos:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample2.cs)]

Você notará acima que nossos nomes de teste são bastante explícito (e pouco detalhado). Estamos fazendo isso porque podemos terminar a criação de centenas ou milhares de testes de pequena e queremos tornar mais fácil de determinar rapidamente a intenção e o comportamento de cada um deles (especialmente quando estamos buscando por meio de uma lista de falhas em um executor de teste). Os nomes de teste devem ser nomeados depois a funcionalidade que estão testando. Acima estamos usando um "substantivo\_deve\_verbo" padrão de nomenclatura.

Nós são estruturar os testes usando o teste padrão – o que significa "Arrange, Act, Assert" "AAA":

- Organizar: Configurar a unidade que está sendo testada
- O ACT: Exercitar a unidade sob teste e capturar resultados
- Assert: Verificar o comportamento

Ao escrever testes que queremos evitar que os testes individuais fazer muito mais. Em vez disso, cada teste deve verificar somente um único conceito (que torna muito mais fácil de identificar a causa de falhas). É uma boa diretriz tentar e ter apenas uma única instrução para cada teste de asserção. Se você tiver mais de uma instrução em um método de teste de asserção, certifique-se de que eles são todos os que estão sendo usados para testar o mesmo conceito. Em caso de dúvida, faça outro teste.

### <a name="running-tests"></a>Executando testes

Visual Studio 2008 Professional (e edições superiores) incluem um executor de teste internos que pode ser usado para executar projetos dentro do IDE do Visual Studio de teste de unidade. Podemos selecionar o **Test -&gt;Run -&gt;todos os testes na solução** menu comando (ou digite Ctrl-R, uma) para executar todos os nossos testes de unidade. Ou como alternativa, podemos posicionar o cursor de dentro de um método de teste ou classe de teste específico e usar o **Test -&gt;Run -&gt;testes no contexto atual** comando de menu (ou digite Ctrl R, T) para executar um subconjunto dos testes de unidade.

Vamos posicionar nosso cursor dentro da classe DinnerTest e digite "Ctrl R, T" para executar os dois testes que acabamos de definir. Quando fazemos isso uma janela de "Test Results" serão exibido dentro do Visual Studio e vamos ver os resultados de nosso teste listado dentro dele:

![](enable-automated-unit-testing/_static/image5.png)

*Observação: A janela de resultados de teste do VS não mostra a coluna de nome de classe por padrão. Você pode adicionar isso clicando duas vezes dentro da janela de resultados de teste e usando o comando de menu Adicionar/remover colunas.*

Nossos dois testes levaram apenas uma fração de segundo para executar – e como você pode ver ambos passados. Agora podemos ir e aumentá-las com a criação de testes adicionais que verifique se as validações de regra específica, bem como abordam os dois métodos auxiliares - IsUserHost() e IsUserRegisterd() – que adicionamos à classe Dinner. Ter todos esses testes em vigor para a classe Dinner tornará muito mais fácil e segura adicionar novas regras de negócio e as validações a ela no futuro. Podemos adicionar nossa nova lógica de regra para o jantar e, em seguida, em segundos, verifique se que ele ainda não dividido qualquer um dos nossos funcionalidade lógica anterior.

Observe como usando um nome descritivo de teste torna mais fácil entender rapidamente o que cada teste é verificar. É recomendável usar o **ferramentas -&gt;opções** comando de menu, abrindo a testar ferramentas -&gt;tela de configuração de execução de teste e verificação de "duas vezes em um resultado de teste de unidade com falha ou inconclusivo exibe o ponto de falha no teste de"caixa de seleção. Isso permitirá que você clique duas vezes em uma falha na janela de resultados do teste e ir imediatamente para a falha de assert.

### <a name="creating-dinnerscontroller-unit-tests"></a>Criando testes de unidade DinnersController

Agora, vamos criar alguns testes de unidade que verifique a nossa funcionalidade DinnersController. Vamos começar pelo botão direito do mouse na pasta "Controladores de" dentro do nosso projeto de teste e, em seguida, escolha o **Add -&gt;novo teste** comando de menu. Vamos criar um teste de unidade"" e nomeie-a como "DinnersControllerTest.cs".

Vamos criar dois métodos de teste que verificam se o método de ação Details() sobre o DinnersController. O primeiro verificará que uma exibição é retornada quando um jantar existente é solicitada. O segundo verificará que um modo de exibição de "NotFound" é retornado quando um jantar inexistente é solicitado:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample3.cs)]

O código acima é compilado limpo. Quando executamos os testes, no entanto, ocorre falha em ambos:

![](enable-automated-unit-testing/_static/image6.png)

Se observarmos as mensagens de erro, veremos que o motivo da falharam de testes foi porque nossa classe DinnersRepository não pôde se conectar a um banco de dados. Nosso aplicativo NerdDinner está usando uma cadeia de conexão para um arquivo local do SQL Server Express que reside sob o \App\_diretório de dados do projeto de aplicativo NerdDinner. Como nosso projeto NerdDinner.Tests compila e executa-o em um diretório diferente, em seguida, o projeto de aplicativo, o local do caminho relativo da nossa cadeia de conexão está incorreto.

Estamos *poderia* corrigir esse problema, copiando o arquivo de banco de dados SQL Express para nosso projeto de teste e, em seguida, adicione uma cadeia de conexão de teste apropriadas a ele no App. config do nosso projeto de teste. Isso teria os testes acima desbloqueado e em execução.

Unidade testando o código usando um banco de dados real, no entanto, traz uma série de desafios. Especificamente:

- Ele diminui significativamente o tempo de execução de testes de unidade. Quanto mais tempo que leva para executar testes, menos provavelmente são para executá-los com frequência. O ideal é que você deseja que seus testes de unidade para poder ser executado em segundos – e tê-lo algo que você fazer como naturalmente como compilar o projeto.
- Ele complica a lógica de instalação e limpeza em testes. Você deseja que cada teste de unidade seja isolada e independente de outras pessoas (com sem efeitos colaterais ou dependências). Ao trabalhar em relação a um banco de dados real, que você precisa estar atento a estado e redefini-lo entre os testes.

Vamos examinar um padrão de design chamado "injeção de dependência" que pode ajudar a resolver esses problemas e evitar a necessidade de usar um banco de dados real com nossos testes.

### <a name="dependency-injection"></a>Injeção de dependência

No momento DinnersController "acoplamento rígido" com a classe de DinnerRepository. "Acoplamento" refere-se a uma situação em que uma classe explicitamente se baseia em outra classe para trabalhar:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample4.cs)]

Como a classe DinnerRepository requer acesso a um banco de dados, a dependência rigidamente acoplada a classe DinnersController tem nas extremidades DinnerRepository a necessidade de ter um banco de dados para que os métodos de ação DinnersController a ser testado.

Podemos contornar isso, empregando um padrão de design chamado "injeção de dependência –" que é uma abordagem em que dependências (como classes de repositório que fornecem acesso a dados) não é mais implicitamente são criadas dentro de classes que usá-los. Em vez disso, as dependências podem ser passadas explicitamente para a classe que usa-os usando argumentos de construtor. Se as dependências são definidas usando interfaces, em seguida, temos a flexibilidade para passar em implementações de dependência "falsificação" para cenários de teste de unidade. Isso nos permite criar implementações de dependência específica de teste que, na verdade, não exigem acesso a um banco de dados.

Para ver isso em ação, vamos implementar injeção de dependência com nosso DinnersController.

#### <a name="extracting-an-idinnerrepository-interface"></a>Extraindo uma interface de IDinnerRepository

Nossa primeira etapa será criar uma nova interface de IDinnerRepository que encapsula o contrato de repositório que nossos controladores exigem para recuperar e atualizar os jantares.

Podemos definir este contrato de interface manualmente clicando duas vezes na pasta \Models e, em seguida, escolhendo a **Add -&gt;Novo Item** comando de menu e criar uma nova interface chamada IDinnerRepository.cs.

Como alternativa, podemos usar a extração de refatoração de ferramentas integrado no Visual Studio Professional (e edições superiores) automaticamente e criar uma interface para nós de nossa classe DinnerRepository existente. Para extrair essa interface usando o VS, simplesmente posicione o cursor no editor de texto na classe DinnerRepository e, em seguida, clique com botão direito e escolha o **refatorar -&gt;extrair Interface** comando de menu:

![](enable-automated-unit-testing/_static/image7.png)

Isso iniciará a caixa de diálogo "Extrair Interface" e nos solicita o nome da interface para criar. Ele será IDinnerRepository como padrão e selecionar automaticamente todos os métodos públicos na classe DinnerRepository existente para adicionar à interface:

![](enable-automated-unit-testing/_static/image8.png)

Quando clicamos no botão "okey", o Visual Studio adicionará uma nova interface de IDinnerRepository ao nosso aplicativo:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample5.cs)]

E nossa classe DinnerRepository existente será atualizado para que ele implementa a interface:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample6.cs)]

#### <a name="updating-dinnerscontroller-to-support-constructor-injection"></a>Atualizando o DinnersController para dar suporte à injeção de construtor

Agora atualizaremos a classe DinnersController para usar a nova interface.

No momento DinnersController é embutido em código, de modo que seu campo "dinnerRepository" sempre é uma classe de DinnerRepository:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample7.cs)]

Vamos alterá-lo para que o campo "dinnerRepository" é do tipo IDinnerRepository, em vez de DinnerRepository. Em seguida, adicionaremos dois construtores públicos de DinnersController. Um dos construtores permite que um IDinnerRepository a ser passado como um argumento. O outro é um construtor padrão que usa nossa implementação de DinnerRepository existente:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample8.cs)]

Como ASP.NET MVC, por padrão cria classes de controlador usando construtores padrão, o nosso DinnersController em tempo de execução continuará a usar a classe de DinnerRepository para executar o acesso a dados.

Agora podemos atualizar nossos testes de unidade, no entanto, para passar em uma implementação de repositório de jantar "falsificação" usando o parâmetro de construtor. Esse repositório de jantar "falsificação" não exigirão acesso a um banco de dados real e em vez disso, usará dados de exemplo na memória.

#### <a name="creating-the-fakedinnerrepository-class"></a>Criando a classe FakeDinnerRepository

Vamos criar uma classe FakeDinnerRepository.

Vamos começar criando um diretório "Fakes" dentro do nosso projeto NerdDinner.Tests e, em seguida, adicione uma nova classe de FakeDinnerRepository a ele (clique com botão direito na pasta e escolha **Add -&gt;nova classe**):

![](enable-automated-unit-testing/_static/image9.png)

Vamos atualizar o código para que a classe FakeDinnerRepository implementa a interface de IDinnerRepository. Podemos, em seguida, clique com botão direito nele e escolha o comando de menu de contexto "Implementar interface IDinnerRepository":

![](enable-automated-unit-testing/_static/image10.png)

Isso fará com que o Visual Studio adicionar automaticamente todos os membros de interface de IDinnerRepository à nossa classe FakeDinnerRepository com implementações padrão de "criar stub":

[!code-csharp[Main](enable-automated-unit-testing/samples/sample9.cs)]

Em seguida, atualizamos a implementação de FakeDinnerRepository trabalhar fora de uma lista na memória&lt;Dinner&gt; coleção passado para ele como um argumento de construtor:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample10.cs)]

Agora temos uma implementação falsa de IDinnerRepository que não requer um banco de dados e, em vez disso, pode ser executado fora de uma lista na memória de objetos de jantar.

#### <a name="using-the-fakedinnerrepository-with-unit-tests"></a>Usando o FakeDinnerRepository com testes de unidade

Vamos retornar para os testes de unidade DinnersController que falhou anteriormente porque o banco de dados não estava disponível. Podemos atualizar os métodos de teste para usar um FakeDinnerRepository preenchido com dados de jantar de na memória de exemplo para o DinnersController usando o código a seguir:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample11.cs)]

E agora ao executar esses testes ambos passam:

![](enable-automated-unit-testing/_static/image11.png)

O melhor de tudo, eles ocupam apenas uma fração de segundo para executar e não exigem qualquer lógica de instalação/limpeza complicada. Podemos agora teste de unidade todo o nosso código de método de ação DinnersController (incluindo listagem, paginação, detalhes, criar, atualizar e excluir) sem nunca precisarem se conectar a um banco de dados real.

| **Tópico de lado: Estruturas de injeção de dependência** |
| --- |
| Execução de injeção de dependência manual (como acima) funciona bem, mas se tornará mais difícil de manter como o número de dependências e componentes em um aplicativo aumenta. Existem várias estruturas de injeção de dependência para .NET que pode ajudar a fornecer mais flexibilidade de gerenciamento de dependência. Essas estruturas, também chamadas de contêineres de "Inversão de controle" (IoC), fornecem mecanismos que permitem que um nível adicional de suporte de configuração para especificar e passar as dependências para objetos em tempo de execução (geralmente usando injeção de construtor ). Alguns da injeção de dependência de software livre mais populares / IOC estruturas no .NET incluem: Windsor, Ninject, Spring.NET, StructureMap e AutoFac. ASP.NET MVC expõe APIs que permitem que os desenvolvedores a participarem na resolução e instanciação dos controladores, e que permite a injeção de dependência de extensibilidade / estruturas IoC seja integrada corretamente dentro desse processo. Usar uma estrutura DI/IOC seria também nos permitem remover o construtor padrão do nosso DinnersController – o que seria remover completamente o acoplamento entre ele e o DinnerRepositorys. Não será utilizada uma injeção de dependência / estrutura IOC com nosso aplicativo NerdDinner. Mas é algo que podemos considerar a possibilidade para o futuro se os recursos e a base de código do NerdDinner cresceram. |

### <a name="creating-edit-action-unit-tests"></a>Criando testes de unidade de ação Editar

Agora, vamos criar alguns testes de unidade que verificam a funcionalidade de edição do DinnersController. Vamos começar Testando a versão de HTTP GET do nosso Editar ação:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample12.cs)]

Vamos criar um teste que verifica que um modo de exibição apoiado por um objeto DinnerFormViewModel será renderizado novamente quando um jantar válido é solicitado:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample13.cs)]

Quando executamos o teste, no entanto, podemos encontrará falhar porque uma exceção de referência nula é lançada quando o método de edição acessa a propriedade User.Identity.Name para executar a verificação de Dinner.IsHostedBy().

O objeto de usuário na classe base do controlador encapsula os detalhes sobre o usuário conectado e é populado pelo ASP.NET MVC, quando ele cria o controlador em tempo de execução. Porque estamos testando o DinnersController fora de um ambiente de servidor web, o objeto de usuário não está definido (portanto, a exceção de referência nula).

### <a name="mocking-the-useridentityname-property"></a>A propriedade User.Identity.Name de simulação

Estruturas fictícias facilitam os testes, permitindo-nos criar dinamicamente falsas versões de objetos dependentes que dão suporte a nossos testes. Por exemplo, podemos usar uma estrutura de simulação em nosso teste de ação de edição para criar dinamicamente um objeto de usuário que nosso DinnersController pode usar para pesquisar um nome de usuário simulada. Isso evitará uma referência nula sejam lançadas quando executamos nosso teste.

Há muitos .NET estruturas que podem ser usadas com o ASP.NET MVC de simulação (você pode ver uma lista deles aqui: [ http://www.mockframeworks.com/ ](http://www.mockframeworks.com/)). Para testar nosso aplicativo NerdDinner, usaremos uma estrutura chamada "Moq" de simulação de software livre, que pode ser baixado gratuitamente do [ http://www.mockframeworks.com/moq ](http://www.mockframeworks.com/moq).

Após o download, vamos adicionar uma referência em nosso projeto NerdDinner.Tests ao assembly Moq.dll:

![](enable-automated-unit-testing/_static/image12.png)

Em seguida, adicionaremos um método auxiliar de "CreateDinnersControllerAs(username)" à nossa classe de teste que usa um nome de usuário como um parâmetro e que, em seguida, "mocks" a propriedade User.Identity.Name na instância DinnersController:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample14.cs)]

Acima Moq estamos usando para criar um objeto de simulação que simula um objeto ControllerContext (que é o que passa o ASP.NET MVC para classes do controlador para expor objetos de tempo de execução como usuário, a solicitação, a resposta e a sessão). Podemos está chamando o método de "SetupGet" na simulação para indicar que a propriedade HttpContext.User.Identity.Name em ControllerContext deve retornar a cadeia de caracteres de nome de usuário que são passados para o método auxiliar.

Podemos pode simular qualquer número de métodos e propriedades ControllerContext. Para ilustrar isso também adicionei uma chamada SetupGet() para a propriedade Request.IsAuthenticated (que, na verdade, não é necessário para os testes abaixo – mas que ajuda a ilustrar como você pode simular propriedades de solicitação). Quando terminamos nós atribuímos uma instância da imitação ControllerContext ao DinnersController nosso método auxiliar é retornado.

Agora podemos escrever testes de unidade que usam esse método auxiliar para testar cenários de edição que envolvem diferentes usuários:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample15.cs)]

E agora, quando executamos os testes passam:

![](enable-automated-unit-testing/_static/image13.png)

### <a name="testing-updatemodel-scenarios"></a>Cenários de teste UpdateModel()

Nós criamos testes que abrangem a versão HTTP-GET da ação de edição. Agora, vamos criar alguns testes que verificam a versão do HTTP POST da ação Editar:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample16.cs)]

O cenário de teste de interessante novos para nosso suporte com esse método de ação é seu uso do método auxiliar UpdateModel() na classe base do controlador. Estamos usando esse método auxiliar para associar valores de postagem de formulário a nossa instância de objeto do jantar.

Abaixo estão dois testes que demonstra como podemos fornecer de formulário postados valores para o método auxiliar UpdateModel() usado. Vamos fazer isso criando e preenchendo um objeto FormCollection e, em seguida, atribuí-lo à propriedade "ValueProvider" no controlador.

O primeiro teste verifica que, em Salvar com êxito, o navegador é redirecionado para a ação de detalhes. O segundo teste verifica quando uma entrada inválida é lançada a ação exibe novamente a exibição de edição novamente com uma mensagem de erro.


[!code-csharp[Main](enable-automated-unit-testing/samples/sample17.cs)]

### <a name="testing-wrap-up"></a>Teste de encerramento

Já abordamos os conceitos básicos envolvidos nas classes de controlador de teste de unidade. Podemos usar essas técnicas para criar facilmente centenas de testes simples que verificam o comportamento do nosso aplicativo.

Como nosso controlador e os testes de modelo não exigem um banco de dados real, elas são extremamente rápido e fácil de ser executado. Poderemos executar centenas de testes automatizados em segundos e imediatamente obter feedback se uma alteração que são feitas rompeu algo. Isso ajudará a fornecer a confiança para melhorar, refatoração, continuamente e refinar o nosso aplicativo.

Abordamos a testar como o último tópico neste capítulo – mas não porque o teste é algo que você deve fazer no final de um processo de desenvolvimento! Ao contrário, você deve escrever testes automatizados mais cedo possível no processo de desenvolvimento. Fazer então permite que você obtenha comentários imediatos à medida que desenvolve, ajuda você a pensar cuidadosamente sobre cenários de caso de uso do seu aplicativo e orienta você a projetar seu aplicativo com limpa disposição em camadas e acoplamento em mente.

Discutir um capítulo posterior no catálogo de desenvolvimento controlado por testes (TDD) e como usá-lo com o ASP.NET MVC. TDD é uma prática de codificação iterativa no qual você primeiro escreve os testes que atenderão o código resultante. Com o TDD comece cada recurso Criando um teste que verifica a funcionalidade que você está prestes a implementar. Escrevendo a unidade de teste primeiro ajuda a garantir que você entenda claramente o recurso e como ele deve para funcionar. Somente depois que o teste é gravado (e você verificou que ele falha) você em seguida, implementar a funcionalidade real que o teste verifica. Porque você já tenha tempo pensando sobre o caso de uso de como o recurso deve para funcionar, você terá uma melhor compreensão dos requisitos e a melhor maneira de implementá-los. Quando você terminar com a implementação, você pode executar novamente o teste – e obtenha comentários imediatos ao como se o recurso funciona corretamente. Abordaremos o TDD mais no capítulo 10.

### <a name="next-step"></a>Próxima etapa

Alguns final quebra automática de linha para cima de comentários.

> [!div class="step-by-step"]
> [Anterior](use-ajax-to-implement-mapping-scenarios.md)
> [Próximo](nerddinner-wrap-up.md)
