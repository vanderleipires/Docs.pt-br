---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: "Integrando o JQuery UI Datepicker com o modelo de associação e formulários da web | Microsoft Docs"
author: tfitzmac
description: "Esta série de tutoriais demonstra aspectos básicos do uso de associação de modelo com um projeto Web Forms do ASP.NET. Associação de modelo torna a interação de dados mais estreita-..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: da3c8f347a709a4c9a47fd0ecce5201d9b0cd1b1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a>Integrando o JQuery UI Datepicker com o modelo de associação e formulários da web
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Esta série de tutoriais demonstra aspectos básicos do uso de associação de modelo com um projeto Web Forms do ASP.NET. Associação de modelo facilita a interação de dados mais direta de lidar com dados de objetos de origem (como ObjectDataSource ou SqlDataSource). Esta série começa com material introdutório e move para conceitos mais avançados em tutoriais subsequentes.
> 
> Este tutorial mostra como adicionar o JQuery UI [widget Datepicker](http://jqueryui.com/datepicker/) para um formulário da Web e usar o modelo de associação para atualizar o banco de dados com o valor selecionado.
> 
> Este tutorial baseia-se no projeto criado no [primeiro](retrieving-data.md) e [segundo](updating-deleting-and-creating-data.md) partes da série.
> 
> Você pode [baixar](https://go.microsoft.com/fwlink/?LinkId=286116) o projeto completo em c# ou VB. O código para download funciona com o Visual Studio 2012 ou Visual Studio 2013. Ele usa o modelo do Visual Studio 2012, que é ligeiramente diferente do que o modelo do Visual Studio 2013 mostrado neste tutorial.


## <a name="what-youll-build"></a>Será possível compilar

Neste tutorial, você vai:

1. Adicionar uma propriedade ao modelo para registrar a data de inscrição do aluno
2. Permitir que o usuário selecionar a data de registro usando o widget JQuery UI Datepicker
3. Aplicar regras de validação para a data de inscrição

O widget JQuery UI Datepicker permite aos usuários selecionar uma data em um calendário que aparece quando o usuário interage com o campo. Pode ser mais conveniente para os usuários que manualmente digitando uma data usar este widget. Integrar o widget Datepicker em uma página que usa uma associação de modelo para operações de dados requer apenas uma pequena quantidade de trabalho adicional.

## <a name="add-a-new-property-to-the-model"></a>Adicionar uma nova propriedade ao modelo

Primeiro, você adicionará um **Datetime** propriedade seu aluno do modelo e migrar essa alteração no banco de dados. Abra **UniversityModels.cs**e adicione o código realçado para o modelo do aluno.

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

O **RangeAttribute** é incluída para impor regras de validação para a propriedade. Para este tutorial, vamos pressupor que Contoso University foi fundado em 1º de janeiro de 2013 e, portanto, as datas de registro anteriores não são válidas.

Na janela Gerenciamento de pacotes, adicione uma migração, executando o comando **AddEnrollmentDate migração adicionar**. Observe que o código de migração adiciona a nova coluna de data e hora para a tabela de alunos. Para corresponder ao valor especificado no RangeAttribute, adicione um valor padrão para a nova coluna, conforme mostrado no código realçado abaixo.

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

Salve suas alterações no arquivo de migração.

Não é necessário propagar os dados novamente. Por isso, abra **Configuration.cs** na pasta migrações e remova ou comentar o código de **semente** método. Salve e feche o arquivo.

Agora, execute o comando **Atualizar banco de dados**. Observe que a coluna agora existe no banco de dados e todos os registros existentes têm o valor padrão para EnrollmentDate.

## <a name="add-dynamic-controls-for-enrollment-date"></a>Adicionar controles dinâmicos para a data de inscrição

Agora você irá adicionar controles para exibir e editar a data de inscrição. Neste ponto, o valor é editado por meio de uma caixa de texto. Posteriormente no tutorial, você irá alterar a caixa de texto para o widget JQuery Datepicker.

Primeiro, é importante observar que não é necessário fazer qualquer alteração de **AddStudent.aspx** arquivo. O controle DynamicEntity automaticamente processe a nova propriedade.

Abra **Students.aspx**e adicione o seguinte código realçado.

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

Execute o aplicativo e observe que você pode definir o valor da data de inscrição digitando uma data. Ao adicionar um novo aluno:

![definir a data](integrating-jquery-ui/_static/image1.png)

Ou editar um valor existente:

![Data da edição](integrating-jquery-ui/_static/image2.png)

Digitar a data funciona, mas ele pode não ser a experiência do cliente que você deseja fornecer. Na próxima seção, você permitirá selecionar uma data por meio de um calendário.

## <a name="install-nuget-package-to-work-with-jquery-ui"></a>Instalar o pacote do NuGet para funcionar com JQuery UI

O **suco de interface do usuário** pacote NuGet permite uma fácil integração de widgets JQuery UI em seu aplicativo web. Para usar este pacote, instale-o através do NuGet.

![Adicionar suco de interface do usuário](integrating-jquery-ui/_static/image3.png)

A versão da interface do usuário de suco de instalação pode entrar em conflito com a versão do JQuery em seu aplicativo. Antes de continuar com este tutorial, tente executar o aplicativo. Se você encontrar um erro de JavaScript, você precisa reconciliar a versão JQuery. Você pode adicionar a versão esperada do JQuery para a pasta de Scripts (versão 1.8.2 em vez de escrever este tutorial) ou Site.master Especifica o caminho para o arquivo JQuery.

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a>Personalizar o modelo de data e hora para incluir o widget Datepicker

Você adicionará o widget Datepicker no modelo de dados dinâmicos para a edição de um valor datetime. Adicionando o widget para o modelo, ele automaticamente será renderizado no formulário para adicionar um novo aluno e na exibição de grade para edição de estudantes. Abra **DateTime\_Edit.ascx**e adicione o seguinte código realçado.

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

O arquivo code-behind, você definirá as datas de mínimas e máxima para o selecionador de data. Ao definir esses valores, você impedirá que os usuários de navegar para datas inválidas. Você vai recuperar os valores mínimo e máximo do **RangeAttribute** na propriedade de data e hora, se houver. Abra **DateTime\_Edit.ascx.cs**e adicione o seguinte código para a página\_método de carga.

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

Execute o aplicativo web e navegue até a página AddStudent. Forneça valores para os campos e observe que, quando você clicar na caixa de texto de data de inscrição, o calendário será exibido.

![seletor de data](integrating-jquery-ui/_static/image4.png)

Escolha uma data e, em seguida, clique em **inserir**. O RangeAttribute impõe validação no servidor. Definindo a propriedade minDate de Datepicker, você também deve aplicar a validação no cliente. O calendário não permite que o usuário navegar para uma data antes do valor de minDate.

Quando você editar um registro na exibição de grade, o calendário também é exibido.

![DatePicker em GridView](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a>Conclusão

Neste tutorial, você aprendeu como incorporar um widget JQuery em um formulário da web que usa a associação de modelo.

Na próxima [tutorial](using-query-string-values-to-retrieve-data.md), você usará um valor de cadeia de caracteres de consulta ao selecionar os dados.

>[!div class="step-by-step"]
[Anterior](sorting-paging-and-filtering-data.md)
[Próximo](using-query-string-values-to-retrieve-data.md)
