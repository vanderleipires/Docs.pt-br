---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: Adicionar camada de lógica de negócios para um projeto que usa o modelo de associação e formulários da web | Microsoft Docs
author: tfitzmac
description: Esta série de tutoriais demonstra aspectos básicos do uso de associação de modelo com um projeto Web Forms do ASP.NET. Associação de modelo torna a interação de dados mais estreita-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: 25e887bdc316abf65c780bb6c8d075e938e85064
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a>Adicionar camada de lógica de negócios para um projeto que usa o modelo de associação e formulários da web
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Esta série de tutoriais demonstra aspectos básicos do uso de associação de modelo com um projeto Web Forms do ASP.NET. Associação de modelo facilita a interação de dados mais direta de lidar com dados de objetos de origem (como ObjectDataSource ou SqlDataSource). Esta série começa com material introdutório e move para conceitos mais avançados em tutoriais subsequentes.
> 
> Este tutorial mostra como usar a associação de modelo com uma camada de lógica de negócios. Defina o membro OnCallingDataMethods para especificar que um objeto que não sejam a página atual é usado para chamar os métodos de dados.
> 
> Este tutorial baseia-se no projeto criado no [anteriores](retrieving-data.md) partes da série.
> 
> Você pode [baixar](https://go.microsoft.com/fwlink/?LinkId=286116) o projeto completo em c# ou VB. O código para download funciona com o Visual Studio 2012 ou Visual Studio 2013. Ele usa o modelo do Visual Studio 2012, que é ligeiramente diferente do que o modelo do Visual Studio 2013 mostrado neste tutorial.


## <a name="what-youll-build"></a>Será possível compilar

Associação de modelo permite que você coloque o código de interação de dados em que o arquivo de code-behind para uma página da web ou em uma classe de lógica de negócios diferentes. Os tutoriais anteriores mostraram como usar os arquivos code-behind para o código de interação de dados. Essa abordagem funciona para pequenos sites, mas pode levar a repetição e a maior dificuldade de código durante a manutenção de um site grande. Ele também pode ser muito difícil testar o código que reside no code-behind arquivos porque não há nenhuma camada de abstração programaticamente.

Para centralizar o código de interação de dados, você pode criar uma camada de lógica de negócios que contém toda a lógica para interagir com dados. Chame a camada de lógica de negócios de suas páginas da web. Este tutorial mostra como mover todo o código que você tenha escrito nos tutoriais anteriores em uma camada de lógica de negócios e, em seguida, usar esse código das páginas.

Neste tutorial, você vai:

1. Mova o código de arquivos code-behind para uma camada de lógica de negócios
2. Alterar os controles de dados vinculados para chamar os métodos na camada de lógica de negócios

## <a name="create-business-logic-layer"></a>Criar camada de lógica comercial

Agora, você criará a classe que é chamada de páginas da web. Os métodos dessa classe semelhante para os métodos usados nos tutoriais anteriores e incluem os atributos de provedor de valor.

Primeiro, adicione uma nova pasta chamada **BLL**.

![Adicionar pasta](adding-business-logic-layer/_static/image1.png)

Na pasta BLL, crie uma nova classe chamada **SchoolBL.cs**. Ele conterá todas as operações de dados residiam originalmente em arquivos code-behind. Os métodos são quase o mesmo que os métodos no arquivo code-behind, mas incluem algumas alterações.

A alteração mais importante a observar é que você não estiver executando o código de dentro de uma instância de **página** classe. A classe da página contém o **TryUpdateModel** método e o **ModelState** propriedade. Quando esse código é movido para uma camada de lógica de negócios, você não tiver uma instância da classe de página para chamar esses membros. Para contornar esse problema, você deve adicionar um **ModelMethodContext** parâmetro para qualquer método que acessa TryUpdateModel ou ModelState. Você pode usar esse parâmetro ModelMethodContext para chamar TryUpdateModel ou recuperar ModelState. Você não precisa alterar nada na página da web para a conta para esse novo parâmetro.

Substitua o código em SchoolBL.cs com o código a seguir.

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a>Revisar páginas existentes para recuperar dados da camada de lógica comercial

Por fim, você converterá as páginas Students.aspx, AddStudent.aspx e Courses.aspx uso de consultas o arquivo code-behind para usar a camada de lógica de negócios.

Em que os arquivos code-behind para estudantes, AddStudent e cursos, exclua ou comente a métodos de consulta a seguir:

- studentsGrid\_GetData
- studentsGrid\_UpdateItem
- studentsGrid\_DeleteItem
- addStudentForm\_InsertItem
- coursesGrid\_GetData

Agora você não deve ter nenhum código no arquivo de code-behind que pertencem a operações de dados.

O **OnCallingDataMethods** manipulador de eventos permite que você especifique um objeto a ser usado para os métodos de dados. No Students.aspx, adicione um valor para o manipulador de eventos e altere os nomes dos métodos para os nomes dos métodos na classe de lógica de negócios.

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

No arquivo de code-behind para Students.aspx, defina o manipulador de eventos para o evento CallingDataMethods. No manipulador de eventos, você deve especificar a classe de lógica de negócios para operações de dados.

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

Em AddStudent.aspx, fazer alterações semelhantes.

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

Em Courses.aspx, fazer alterações semelhantes.

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

Execute o aplicativo e observe que todas as páginas funcionem como tinham anteriormente. A lógica de validação também funciona corretamente.

## <a name="conclusion"></a>Conclusão

Neste tutorial, você estruturado novamente seu aplicativo para usar uma camada de acesso a dados e a camada de lógica comercial. Você especificou que os controles de dados usam um objeto que não é a página atual para operações de dados.

> [!div class="step-by-step"]
> [Anterior](using-query-string-values-to-retrieve-data.md)
