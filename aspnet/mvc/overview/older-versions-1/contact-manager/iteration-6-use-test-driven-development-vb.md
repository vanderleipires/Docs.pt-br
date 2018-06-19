---
uid: mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-vb
title: 'Iteração #6 – Use o desenvolvimento controlado por testes (VB) | Microsoft Docs'
author: microsoft
description: Essa iteração do sexto, adicionamos novas funcionalidades para nosso aplicativo escrevendo testes de unidade primeiro e escrever código os testes de unidade. Essa iteração,...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: e1fd226f-3f8e-4575-a179-5c75b240333d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-vb
msc.type: authoredcontent
ms.openlocfilehash: 71b3425c5ca8cbfc1b89493c7afb26681f8bdc9d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877582"
---
<a name="iteration-6--use-test-driven-development-vb"></a>Iteração #6 – Use o desenvolvimento controlado por testes (VB)
====================
por [Microsoft](https://github.com/microsoft)

[Baixar o código](iteration-6-use-test-driven-development-vb/_static/contactmanager_6_vb1.zip)

> Essa iteração do sexto, adicionamos novas funcionalidades para nosso aplicativo escrevendo testes de unidade primeiro e escrever código os testes de unidade. Essa iteração, adicionamos grupos de contatos.


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Criando um aplicativo ASP.NET MVC de gerenciamento de contatos (VB)
  

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

A iteração anterior do aplicativo Gerenciador de contato, criamos os testes de unidade para fornecer uma rede de segurança de nosso código. Motivação para criar testes de unidade foi criar nosso código mais resiliente a alterar. Com testes de unidade em vigor, podemos Felizmente fazer qualquer alteração em nosso código e saber imediatamente se dividimos funcionalidade existente.

Essa iteração, usamos testes de unidade para uma finalidade totalmente diferente. Essa iteração, usamos testes de unidade como parte de uma filosofia de design de aplicativo chamada *desenvolvimento controlado por testes*. Quando você praticar desenvolvimento controlado por testes, você escrever testes primeiro e, em seguida, escreve código para os testes.

Mais precisamente, ao desenvolvimento controlado por testes de exercícios, há três etapas que você concluir durante a criação de código (vermelho / verde/refatorar):

1. Escrever um teste de unidade que falha (vermelho)
2. Escrever um código que passe no teste de unidade (verde)
3. Refatorar o código (refatorar)

Primeiro, você pode gravar o teste de unidade. O teste de unidade deve expressar sua intenção de como você espera que seu código se comportam. Quando você cria o teste de unidade, o teste de unidade apresentará falha. O teste apresentará falha porque você ainda não foram gravadas qualquer código de aplicativo que satisfaz o teste.

Em seguida, você escrever código apenas o suficiente para que o teste de unidade passar. O objetivo é escrever o código da forma laziest, sloppiest e mais rápido possível. Você não deve gastar tempo pensando sobre a arquitetura do seu aplicativo. Em vez disso, você deve se concentrar em escrever a quantidade mínima de código necessário para atender a intenção expressada pelo teste de unidade.

Finalmente, depois de escrever código suficiente, você pode voltar e considere a arquitetura geral do seu aplicativo. Nesta etapa, você reescreva (refatorar) seu código, aproveitando o design de software padrões – como o padrão de repositório – para que seu código seja mais fácil manutenção. Você pode reescrever fearlessly seu código nesta etapa, porque seu código é coberto por testes de unidade.

Há muitos benefícios que resultam de prática de desenvolvimento controlado por testes. Primeiro desenvolvimento controlado por testes força você a focar no código que realmente precisa ser gravados. Porque você constantemente foco apenas escrevendo código suficiente para passar um teste em particular, você não conseguirá captado para o crescimento e a gravação de grandes quantidades de código que nunca serão usados.

Em segundo lugar, uma metodologia de design "teste primeiro" força você a escrever código da perspectiva de como seu código será usado. Em outras palavras, quando exercícios desenvolvimento controlado por testes, você está escrevendo constantemente os testes da perspectiva do usuário. Portanto, desenvolvimento controlado por testes pode resultar em APIs de limpo e mais compreensíveis.

Por fim, desenvolvimento controlado por testes força você a escrever testes de unidade como parte do processo normal de escrever um aplicativo. Como se aproxima de um prazo de projeto, teste normalmente é a primeira coisa que sai da janela. Quando praticar desenvolvimento controlado por testes, por outro lado, você está mais probabilidade de ser virtuoso sobre como escrever testes de unidade como o desenvolvimento controlado por testes faz testes de unidade central para o processo de criação de um aplicativo.

> [!NOTE] 
> 
> Para saber mais sobre o desenvolvimento controlado por testes, é recomendável que você leia o livro de difusões de Michael **trabalhar efetivamente com o código herdado**.


Essa iteração, adicionamos um novo recurso para o nosso aplicativo Gerenciador de contato. Adicionamos suporte para grupos de contatos. Você pode usar grupos de contato para organizar seus contatos em categorias, como Business e amigo.

Vamos adicionar essa nova funcionalidade para nosso aplicativo por um processo de desenvolvimento controlado por testes a seguir. Vamos escrever os testes de unidade primeiro e vamos gravar todo nosso código em relação a esses testes.

## <a name="what-gets-tested"></a>O que é testado

Conforme abordado na iteração anterior, você normalmente não gravar testes de unidade para lógica de acesso a dados ou exibir lógica. Você não os testes de unidade de gravação de t para lógica de acesso a dados como acessar um banco de dados é uma operação relativamente lenta. Você não t gravação testes de unidade lógica de exibição como acessar uma exibição exige ativar ou desativar um servidor web que é uma operação relativamente lenta. Você deve gravar um teste de unidade, a menos que o teste pode ser executado repetidamente muito rápido

Como o desenvolvimento controlado por testes é controlado por testes de unidade, nosso foco inicialmente escrevendo o controlador e lógica de negócios. Podemos evitar a necessidade de alterar o banco de dados ou exibições. Podemos ganha t modificar o banco de dados ou criar nossas exibições até o final deste tutorial. Vamos começar com o que pode ser testado.

## <a name="creating-user-stories"></a>Criar casos de usuário

Ao exercícios desenvolvimento controlado por testes, você sempre começa ao escrever um teste. Isso gera imediatamente a pergunta: como decidir quais testes para gravar primeiro? Para responder essa pergunta, você deve escrever um conjunto de [ *histórias de usuários*](http://en.wikipedia.org/wiki/User_stories).

Uma história de usuário é uma descrição (geralmente uma frase) muito breve de um requisito de software. Ele deve ser uma descrição não técnico de um requisito escrito da perspectiva do usuário.

Veja o conjunto de histórias de usuários que descrevem os recursos necessários para a nova funcionalidade de grupo do contato:

1. Usuário pode exibir uma lista de grupos de contatos.
2. Usuário pode criar um novo grupo de contato.
3. Usuário pode excluir um grupo de contato existente.
4. Usuário pode selecionar um grupo de contato ao criar um novo contato.
5. Usuário pode selecionar um grupo de contato ao editar um contato existente.
6. Uma lista de grupos de contato é exibida na exibição do índice.
7. Quando um usuário clica em um grupo de contatos, é exibida uma lista de contatos correspondentes.

Observe que esta lista de histórias de usuários é completamente entendida pelo cliente. Não há nenhuma referência de detalhes de implementação técnica.

Enquanto o processo de criação de seu aplicativo, o conjunto de histórias de usuário pode se tornar mais refinado. Você pode dividir uma história de usuário em vários artigos (requisitos). Por exemplo, você pode decidir criar um novo grupo de contato deve envolver a validação. Enviar um grupo sem um nome de contato deve retornar um erro de validação.

Depois de criar uma lista de histórias de usuários, você está pronto para escrever seu primeiro teste de unidade. Vamos começar criando um teste de unidade para exibir a lista de grupos de contatos.

## <a name="listing-contact-groups"></a>Listar grupos de contatos

Nossa primeira história de usuário é que um usuário deve ser capaz de exibir uma lista de grupos de contatos. Precisamos express este texto com um teste.

Crie um novo teste de unidade clicando-se a pasta controladores no projeto ContactManager.Tests, selecionando **adicionar, novo teste**e selecionando o **de teste de unidade** modelo (consulte a Figura 1). A nova unidade GroupControllerTest.vb de teste e clique no **Okey** botão.


[![Adicionando o teste de unidade GroupControllerTest](iteration-6-use-test-driven-development-vb/_static/image1.jpg)](iteration-6-use-test-driven-development-vb/_static/image1.png)

**Figura 01**: adicionando o teste de unidade GroupControllerTest ([clique para exibir a imagem em tamanho normal](iteration-6-use-test-driven-development-vb/_static/image2.png))


Nosso primeiro teste de unidade está contido na listagem 1. Esse teste verifica que o método Index do controlador grupo retorna um conjunto de grupos. O teste verifica se uma coleção de grupos é retornada no modo de exibição dados.

**Listando 1 - Controllers\GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample1.vb)]

