---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
title: Introdução ao banco de dados do Entity Framework 4.0 First e 4 do ASP.NET Web Forms - parte 8 | Microsoft Docs
author: tdykstra
description: Aplicativo web de exemplo Contoso University demonstra como criar aplicativos Web Forms do ASP.NET usando o Entity Framework. O aplicativo de exemplo é...
ms.author: aspnetcontent
ms.date: 12/03/2010
ms.assetid: aaadd9bb-5508-448c-ad57-5497dff90e13
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
msc.type: authoredcontent
ms.openlocfilehash: 545438a48c57bed00530f3d4cc143542d8999581
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826086"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-8"></a>Introdução ao banco de dados do Entity Framework 4.0 First e 4 do Web Forms do ASP.NET – parte 8
====================
por [Tom Dykstra](https://github.com/tdykstra)

> Aplicativo web de exemplo Contoso University demonstra como criar aplicativos Web Forms do ASP.NET usando o Entity Framework 4.0 e o Visual Studio 2010. Para obter informações sobre a série de tutoriais, consulte [o primeiro tutorial na série](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="using-dynamic-data-functionality-to-format-and-validate-data"></a>Usando a funcionalidade de dados dinâmicos para formatar e validar dados

No tutorial anterior, você implementou procedimentos armazenados. Este tutorial mostrará como funcionalidade dinâmica de dados pode fornecer os seguintes benefícios:

- Campos automaticamente são formatados para exibição com base no tipo de dados.
- Campos são validados automaticamente com base no tipo de dados.
- Você pode adicionar metadados ao modelo de dados para personalizar o comportamento de formatação e validação. Quando você fizer isso, você pode adicionar as regras de formatação e validação em um só lugar e automaticamente são aplicados em qualquer lugar pode acessar os campos usando os controles de dados dinâmicos.

Para ver como isso funciona, você alterará os controles usados para exibir e editar campos existentes *Students.aspx* página e você adicionará formatação e validação de metadados para os campos nome e data do `Student` tipo de entidade.

[![Para Image01](the-entity-framework-and-aspnet-getting-started-part-8/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image1.png)

## <a name="using-dynamicfield-and-dynamiccontrol-controls"></a>Usando os controles de DynamicControl e DynamicField

Abra o *Students.aspx* página e, no `StudentsGridView` substituição do controle a **nome** e **data de registro** `TemplateField` elementos com a seguinte marcação:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample1.aspx)]

Essa marcação usa `DynamicControl` controla no lugar de `TextBox` e `Label` campo do modelo de nome de controles em que o aluno e, em seguida, usa um `DynamicField` controle para a data de registro. Sem cadeias de caracteres de formato são especificadas.

Adicionar um `ValidationSummary` controle após o `StudentsGridView` controle.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample2.aspx)]

No `SearchGridView` controle substitua a marcação para o **nome** e **data de registro** colunas como você foram no `StudentsGridView` controlar, exceto omitir o `EditItemTemplate` elemento. O `Columns` elemento o `SearchGridView` controle agora contém a marcação a seguir:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample3.aspx)]

Abra *Students.aspx.cs* e adicione o seguinte `using` instrução:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample4.cs)]

Adicionar um manipulador para a página `Init` eventos:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample5.cs)]

Esse código especifica que os dados dinâmicos fornecerá formatação e validação nesses controles ligados a dados para campos do `Student` entidade. Se você receber uma mensagem de erro semelhante ao seguinte exemplo, quando você executa a página, ele geralmente significa que você esqueceu de chamar o `EnableDynamicData` método no `Page_Init`:

`Could not determine a MetaTable. A MetaTable could not be determined for the data source 'StudentsEntityDataSource' and one could not be inferred from the request URL.`

Execute a página.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-8/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image3.png)

No **data de registro** coluna, a hora é exibida junto com a data como o tipo de propriedade é `DateTime`. Você corrigirá isso mais tarde.

Por enquanto, observe que os dados dinâmicos automaticamente fornece validação de dados básicos. Por exemplo, clique em **edite**, desmarque o campo de data, clique em **atualização**, e você verá que os dados dinâmicos automaticamente torna isso um campo obrigatório porque o valor não é anulável no modelo de dados. A página exibe um asterisco após o campo e uma mensagem de erro no `ValidationSummary` controle:

