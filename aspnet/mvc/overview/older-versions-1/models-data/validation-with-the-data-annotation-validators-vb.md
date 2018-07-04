---
uid: mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
title: Validação com os validadores de anotação de dados (VB) | Microsoft Docs
author: microsoft
description: Aproveite o associador de modelo de anotação de dados para executar a validação dentro de um aplicativo ASP.NET MVC. Saiba como usar os diferentes tipos de validador...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/29/2009
ms.topic: article
ms.assetid: 0d23ff2b-f2ec-434a-be3b-1180beeccba3
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
msc.type: authoredcontent
ms.openlocfilehash: 94410d0e8ad132a4e7a31ac01163663b597c3225
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391406"
---
<a name="validation-with-the-data-annotation-validators-vb"></a>Validação com os validadores de anotação de dados (VB)
====================
por [Microsoft](https://github.com/microsoft)

> Aproveite o associador de modelo de anotação de dados para executar a validação dentro de um aplicativo ASP.NET MVC. Saiba como usar os diferentes tipos de atributos de validador e trabalhar com eles no Microsoft Entity Framework.


Neste tutorial, você aprenderá como usar os validadores de anotação de dados para executar a validação em um aplicativo ASP.NET MVC. A vantagem de usar os validadores de anotação de dados é que eles permitem que você executar a validação simplesmente adicionando um ou mais atributos – como exigida ou o atributo StringLength – para uma propriedade de classe.

Antes de usar os validadores de anotação de dados, você deve baixar o associador de modelo de anotações de dados. Você pode baixar a amostra de associador de modelo de anotações de dados do site do CodePlex clicando [aqui](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).


É importante entender que o associador de modelo de anotações de dados não é uma parte oficial da Microsoft ASP.NET MVC framework. Embora o associador de modelo de anotações de dados foi criado pela equipe do Microsoft ASP.NET MVC, a Microsoft não oferece suporte oficial do produto para o associador de modelo de anotações de dados descrito e usado neste tutorial.


## <a name="using-the-data-annotation-model-binder"></a>Usando o associador de modelo de anotação de dados

Para usar o associador de modelo de anotações de dados em um aplicativo ASP.NET MVC, primeiro você precisa adicionar uma referência ao assembly Microsoft.Web.Mvc.DataAnnotations.dll e o assembly de unirem às. Selecione a opção de menu **projeto, adicionar referência**. Em seguida clique o **navegue** guia e navegue até o local onde você baixou (e descompactado) a amostra de associador de modelo de anotações de dados (consulte **Figura 1**).

[![](validation-with-the-data-annotation-validators-vb/_static/image2.png)](validation-with-the-data-annotation-validators-vb/_static/image1.png)

**Figura 1**: adicionar uma referência ao associador de modelo de anotações de dados ([clique para exibir a imagem em tamanho normal](validation-with-the-data-annotation-validators-vb/_static/image3.png))

Selecione o assembly Microsoft.Web.Mvc.DataAnnotations.dll e o assembly de unirem às e clique o **Okey** botão.


Você não pode usar o assembly de unirem às incluído com o .NET Framework Service Pack 1 com o associador de modelo de anotações de dados. Você deve usar a versão do assembly unirem às incluído no download de amostra de associador de modelo de anotações de dados.


Por fim, você precisa registrar o associador de modelo de DataAnnotations no arquivo global. asax. Adicione a seguinte linha de código para o aplicativo\_manipulador de eventos Start () para que o aplicativo\_método Start () tem esta aparência:

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample1.vb)]

Esta linha de código registra o DataAnnotationsModelBinder como o associador de modelo padrão para todo o aplicativo ASP.NET MVC.

## <a name="using-the-data-annotation-validator-attributes"></a>Usando os atributos de validador de anotação de dados

Quando você usa o associador de modelo de anotações de dados, você use atributos de validador para executar a validação. O namespace DataAnnotations inclui os seguintes atributos de validador:

- Intervalo de – permite que você valide se o valor de uma propriedade fica entre um intervalo de valores especificado.
- ReqularExpression – permite que você valide se o valor de uma propriedade corresponde a um padrão de expressão regular especificada.
- Obrigatório – permite marcar uma propriedade conforme necessário.
- StringLength – permite que você especificar um comprimento máximo para uma propriedade de cadeia de caracteres.
- Validação – a classe base para todos os atributos de validador.

> [!NOTE] 
> 
> Se suas necessidades de validação não são atendidas por qualquer um dos validadores padrão, em seguida, você sempre terá a opção de criação de um atributo de validador personalizado herdando de um novo atributo de validador de atributo de validação de base.


A classe de produto na **listagem 1** ilustra como usar esses atributos de validador. As propriedades de nome, descrição e UnitPrice são marcadas conforme necessário. A propriedade de nome deve ter um comprimento de cadeia de caracteres que é menor que 10 caracteres. Por fim, a propriedade UnitPrice deve corresponder a um padrão de expressão regular que representa um valor de moeda.

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample2.vb)]

**Listagem 1**: Models\Product.vb

A classe Product ilustra como usar um atributo adicional: o atributo DisplayName. O atributo DisplayName permite que você modifique o nome da propriedade quando a propriedade é exibida em uma mensagem de erro. Em vez de exibir a mensagem de erro "o campo UnitPrice é necessário" é possível exibir a mensagem de erro "o campo de preço é necessário".

> [!NOTE] 
> 
> Se você quiser personalizar completamente a mensagem de erro exibida por um validador pode atribuir uma mensagem de erro personalizado à propriedade de ErrorMessage do validador como este: `<Required(ErrorMessage:="This field needs a value!")>`


