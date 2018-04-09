---
uid: mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-cs
title: 'Testes de unidade de criação de iteração #5 – (c#) | Microsoft Docs'
author: microsoft
description: Na quinta iteração, fazemos nosso aplicativo mais fácil de manter e modificar adicionando testes de unidade. Vamos simular nossas classes de modelo de dados e criar testes de unidade para o...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: 28ad8f80-b8a5-444e-b478-8b15a846060c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-cs
msc.type: authoredcontent
ms.openlocfilehash: 7a61b5791a40088df9d27f7b1bd37df1831ef22b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="iteration-5--create-unit-tests-c"></a>Iteração #5 – criar testes de unidade (c#)
====================
por [Microsoft](https://github.com/microsoft)

[Baixar o código](iteration-5-create-unit-tests-cs/_static/contactmanager_5_cs1.zip)

> Na quinta iteração, fazemos nosso aplicativo mais fácil de manter e modificar adicionando testes de unidade. Vamos simular nossas classes de modelo de dados e criar testes de unidade para nossos controladores e lógica de validação.


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Criando um aplicativo ASP.NET MVC de gerenciamento de contatos (c#)

Esta série de tutoriais, vamos criar um aplicativo de gerenciamento de entre em contato com toda a partir do início ao fim. O aplicativo Gerenciador de entrar em contato com permite armazenar informações de contato - nomes, números de telefone e endereços de email - para obter uma lista de pessoas.

Vamos criar o aplicativo em várias iterações. Com cada iteração, podemos melhorar gradualmente o aplicativo. O objetivo dessa abordagem iteração vários é para que você possa entender o motivo para cada alteração.

- Iteração #1 - criar o aplicativo. A primeira iteração, criamos o gerente do contato da maneira mais simples possível. Adicionamos suporte para operações de banco de dados básicos: criar, leitura, atualização e exclusão (CRUD).

- Iteração #2 - Verifique o aplicativo parecer adequado. Essa iteração, podemos melhorar a aparência do aplicativo, modificando a página mestra do ASP.NET MVC exibição padrão e em cascata a folha de estilos.

- Iteração #3 - adicionar validação do formulário. Na terceira iteração, podemos adicionar validação de forma básica. Vamos impedir que pessoas enviando um formulário sem concluir os campos obrigatórios do formulário. Além disso, podemos validar endereços de email e números de telefone.

- Iteração #4 - Verifique o aplicativo acoplados de forma flexível. Essa terceira iteração, podemos aproveitar vários padrões de design de software para facilitar a manutenção e modificar o aplicativo Gerenciador de contato. Por exemplo, podemos refatorar nosso aplicativo para usar o padrão de repositório e o padrão de injeção de dependência.

- Iteração #5 - criar testes de unidade. Na quinta iteração, fazemos nosso aplicativo mais fácil de manter e modificar adicionando testes de unidade. Vamos simular nossas classes de modelo de dados e criar testes de unidade para nossos controladores e lógica de validação.

- Iteração #6 - Use desenvolvimento controlado por testes. Essa iteração do sexto, adicionamos novas funcionalidades para nosso aplicativo escrevendo testes de unidade primeiro e escrever código os testes de unidade. Essa iteração, adicionamos grupos de contatos.

- Iteração #7 - adicionar funcionalidade Ajax. A sétima iteração, podemos melhorar a capacidade de resposta e o desempenho do nosso aplicativo, adicionando suporte para Ajax.


## <a name="this-iteration"></a>Essa iteração

Na iteração anterior do aplicativo Gerenciador de contato, podemos refatorar o aplicativo para ser acoplados de forma mais flexível. Separamos o aplicativo em controlador distinto, serviço e camadas de repositório. Cada camada interage com a camada abaixo dela através de interfaces.

Podemos refatorar o aplicativo para tornar o aplicativo mais fácil de manter e modificar. Por exemplo, se for necessário usar uma nova tecnologia de acesso de dados, podemos simplesmente alterar a camada de repositório sem tocar o controlador ou a camada de serviço. Fazendo o gerente do contato acoplados de forma flexível, podemos ve feitas o aplicativos mais resiliente a alterar.

Mas, o que acontece quando precisamos adicionar um novo recurso para o aplicativo Gerenciador de contato? Ou, o que acontece quando podemos corrigir um bug? Uma verdade sad, mas também comprovada de escrever código que é sempre que você tocar o código que você cria o risco de introduzir novos bugs.

Por exemplo, um dia, o gerente pode solicitar que você adicionar um novo recurso para o gerente do contato. Ela quer que você adicionar suporte para grupos de contatos. Ela quer que você habilitar usuários para organizar seus contatos em grupos, como amigos, negócios e assim por diante.

Para implementar esse novo recurso, você precisará modificar todas as três camadas do aplicativo Gerenciador de contato. Você precisará adicionar nova funcionalidade para os controladores, a camada de serviço e o repositório. Assim que você começa a modificar o código, você corre o risco de interromper a funcionalidade que funcionavam antes.

Refatoração nosso aplicativo em camadas separadas, como foi a iteração anterior, é bom. Ele foi um bom porque ele nos permite fazer alterações em camadas inteiras sem tocar o restante do aplicativo. No entanto facilitar a manutenção e modificar o código dentro de uma camada, você precisa criar testes de unidade para o código.

Você usar uma unidade de teste para testar uma unidade individual de código. Essas unidades de código são menores que as camadas de aplicativo inteiro. Normalmente, você deve usar um teste de unidade para verificar se um método específico em seu código se comporta da maneira esperada. Por exemplo, você criaria um teste de unidade para o método CreateContact() exposto pela classe ContactManagerService.

Os testes de unidade para um trabalho de aplicativo apenas como uma rede de segurança. Sempre que modificar o código em um aplicativo, você pode executar um conjunto de testes de unidade para verificar se a modificação interrompe a funcionalidade existente. Testes de unidade tornam seguro para modificar o seu código. Testes de unidade fazer todo o código em seu aplicativo mais resiliente a alterar.

Essa iteração, adicionamos testes de unidade para nosso aplicativo Gerenciador de contato. Dessa forma, na próxima iteração, podemos adicionar grupos de contato para nosso aplicativo sem se preocupar em interromper a funcionalidade existente.

> [!NOTE] 
> 
> Há uma variedade de estruturas, incluindo NUnit xUnit.net e MbUnit de teste de unidade. Neste tutorial, usamos o framework incluído com o Visual Studio de teste de unidade. No entanto, você pode facilmente usar uma dessas estruturas alternativas.


## <a name="what-gets-tested"></a>O que é testado

No mundo ideal, todo o código seria coberto por testes de unidade. No mundo ideal, você teria a rede de segurança perfeita. Você poderá modificar qualquer linha de código em seu aplicativo e saiba instantaneamente, executando os testes de unidade, se a alteração rompeu a funcionalidade existente.

No entanto, podemos don t ao vivo em um mundo perfeito. Na prática, ao escrever testes de unidade, você se concentrar em escrever testes para sua lógica de negócios (por exemplo, a lógica de validação). Em particular, você *não* gravar testes de unidade para seus dados acessar lógica ou sua lógica de exibição.

Para ser útil, testes de unidade devem ser executado muito rapidamente. Facilmente você pode acumular centenas (ou até mesmo milhares) de testes de unidade para um aplicativo. Se os testes de unidade levar muito tempo para ser executado, você evitará executá-los. Em outras palavras, em tempo de execução de testes de unidade são inúteis para fins de codificação do dia a dia.

Por esse motivo, você normalmente não gravar testes de unidade para código que interage com um banco de dados. Que executam centenas de testes de unidade em relação a um banco de dados ao vivo seria muito lento. Em vez disso, você pode simular o banco de dados e escreve código que interage com o banco de dados fictício (discutiremos simulação de um banco de dados abaixo).

Pelo mesmo motivo, você normalmente não gravar testes de unidade para modos de exibição. Para testar um modo de exibição, você deve criar um servidor web. Como ativar ou desativar um servidor web é um processo relativamente lento, não é recomendável criar testes de unidade nos modos de exibição.

Se o modo de exibição contém a lógica complexa, em seguida, considere mover a lógica para métodos auxiliares. Você pode escrever testes de unidade para métodos auxiliares que execute sem ativar ou desativar um servidor web.

> [!NOTE] 
> 
> Ao escrever testes para lógica de acesso a dados ou lógica de exibição não é uma boa ideia ao escrever testes de unidade, esses testes podem ser muito úteis quando testa a construção funcional ou integração.


> [!NOTE] 
> 
> ASP.NET MVC é o mecanismo de exibição de formulários da Web. Enquanto o mecanismo de exibição de formulários da Web é dependente de um servidor web, outros mecanismos de exibição não podem ser.


## <a name="using-a-mock-object-framework"></a>Usando uma estrutura de objeto

Ao criar testes de unidade, quase sempre você precisa tirar proveito de uma estrutura de objetos de simulação. Uma estrutura de objetos simular permite que você crie as simulações e os stubs para as classes em seu aplicativo.

Por exemplo, você pode usar uma estrutura de objetos de simulação para gerar uma versão fictícia da classe do repositório. Dessa forma, você pode usar a classe de simulação de repositório em vez da classe de repositório real nos testes de unidade. Usar o repositório de simulação permite evitar a execução de código do banco de dados ao executar um teste de unidade.

O Visual Studio não inclui uma estrutura de objetos de simulação. No entanto, há várias estruturas de objeto simular comerciais e de código aberto disponíveis para o .NET framework:

1. Moq - essa estrutura está disponível sob a licença BSD de código-fonte aberto. Você pode baixar Moq de [ https://code.google.com/p/moq/ ](https://code.google.com/p/moq/).
2. Rhino Mocks - essa estrutura está disponível sob a licença BSD de código-fonte aberto. Você pode baixar Rhino Mocks de [ http://ayende.com/projects/rhino-mocks.aspx ](http://ayende.com/projects/rhino-mocks.aspx).
3. Typemock Isolator - este é um framework comercial. Você pode baixar uma versão de avaliação do [ http://www.typemock.com/ ](http://www.typemock.com/).

Neste tutorial, decidi usar Moq. No entanto, você poderia usar facilmente Rhino Mocks ou Typemock Isolator para criar a simulação objetos para o aplicativo do Gerenciador de contato.

Antes de usar Moq, você precisa concluir as etapas a seguir:

1. .
2. Antes do download é descompactado, certifique-se de que o arquivo de atalho e clique no botão **desbloquear** (consulte a Figura 1).
3. Descompacte o download.
4. Adicione uma referência ao assembly Moq clicando duas vezes na pasta de referências do projeto ContactManager.Tests e selecionando **adicionar referência**. Na guia Procurar, navegue até a pasta onde você descompactou Moq e selecione o assembly Moq.dll. Clique o **Okey** botão.
5. Depois de concluir essas etapas, sua pasta de referências deve parecer com a Figura 2.


[![O desbloqueio Moq](iteration-5-create-unit-tests-cs/_static/image1.jpg)](iteration-5-create-unit-tests-cs/_static/image1.png)

**Figura 01**: Moq desbloqueio ([clique para exibir a imagem em tamanho normal](iteration-5-create-unit-tests-cs/_static/image2.png))


[![Referências depois de adicionar Moq](iteration-5-create-unit-tests-cs/_static/image2.jpg)](iteration-5-create-unit-tests-cs/_static/image3.png)

**Figura 02**: referências depois da adição de Moq ([clique para exibir a imagem em tamanho normal](iteration-5-create-unit-tests-cs/_static/image4.png))


## <a name="creating-unit-tests-for-the-service-layer"></a>Criando testes de unidade para a camada de serviço

Permitir que o s comece criando um conjunto de testes de unidade para nossa camada de serviço do Gerenciador de contato aplicativo s. Vamos usar esses testes para verificar a nossa lógica de validação.

Crie uma nova pasta chamada modelos no projeto ContactManager.Tests. Em seguida, clique na pasta de modelos e selecione **adicionar, novo teste**. O **adicionar novo teste** caixa de diálogo mostrada na Figura 3 é exibida. Selecione o **de teste de unidade** modelo e nomeie o novo teste ContactManagerServiceTest.cs. Clique o **Okey** botão para adicionar o novo teste ao seu projeto de teste.

> [!NOTE] 
> 
> Em geral, você deseja que a estrutura de pastas do seu projeto de teste para corresponderem à estrutura de pasta do projeto ASP.NET MVC. Por exemplo, você pode coloca o controlador de testes em uma pasta de controladores, testes de modelo em uma pasta de modelos e assim por diante.


[![Models\ContactManagerServiceTest.cs](iteration-5-create-unit-tests-cs/_static/image3.jpg)](iteration-5-create-unit-tests-cs/_static/image5.png)

**Figura 03**: Models\ContactManagerServiceTest.cs ([clique para exibir a imagem em tamanho normal](iteration-5-create-unit-tests-cs/_static/image6.png))


Inicialmente, queremos testar o método CreateContact() exposto pela classe ContactManagerService. Vamos criar os cinco testes a seguir:

- CreateContact() - testes que CreateContact() retorna o valor true quando um contato válido é passado para o método.
- CreateContactRequiredFirstName() - testes que uma mensagem de erro é adicionada ao estado de modelo quando um contato com um nome ausente é passado para o método CreateContact().
- CreateContactRequredLastName() - testes que uma mensagem de erro é adicionada ao estado de modelo quando um contato com um sobrenome ausente é passado para o método CreateContact().
- CreateContactInvalidPhone() - testes que uma mensagem de erro é adicionada ao estado de modelo quando um contato com um número de telefone inválido é passado para o método CreateContact().
- CreateContactInvalidEmail() - testes que uma mensagem de erro é adicionada ao estado de modelo quando um contato com um endereço de email inválido é passado para o método CreateContact().

O primeiro teste verifica se um contato válido não gera um erro de validação. Os testes restantes verificam cada uma das regras de validação.

O código para esses testes está contido na listagem 1.

**Listando 1 - Models\ContactManagerServiceTest.cs**

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample1.cs)]


Como usamos a classe de contato na listagem 1, é preciso adicionar uma referência à Microsoft Entity Framework ao nosso projeto de teste. Adicione uma referência ao assembly System.Data.Entity.


Listar 1 contém um método chamado Initialize decorada com o atributo [TestInitialize]. Este método é chamado automaticamente antes de cada um dos testes de unidade é executada (ele é chamado 5 vezes antes de cada um dos testes de unidade). O método Initialize () cria um repositório de simulação com a seguinte linha de código:

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample2.cs)]

Esta linha de código usa a estrutura de Moq para gerar um repositório de simulação da interface IContactManagerRepository. O repositório de simulação é usado em vez de EntityContactManagerRepository real para evitar a acessar o banco de dados quando cada teste de unidade é executado. O repositório de simulação implementa os métodos da interface IContactManagerRepository, mas métodos don t fazer nada.

> [!NOTE] 
> 
> Ao usar o framework Moq, há uma distinção entre \_mockRepository e \_mockRepository.Object. O primeiro refere-se para a simulação&lt;IContactManagerRepository&gt; classe que contém métodos para especificar como o repositório de simulação se comportará. O último se refere ao repositório fictício real que implementa a interface IContactManagerRepository.


O repositório de simulação é usado no método Initialize () ao criar uma instância da classe ContactManagerService. Todos os testes de unidade individuais usam essa instância da classe ContactManagerService.

Listar 1 contém cinco métodos que correspondem a cada um dos testes de unidade. Cada um desses métodos está decorada com o atributo [TestMethod]. Quando você executar os testes de unidade, qualquer método que tem esse atributo é chamado. Em outras palavras, qualquer método decorada com o atributo [TestMethod] é um teste de unidade.

O primeiro teste de unidade, denominado CreateContact(), verifica se chamar CreateContact() retorna o valor true quando uma instância válida de classe de contato é passada para o método. O teste cria uma instância da classe Contact, chama o método CreateContact() e verifica CreateContact() retorna o valor true.

Verifique se os testes restantes que quando o método CreateContact() é chamado com um contato inválido, em seguida, o método retornará false e a mensagem de erro de validação esperado é adicionada ao estado do modelo. Por exemplo, o teste CreateContactRequiredFirstName() cria uma instância da classe Contact com uma cadeia de caracteres vazia para a propriedade de nome. Em seguida, o método CreateContact() é chamado com o contato inválido. Por fim, o teste verifica que CreateContact() retorna false e o estado do modelo contém a mensagem de erro de validação esperado "nome é necessário."

Você pode executar os testes de unidade na listagem 1, selecionando a opção de menu **teste, executar, todos os testes na solução (CTRL + R, A)**. Os resultados dos testes são exibidos na janela de resultados de teste (consulte a Figura 4).


[![Resultados de teste](iteration-5-create-unit-tests-cs/_static/image4.jpg)](iteration-5-create-unit-tests-cs/_static/image7.png)

**Figura 04**: resultados de teste ([clique para exibir a imagem em tamanho normal](iteration-5-create-unit-tests-cs/_static/image8.png))


## <a name="creating-unit-tests-for-controllers"></a>Criando testes de unidade para controladores

Aplicativo ASP.NETMVC controlar o fluxo de interação do usuário. Quando um controlador de teste, você deseja testar se o controlador retorna os dados de resultado e o modo de exibição de ação à direita. Também convém testar se um controlador interage com classes de modelo da maneira esperada.

Por exemplo, a listagem 2 contém dois testes de unidade para o método Create () do controlador de contato. O teste de unidade primeiro verifica se quando um contato válido é passado para o método Create (), em seguida, o método Create () redireciona para a ação de índice. Em outras palavras, quando passado um contato válido, o método Create () deve retornar um RedirectToRouteResult que representa a ação de índice.

Podemos don t deseja testar a camada de serviço ContactManager quando estamos testando a camada de controlador. Portanto, vamos simular a camada de serviço com o seguinte código no método Initialize:

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample3.cs)]

