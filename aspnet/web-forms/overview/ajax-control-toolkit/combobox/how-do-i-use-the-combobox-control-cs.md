---
uid: web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-cs
title: Como usar o controle ComboBox? (C#) | Microsoft Docs
author: microsoft
description: Caixa de combinação é um controle AJAX do ASP.NET que combina a flexibilidade de uma caixa de texto com uma lista de opções do qual os usuários podem escolher.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 0bbf4134-04df-4226-8930-d5bb99e27128
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 20afd7334437a021f6f68216f84406eef5ea65c6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875221"
---
<a name="how-do-i-use-the-combobox-control-c"></a>Como usar o controle ComboBox? (C#)
====================
por [Microsoft](https://github.com/microsoft)

> Caixa de combinação é um controle AJAX do ASP.NET que combina a flexibilidade de uma caixa de texto com uma lista de opções do qual os usuários podem escolher.


O objetivo deste tutorial é explicar o controle de caixa de combinação de kit de ferramentas de controle AJAX. A caixa de combinação funciona como uma combinação entre um controle padrão do ASP.NET DropDownList e um controle de caixa de texto. Você pode selecionar em uma lista pré-existente de itens ou insira um novo item.

A caixa de combinação é semelhante ao extensor de controle de preenchimento automático, mas os controles são usados em cenários diferentes. O extensor AutoComplete consulta um serviço web para obter entradas correspondentes. O controle ComboBox, por outro lado, é inicializado com um conjunto de itens. Usando o preenchimento automático extensor faz sentido quando você estiver trabalhando com um grande conjunto de dados (em milhões de partes de carro) ao usar o controle ComboBox faz sentido quando estiver trabalhando com um pequeno conjunto de dados (dezenas de partes de carro).

## <a name="selecting-from-a-static-list-of-items"></a>A seleção de uma lista estática de itens

Permitir que o s começar com um exemplo simple de usar o controle de caixa de combinação. Imagine que você deseja exibir uma lista estática de itens em uma lista suspensa. No entanto, você deseja deixar aberta a possibilidade de que a lista não foi concluída. Você deseja permitir que um usuário insira um valor personalizado na lista.

Podemos ll criar uma nova página de Web Forms do ASP.NET e use o controle ComboBox na página. Adicione a nova página ASP.NET ao seu projeto e alterne para o modo de Design.

Se você quiser usar o controle ComboBox na página deve adicionar um controle ScriptManager à página. Arraste o controle ScriptManager do guia Extensões AJAX para a superfície do Designer. Você deve adicionar o controle ScriptManager na parte superior da página. Você pode adicioná-lo imediatamente abaixo do lado do servidor abertura &lt;formulário&gt; marca.

Em seguida, arraste o controle de caixa de combinação para a página. Você pode encontrar o controle ComboBox na caixa de ferramentas com outros controles do AJAX Control Toolkit e extensores de controle (consulte a Figura 1).


[![Formulário Simple para criar um cartão de visita](how-do-i-use-the-combobox-control-cs/_static/image1.jpg)](how-do-i-use-the-combobox-control-cs/_static/image1.png)

**Figura 01**: selecionando o controle de caixa de combinação da caixa de ferramentas ([clique para exibir a imagem em tamanho normal](how-do-i-use-the-combobox-control-cs/_static/image2.png))


Podemos ll usar o controle ComboBox para exibir uma lista estática de opções. O usuário pode selecionar um nível específico de spiciness para o alimento em uma lista de três opções: leve, médio e acesso (consulte a Figura 2).


[![A seleção de uma lista estática de itens](how-do-i-use-the-combobox-control-cs/_static/image2.jpg)](how-do-i-use-the-combobox-control-cs/_static/image3.png)

**Figura 02**: seleção de uma lista estática de itens ([clique para exibir a imagem em tamanho normal](how-do-i-use-the-combobox-control-cs/_static/image4.png))


Há duas maneiras que você pode adicionar essas opções para o controle de caixa de combinação. Primeiro, selecione a opção de tarefa de opções de edição quando posicionar o mouse sobre o controle no modo de Design e abra o Editor de Item (consulte a Figura 3).


[![Editando itens de caixa de combinação](how-do-i-use-the-combobox-control-cs/_static/image3.jpg)](how-do-i-use-the-combobox-control-cs/_static/image5.png)

**Figura 03**: itens de edição de ComboBox ([clique para exibir a imagem em tamanho normal](how-do-i-use-the-combobox-control-cs/_static/image6.png))


A segunda opção é adicionar a lista de itens entre a abertura e fechamento &lt;asp: ComboBox&gt; marcas no modo de exibição de origem. A página na listagem 1 contém a ComboBox atualizada com a lista de itens.

**Listando 1 - Static.aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-cs/samples/sample1.aspx)]

Quando você abre a página na listagem 1, você pode selecionar uma das opções pré-existente a ComboBox. Em outras palavras, a caixa de combinação funciona como um controle DropDownList.

No entanto, você também tem a opção de inserir uma nova opção (por exemplo, Super Spicy) que não está na lista existente. Portanto, a caixa de combinação também funciona como um controle de caixa de texto.

Independentemente de se você escolher um pré-existente item ou inserir um item personalizado, ao enviar o formulário, a opção é exibida no controle de rótulo. Quando você envia o formulário, o btnSubmit\_clique manipulador executa e atualiza o rótulo (consulte a Figura 4).


[![Exibindo o item selecionado](how-do-i-use-the-combobox-control-cs/_static/image4.jpg)](how-do-i-use-the-combobox-control-cs/_static/image7.png)

**Figura 04**: exibindo o item selecionado ([clique para exibir a imagem em tamanho normal](how-do-i-use-the-combobox-control-cs/_static/image8.png))


A caixa de combinação suporta as mesmas propriedades de controle DropDownList para recuperar o item selecionado depois de um formulário é enviado:

- SelectedItem.Text - exibe o valor da propriedade Text do item selecionado.
- SelectedItem.Value - exibe o valor da propriedade Value do item selecionado ou o texto digitado na ComboBox.
- SelectedValue - mesmo que SelectedItem.Value, exceto pelo fato dessa propriedade permite que você especifique o item selecionado (inicial) padrão.

Se você digitar uma opção personalizada na caixa de combinação, em seguida, a opção personalizada é atribuída a propriedades de SelectedItem.Text e SelectedItem.Value.

## <a name="selecting-the-list-of-items-from-the-database"></a>Selecionando a lista de itens do banco de dados

Você pode recuperar a lista de itens que exibe a caixa de combinação de um banco de dados. Por exemplo, você pode vincular a caixa de combinação para um controle SqlDataSource, um controle ObjectDataSource, um LinqDataSource ou um EntityDataSource.

Imagine que você deseja exibir uma lista de filmes em uma caixa de combinação. Você deseja recuperar a lista de filmes da tabela de banco de dados de filmes. Siga estas etapas:

1. Crie uma página chamada Movies.aspx
2. Adicione um controle ScriptManager à página, arrastando o ScriptManager-guia Extensões AJAX na caixa de ferramentas para a página.
3. Adicione um controle de caixa de combinação para a página arrastando a caixa de combinação para a página.
4. No modo Design, passe o mouse sobre o controle de caixa de combinação e selecione o **Escolher fonte de dados** opção da tarefa (consulte a Figura 5). O Assistente de configuração de fonte de dados é iniciado.
5. No **escolher uma fonte de dados** etapa, selecione o &lt;nova fonte de dados&gt; opção.
6. No **escolher um tipo de fonte de dados** etapa, selecione o banco de dados.
7. No **escolha sua Conexão de dados** etapa, selecione o banco de dados (por exemplo, MoviesDB.mdf).
8. No **salvar a cadeia de Conexão no arquivo de configuração do aplicativo** etapa, selecione a opção para salvar a cadeia de conexão.
9. No **configurar a instrução Select** etapa, selecione a tabela de banco de dados de filmes e selecionar todas as colunas.
10. No **consulta teste** etapa, clique no botão Concluir.
11. Volta o **Escolher fonte de dados** etapa, selecione a coluna de título para o campo a ser exibido e a coluna de Id para os dados do campo (consulte a figura).
12. Clique no botão Okey para fechar o assistente.


[![Escolhendo uma fonte de dados](how-do-i-use-the-combobox-control-cs/_static/image5.jpg)](how-do-i-use-the-combobox-control-cs/_static/image9.png)

**Figura 05**: escolher uma fonte de dados ([clique para exibir a imagem em tamanho normal](how-do-i-use-the-combobox-control-cs/_static/image10.png))


[![Escolha os campos de texto e o valor de dados](how-do-i-use-the-combobox-control-cs/_static/image6.jpg)](how-do-i-use-the-combobox-control-cs/_static/image11.png)

**Figura 06**: escolhendo os campos de texto e o valor de dados ([clique para exibir a imagem em tamanho normal](how-do-i-use-the-combobox-control-cs/_static/image12.png))


Depois de concluir as etapas acima, a caixa de combinação está associada a um controle SqlDataSource que representa os filmes da tabela de banco de dados de filmes. A fonte da página é semelhante a listagem 2 (que limpa um pouco a formatação).

**A listagem 2 - Movies.aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-cs/samples/sample2.aspx)]