Ao digitar o código na listagem 1 no Visual Studio, você terá muitas linhas curvadas vermelhas. Você criou as classes GroupController ou grupo não.

Neste ponto, podemos compilação mesmo t nosso aplicativo para que possa t executar nosso primeiro teste de unidade. Boa que s. Que conta como um teste que falhou. Portanto, agora temos permissão para iniciar a gravação de código do aplicativo. É preciso escrever código suficiente para executar o nosso teste.

A classe do controlador do grupo na lista 2 contém o mínimo de código necessário para passar no teste de unidade. A ação Index () retorna uma lista estaticamente codificada de grupos (a classe de grupo é definida na listagem 3).

**A listagem 2 - Controllers\GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample2.vb)]

**A listagem 3 - Models\Group.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample3.vb)]

Após a inclusão das classes GroupController e grupo ao nosso projeto, nosso primeiro teste de unidade é concluída com êxito (consulte a Figura 2). Fizemos o mínimo necessário para passar no teste de trabalho. É hora de celebrar.


[![Sucesso!](iteration-6-use-test-driven-development-vb/_static/image2.jpg)](iteration-6-use-test-driven-development-vb/_static/image3.png)

**Figura 02**: êxito! ( [Clique para exibir a imagem em tamanho normal](iteration-6-use-test-driven-development-vb/_static/image4.png))


