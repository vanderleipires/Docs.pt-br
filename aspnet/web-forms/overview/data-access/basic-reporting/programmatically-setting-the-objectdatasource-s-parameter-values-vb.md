---
uid: web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb
title: Configurando programaticamente os valores do parâmetro ObjectDataSource (VB) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, examinaremos adicionando um método a nossa DAL e BLL que aceita um único parâmetro de entrada e retorna dados. O exemplo será definir esse parâmetro...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 0ecb03b6-52a0-4731-8c7a-436391d36838
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb
msc.type: authoredcontent
ms.openlocfilehash: f823d1db7f98dcbbef12d20df4a28e39fae0ac26
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823477"
---
<a name="programmatically-setting-the-objectdatasources-parameter-values-vb"></a>Configurando programaticamente os valores do parâmetro ObjectDataSource (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_6_VB.exe) ou [baixar PDF](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/datatutorial06vb1.pdf)

> Neste tutorial, examinaremos adicionando um método a nossa DAL e BLL que aceita um único parâmetro de entrada e retorna dados. O exemplo será definir esse parâmetro por meio de programação.


## <a name="introduction"></a>Introdução

Como vimos na [tutorial anterior](declarative-parameters-vb.md), várias opções estão disponíveis para declarativamente passar valores de parâmetro para métodos do ObjectDataSource. Se o valor do parâmetro é embutido no código, vem de um controle da Web na página ou está em qualquer outra fonte que pode ser lido por uma fonte de dados `Parameter` do objeto, por exemplo, se o valor pode ser associado ao parâmetro de entrada sem escrever uma linha de código.

Pode haver ocasiões, no entanto, quando o valor do parâmetro trata de alguma origem não já contabilizada por uma fonte de dados internos `Parameter` objetos. Se nosso site de suporte para contas de usuário, talvez seja conveniente definir o parâmetro com base em conectada no momento na ID de usuário. do visitante Ou, talvez seja necessário personalizar o valor do parâmetro antes de enviá-lo ao longo de método do objeto subjacente do ObjectDataSource.

Sempre que o ObjectDataSource `Select` método é invocado pela primeira vez dispara o ObjectDataSource seus [evento Selecting](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting%28VS.80%29.aspx). Método do objeto subjacente do ObjectDataSource, em seguida, é invocado. Depois de concluído o ObjectDataSource [eventos selecionados](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selected%28VS.80%29.aspx) é acionado (Figura 1 ilustra essa sequência de eventos). Os valores de parâmetro passados para o método do objeto subjacente do ObjectDataSource podem ser definidos ou personalizados em um manipulador de eventos para o `Selecting` eventos.