Observe que o controle ComboBox tem uma propriedade DataSourceID que aponta para o controle SqlDataSource. Quando você abre a página em um navegador, a lista de filmes do banco de dados é exibida (consulte a Figura 7). Você pode uma escolha um filme na lista ou digite um novo filme digitando o filme no ComboBox.


[![Exibindo uma lista de filmes](how-do-i-use-the-combobox-control-cs/_static/image7.jpg)](how-do-i-use-the-combobox-control-cs/_static/image13.png)

**Figura 07**: exibindo uma lista de filmes ([clique para exibir a imagem em tamanho normal](how-do-i-use-the-combobox-control-cs/_static/image14.png))


## <a name="setting-the-dropdownstyle"></a>Definindo o DropDownStyle

Você pode usar a propriedade ComboBox DropDownStyle para alterar o comportamento do ComboBox. Essa propriedade aceita há valores possíveis:

- Lista suspensa - exibe a caixa de combinação de (valor padrão) uma lista suspensa lista quando você clicar na seta e você pode inserir um valor personalizado.
- Simples - a caixa de combinação exibe uma lista suspensa automaticamente e você pode inserir um valor personalizado.
- DropDownList - ComboBox funciona como um controle DropDownList.

A diferença entre a lista suspensa e simples é quando a lista de itens é exibida. No caso simples, a lista é exibida imediatamente quando você move o foco para a caixa de combinação. No caso do menu suspenso, você deve clicar na seta para ver a lista de itens.

