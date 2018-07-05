---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: A integração do JQuery UI Datepicker com associação de modelos e formulários da web | Microsoft Docs
author: tfitzmac
description: Esta série de tutoriais demonstra aspectos básicos de como usar a associação de modelo com um projeto de Web Forms do ASP.NET. Associação de modelo torna a interação de dados mais simples-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 7d60d2945dbf9daca33422ab82b9265fedc2ba31
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393821"
---
<a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a>A integração do JQuery UI Datepicker com associação de modelos e formulários da web
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Esta série de tutoriais demonstra aspectos básicos de como usar a associação de modelo com um projeto de Web Forms do ASP.NET. Associação de modelo torna a interação de dados mais simples que lidam com dados de objetos de origem (como ObjectDataSource ou SqlDataSource). Esta série começa com material introdutório e move para conceitos mais avançados em tutoriais posteriores.
> 
> Este tutorial mostra como adicionar o JQuery UI [widget Datepicker](http://jqueryui.com/datepicker/) a um formulário da Web e usar o modelo de associação para atualizar o banco de dados com o valor selecionado.
> 
> Este tutorial se baseia no projeto criado a [primeira](retrieving-data.md) e [segundo](updating-deleting-and-creating-data.md) partes da série.
> 
> Você pode [baixar](https://go.microsoft.com/fwlink/?LinkId=286116) o projeto completo em c# ou VB. O código para download funciona com o Visual Studio 2012 ou Visual Studio 2013. Ele usa o modelo do Visual Studio 2012, que é ligeiramente diferente do que o modelo do Visual Studio 2013 mostrado neste tutorial.


## <a name="what-youll-build"></a>O que você vai criar

Neste tutorial, você vai:

1. Adicionar uma propriedade ao seu modelo para registrar a data de registro do aluno
2. Permitir que o usuário selecionar a data de registro usando o widget Datepicker do JQuery UI
3. Impor regras de validação para a data de registro

O widget Datepicker do JQuery UI permite aos usuários selecionar facilmente uma data em um calendário pop-up quando o usuário interage com o campo. Usar este widget pode ser mais conveniente para os usuários do que digitar manualmente uma data. Integrar o widget Datepicker em uma página que usa a associação de modelo para operações de dados requer apenas uma pequena quantidade de trabalho adicional.

## <a name="add-a-new-property-to-the-model"></a>Adicionar uma nova propriedade ao modelo

Primeiro, você adicionará uma **Datetime** propriedade para seus estudantes de modelo e migrar essa alteração no banco de dados. Abra **UniversityModels.cs**e adicione o código realçado ao modelo de aluno.

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

O **RangeAttribute** é incluída para impor regras de validação para a propriedade. Para este tutorial, vamos pressupor que a Contoso University foi fundada em 1º de janeiro de 2013 e, portanto, as datas de registro anteriores não são válidas.

Na janela Gerenciamento de pacotes, adicione uma migração, executando o comando **AddEnrollmentDate migração adicionar**. Observe que o código de migração adiciona a nova coluna de data e hora para a tabela aluno. Para corresponder ao valor especificado no RangeAttribute, adicione um valor padrão para a nova coluna, conforme mostrado no código realçado a seguir.

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

Salve a alteração no arquivo de migração.

Não é preciso propagar os dados novamente. Portanto, abra **Configuration.cs** na pasta migrações e remova ou comente o código na **semente** método. Salve e feche o arquivo.

Agora, execute o comando **update-database**. Observe que agora existe a coluna no banco de dados e todos os registros existentes têm o valor padrão para EnrollmentDate.

## <a name="add-dynamic-controls-for-enrollment-date"></a>Adicionar controles dinâmicos para a data de registro

Agora você irá adicionar controles para exibir e editar a data de inscrição. Neste ponto, o valor será editado por meio de uma caixa de texto. Posteriormente no tutorial, você irá alterar a caixa de texto para o widget de Datepicker do JQuery.

Primeiro, é importante observar que você não precisa fazer nenhuma alteração para o **AddStudent.aspx** arquivo. O controle DynamicEntity renderizará automaticamente a nova propriedade.

Abra **Students.aspx**e adicione o seguinte código realçado.

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

Execute o aplicativo e observe que você pode definir o valor da data de inscrição digitando uma data. Ao adicionar um novo aluno:

![definir a data](integrating-jquery-ui/_static/image1.png)

Ou, edição de um valor existente:

![Editar Data](integrating-jquery-ui/_static/image2.png)

Digitar a data funciona, mas ele pode não ser a experiência do cliente que você deseja fornecer. Na próxima seção, você habilitará a seleção de uma data por meio de um calendário.

## <a name="install-nuget-package-to-work-with-jquery-ui"></a>Instalar o pacote do NuGet para trabalhar com o JQuery UI

O **suco de interface do usuário do** pacote NuGet permite fácil integração dos widgets do JQuery UI ao seu aplicativo web. Para usar este pacote, instalá-lo por meio do NuGet.

![Adicionar suco da interface do usuário](integrating-jquery-ui/_static/image3.png)

A versão de suco de interface do usuário que você instala pode entrar em conflito com a versão do JQuery em seu aplicativo. Antes de continuar com este tutorial, tente executar seu aplicativo. Se você encontrar um erro de JavaScript, você precisa reconciliar a versão do JQuery. Você pode adicionar a versão esperada do JQuery para sua pasta de Scripts (versão 1.8.2 no momento da redação deste tutorial) ou no site, especifique o caminho para o arquivo do JQuery.

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a>Personalizar o modelo de data e hora para incluir o widget Datepicker

Você adicionará o widget Datepicker o modelo de dados dinâmicos para a edição de um valor de data e hora. Adicionando o widget para o modelo, ele automaticamente será renderizado no formulário para adicionar um novo aluno e na exibição de grade para alunos de edição. Abra **DateTime\_Edit.ascx**e adicione o seguinte código realçado.

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

O arquivo code-behind, você definirá as datas mínimas e máxima para o DatePicker. Ao definir esses valores, você impedirá que os usuários naveguem para datas inválidas. Você vai recuperar os valores mínimo e máximo do **RangeAttribute** na propriedade de data e hora, se fornecido. Abra **DateTime\_Edit.ascx.cs**e adicione o seguinte código realçado para a página\_método de carga.

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

Executar o aplicativo web e navegue até a página AddStudent. Forneça valores para os campos e observe que, quando você clica na caixa de texto para data de registro, o calendário é exibido.

![seletor de data](integrating-jquery-ui/_static/image4.png)

Escolha uma data e, em seguida, clique em **inserir**. O RangeAttribute impõe a validação no servidor. Definindo a propriedade minDate no Datepicker, você também deve aplicar validação no cliente. O calendário não permite que o usuário navegar para uma data antes do valor de minDate.

Quando você edita um registro na exibição de grade, o calendário também é exibido.

![DatePicker em GridView](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a>Conclusão

Neste tutorial, você aprendeu como incorporar um widget JQuery em um formulário da web que usa a associação de modelos.

Nos próximos [tutorial](using-query-string-values-to-retrieve-data.md), você usará um valor de cadeia de caracteres de consulta ao selecionar os dados.

> [!div class="step-by-step"]
> [Anterior](sorting-paging-and-filtering-data.md)
> [Próximo](using-query-string-values-to-retrieve-data.md)
