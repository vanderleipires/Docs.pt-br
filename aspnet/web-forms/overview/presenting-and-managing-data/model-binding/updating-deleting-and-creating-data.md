---
uid: web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
title: "Atualizar, excluir e criar dados com o modelo de associação e formulários da web | Microsoft Docs"
author: tfitzmac
description: "Esta série de tutoriais demonstra aspectos básicos do uso de associação de modelo com um projeto Web Forms do ASP.NET. Associação de modelo torna a interação de dados mais estreita-..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 602baa94-5a4f-46eb-a717-7a9e539c1db4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
msc.type: authoredcontent
ms.openlocfilehash: 18c065b44524e7738c048b5908fa50c592188064
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="updating-deleting-and-creating-data-with-model-binding-and-web-forms"></a>Atualizar, excluir e criar dados com o modelo de associação e formulários da web
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Esta série de tutoriais demonstra aspectos básicos do uso de associação de modelo com um projeto Web Forms do ASP.NET. Associação de modelo facilita a interação de dados mais direta de lidar com dados de objetos de origem (como ObjectDataSource ou SqlDataSource). Esta série começa com material introdutório e move para conceitos mais avançados em tutoriais subsequentes.
> 
> Este tutorial mostra como criar, atualizar e excluir dados com associação de modelo. Você definirá as propriedades a seguir:
> 
> - DeleteMethod
> - InsertMethod
> - UpdateMethod
> 
> Essas propriedades recebem o nome do método que trata a operação correspondente. Dentro desse método, você pode fornecer a lógica para interagir com os dados.
> 
> Este tutorial baseia-se no projeto criado no primeiro [parte](retrieving-data.md) da série.
> 
> Você pode [baixar](https://go.microsoft.com/fwlink/?LinkId=286116) o projeto completo em c# ou VB. O código para download funciona com o Visual Studio 2012 ou Visual Studio 2013. Ele usa o modelo do Visual Studio 2012, que é ligeiramente diferente do que o modelo do Visual Studio 2013 mostrado neste tutorial.


## <a name="what-youll-build"></a>Será possível compilar

Neste tutorial, você vai:

1. Adicionar modelos de dados dinâmicos
2. Habilitar atualização e exclusão de dados por meio de métodos de associação de modelo
3. Aplicar regras de validação de dados - permitem a criação de um novo registro no banco de dados

## <a name="add-dynamic-data-templates"></a>Adicionar modelos de dados dinâmicos

Para fornecer a melhor experiência de usuário e minimizar a repetição de código, você usará os modelos de dados dinâmicos. Você pode facilmente integrar modelos predefinidos dados dinâmicos em seu site existente ao instalar um pacote do NuGet.

Do **gerenciar pacotes NuGet**, instale o **DynamicDataTemplatesCS**.

![modelos de dados dinâmicos](updating-deleting-and-creating-data/_static/image1.png)

Observe que o projeto agora inclui uma pasta denominada **DynamicData**. Nessa pasta, você encontrará os modelos que são aplicados automaticamente aos controles dinâmicos em seus formulários da web.

![pasta de dados dinâmicos](updating-deleting-and-creating-data/_static/image2.png)

## <a name="enable-updating-and-deleting"></a>Ativar a atualização e exclusão

Habilitar os usuários a atualizar e excluir registros no banco de dados é muito semelhante ao processo de recuperação de dados. No **UpdateMethod** e **DeleteMethod** propriedades, que você especificar os nomes dos métodos que executam essas operações. Com um controle GridView, você também pode especificar a geração automática de editar e excluir botões. O seguinte código mostra as adições para o código de GridView.

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample1.aspx?highlight=4-5)]

O arquivo code-behind, adicione um usando a instrução para **System.Data.Entity.Infrastructure**.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample2.cs)]

Em seguida, adicione a seguinte atualização e exclusão métodos.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample3.cs)]

O **TryUpdateModel** método aplica-se os valores correspondentes de associação de dados do formulário da web para o item de dados. O item de dados é recuperado com base no valor do parâmetro id.