O valor de DropDownList faz com que o controle de caixa de combinação funcionar como um controle DropDownList padrão. No entanto, há uma importante diferença. Versões mais antigas do Internet Explorer exibem um controle DropDownList com um índice z infinito, portanto, o controle será exibido na frente de qualquer controle colocado na frente dele. Porque a caixa de combinação renderiza uma marca HTML &lt;div&gt; marca em vez de uma marca HTML &lt;selecione&gt; marca, a caixa de combinação corretamente respeita ordem z.

## <a name="setting-the-autocompletemode"></a>Definindo o AutoCompleteMode

Você pode usar a propriedade ComboBox AutoCompleteMode para especificar o que acontece quando alguém digita texto no ComboBox. Essa propriedade aceita os seguintes valores possíveis:

- Nenhum - (valor padrão) a caixa de combinação não tem nenhum comportamento de preenchimento automático.
- Sugerir - a caixa de combinação exibe a lista e realça o item correspondente na lista (consulte a Figura 8).
- Acrescente - a ComboBox não exibir a lista e ele adiciona o item correspondente da lista para o que você digitou (consulte a Figura 9).
- SuggestAppend - a caixa de combinação exibe a lista e anexa o item correspondente da lista para o que você digitou (consulte a Figura 10).


[![A caixa de combinação torna uma sugestão](how-do-i-use-the-combobox-control-cs/_static/image8.jpg)](how-do-i-use-the-combobox-control-cs/_static/image15.png)

**Figura 08**: ComboBox o torna uma sugestão ([clique para exibir a imagem em tamanho normal](how-do-i-use-the-combobox-control-cs/_static/image16.png))


[![Caixa de combinação acrescenta texto correspondente](how-do-i-use-the-combobox-control-cs/_static/image9.jpg)](how-do-i-use-the-combobox-control-cs/_static/image17.png)

**Figura 09**: ComboBox acrescenta texto correspondente ([clique para exibir a imagem em tamanho normal](how-do-i-use-the-combobox-control-cs/_static/image18.png))


[![A caixa de combinação sugere e acrescenta](how-do-i-use-the-combobox-control-cs/_static/image10.jpg)](how-do-i-use-the-combobox-control-cs/_static/image19.png)

**Figura 10**: A ComboBox sugere e acrescenta ([clique para exibir a imagem em tamanho normal](how-do-i-use-the-combobox-control-cs/_static/image20.png))


## <a name="summary"></a>Resumo

Neste tutorial, você aprendeu a usar o controle ComboBox para exibir um conjunto fixo de itens. Podemos associado o controle de caixa de combinação para um conjunto de itens de estático e para uma tabela de banco de dados. Por fim, você aprendeu como modificar o comportamento de ComboBox definindo suas propriedades DropDownStyle DropDownList e AutoCompleteMode.

> [!div class="step-by-step"]
> [Avançar](how-do-i-use-the-combobox-control-vb.md)
