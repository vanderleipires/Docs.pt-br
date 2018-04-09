---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
title: Introdução ao banco de dados do Entity Framework 4.0 primeiro e o ASP.NET 4 Web Forms - parte 8 | Microsoft Docs
author: tdykstra
description: O aplicativo web de exemplo Contoso University demonstra como criar aplicativos Web Forms do ASP.NET usando o Entity Framework. O aplicativo de exemplo é...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: aaadd9bb-5508-448c-ad57-5497dff90e13
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
msc.type: authoredcontent
ms.openlocfilehash: 035cce022d1b3697b825a96487529dbc9675d90e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-8"></a>Introdução ao banco de dados do Entity Framework 4.0 primeiro e 4 Web Forms do ASP.NET - parte 8
====================
por [Tom Dykstra](https://github.com/tdykstra)

> O aplicativo web de exemplo Contoso University demonstra como criar aplicativos Web Forms do ASP.NET usando o Entity Framework 4.0 e o Visual Studio 2010. Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial da série](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="using-dynamic-data-functionality-to-format-and-validate-data"></a>Usando a funcionalidade de dados dinâmicos para formatar e validar dados

No tutorial anterior, você implementou os procedimentos armazenados. Este tutorial mostra como a funcionalidade de dados dinâmicos pode fornecer os seguintes benefícios:

- Campos são formatados automaticamente de acordo com o tipo de dados de exibição.
- Campos são validados automaticamente com base em seu tipo de dados.
- Você pode adicionar metadados para o modelo de dados para personalizar o comportamento de formatação e validação. Quando você fizer isso, você pode adicionar as regras de formatação e validação em um só lugar e automaticamente são aplicados em todos os lugares pode acessar os campos usando controles de dados dinâmicos.

Para ver como isso funciona, você alterará os controles usados para exibir e editar os campos existentes no *Students.aspx* página e você adicionará formatação e validação de metadados para os campos nome e a data do `Student` tipo de entidade.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-8/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image1.png)

## <a name="using-dynamicfield-and-dynamiccontrol-controls"></a>Usando os controles de DynamicControl e DynamicField

Abra o *Students.aspx* página e no `StudentsGridView` controle substituir o **nome** e **data de inscrição** `TemplateField` elementos com a seguinte marcação:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample1.aspx)]

Essa marcação usa `DynamicControl` controla no lugar de `TextBox` e `Label` controles na student nome de campo do modelo e, em seguida, usa um `DynamicField` controle para a data de inscrição. Nenhuma cadeia de caracteres de formato é especificadas.

Adicionar um `ValidationSummary` controle após o `StudentsGridView` controle.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample2.aspx)]

No `SearchGridView` controle substitua a marcação para o **nome** e **data de inscrição** colunas como você foram no `StudentsGridView` controlar, exceto omitir o `EditItemTemplate` elemento. O `Columns` elemento o `SearchGridView` controle agora contém a seguinte marcação:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample3.aspx)]

Abra *Students.aspx.cs* e adicione o seguinte `using` instrução:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample4.cs)]

Adicionar um manipulador para a página `Init` evento:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample5.cs)]

Esse código especifica que dados dinâmicos fornecerá formatação e validação nesses controles de associação de dados para campos do `Student` entidade. Se você receber uma mensagem de erro semelhante ao seguinte exemplo, quando você executar a página, isso normalmente significa você esqueceu de chamar o `EnableDynamicData` método `Page_Init`:

`Could not determine a MetaTable. A MetaTable could not be determined for the data source 'StudentsEntityDataSource' and one could not be inferred from the request URL.`

Execute a página.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-8/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image3.png)

No **data de inscrição** coluna, a hora é exibida junto com a data, porque o tipo de propriedade é `DateTime`. Isso será corrigido posteriormente.

Agora, observe que os dados dinâmicos automaticamente fornece validação de dados básico. Por exemplo, clique em **editar**, desmarque o campo de data, clique em **atualização**, e você verá que os dados dinâmicos automaticamente torna isso um campo obrigatório porque o valor não é anulável no modelo de dados. A página exibe um asterisco após o campo e uma mensagem de erro no `ValidationSummary` controle:

[![Image05](the-entity-framework-and-aspnet-getting-started-part-8/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image5.png)

Você pode omitir o `ValidationSummary` controle, porque você também pode manter o ponteiro do mouse sobre o asterisco para ver a mensagem de erro:

[![Image06](the-entity-framework-and-aspnet-getting-started-part-8/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image7.png)

Dados dinâmicos também validará que os dados digitados no **data de inscrição** campo for uma data válida:

[![Image04](the-entity-framework-and-aspnet-getting-started-part-8/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image9.png)

Como você pode ver, essa é uma mensagem de erro genérica. Na próxima seção, você verá como personalizar mensagens, bem como a validação e regras de formatação.

## <a name="adding-metadata-to-the-data-model"></a>Adicionar metadados para o modelo de dados

Normalmente, você deseja personalizar a funcionalidade fornecida pelo dados dinâmicos. Por exemplo, você pode alterar como os dados são exibidos e o conteúdo das mensagens de erro. Você normalmente também personalizar regras de validação de dados para fornecer mais funcionalidade do que o que fornece dados dinâmicos automaticamente com base nos tipos de dados. Para fazer isso, você deve criar classes parciais que correspondem aos tipos de entidade.

Em **Solution Explorer**, com o botão direito do **ContosoUniversity** projeto, selecione **adicionar referência**e adicione uma referência a `System.ComponentModel.DataAnnotations`.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-8/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image11.png)

No *DAL* pasta, crie um novo arquivo de classe, nomeie-o *Student.cs*e substitua o código do modelo com o código a seguir.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample6.cs)]

Esse código cria uma classe parcial para a `Student` entidade. O `MetadataType` atributo aplicado a essa classe parcial identifica a classe que você está usando para especificar os metadados. A classe de metadados pode ter qualquer nome, mas usar o nome da entidade mais "Metadados" é uma prática comum.

Os atributos aplicados às propriedades de classe de metadados que especificam a formatação, validação, regras e mensagens de erro. Os atributos mostrados aqui terá os seguintes resultados:

- `EnrollmentDate` será exibido como uma data (sem uma hora).
- Os campos de nome devem ser 25 caracteres ou menos em tamanho e uma mensagem de erro personalizada é fornecido.
- Ambos os campos de nome são necessários, e uma mensagem de erro personalizada é fornecida.

Execute o *Students.aspx* página novamente, e você vê que as datas são exibidas agora sem vezes:

[![Image08](the-entity-framework-and-aspnet-getting-started-part-8/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image13.png)

Editar uma linha e tentar limpar os valores nos campos de nome. Os asteriscos que indicam erros de campo aparecem assim que você deixar um campo, antes de clicar em **atualização**. Quando você clica em **atualização**, a página exibe o texto da mensagem de erro especificado.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-8/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image15.png)

Tente digitar os nomes de mais de 25 caracteres, clique em **atualização**, e a página exibe o texto da mensagem de erro especificado.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-8/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image17.png)

Agora que você configurar essas regras de formatação e validação de metadados do modelo de dados, as regras serão aplicadas automaticamente em cada página que exibe ou permite que as alterações nesses campos, desde que você use `DynamicControl` ou `DynamicField` controles. Isso reduz a quantidade de código redundante você precisa escrever, que torna a programação e testes, e garante que a validação e formatação de dados são consistentes ao longo de um aplicativo.

## <a name="more-information"></a>Mais Informações

Isso conclui esta série de tutoriais de Introdução com o Entity Framework. Para obter mais recursos para ajudá-lo a aprender a usar o Entity Framework, continue com [primeiro tutorial na próxima série de tutoriais do Entity Framework](../continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md) ou visite os seguintes sites:

- [Perguntas Frequentes do Entity Framework](http://www.ef-faq.org/introduction.html)
- [Blog da equipe do Entity Framework](https://blogs.msdn.com/b/adonet/)
- [Entity Framework na biblioteca MSDN](https://msdn.microsoft.com/library/bb399572.aspx)
- [Entity Framework do MSDN Data Developer Center](https://msdn.microsoft.com/data/ef.aspx)
- [Visão geral de controle de servidor Web EntityDataSource na biblioteca MSDN](https://msdn.microsoft.com/library/cc488502.aspx)
- [Controle de EntityDataSource referência da API na biblioteca MSDN](https://msdn.microsoft.com/library/system.web.ui.webcontrols.entitydatasource.aspx)
- [Fóruns do Entity Framework no MSDN](https://social.msdn.microsoft.com/forums/adodotnetentityframework/)
- [Blog de Julie Lerman](http://thedatafarm.com/blog/)

> [!div class="step-by-step"]
> [Anterior](the-entity-framework-and-aspnet-getting-started-part-7.md)