Você pode usar a classe de produto na **listagem 1** com a ação de controlador de Create () em **listagem 2**. Esta ação do controlador exibe novamente a exibição criar quando o estado do modelo contém erros.

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample3.vb)]

**Listagem 2**: Controllers\ProductController.vb

Por fim, você pode criar o modo de exibição **listagem 3** clicando duas vezes a ação Create () e selecionando a opção de menu **adicionar exibição**. Crie uma exibição fortemente tipada com a classe Product como a classe de modelo. Selecione **Create** na lista suspensa de conteúdo view (consulte **Figura 2**).

[![](validation-with-the-data-annotation-validators-vb/_static/image5.png)](validation-with-the-data-annotation-validators-vb/_static/image4.png)

**Figura 2**: adição de modo de exibição criar

[!code-aspx[Main](validation-with-the-data-annotation-validators-vb/samples/sample4.aspx)]

**Listagem 3**: Views\Product\Create.aspx

> [!NOTE] 
> 
> Remover o campo de Id do formulário Criar gerado pelo **adicionar exibição** opção de menu. Como o campo de Id corresponde a uma coluna de identidade, você não deseja permitir que os usuários a inserir um valor para esse campo.


Se você envia o formulário para a criação de um produto e você não inserir valores para os campos obrigatórios, então as mensagens de erro de validação no **Figura 3** são exibidos.

[![](validation-with-the-data-annotation-validators-vb/_static/image7.png)](validation-with-the-data-annotation-validators-vb/_static/image6.png)

**Figura 3**: campos obrigatórios ausentes

Se você inserir um valor inválido para moeda, em seguida, a mensagem de erro no **Figura 4** é exibida.

[![](validation-with-the-data-annotation-validators-vb/_static/image9.png)](validation-with-the-data-annotation-validators-vb/_static/image8.png)

**Figura 4**: valor inválido de moeda

## <a name="using-data-annotation-validators-with-the-entity-framework"></a>Usando os validadores de anotação com o Entity Framework

Se você estiver usando o Microsoft Entity Framework para gerar classes de modelo de dados, em seguida, é possível aplicar os atributos de validador diretamente às suas classes. Como o Entity Framework Designer gera as classes de modelo, as alterações feitas para as classes de modelo serão substituídas na próxima vez que você fizer alterações no Designer.

Se você quiser usar os validadores com as classes geradas pelo Entity Framework, em seguida, você precisa criar classes de dados de metadados. Você pode aplicar os validadores para a classe de dados de metadados em vez de aplicar os validadores à classe real.

Por exemplo, imagine que você tenha criado uma classe de filme usando o Entity Framework (consulte **Figura 5**). Além disso, imagine que você deseja tornar o título do filme e diretor de propriedades propriedades necessárias. Nesse caso, você pode criar a classe parcial e a classe de dados de meta no **listagem 4**.

[![](validation-with-the-data-annotation-validators-vb/_static/image11.png)](validation-with-the-data-annotation-validators-vb/_static/image10.png)

**Figura 5**: classe Movie gerada pelo Entity Framework

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample5.vb)]

**Listagem 4**: Models\Movie.vb

O arquivo no **listagem 4** contém duas classes chamadas de filmes e MovieMetaData. A classe de filme é uma classe parcial. Ele corresponde à classe parcial gerada pelo Entity Framework que está contido no arquivo DataModel.Designer.vb.

Atualmente, o .NET framework não dá suporte a propriedades parciais. Portanto, não há nenhuma maneira de aplicar os atributos de validador para as propriedades da classe Movie definido no arquivo DataModel.Designer.vb aplicando os atributos de validador para as propriedades da classe Movie definida no arquivo no **listagem 4**.

Observe que a classe parcial de filme é decorada com um atributo MetadataType que aponta para a classe MovieMetaData. A classe MovieMetaData contém propriedades de proxy para as propriedades da classe filme.

Os atributos de validação são aplicados às propriedades da classe MovieMetaData. As propriedades do título, diretor e DateReleased todos estiverem marcadas como propriedades necessárias. A propriedade diretor deve ser atribuída a uma cadeia de caracteres que contém menos de 5 caracteres. Por fim, o atributo DisplayName é aplicado à propriedade DateReleased para exibir uma mensagem de erro como "o campo de data de lançamento é obrigatório." em vez do erro "o campo DateReleased é necessário."

> [!NOTE] 
> 
> Observe que as propriedades de proxy na classe MovieMetaData não precisam representar os mesmos tipos de propriedades correspondentes na classe de filme. Por exemplo, a propriedade de diretor é uma propriedade de cadeia de caracteres na classe de filme e uma propriedade de objeto na classe MovieMetaData.


A página no **Figura 6** ilustra as mensagens de erro retornadas quando você insere valores inválidos para as propriedades do filme.

[![](validation-with-the-data-annotation-validators-vb/_static/image13.png)](validation-with-the-data-annotation-validators-vb/_static/image12.png)

**Figura 6**: usando validadores com o Entity Framework ([clique para exibir a imagem em tamanho normal](validation-with-the-data-annotation-validators-vb/_static/image14.png))

## <a name="summary"></a>Resumo

Neste tutorial, você aprendeu como aproveitar o associador de modelo de anotação de dados para executar a validação dentro de um aplicativo ASP.NET MVC. Você aprendeu a usar os diferentes tipos de atributos de validador, como o necessário e StringLength. Você também aprendeu a usar esses atributos ao trabalhar com o Entity Framework da Microsoft.

> [!div class="step-by-step"]
> [Anterior](validating-with-a-service-layer-vb.md)
