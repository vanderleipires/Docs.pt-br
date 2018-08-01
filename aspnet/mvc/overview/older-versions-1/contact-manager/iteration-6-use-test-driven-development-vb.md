---
uid: mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-vb
title: 'Iteração #6 – usar desenvolvimento controlado por testes (VB) | Microsoft Docs'
author: microsoft
description: Essa iteração sexta, adicionamos novas funcionalidades ao nosso aplicativo escrevendo testes de unidade pela primeira vez e escrever código contra os testes de unidade. Nesta iteração,...
ms.author: aspnetcontent
ms.date: 02/20/2009
ms.assetid: e1fd226f-3f8e-4575-a179-5c75b240333d
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-vb
msc.type: authoredcontent
ms.openlocfilehash: 5108e427025f42424c2dc6b657806544f9f8eeaa
ms.sourcegitcommit: a25b572eaed21791230c85416f449f66a405ec19
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39396108"
---
<a name="iteration-6--use-test-driven-development-vb"></a>Iteração #6 – usar desenvolvimento controlado por testes (VB)
====================
por [Microsoft](https://github.com/microsoft)

[Baixar o código](iteration-6-use-test-driven-development-vb/_static/contactmanager_6_vb1.zip)

> Essa iteração sexta, adicionamos novas funcionalidades ao nosso aplicativo escrevendo testes de unidade pela primeira vez e escrever código contra os testes de unidade. Essa iteração, adicionamos os grupos de contatos.


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

A iteração anterior do aplicativo Contact Manager, criamos os testes de unidade para fornecer uma rede de segurança para o nosso código. A motivação para a criação de testes de unidade era tornar nosso código mais resiliente a alterar. Com testes de unidade em vigor, podemos Felizmente fazer nenhuma alteração ao nosso código e saber imediatamente se nós dividimos a funcionalidade existente.

Essa iteração, usamos testes de unidade para uma finalidade totalmente diferente. Essa iteração, podemos usar testes de unidade como parte de uma filosofia de design de aplicativo chamada *desenvolvimento controlado por teste*. Quando você praticar o desenvolvimento controlado por teste, você escreve testes primeiro e, em seguida, escreve código contra os testes.

Mais precisamente, ao praticar o desenvolvimento controlado por teste, há três etapas que você concluir durante a criação de código (vermelho / verde/de refatoração):

1. Escrever um teste de unidade que falha (vermelho)
2. Escrever um código que passa o teste de unidade (verde)
3. Refatorar seu código (refatorar)

Primeiro, você pode escrever o teste de unidade. O teste de unidade deve expressar sua intenção de como você espera que seu código para se comportar. Quando você cria o teste de unidade, o teste de unidade deve falhar. O teste deverá falhar porque você não tiver gravado qualquer código de aplicativo que satisfaz o teste.

Em seguida, você deve escrever código apenas o suficiente para que o teste de unidade passar. O objetivo é escrever o código da forma laziest, sloppiest e mais rápido possível. Você não deve gastar tempo pensando sobre a arquitetura do seu aplicativo. Em vez disso, você deve se concentrar em escrever a quantidade mínima de código necessário para atender a intenção expressada pelo teste de unidade.

Por fim, depois de escrever código suficiente, você pode voltar atrás e considerar a arquitetura geral do seu aplicativo. Nesta etapa, você (o refactor) de reescrever seu código, tirando proveito do design de software padrões – como o padrão de repositório – para que seu código seja mais sustentável. Sem medo e você pode reescrever o código nesta etapa, pois seu código é coberto por testes de unidade.

Há muitos benefícios resultantes de praticar desenvolvimento controlado por teste. Primeiro, orientado a testes de desenvolvimento força você a se concentrar no código que realmente precisa ser escrito. Porque você constantemente visavam apenas escrever o código necessário para passar um teste específico, você será impedido de vagando o supérfluo e gravação de grandes quantidades de código que nunca será usada.

Em segundo lugar, uma metodologia de design de "teste primeiro" força a escrever o código da perspectiva de como seu código será usado. Em outras palavras, ao praticar o desenvolvimento controlado por teste, você está gravando constantemente os testes de uma perspectiva do usuário. Portanto, o desenvolvimento controlado por teste pode resultar em APIs mais limpo e mais compreensíveis.

Por fim, o desenvolvimento controlado por teste força você a escrever testes de unidade como parte do processo normal de escrever um aplicativo. Como se aproxima de um prazo final do projeto, teste normalmente é a primeira coisa que sai da janela. Ao praticar desenvolvimento orientado por testes, por outro lado, você está mais provável de ser virtuoso sobre como escrever testes de unidade porque o desenvolvimento controlado por teste faz testes de unidade central para o processo de criação de um aplicativo.

> [!NOTE] 
> 
> Para saber mais sobre o desenvolvimento controlado por teste, recomendo que você leia o livro de Michael Feathers **Working Effectively with Legacy Code**.


Essa iteração, adicionamos um novo recurso ao nosso aplicativo Contact Manager. Adicionamos suporte para grupos de contato. Você pode usar grupos de contato para organizar seus contatos em categorias, como Business e Friend.

Vamos adicionar essa nova funcionalidade ao nosso aplicativo seguindo um processo de desenvolvimento controlado por teste. Vamos escrever nossos testes de unidade pela primeira vez, e vamos escrever todo o nosso código contra esses testes.

## <a name="what-gets-tested"></a>O que é testado

Conforme discutimos na iteração anterior, você normalmente não escrever testes de unidade de lógica de acesso a dados ou exibir lógica. Você não t gravar testes de unidade para lógica de acesso a dados, como acessar um banco de dados é uma operação relativamente lenta. Você não t gravar testes de unidade para a lógica de exibição porque o acesso a um modo de exibição requer bagunçar um servidor web que é uma operação relativamente lenta. Você não deve gravar um teste de unidade, a menos que o teste pode ser executado repetidamente muito rápido

Porque o desenvolvimento controlado por teste é orientado por testes de unidade, vamos nos concentrar inicialmente em escrever a lógica de negócios e o controlador. Podemos evitar tocar o banco de dados ou exibições. Ganhamos t modificar o banco de dados ou criar nossos modos de exibição até o final deste tutorial. Vamos começar com o que pode ser testado.

## <a name="creating-user-stories"></a>Criar histórias de usuários

Ao praticar o desenvolvimento controlado por teste, você sempre começar escrevendo um teste. Isso gera imediatamente a pergunta: como você decide qual teste gravar primeiro? Para responder essa pergunta, você deve escrever um conjunto de [ *histórias de usuários*](http://en.wikipedia.org/wiki/User_stories).

Uma história de usuário é uma descrição (geralmente uma sentença) muito breve de um requisito de software. Ele deve ser uma descrição não técnica de um requisito escrito da perspectiva do usuário.

Veja o conjunto de histórias de usuários que descrevem os recursos necessários para a nova funcionalidade de grupo do contato:

1. Usuário pode exibir uma lista de grupos de contatos.
2. Usuário pode criar um novo grupo de contato.
3. Usuário pode excluir um grupo existente de contato.
4. Usuário pode selecionar um grupo de contatos ao criar um novo contato.
5. Usuário pode selecionar um grupo de contatos ao editar um contato existente.
6. Uma lista de grupos de contato é exibida no modo de exibição de índice.
7. Quando um usuário clica em um grupo de contatos, é exibida uma lista de contatos correspondentes.

Observe que essa lista de histórias de usuários é totalmente compreensível por um cliente. Não há nenhuma menção dos detalhes de implementação técnica.

Enquanto o processo de criação de seu aplicativo, o conjunto de histórias de usuários pode se tornar mais refinado. Você pode dividir uma história de usuário em várias matérias (requisitos). Por exemplo, você pode decidir criar um novo grupo de contato deve envolver validação. Enviar um grupo sem um nome de contato deve retornar um erro de validação.

Depois de criar uma lista de histórias de usuários, você está pronto para escrever seu primeiro teste de unidade. Vamos começar criando um teste de unidade para exibir a lista de grupos de contatos.

## <a name="listing-contact-groups"></a>Listar grupos de contatos

Nossa história de usuário a primeira é que um usuário deve ser capaz de exibir uma lista de grupos de contatos. É necessário expressar essa história com um teste.

Criar um novo teste de unidade pelo botão direito do mouse na pasta controladores no projeto ContactManager.Tests, selecionando **Add, New Test**e selecionando o **de teste de unidade** modelo (consulte a Figura 1). Nome da nova unidade GroupControllerTest.vb de teste e clique no **Okey** botão.


[![Adicionando o teste de unidade GroupControllerTest](iteration-6-use-test-driven-development-vb/_static/image1.jpg)](iteration-6-use-test-driven-development-vb/_static/image1.png)

**Figura 01**: adicionar o teste de unidade GroupControllerTest ([clique para exibir a imagem em tamanho normal](iteration-6-use-test-driven-development-vb/_static/image2.png))


Nosso primeiro teste de unidade está contido na listagem 1. Esse teste verifica que o método Index () do controlador grupo retorna um conjunto de grupos. O teste verifica que uma coleção de grupos é retornada no modo de exibição dados.

**Listagem 1 - Controllers\GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample1.vb)]