## <a name="creating-contact-groups"></a>Criando grupos de contatos

Agora podemos pode passar para a segunda história de usuário. Precisamos ser capaz de criar novos grupos de contatos. Precisamos express essa intenção com um teste.

O teste na listagem 4 verifica que chamar o método com um novo grupo adiciona o grupo à lista de grupos retornado pelo método Index Create (de). Em outras palavras, se criar um novo grupo, em seguida, eu deve ser capaz de obter o novo grupo de lista de grupos retornado pelo método Index ().

**A listagem 4 - Controllers\GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample4.vb)]

O teste na listagem 4 chama o método Create () com um novo grupo de contato do controlador de grupo. Em seguida, o teste verifica que chamar o método Index () do controlador de grupo retorna o novo grupo de dados de exibição.

O controlador de grupo modificado na listagem 5 contém o mínimo necessário para passar o novo teste de alterações.

**Listagem 5 - Controllers\GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample5.vb)]

## <a name="the-group-controller-in-listing-5-has-a-new-create-action-this-action-adds-a-group-to-a-collection-of-groups-notice-that-the-index-action-has-been-modified-to-return-the-contents-of-the-collection-of-groups"></a>O controlador de grupo na listagem 5 tem uma nova ação Create (). Essa ação adiciona um grupo para uma coleção de grupos. Observe que a ação Index () foi modificada para retornar o conteúdo da coleção de grupos.

Fizemos uma vez, a quantidade mínima de trabalho necessário para passar no teste de unidade. Depois que podemos fazer essas alterações para o controlador de grupo, todos os nossos unidade testes foram bem-sucedidos.

## <a name="adding-validation"></a>Adicionando uma Validação

Esse requisito não foi declarado explicitamente na história de usuário. No entanto, é razoável exigir que um grupo tem um nome. Caso contrário, organizando os contatos em grupos não seria muito útil.

Listar 6 contém um novo teste que expressa a intenção. Esse teste verifica se a tentativa de criar um grupo sem fornecer um resultados de nome em uma mensagem de erro de validação em estado de modelo.

