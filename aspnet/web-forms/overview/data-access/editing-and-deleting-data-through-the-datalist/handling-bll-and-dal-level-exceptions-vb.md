---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-vb
title: Tratamento de exceções de nível BLL e DAL (VB) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, veremos como tactfully tratar as exceções geradas durante o fluxo de trabalho de atualização do DataList um editável.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: ca665073-b379-4239-9404-f597663ca65e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-vb
msc.type: authoredcontent
ms.openlocfilehash: 0655f73b63f3eae72a866c912589779a8ccc7664
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400558"
---
<a name="handling-bll--and-dal-level-exceptions-vb"></a>Tratamento de exceções de nível BLL e DAL (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_38_VB.exe) ou [baixar PDF](handling-bll-and-dal-level-exceptions-vb/_static/datatutorial38vb1.pdf)

> Neste tutorial, veremos como tactfully tratar as exceções geradas durante o fluxo de trabalho de atualização do DataList um editável.


## <a name="introduction"></a>Introdução

No [visão geral de edição e exclusão de dados no DataList](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md) tutorial, criamos uma DataList que oferecidos simples de edição e exclusão de recursos. Embora seja totalmente funcional, foi mal amigável, como os erros ocorridos durante a edição ou exclusão de processo resultou em uma exceção sem tratamento. Por exemplo, omitindo o nome do produto s ou, ao editar um produto, inserir um valor de preço de muito acessível!, gera uma exceção. Uma vez que essa exceção não for capturada no código, ele propaga-se até o tempo de execução do ASP.NET, que, em seguida, exibe os detalhes da exceção s na página da web.

Como vimos na [tratamento BLL - e exceções de nível DAL em uma página ASP.NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) tutorial, se uma exceção for gerada das profundezas da lógica de negócios ou camadas de acesso de dados, os detalhes da exceção são retornados para o ObjectDataSource e em seguida, para o GridView. Vimos como lidar normalmente com essas exceções, criando `Updated` ou `RowUpdated` manipuladores de eventos para o ObjectDataSource ou GridView, verificando uma exceção e, em seguida, indicando que a exceção foi tratada.

Nossos DataList tutoriais, no entanto, t não estão usando o ObjectDataSource para atualizar e excluir dados. Em vez disso, estamos trabalhando diretamente em relação a BLL. Para detectar exceções originadas da BLL ou DAL, precisamos implementar o código no code-behind da página ASP.NET de tratamento de exceções. Neste tutorial, veremos como mais tactfully tratar as exceções geradas durante um s DataList editável atualizando fluxo de trabalho.

> [!NOTE]
> No *An de visão geral de edição e exclusão de dados no DataList* envolvido de algumas técnicas de tutorial discutimos diferentes técnicas para edição e exclusão de dados do DataList, usando um ObjectDataSource para a atualização e a exclusão. Se você usar essas técnicas, você pode tratar exceções da BLL ou DAL por meio de s ObjectDataSource `Updated` ou `Deleted` manipuladores de eventos.


## <a name="step-1-creating-an-editable-datalist"></a>Etapa 1: Criando um DataList editável

Antes de nos preocupamos com tratamento de exceções que ocorrem durante o fluxo de trabalho de atualização, deixe s primeiro criar uma DataList editável. Abra o `ErrorHandling.aspx` página na `EditDeleteDataList` pasta, adicionar uma DataList ao Designer de conjunto seu `ID` propriedade a ser `Products`, e adicione um novo ObjectDataSource chamado `ProductsDataSource`. Configurar o ObjectDataSource para usar o `ProductsBLL` classe s `GetProducts()` método para selecionar registros; definir as listas suspensas em INSERT, UPDATE e excluir guias como (nenhum).


[![Retornar as informações de produto usando o método GetProducts()](handling-bll-and-dal-level-exceptions-vb/_static/image2.png)](handling-bll-and-dal-level-exceptions-vb/_static/image1.png)

**Figura 1**: retornar as informações de produto usando o `GetProducts()` método ([clique para exibir a imagem em tamanho normal](handling-bll-and-dal-level-exceptions-vb/_static/image3.png))


Depois de concluir o assistente ObjectDataSource, Visual Studio criará automaticamente um `ItemTemplate` para DataList. Substitua-o por um `ItemTemplate` que exibe o nome do produto s e o preço e inclui um botão Editar. Em seguida, crie um `EditItemTemplate` com um controle de TextBox Web para o nome e o preço e botões de atualização e Cancelar. Por fim, defina DataList s `RepeatColumns` propriedade como 2.

Após essas alterações, sua marcação declarativa de s de página deve ser semelhante ao seguinte. Verifique novamente para fazer a certeza de que a edição, cancelar, e botões de atualização têm seus `CommandName` propriedades definidas para editar, cancelar e atualizar, respectivamente.