Ao digitar o código na listagem 1 no Visual Studio, você terá muitas linhas onduladas vermelhas. Não criamos as classes GroupController ou grupo.

Neste ponto, podemos compilação até mesmo de t nosso aplicativo, portanto, t, podemos executar nosso primeiro teste de unidade. Que bom s. Que conta como um teste que falhou. Portanto, agora temos permissão para começar a escrever o código do aplicativo. Precisamos escrever o código necessário para executar nosso teste.

A classe de controlador de grupo na listagem 2 contém o mínimo de código necessárias para passar no teste de unidade. A ação Index () retorna uma lista estaticamente codificada de grupos (a classe de grupo é definida na listagem 3).

**Listagem 2 - Controllers\GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample2.vb)]

**Listagem 3 - Models\Group.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample3.vb)]

Depois de adicionar as classes GroupController e o grupo ao nosso projeto, o nosso primeiro teste de unidade for concluída com êxito (veja a Figura 2). Fizemos o trabalho mínimo necessário para passar no teste. É hora de celebrar.


[![Sucesso!](iteration-6-use-test-driven-development-vb/_static/image2.jpg)](iteration-6-use-test-driven-development-vb/_static/image3.png)

**Figura 02**: sucesso! ( [Clique para exibir a imagem em tamanho normal](iteration-6-use-test-driven-development-vb/_static/image4.png))


