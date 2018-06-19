---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-vb
title: Tratamento de exceções de nível BLL e DAL (VB) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, veremos como tactfully lidar com exceções geradas durante o fluxo de trabalho de atualização do DataList um editável.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: ca665073-b379-4239-9404-f597663ca65e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-vb
msc.type: authoredcontent
ms.openlocfilehash: 0ee0eeff08b6ba490b401540de833eecd7122a17
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30880096"
---
<a name="handling-bll--and-dal-level-exceptions-vb"></a>Tratamento de exceções de nível BLL e DAL (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_38_VB.exe) ou [baixar PDF](handling-bll-and-dal-level-exceptions-vb/_static/datatutorial38vb1.pdf)

> Neste tutorial, veremos como tactfully lidar com exceções geradas durante o fluxo de trabalho de atualização do DataList um editável.


## <a name="introduction"></a>Introdução

No [visão geral de editar e excluir dados em DataList](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md) tutorial, criamos uma DataList que oferecidos simples de editar e excluir recursos. Enquanto totalmente funcional, foi dificilmente amigável, como qualquer erro que ocorreu durante a edição ou exclusão de processo resultou em uma exceção sem tratamento. Por exemplo, omitindo o nome do produto s ou, ao editar um produto, inserindo um valor do preço de muito acessível!, gera uma exceção. Desde que essa exceção não for detectada no código, ele bolhas até o tempo de execução do ASP.NET, que, em seguida, exibe os detalhes da exceção s na página da web.

Como vimos no [tratamento BLL e DAL nível exceções em uma página ASP.NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) tutorial, se uma exceção é gerada de fundo da lógica de negócios ou camadas de acesso de dados, os detalhes da exceção são retornados para o ObjectDataSource e em seguida, para o GridView. Vimos como tratar essas exceções normalmente criando `Updated` ou `RowUpdated` manipuladores de eventos para o ObjectDataSource ou GridView, verificando uma exceção e, em seguida, indicando que a exceção foi tratada.

Nosso DataList tutoriais, no entanto, t usando ObjectDataSource para atualizar e excluir dados. Em vez disso, estamos trabalhando diretamente no BLL. Para detectar exceções originadas do BLL ou DAL, é preciso implementar o código dentro do code-behind da página ASP.NET manipulação de exceção. Neste tutorial, veremos como mais tactfully lidar com exceções geradas durante um s DataList editável atualizar fluxo de trabalho.

> [!NOTE]
> No *uma visão geral da edição de e excluindo dados em DataList* envolvidos de algumas técnicas de tutorial discutimos técnicas diferentes para editar e excluir dados de DataList, usando um ObjectDataSource para a atualização e a exclusão. Se você usar essas técnicas, você pode manipular exceções da DAL ou BLL através da s ObjectDataSource `Updated` ou `Deleted` manipuladores de eventos.


## <a name="step-1-creating-an-editable-datalist"></a>Etapa 1: Criando um DataList editável

Antes de nos preocupamos com tratamento de exceções que ocorrem durante o fluxo de trabalho de atualização, deixe-s primeiro criar um DataList editável. Abrir o `ErrorHandling.aspx` página o `EditDeleteDataList` adicionar DataList ao Designer de conjunto de pastas, seu `ID` propriedade `Products`, e adicione um novo ObjectDataSource denominado `ProductsDataSource`. Configurar o ObjectDataSource para usar o `ProductsBLL` classe s `GetProducts()` método para selecionar registros; defina a lista suspensa na instrução INSERT, UPDATE e excluir guias como (nenhum).


[![Retornar as informações de produto usando o método GetProducts()](handling-bll-and-dal-level-exceptions-vb/_static/image2.png)](handling-bll-and-dal-level-exceptions-vb/_static/image1.png)

**Figura 1**: retornar as informações de produto usando o `GetProducts()` método ([clique para exibir a imagem em tamanho normal](handling-bll-and-dal-level-exceptions-vb/_static/image3.png))


Depois de concluir o assistente ObjectDataSource, Visual Studio criará automaticamente um `ItemTemplate` para DataList. Substitua-o com um `ItemTemplate` que exibe cada nome de produto s e o preço e inclui um botão de edição. Em seguida, crie um `EditItemTemplate` com um controle de Web de caixa de texto para o nome e o preço e botões de atualização e Cancelar. Finalmente, defina o DataList s `RepeatColumns` propriedade para 2.

Após essas alterações, a marcação declarativa de s de página deve ser semelhante ao seguinte. Verifique se a edição, cancelar, e tem botões atualizar seus `CommandName` propriedades definidas para editar, cancelar e atualizar, respectivamente.


[!code-aspx[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample1.aspx)]

