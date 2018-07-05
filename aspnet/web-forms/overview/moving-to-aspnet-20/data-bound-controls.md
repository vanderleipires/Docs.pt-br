---
uid: web-forms/overview/moving-to-aspnet-20/data-bound-controls
title: Controles ligados a dados | Microsoft Docs
author: microsoft
description: A maioria dos aplicativos do ASP.NET dependem de um certo grau de apresentação de dados de uma fonte de dados back-end. Controles ligados a dados tem sido uma parte crítica da implantação de interação de w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 0e23ff32-646d-43f3-8bec-6b2313d3abd6
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-bound-controls
msc.type: authoredcontent
ms.openlocfilehash: adaf8a40c1877db4181e1b1c7a74a2ecbaa373ad
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368253"
---
<a name="data-bound-controls"></a>Controles ligados a dados
====================
por [Microsoft](https://github.com/microsoft)

> A maioria dos aplicativos do ASP.NET dependem de um certo grau de apresentação de dados de uma fonte de dados back-end. Controles ligados a dados tem sido uma parte crítica da implantação de interagir com dados em aplicativos Web dinâmicos. O ASP.NET 2.0 introduz algumas melhorias substanciais para controles ligados a dados, incluindo uma nova classe BaseDataBoundControl e a sintaxe declarativa.


A maioria dos aplicativos do ASP.NET dependem de um certo grau de apresentação de dados de uma fonte de dados back-end. Controles ligados a dados tem sido uma parte crítica da implantação de interagir com dados em aplicativos Web dinâmicos. O ASP.NET 2.0 introduz algumas melhorias substanciais para controles ligados a dados, incluindo uma nova classe BaseDataBoundControl e a sintaxe declarativa.

O BaseDataBoundControl atua como a classe base para a classe de DataBoundControl e a classe HierarchicalDataBoundControl. Neste módulo, abordaremos as seguintes classes que derivam de DataBoundControl:

- AdRotator
- Controles de lista
- GridView
- FormView
- DetailsView

Nós também abordaremos as seguintes classes que derivam da classe HierarchicalDataBoundControl:

- TreeView
- Menu
- SiteMapPath

## <a name="databoundcontrol-class"></a>Classe de DataBoundControl

A classe DataBoundControl é uma classe abstrata (marcada MustInherit no VB) usada para interagir com a tabela ou dados de estilo de lista. Os controles a seguir estão alguns dos controles que derivam de DataBoundControl.

## <a name="adrotator"></a>AdRotator

O controle AdRotator permite exibir uma faixa de gráfico em uma página da Web que esteja vinculada a uma URL específica. O elemento gráfico que é exibido é girado usando as propriedades para o controle. A frequência de uma exibição de ad específico em uma página pode ser configurada usando o **impressões** anúncios e propriedade podem ser filtrados usando a filtragem de palavra-chave.

Controles de AdRotator usam um arquivo XML ou uma tabela em um banco de dados para os dados. Os seguintes atributos são usados em arquivos XML para configurar o controle AdRotator.

### <a name="imageurl"></a>ImageUrl
A URL de uma imagem a ser exibida para o ad.

### <a name="navigateurl"></a>NavigateUrl
A URL que o usuário deve ser levado para quando o anúncio é clicado. Isso deve ser codificado por URL.

### <a name="alternatetext"></a>AlternateText
O texto alternativo que é exibido em uma dica de ferramenta e lido por leitores de tela. Também é exibida quando a imagem especificada pela ImageUrl não está disponível.

### <a name="keyword"></a>Palavra-chave
Define uma palavra-chave que pode ser usada ao usar a filtragem de palavra-chave. Se for especificado, somente esses anúncios com uma palavra-chave que corresponde ao filtro de palavra-chave serão exibidos.

### <a name="impressions"></a>Impressões
Um número de ponderação que determina a frequência com que um anúncio específico é provável que apareça. Ele é relativo à impressão de outros anúncios no mesmo arquivo. O valor máximo das impressões coletivos para todos os anúncios em um arquivo XML é 2,048,000,000 1.

### <a name="height"></a>Altura
A altura do anúncio em pixels.

### <a name="width"></a>Largura
A largura do anúncio em pixels.


> [!NOTE]
> Os atributos de altura e largura substituem a altura e largura para o controle AdRotator em si.


Um típico arquivo XML pode parecer com o seguinte:

[!code-xml[Main](data-bound-controls/samples/sample1.xml)]

No exemplo acima, o ad da Contoso é duas vezes mais provável de aparecer como o ad para o site da Web do ASP.NET devido ao valor do atributo de impressões.

Para exibir anúncios do arquivo XML acima, adicione um controle AdRotator a uma página e defina as **AdvertisementFile** propriedade para apontar para o arquivo XML, conforme mostrado abaixo:

[!code-aspx[Main](data-bound-controls/samples/sample2.aspx)]

Se você optar por usar uma tabela de banco de dados como a fonte de dados para o seu controle AdRotator, primeiro você precisará configurar um banco de dados usando o esquema a seguir:

| **Nome da coluna** | **Tipo de dados** | **Descrição** |
| --- | --- | --- |
| ID | int | Chave primária. Esta coluna pode ter qualquer nome. |
| ImageUrl | nvarchar(*length*) | A URL relativa ou absoluta da imagem a ser exibida para o ad. |
| NavigateUrl | nvarchar(*length*) | A URL de destino para o ad. Se você não fornecer um valor, o ad não é um hiperlink. |
| AlternateText | nvarchar(*length*) | O texto exibido se a imagem não pode ser encontrada. Em alguns navegadores, o texto é exibido como uma dica de ferramenta. Texto alternativo também é usado para acessibilidade para que os usuários que não é possível ver o gráfico podem ouvir sua descrição ler em voz alta. |
| Palavra-chave | nvarchar(*length*) | Uma categoria para o anúncio no qual a página pode filtrar. |
| Impressões | Int(4) | Um número que indica a probabilidade da frequência com que o anúncio é exibido. Quanto maior o número, mais frequentemente o anúncio será exibido. O total de todos os valores de impressões no arquivo XML não pode exceder 2.048.000.000-1. |
| Largura | Int(4) | A largura da imagem em pixels. |
| Altura | Int(4) | A altura da imagem em pixels. |

Em casos em que você já tiver um banco de dados com um esquema diferente, você pode usar o **AlternateTextField**, **ImageUrlField**, e **NavigateUrlField** propriedades para mapear o Atributos de AdRotator para seu banco de dados existente. Para exibir os dados do banco de dados no controle AdRotator, adicione um controle de fonte de dados para a página Configurar a cadeia de conexão para o controle de fonte de dados apontar para seu banco de dados e defina o controle de AdRotator **DataSourceID** propriedade para a ID do controle de fonte de dados. Em casos onde você tem uma necessidade de configurar AdRotator anúncios programaticamente, usam o evento AdCreated. O evento AdCreated usa dois parâmetros; um é um objeto e o outro uma instância de AdCreatedEventArgs. O AdCreatedEventArgs é uma referência para o ad que está sendo criado.

O trecho de código a seguir define a ImageUrl, NavigateUrl e AlternateText para um anúncio por meio de programação:

[!code-csharp[Main](data-bound-controls/samples/sample3.cs)]

## <a name="list-controls"></a>Controles de lista

Controles de lista incluem o ListBox, DropDownList, CheckBoxList, RadioButtonList e BulletedList. Cada um desses controles pode ser associada a dados para uma fonte de dados. Eles usam um campo na fonte de dados como texto de exibição e, opcionalmente, podem usar um segundo campo como o valor de um item. Itens também podem ser adicionados estaticamente no tempo de design, e você pode combinar itens estáticos e dinâmicos itens adicionados de uma fonte de dados.

Para dados de associar um controle de lista, adicione um controle de fonte de dados para a página. Especifique um comando SELECT para o controle de fonte de dados e, em seguida, defina a propriedade DataSourceID do controle de lista para a ID do controle de fonte de dados. Use o **DataTextField** e **DataValueField** propriedades para definir o texto de exibição e o valor para o controle. Além disso, você pode usar o **DataTextFormatString** propriedade para controlar a aparência do texto da seguinte maneira:

| **Expressão** | **Descrição** |
| --- | --- |
| Preço: {0:C} | Para dados numéricos/decimal. Exibe o literal "preço:" seguido de números em formato de moeda. O formato de moeda depende da configuração de cultura especificada no atributo de cultura sobre o **página** diretiva ou no arquivo Web. config. |
| {0:D4} | Para dados de inteiro. Não pode ser usado com números decimais. Inteiros são exibidos em um campo sem preenchimento que é de quatro caracteres largos. |
| {0:N2}% | Para dados numéricos. Exibe o número da casa decimal 2 precisão seguido pelo literal "%". |
| {0:000.0} | Para dados numéricos/decimal. Números são arredondados para uma casa decimal. Os números com menos de três dígitos zero são preenchidos com zeros. |
| {0:D} | Para dados de data/hora. Formato de data por extenso de exibições ("quinta-feira, 06 de agosto, de 1996"). O formato de data depende da configuração da cultura da página ou do arquivo Web.config. |
| {0:d} | Para dados de data/hora. Formato de data abreviada do exibe ("12/31/99"). |
| {0:yy-MM-dd} | Para dados de data/hora. Exibe a data no formato de ano-mês-dia numérico (96-08-06) |

## <a name="gridview"></a>GridView

O controle GridView permite edição usando uma abordagem declarativa e a exibição de dados de tabela e é o sucessor do controle DataGrid. Os seguintes recursos estão disponíveis no controle GridView.

- Associando dados a controles de fonte, como SqlDataSource.
- Recursos internos de classificação.
- Atualizando e excluir recursos internos.
- Recursos de paginação internos.
- Recursos de seleção de linha interna.
- Acesso programático ao modelo de objeto GridView para definir dinamicamente a propriedades, lidar com eventos e assim por diante.
- Vários campos de chave.
- Vários campos de dados para as colunas de hiperlink.
- Aparência personalizável por meio de estilos e temas.

**Campos de coluna**

Cada coluna no controle GridView é representada por um objeto DataControlField. Por padrão, a propriedade AutoGenerateColumns for definida como **verdadeira**, que cria um objeto AutoGeneratedField para cada campo na fonte de dados. Cada campo é renderizado como uma coluna no controle GridView na ordem em que cada campo é exibido na fonte de dados. Manualmente, você pode controlar qual coluna campos são exibidos na **GridView** controle definindo o **AutoGenerateColumns** propriedade a ser **false** e, em seguida, definindo seu próprio coleção de campos de coluna. Tipos de campo de coluna diferente determinam o comportamento das colunas no controle.

A tabela a seguir lista os tipos de campo de coluna diferente que podem ser usados.

| **Tipo de campo de coluna** | **Descrição** |
| --- | --- |
| BoundField | Exibe o valor de um campo em uma fonte de dados. Isso é o tipo de coluna padrão do controle GridView. |
| ButtonField | Exibe um botão de comando para cada item no controle GridView. Isso permite que você crie uma coluna de controles de botão personalizado, como adicionar ou remover. |
| CheckBoxField | Exibe uma caixa de seleção para cada item no controle GridView. Esse tipo de campo de coluna é comumente usado para exibir campos com um valor booliano. |
| CommandField | Exibe predefinidos botões de comando para executar a seleção, editando ou excluindo as operações. |
| HyperLinkField | Exibe o valor de um campo em uma fonte de dados como um hiperlink. Esse tipo de campo de coluna permite que você associe um segundo campo a URL do hiperlink. |
| ImageField | Exibe uma imagem para cada item no controle GridView. |
| TemplateField | Exibe conteúdo definido pelo usuário para cada item no controle GridView acordo com um modelo especificado. Esse tipo de campo de coluna permite que você crie um campo de coluna personalizado. |

Para definir uma coleção de campos de coluna de forma declarativa, primeiro adicione abrindo e fechando **&lt;colunas&gt;** marcas entre as marcas de abertura e fechamento do controle GridView. Em seguida, liste os campos de coluna que você deseja incluir entre a abertura e fechamento **&lt;colunas&gt;** marcas. As colunas especificadas são adicionadas à coleção de colunas na ordem listada. O **colunas** collection armazena todos os campos no controle da coluna e lhe permite gerenciar programaticamente os campos de coluna no controle GridView.

Campos de coluna explicitamente declarado podem ser exibidos em combinação com os campos de coluna gerado automaticamente. Quando ambos são usados, os campos de coluna explicitamente declarado são renderizados em primeiro lugar, seguido pelos campos de coluna gerado automaticamente.

## <a name="binding-to-data"></a>Associando a dados

O controle GridView pode ser associado a um controle de fonte de dados (como **SqlDataSource**, **ObjectDataSource**e assim por diante), assim como qualquer fonte de dados que implementa a IEnumerable interface (como System.Data.DataView, ArrayList ou Hashtable). Use um dos seguintes métodos para associar o controle GridView para o tipo de fonte de dados apropriado:

- Para associar a um controle de fonte de dados, defina a propriedade DataSourceID do controle GridView para o valor da ID do controle de fonte de dados. O controle GridView automaticamente associa para o controle de fonte de dados especificado e pode aproveitar a fonte de dados recursos do controle para realizar a classificação, atualizar, excluir e funcionalidade de paginação. Esse é o método preferencial para associar aos dados.
- Para associar a uma fonte de dados que implementa a interface IEnumerable, definir programaticamente a propriedade DataSource do controle GridView à fonte de dados e, em seguida, chama o método DataBind. Ao usar esse método, o controle GridView não fornece classificação, atualizar, excluir e funcionalidade de paginação internos. Você precisa fornecer essa funcionalidade por conta própria.

## <a name="data-operations"></a>Operações de dados

O controle GridView oferece muitos recursos internos que permitem ao usuário classificar, atualizar, excluir, selecione e página por meio de itens no controle. Quando o controle GridView é vinculado a um controle de fonte de dados, o controle GridView pode tirar proveito da fonte de dados de recursos do controle e fornecer a classificação automática, atualizar e excluir a funcionalidade.

> [!NOTE]
> O controle GridView pode fornecer suporte para classificação, atualizando e excluindo-se com outros tipos de fontes de dados; No entanto, você precisará fornecer um manipulador de eventos apropriado com a implementação para essas operações.


Classificação permite que o usuário classificar os itens no controle GridView em relação a uma coluna específica, clicando no cabeçalho da coluna. Para habilitar a classificação, defina a propriedade de AllowSorting como **verdadeira**.

As funcionalidades de atualização, exclusão e seleção automática estão habilitadas quando um botão em um **ButtonField** ou **TemplateField** campo de coluna, com um nome de comando de "Editar", "Delete" e "Select", respectivamente, é clicado. O controle GridView pode adicionar automaticamente um **CommandField** campo de coluna com um editar, excluir ou botão de seleção se a propriedade AutoGenerateEditButton, AutoGenerateDeleteButton ou AutoGenerateSelectButton for definida como **verdadeiro**, respectivamente.

> [!NOTE]
> Não há suporte diretamente ao inserir registros na fonte de dados pelo controle GridView. No entanto, é possível inserir os registros usando o controle GridView em conjunto com DetailsView ou FormView controle.


Em vez de exibir todos os registros na fonte de dados ao mesmo tempo, o controle GridView pode automaticamente divida os registros em páginas. Para habilitar a paginação, defina a propriedade de AllowPaging como **verdadeira**.

## <a name="customizing-the-user-interface"></a>Personalizando a interface do usuário

Você pode personalizar a aparência do controle GridView, definindo as propriedades de estilo para as diferentes partes do controle. A tabela a seguir lista as propriedades de estilo diferente.

| **Propriedade de estilo** | **Descrição** |
| --- | --- |
| AlternatingRowStyle | As configurações de estilo para as linhas de dados alternados no controle GridView. Quando essa propriedade é definida, as linhas de dados são exibidas alternando entre as configurações de RowStyle e o **AlternatingRowStyle** configurações. |
| EditRowStyle | As configurações de estilo para a linha que está sendo editado no controle GridView. |
| EmptyDataRowStyle | As configurações de estilo para a linha de dados vazia exibida no controle GridView quando a fonte de dados não contém nenhum registro. |
| FooterStyle | As configurações de estilo para a linha de rodapé do controle GridView. |
| HeaderStyle | As configurações de estilo para a linha de cabeçalho do controle GridView. |
| PagerStyle | As configurações de estilo da linha do pager do controle GridView. |
| RowStyle | As configurações de estilo para as linhas de dados no controle GridView. Quando o **AlternatingRowStyle** propriedade também for definida, as linhas de dados são exibidas, alternando entre os **RowStyle** configurações e o **AlternatingRowStyle** configurações. |
| SelectedRowStyle | As configurações de estilo para a linha selecionada no controle GridView. |

Você também pode mostrar ou ocultar partes diferentes do controle. A tabela a seguir lista as propriedades que controlam quais partes são mostradas ou ocultas.

| **Property** | **Descrição** |
| --- | --- |
| ShowFooter | Mostra ou oculta a seção de rodapé do controle GridView. |
| ShowHeader | Mostra ou oculta a seção de cabeçalho do controle GridView. |

### <a name="events"></a>Eventos

O controle GridView fornece vários eventos que você pode programar. Isso permite que você execute uma rotina personalizada sempre que um evento ocorre. A tabela a seguir lista os eventos com suporte pelo controle GridView.

| **Event** | **Descrição** |
| --- | --- |
| PageIndexChanged | Ocorre quando um dos botões de paginação é clicado, mas após o controle GridView manipula a operação de paginação. Esse evento normalmente é usado quando você precisa executar uma tarefa depois que o usuário navega para uma página diferente no controle. |
| PageIndexChanging | Ocorre quando um dos botões de paginação é acionado, mas antes do GridView controle manipula a operação de paginação. Esse evento geralmente é usado para cancelar a operação de paginação. |
| RowCancelingEdit | Ocorre quando o botão de cancelamento de uma linha é clicado, mas antes do controle GridView sai do modo de edição. Esse evento é geralmente usado para interromper a operação de cancelamento. |
| RowCommand | Ocorre quando um botão é clicado no controle GridView. Esse evento geralmente é usado para executar uma tarefa quando um botão é clicado no controle. |
| RowCreated | Ocorre quando uma nova linha é criada no controle GridView. Esse evento geralmente é usado para modificar o conteúdo de uma linha quando a linha é criada. |
| RowDataBound | Ocorre quando uma linha de dados está associada aos dados no controle GridView. Esse evento geralmente é usado para modificar o conteúdo de uma linha quando a linha está associada aos dados. |
| RowDeleted | Ocorre quando o botão de exclusão de uma linha é clicado, mas após o controle GridView exclui o registro da fonte de dados. Esse evento é geralmente usado para verificar os resultados da operação de exclusão. |
| RowDeleting | Ocorre quando o botão de exclusão de uma linha é clicado, mas antes do GridView controle exclui o registro da fonte de dados. Esse evento geralmente é usado para cancelar a operação de exclusão. |
| RowEditing | Ocorre quando o botão de edição de uma linha é clicado, mas antes do GridView controle entra em modo de edição. Esse evento geralmente é usado para cancelar a operação de edição. |
| RowUpdated | Ocorre quando um botão Atualizar linha é clicado, mas após o controle GridView atualiza a linha. Esse evento é geralmente usado para verificar os resultados da operação de atualização. |
| RowUpdating | Ocorre quando um botão Atualizar linha é clicado, mas antes do GridView controle atualiza a linha. Esse evento geralmente é usado para cancelar a operação de atualização. |
| SelectedIndexChanged | Ocorre quando o botão de seleção de uma linha é clicado, mas após o controle GridView manipula a operação de seleção. Esse evento é geralmente usado para executar uma tarefa depois que uma linha está selecionada no controle. |
| SelectedIndexChanging | Ocorre quando o botão de seleção de uma linha é clicado, mas antes do GridView controle manipula a operação de seleção. Esse evento geralmente é usado para cancelar a operação de seleção. |
| Classificados | Ocorre quando o hiperlink para classificar uma coluna é clicado, mas após o controle GridView manipula a operação de classificação. Esse evento normalmente é usado para executar uma tarefa depois que o usuário clica em um hiperlink para classificar uma coluna. |
| Classificação | Ocorre quando o hiperlink para classificar uma coluna é clicado, mas antes do GridView controle manipula a operação de classificação. Esse evento é geralmente usado para cancelar a operação de classificação ou para executar uma rotina de classificação personalizada. |

## <a name="formview"></a>FormView

O controle FormView é usado para exibir um registro individual de uma fonte de dados. É semelhante ao controle DetailsView, exceto que ele exibe os modelos definidos pelo usuário em vez de campos de linha. Criando seus próprios modelos lhe dá maior flexibilidade para controlar como os dados são exibidos. O controle FormView suporta os seguintes recursos:

- Associando dados a controles de fonte, como SqlDataSource e ObjectDataSource.
- Recursos internos de inserção.
- Atualizando e excluir recursos internos.
- Recursos de paginação internos.
- Acesso programático ao modelo de objeto FormView para definir dinamicamente a propriedades, lidar com eventos e assim por diante.
- Aparência personalizável por meio de modelos definidos pelo usuário, temas e estilos.

## <a name="templates"></a>Modelos

Para o controle FormView exibir o conteúdo, você precisa criar modelos para as diferentes partes do controle. A maioria dos modelos são opcionais. No entanto, você deve criar um modelo para o modo no qual o controle está configurado. Por exemplo, um controle FormView que dá suporte à inserção de registros deve ter um modelo de item de inserção definido. A tabela a seguir lista os modelos diferentes que você pode criar.

| **Tipo de modelo** | **Descrição** |
| --- | --- |
| EditItemTemplate | Define o conteúdo da linha de dados quando o controle FormView está no modo de edição. Este modelo geralmente contém controles de entrada e botões de comando com a qual o usuário pode editar um registro existente. |
| EmptyDataTemplate | Define o conteúdo para a linha de dados vazia exibida quando o controle FormView é associado a uma fonte de dados que não contém nenhum registro. Este modelo geralmente contém conteúdo para alertar o usuário que a fonte de dados não contém nenhum registro. |
| FooterTemplate | Define o conteúdo para a linha de rodapé. Este modelo geralmente contém qualquer conteúdo adicional que você deseja exibir na linha de rodapé. Como alternativa, você pode simplesmente especificar o texto a ser exibido na linha de rodapé, definindo a propriedade FooterText. |
| HeaderTemplate | Define o conteúdo para a linha de cabeçalho. Este modelo geralmente contém qualquer conteúdo adicional que você deseja exibir na linha de cabeçalho. Como alternativa, você pode simplesmente especificar o texto a ser exibido na linha de cabeçalho, definindo a propriedade HeaderText. |
| ItemTemplate | Define o conteúdo da linha de dados quando o controle FormView está no modo somente leitura. Este modelo geralmente contém conteúdo para exibir os valores de um registro existente. |
| InsertItemTemplate | Define o conteúdo da linha de dados quando o controle FormView está no modo de inserção. Este modelo geralmente contém controles de entrada e botões de comando com a qual o usuário pode adicionar um novo registro. |
| PagerTemplate | Define o conteúdo da linha do pager exibida quando o recurso de paginação está habilitado (quando a propriedade AllowPaging é definida como **verdadeira**). Este modelo geralmente contém controles com os quais o usuário pode navegar para outro registro. |

Controles de entrada no modelo de item de inserção e editar o modelo de item podem ser vinculados aos campos de uma fonte de dados usando uma expressão de associação bidirecional. Isso permite que o controle FormView extrair os valores de controle de entrada para uma atualização ou operação de inserção automaticamente. Expressões de associação bidirecional também permitem que os controles de entrada em uma edição de modelo de item para exibir automaticamente os valores de campo original.

### <a name="binding-to-data"></a>Associando a dados

O controle FormView pode ser associado a um controle de fonte de dados (como **SqlDataSource**, AccessDataSource, **ObjectDataSource** e assim por diante), ou para qualquer fonte de dados que implementa o Interface IEnumerable (como System.Data.DataView ArrayList e Hashtable). Use um dos seguintes métodos para associar o controle FormView para o tipo de fonte de dados apropriado:

- Para associar a um controle de fonte de dados, defina a propriedade DataSourceID do controle FormView para o valor da ID do controle de fonte de dados. O controle FormView automaticamente associa para o controle de fonte de dados especificado e pode aproveitar a fonte de dados recursos do controle para executar a inserção, atualização, exclusão e funcionalidade de paginação. Esse é o método preferencial para associar aos dados.
- Para associar a uma fonte de dados que implementa o **IEnumerable** de interface, definir programaticamente a propriedade DataSource do controle FormView para a fonte de dados e, em seguida, chama o método DataBind. Ao usar esse método, o controle FormView não fornece internos inserindo, atualizando, excluindo e funcionalidade de paginação. Você precisa fornecer essa funcionalidade usando o evento apropriado.

## <a name="data-operations"></a>Operações de dados

O controle FormView oferece muitos recursos internos que permitem ao usuário atualizar, excluir, inserir e página por meio de itens no controle. Quando o controle FormView é associado a um controle de fonte de dados, o controle FormView pode tirar proveito da fonte de dados de recursos do controle e fornecer a atualizar, excluir, inserir e funcionalidade de paginação automática. O controle FormView pode fornecer suporte para atualização, exclusão, inserção e operações de paginação com outros tipos de fontes de dados; No entanto, você deve fornecer um manipulador de eventos apropriado com a implementação para essas operações.

Como o controle FormView usa modelos, ele não fornece uma maneira de gerar automaticamente os botões de comando para executar a atualização, exclusão ou operações de inserção. Você deverá incluir manualmente esses botões de comando em que o modelo apropriado. O controle FormView reconhece determinados botões que têm suas **CommandName** propriedades definidas como valores específicos. A tabela a seguir lista os botões de comando que reconhece o controle FormView.

| **Button** | **Valor CommandName** | **Descrição** |
| --- | --- | --- |
| Cancelar | "Cancelar" | Usado na atualização ou inserção de operações para cancelar a operação e descartar os valores inseridos pelo usuário. Em seguida, retorna o controle FormView para o modo especificado pela propriedade DefaultMode. |
| Excluir | "Excluir" | Usada em operações de exclusão para excluir o registro exibido da fonte de dados. Gera os eventos ItemDeleting e ItemDeleted. |
| Editar | "Editar" | Usada em operações de atualização para colocar o controle FormView no modo de edição. O conteúdo especificado na **EditItemTemplate** propriedade é exibida para a linha de dados. |
| Inserir | "Insert" | Usado em operações de inserção para tentar inserir um novo registro na fonte de dados usando os valores fornecidos pelo usuário. Gera os eventos ItemInserting e ItemInserted. |
| Novo | "Novo" | Usada em operações de inserção para colocar o controle FormView no modo de inserção. O conteúdo especificado na **InsertItemTemplate** propriedade é exibida para a linha de dados. |
| Página | "Página" | Usado em operações de paginação para representar um botão na linha de pager que executa a paginação. Para especificar a operação de paginação, defina as **CommandArgument** propriedade do botão "Avançar", "Prev", "Primeiro", "Último" ou o índice da página para o qual navegar. Gera os eventos PageIndexChanging e PageIndexChanged. |
| Atualização | "Atualização" | Usado em operações de atualização para tentar atualizar o registro exibido na fonte de dados com os valores fornecidos pelo usuário. Gera os eventos ItemUpdating e ItemUpdated. |

Ao contrário da exclusão de botão (o que exclui o registro exibido imediatamente), quando o botão Edit ou New é clicado, o controle passa para a edição de FormView ou modo de inserção, respectivamente. No modo de edição, o conteúdo contido na **EditItemTemplate** propriedade é exibida para o item de dados atual. Normalmente, o modelo de item de edição é definido, de modo que o botão Editar é substituído por uma atualização e um botão Cancelar. Controles de entrada que são apropriados para o tipo de dados do campo (como uma caixa de texto ou um controle de caixa de seleção) também geralmente são exibidos com um valor de campo para o usuário modificar. Clicar no botão de atualização atualiza o registro na fonte de dados, enquanto clicando no botão Cancelar abandona quaisquer alterações.

Da mesma forma, o conteúdo contido a **InsertItemTemplate** propriedade é exibida para o item de dados quando o controle está no modo de inserção. O modelo de item de inserção é normalmente definido, de modo que o novo botão é substituído por uma instrução Insert e um botão Cancelar, e controles de entrada vazios são exibidos para o usuário insira os valores para o novo registro. Clicar no botão Insert insere o registro na fonte de dados, enquanto clicando no botão Cancelar abandona quaisquer alterações.

O controle FormView fornece um recurso de paginação, que permite ao usuário navegar para outros registros na fonte de dados. Quando habilitada, uma linha do pager é exibida no controle FormView que contém os controles de navegação de página. Para habilitar a paginação, defina as **AllowPaging** propriedade **verdadeiro**. Você pode personalizar a linha de paginação, definindo as propriedades dos objetos contidos no PagerStyle e a propriedade PagerSettings. Em vez de usar a linha de paginação internos da interface do usuário, você pode criar sua própria interface do usuário usando o **PagerTemplate** propriedade.

## <a name="customizing-the-user-interface"></a>Personalizando a interface do usuário

Você pode personalizar a aparência do controle FormView definindo as propriedades de estilo para as diferentes partes do controle. A tabela a seguir lista as propriedades de estilo diferente.

| **Propriedade de estilo** | **Descrição** |
| --- | --- |
| EditRowStyle | As configurações de estilo da linha de dados quando o controle FormView está no modo de edição. |
| EmptyDataRowStyle | As configurações de estilo para a linha de dados vazia exibida no controle FormView quando a fonte de dados não contém nenhum registro. |
| FooterStyle | As configurações de estilo para a linha de rodapé do controle FormView. |
| HeaderStyle | As configurações de estilo para a linha de cabeçalho do controle FormView. |
| InsertRowStyle | As configurações de estilo da linha de dados quando o controle FormView está no modo de inserção. |
| PagerStyle | As configurações de estilo para a linha de pager exibida no controle FormView quando o recurso de paginação está habilitado. |
| RowStyle | As configurações de estilo da linha de dados quando o controle FormView está no modo somente leitura. |

## <a name="events"></a>Eventos

O controle FormView fornece vários eventos que você pode programar. Isso permite que você execute uma rotina personalizada sempre que um evento ocorre. A tabela a seguir lista os eventos compatíveis com o controle FormView.

| **Event** | **Descrição** |
| --- | --- |
| ItemCommand | Ocorre quando um botão em um controle FormView é clicado. Esse evento geralmente é usado para executar uma tarefa quando um botão é clicado no controle. |
| ItemCreated | Ocorre depois que todos os objetos FormViewRow são criados no controle FormView. Esse evento é geralmente usado para modificar os valores de um registro antes de ser exibido. |
| ItemDeleted | Ocorre quando um botão Delete (um botão com sua **CommandName** propriedade definida como "Excluir") é clicado, mas após o controle FormView exclui o registro da fonte de dados. Esse evento é geralmente usado para verificar os resultados da operação de exclusão. |
| ItemDeleting | Ocorre quando um botão Excluir é clicado, mas antes de FormView controle exclui o registro da fonte de dados. Esse evento geralmente é usado para cancelar a operação de exclusão. |
| ItemInserted | Ocorre quando um botão de inserção (um botão com sua **CommandName** propriedade definida como "Insert") é clicado, mas após o controle FormView insere o registro. Esse evento é geralmente usado para verificar os resultados da operação de inserção. |
| ItemInserting | Ocorre quando um botão de inserção é clicado, mas antes de FormView controle insere o registro. Esse evento geralmente é usado para cancelar a operação de inserção. |
| ItemUpdated | Ocorre quando um botão de atualização (um botão com sua **CommandName** propriedade definida como "Update") é clicado, mas após o controle FormView atualiza a linha. Esse evento é geralmente usado para verificar os resultados da operação de atualização. |
| ItemUpdating | Ocorre quando um botão de atualização é clicado, mas antes de FormView controle atualiza o registro. Esse evento geralmente é usado para cancelar a operação de atualização. |
| ModeChanged | Ocorre depois que o controle FormView altera modos (para edição, inserção ou modo somente leitura). Esse evento é geralmente usado para executar uma tarefa quando o controle FormView altera os modos. |
| ModeChanging | Ocorre antes que o controle FormView altera modos (para edição, inserção ou modo somente leitura). Esse evento geralmente é usado para cancelar uma alteração de modo. |
| PageIndexChanged | Ocorre quando um dos botões de paginação é clicado, mas após o controle FormView manipula a operação de paginação. Esse evento normalmente é usado quando você precisa executar uma tarefa depois que o usuário navega para um registro diferente no controle. |
| PageIndexChanging | Ocorre quando um dos botões de paginação é acionado, mas antes de FormView controle manipula a operação de paginação. Esse evento geralmente é usado para cancelar a operação de paginação. |

## <a name="detailsview"></a>DetailsView

O controle DetailsView é usado para exibir um registro individual de uma fonte de dados em uma tabela, onde cada campo do registro é exibido em uma linha da tabela. Ele pode ser usado em combinação com um controle GridView para cenários de detalhes mestre. O controle DetailsView suporta os seguintes recursos:

- Associando dados a controles de fonte, como SqlDataSource.
- Recursos internos de inserção.
- Atualizando e excluir recursos internos.
- Recursos de paginação internos.
- Acesso programático ao modelo de objeto DetailsView para definir dinamicamente a propriedades, lidar com eventos e assim por diante.
- Aparência personalizável por meio de estilos e temas.

## <a name="row-fields"></a>Campos de linha

Cada linha de dados no controle DetailsView é criada por meio da declaração de um controle de campo. Tipos de campo de linha diferente determinam o comportamento das linhas no controle. Controles de campo derivam DataControlField. A tabela a seguir lista os tipos de campo de linha diferentes que podem ser usados.

| **Tipo de campo de coluna** | **Descrição** |
| --- | --- |
| BoundField | Exibe o valor de um campo em uma fonte de dados como texto. |
| ButtonField | Exibe um botão de comando no controle DetailsView. Isso permite que você exiba uma linha com um controle de botão personalizado, como uma adição ou um botão Remover. |
| CheckBoxField | Exibe uma caixa de seleção no controle DetailsView. Esse tipo de campo de linha é comumente usado para exibir campos com um valor booliano. |
| CommandField | Exibe o comando interno botões para executar a edição, inserção ou exclusão operações no controle DetailsView. |
| HyperLinkField | Exibe o valor de um campo em uma fonte de dados como um hiperlink. Esse tipo de campo de linha permite associar um segundo campo a URL do hiperlink. |
| ImageField | Exibe uma imagem no controle DetailsView. |
| TemplateField | Exibe conteúdo definido pelo usuário para uma linha no controle DetailsView acordo com um modelo especificado. Esse tipo de campo de linha permite que você crie um campo de linha personalizado. |

Por padrão, a propriedade AutoGenerateRows é definida como **verdadeira**, que gera automaticamente um objeto de campo de limite de linha para cada campo de um tipo associável na fonte de dados. Tipos de associáveis válidos são String, DateTime, Decimal, Guid e o conjunto de tipos primitivos. Cada campo é exibido em uma linha como texto, na ordem em que cada campo aparece na fonte de dados.

Geração automática de linhas fornece uma maneira rápida e fácil para exibir todos os campos no registro. No entanto, para utilizar DetailsView controle de recursos avançados de que você deve declarar explicitamente os campos de linha para incluir no controle DetailsView. Para declarar os campos de linha, primeiro defina a **AutoGenerateRows** propriedade **falso**. Em seguida, adicione a abertura e fechamento **&lt;campos&gt;** marcas entre as marcas de abertura e fechamento do controle DetailsView. Por fim, liste os campos de linha que você deseja incluir entre a abertura e fechamento **&lt;campos&gt;** marcas. Os campos de linha especificados são adicionados à coleção de campos na ordem listada. O **campos** coleção permite que você gerenciar programaticamente os campos de linha no controle DetailsView.

> [!NOTE]
> Gerado automaticamente os campos não são adicionados à coleção de campos de linha.


## <a name="binding-to-data"></a>Associando a dados

O controle DetailsView pode ser associado a um controle de fonte de dados, como **SqlDataSource** ou AccessDataSource, ou para qualquer fonte de dados que implementa a interface IEnumerable, como System.Data.DataView, ArrayList e Hashtable.

Use um dos seguintes métodos para associar o controle DetailsView para o tipo de fonte de dados apropriado:

- Para associar a um controle de fonte de dados, defina a propriedade DataSourceID do controle DetailsView para o valor da ID do controle de fonte de dados. O controle DetailsView associa automaticamente para o controle de fonte de dados especificado. Esse é o método preferencial para associar aos dados.
- Para associar a uma fonte de dados que implementa o **IEnumerable** de interface, definir programaticamente a propriedade DataSource do controle DetailsView à fonte de dados e, em seguida, chama o método DataBind.

## <a name="security"></a>Segurança

Esse controle pode ser usado para exibir a entrada do usuário, que pode incluir um script de cliente mal-intencionado. Verificar as informações que são enviadas de um cliente para o script executável, instruções SQL ou outro código antes de exibi-lo em seu aplicativo. O ASP.NET fornece um recurso de validação de solicitação de entrada para o bloco de script e HTML na entrada do usuário.

## <a name="data-operations"></a>Operações de dados

O controle DetailsView fornece recursos internos que permitem ao usuário atualizar, excluir, inserir e página por meio de itens no controle. Quando o controle DetailsView é associado a um controle de fonte de dados, o controle DetailsView pode tirar proveito da fonte de dados de recursos do controle e fornecer a atualizar, excluir, inserir e funcionalidade de paginação automática.

O controle DetailsView pode fornecer suporte para atualização, exclusão, inserção e operações de paginação com outros tipos de fontes de dados; No entanto, você deve fornecer a implementação para essas operações em um manipulador de eventos apropriado.

O controle DetailsView pode adicionar automaticamente um **CommandField** campo de linha com um botão Editar, excluir ou novo, definindo as propriedades AutoGenerateEditButton, AutoGenerateDeleteButton ou AutoGenerateInsertButton para **verdadeiro**, respectivamente. Ao contrário da exclusão de botão (o que exclui o registro selecionado imediatamente), quando é clicado no botão Edit ou New, DetailsView controle passa para editar ou modo de inserção, respectivamente. No modo de edição, no botão Editar é substituído por uma atualização e um botão Cancelar. Controles de entrada que são apropriados para o tipo de dados do campo (como uma caixa de texto ou um controle de caixa de seleção) são exibidos com um valor de campo para o usuário modificar. Clicar no botão de atualização atualiza o registro na fonte de dados, enquanto clicando no botão Cancelar abandona quaisquer alterações. Da mesma forma, no modo de inserção, o novo botão é substituído por uma instrução Insert e um botão Cancelar, e controles de entrada vazios são exibidos para o usuário insira os valores para o novo registro.

O controle DetailsView fornece um recurso de paginação, que permite ao usuário navegar para outros registros na fonte de dados. Quando habilitado, os controles de navegação de página são exibidos em uma linha de pager. Para habilitar a paginação, defina a propriedade de AllowPaging como **verdadeira**. A linha de pager pode ser personalizada usando as propriedades PagerStyle e PagerSettings.

## <a name="customizing-the-user-interface"></a>Personalizando a interface do usuário

Você pode personalizar a aparência do controle DetailsView definindo as propriedades de estilo para as diferentes partes do controle. A tabela a seguir lista as propriedades de estilo diferente.

| **Propriedade de estilo** | **Descrição** |
| --- | --- |
| AlternatingRowStyle | As configurações de estilo para as linhas de dados alternados no controle DetailsView. Quando essa propriedade é definida, as linhas de dados são exibidas alternando entre as configurações de RowStyle e o **AlternatingRowStyle** configurações. |
| CommandRowStyle | As configurações de estilo para a linha que contém os botões de comando internas no controle DetailsView. |
| EditRowStyle | As configurações de estilo para as linhas de dados quando o controle DetailsView está no modo de edição. |
| EmptyDataRowStyle | As configurações de estilo para a linha de dados vazia exibida no controle DetailsView quando a fonte de dados não contém nenhum registro. |
| FooterStyle | As configurações de estilo para a linha de rodapé do controle DetailsView. |
| HeaderStyle | As configurações de estilo para a linha de cabeçalho do controle DetailsView. |
| InsertRowStyle | As configurações de estilo para as linhas de dados quando o controle DetailsView está no modo de inserção. |
| PagerStyle | As configurações de estilo da linha do pager do controle DetailsView. |
| RowStyle | As configurações de estilo para as linhas de dados no controle DetailsView. Quando o **AlternatingRowStyle** propriedade também for definida, as linhas de dados são exibidas, alternando entre os **RowStyle** configurações e o **AlternatingRowStyle** configurações. |
| FieldHeaderStyle | As configurações de estilo para a coluna de cabeçalho do controle DetailsView. |

## <a name="events"></a>Eventos

O controle DetailsView fornece vários eventos que você pode programar. Isso permite que você execute uma rotina personalizada sempre que um evento ocorre. A tabela a seguir lista os eventos com suporte pelo controle DetailsView. O controle DetailsView também herda esses eventos de suas classes base: vinculação de dados, associação de dados, Disposed, Init, Load, PreRender e renderização.

| **Event** | **Descrição** |
| --- | --- |
| ItemCommand | Ocorre quando um botão é clicado no controle DetailsView. |
| ItemCreated | Ocorre depois que todos os objetos DetailsViewRow são criados no controle DetailsView. Esse evento é geralmente usado para modificar os valores de um registro antes de ser exibido. |
| ItemDeleted | Ocorre quando um botão Excluir é clicado, mas após o controle DetailsView exclui o registro da fonte de dados. Esse evento é geralmente usado para verificar os resultados da operação de exclusão. |
| ItemDeleting | Ocorre quando um botão Excluir é clicado, mas antes do DetailsView controle exclui o registro da fonte de dados. Esse evento geralmente é usado para cancelar a operação de exclusão. |
| ItemInserted | Ocorre quando um botão de inserção é clicado, mas após o controle DetailsView insere o registro. Esse evento é geralmente usado para verificar os resultados da operação de inserção. |
| ItemInserting | Ocorre quando um botão de inserção é clicado, mas antes do DetailsView controle insere o registro. Esse evento geralmente é usado para cancelar a operação de inserção. |
| ItemUpdated | Ocorre quando um botão de atualização é clicado, mas após o controle DetailsView atualiza a linha. Esse evento é geralmente usado para verificar os resultados da operação de atualização. |
| ItemUpdating | Ocorre quando um botão de atualização é clicado, mas antes do DetailsView controle atualiza o registro. Esse evento geralmente é usado para cancelar a operação de atualização. |
| ModeChanged | Ocorre depois que o controle DetailsView altera modos (edição, inserção ou modo somente leitura). Esse evento é geralmente usado para executar uma tarefa quando o controle DetailsView altera os modos. |
| ModeChanging | Ocorre antes que o controle DetailsView altera modos (edição, inserção ou modo somente leitura). Esse evento geralmente é usado para cancelar uma alteração de modo. |
| PageIndexChanged | Ocorre quando um dos botões de paginação é clicado, mas após o controle DetailsView manipula a operação de paginação. Esse evento normalmente é usado quando você precisa executar uma tarefa depois que o usuário navega para um registro diferente no controle. |
| PageIndexChanging | Ocorre quando um dos botões de paginação é acionado, mas antes do DetailsView controle manipula a operação de paginação. Esse evento geralmente é usado para cancelar a operação de paginação. |

## <a name="the-menu-control"></a>O controle Menu

O controle de Menu no ASP.NET 2.0 é projetado para ser um sistema completo de navegação. Ele pode ser vinculados a dados com facilidade a fontes de dados hierárquicos como SiteMapDataSource.

Uma estrutura de controles de Menu pode ser definida declarativamente ou dinamicamente e consiste em um único nó raiz e qualquer número de subnós. O código a seguir define declarativamente um menu para o controle de Menu.

[!code-aspx[Main](data-bound-controls/samples/sample4.aspx)]

No exemplo acima, o nó de home. aspx é o nó raiz. Todos os outros nós são aninhados dentro do nó raiz em vários níveis.

Há dois tipos de menus que pode renderizar o controle de Menu; menus de estáticas e dinâmica. Menus estáticos consistem em itens de menu estão sempre visíveis. Menus dinâmicos consistem em itens de menu que são visíveis apenas quando o usuário focalizá-los com o mouse. Os clientes geralmente podem confundir menus estáticos com definidas de maneira declarativa de menus e menus dinâmicos com menus que estão vinculados a dados em tempo de execução. Na verdade, menus dinâmicos e estáticos estão relacionados ao método da população. Os termos *estáticos* e *dinâmico* só se referir a ou não o menu é estaticamente exibido por padrão, ou apenas exibida quando o usuário executa alguma ação.

O **ExibirNíveisEstaticamente** propriedade é usada para configurar quantos níveis de menu são estáticos e, portanto, é exibido por padrão. No exemplo acima, definindo o **ExibirNíveisEstaticamente** propriedade para um valor de 2 causaria menu Exibir estaticamente o nó inicial, o nó de música e o nó de filmes. Todos os outros nós seriam exibidos dinamicamente quando o usuário passa o mouse sobre o nó pai.

O **MaximumDynamicDisplayLevels** propriedade configura o número máximo de níveis de dinâmicos de menu é capaz de exibir. Qualquer menus dinâmicos em um nível mais alto que o valor especificado pela **MaximumDynamicDisplayLevels** propriedade são descartados.

> [!NOTE]
> É quase certo que você pode encontrar situações em que os menus não aparecem renderizar devido à propriedade MaximumDynamicDisplayLevels. Nesses casos, certifique-se de que a propriedade é definida suficientemente para permitir a exibição de menus de clientes.


## <a name="data-binding-the-menu-control"></a>O controle de Menu de vinculação de dados

O controle de Menu pode ser associado a qualquer fonte de dados hierárquicos, como o SiteMapDataSource ou XMLDataSource. SiteMapDataSource é o método mais comumente usado para associação de dados para um controle de Menu porque ele feeds de fora o arquivo Web. sitemap e seu esquema fornece uma API conhecida para o controle de Menu. Lista a seguir mostra um simples arquivo Web. sitemap.

[!code-xml[Main](data-bound-controls/samples/sample5.xml)]

Observe que há apenas um elemento raiz siteMapNode, nesse caso, o elemento inicial. Vários atributos podem ser configurados para cada siteMapNode. Os atributos mais usados são:

- **URL** Especifica a URL a ser exibido quando um usuário clica no item de menu. Se esse atributo não estiver presente, o nó simplesmente será postado quando clicado.
- **título** Especifica o texto que é exibido no item de menu.
- **Descrição** usado como documentação para o nó. Também é exibida como uma dica de ferramenta quando o mouse é colocado sobre o nó.
- **siteMapFile** permite sitemaps aninhados. Esse atributo deve apontar para um arquivo de mapa do site do ASP.NET bem formado.
- **funções** permite a aparência de um nó para ser controlada pela remoção de segurança do ASP.NET.

Observe que enquanto esses atributos são todos opcionais, o comportamento do menu pode não ser o que é esperado se eles não forem especificados. Por exemplo, se o *url* atributo for especificado, mas o *descrição* atributo não estiver, o nó não será visível e não haverá nenhuma maneira de navegar para a URL especificada.

## <a name="controlling-a-menus-operation"></a>Controlando uma operação de Menus

Há várias propriedades que afetam a operação de um controle de Menu do ASP.NET; o **orientação** propriedade, o **DisappearAfter** propriedade, o **StaticItemFormatString** propriedade e o **StaticPopoutImageUrl**propriedade são apenas alguns deles.

- O **orientação** pode ser definido como *horizontal* ou *vertical* e controla se os itens de menu estático são dispostos horizontalmente em uma linha ou verticalmente e eles empilhados em cima uma da outra. Essa propriedade não afeta menus dinâmicos.
- O **DisappearAfter** propriedade configura quanto um menu dinâmico deve permanecer visível depois que o mouse foi movido para longe dela. O valor é especificado em milissegundos e o padrão é 500. Definir essa propriedade como um valor de -1 fará com que o menu de nunca desaparecem automaticamente. Nesse caso, o menu apenas desaparecerá quando o usuário clica fora do menu.
- O **StaticItemFormatString** propriedade torna mais fácil manter argumentação consistente em seu sistema de menu. Ao especificar essa propriedade, *{0}* deve ser inserido em vez da descrição que aparece na fonte de dados. Por exemplo, para que o item de menu do Exercício 1 say visite nossa página de produtos, etc., você especificaria visita nossa {0} página para o StaticItemFormatString. No tempo de execução ASP.NET substituirá qualquer ocorrência de {0} com a descrição correta para o item de menu.
- O **StaticPopoutImageUrl** propriedade especifica a imagem que é usada para indicar que um nó de determinado menu tem nós filho que podem ser acessados passando o mouse sobre ele. Menus dinâmicos continuarão a usar a imagem padrão.

## <a name="templated-menu-controls"></a>Controles de Menu modelado

O controle de Menu é um controle personalizado e permite dois ItemTemplates diferentes; o StaticItemTemplate e o DynamicItemTemplate. Usando esses modelos, você pode adicionar facilmente controles de servidor ou controles de usuário para seus menus.

Para editar os modelos no Visual Studio .NET, clique no botão de marca inteligente no menu e escolha Editar modelos. Em seguida, você pode escolher entre o StaticItemTemplate ou o DynamicItemTemplate de edição.

Todos os controles adicionados ao StaticItemTemplate aparecerá no menu estático quando a página for carregada. Todos os controles adicionados ao DynamicItemTemplate aparecerá em todos os menus pop-up.

## <a name="menu-events"></a>Eventos de menu

O controle de Menu tem dois eventos que são exclusivos a ele. o **MenuItemClicked** e o **MenuItemDatabound** eventos.

O evento MenuItemClicked é gerado quando um item de menu é clicado. O evento MenuItemDatabound é gerado quando um item de menu é vinculação de dados. O **MenuEventArgs** que é passado para o evento manipulador fornece acesso ao item de menu por meio da propriedade do Item.

## <a name="controlling-a-menus-appearance"></a>Controlando uma aparência de Menus

Você também pode afetar a aparência de um controle de menu usando um ou mais dos muitos estilos disponíveis para o formato de menus. Entre eles estão **StaticMenuStyle**, **DynamicMenuStyle**, **DynamicMenuItemStyle**, **DynamicSelectedStyle**e **DynamicHoverStyle**. Essas propriedades são configuradas usando uma cadeia de caracteres de estilo HTML padrão. Por exemplo, o seguinte afetará o estilo para menus dinâmicos.

[!code-aspx[Main](data-bound-controls/samples/sample6.aspx)]

> [!NOTE]
> Se você estiver usando qualquer um dos estilos em foco, você precisará adicionar um &lt;head&gt; elemento em até a página com o *runat* elemento definido como *server*.


Controles de menu também suportam o uso de temas do ASP.NET 2.0.

## <a name="the-treeview-control"></a>O controle TreeView

O controle TreeView exibe dados em uma estrutura de árvore. Assim como acontece com o controle de Menu pode ser facilmente dados ligados a qualquer fonte de dados hierárquicos, como o SiteMapDataSource.

A primeira pergunta que os clientes possam perguntar sobre o controle TreeView do ASP.NET 2.0 é ou não está relacionado ao WebControl do IE TreeView que estava disponível para o ASP.NET 1.x. Não é. O controle TreeView do ASP.NET 2.0 foi criado desde o início e oferece uma melhoria significativa em relação a WebControl de TreeView do IE que estava disponível anteriormente.

Não entrarei em detalhes sobre como associar um controle TreeView a um mapa de site que ele seja executado exatamente da mesma maneira como o controle de Menu. No entanto, o controle TreeView tem algumas diferenças distintas da maneira que ele opera.

Por padrão, um controle TreeView aparece totalmente expandido. Para alterar o nível de expansão após o carregamento inicial, modifique a **ExpandDepth** propriedade do controle. Isso é particularmente importante em casos em que o modo de exibição de árvore é vinculação de dados após a expansão de nós específicos.

## <a name="databinding-the-treeview-control"></a>Associação de dados no controle TreeView

Ao contrário do controle de Menu, o modo de exibição de árvore funciona bem para tratar grandes quantidades de dados. Portanto, além de ligação de dados com um XMLDataSource ou SiteMapDataSource um, o modo de exibição de árvore geralmente é associada a dados para um conjunto de dados ou outros dados relacionais. Em casos onde o controle TreeView é associado a grandes quantidades de dados, é melhor associar-se somente aos dados que são realmente visíveis no controle. Você pode associar dados a dados adicionais como nós de TreeView são expandidos.

Nesses casos, o **PopulateOnDemand** propriedade de TreeView deve ser definida como *verdadeiro*. Em seguida, você precisará fornecer uma implementação para o **TreeNodePopulate** método.

## <a name="data-binding-without-postback"></a>Vinculação de dados sem um PostBack

Observe que, quando você expande um nó no exemplo anterior pela primeira vez, a página é postada e é atualizada. O que não é um problema neste exemplo, mas você pode imaginar que pode estar em um ambiente de produção com uma grande quantidade de dados. Um cenário melhor seria um no qual o modo de exibição de árvore seria ainda dinamicamente preencher seus nós, mas sem uma postagem de volta para o servidor.

Definindo o **PopulateNodesFromClient** e o **PopulateOnDemand** propriedades como true, o controle TreeView do ASP.NET preencherá dinamicamente os nós sem uma postagem de volta. Quando o nó pai é expandido, uma solicitação XMLHttp é feita do cliente e o evento OnTreeNodePopulate é disparado. O servidor responde com uma ilha de dados XML que é usado para dados associar os nós filho.

O ASP.NET cria dinamicamente o código do cliente que implementa essa funcionalidade. O &lt;script&gt; marcas que contém o script são geradas que aponta para um arquivo AXD. Por exemplo, a lista a seguir mostra os links de script para o código de script que gera a solicitação XMLHttp.

[!code-html[Main](data-bound-controls/samples/sample7.html)]

Se você procurar o arquivo AXD acima em seu navegador e abri-lo, você verá o código que implementa a solicitação XMLHttp. Esse método impede que os clientes de modificar o arquivo de script.

## <a name="controlling-the-operation-of-the-treeview-control"></a>Controlar a operação do controle TreeView

O controle TreeView tem várias propriedades que afetam a operação do controle. As propriedades mais óbvias são os **ShowCheckBoxes**, **ShowExpandCollapse**, e **ShowLines**.

O **ShowCheckBoxes** propriedade afeta se nós exibem uma caixa de seleção quando renderizado. Os valores válidos para essa propriedade são **None**, **raiz**, **pai**, **folha**, e **todos os**. Elas afetam o controle TreeView da seguinte maneira:

| **Valor da propriedade** | **Em vigor** |
| --- | --- |
| Nenhum | Caixas de seleção não são exibidas em todos os nós. Essa é a configuração padrão. |
| Raiz | Uma caixa de seleção só é exibida no nó raiz. |
| Pai | Uma caixa de seleção é exibida apenas em nós que têm nós filho. Esses nós filho podem ser nós pai ou nós folha. |
| Folha | Uma caixa de seleção é exibida somente nos nós que não têm nós filhos. |
| Todos | Uma caixa de seleção é exibida em todos os nós. |

Quando as caixas de seleção estão sendo usadas, o **CheckedNodes** propriedade retornará uma coleção de nós TreeView que serão verificadas após o postback.

O **ShowExpandCollapse** propriedade controla a aparência da imagem de expandir/recolher ao lado de nós pai e raiz. Se essa propriedade é definida como **falsos**, TreeView nós são renderizados como hiperlinks e são expandido/recolhido clicando no link.

O **ShowLines** propriedade controla se as linhas são exibidas se conectar a nós pai nós filho. Quando **falsos** (padrão), sem as linhas são exibidas. Quando **verdadeira**, o controle TreeView usará imagens de linhas na pasta especificada pela **LineImagesFolder** propriedade.

Para personalizar a aparência das linhas de TreeView, o Visual Studio .NET 2005 inclui uma ferramenta de linha de Designer. Você pode acessar essa ferramenta usando o botão de marca inteligente do controle TreeView como abaixo.


![](data-bound-controls/_static/image1.jpg)

**Figura 1**


Quando seleciona os **personalizar imagens de linha** opção de menu, a ferramenta de linha Designer será iniciada, permitindo que você configurar a aparência das linhas de TreeView.

## <a name="treeview-events"></a>Eventos de TreeView

O controle TreeView tem os seguintes eventos exclusivos:

- SelectedNodeChanged ocorre quando um nó é selecionado com base nas **SelectAction** propriedade.
- TreeNodeCheckChanged ocorre quando um estado de checkboxs nós é alterado.
- TreeNodeExpanded ocorre quando um nó é expandido com base nas **SelectAction** propriedade.
- TreeNodeCollapsed ocorre quando um nó é recolhido.
- TreeNodeDataBound ocorre quando um nó de dados associado.
- TreeNodePopulate ocorre quando um nó é preenchido.

O **SelectAction** propriedade permite que você configure qual evento é acionado quando um nó é selecionado. A propriedade SelectAction fornece as seguintes ações:

- TreeNodeSelectAction.Expand gera TreeNodeExpanded quando o nó é selecionado.
- TreeNodeSelectAction.None não gera nenhum evento quando o nó é selecionado.
- TreeNodeSelectAction.Select aciona o evento SelectedNodeChanged quando o nó é selecionado.
- TreeNodeSelectAction.SelectExpand aciona o evento SelectedNodeChanged e o evento TreeNodeExpanded quando o nó é selecionado.

## <a name="controlling-appearance-with-styles"></a>Controlando a aparência com estilos

O controle TreeView fornece muitas propriedades para controlar a aparência do controle com estilos. As seguintes propriedades estão disponíveis.

| **Nome da propriedade** | **Controles** |
| --- | --- |
| HoverNodeStyle | Controla o estilo de nós quando o mouse é colocado sobre eles. |
| LeafNodeStyle | Controla o estilo de nós folha. |
| NodeStyle | Controla o estilo de todos os nós. Estilos de nó específico (como LeafNodeStyle) substituem esse estilo. |
| ParentNodeStyle | Controla o estilo de todos os nós pai. |
| RootNodeStyle | Controla o estilo para o nó raiz. |
| SelectedNodeStyle | Controla o estilo para o nó selecionado. |

Cada uma dessas propriedades é somente leitura. No entanto, eles serão cada retorno de um **TreeNodeStyle** objeto e as propriedades desse objeto podem ser modificado usando o *subpropriedade da propriedade* formato. Por exemplo, para definir a **ForeColor** propriedade da **SelectedNodeStyle**, você usaria a seguinte sintaxe:

[!code-aspx[Main](data-bound-controls/samples/sample8.aspx)]

Observe que a marcação acima não é fechada. Isso ocorre porque, ao usar a sintaxe declarativa mostrada aqui, você incluiria os nós TreeViews no código HTML.

As propriedades de estilo também podem ser especificadas no código usando o *Property. Subproperty* formato. Por exemplo, para definir a **ForeColor** propriedade da **RootNodeStyle** no código, você usaria a seguinte sintaxe:

[!code-csharp[Main](data-bound-controls/samples/sample9.cs)]

> [!NOTE]
> Para obter uma lista abrangente das propriedades de estilo diferente, consulte a documentação do MSDN no objeto TreeNodeStyle.


## <a name="the-sitemappath-control"></a>O controle SiteMapPath

O controle SiteMapPath fornece um controle de navegação de trilha de navegação para os desenvolvedores do ASP.NET. Como os outros controles de navegação, ele pode ser facilmente dados ligados a dados hierárquicos fontes como o XmlDataSource ou SiteMapDataSource.

Um controle SiteMapPath é composto por objetos SiteMapNodeItem. Há três tipos de nós; o nó raiz, nós pai e o nó atual. O nó raiz é o nó na parte superior da estrutura hierárquica. O nó atual representa a página atual. Todos os outros nós são nós pai.

## <a name="controlling-the-operation-of-the-sitemappath-control"></a>Controlar a operação do controle SiteMapPath

As propriedades que controlam a operação do controle SiteMapPath são da seguinte maneira:

| **Property** | **Descrição da propriedade** |
| --- | --- |
| ParentLevelsDisplayed | Controla quantos nós pai são exibidos. O padrão é -1 que não impõe nenhuma restrição no número de nós pai exibidos. |
| PathDirection | Controla a direção do SiteMapPath. Os valores válidos são RootToCurrent (padrão) e CurrentToRoot. |
| PathSeparator | Uma cadeia de caracteres que controla o caractere que separa nós em um controle SiteMapPath. O valor padrão é:. |
| RenderCurrentNodeAsLink | Um valor booliano que controla se ou não o nó atual é renderizado como um link. O padrão é False. |
| SkipLinkText | Ajuda com acessibilidade quando a página é exibida pelos leitores de tela. Essa propriedade permite que os leitores de tela para ignorar o controle SiteMapPath. Para desabilitar esse recurso, defina a propriedade como string. Empty. |

## <a name="templated-sitemappath-controls"></a>Controles SiteMapPath com modelo

O SiteMapControl é um controle personalizado e como tal, você pode definir modelos diferentes para uso na exibição de controle. Para editar modelos em um controle SiteMapPath, clique no botão de marca inteligente do controle e escolha Editar modelos no menu. Isso exibe o menu de SiteMapTasks, conforme mostrado abaixo em que você pode escolher entre os diferentes modelos disponíveis.


![](data-bound-controls/_static/image2.jpg)

**Figura 2**


O **NodeTemplate** modelo se refere a qualquer nó no SiteMapPath. Se o nó é um nó raiz ou o nó atual e um **RootNodeTemplate** ou **CurrentNodeTemplate** é configurado, NodeTemplate é substituído.

## <a name="sitemappath-events"></a>Eventos de SiteMapPath

O controle SiteMapPath tem dois eventos que não são derivados da classe de controle; o **ItemCreated** evento e o **ItemDataBound** eventos. O evento ItemCreated é gerado quando um item SiteMapPath é criado. ItemDataBound é disparado quando o método DataBind é chamado durante a vinculação de dados de um nó SiteMapPath. Um **SiteMapNodeItemEventArgs** objeto fornece acesso para o SiteMapNodeItem específico por meio da propriedade do Item.

## <a name="controlling-appearance-with-styles"></a>Controlando a aparência com estilos

Os estilos a seguir estão disponíveis para formatação de um controle SiteMapPath.

| **Nome da propriedade** | **Controles** |
| --- | --- |
| CurrentNodeStyle | Controla o estilo do texto para o nó atual. |
| RootNodeStyle | Controla o estilo do texto para o nó raiz. |
| NodeStyle | Controla o estilo do texto para todos os nós, supondo que um CurrentNodeStyle ou RootNodeStyle não se aplica. |

A propriedade NodeStyle é substituída pelo CurrentNodeStyle ou o RootNodeStyle. Cada uma dessas propriedades é somente leitura e retorna um **estilo** objeto. Para afetar a aparência de um nó usando uma dessas propriedades, você precisará definir as propriedades do objeto de estilo que é retornado. Por exemplo, o código a seguir altera a propriedade de cor de primeiro plano do nó atual.

[!code-aspx[Main](data-bound-controls/samples/sample10.aspx)]

A propriedade também pode ser aplicada por meio de programação da seguinte maneira:

[!code-csharp[Main](data-bound-controls/samples/sample11.cs)]

> [!NOTE]
> Se um modelo é aplicado, o estilo não será aplicado.


## <a name="lab-1-configuring-an-aspnet-menu-control"></a>Laboratório 1: Configurar um controle de Menu do ASP.NET

1. Crie um novo site.
2. Adicione um arquivo de mapa de Site selecionando arquivo, novo, arquivo e escolhendo o mapa do Site na lista de modelos de arquivos.
3. Abra o mapa de site (sitemap por padrão) e modificá-lo para que ele se parece com a listagem a seguir. As páginas para o qual você está vinculando no arquivo de mapa de site não realmente existem, mas que não será um problema para este exercício.

    [!code-xml[Main](data-bound-controls/samples/sample12.xml)]
4. Abra o formulário da Web padrão no modo de Design.
5. Na seção de navegação da caixa de ferramentas, adicione um novo controle de Menu para a página.
6. Da seção de dados da caixa de ferramentas, adicione um novo SiteMapDataSource. SiteMapDataSource usará automaticamente o arquivo Web. sitemap no seu site. (O arquivo Web. sitemap *deve* estar na pasta raiz do site.)
7. Clique no controle de Menu e, em seguida, clique no botão de marca inteligente para exibir a caixa de diálogo de tarefas do Menu.
8. No menu suspenso Escolher fonte de dados, selecione SiteMapDataSource1.
9. Clique no link de AutoFormatação e escolha um formato para o Menu.
10. No painel Propriedades, defina as **ExibirNíveisEstaticamente** propriedade como 2. O controle Menu agora deve exibir o nó inicial, produtos e serviços no Designer.
11. Procure a página em seu navegador para usar o menu. (Como as páginas que você adicionou ao mapa de site, na verdade, não existem, você verá um erro quando você tentar e procurá-los.)

Fazer experiências alterando o ExibirNíveisEstaticamente e as propriedades de MaximumDynamicDisplayLevels e ver como elas afetam como o menu é renderizado.

## <a name="lab-2-dynamically-binding-a-treeview-control"></a>Laboratório 2: Associando dinamicamente um controle TreeView

Este exercício pressupõe que você tenha o SQL Server em execução localmente e se o banco de dados Northwind está presente na instância do SQL Server. Se essas condições não forem atendidas, altere a cadeia de caracteres de conexão no exemplo. Observe que você talvez também precise especificar a autenticação do SQL Server em vez de uma conexão confiável.

1. Crie um novo site.
2. Alterne para modo de exibição de código para default. aspx e substitua todo o código pelo código listado abaixo. 

    [!code-aspx[Main](data-bound-controls/samples/sample13.aspx)]
3. Salve a página como treeview.aspx.
4. Procure a página.
5. Quando a página é exibida pela primeira vez, exiba a origem da página em seu navegador. Observe que somente os nós visíveis foram enviados ao cliente.
6. Clique no sinal de adição ao lado de qualquer nó.
7. Exibir código-fonte na página novamente. Observe que os nós exibidos recentemente agora estão presentes.

## <a name="lab-3-details-view-and-editing-data-using-a-gridview-and-detailsview"></a>Laboratório 3: Detalhes exibir e editar dados usando um GridView e DetailsView

1. Crie um novo site.
2. Adicione um novo Web. config para o site da Web.
3. Adicione uma cadeia de caracteres de conexão no arquivo Web. config, conforme mostrado abaixo: 

    [!code-xml[Main](data-bound-controls/samples/sample14.xml)]

    > [!NOTE]
    > Você talvez precise alterar a cadeia de caracteres de conexão com base em seu ambiente.
4. Salve e feche o arquivo Web. config.
5. Abra default. aspx e adicione um novo controle SqlDataSource.
6. Alterar a ID do controle SqlDataSource **produtos**.
7. No **SqlDataSource Tasks** menu, clique em **configurar fonte de dados**.
8. Selecione **Northwind** na lista suspensa de conexão e clique em Avançar.
9. Selecione **produtos** da **nome** lista suspensa e verifique o **ProductID**, **ProductName**, **UnitPrice**, e **UnitsInStock** caixas de seleção, conforme mostrado abaixo. 

![](data-bound-controls/_static/image3.jpg)

    **Figure 3**
10. Clique em **Avançar**.
11. Clique em **Finalizar**.
12. Alternar a exibição da fonte e examine o código que foi gerado. Observe que o **SelectCommand**, **DeleteCommand**, **InsertCommand**, e **UpdateCommand** que foram adicionados ao SqlDataSource controle. Além disso, observe os parâmetros que foram adicionados.
13. Alterne para modo de Design e adicionar um novo controle GridView à página.
14. Selecione **produtos** da **Escolher fonte de dados** lista suspensa.
15. Verifique **habilitar paginação** e **Ativar seleção** conforme mostrado abaixo. 

![](data-bound-controls/_static/image4.jpg)

    **Figure 4**
16. Clique o **Edit Columns** vincular e certifique-se de que **gerar automaticamente campos** é verificada.
17. Clique em **OK**.
18. Com o controle GridView selecionado, clique no botão ao lado de **DataKeyNames** propriedade no painel Propriedades.
19. Selecione **ProductID** da **campos de dados disponíveis** lista e clique no **&gt;** botão para adicioná-lo.
20. Clique em OK.
21. Adicione um novo controle SqlDataSource para a página.
22. Alterar a ID do controle SqlDataSource **detalhes**.
23. No menu tarefas SqlDataSource, escolha **configurar fonte de dados**.
24. Escolher **Northwind** na lista suspensa e clique **próxima**.
25. Selecione <strong>produtos</strong> da <strong>nome</strong> lista suspensa e verifique o <strong> \</ strong > * caixa de seleção no <strong>colunas</strong> listbox.
26. Clique o **onde** botão.
27. Selecione **ProductID** da **coluna** lista suspensa.
28. Selecione **=** na lista suspensa do operador.
29. Selecione **controle** da **origem** lista suspensa.
30. Selecione **GridView1** da **ID do controle** lista suspensa.
31. Clique o **adicionar** botão para adicionar a cláusula WHERE.
32. Clique em **OK**.
33. Clique o **Advanced** botão e verifique o **gerar instruções INSERT, UPDATE e DELETE** caixa de seleção.
34. Clique em **OK**.
35. Clique em **próxima** e clique em **concluir**.
36. Adicione um controle DetailsView para a página.
37. No **Choose Data Source** lista suspensa, escolha **detalhes**.
38. Verifique as **habilitar edição** caixa de seleção, conforme mostrado abaixo. 

![](data-bound-controls/_static/image1.gif)

    **Figure 5**
39. Salve a página e procure o default. aspx.
40. Clique o **selecionar** link ao lado de registros diferentes para ver a atualização DetailsView automaticamente.
41. Clique o **editar** link no controle DetailsView.
42. Faça uma alteração no registro e clique em **atualização**.
