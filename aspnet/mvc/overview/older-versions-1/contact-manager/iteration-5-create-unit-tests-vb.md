---
uid: mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-vb
title: 'Iteração #5 – criar testes de unidade (VB) | Microsoft Docs'
author: microsoft
description: Na quinta iteração, podemos tornar nosso aplicativo mais fácil de manter e modificar adicionando testes de unidade. Vamos simular a nossas classes de modelo de dados e criar testes de unidade para o...
ms.author: aspnetcontent
ms.date: 02/20/2009
ms.assetid: c6e5c036-2265-4fa7-a9eb-47f197bdc262
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-vb
msc.type: authoredcontent
ms.openlocfilehash: 49e8b3eb6c0e8fb7199816d0c2b0843e0519872a
ms.sourcegitcommit: a25b572eaed21791230c85416f449f66a405ec19
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39396083"
---
<a name="iteration-5--create-unit-tests-vb"></a>Iteração #5 – criar testes de unidade (VB)
====================
por [Microsoft](https://github.com/microsoft)

[Baixar o código](iteration-5-create-unit-tests-vb/_static/contactmanager_5_vb1.zip)

> Na quinta iteração, podemos tornar nosso aplicativo mais fácil de manter e modificar adicionando testes de unidade. Vamos simular a nossas classes de modelo de dados e criar testes de unidade para nossos controladores e lógica de validação.


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Criando um aplicativo ASP.NET MVC de gerenciamento de contatos (VB)

Esta série de tutoriais, vamos criar um aplicativo de gerenciamento de contatos inteiro do início ao fim. O aplicativo Gerenciador de contatos permite que você armazene informações de contato - nomes, números de telefone e endereços de email - para obter uma lista de pessoas.

Criamos o aplicativo ao longo de várias iterações. Com cada iteração, podemos melhorar gradualmente o aplicativo. A meta dessa abordagem de iteração vários é que você possa entender o motivo para cada alteração.

- Iteração #1 - criar o aplicativo. A primeira iteração, podemos criar o Gerenciador de contatos da maneira mais simples possível. Adicionamos suporte para operações de banco de dados básico: criar, ler, atualizar e excluir (CRUD).

- Iteração #2 - tornar o aplicativo interessante. Nesta iteração, podemos melhorar a aparência do aplicativo modificando a página mestra do ASP.NET MVC exibição padrão e em cascata de folha de estilos.

- Iteração #3 - adicionar validação de formulário. Na terceira iteração, podemos adicionar validação de formulário básico. Podemos impedir que pessoas enviando um formulário sem preencher os campos obrigatórios do formulário. Podemos também validar endereços de email e números de telefone.

- Iteração #4 – tornar o aplicativo fracamente acoplado. Nesta quarta iteração, podemos tirar proveito dos diversos padrões de design de software para facilitar a manutenção e modificar o aplicativo Gerenciador de contatos. Por exemplo, podemos refatorar nosso aplicativo para usar o padrão de repositório e o padrão de injeção de dependência.

- Iteração #5 - criar testes de unidade. Na quinta iteração, podemos tornar nosso aplicativo mais fácil de manter e modificar adicionando testes de unidade. Vamos simular a nossas classes de modelo de dados e criar testes de unidade para nossos controladores e lógica de validação.

- Iteração #6 – usar desenvolvimento controlado por teste. Essa iteração sexta, adicionamos novas funcionalidades ao nosso aplicativo escrevendo testes de unidade pela primeira vez e escrever código contra os testes de unidade. Essa iteração, adicionamos os grupos de contatos.

- Iteração #7 - adicionar a funcionalidade do Ajax. A sétima iteração, podemos melhorar a capacidade de resposta e o desempenho do nosso aplicativo, adicionando suporte para Ajax.


## <a name="this-iteration"></a>Essa iteração

Na iteração anterior do aplicativo Contact Manager, podemos refatorar o aplicativo para ser acoplamento mais flexível. Separamos o aplicativo em controlador distinto, serviço e camadas de repositório. Cada camada interage com a camada abaixo dela, por meio de interfaces.

Podemos refatorar o aplicativo para tornar o aplicativo mais fácil de manter e modificar. Por exemplo, se é necessário usar uma nova tecnologia de acesso de dados, podemos simplesmente alterar a camada de repositório sem tocar o controlador ou a camada de serviço. Fazendo o Gerenciador de contatos acoplamento menos rígido, podemos ve tornei o aplicativo mais resiliente a alterar.

Mas, o que acontece quando precisamos adicionar um novo recurso para o aplicativo Contact Manager? Ou então, o que acontece quando podemos corrigir um bug? Uma verdade triste, mas também comprovada de escrever código é que, sempre que você tocar o código que você cria o risco de introduzir novos bugs.

Por exemplo, um dia BOM, seu gerente pode solicitar que você adicionar um novo recurso ao Gerenciador de contatos. Ela quer que você adicionar suporte para grupos de contatos. Ela quer que você permitir que os usuários organizar seus contatos em grupos como amigos, negócios e assim por diante.

Para implementar esse novo recurso, você precisará modificar todas as três camadas do aplicativo Gerenciador de contatos. Você precisará adicionar uma nova funcionalidade para os controladores, a camada de serviço e o repositório. Assim que você comece a modificar o código, você corre o risco de interromper a funcionalidade que funcionavam antes.

Refatoração de nosso aplicativo em camadas separadas, como fizemos na iteração anterior, era uma coisa boa. Era uma coisa boa porque ela nos permite fazer alterações em camadas inteiras sem tocar no resto do aplicativo. No entanto, se você quiser facilitar a manutenção e modificar o código dentro de uma camada, você precisará criar testes de unidade para o código.

Você use uma unidade de teste para testar uma unidade de código individual. Essas unidades de código são menores do que as camadas de aplicativo inteiro. Normalmente, você deve usar um teste de unidade para verificar se um método específico em seu código se comporta da maneira que você espera. Por exemplo, você criaria um teste de unidade para o método CreateContact() exposto pela classe ContactManagerService.

Os testes de unidade para um trabalho de aplicativo apenas como uma rede de segurança. Sempre que modificar o código em um aplicativo, você pode executar um conjunto de testes de unidade para verificar se a modificação interrompe a funcionalidade existente. Testes de unidade tornam seu código seguro modificar. Testes de unidade fazer todo o código em seu aplicativo mais resiliente a alterar.

Essa iteração, adicionamos testes de unidade para nosso aplicativo Contact Manager. Dessa forma, na próxima iteração, podemos adicionar grupos de contato para o nosso aplicativo sem se preocupar sobre como interromper a funcionalidade existente.

> [!NOTE] 
> 
> Há uma variedade de estruturas, incluindo o MbUnit, xUnit.net e NUnit de teste de unidade. Neste tutorial, usamos o unit testing framework incluído com o Visual Studio. No entanto, você pode facilmente usar uma dessas estruturas alternativas.


## <a name="what-gets-tested"></a>O que é testado

No mundo perfeito, todo o código deve ser coberto por testes de unidade. No mundo perfeito, você teria de rede perfeita de segurança. Você seria capaz de modificar qualquer linha de código em seu aplicativo e saiba instantaneamente, executando os testes de unidade, se a alteração interrompeu a funcionalidade existente.

No entanto, podemos don t ao vivo em um mundo perfeito. Na prática, ao escrever testes de unidade, você se concentrar em escrever testes para sua lógica de negócios (por exemplo, a lógica de validação). Em particular, você *não* escrever testes de unidade para seus dados de acesso lógico ou sua lógica de exibição.

Para ser útil, testes de unidade devem ser executadas muito rapidamente. Você facilmente pode acumular centenas (ou até mesmo milhares) de testes de unidade para um aplicativo. Se os testes de unidade demoram muito para ser executado, você evitará a executá-los. Em outras palavras, testes de unidade de longa execução são inúteis para propósitos de codificação do dia a dia.

Por esse motivo, você normalmente não escrever testes de unidade para código que interage com um banco de dados. Execução de centenas de testes de unidade em relação a um banco de dados ao vivo seria muito lento. Em vez disso, você pode simular seu banco de dados e escreve código que interage com o banco de dados fictício (discutiremos a simulação de um banco de dados abaixo).

Pelo mesmo motivo, você normalmente não escrever testes de unidade para modos de exibição. Para testar um modo de exibição, você deve criar um servidor web. Como bagunçar um servidor web é um processo relativamente lento, não é recomendável criar testes de unidade para seus modos de exibição.

Se o modo de exibição contiver a lógica complicada, em seguida, considere mover a lógica para métodos auxiliares. Você pode escrever testes de unidade para métodos auxiliares que execute sem bagunçar um servidor web.

> [!NOTE] 
> 
> Embora escrever testes para lógica de acesso a dados ou lógica de exibição não é uma boa ideia ao escrever testes de unidade, esses testes podem ser muito valiosos quando os testes de integração ou construção funcional.


> [!NOTE] 
> 
> ASP.NET MVC é o mecanismo de exibição de formulários da Web. Enquanto o mecanismo de exibição de formulários da Web é dependente de um servidor web, outros mecanismos de exibição podem não ser.


## <a name="using-a-mock-object-framework"></a>Usando uma estrutura de objeto

Ao criar testes de unidade, quase sempre você precisa tirar proveito de uma estrutura de objetos fictícios. Uma estrutura de simulação de objetos permite que você crie simulações e stubs para as classes em seu aplicativo.

Por exemplo, você pode usar uma estrutura de objetos fictícios para gerar uma versão fictícia de sua classe de repositório. Dessa forma, você pode usar a classe de repositório fictício em vez da classe de repositório reais nos testes de unidade. Usar o repositório fictício permite que você evite a execução de código do banco de dados ao executar um teste de unidade.

Visual Studio não inclui uma estrutura de objetos fictícios. No entanto, há várias estruturas de objeto de simulação de software livre e comercial disponível para o .NET framework:

1. Moq - essa estrutura está disponível sob a licença BSD de código-fonte aberto. Você pode baixar o Moq partir [ https://code.google.com/p/moq/ ](https://code.google.com/p/moq/).
2. Rhino Mocks - essa estrutura está disponível sob a licença BSD de código-fonte aberto. Você pode baixar a Rhino Mocks partir [ http://ayende.com/projects/rhino-mocks.aspx ](http://ayende.com/projects/rhino-mocks.aspx).
3. Typemock Isolator - isso é uma estrutura comercial. Você pode baixar uma versão de avaliação [ http://www.typemock.com/ ](http://www.typemock.com/).

Neste tutorial, decidi usar Moq. No entanto, você poderia usar facilmente a Rhino Mocks ou Typemock Isolator para criar a simulação de objetos para o aplicativo Gerenciador de contatos.

Antes de usar Moq, você precisará concluir as etapas a seguir:

1. .
2. Antes que o download é descompactado, certifique-se de que o arquivo com o botão direito e clique no botão rotulado **Unblock** (veja a Figura 1).
3. Descompacte o download.
4. Adicione uma referência ao assembly Moq ao seu projeto de teste, selecionando a opção de menu **projeto, adicionar referência** para abrir o **adicionar referência** caixa de diálogo. Na guia Procurar, navegue até a pasta onde você descompactou o Moq e selecione o assembly de Moq.dll. Clique o **Okey** botão (consulte a Figura 2).


[![Como desbloquear Moq](iteration-5-create-unit-tests-vb/_static/image1.jpg)](iteration-5-create-unit-tests-vb/_static/image1.png)

**Figura 01**: Moq desbloqueio ([clique para exibir a imagem em tamanho normal](iteration-5-create-unit-tests-vb/_static/image2.png))


[![Referências depois de adicionar o Moq](iteration-5-create-unit-tests-vb/_static/image2.jpg)](iteration-5-create-unit-tests-vb/_static/image3.png)

**Figura 02**: referências depois de adicionar o Moq ([clique para exibir a imagem em tamanho normal](iteration-5-create-unit-tests-vb/_static/image4.png))


## <a name="creating-unit-tests-for-the-service-layer"></a>Criando testes de unidade para a camada de serviço

Deixe o s começar criando um conjunto de testes de unidade para nossa camada de serviço do Gerenciador de contatos aplicativo s. Vamos usar esses testes para verificar nossa lógica de validação.

Crie uma nova pasta chamada modelos no projeto ContactManager.Tests. Em seguida, clique com botão direito na pasta modelos e selecione **Add, o novo teste**. O **Add New Test** aparece da caixa de diálogo mostrada na Figura 3. Selecione o **de teste de unidade** modelo e nomeie seu novo teste ContactManagerServiceTest.vb. Clique o **Okey** botão para adicionar seu novo teste ao seu projeto de teste.

> [!NOTE] 
> 
> Em geral, você deseja que a estrutura de pastas do seu projeto de teste para coincidir com a estrutura de pastas do seu projeto ASP.NET MVC. Por exemplo, você pode colocar o controlador de testes em uma pasta de controladores, testes de modelo em uma pasta de modelos e assim por diante.


[![Models\ContactManagerServiceTest.cs](iteration-5-create-unit-tests-vb/_static/image3.jpg)](iteration-5-create-unit-tests-vb/_static/image5.png)

**Figura 03**: Models\ContactManagerServiceTest.cs ([clique para exibir a imagem em tamanho normal](iteration-5-create-unit-tests-vb/_static/image6.png))


Inicialmente, queremos testar o método CreateContact() exposto pela classe ContactManagerService. Criaremos os cinco testes a seguir:

- CreateContact() - testes que CreateContact() retorna o valor true quando um contato válido é passado para o método.
- CreateContactRequiredFirstName() - testes que uma mensagem de erro é adicionada ao estado de modelo quando um contato com um nome ausente é passado para o método CreateContact().
- CreateContactRequredLastName() - testes que uma mensagem de erro é adicionada ao estado de modelo quando um contato com um sobrenome ausente é passado para o método CreateContact().
- CreateContactInvalidPhone() - testes que uma mensagem de erro é adicionada ao estado de modelo quando um contato com um número de telefone inválido é passado para o método CreateContact().
- CreateContactInvalidEmail() - testes que uma mensagem de erro é adicionada ao estado de modelo quando um contato com um endereço de email inválido é passado para o método CreateContact()...

O primeiro teste verifica se um contato válido não gera um erro de validação. Os testes restantes verificam cada das regras de validação.

O código para esses testes está contido na listagem 1.

**Listagem 1 - Models\ContactManagerServiceTest.vb**

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample1.vb)]


Como usamos a classe de contato na listagem 1, precisamos adicionar uma referência para o Entity Framework da Microsoft ao nosso projeto de teste. Adicione uma referência ao assembly System.


Listagem 1 contém um método chamado Initialize () que é decorada com o atributo [TestInitialize]. Esse método é chamado automaticamente antes de cada um dos testes de unidade é executada (ele é chamado 5 vezes antes de cada um dos testes de unidade). O método Initialize () cria um repositório fictício com a seguinte linha de código:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample2.vb)]