> [!NOTE]
> Estado de exibição de s deve ser habilitado para este tutorial do DataList.


Reserve um tempo para exibir nosso andamento por meio de um navegador (consulte a Figura 2).


[![Cada produto inclui um botão de edição](handling-bll-and-dal-level-exceptions-vb/_static/image5.png)](handling-bll-and-dal-level-exceptions-vb/_static/image4.png)

**Figura 2**: cada produto inclui um botão Editar ([clique para exibir a imagem em tamanho normal](handling-bll-and-dal-level-exceptions-vb/_static/image6.png))


Atualmente, o botão Editar somente causa um postback-t ainda tornam o produto editável. Para habilitar a edição, precisamos criar manipuladores de eventos para o DataList s `EditCommand`, `CancelCommand`, e `UpdateCommand` eventos. O `EditCommand` e `CancelCommand` eventos simplesmente atualizar DataList s `EditItemIndex` propriedade e reassociar os dados para o DataList:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample2.vb)]

O `UpdateCommand` manipulador de eventos é um pouco mais envolvido. Precisa ler no produto editado s `ProductID` do `DataKeys` coleção junto com o nome do produto s e o preço de caixas de texto no `EditItemTemplate`e, em seguida, chame o `ProductsBLL` classe s `UpdateProduct` método antes de retornar o DataList estado de edição previamente.

Por enquanto, vamos s usar apenas exatamente o mesmo código do `UpdateCommand` manipulador de eventos de *visão geral de editar e excluir dados em DataList* tutorial. Vamos adicionar o código para manipular normalmente as exceções na etapa 2.


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample3.vb)]

No caso de entrada inválida, que pode ser na forma de um preço de unidade formatada incorretamente, um valor inválido de unidade como - US $5,00 ou a omissão do nome do produto que será gerada uma exceção. Desde o `UpdateCommand` manipulador de eventos não inclue qualquer código manipulação de exceção neste ponto, a exceção bolha até o tempo de execução do ASP.NET, onde ele será exibido para o usuário final (consulte a Figura 3).


![Quando ocorre uma exceção sem tratamento, o usuário final verá uma página de erro](handling-bll-and-dal-level-exceptions-vb/_static/image7.png)

**Figura 3**: quando ocorre uma exceção sem tratamento, o usuário final verá uma página de erro


## <a name="step-2-gracefully-handling-exceptions-in-the-updatecommand-event-handler"></a>Etapa 2: Normalmente tratamento de exceções no manipulador de eventos de UpdateCommand

Durante o fluxo de trabalho de atualização, as exceções podem ocorrer no `UpdateCommand` DAL, BLL ou manipulador de eventos. Por exemplo, se um usuário inserir um preço de muito caras, a `Decimal.Parse` instrução no `UpdateCommand` lançará o manipulador de eventos uma `FormatException` exceção. Se o usuário omite o nome do produto s ou se o preço tiver um valor negativo, a DAL gerará uma exceção.

Quando ocorre uma exceção, queremos exibir uma mensagem informativa na própria página. Adicionar uma Web de rótulo de controle para a página cuja `ID` é definido como `ExceptionDetails`. Configurar o texto do rótulo s para exibir em vermelho, extra grande, a fonte em negrito e itálico atribuindo seu `CssClass` propriedade para o `Warning` classe CSS, que é definido no `Styles.css` arquivo.

Quando ocorre um erro, só queremos o rótulo a ser exibido uma vez. Isto é, em postagens subsequentes, a mensagem de aviso de rótulo s deve desaparecer. Isso pode ser feito limpando o rótulo s `Text` propriedade ou configurações de seu `Visible` propriedade `False` no `Page_Load` manipulador de eventos (como foi feito no [tratamento BLL e DAL nível exceções em um ASP Página de .NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md) tutorial) ou desativando o suporte de estado de exibição de rótulo s. Permitir que o s use a última opção.


[!code-aspx[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample4.aspx)]

Quando uma exceção é gerada, podemos atribuirá os detalhes da exceção para o `ExceptionDetails` rótulo de controle s `Text` propriedade. Uma vez que seu estado de exibição é desabilitado em postagens subsequentes a `Text` alterações de propriedade s programático serão perdidas, reverter para o texto padrão (uma cadeia de caracteres vazia), assim, ocultar a mensagem de aviso.

Para determinar quando um erro foi gerado para exibir uma mensagem útil na página, precisamos adicionar um `Try ... Catch` bloquear o `UpdateCommand` manipulador de eventos. O `Try` parte contém código que pode levar a uma exceção, enquanto o `Catch` bloco contém código que é executado no caso de uma exceção. Check-out de [conceitos básicos de tratamento de exceção](https://msdn.microsoft.com/library/2w8f0bss.aspx) seção na documentação do .NET Framework para obter mais informações sobre o `Try ... Catch` bloco.


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample5.vb)]

