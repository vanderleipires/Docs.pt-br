---
uid: web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-cs
title: Como usar o controle ComboBox? (C#) | Microsoft Docs
author: microsoft
description: Caixa de combinação é um controle do ASP.NET AJAX que combina a flexibilidade de uma caixa de texto com uma lista de opções do qual os usuários podem escolher.
ms.author: aspnetcontent
ms.date: 05/12/2009
ms.assetid: 0bbf4134-04df-4226-8930-d5bb99e27128
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 07da594647c9b43bd31644aa8c7fedb256a12916
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839275"
---
<a name="how-do-i-use-the-combobox-control-c"></a>Como usar o controle ComboBox? (C#)
====================
por [Microsoft](https://github.com/microsoft)

> Caixa de combinação é um controle do ASP.NET AJAX que combina a flexibilidade de uma caixa de texto com uma lista de opções do qual os usuários podem escolher.


O objetivo deste tutorial é explicar o controle de caixa de combinação de kit de ferramentas de controle AJAX. A caixa de combinação funciona como uma combinação entre um controle DropDownList ASP.NET padrão e um controle de caixa de texto. Você pode selecionar em uma lista já existente dos itens ou insira um novo item.

A caixa de combinação é semelhante ao extensor do controle de preenchimento automático, mas os controles são usados em cenários diferentes. O extensor AutoComplete consulta um serviço web para obter entradas correspondentes. O controle ComboBox, por outro lado, é inicializado com um conjunto de itens. Usando o AutoComplete extender faz sentido quando você estiver trabalhando com um grande conjunto de dados (em milhões de partes de carro) ao usar o controle de caixa de combinação faz sentido ao trabalhar com um pequeno conjunto de dados (dezenas de partes de carro).

## <a name="selecting-from-a-static-list-of-items"></a>A seleção de uma lista estática de itens

Permitir que o s comece com um exemplo simple de usar o controle de caixa de combinação. Imagine que você deseja exibir uma lista estática de itens em uma lista suspensa. No entanto, convém deixar aberta a possibilidade de que a lista não foi concluída. Você deseja permitir que um usuário insira um valor personalizado na lista.

Podemos ll, crie uma nova página de Web Forms do ASP.NET e use o controle de caixa de combinação na página. Adicione a nova página ASP.NET ao seu projeto e alterne para modo Design.

Se você quiser usar o controle de caixa de combinação da página deve adicionar um controle ScriptManager à página. Arraste o controle ScriptManager do guia Extensões AJAX para a superfície de Designer. Você deve adicionar o controle ScriptManager na parte superior da página; Você pode adicioná-lo imediatamente abaixo do lado do servidor abrindo &lt;formulário&gt; marca.

Em seguida, arraste o controle de caixa de combinação para a página. Você pode encontrar o controle de caixa de combinação na caixa de ferramentas com outros controles do AJAX Control Toolkit e extensores de controle (consulte a Figura 1).


[![Formulário simples para a criação de um cartão de visita](how-do-i-use-the-combobox-control-cs/_static/image1.jpg)](how-do-i-use-the-combobox-control-cs/_static/image1.png)

**Figura 01**: selecionando o controle de caixa de combinação da caixa de ferramentas ([clique para exibir a imagem em tamanho normal](how-do-i-use-the-combobox-control-cs/_static/image2.png))


Podemos ll use o controle de caixa de combinação para exibir uma lista estática de opções. O usuário pode selecionar um nível específico de spiciness para seus alimentos de uma lista de três opções: leve, média e quente (veja a Figura 2).


[![A seleção de uma lista estática de itens](how-do-i-use-the-combobox-control-cs/_static/image2.jpg)](how-do-i-use-the-combobox-control-cs/_static/image3.png)

**Figura 02**: a seleção de uma lista estática de itens ([clique para exibir a imagem em tamanho normal](how-do-i-use-the-combobox-control-cs/_static/image4.png))


Há duas maneiras que você pode adicionar essas opções para o controle de caixa de combinação. Primeiro, selecione a opção de tarefa Editar opções ao focalizar o mouse no controle no modo de Design e abra o Editor de Item (veja a Figura 3).


[![Edição de itens de caixa de combinação](how-do-i-use-the-combobox-control-cs/_static/image3.jpg)](how-do-i-use-the-combobox-control-cs/_static/image5.png)

**Figura 03**: itens de caixa de combinação de edição ([clique para exibir a imagem em tamanho normal](how-do-i-use-the-combobox-control-cs/_static/image6.png))


A segunda opção é adicionar a lista de itens entre a abertura e fechamento &lt;asp: ComboBox&gt; marcas no modo de exibição de código-fonte. A página na listagem 1 contém a caixa de combinação atualizada que tem a lista de itens.

**Listagem 1 - Static.aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-cs/samples/sample1.aspx)]

Quando você abre a página na listagem 1, você pode selecionar uma das opções de pré-existentes da caixa de combinação. Em outras palavras, a caixa de combinação funciona como um controle DropDownList.

No entanto, você também tem a opção de inserir uma nova opção (por exemplo, Super Spicy) que não está na lista existente. Portanto, a caixa de combinação também funciona como um controle de caixa de texto.

Independentemente se você escolher um pré-existentes item ou inserir um item personalizado, quando você envia o formulário, sua escolha é exibida no controle de rótulo. Quando você envia o formulário, a btnSubmit\_clique manipulador executa e atualiza o rótulo (veja a Figura 4).


[![Exibindo o item selecionado](how-do-i-use-the-combobox-control-cs/_static/image4.jpg)](how-do-i-use-the-combobox-control-cs/_static/image7.png)

**Figura 04**: exibindo o item selecionado ([clique para exibir a imagem em tamanho normal](how-do-i-use-the-combobox-control-cs/_static/image8.png))


A caixa de combinação suporta as mesmas propriedades que o controle DropDownList para recuperar o item selecionado depois que um formulário é enviado:

- SelectedItem.Text - exibe o valor da propriedade Text do item selecionado.
- SelectedItem.Value - exibe o valor da propriedade Value do item selecionado ou o texto digitado na caixa de combinação.
- SelectedValue - igual SelectedItem.Value, exceto pelo fato de que essa propriedade permite que você especifique o item selecionado (inicial) padrão.

Se você digitar uma opção personalizada na caixa de combinação, em seguida, a opção personalizada é atribuída às propriedades de SelectedItem.Text e SelectedItem.Value.

## <a name="selecting-the-list-of-items-from-the-database"></a>Selecionar a lista de itens do banco de dados

Você pode recuperar a lista de itens que exibe a caixa de combinação de um banco de dados. Por exemplo, você pode associar a caixa de combinação a um controle SqlDataSource, um controle ObjectDataSource, um LinqDataSource ou um EntityDataSource.

Imagine que você deseja exibir uma lista de filmes em uma caixa de combinação. Você deseja recuperar a lista de filmes da tabela de banco de dados de filmes. Siga estas etapas:

1. Crie uma página chamada Movies.aspx
2. Adicione um controle ScriptManager à página, arrastando o ScriptManager de guia Extensões AJAX na caixa de ferramentas para a página.
3. Adicione um controle ComboBox para a página, arrastando a caixa de combinação para a página.
4. No modo Design, passe o mouse sobre o controle de caixa de combinação e selecione a **Choose Data Source** opção de tarefa (consulte a Figura 5). O Assistente de configuração de fonte de dados é iniciado.
5. No **escolher uma fonte de dados** etapa, selecione o &lt;nova fonte de dados&gt; opção.
6. No **escolher um tipo de fonte de dados** etapa, selecione o banco de dados.
7. No **escolha sua Conexão de dados** etapa, selecione seu banco de dados (por exemplo, MoviesDB.mdf).
8. No **salvar a cadeia de Conexão no arquivo de configuração de aplicativo** etapa, selecione a opção para salvar a cadeia de conexão.
9. No **configurar a instrução Select** etapa, selecione a tabela de banco de dados de filmes e selecione todas as colunas.
10. No **consulta de teste** etapa, clique no botão Concluir.
11. Volta a **Choose Data Source** etapa, selecione a coluna de título para o campo para exibir e a coluna de Id para os dados de campo (veja a figura).
12. Clique no botão Okey para fechar o assistente.


[![Escolher uma fonte de dados](how-do-i-use-the-combobox-control-cs/_static/image5.jpg)](how-do-i-use-the-combobox-control-cs/_static/image9.png)

**Figura 05**: escolha uma fonte de dados ([clique para exibir a imagem em tamanho normal](how-do-i-use-the-combobox-control-cs/_static/image10.png))


[![Escolha os campos de texto e o valor de dados](how-do-i-use-the-combobox-control-cs/_static/image6.jpg)](how-do-i-use-the-combobox-control-cs/_static/image11.png)

**Figura 06**: escolhendo os campos de texto e o valor de dados ([clique para exibir a imagem em tamanho normal](how-do-i-use-the-combobox-control-cs/_static/image12.png))


Depois de concluir as etapas acima, a caixa de combinação está associada a um controle SqlDataSource que representa os filmes da tabela de banco de dados de filmes. O código-fonte para a página semelhante à listagem 2 (eu limpa a formatação de um pouco).

**Listagem 2 - Movies.aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-cs/samples/sample2.aspx)]