## <a name="creating-contact-groups"></a>Criando grupos de contatos

Agora podemos pode mover para a segunda história de usuário. É preciso ser capaz de criar novos grupos de contato. É necessário expressar essa intenção com um teste.

O teste na listagem 4 verifica que chamando o método com um novo grupo adiciona o grupo à lista de grupos retornado pelo método Index () Create (). Em outras palavras, se eu criar um novo grupo, em seguida, eu deve ser capaz de obter o novo grupo de lista de grupos retornado pelo método Index ().

**Listagem 4 - Controllers\GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample4.vb)]

O teste na listagem 4 chama o método Create () com um novo grupo de contato do controlador de grupo. Em seguida, o teste verifica que chamar o método Index () do controlador de grupo retorna o novo grupo de dados de exibição.

O controlador de grupo modificado na listagem 5 contém o mínimo necessário das alterações necessárias para passar no teste de novo.

**Listagem 5 - Controllers\GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample5.vb)]

## <a name="the-group-controller-in-listing-5-has-a-new-create-action-this-action-adds-a-group-to-a-collection-of-groups-notice-that-the-index-action-has-been-modified-to-return-the-contents-of-the-collection-of-groups"></a>O controlador de grupo na listagem 5 tem uma nova ação de Create (). Essa ação adiciona um grupo a uma coleção de grupos. Observe que a ação Index () foi modificada para retornar o conteúdo da coleção de grupos.

Mais uma vez, poderíamos ter executado a quantidade mínima de trabalho necessária para passar no teste de unidade. Depois que podemos fazer essas alterações para o controlador de grupo, todos os de nossa unidade de testes de passagem.

## <a name="adding-validation"></a>Adicionando uma Validação

Esse requisito não foi declarado explicitamente na história do usuário. No entanto, é razoável exigir que um grupo tem um nome. Caso contrário, organizando os contatos em grupos não seria muito útil.

Listagem 6 contém um novo teste que expressa a intenção. Esse teste verifica se a tentativa de criar um grupo sem fornecer um resultados de nome em uma mensagem de erro de validação no estado do modelo.