**Listando 6 - Controllers\GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample6.vb)]

Para satisfazer este teste, que é preciso adicionar uma propriedade de nome a nossa classe de grupo (consulte a listagem 7). Além disso, é necessário adicionar um pouco de lógica de validação ao nosso controlador grupo s ação Create () (consulte a listagem 8).

**Listando 7 - Models\Group.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample7.vb)]

**Listando 8 - Controllers\GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample8.vb)]

Observe que o controlador de grupo agora a ação Create () contém a lógica de validação e o banco de dados. Atualmente, o banco de dados usado pelo controlador de grupo consiste em nada mais que uma coleção em memória.

## <a name="time-to-refactor"></a>Tempo de refatoração

A terceira etapa em vermelho/verde/refatorar é a parte de refatoração. Neste ponto, é preciso entrar novamente em nosso código e considere como podemos refatorar nosso aplicativo para melhorar seu design. O estágio de refatoração é o estágio em que acreditamos que rígido sobre a melhor maneira de implementar os padrões e os princípios de design de software.

Estamos livres para modificar o nosso código de qualquer forma que escolhemos para aprimorar o design do código. Temos uma rede de segurança de testes de unidade que nos impedem de interromper a funcionalidade existente.

Agora, nosso controlador de grupo é uma mensagem de uma perspectiva de bom design de software. O controlador de grupo contém uma confusão tangled de validação e o código de acesso a dados. Para evitar a violação o princípio da responsabilidade única, é preciso separar essas preocupações em classes diferentes.

Nossa classe de controlador grupo refatorado está contido na listagem 9. O controlador foi modificado para usar a camada de serviço ContactManager. Essa é a camada de serviço mesmo que podemos usar com o controlador do contato.

A listagem 10 contém os novos métodos adicionados à camada de serviço ContactManager para dar suporte a validação, listando e criar grupos. A interface IContactManagerService foi atualizada para incluir os novos métodos.

Listagem 11 contém uma nova classe FakeContactManagerRepository que implementa a interface IContactManagerRepository. Ao contrário de classe EntityContactManagerRepository que também implementa a interface IContactManagerRepository, nossa nova classe FakeContactManagerRepository não se comunicar com o banco de dados. A classe FakeContactManagerRepository usa uma coleção de memória como um proxy para o banco de dados. Vamos usar essa classe em nossos testes de unidade como uma camada de repositório falsa.

**Listagem 9 - Controllers\GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample9.vb)]

**Listing 10 - Controllers\ContactManagerService.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample10.vb)]

**Listando 11 - Controllers\FakeContactManagerRepository.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample11.vb)]


Modificando o IContactManagerRepository interface requer usar para implementar os métodos CreateGroup() e ListGroups() na classe EntityContactManagerRepository. A maneira mais rápida e laziest de fazer isso é adicionar métodos de stub ter esta aparência:

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample12.vb)]


Por fim, essas alterações ao design do nosso aplicativo requerem que fazer algumas modificações para nossos testes de unidade. Agora, é preciso usar o FakeContactManagerRepository ao executar os testes de unidade. A classe GroupControllerTest atualizada está contida na listagem 12.

**Listando 12 - Controllers\GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample13.vb)]

Depois de fazer todas essas alterações, uma vez, todos os testes de unidade aprovados. Já foram concluídos todo o ciclo de vermelho/verde/refatoração. Implementamos as histórias de usuário de dois primeiro. Agora temos suporte para testes de unidade para os requisitos expressos em histórias de usuários. Implementar o restante das histórias de usuários envolve o mesmo ciclo de vermelho/verde/refatoração de repetição.

## <a name="modifying-our-database"></a>Modificando o nosso banco de dados

Infelizmente, mesmo que nós foram satisfeitos todos os requisitos expressados por nossos testes de unidade, nosso trabalho não for feito. Ainda é preciso modificar o nosso banco de dados.

É preciso criar uma nova tabela de banco de dados do grupo. Siga estas etapas:

1. Na janela do Gerenciador de servidores, clique com botão direito na pasta tabelas e selecione a opção de menu **adicionar nova tabela**.
2. Insira as duas colunas descritas abaixo no Designer de tabela.
3. Marque a coluna de Id como uma chave primária e uma coluna de identidade.
4. Salve a nova tabela com os grupos de nome clicando no ícone de disquete.

