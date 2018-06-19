---
uid: web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs
title: Configurando programaticamente os valores do parâmetro ObjectDataSource (c#) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, examinaremos adicionando um método para nossos DAL e BLL que aceita um único parâmetro de entrada e retorna dados. O exemplo definirá esse parâmetro...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 1c4588bb-255d-4088-b319-5208da756f4d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs
msc.type: authoredcontent
ms.openlocfilehash: 1bd1fd63e5aae74459675d45dd399e449d7897b8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875676"
---
<a name="programmatically-setting-the-objectdatasources-parameter-values-c"></a>Configurando programaticamente os valores do parâmetro ObjectDataSource (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_6_CS.exe) ou [baixar PDF](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/datatutorial06cs1.pdf)

> Neste tutorial, examinaremos adicionando um método para nossos DAL e BLL que aceita um único parâmetro de entrada e retorna dados. O exemplo definirá esse parâmetro programaticamente.


## <a name="introduction"></a>Introdução

Como vimos no [tutorial anterior](declarative-parameters-cs.md), várias opções estão disponíveis para declarativamente passar valores de parâmetro para métodos do ObjectDataSource. Se o valor do parâmetro é codificado, vem de um controle da Web na página ou está em qualquer outra fonte que pode ser lido por uma fonte de dados `Parameter` do objeto, por exemplo, que o valor pode ser associado ao parâmetro de entrada sem escrever uma linha de código.

Pode haver ocasiões, entretanto, quando o valor do parâmetro é fornecido de origem ainda não contabilizada por uma fonte de dados internos `Parameter` objetos. Se nosso site de suporte para contas de usuário, talvez queira definir o parâmetro com base em atualmente conectado na ID de usuário. do visitante Ou, talvez seja necessário personalizar o valor do parâmetro antes de enviá-lo ao longo de método do objeto subjacente do ObjectDataSource.

Sempre que o ObjectDataSource `Select` método é invocado pela primeira vez dispara o ObjectDataSource seu [evento Selecting](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting%28VS.80%29.aspx). Método do objeto do ObjectDataSource subjacente é invocado em seguida. Depois de concluir o ObjectDataSource [eventos selecionados](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selected%28VS.80%29.aspx) acionado (Figura 1 mostra esta sequência de eventos). Os valores de parâmetro passados para o método do objeto do ObjectDataSource subjacente podem ser definidos ou personalizados em um manipulador de eventos para o `Selecting` evento.


[![O ObjectDataSource selecionados e selecionar eventos disparados antes e do depois de seu objeto subjacente método é invocado](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image2.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image1.png)

**Figura 1**: do ObjectDataSource `Selected` e `Selecting` eventos disparados antes e do depois de seu objeto subjacente método é invocado ([clique para exibir a imagem em tamanho normal](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image3.png))


Neste tutorial, examinaremos adicionando um método para nossos DAL e BLL que aceita um único parâmetro de entrada `Month`, do tipo `int` e retorna um `EmployeesDataTable` objeto preenchido com os funcionários que têm seu aniversário contratação do `Month`. Nosso exemplo definirá esse parâmetro programaticamente com base no mês atual, mostrando uma lista de "Funcionário aniversários deste mês".

Vamos começar!

## <a name="step-1-adding-a-method-toemployeestableadapter"></a>Etapa 1: Adicionar um método`EmployeesTableAdapter`

Em nosso exemplo, primeiro, precisamos adicionar um meio para recuperar os funcionários cujo `HireDate` ocorreu em um mês especificado. Para fornecer essa funcionalidade de acordo com nossa arquitetura é preciso primeiro criar um método em `EmployeesTableAdapter` que mapeia para a instrução SQL apropriada. Para fazer isso, comece abrindo o conjunto de dados tipado do Northwind. Clique com botão direito no `EmployeesTableAdapter` rótulo e escolha Adicionar consulta.


[![Adicionar uma nova consulta para o EmployeesTableAdapter](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image5.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image4.png)

**Figura 2**: adicionar uma nova consulta para o `EmployeesTableAdapter` ([clique para exibir a imagem em tamanho normal](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image6.png))


Escolha esta opção Adicionar uma instrução SQL que retorna linhas. Quando você chegar ao especificar um `SELECT` o padrão de tela de instrução `SELECT` instrução para o `EmployeesTableAdapter` já será carregado. Basta adicionar no `WHERE` cláusula: `WHERE DATEPART(m, HireDate) = @Month`. [DATEPART](https://msdn.microsoft.com/library/ms174420.aspx) é uma função de T-SQL que retorna uma parte de data específica de um `datetime` tipo; nesse caso estamos usando `DATEPART` para retornar o mês do `HireDate` coluna.


[![Retorno apenas as linhas onde a coluna HireDate é menor ou igual a @HiredBeforeDate parâmetro](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image8.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image7.png)

**Figura 3**: retornar somente as linhas onde a `HireDate` coluna for menor ou igual ao `@HiredBeforeDate` parâmetro ([clique para exibir a imagem em tamanho normal](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image9.png))


Por fim, altere o `FillBy` e `GetDataBy` nomes de método para `FillByHiredDateMonth` e `GetEmployeesByHiredDateMonth`, respectivamente.


[![Escolha nomes de método mais apropriados que FillBy e GetDataBy](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image11.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image10.png)

**Figura 4**: escolha mais apropriado método nomes que `FillBy` e `GetDataBy` ([clique para exibir a imagem em tamanho normal](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image12.png))


Clique em Concluir para concluir o assistente e retornar à superfície de design do conjunto de dados. O `EmployeesTableAdapter` agora deve incluir um novo conjunto de métodos para acessar os funcionários contratados em um mês especificado.


[![Os novos métodos aparecem na superfície de Design do conjunto de dados](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image14.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image13.png)

**Figura 5**: O novo métodos aparecem na superfície de Design do conjunto de dados ([clique para exibir a imagem em tamanho normal](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image15.png))


## <a name="step-2-adding-thegetemployeesbyhireddatemonthmonthmethod-to-the-business-logic-layer"></a>Etapa 2: Adicionando a`GetEmployeesByHiredDateMonth(month)`método para a camada de lógica de negócios

Como nosso aplicativo arquitetura usa um separado da camada para a lógica de negócios e dados de lógica de acesso, precisamos adicionar um método para nosso BLL que chamadas até a DAL para recuperar os funcionários contratadas antes de uma data especificada. Abra o `EmployeesBLL.cs` de arquivo e adicione o seguinte método:


[!code-csharp[Main](programmatically-setting-the-objectdatasource-s-parameter-values-cs/samples/sample1.cs)]

Assim como acontece com os outros métodos nessa classe, `GetEmployeesByHiredDateMonth(month)` simplesmente chama para baixo a DAL e retorna os resultados.

## <a name="step-3-displaying-employees-whose-hiring-anniversary-is-this-month"></a>Etapa 3: Exibir funcionários cuja data de contratação especial é deste mês

A etapa final para este exemplo é exibir os funcionários cuja data de contratação especial é deste mês. Comece adicionando um controle GridView para o `ProgrammaticParams.aspx` página o `BasicReporting` pasta e adicionar um novo ObjectDataSource como sua fonte de dados. Configurar o ObjectDataSource para usar o `EmployeesBLL` classe com o `SelectMethod` definido como `GetEmployeesByHiredDateMonth(month)`.


[![Use a classe EmployeesBLL](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image17.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image16.png)

**Figura 6**: Use o `EmployeesBLL` classe ([clique para exibir a imagem em tamanho normal](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image18.png))


[![Selecione de GetEmployeesByHiredDateMonth(month) o método](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image20.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image19.png)

**Figura 7**: Select From a `GetEmployeesByHiredDateMonth(month)` método ([clique para exibir a imagem em tamanho normal](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image21.png))


Tela final pergunta para fornecer o `month` fonte do valor do parâmetro. Desde que vamos definir esse valor por meio de programação, deixe a origem de parâmetro definido para o padrão nenhuma opção e clique em Concluir.


[![Deixe o conjunto de parâmetro de origem para nenhum](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image23.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image22.png)

**Figura 8**: deixar a fonte de parâmetro definido como None ([clique para exibir a imagem em tamanho normal](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image24.png))


Isso criará uma `Parameter` objeto o ObjectDataSource `SelectParameters` coleção que não tem um valor especificado.


[!code-aspx[Main](programmatically-setting-the-objectdatasource-s-parameter-values-cs/samples/sample2.aspx)]

Para definir esse valor por meio de programação, é preciso criar um manipulador de eventos para o ObjectDataSource `Selecting` eventos. Para fazer isso, vá para o modo de exibição de Design e clique duas vezes em ObjectDataSource. Como alternativa, selecione o ObjectDataSource, vá para a janela de propriedades e clique no ícone de raio. Em seguida, um clique duas vezes na caixa de texto ao lado de `Selecting` evento ou digite o nome do manipulador de eventos que você deseja usar.


![Clique no ícone de raio na janela Propriedades à lista de eventos do controle de Web](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image25.png)

**Figura 9**: clique no ícone de raio na janela Propriedades à lista de eventos do controle de Web


Ambas as abordagens de adicionar um novo manipulador de eventos para o ObjectDataSource `Selecting` evento para a classe de code-behind da página. Nesse manipulador de eventos pode ler e gravar os valores de parâmetro usando `e.InputParameters[parameterName]`, onde *`parameterName`* é o valor da `Name` atributo no `<asp:Parameter>` marca (o `InputParameters` coleção também pode ser indexada ordinalmente, como em `e.InputParameters[index]`). Para definir o `month` parâmetro para o mês atual, adicione o seguinte para o `Selecting` manipulador de eventos:


[!code-csharp[Main](programmatically-setting-the-objectdatasource-s-parameter-values-cs/samples/sample3.cs)]

Quando esta página por meio de um navegador de visita podemos ver que apenas um funcionário foi contratado deste mês (março) Laura Callahan, que está com a empresa desde 1994.


[![Os funcionários cujos aniversários deste mês são mostrados](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image27.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image26.png)

**Figura 10**: os funcionários cujo aniversários este mês são mostrados ([clique para exibir a imagem em tamanho normal](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image28.png))


## <a name="summary"></a>Resumo

Enquanto os valores dos parâmetros do ObjectDataSource geralmente podem ser definidos declarativamente, sem a necessidade de uma linha de código, é fácil definir os valores de parâmetro programaticamente. Tudo que precisamos fazer é criar um manipulador de eventos para o ObjectDataSource `Selecting` evento que dispara antes do método do objeto subjacente é chamado e definir manualmente os valores para um ou mais parâmetros por meio de `InputParameters` coleção.

Este tutorial conclui a seção de relatórios básicos. O [tutorial próxima](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) inicia a seção de filtragem e cenários de detalhes mestre, que vamos examinar técnicas para permitir que o visitante para filtrar os dados e fazer uma busca detalhada de um relatório mestre em um relatório de detalhes.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisor levar para este tutorial foi Giesenow Hilton. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](declarative-parameters-cs.md)
> [Próximo](displaying-data-with-the-objectdatasource-vb.md)