## <a name="enforce-validation-requirements"></a>Impor requisitos de validação

Os atributos de validação que são aplicados às propriedades na classe aluno FirstName, LastName e ano são aplicados automaticamente ao atualizar os dados. Os controles DynamicField adicionar validadores de cliente e servidor com base nos atributos de validação. As propriedades de nome e sobrenome são ambos necessários. Nome não pode exceder 20 caracteres de comprimento e LastName não pode exceder 40 caracteres. Ano deve ser um valor válido para a enumeração AcademicYear.

Se o usuário violar um dos requisitos de validação, a atualização não continuará. Para ver a mensagem de erro, adicione um controle ValidationSummary acima GridView. Para exibir os erros de validação da associação de modelo, defina o **ShowModelStateErrors** propriedade definida como **true**. 

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample4.aspx)]

Executar o aplicativo da web, atualizar e excluir todos os registros.

![Atualizar dados](updating-deleting-and-creating-data/_static/image3.png)

Observe que, quando no modo de edição de valor para a propriedade de ano automaticamente é renderizado como uma lista suspensa. A propriedade Year é um valor de enumeração e o modelo de dados dinâmicos para um valor de enumeração Especifica uma lista suspensa de edição. Você pode encontrar esse modelo abrindo o **enumeração\_Edit.ascx** arquivo o **DynamicData**/**FieldTemplates** pasta.

Se você fornecer valores válidos, a atualização for concluída com êxito. Se você violar um dos requisitos de validação, a atualização não continuará e uma mensagem de erro é exibida acima da grade.

![Mensagem de erro](updating-deleting-and-creating-data/_static/image4.png)

## <a name="add-new-records"></a>Adicionar novos registros

O controle GridView não inclui o **InsertMethod** propriedade e, portanto, não pode ser usado para adicionar um novo registro com associação de modelo. Você pode encontrar a propriedade InsertMethod no **FormView**, **DetailsView**, ou **ListView** controles. Neste tutorial, você usará um controle FormView para adicionar um novo registro.

Primeiro, adicione um link para a nova página que você criará para adicionar um novo registro. Acima ValidationSummary, adicione:

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample5.aspx)]

O novo link será exibido na parte superior do conteúdo para a página de alunos.

![novo link](updating-deleting-and-creating-data/_static/image5.png)

Em seguida, adicione um novo formulário da web usando uma página mestra e nomeie-o **AddStudent**. Selecione Site.Master como a página mestra.

Você irá processar os campos para adicionar um novo aluno usando um **DynamicEntity** controle. O controle DynamicEntity renderiza que propriedades editáveis na classe especificada na propriedade ItemType. A propriedade StudentID foi marcada com o **[ScaffoldColumn(false)]** atributo para que ele não será renderizado. O espaço reservado de MainContent da página AddStudent, adicione o código a seguir.

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample6.aspx)]

No arquivo code-behind (AddStudent.aspx.cs), adicione um **usando** instrução para o **ContosoUniversityModelBinding.Models** namespace.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample7.cs)]

Em seguida, adicione os seguintes métodos para especificar como inserir um novo registro e um manipulador de eventos para o botão Cancelar.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample8.cs)]

Salve todas as alterações.

Executar o aplicativo web e criar um novo aluno.

![Adicionar novo aluno](updating-deleting-and-creating-data/_static/image6.png)

Clique em **inserir** e observe o novo aluno foi criado.

![Exibir o novo aluno](updating-deleting-and-creating-data/_static/image7.png)

## <a name="conclusion"></a>Conclusão

Neste tutorial, você habilitou a atualização, exclusão e criação de dados. Você garante que as regras de validação são aplicadas ao interagir com os dados.

Na próxima [tutorial](sorting-paging-and-filtering-data.md) nesta série, você habilitará a classificação, paginação e filtragem de dados.

>[!div class="step-by-step"]
[Anterior](retrieving-data.md)
[Próximo](sorting-paging-and-filtering-data.md)