**Listagem 6 - Controllers\GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample6.vb)]

Para satisfazer este teste, precisamos adicionar uma propriedade de nome a nossa classe de grupo (consulte a listagem 7). Além disso, precisamos adicionar uma pequena parte da lógica de validação ao nosso controlador grupo s ação Create () (consulte a listagem 8).

**Listagem 7 - Models\Group.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample7.vb)]

**Listagem 8 - Controllers\GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample8.vb)]

Observe que o controlador de grupo de ação Create () agora contém a lógica de validação e o banco de dados. Atualmente, o banco de dados usado pelo controlador de grupo consiste em nada mais do que uma coleção em memória.

## <a name="time-to-refactor"></a>Hora de refatoração

A terceira etapa vermelho/verde/de refatoração é a parte de refatoração. Neste ponto, precisamos do nosso código e considere como podemos refatorar nosso aplicativo para melhorar o seu design. O estágio de refatoração é o estágio em que achamos difícil sobre a melhor maneira de implementar os padrões e os princípios de design de software.

Estamos livres para modificar nosso código de qualquer forma que escolhemos para aprimorar o design do código. Temos uma rede de segurança de testes de unidade que nos impedem de interromper a funcionalidade existente.

Agora, nosso controlador de grupo está uma bagunça da perspectiva do bom design de software. O controlador de grupo contém uma bagunça complicada de validação e código de acesso a dados. Para evitar violando o princípio da responsabilidade única, é preciso separar essas preocupações em classes diferentes.

Nossa classe de controlador refatorado grupo está contida na listagem 9. O controlador foi modificado para usar a camada de serviço ContactManager. Isso é a mesma camada de serviço que usamos com o controlador de contato.

Listagem 10 contém os novos métodos adicionados à camada de serviço ContactManager para dar suporte à validação, listar e criar grupos. A interface IContactManagerService foi atualizada para incluir os novos métodos.

Listagem 11 contém uma nova classe FakeContactManagerRepository que implementa a interface IContactManagerRepository. Ao contrário da classe EntityContactManagerRepository que também implementa a interface IContactManagerRepository, nossa nova classe FakeContactManagerRepository não se comunica com o banco de dados. A classe FakeContactManagerRepository usa uma coleção em memória como um proxy para o banco de dados. Vamos usar essa classe em nossos testes de unidade como uma camada de repository falso.

**Listagem 9 - Controllers\GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample9.vb)]

**Listagem 10 - Controllers\ContactManagerService.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample10.vb)]

**Listagem 11 - Controllers\FakeContactManagerRepository.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample11.vb)]


Modificando o IContactManagerRepository interface requer que usar para implementar os métodos CreateGroup() e ListGroups() na classe EntityContactManagerRepository. A maneira mais rápida e laziest de fazer isso é adicionar os métodos stub que ter esta aparência:

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample12.vb)]


Por fim, essas alterações no design do nosso aplicativo exigem que fazer algumas modificações para nossos testes de unidade. Agora, precisamos usar o FakeContactManagerRepository ao executar os testes de unidade. A classe GroupControllerTest atualizada está contida na listagem 12.

**Listagem 12 - Controllers\GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample13.vb)]

Depois de fazer tudo isso, mais uma vez, todas as alterações de nossos testes de unidade aprovados. Concluímos todo o ciclo de vermelho/verde/de refatoração. Implementamos as histórias de dois usuário primeiro. Agora, temos que dão suporte a testes de unidade para os requisitos de expressa em histórias de usuários. Implementar o restante das histórias de usuários envolve o mesmo ciclo de vermelho/verde/de refatoração de repetição.

## <a name="modifying-our-database"></a>Modificar nosso banco de dados

Infelizmente, mesmo que podemos ter atendido todos os requisitos expressados por nossos testes de unidade, nosso trabalho não é feito. Ainda precisamos modificar nosso banco de dados.

É necessário criar uma nova tabela de banco de dados do grupo. Siga estas etapas:

1. Na janela do Gerenciador de servidores, clique com botão direito na pasta tabelas e selecione a opção de menu **adicionar nova tabela**.
2. Insira as duas colunas descritas abaixo no Designer de tabela.
3. Marque a coluna de Id como uma chave primária e uma coluna de identidade.
4. Salve a nova tabela com os grupos de nome clicando no ícone de disquete.