No teste de unidade CreateValidContact(), é possível simular o comportamento de chamar a método CreateContact() com a seguinte linha de código da camada de serviço:

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample4.cs)]

Esta linha de código faz com que o serviço ContactManager fictício retornar o valor true quando o método CreateContact() é chamado. Por fictícias a camada de serviço, podemos testar o comportamento do nosso controlador sem a necessidade de executar qualquer código na camada de serviço.

O segundo teste de unidade verifica que a ação Create () retorna o modo de criação quando um contato inválido é passado para o método. Podemos fazer com que a camada de serviço CreateContact() método para retornar o valor false com a seguinte linha de código:

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample5.cs)]

Se o método Create () se comporta como Esperamos que ele deve retornar Create view quando a camada de serviço retorna o valor false. Dessa forma, o controlador pode exibir as mensagens de erro de validação no modo de exibição de criação e o usuário tem a chance de solucionar esse propriedades inválidas do contato.


Se você planeja criar testes de unidade para os controladores, em seguida, você precisa retornar nomes de exibição explícita de suas ações do controlador. Por exemplo, não retorne uma exibição como este:

retornar View();

Em vez disso, retorne o modo de exibição como esta:

retornar View("Create");

Se você não explícita ao retornar que uma exibição, em seguida, a propriedade ViewResult.ViewName retorna uma cadeia de caracteres vazia.


