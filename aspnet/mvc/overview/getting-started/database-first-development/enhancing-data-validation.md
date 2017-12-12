---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: "Banco de dados EF primeiro com o ASP.NET MVC: melhorando a validação de dados | Microsoft Docs"
author: tfitzmac
description: "Usando o MVC, Entity Framework e estrutura do ASP.NET, você pode criar um aplicativo web que fornece uma interface para um banco de dados existente. Este tutorial série..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/29/2014
ms.topic: article
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: 465be4b51ab280d0b3e2f4b33362fa771480d5e1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="ef-database-first-with-aspnet-mvc-enhancing-data-validation"></a>Banco de dados EF primeiro com o ASP.NET MVC: melhorando a validação de dados
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Usando o MVC, Entity Framework e estrutura do ASP.NET, você pode criar um aplicativo web que fornece uma interface para um banco de dados existente. Esta série de tutorial mostra como automaticamente gerar um código que permite aos usuários exibir, editar, criar e excluir dados que residem em uma tabela de banco de dados. O código gerado corresponde às colunas na tabela de banco de dados.
> 
> Esta parte da série se concentra na adição de anotações de dados para o modelo de dados para especificar os requisitos de validação e formatação de exibição. Foi aprimorado com base nos comentários dos usuários na seção comentários.


## <a name="add-data-annotations"></a>Adicionar anotações de dados

Como você viu um tópico anterior, algumas regras de validação de dados são aplicadas automaticamente a entrada do usuário. Por exemplo, você só pode fornecer um número para a propriedade de classificação. Para especificar mais regras de validação de dados, você pode adicionar anotações de dados à sua classe de modelo. Essas anotações são aplicadas em todo o aplicativo web para a propriedade especificada. Você também pode aplicar os atributos de formatação que alterar como as propriedades são exibidas; como alterar o valor usado para rótulos de texto.

Neste tutorial, você irá adicionar anotações de dados para restringir o comprimento dos valores fornecidos para as propriedades de FirstName, LastName e MiddleName. No banco de dados, esses valores são limitados a 50 caracteres. No entanto, em seu aplicativo web que limite de caracteres no momento não é imposta. Se um usuário fornece a mais de 50 caracteres para um desses valores, a página falhará ao tentar salvar o valor para o banco de dados. Também você restringirá a classificação para valores entre 0 e 4.

Abra o **Student.cs** de arquivos no **modelos** pasta. Adicione o seguinte código à classe.

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

No Enrollment.cs, adicione o seguinte código realçado.

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

Compile a solução.

Navegue até uma página para editar ou criar um aluno. Se você tentar inserir mais de 50 caracteres, uma mensagem de erro será exibida.

![Mostrar mensagem de erro](enhancing-data-validation/_static/image1.png)

Navegue até a página de edição de registros e tenta fornecer um nível superior a 4.

![Erro de intervalo da classificação](enhancing-data-validation/_static/image2.png)

Para obter uma lista completa de anotações de validação de dados, você pode aplicar a classes e propriedades, consulte [DataAnnotations](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx).

## <a name="add-metadata-classes"></a>Adicionar classes de metadados

Adicionar os atributos de validação diretamente para a classe de modelo funciona quando você não espera que o banco de dados a ser alterado; No entanto, se as alterações do banco de dados e você precisar regenerar a classe de modelo, você perderá todos os atributos aplicado à classe de modelo. Essa abordagem pode ser muito ineficiente e propenso a perder as regras de validação importantes.

Para evitar esse problema, você pode adicionar uma classe de metadados que contém os atributos. Quando você associa a classe de modelo para a classe de metadados, esses atributos são aplicados ao modelo. Nessa abordagem, a classe de modelo pode ser regenerada sem perder todos os atributos que foram aplicados à classe de metadados.

No **modelos** pasta, adicione uma classe denominada **Metadata.cs**.

![Adicionar classe de metadados](enhancing-data-validation/_static/image3.png)

Substitua o código em Metadata.cs com o código a seguir.

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

Essas classes de metadados contém todos os atributos de validação aplicados anteriormente para as classes de modelo. O **exibição** atributo é usado para alterar o valor usado para rótulos de texto.

Agora, você deve associar as classes de modelo com as classes de metadados.

No **modelos** pasta, adicione uma classe denominada **PartialClasses.cs**.

Substitua o conteúdo do arquivo com o código a seguir.

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

Observe que cada classe está marcada como um `partial` classe e cada coincide com o nome e o namespace da classe que é gerada automaticamente. Aplicando o atributo de metadados para a classe parcial, você garantir que os atributos de validação de dados serão aplicados à classe gerada automaticamente. Esses atributos não serão perdidos quando você regenerar as classes de modelo, como o atributo de metadados é aplicado em classes parciais não são geradas novamente.

Para gerar novamente as classes geradas automaticamente, abra o arquivo de ContosoModel.edmx. Novamente, clique na superfície de design e selecione **modelo de atualização de banco de dados**. Embora você não alterou o banco de dados, esse processo irá regenerar as classes. No **atualizar** guia, selecione **tabelas** e **concluir**.

![Atualizar tabelas](enhancing-data-validation/_static/image4.png)

Salve o arquivo ContosoModel.edmx para aplicar as alterações.

Abra o arquivo Student.cs ou Enrollment.cs e observe que os atributos de validação de dados que você aplicou anteriormente não estão mais no arquivo. No entanto, executar o aplicativo e observe que as regras de validação ainda são aplicadas quando você insere dados.

>[!div class="step-by-step"]
[Anterior](customizing-a-view.md)
[Próximo](publish-to-azure.md)