[!code-aspx[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample1.aspx)]

> [!NOTE]
> Para este tutorial DataList o estado de exibição s deve ser habilitado.


Reserve um tempo para exibir nosso progresso através de um navegador (veja a Figura 2).


[![Cada produto inclui um botão Editar](handling-bll-and-dal-level-exceptions-vb/_static/image5.png)](handling-bll-and-dal-level-exceptions-vb/_static/image4.png)

**Figura 2**: cada produto inclui um botão Editar ([clique para exibir a imagem em tamanho normal](handling-bll-and-dal-level-exceptions-vb/_static/image6.png))


Atualmente, o botão Editar apenas faz com que um postback-t ainda tornam o produto editável. Para habilitar a edição, precisamos criar manipuladores de eventos para o s DataList `EditCommand`, `CancelCommand`, e `UpdateCommand` eventos. O `EditCommand` e `CancelCommand` eventos simplesmente atualizar DataList s `EditItemIndex` reassociar os dados para o DataList e propriedade:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample2.vb)]

O `UpdateCommand` manipulador de eventos é um pouco mais complicado. Ele precisa ler no produto editado s `ProductID` do `DataKeys` coleção juntamente com o nome do produto s e o preço de TextBoxes a `EditItemTemplate`e, em seguida, chame o `ProductsBLL` classe s `UpdateProduct` método antes de retornar do DataList para seu estado de edição previamente.

Por enquanto, let s apenas usar o mesmo código do `UpdateCommand` manipulador de eventos em de *visão geral de edição e exclusão de dados no DataList* tutorial. Adicionaremos o código para tratar normalmente exceções na etapa 2.


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample3.vb)]

Diante de uma entrada inválida, que pode ser na forma de um preço de unidade formatada incorretamente, um valor de preço de unidade ilegal, como - US $5,00 ou a omissão do nome de s do produto, que uma exceção será gerada. Uma vez que o `UpdateCommand` manipulador de eventos não inclui qualquer neste ponto de código de tratamento de exceção, a exceção serão propagados o tempo de execução do ASP.NET, onde ele será exibido para o usuário final (veja a Figura 3).


![Quando ocorre uma exceção sem tratamento, o usuário final vê uma página de erro](handling-bll-and-dal-level-exceptions-vb/_static/image7.png)

**Figura 3**: quando ocorre uma exceção sem tratamento, o usuário final vê uma página de erro


## <a name="step-2-gracefully-handling-exceptions-in-the-updatecommand-event-handler"></a>Etapa 2: Normalmente tratamento de exceções no manipulador de eventos UpdateCommand

Durante o fluxo de trabalho de atualização, poderão ocorrer exceções no `UpdateCommand` a DAL, a BLL ou manipulador de eventos. Por exemplo, se um usuário inserir um preço de muito caro, o `Decimal.Parse` instrução na `UpdateCommand` lançará o manipulador de eventos um `FormatException` exceção. Se o usuário omite o nome do produto s ou se o preço tem um valor negativo, a DAL irá gerar uma exceção.

Quando ocorre uma exceção, queremos exibir uma mensagem informativa da página em si. Adicionar uma Web de rótulo de controle para a página cuja `ID` é definido como `ExceptionDetails`. Configurar o texto do rótulo s para exibir em vermelho, extra grande, a fonte em negrito e itálico, atribuindo suas `CssClass` propriedade para o `Warning` classe CSS, que é definido no `Styles.css` arquivo.

Quando ocorre um erro, só queremos o rótulo a ser exibido uma vez. Ou seja, em postbacks subsequentes, a mensagem de aviso de rótulo s deve desaparecer. Isso pode ser feito limpando qualquer rótulo s `Text` propriedade ou as configurações de seu `Visible` propriedade a ser `False` no `Page_Load` manipulador de eventos (como fizemos no [BLL tratando - e exceções de nível DAL em um ASP Página do .NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md) tutorial) ou desativando o suporte de estado de exibição de rótulo s. Permitir que o s use a última opção.


[!code-aspx[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample4.aspx)]

Quando uma exceção é gerada, vamos atribuir os detalhes da exceção para o `ExceptionDetails` s de controle de rótulo `Text` propriedade. Uma vez que seu estado de exibição estiver desabilitado, em postbacks subsequentes a `Text` alterações programáticas de propriedade s serão perdidas, reverter para o texto padrão (uma cadeia de caracteres vazia), ocultando assim a mensagem de aviso.

Para determinar quando um erro foi ativado para exibir uma mensagem útil na página, precisamos adicionar um `Try ... Catch` bloco para o `UpdateCommand` manipulador de eventos. O `Try` parte contém o código que pode levar a uma exceção, enquanto o `Catch` bloco contém código que é executado diante de uma exceção. Confira o [conceitos básicos de manipulação de exceção](https://msdn.microsoft.com/library/2w8f0bss.aspx) seção na documentação do .NET Framework para obter mais informações sobre o `Try ... Catch` bloco.


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample5.vb)]