Esta linha de código usa a estrutura de Moq para gerar um repositório fictício da interface IContactManagerRepository. O repositório fictício é usado em vez do EntityContactManagerRepository real para evitar a acessar o banco de dados quando cada teste de unidade é executado. O repositório fictício implementa os métodos da interface IContactManagerRepository, mas o métodos don t, na verdade, fazer nada.

> [!NOTE] 
> 
> Ao usar a estrutura Moq, há uma distinção entre \_mockRepository e \_mockRepository.Object. O primeiro refere-se a classe de simulação (de IContactManagerRepository) que contém métodos para especificar como o repositório fictício irá se comportar. O último se refere ao repositório fictício real que implementa a interface IContactManagerRepository.


O repositório fictício é usado no método Initialize () ao criar uma instância da classe ContactManagerService. Todos os testes de unidade individuais usam essa instância da classe ContactManagerService.

Listagem 1 contém cinco métodos que correspondem a cada um dos testes de unidade. Cada um desses métodos é decorada com o atributo [TestMethod]. Quando você executa os testes de unidade, qualquer método que tem esse atributo é chamado. Em outras palavras, qualquer método que está decorado com o atributo [TestMethod] é um teste de unidade.

O primeiro teste de unidade, chamado CreateContact(), verifica que chamar CreateContact() retorna o valor true quando uma instância válida da classe contato é passada para o método. O teste cria uma instância da classe Contact, chama o método CreateContact() e verifica CreateContact() retorna o valor true.