[![Image05](the-entity-framework-and-aspnet-getting-started-part-8/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image5.png)

Você pode omitir o `ValidationSummary` controle, porque você também pode manter o ponteiro do mouse sobre o asterisco para ver a mensagem de erro:

[![Image06](the-entity-framework-and-aspnet-getting-started-part-8/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image7.png)

Dados dinâmicos também validará que os dados digitados na **data de registro** campo for uma data válida:

[![Image04](the-entity-framework-and-aspnet-getting-started-part-8/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image9.png)

Como você pode ver, esta é uma mensagem de erro genérica. Na próxima seção, você verá como personalizar as mensagens, bem como a validação e regras de formatação.

## <a name="adding-metadata-to-the-data-model"></a>Adição de metadados para o modelo de dados

Normalmente, você deseja personalizar a funcionalidade fornecida pelos dados dinâmicos. Por exemplo, você pode alterar como os dados são exibidos e o conteúdo das mensagens de erro. Você normalmente também personalizar regras de validação de dados para fornecer mais funcionalidade do que o que fornece os dados dinâmicos automaticamente com base nos tipos de dados. Para fazer isso, você deve criar classes parciais que correspondem aos tipos de entidade.

No **Gerenciador de soluções**, com o botão direito do **ContosoUniversity** projeto, selecione **Add Reference**e adicione uma referência ao `System.ComponentModel.DataAnnotations`.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-8/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image11.png)

No *DAL* pasta, crie um novo arquivo de classe, nomeie- *Student.cs*e substitua o código do modelo com o código a seguir.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample6.cs)]

Esse código cria uma classe parcial para o `Student` entidade. O `MetadataType` atributo aplicado a essa classe parcial identifica a classe que você está usando para especificar os metadados. A classe de metadados pode ter qualquer nome, mas usando o nome da entidade de adição "Metadados" é uma prática comum.

Os atributos aplicados a propriedades na classe de metadados que especificam a formatação, validação, regras e mensagens de erro. Os atributos mostrados aqui terá os seguintes resultados:

- `EnrollmentDate` será exibido como uma data (sem uma hora).
- Ambos os campos de nome devem ser 25 caracteres ou menos em tamanho e uma mensagem de erro personalizada é fornecido.
- Ambos os campos de nome são obrigatórios, e uma mensagem de erro personalizada é fornecida.

Execute o *Students.aspx* página novamente e você verá que as datas são agora exibidas sem tempos de:

[![Image08](the-entity-framework-and-aspnet-getting-started-part-8/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image13.png)

Editar uma linha e tente limpar os valores nos campos de nome. Os asteriscos que indicam erros de campo aparecem assim que você deixar um campo, antes de clicar em **atualização**. Quando você clica em **atualização**, a página exibe o texto da mensagem de erro especificado.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-8/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image15.png)

Tente digitar nomes de mais de 25 caracteres, clique em **atualização**, e a página exibe o texto da mensagem de erro especificado.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-8/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image17.png)

Agora que você configurar essas regras de formatação e validação nos metadados do modelo de dados, as regras serão aplicadas automaticamente em cada página que exibe ou permite que as alterações nesses campos, desde que você use `DynamicControl` ou `DynamicField` controles. Isso reduz a quantidade de código redundante é preciso escrever, que torna a programação e a realização dos testes, e garante que a validação e formatação de dados são consistentes em todo um aplicativo.

## <a name="more-information"></a>Mais informações

Isso conclui esta série de tutoriais sobre como começar com o Entity Framework. Para obter mais recursos para ajudá-lo a aprender a usar o Entity Framework, continue com [o primeiro tutorial na série de tutoriais do Entity Framework próximo](../continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md) ou visite os seguintes sites:

- [Perguntas Frequentes do Entity Framework](http://www.ef-faq.org/introduction.html)
- [Blog da equipe do Entity Framework](https://blogs.msdn.com/b/adonet/)
- [Entity Framework na biblioteca MSDN](https://msdn.microsoft.com/library/bb399572.aspx)
- [Entity Framework no MSDN Data Developer Center](https://msdn.microsoft.com/data/ef.aspx)
- [Visão de geral do controle de servidor de Web do EntityDataSource na biblioteca MSDN](https://msdn.microsoft.com/library/cc488502.aspx)
- [Controle EntityDataSource referência da API na biblioteca MSDN](https://msdn.microsoft.com/library/system.web.ui.webcontrols.entitydatasource.aspx)
- [Fóruns do Entity Framework no MSDN](https://social.msdn.microsoft.com/forums/adodotnetentityframework/)
- [Blog de Julie](http://thedatafarm.com/blog/)

> [!div class="step-by-step"]
> [Anterior](the-entity-framework-and-aspnet-getting-started-part-7.md)