**A listagem 2 - Controllers\ContactControllerTest.cs**

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample6.cs)]

## <a name="summary"></a>Resumo

Essa iteração, criamos testes de unidade para nosso aplicativo Gerenciador de contato. Podemos pode executar esses testes de unidade a qualquer momento para verificar que nosso aplicativo ainda se comporta da maneira que esperamos. Os testes de unidade atuam como uma rede de segurança para o nosso aplicativo que nos permite modificar com segurança o nosso aplicativo no futuro.

Nós criamos dois conjuntos de testes de unidade. Primeiro, testamos nossa lógica de validação ao criar testes de unidade para nossa camada de serviço. Em seguida, testamos nossa lógica de fluxo de controle com a criação de testes de unidade para nossa camada de controlador. Ao testar nossa camada de serviço, é isolado nossos testes para nossa camada de serviço da camada de nosso repositório por fictícias nossa camada de repositório. Quando a camada de controlador de teste, estamos isolado nossos testes para nossa camada controlador por fictícias a camada de serviço.

Na próxima iteração, podemos modificar o aplicativo Gerenciador de contato para que ele oferece suporte a grupos de contato. Vamos adicionar essa nova funcionalidade para nosso aplicativo usando um processo de design de software chamado desenvolvimento controlado por testes.

> [!div class="step-by-step"]
> [Anterior](iteration-4-make-the-application-loosely-coupled-cs.md)
> [Próximo](iteration-6-use-test-driven-development-cs.md)