Verifique se os testes restantes que quando o método CreateContact() é chamado com um contato inválido, em seguida, o método retornará false e a mensagem de erro de validação esperado é adicionada ao estado do modelo. Por exemplo, o teste CreateContactRequiredFirstName() cria uma instância da classe Contact com uma cadeia de caracteres vazia para sua propriedade FirstName. Em seguida, o método CreateContact() é chamado com o contato inválido. Por fim, o teste verifica que CreateContact() retorna false e o estado do modelo contém a mensagem de erro de validação esperado "nome é obrigatório."

Você pode executar os testes de unidade na listagem 1, selecionando a opção de menu **teste, execute todos os testes na solução (CTRL + R, um)**. Os resultados dos testes são exibidos na janela de resultados de teste (veja a Figura 4).


[![Resultados de teste](iteration-5-create-unit-tests-vb/_static/image4.jpg)](iteration-5-create-unit-tests-vb/_static/image7.png)

**Figura 04**: resultados de teste ([clique para exibir a imagem em tamanho normal](iteration-5-create-unit-tests-vb/_static/image8.png))


## <a name="creating-unit-tests-for-controllers"></a>Criando testes de unidade para controladores

Aplicativo ASP.NET MVC controlam o fluxo de interação do usuário. Quando um controlador de teste, você deseja testar se o controlador retorna os dados de resultado e o modo de exibição de ação à direita. Também convém testar se um controlador interage com as classes de modelo da maneira esperada.