Quando uma exceção de qualquer tipo é gerada pelo código dentro de `Try` bloco, o `Catch` código do bloco s começará a executar. O tipo de exceção que é lançada `DbException`, `NoNullAllowedException`, `ArgumentException`e assim por diante depende de qual, exatamente, originou a necessidade do erro em primeiro lugar. Se há um problema de s no nível do banco de dados, um `DbException` será lançada. Se for digitado um valor inválido para o `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, ou `ReorderLevel` campos, um `ArgumentException` será gerada, como adicionamos código para validar esses valores de campo no `ProductsDataTable` classe (consulte o [ Criando uma camada de lógica de negócios](../introduction/creating-a-business-logic-layer-vb.md) tutorial).

Podemos fornecer uma explicação mais úteis para o usuário final baseando-se o texto da mensagem do tipo de exceção detectada. O código a seguir que foi usado de forma praticamente idêntica no [tratamento BLL e DAL nível exceções em uma página ASP.NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md) tutorial fornece esse nível de detalhe:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample6.vb)]

Para concluir este tutorial, simplesmente chamar o `DisplayExceptionDetails` método do `Catch` bloco passando o capturada `Exception` instância (`ex`).

Com o `Try ... Catch` bloco em vigor, os usuários são apresentados com uma mensagem de erro mais informativa, como figuras 4 e 5 Mostrar. Observe que, no caso de uma exceção do DataList permanece no modo de edição. Isso ocorre porque quando a exceção ocorre, o fluxo de controle é redirecionado imediatamente para o `Catch` bloco, ignorando o código que retorna o DataList para seu estado de edição previamente.


[![Uma mensagem de erro será exibida se um usuário omite um campo necessário](handling-bll-and-dal-level-exceptions-vb/_static/image9.png)](handling-bll-and-dal-level-exceptions-vb/_static/image8.png)

**Figura 4**: uma mensagem de erro será exibida se um usuário omite um campo necessário ([clique para exibir a imagem em tamanho normal](handling-bll-and-dal-level-exceptions-vb/_static/image10.png))


[![Uma mensagem de erro é exibido ao inserir um preço negativo](handling-bll-and-dal-level-exceptions-vb/_static/image12.png)](handling-bll-and-dal-level-exceptions-vb/_static/image11.png)

**Figura 5**: uma mensagem de erro é exibido ao inserir um preço negativo ([clique para exibir a imagem em tamanho normal](handling-bll-and-dal-level-exceptions-vb/_static/image13.png))


## <a name="summary"></a>Resumo

O GridView e ObjectDataSource fornecem manipuladores de evento de pós-nível que incluem informações sobre quaisquer exceções que foram geradas durante o fluxo de trabalho de atualização e exclusão, bem como as propriedades que podem ser definidas para indicar se a exceção foi tratado. Esses recursos, no entanto, não estão disponíveis quando trabalhando com DataList e usando BLL diretamente. Em vez disso, somos responsáveis para implementar o tratamento de exceção.

Neste tutorial, vimos como adicionar tratamento de exceções para s DataList editável atualizar o fluxo de trabalho adicionando um `Try ... Catch` bloquear o `UpdateCommand` manipulador de eventos. Se uma exceção é gerada durante o fluxo de trabalho de atualização, o `Catch` executa código do bloco s, exibindo informações úteis no `ExceptionDetails` rótulo.

Neste ponto, DataList não faz nenhum esforço para evitar exceções de acontecer em primeiro lugar. Mesmo que sabemos que um preço negativo resultará em uma exceção, podemos ainda t ainda adicionada funcionalidade para evitar proativamente um usuário de inserir essa entrada inválida. Em nosso tutorial Avançar veremos como ajudar a reduzir as exceções causadas por entrada de usuário inválido com a adição de controles de validação no `EditItemTemplate`.

Boa programação!

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Diretrizes de design para exceções](https://msdn.microsoft.com/library/ms298399.aspx)
- [Módulos de log de erros e manipuladores (ELMAH)](http://workspaces.gotdotnet.com/elmah) (uma biblioteca de código-fonte aberto para o log de erros)
- [Enterprise Library para o .NET Framework 2.0](https://www.microsoft.com/downloads/details.aspx?familyid=5A14E870-406B-4F2A-B723-97BA84AE80B5&amp;displaylang=en) (inclui o bloco de aplicativo de gerenciamento de exceção)

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisor levar para este tutorial foi Ken Pespisa. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](performing-batch-updates-vb.md)
> [Próximo](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md)