Observe que o controle de caixa de combinação tem uma propriedade DataSourceID que aponta para o controle SqlDataSource. Quando você abre a página em um navegador, a lista de filmes do banco de dados é exibida (veja a Figura 7). É possível que uma escolha um filme na lista ou digite um novo filme digitando o filme na caixa de combinação.


[![Exibindo uma lista de filmes](how-do-i-use-the-combobox-control-cs/_static/image7.jpg)](how-do-i-use-the-combobox-control-cs/_static/image13.png)

**Figura 07**: exibindo uma lista de filmes ([clique para exibir a imagem em tamanho normal](how-do-i-use-the-combobox-control-cs/_static/image14.png))


## <a name="setting-the-dropdownstyle"></a>Definindo o DropDownStyle

Você pode usar a propriedade DropDownStyle do ComboBox para alterar o comportamento da caixa de combinação. Essa propriedade existe aceita os valores possíveis:

- Caixa suspensa – exibe a caixa de combinação de (valor padrão) uma lista suspensa lista quando você clicar na seta e você pode inserir um valor personalizado.
- Simples - caixa de combinação exibe uma lista suspensa automaticamente e você pode inserir um valor personalizado.
- DropDownList - caixa de combinação funciona como um controle DropDownList.

A diferença entre a lista suspensa e simples é quando a lista de itens é exibida. No caso simples, a lista será exibida imediatamente quando você move o foco para a caixa de combinação. No caso do menu suspenso, você deve clicar na seta para ver a lista de itens.

