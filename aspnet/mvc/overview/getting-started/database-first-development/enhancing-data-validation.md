---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: 'Banco de dados do EF primeiro com o ASP.NET MVC: Aprimorando a validação de dados | Microsoft Docs'
author: tfitzmac
description: Usando o MVC, Entity Framework e o Scaffolding do ASP.NET, você pode criar um aplicativo web que fornece uma interface para um banco de dados existente. Esta série de tutoriais...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/29/2014
ms.topic: article
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: a328aa8aec2c512d77ddabec31b3785b8742e018
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376708"
---
<a name="ef-database-first-with-aspnet-mvc-enhancing-data-validation"></a>Banco de dados do EF primeiro com o ASP.NET MVC: Aprimorando a validação de dados
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Usando o MVC, Entity Framework e o Scaffolding do ASP.NET, você pode criar um aplicativo web que fornece uma interface para um banco de dados existente. Esta série de tutoriais mostra como automaticamente gerar um código que permite aos usuários exibir, editar, criar e excluir dados que residem em uma tabela de banco de dados. O código gerado corresponde às colunas na tabela de banco de dados.
> 
> Esta parte da série se concentra em Adicionar anotações de dados para o modelo de dados para especificar os requisitos de validação e formatação de exibição. Ele foi aprimorado com base nos comentários dos usuários na seção de comentários.


## <a name="add-data-annotations"></a>Adicionar anotações de dados

Como você viu em um tópico anterior, algumas regras de validação de dados são aplicadas automaticamente para a entrada do usuário. Por exemplo, você só pode fornecer um número para a propriedade de nível empresarial. Para especificar mais regras de validação de dados, você pode adicionar anotações de dados à sua classe de modelo. Essas anotações são aplicadas em todo o seu aplicativo web para a propriedade especificada. Você também pode aplicar atributos de formatação que alterar como as propriedades são exibidas; Por exemplo, alterando o valor usado para rótulos de texto.

Neste tutorial, você irá adicionar anotações de dados para restringir o tamanho dos valores fornecidos para as propriedades FirstName, LastName e MiddleName. No banco de dados, esses valores são limitados a 50 caracteres. No entanto, em seu aplicativo web que limite de caracteres no momento, não é imposta. Se um usuário fornece mais de 50 caracteres para um desses valores, a página falhará ao tentar salvar o valor para o banco de dados. Também você restringirá a nível empresarial para valores entre 0 e 4.

Abra o **Student.cs** arquivo na **modelos** pasta. Adicione o seguinte código realçado à classe.

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

No Enrollment.cs, adicione o seguinte código realçado.

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

Compile a solução.

Navegue até uma página para editar ou criar um aluno. Se você tentar inserir mais de 50 caracteres, uma mensagem de erro será exibida.

![Mostrar mensagem de erro](enhancing-data-validation/_static/image1.png)

Navegue até a página para edição de registros e tenta fornecer um nível superior a 4.

![Erro de intervalo de nível empresarial](enhancing-data-validation/_static/image2.png)

Para obter uma lista completa de anotações de validação de dados, você pode aplicar a classes e propriedades, consulte [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).

## <a name="add-metadata-classes"></a>Adicionar classes de metadados

Adicionar os atributos de validação diretamente para a classe de modelo funciona quando você não espera que o banco de dados a ser alterado; No entanto, se as alterações de banco de dados e você precisar regenerar a classe de modelo, você perderá todos os atributos que você tivesse aplicado à classe de modelo. Essa abordagem pode ser muito ineficiente e propenso a perder as regras de validação importantes.

Para evitar esse problema, você pode adicionar uma classe de metadados que contém os atributos. Quando você associa a classe de modelo para a classe de metadados, esses atributos são aplicados ao modelo. Nessa abordagem, a classe de modelo pode ser regenerada sem perder todos os atributos que foram aplicados à classe de metadados.

No **modelos** pasta, adicione uma classe chamada **Metadata.cs**.

![Adicionar classe de metadados](enhancing-data-validation/_static/image3.png)

Substitua o código em Metadata.cs com o código a seguir.

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

Essas classes de metadados contêm todos os atributos de validação aplicados anteriormente para as classes de modelo. O **exibição** atributo é usado para alterar o valor usado para rótulos de texto.

Agora, você deve associar as classes de modelo com as classes de metadados.

No **modelos** pasta, adicione uma classe chamada **PartialClasses.cs**.

Substitua o conteúdo do arquivo pelo código a seguir.

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

Observe que cada classe é marcada como um `partial` classe e cada uma corresponde ao nome e namespace como a classe que é gerada automaticamente. Aplicando o atributo de metadados para a classe parcial, você certifique-se de que os atributos de validação de dados serão aplicados à classe gerada automaticamente. Esses atributos não serão perdidos quando você regenerar as classes de modelo porque o atributo de metadados é aplicado em classes parciais que não serão regeneradas.

Para regenerar as classes geradas automaticamente, abra o arquivo de ContosoModel.edmx. Mais uma vez, com o botão direito na superfície do design e selecione **modelo de atualização do banco de dados**. Embora você não alterou o banco de dados, esse processo regenerará as classes. No **Refresh** guia, selecione **tabelas** e **concluir**.

![Atualizar tabelas](enhancing-data-validation/_static/image4.png)

Salve o arquivo ContosoModel.edmx para aplicar as alterações.

Abra o arquivo de Student.cs ou o arquivo Enrollment.cs e observe que os atributos de validação de dados que você aplicou anteriormente não estão mais no arquivo. No entanto, executar o aplicativo e observe que as regras de validação ainda são aplicadas quando você insere dados.

> [!div class="step-by-step"]
> [Anterior](customizing-a-view.md)
> [Próximo](publish-to-azure.md)