<a id="0.12_table01"></a>


| **Nome da coluna** | **Tipo de dados** | **Permitir nulos** |
| --- | --- | --- |
| Id | int | False |
| Nome | nvarchar(50) | False |


Em seguida, é preciso excluir todos os dados da tabela de contatos (caso contrário, ganhamos t ser capaz de criar uma relação entre as tabelas de contatos e grupos). Siga estas etapas:

1. A tabela de contatos com o botão direito e selecione a opção de menu **Mostrar dados da tabela**.
2. Exclua todas as linhas.

Em seguida, precisamos definir uma relação entre a tabela de banco de dados de grupos e a tabela de banco de dados de contatos existente. Siga estas etapas:

1. Clique duas vezes a tabela de contatos na janela Gerenciador de servidores para abrir o Designer de tabela.
2. Adicione uma nova coluna de inteiro à tabela Contatos denominada GroupId.
3. Clique no botão Relationship para abrir a caixa de diálogo relações de chave estrangeira (consulte a Figura 3).
4. Clique no botão Adicionar.
5. Clique no botão de reticências que aparece ao lado do botão de tabela e uma especificação de colunas.
6. Na caixa de diálogo tabelas e colunas, selecione grupos como a tabela de chave primária e a Id como a coluna de chave primária. Selecionar contatos como a tabela de chave estrangeira e a ID do grupo como a coluna de chave estrangeira (consulte a Figura 4). Clique no botão Okey.
7. Sob **especificação INSERT e UPDATE**, selecione o valor **Cascade** para **Excluir regra**.
8. Clique no botão Fechar para fechar a caixa de diálogo relações de chave estrangeira.
9. Clique no botão Salvar para salvar as alterações à tabela Contatos.


[![Criando uma relação de tabela do banco de dados](iteration-6-use-test-driven-development-vb/_static/image3.jpg)](iteration-6-use-test-driven-development-vb/_static/image5.png)

**Figura 03**: criar uma relação de tabela do banco de dados ([clique para exibir a imagem em tamanho normal](iteration-6-use-test-driven-development-vb/_static/image6.png))


[![Especificando relações de tabela](iteration-6-use-test-driven-development-vb/_static/image4.jpg)](iteration-6-use-test-driven-development-vb/_static/image7.png)

**Figura 04**: especificando relações de tabela ([clique para exibir a imagem em tamanho normal](iteration-6-use-test-driven-development-vb/_static/image8.png))


### <a name="updating-our-data-model"></a>Atualizando o nosso modelo de dados

Em seguida, precisamos atualizar nosso modelo de dados para representar a nova tabela de banco de dados. Siga estas etapas:

1. Clique duas vezes o arquivo ContactManagerModel.edmx na pasta de modelos para abrir o Designer de entidade.
2. A superfície do Designer com o botão direito e selecione a opção de menu **modelo de atualização do banco de dados**.
3. No Assistente de atualização, selecione os grupos de tabela e clique em Concluir botão (consulte a Figura 5).
4. A entidade de grupos com o botão direito e selecione a opção de menu **Renomear**. Altere o nome da *grupos* entidade *grupo* (singular).
5. Clique com botão direito a propriedade de navegação de grupos aparece na parte inferior da entidade contato. Altere o nome da *grupos* propriedade de navegação *grupo* (singular).


[![Atualizando um modelo do Entity Framework do banco de dados](iteration-6-use-test-driven-development-vb/_static/image5.jpg)](iteration-6-use-test-driven-development-vb/_static/image9.png)

**Figura 05**: atualizando um modelo do Entity Framework do banco de dados ([clique para exibir a imagem em tamanho normal](iteration-6-use-test-driven-development-vb/_static/image10.png))


Depois de concluir essas etapas, seu modelo de dados representará a tabelas de contatos e grupos. O Designer de entidade deve mostrar as duas entidades (veja a Figura 6).


[![Designer de entidade exibindo Group e Contact](iteration-6-use-test-driven-development-vb/_static/image6.jpg)](iteration-6-use-test-driven-development-vb/_static/image11.png)

**Figura 06**: Entity Designer exibindo Group e Contact ([clique para exibir a imagem em tamanho normal](iteration-6-use-test-driven-development-vb/_static/image12.png))