O valor de DropDownList faz com que o controle de caixa de combinação para que funcionem como um controle DropDownList padrão. No entanto, há uma diferença importante aqui. Versões mais antigas do Internet Explorer exibem um controle DropDownList com o z-index infinito para que o controle será exibido na frente de qualquer controle colocado na frente dele. Porque a caixa de combinação renderiza uma marca HTML &lt;div&gt; marca em vez de um HTML &lt;selecione&gt; marca, a caixa de combinação corretamente respeita a ordenação z.

## <a name="setting-the-autocompletemode"></a>Definindo o AutoCompleteMode

Você pode usar a propriedade ComboBox AutoCompleteMode para especificar o que acontece quando alguém digita texto na caixa de combinação. Essa propriedade aceita os seguintes valores possíveis:

- Nenhum - (valor padrão) a caixa de combinação não fornece qualquer comportamento de preenchimento automático.
- Sugerir - caixa de combinação exibe a lista e realça o item correspondente na lista (veja a Figura 8).
- Acrescente - a caixa de combinação não exibe a lista e ele acrescenta o item correspondente na lista para o que você digitou (veja a Figura 9).
- SuggestAppend - caixa de combinação exibe a lista e acrescenta o item correspondente na lista para o que você digitou (veja a Figura 10).


[![A caixa de combinação torna uma sugestão](how-do-i-use-the-combobox-control-cs/_static/image8.jpg)](how-do-i-use-the-combobox-control-cs/_static/image15.png)

**Figura 08**: A caixa de combinação torna uma sugestão ([clique para exibir a imagem em tamanho normal](how-do-i-use-the-combobox-control-cs/_static/image16.png))


[![Caixa de combinação acrescenta o texto correspondente](how-do-i-use-the-combobox-control-cs/_static/image9.jpg)](how-do-i-use-the-combobox-control-cs/_static/image17.png)

**Figura 09**: caixa de combinação acrescenta o texto correspondente ([clique para exibir a imagem em tamanho normal](how-do-i-use-the-combobox-control-cs/_static/image18.png))


[![A caixa de combinação sugere e acrescenta](how-do-i-use-the-combobox-control-cs/_static/image10.jpg)](how-do-i-use-the-combobox-control-cs/_static/image19.png)

**Figura 10**: A caixa de combinação sugere e acrescenta ([clique para exibir a imagem em tamanho normal](how-do-i-use-the-combobox-control-cs/_static/image20.png))


## <a name="summary"></a>Resumo

Neste tutorial, você aprendeu como usar o controle de caixa de combinação para exibir um conjunto fixo de itens. Podemos ligar o controle de caixa de combinação tanto para um conjunto de itens de estático e uma tabela de banco de dados. Por fim, você aprendeu como modificar o comportamento da caixa de combinação definindo suas propriedades DropDownStyle e AutoCompleteMode.

> [!div class="step-by-step"]
> [Avançar](how-do-i-use-the-combobox-control-vb.md)