[![O ObjectDataSource Selected e selecionando incêndio de eventos antes e do depois de seu objeto subjacente método é invocado](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image2.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image1.png)

**Figura 1**: O ObjectDataSource `Selected` e `Selecting` acionar eventos antes e do depois de seu objeto subjacente método é invocado ([clique para exibir a imagem em tamanho normal](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image3.png))


Neste tutorial, examinaremos adicionando um método a nossa DAL e BLL que aceita um único parâmetro de entrada `Month`, do tipo `Integer` e retorna um `EmployeesDataTable` objeto preenchido com os funcionários que têm seu aniversário contratação especificado `Month`. Nosso exemplo definirá esse parâmetro por meio de programação com base no mês atual, mostrando uma lista de "Funcionários aniversários deste mês".

Vamos começar!

## <a name="step-1-adding-a-method-toemployeestableadapter"></a>Etapa 1: Adicionando um método para`EmployeesTableAdapter`

Para nosso primeiro exemplo, precisamos adicionar um meio para recuperar os funcionários cujo `HireDate` ocorreu em um mês especificado. Para fornecer essa funcionalidade de acordo com nossa arquitetura é necessário primeiro criar um método em `EmployeesTableAdapter` que é mapeado para a instrução SQL adequada. Para fazer isso, comece abrindo o conjunto de dados tipados do Northwind. Clique com botão direito no `EmployeesTableAdapter` de rótulo e escolha Add Query.


[![Adicionar uma nova consulta para o EmployeesTableAdapter](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image5.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image4.png)

**Figura 2**: adicionar uma nova consulta para o `EmployeesTableAdapter` ([clique para exibir a imagem em tamanho normal](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image6.png))


Escolha esta opção Adicionar uma instrução SQL que retorna linhas. Quando você chegar a especificar uma `SELECT` o padrão de tela da instrução `SELECT` instrução para o `EmployeesTableAdapter` já será carregado. Basta adicionar na `WHERE` cláusula: `WHERE DATEPART(m, HireDate) = @Month`. [DATEPART](https://msdn.microsoft.com/library/ms174420.aspx) é uma função T-SQL que retorna uma parte de data específica de um `datetime` tipo; nesse caso, estamos usando `DATEPART` para retornar o mês do `HireDate` coluna.


[![Retorno apenas aquelas linhas onde a coluna HireDate é menor ou igual ao @HiredBeforeDate parâmetro](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image8.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image7.png)

**Figura 3**: retornar apenas as linhas em que o `HireDate` coluna for menor ou igual ao `@HiredBeforeDate` parâmetro ([clique para exibir a imagem em tamanho normal](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image9.png))


Por fim, altere o `FillBy` e `GetDataBy` nomes de método para `FillByHiredDateMonth` e `GetEmployeesByHiredDateMonth`, respectivamente.


[![Escolha nomes mais apropriados do método que FillBy e GetDataBy](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image11.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image10.png)

**Figura 4**: escolha mais apropriada método nomes que `FillBy` e `GetDataBy` ([clique para exibir a imagem em tamanho normal](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image12.png))


Clique em Concluir para concluir o assistente e retornar à superfície de design do conjunto de dados. O `EmployeesTableAdapter` agora deve incluir um novo conjunto de métodos para acessar o employees contratados em um mês especificado.


[![Os novos métodos aparecem na superfície de Design do conjunto de dados](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image14.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image13.png)

**Figura 5**: os novos métodos aparecem na superfície de Design do conjunto de dados ([clique para exibir a imagem em tamanho normal](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image15.png))


## <a name="step-2-adding-thegetemployeesbyhireddatemonthmonthmethod-to-the-business-logic-layer"></a>Etapa 2: Adicionando o`GetEmployeesByHiredDateMonth(month)`método para a camada de lógica de negócios

Como nossos usos de arquitetura do aplicativo um separado de camada para a lógica de negócios e os dados de acesso lógico, precisamos adicionar um método à nossa BLL que chamadas para baixo até a DAL para recuperar os funcionários contratadas antes de uma data especificada. Abra o `EmployeesBLL.vb` arquivo e adicione o seguinte método:


[!code-vb[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample1.vb)]

Assim como acontece com nossos outros métodos nessa classe, `GetEmployeesByHiredDateMonth(month)` simplesmente chama para baixo da DAL e retorna os resultados.

## <a name="step-3-displaying-employees-whose-hiring-anniversary-is-this-month"></a>Etapa 3: Exibir funcionários cuja data de contratação especial é este mês

A etapa final para este exemplo é exibir esses funcionários cuja data de contratação especial é este mês. Comece adicionando um GridView para o `ProgrammaticParams.aspx` página o `BasicReporting` pasta e adicione um novo ObjectDataSource como sua fonte de dados. Configurar o ObjectDataSource para usar o `EmployeesBLL` classe com o `SelectMethod` definido como `GetEmployeesByHiredDateMonth(month)`.


[![Use a classe EmployeesBLL](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image17.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image16.png)

**Figura 6**: Use o `EmployeesBLL` classe ([clique para exibir a imagem em tamanho normal](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image18.png))


[![Selecione de GetEmployeesByHiredDateMonth(month) o método](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image20.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image19.png)

**Figura 7**: Select From a `GetEmployeesByHiredDateMonth(month)` método ([clique para exibir a imagem em tamanho normal](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image21.png))


A tela final pede que possamos fornecer os `month` origem do valor do parâmetro. Já que vamos definir esse valor por meio de programação, deixe a fonte de parâmetro definidos para o padrão nenhuma opção e clique em Concluir.


[![Deixe o código-fonte do parâmetro definido como None](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image23.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image22.png)

**Figura 8**: deixar a fonte de parâmetro definido como None ([clique para exibir a imagem em tamanho normal](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image24.png))


Isso criará um `Parameter` objeto do ObjectDataSource `SelectParameters` coleção que não tem um valor especificado.


[!code-aspx[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample2.aspx)]

Para definir esse valor por meio de programação, precisamos criar um manipulador de eventos para o ObjectDataSource `Selecting` eventos. Para fazer isso, vá para a exibição de Design e clique duas vezes o ObjectDataSource. Como alternativa, selecione o ObjectDataSource, vá para a janela de propriedades e clique no ícone de raio. Em seguida, clique duas vezes na caixa de texto ao lado de `Selecting` evento ou digite o nome do manipulador de eventos que você deseja usar. Como uma terceira opção, você pode criar o manipulador de eventos, selecionando o ObjectDataSource e seu `Selecting` eventos nas duas listas de lista suspensa na parte superior da classe de code-behind da página.


![Clique no ícone de raio na janela Propriedades para listar os eventos de controle da Web](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image25.png)

**Figura 9**: clique no ícone de raio na janela Propriedades para listar os eventos de controle da Web


Todas as três abordagens adicionar um novo manipulador de eventos para o ObjectDataSource `Selecting` evento para a classe de code-behind da página. Nesse manipulador de eventos podem ler e gravar aos valores de parâmetro usando `e.InputParameters(parameterName)`, onde *`parameterName`* é o valor da `Name` atributo no `<asp:Parameter>` marca (o `InputParameters` coleção também pode ser indexada ordinalmente, como em `e.InputParameters(index)`). Para definir a `month` parâmetro para o mês atual, adicione o seguinte para o `Selecting` manipulador de eventos:


[!code-vb[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample3.vb)]

Ao visitar esta página por meio de um navegador, vemos que apenas um funcionário foi contratado neste mês (março) Laura Callahan, que tem sido a empresa desde 1994.


[![Esses funcionários cujas datas especiais deste mês são mostrados](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image27.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image26.png)

**Figura 10**: os funcionários cujo aniversários deste mês são mostrados ([clique para exibir a imagem em tamanho normal](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image28.png))


## <a name="summary"></a>Resumo

Enquanto os valores dos parâmetros do ObjectDataSource podem normalmente ser definidos de forma declarativa, sem a necessidade de uma linha de código, é fácil definir os valores de parâmetro programaticamente. Tudo o que precisamos fazer é criar um manipulador de eventos para o ObjectDataSource `Selecting` evento, que é acionado antes que o método do objeto subjacente é invocado e definir manualmente os valores para um ou mais parâmetros via o `InputParameters` coleção.

Esse tutorial conclui a seção de relatórios básicos. O [próximo tutorial](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) inicia a seção de filtragem e cenários de detalhes mestre, em que vamos examinar técnicas para permitir que o visitante para filtrar os dados e fazer drill down de um relatório mestre em um relatório de detalhes.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Revisor de avanço para este tutorial foi Hilton Giesenow. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](declarative-parameters-vb.md)