<a id="0.12_table01"></a>


| **Nome da coluna** | **Tipo de dados** | **Permitir nulos** |
| --- | --- | --- |
| Id | int | False |
| Nome | nvarchar(50) | False |


Em seguida, é preciso excluir todos os dados da tabela Contacts (caso contrário, podemos ganha não poderá criar uma relação entre as tabelas de contatos e grupos). Siga estas etapas:

1. A tabela Contatos e selecione a opção de menu **Mostrar dados da tabela**.
2. Exclua todas as linhas.

Em seguida, é preciso definir uma relação entre a tabela de banco de dados de grupos e a tabela de banco de dados de contatos existente. Siga estas etapas:

1. Clique duas vezes a tabela contatos na janela do Gerenciador de servidores para abrir o Designer de tabela.
2. Adicione uma nova coluna de inteiro à tabela Contatos denominada GroupId.
3. Clique no botão de relação para abrir a caixa de diálogo relações de chave estrangeira (consulte a Figura 3).
4. Clique no botão Adicionar.
5. Clique no botão de reticências que aparece ao lado do botão de tabela e uma especificação de colunas.
6. Na caixa de diálogo tabelas e colunas, selecione grupos como a tabela de chave primária e a Id como a coluna de chave primária. Selecione contatos como a tabela de chave estrangeira e a ID do grupo como a coluna de chave estrangeira (consulte a Figura 4). Clique no botão Okey.
7. Em **especificação INSERT e UPDATE**, selecione o valor **Cascade** para **Excluir regra**.
8. Clique no botão Fechar para fechar a caixa de diálogo relações de chave estrangeira.
9. Clique no botão Salvar para salvar as alterações à tabela de contatos.


[![Criar uma relação de tabela do banco de dados](iteration-6-use-test-driven-development-vb/_static/image3.jpg)](iteration-6-use-test-driven-development-vb/_static/image5.png)

**Figura 03**: criar uma relação de tabela do banco de dados ([clique para exibir a imagem em tamanho normal](iteration-6-use-test-driven-development-vb/_static/image6.png))


[![Especificando relações de tabela](iteration-6-use-test-driven-development-vb/_static/image4.jpg)](iteration-6-use-test-driven-development-vb/_static/image7.png)

**Figura 04**: especificar relações de tabela ([clique para exibir a imagem em tamanho normal](iteration-6-use-test-driven-development-vb/_static/image8.png))


### <a name="updating-our-data-model"></a>Atualizando o modelo de dados

Em seguida, precisamos atualizar o modelo de dados para representar a nova tabela de banco de dados. Siga estas etapas:

1. Duas vezes no arquivo ContactManagerModel.edmx na pasta de modelos para abrir o Designer de entidade.
2. A superfície do Designer e selecione a opção de menu **modelo de atualização de banco de dados**.
3. No Assistente de atualização, selecione os grupos de tabela e clique no término botão (consulte a Figura 5).
4. A entidade de grupos e selecione a opção de menu **Renomear**. Alterar o nome do *grupos* entidade *grupo* (singular).
5. Clique com botão direito a propriedade de navegação de grupos aparece na parte inferior da entidade do contato. Alterar o nome do *grupos* propriedade de navegação *grupo* (singular).


[![Atualizando um modelo do Entity Framework do banco de dados](iteration-6-use-test-driven-development-vb/_static/image5.jpg)](iteration-6-use-test-driven-development-vb/_static/image9.png)

**Figura 05**: atualizando um modelo do Entity Framework do banco de dados ([clique para exibir a imagem em tamanho normal](iteration-6-use-test-driven-development-vb/_static/image10.png))


Depois de concluir essas etapas, o modelo de dados representa tabelas de grupos e contatos. O Designer de entidade devem mostrar ambas as entidades (consulte a Figura 6).


[![Exibição de grupo e contato do Designer de entidade](iteration-6-use-test-driven-development-vb/_static/image6.jpg)](iteration-6-use-test-driven-development-vb/_static/image11.png)

**Figura 06**: Entity Designer exibindo Group e Contact ([clique para exibir a imagem em tamanho normal](iteration-6-use-test-driven-development-vb/_static/image12.png))