Por exemplo, a listagem 2 contém dois testes de unidade para o método Create () do controlador de contato. O teste de unidade primeiro verifica se a quando um contato válido é passado para o método Create (), em seguida, o método Create () redireciona para a ação de índice. Em outras palavras, quando passado um contato válido, o método Create () deve retornar um RedirectToRouteResult que representa a ação de índice.

Podemos don t deseja testar a camada de serviço ContactManager quando estamos testando a camada de controlador. Portanto, vamos simular a camada de serviço com o seguinte código no método de inicialização:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample3.vb)]

No teste de unidade CreateValidContact(), podemos simule o comportamento de chamar a método CreateContact() com a seguinte linha de código da camada de serviço:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample4.vb)]

Esta linha de código faz com que o serviço ContactManager fictício retornar o valor true quando seu método CreateContact() é chamado. Por que a camada de serviço de simulação, podemos testar o comportamento do nosso controlador sem a necessidade de executar qualquer código na camada de serviço.

O segundo teste de unidade verifica que a ação Create () retorna o modo de exibição criar quando um contato inválido é passado para o método. Podemos fazer com que a camada de serviço CreateContact() método para retornar o valor false com a seguinte linha de código:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample5.vb)]

Se o método Create () se comporta conforme esperado, em seguida, ele deverá retornar o modo de exibição criar quando a camada de serviço retorna o valor false. Dessa forma, o controlador pode exibir as mensagens de erro de validação no modo de exibição criar e o usuário tem a oportunidade de corrigir esse propriedades inválidas do contato.