Quando uma exceção de qualquer tipo é lançada pelo código dentro de `Try` bloco, o `Catch` código do bloco s inicia a execução. O tipo de exceção que é lançada `DbException`, `NoNullAllowedException`, `ArgumentException`e assim por diante depende de qual, exatamente, precipitou o erro em primeiro lugar. Se há um problema de s no nível do banco de dados, um `DbException` será lançada. Se um valor inválido for inserido para o `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, ou `ReorderLevel` campos, uma `ArgumentException` será gerada, como adicionamos código para validar esses valores de campo no `ProductsDataTable` classe (consulte a [ Criando uma camada de lógica de negócios](../introduction/creating-a-business-logic-layer-vb.md) tutorial).

Podemos fornecer uma explicação mais úteis para o usuário final baseando-se o texto da mensagem no tipo de exceção capturada. O seguinte código que foi usado de forma praticamente idêntica volta a [tratamento BLL - e exceções de nível DAL em uma página ASP.NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md) tutorial fornece esse nível de detalhe:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample6.vb)]

Para concluir este tutorial, simplesmente chame o `DisplayExceptionDetails` método a partir de `Catch` bloco passando capturado `Exception` instância (`ex`).

Com o `Try ... Catch` bloquear em vigor, os usuários são apresentados com uma mensagem de erro mais informativa, como as figuras 4 e 5 show. Observe que, diante de uma exceção DataList permanece no modo de edição. Isso ocorre porque quando a exceção ocorre, o fluxo de controle é imediatamente redirecionado para o `Catch` bloco, ignorando o código que retorna DataList para seu estado de edição previamente.


[![Uma mensagem de erro será exibida se um usuário omite um campo obrigatório](handling-bll-and-dal-level-exceptions-vb/_static/image9.png)](handling-bll-and-dal-level-exceptions-vb/_static/image8.png)

**Figura 4**: uma mensagem de erro será exibida se um usuário omite um campo obrigatório ([clique para exibir a imagem em tamanho normal](handling-bll-and-dal-level-exceptions-vb/_static/image10.png))


[![Uma mensagem de erro é exibido ao inserir um preço negativo](handling-bll-and-dal-level-exceptions-vb/_static/image12.png)](handling-bll-and-dal-level-exceptions-vb/_static/image11.png)

**Figura 5**: uma mensagem de erro é exibido ao inserir um preço negativo ([clique para exibir a imagem em tamanho normal](handling-bll-and-dal-level-exceptions-vb/_static/image13.png))


## <a name="summary"></a>Resumo

O GridView e ObjectDataSource oferecem manipuladores de evento de pós-nível que incluem informações sobre quaisquer exceções que foram geradas durante o fluxo de trabalho de atualização e exclusão, bem como as propriedades que podem ser definidas para indicar se a exceção tiver sido manipulado. Esses recursos, no entanto, não estão disponíveis ao trabalhar com o DataList e usando a BLL diretamente. Em vez disso, somos responsáveis por implementar o tratamento de exceção.

Neste tutorial vimos como adicionar tratamento de exceções para um s DataList editável atualizar o fluxo de trabalho adicionando um `Try ... Catch` bloco para o `UpdateCommand` manipulador de eventos. Se uma exceção for gerada durante o fluxo de trabalho de atualização, o `Catch` s de bloco de código é executado, exibindo informações úteis no `ExceptionDetails` rótulo.

Neste ponto, DataList não faz nenhum esforço para impedir exceções aconteçam em primeiro lugar. Mesmo que nós sabemos que um preço negativo resultará em uma exceção, podemos foram t ainda qualquer funcionalidade adicionada para proativamente impedir que um usuário insira essa entrada inválida. Em nosso próximo tutorial, veremos como ajudar a reduzir as exceções causadas pela entrada de usuário inválido, adicionando controles de validação no `EditItemTemplate`.

Boa programação!

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Diretrizes de design para exceções](https://msdn.microsoft.com/library/ms298399.aspx)
- [Módulos de log de erros e manipuladores (ELMAH)](http://workspaces.gotdotnet.com/elmah) (uma biblioteca de código-fonte aberto para o log de erros)
- [A Enterprise Library para o .NET Framework 2.0](https://www.microsoft.com/downloads/details.aspx?familyid=5A14E870-406B-4F2A-B723-97BA84AE80B5&amp;displaylang=en) (inclui o Exception Management Application Block)

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Revisor de avanço para este tutorial foi Ken Pespisa. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](performing-batch-updates-vb.md)
> [Próximo](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md)