### <a name="creating-our-repository-classes"></a>Criando Classes nosso repositório

Em seguida, é preciso implementar nossa classe de repositório. Durante o curso dessa iteração, adicionamos vários novos métodos para a interface IContactManagerRepository ao escrever código para satisfazer nossos testes de unidade. A versão final da interface IContactManagerRepository está contida na 14 de listagem.

**Listando 14 - Models\IContactManagerRepository.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample14.vb)]

Nós realmente implementado refúgio t e qualquer um dos métodos relacionados a trabalhar com grupos de contatos em nossa classe EntityContactManagerRepository real. Atualmente, a classe EntityContactManagerRepository tem métodos de stub para cada um dos métodos de contato de grupo listados na interface do IContactManagerRepository. Por exemplo, o método ListGroups() atualmente esta aparência:

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample15.vb)]

Os métodos de stub habilitado para compilar nosso aplicativo e passar nos testes de unidade. No entanto, agora é hora de realmente implementar esses métodos. A versão final da classe EntityContactManagerRepository está contida na listagem 13.

**A listagem 13 - Models\EntityContactManagerRepository.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample16.vb)]

### <a name="creating-the-views"></a>Criando os modos de exibição

Quando você usa o mecanismo de exibição do ASP.NET padrão de aplicativo do ASP.NET MVC. Portanto, Anibal t criar modos de exibição em resposta a um teste de unidade específica. No entanto, como um aplicativo seria inútil sem exibições, podemos t concluir essa iteração sem criar e modificar os modos de exibição contidos no aplicativo Gerenciador de contato.

É necessário criar as novas exibições a seguir para gerenciar grupos de contato (consulte a Figura 7):

- Views\Group\Index.aspx - exibe a lista de grupos de contatos
- Views\Group\Delete.aspx - formulário de confirmação exibida para excluir um grupo de contatos


[![O modo de exibição do índice de grupo](iteration-6-use-test-driven-development-vb/_static/image7.jpg)](iteration-6-use-test-driven-development-vb/_static/image13.png)

**Figura 07**: exibir o índice de grupo ([clique para exibir a imagem em tamanho normal](iteration-6-use-test-driven-development-vb/_static/image14.png))


É necessário modificar as seguintes exibições existentes para que elas incluem grupos de contato:

- Views\Home\Create.aspx
- Views\Home\Edit.aspx
- Views\Home\Index.aspx

Você pode ver os modos modificados examinando o aplicativo do Visual Studio que acompanha este tutorial. Por exemplo, a Figura 8 ilustra o modo de exibição do índice de contatos.


[![O modo de exibição do índice de contatos](iteration-6-use-test-driven-development-vb/_static/image8.jpg)](iteration-6-use-test-driven-development-vb/_static/image15.png)

**Figura 08**: exibir o índice do contato ([clique para exibir a imagem em tamanho normal](iteration-6-use-test-driven-development-vb/_static/image16.png))


## <a name="summary"></a>Resumo

Essa iteração, nós adicionamos novas funcionalidades para nosso aplicativo Contact Manager seguindo uma metodologia de design de aplicativo de desenvolvimento controlado por testes. Começamos criando um conjunto de histórias de usuários. Criamos um conjunto de testes de unidade que corresponde aos requisitos expressados por histórias de usuários. Por fim, escrevemos código apenas o suficiente para atender os requisitos expressados por testes de unidade.

Depois de concluída escrevendo código suficiente para atender os requisitos expressados por testes de unidade, atualizamos nosso banco de dados e os modos de exibição. Adicionamos uma nova tabela de grupos para nosso banco de dados e atualizado nosso modelo de dados do Entity Framework. Também é criado e modificado de um conjunto de exibições.

Na próxima iteração – a iteração final – estamos reescreva nosso aplicativo para tirar proveito do Ajax. Aproveitando Ajax, podemos vai melhorar a capacidade de resposta e o desempenho do aplicativo Gerenciador de contato.

> [!div class="step-by-step"]
> [Anterior](iteration-5-create-unit-tests-vb.md)
> [Próximo](iteration-7-add-ajax-functionality-vb.md)