### <a name="creating-our-repository-classes"></a>Criando a nossas Classes de repositório

Em seguida, precisamos implementar nossa classe de repositório. Ao longo dessa iteração, adicionamos vários novos métodos na interface IContactManagerRepository durante a gravação de código para atender aos nossos testes de unidade. A versão final da interface IContactManagerRepository está contida na listagem 14.

**Listagem 14 - Models\IContactManagerRepository.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample14.vb)]

Podemos realmente implementados refúgio t e qualquer um dos métodos relacionados ao trabalho com grupos de contato em nossa classe EntityContactManagerRepository real. Atualmente, a classe EntityContactManagerRepository tem métodos stub para cada um dos métodos de contato de grupo listados na interface do IContactManagerRepository. Por exemplo, o método ListGroups() no momento, esta aparência:

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample15.vb)]

Os métodos stub permitiu-nos compilar nosso aplicativo e passar nos testes de unidade. No entanto, agora é hora de implementar, na verdade, esses métodos. A versão final da classe EntityContactManagerRepository está contida na listagem 13.

**Listagem 13 - Models\EntityContactManagerRepository.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample16.vb)]

### <a name="creating-the-views"></a>Criando os modos de exibição

Quando você usa o mecanismo de exibição do ASP.NET padrão de aplicativo do ASP.NET MVC. Portanto, don t criar modos de exibição em resposta a um teste de unidade específico. No entanto, porque um aplicativo seria inútil sem exibições, podemos t concluir essa iteração sem criar e modificar os modos de exibição contidos no aplicativo Gerenciador de contatos.

É preciso criar as novas exibições a seguir para gerenciar grupos de contato (veja a Figura 7):

- Views\Group\Index.aspx - exibe a lista de grupos de contatos
- Views\Group\Delete.aspx - formulário de confirmação exibida para a exclusão de um grupo de contatos


[![O modo de exibição de índice do grupo](iteration-6-use-test-driven-development-vb/_static/image7.jpg)](iteration-6-use-test-driven-development-vb/_static/image13.png)

**Figura 07**: exibição o índice do grupo ([clique para exibir a imagem em tamanho normal](iteration-6-use-test-driven-development-vb/_static/image14.png))


Precisamos modificar as seguintes exibições existentes para que elas incluem grupos de contato:

- Views\Home\Create.aspx
- Views\Home\Edit.aspx
- Views\Home\Index.aspx

Você pode ver os modos de exibição modificados, observando o aplicativo do Visual Studio que acompanha este tutorial. Por exemplo, a Figura 8 ilustra o modo de exibição de índice de contatos.


[![O modo de exibição de índice de contatos](iteration-6-use-test-driven-development-vb/_static/image8.jpg)](iteration-6-use-test-driven-development-vb/_static/image15.png)

**Figura 08**: exibição o índice de contato ([clique para exibir a imagem em tamanho normal](iteration-6-use-test-driven-development-vb/_static/image16.png))


## <a name="summary"></a>Resumo

Essa iteração, adicionamos novas funcionalidades ao nosso aplicativo Contact Manager seguindo uma metodologia de design de aplicativo de desenvolvimento controlado por teste. Começamos criando um conjunto de histórias de usuários. Criamos um conjunto de testes de unidade que corresponde aos requisitos expressados pelas histórias de usuários. Por fim, escrevemos código apenas o suficiente para satisfazer os requisitos expressados por testes de unidade.

Depois de concluída escrevendo código suficiente para satisfazer os requisitos expressados por testes de unidade, atualizamos nosso banco de dados e modos de exibição. Adicionamos uma nova tabela de grupos para nosso banco de dados e atualizado o nosso modelo de dados do Entity Framework. Também podemos criado e modificado de um conjunto de exibições.

Na próxima iteração – a iteração final – podemos reescrever o nosso aplicativo para tirar proveito do Ajax. Ao tirar proveito do Ajax, podemos irá melhorar a capacidade de resposta e o desempenho do aplicativo Gerenciador de contatos.

> [!div class="step-by-step"]
> [Anterior](iteration-5-create-unit-tests-vb.md)
> [Próximo](iteration-7-add-ajax-functionality-vb.md)