Se você planeja criar testes de unidade para seus controladores, em seguida, você precisará retornar nomes de exibição explícita de ações do controlador. Por exemplo, não retornam um modo de exibição como esta:

Retornar View()

Nesse caso, devolva o modo de exibição como esta:

Retornar View("Create")

Se você não explícita ao retornar que uma exibição, em seguida, a propriedade ViewResult.ViewName retorna uma cadeia de caracteres vazia.


**Listagem 2 - Controllers\ContactControllerTest.vb**

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample6.vb)]

## <a name="summary"></a>Resumo

Essa iteração, criamos testes de unidade para nosso aplicativo Contact Manager. Podemos executar esses testes de unidade a qualquer momento para verificar que nosso aplicativo ainda se comporta da maneira que esperamos. Os testes de unidade atuam como uma rede de segurança para nosso aplicativo que nos permite modificar com segurança nosso aplicativo no futuro.

Nós criamos dois conjuntos de testes de unidade. Primeiro, testamos nossa lógica de validação, criando testes de unidade para nossa camada de serviço. Em seguida, testamos nossa lógica de controle de fluxo, criando testes de unidade para nossa camada de controlador. Ao testar nossa camada de serviço, podemos isolado nossos testes para nossa camada de serviço da nossa camada de repositório por nossa camada de repositório de simulação. Quando a camada de controlador de teste, podemos isolado nossos testes para nossa camada de controlador pela camada de serviço de simulação.

A próxima iteração, modificamos o aplicativo Gerenciador de contatos para que ele oferece suporte a grupos de contato. Vamos adicionar essa nova funcionalidade ao nosso aplicativo usando um processo de design de software chamado desenvolvimento controlado por teste.

> [!div class="step-by-step"]
> [Anterior](iteration-4-make-the-application-loosely-coupled-vb.md)
> [Próximo](iteration-6-use-test-driven-development-vb.md)
