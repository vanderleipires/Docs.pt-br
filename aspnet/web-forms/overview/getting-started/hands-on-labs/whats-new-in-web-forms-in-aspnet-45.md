---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
title: O que há de novo no Web Forms do ASP.NET 4.5 | Microsoft Docs
author: rick-anderson
description: A nova versão do ASP.NET Web Forms apresenta uma série de melhorias que se concentrou em melhorar a experiência do usuário ao trabalhar com dados. Em versões anteriores do...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 0a1f88bd-97da-4ed1-86f1-605199dc75a4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: db2658ff1feae1d4c20e4cfd19c36cfdf9492761
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="whats-new-in-web-forms-in-aspnet-45"></a>O que há de novo no Web Forms no ASP.NET 4.5
====================
Por [Web Camps Team](https://twitter.com/webcamps)

> A nova versão do ASP.NET Web Forms apresenta uma série de melhorias que se concentrou em melhorar a experiência do usuário ao trabalhar com dados.
> 
> Em versões anteriores do Web Forms, ao usar a associação de dados para emitir o valor de um membro de objeto, você usou as expressões de associação de dados BIND () ou Eval (). Na nova versão do ASP.NET, é possível declarar o tipo de dados irá deve ser vinculado por meio de uma nova propriedade ItemType um controle. Definir esta propriedade permite que você use uma variável digitada para receber todos os benefícios da experiência de desenvolvimento do Visual Studio, como IntelliSense, navegação de membro e a verificação de tempo de compilação.
> 
> Com os controles de associação de dados, agora você pode também especificar seus próprios métodos personalizados para selecionar, atualização, exclusão e inserção de dados, simplificando a interação entre os controles da página e a lógica do aplicativo. Além disso, foram adicionados recursos de modelo de associação ASP.NET, o que significa que você pode mapear os dados da página de diretamente em parâmetros de tipo de método.
> 
> Validando a entrada do usuário também deve ser mais fácil com a versão mais recente do Web Forms. Agora você pode anotar as classes de modelo com atributos de validação do **DataAnnotations** solicitação que controla todos os seus sites e namespace validam entrada de usuário usando essas informações. Validação do lado do cliente em formulários da Web agora está integrada com o jQuery, fornecendo recursos de JavaScript discretas e limpeza código do lado do cliente.
> 
> Na área de validação de solicitação, melhorias foram feitas para tornar mais fácil desativar a validação de solicitação para partes específicas de seus aplicativos seletivamente ou ler dados de solicitação inválida.
> 
> Algumas melhorias foram feitas para Web Forms controles de servidor para tirar proveito dos novos recursos do HTML5:
> 
> - A propriedade TextMode do controle TextBox foi atualizada para dar suporte os novos tipos de entrada do HTML5 como email, datetime e assim por diante.
> - O controle de carregamento de arquivos agora dá suporte a vários carregamentos de arquivos de navegadores que oferecem suporte a esse recurso do HTML5.
> - Controles de validação agora suporte validação HTML5 elementos de entrada.
> - Novos elementos de HTML5 que têm atributos que representam uma URL agora oferecem suporte a runat =&quot;server&quot;. Como resultado, você pode usar as convenções do ASP.NET em caminhos de URL, como o ~ operador para representar a raiz do aplicativo (por exemplo, &lt;vídeo runat =&quot;servidor&quot; src =&quot;~/myVideo.wmv&quot; &gt; &lt;/vídeo&gt;).
> - O controle UpdatePanel foi corrigido para dar suporte a campos de entrada de lançamento HTML5.
> 
> No portal oficial do ASP.NET, você pode encontrar mais exemplos dos novos recursos no ASP.NET WebForms 4.5: [o que há de novo no ASP.NET 4.5 e o Visual Studio 2012](../../../../whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012.md#_Toc318097385)
> 
> Todo o código de exemplo e trechos de código são incluídos no Web Camps treinamento Kit, disponíveis em [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

Este laboratório prático, você aprenderá como:

- Usar expressões de associação de dados fortemente tipados
- Usar novos recursos de associação de modelo no Web Forms
- Usar provedores de valor para mapear os dados da página para métodos de lógica
- Usar anotações de dados para validação de entrada do usuário
- Demorar advange de validação do lado do cliente unobstrusive com jQuery nos formulários da Web
- Implementar a validação de solicitação granular
- Implementar o processamento em formulários da Web da página assíncrona

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Pré-requisitos

Você deve ter os seguintes itens para concluir este laboratório:

- [Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou superior (leitura [Apêndice A](#AppendixA) para obter instruções sobre como instalá-lo).

<a id="Setup"></a>
### <a name="setup"></a>Configuração

**Instalando os trechos de código**

Para sua conveniência, boa parte do código que você estiver gerenciando ao longo deste laboratório está disponível como trechos de código do Visual Studio. Para instalar os trechos de código executar **.\Source\Setup\CodeSnippets.vsi** arquivo.

Se você não estiver familiarizado com os trechos de código do Visual Studio e aprender como usá-los, consulte o Apêndice deste documento &quot; [trechos de código usando do apêndice c:](#AppendixC)&quot;.

<a id="Exercises"></a>
## <a name="exercises"></a>Exercícios

Este laboratório prático inclui os seguintes exercícios:

1. [Exercício 1: Modelo de associação em Web Forms do ASP.NET](#Exercise1)
2. [Exercício 2: Validação de dados](#Exercise2)
3. [Exercício 3: Página assíncrona de processamento no ASP.NET Web Forms](#Exercise3)

> [!NOTE]
> Cada exercício é acompanhado por um **final** pasta que contém a solução resultante, você deve obter depois de concluir os exercícios. Você pode usar essa solução como um guia se você precisar trabalhar com os exercícios de ajuda adicional.


Tempo estimado para concluir este laboratório: **60 minutos**.

<a id="Exercise1"></a>

<a id="Exercise_1_Model_Binding_in_ASPNET_Web_Forms"></a>
### <a name="exercise-1-model-binding-in-aspnet-web-forms"></a>Exercício 1: Modelo de associação em Web Forms do ASP.NET

A nova versão do ASP.NET Web Forms apresenta uma série de melhorias que se concentrou em melhorar a experiência ao trabalhar com dados. Ao longo deste exercício, você saiba mais sobre os controles de dados com rigidez de tipos e associação de modelo.

<a id="Task_1_-_Using_Strongly-Typed_Data-Bindings"></a>
#### <a name="task-1---using-strongly-typed-data-bindings"></a>Tarefa 1: usando associações de dados fortemente tipados

Nesta tarefa, você descobrirá que as novas associações fortemente tipada disponíveis no ASP.NET 4.5.

1. Abra o **começar** solução localizado em **fonte/Ex1-ModelBinding/Begin/** pasta.

   1. Você precisará baixar alguns pacotes do NuGet ausentes antes de continuar. Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.
   2. No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, compile a solução clicando **criar** | **compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto. É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.
2. Abra o **Customers.aspx** página. Colocar uma lista não numerada no controle principal e incluir um controle repetidor dentro para listar cada cliente. Defina o nome do repetidor para **customersRepeater** conforme mostrado no código a seguir.

    Em versões anteriores do Web Forms, ao usar a associação de dados para emitir o valor de um membro em um objeto estiver associação de dados, você usaria uma expressão de associação de dados, juntamente com uma chamada para o método de avaliação, passando no nome do membro como uma cadeia de caracteres.

    Em tempo de execução, essas chamadas Eval usam a reflexão em relação ao objeto vinculado no momento para ler o valor do membro com o nome fornecido e exibir o resultado no HTML. Essa abordagem facilita muito a associação de dados em relação aos dados arbitrários, unshaped.

    Infelizmente, você perderá muitos dos recursos ótima experiência de tempo de desenvolvimento no Visual Studio, incluindo IntelliSense para nomes de membros, suporte para navegação (como ir para definição) e a verificação de tempo de compilação.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample1.aspx)]
3. Abra o **Customers.aspx.cs** arquivo.
4. Adicione o seguinte usando a instrução.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample2.cs)]
5. No **página\_carga** método, adicione código para preencher o repetidor com a lista de clientes.

    (Código de trecho - *Web de fonte de dados de clientes de ligação do Forms laboratório - Ex01 -*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample3.cs)]

    A solução usa EntityFramework junto com CodeFirst para criar e acessar o banco de dados. No código a seguir, o customersRepeater está associado a uma consulta materializada que retorna todos os clientes do banco de dados.
6. Pressione **F5** para executar a solução e vá para o **clientes** página para ver o repetidor em ação. Como a solução está usando CodeFirst, o banco de dados será criado e preenchido em sua instância local do SQL Express quando executar o aplicativo.

    ![Listando os clientes com um repetidor](whats-new-in-web-forms-in-aspnet-45/_static/image1.png "listando os clientes com um repetidor")

    *Listando os clientes com um repetidor*

    > [!NOTE]
    > No Visual Studio 2012, o IIS Express é o servidor de desenvolvimento de Web padrão.
7. Feche o navegador e volte para o Visual Studio.
8. Agora, substitua a implementação para usar com rigidez de tipos de associações. Abra o **Customers.aspx** página e use a nova **ItemType** atributo no repetidor para definir o **cliente** tipo como o tipo de associação.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample4.aspx)]

    A propriedade ItemType permite declarar o tipo de dados o controle é a ser associado ao e permite que você use fortemente tipada associação dentro do controle de associação de dados.
9. Substitua o ItemTemplate conteúdo com o código a seguir.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample5.aspx)]

    Uma desvantagem com abordagens acima é que as chamadas para Eval () e BIND () são tardia - que significa que passar cadeias de caracteres para representar os nomes de propriedade. Isso significa que você não obtiver o Intellisense para os nomes de membros, suporte a navegação de código (como ir para definição), nem suporte de verificação de tempo de compilação.

    Definir a propriedade ItemType faz com que duas novas variáveis de tipo a ser gerado no escopo das expressões de associação de dados: **Item** e **BindItem**. Você pode usar essas variáveis com rigidez de tipos de expressões de associação de dados e obtenha todos os benefícios da experiência de desenvolvimento do Visual Studio.

    O &quot; **:** &quot; usado na expressão será automaticamente a codificação HTML a saída para evitar problemas de segurança (por exemplo, ataques script entre sites). Esta notação estava disponível desde o .NET 4 para resposta de gravação, mas agora também está disponível em expressões de associação de dados.

    > [!NOTE]
    > O membro de Item funciona para associação unidirecional. Se você deseja executar o uso de associação bidirecional de **BindItem** membro.

    ![Suporte do IntelliSense na associação fortemente tipada](whats-new-in-web-forms-in-aspnet-45/_static/image2.png "suporte do IntelliSense na associação fortemente tipada")

    *Suporte do IntelliSense na associação fortemente tipada*
10. Pressione **F5** para executar a solução e vá para a página de clientes para garantir que as alterações funcionem conforme o esperado.

    ![Listando os detalhes do cliente](whats-new-in-web-forms-in-aspnet-45/_static/image3.png "listando os detalhes do cliente")

    *Listando os detalhes do cliente*
11. Feche o navegador e volte para o Visual Studio.

<a id="Task_2_-_Introducing_Model_Binding_in_Web_Forms"></a>
#### <a name="task-2---introducing-model-binding-in-web-forms"></a>Tarefa 2 - Introdução ao modelo de associação em formulários da Web

Em versões anteriores do Web Forms do ASP.NET, quando você desejava executar associação de dados bidirecional, tanto recuperar e atualizar dados, era necessário usar um objeto de fonte de dados. Isso pode ser um objeto de fonte de dados, uma fonte de dados SQL, uma fonte de dados LINQ e assim por diante. No entanto se seu cenário necessário código personalizado para manipular os dados, você precisa usar o objeto de fonte de dados e isso colocado apresenta algumas desvantagens. Por exemplo, é necessário para evitar tipos complexos e é necessária para lidar com exceções ao executar a lógica de validação.

A nova versão do ASP.NET Web Forms os controles de associação de dados oferecem suporte à associação de modelo. Isso significa que você pode especificar selecionar, atualizar, inserir e excluir métodos diretamente no controle associado a dados para chamar a lógica de seu arquivo de code-behind ou de outra classe.

Para saber mais sobre isso, você usará um GridView para listar as categorias de produto usando o novo **SelectMethod** atributo. Este atributo permite que você especifique um método para recuperar os dados de GridView.

1. Abra o **Products. aspx** página e incluir um **GridView**. Configure o GridView, conforme mostrado a seguir para usar associações fortemente tipada e Habilitar classificação e paginação.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample6.aspx)]
2. Use a nova **SelectMethod** atributo para configurar o GridView para chamar um **GetCategories** método para selecionar os dados.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample7.aspx)]
3. Abra o **Products.aspx.cs** de lógica de arquivo e adicione o seguinte usando instruções.

    (Código de trecho - *Web Forms laboratório - Ex01 - Namespaces*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample8.cs)]
4. Adicionar um membro privado no **produtos** classe e atribuir uma nova instância da **ProductsContext**. Essa propriedade armazena o contexto de dados do Entity Framework que permite que você se conectar ao banco de dados.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample9.cs)]
5. Criar um **GetCategories** método para recuperar a lista de categorias usando LINQ. A consulta incluirá o **produtos** propriedade para que o GridView pode mostrar a quantidade de produtos para cada categoria. Observe que o método retorna um objeto IQueryable bruto que representam a consulta a ser executado posteriormente o ciclo de vida da página.

    (Código de trecho - *Web Forms laboratório - Ex01 - GetCategories*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample10.cs)]

    > [!NOTE]
    > Em versões anteriores do Web Forms do ASP.NET, Habilitar classificação e paginação usando sua própria lógica de repositório dentro de um contexto de objeto de fonte de dados, necessário para gravar seu próprio código personalizado e receber todos os parâmetros necessários. Agora, como os métodos de associação de dados podem retornar IQueryable e isso representa uma consulta ainda para ser executado, ASP.NET pode cuidar de modificar a consulta para adicionar a classificação correta e parâmetros de paginação.
6. Pressione **F5** para iniciar a depuração do site e vá para a página de produtos. Você verá que o GridView é populado com as categorias retornadas pelo método GetCategories.

    ![Preencher um GridView usando associação de modelo](whats-new-in-web-forms-in-aspnet-45/_static/image4.png "preencher um GridView usando associação de modelo")

    *Preencher um GridView usando associação de modelo*
7. Pressione **SHIFT**+**F5** parar a depuração.

<a id="Task_3_-_Value_Providers_in_Model_Binding"></a>
#### <a name="task-3---value-providers-in-model-binding"></a>Tarefa 3: provedores de valor na associação de modelo

Associação de modelo não só permite que você especifique métodos personalizados para trabalhar com seus dados diretamente no controle associado a dados, mas também permite que você mapear os dados da página para parâmetros de métodos. No parâmetro de método, você pode usar atributos de provedor de valor para especificar a fonte de dados do valor. Por exemplo:

- Controles na página
- Valores de cadeia de caracteres de consulta
- Exibir dados
- Estado de sessão
- Cookies
- Dados de postagem de formulário
- Estado de exibição
- Provedores de valor personalizado também têm suporte

Se você tiver usado o ASP.NET MVC 4, você observará que o suporte da associação de modelo é semelhante. Na verdade, esses recursos foram obtidos do ASP.NET MVC e movidos para o **System. Web** assembly para poder usá-los em Web Forms também.

Nesta tarefa, você atualizará o GridView para filtrar os resultados pela quantidade de produtos para cada categoria, recebendo o parâmetro de filtro com associação de modelo.

1. Volte para o **Products. aspx** página.
2. Na parte superior do GridView, adicione um **rótulo** e um **ComboBox** para selecionar o número de produtos para cada categoria, conforme mostrado abaixo.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample11.aspx)]
3. Adicionar uma **EmptyDataTemplate** GridView para mostrar uma mensagem quando não houver nenhuma categoria com o número selecionado de produtos.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample12.aspx)]
4. Abra o **Products.aspx.cs** code-behind e adicione o seguinte usando a instrução.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample13.cs)]
5. Modificar o **GetCategories** método para receber um número inteiro **minProductsCount** argumento e filtrar os resultados retornados. Para fazer isso, substitua o método com o código a seguir.

    (Código de trecho - *Web Forms laboratório - Ex01 - GetCategories 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample14.cs)]

    O novo **[controle]** atributo no **minProductsCount** argumento avisará ASP.NET seu valor deve ser preenchido usando um controle na página. O ASP.NET será procure qualquer controle que corresponde ao nome do argumento (minProductsCount) e executar o mapeamento necessário e a conversão para preencher o parâmetro com o valor do controle.

    Como alternativa, o atributo fornece um construtor sobrecarregado que permite que você especifique o controle de onde obter o valor.

    > [!NOTE]
    > É uma meta dos recursos de associação de dados reduzir a quantidade de código que precisa ser escrito para interação de página. Além de provedor de valor [controle], você pode usar outros provedores de associação de modelos em seus parâmetros de método. Algumas delas são listadas na introdução de tarefa.
6. Pressione **F5** para iniciar a depuração do site e vá para a página de produtos. Selecione um número de produtos na lista suspensa e observe como o GridView está atualizado.

    ![Filtragem de GridView com um valor de lista suspensa](whats-new-in-web-forms-in-aspnet-45/_static/image5.png "filtragem GridView com um valor de lista suspensa")

    *Filtragem de GridView com um valor de lista suspensa*
7. Pare a depuração.

<a id="Task_4_-_Using_Model_Binding_for_Filtering"></a>
#### <a name="task-4---using-model-binding-for-filtering"></a>Tarefa 4 - usando o modelo de associação para filtragem

Nesta tarefa, você irá adicionar um segundo filho GridView para mostrar os produtos na categoria selecionada.

1. Abra o **Products. aspx** página e atualizar as categorias de GridView para gerar automaticamente o botão de seleção.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample15.aspx)]
2. Adicionar um segundo **GridView** chamado **productsGrid** na parte inferior. Definir o **ItemType** para **WebFormsLab.Model.Product**, o **DataKeyNames** para **ProductId** e **SelectMethod**  para **GetProducts**. Definir **AutoGenerateColumns** para **false** e adicione as colunas ProductId, ProductName, descrição e UnitPrice.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample16.aspx)]
3. Abra o **Products.aspx.cs** arquivo code-behind. Implementar o **GetProducts** método para receber a ID da categoria da categoria GridView e filtrar os produtos. Associação de modelo definirá o valor do parâmetro usando a linha selecionada no **categoriesGrid**. Como o nome do argumento e o nome do controle não coincidirem, você deve especificar o nome do controle no provedor de controle de valor.

    (Código de trecho - *Web Forms laboratório - Ex01 - GetProducts*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample17.cs)]

    > [!NOTE]
    > Essa abordagem facilita a unidade esses métodos de teste. Em um contexto de teste de unidade, onde formulários da Web não está em execução, o atributo [controle] não executará qualquer ação específica.
4. Abra o **Products. aspx** página e localize os produtos GridView. Atualize os produtos GridView para mostrar um link para a edição do produto selecionado.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample18.aspx)]
5. Abra o **ProductDetails.aspx** página por trás do código e substitua o **SelectProduct** método com o código a seguir.

    (Código de trecho - *Web Forms laboratório - Ex01 - SelectProduct método*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample19.cs)]

    > [!NOTE]
    > Observe que o **[QueryString]** atributo é usado para preencher o parâmetro do método de um parâmetro de productId na cadeia de caracteres de consulta.
6. Pressione **F5** para iniciar a depuração do site e vá para a página de produtos. Selecione qualquer categoria de categorias de GridView e observe que os produtos GridView é atualizado.

    ![Mostrar produtos da categoria selecionada](whats-new-in-web-forms-in-aspnet-45/_static/image6.png "mostrando produtos da categoria selecionada")

    *Mostrar produtos da categoria selecionada*
7. Clique o **exibição** link em um produto para abrir a página ProductDetails.aspx.

    Observe que a página está recuperando o produto com o SelectMethod usando o parâmetro productId da cadeia de consulta.

    ![Exibindo os detalhes do produto](whats-new-in-web-forms-in-aspnet-45/_static/image7.png "exibindo os detalhes do produto")

    *Exibindo os detalhes do produto*

    > [!NOTE]
    > A capacidade de digitar uma descrição de HTML será implementada no próximo exercício.

<a id="Task_5_-_Using_Model_Binding_for_Update_Operations"></a>
#### <a name="task-5---using-model-binding-for-update-operations"></a>Tarefa 5 - usando o modelo de associação para operações de atualização

Na tarefa anterior, você usou a associação de modelo principalmente para selecionar os dados, essa tarefa, você aprenderá como usar a associação de modelo em operações de atualização.

Você atualizará as categorias de GridView para permitir que o usuário categorias de atualização.

1. Abra o **Products. aspx** página e atualizar as categorias de GridView para gerar automaticamente o botão Editar e usar o novo **UpdateMethod** atributo para especificar um **UpdateCategory não**método para atualizar o item selecionado.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample20.aspx)]

    O atributo DataKeyNames em GridView definir quais são os membros que identificam exclusivamente o objeto de modelo associado e, portanto, quais são os parâmetros que do método update deve receber pelo menos.
2. Abra o **Products.aspx.cs** arquivo code-behind e implementar o **UpdateCategory não** método. O método deve receber a ID da categoria para carregar a categoria, preencha os valores de GridView e, em seguida, atualizar a categoria.

    (Código de trecho - *Web UpdateCategory de laboratório - Ex01 - não formulários*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample21.cs)]

    O novo **TryUpdateModel** método na classe de página é responsável de preencher o objeto de modelo usando os valores dos controles na página. Nesse caso, ele substituirá os valores atualizados da linha atual de GridView está sendo editado para o **categoria** objeto.

    > [!NOTE]
    > O próximo exercício explica o uso de ModelState para validar os dados inseridos pelo usuário ao editar o objeto.
3. Executar o site e vá para a página de produtos. Edite uma categoria. Digite um novo nome e, em seguida, clique em **atualização** para manter as alterações.

    ![Editando categorias](whats-new-in-web-forms-in-aspnet-45/_static/image8.png "editando categorias")

    *Editando categorias*

<a id="Exercise2"></a>

<a id="Exercise_2_Data_Validation"></a>
### <a name="exercise-2-data-validation"></a>Exercício 2: Validação de dados

Neste exercício, você aprenderá sobre os novos recursos de validação de dados no ASP.NET 4.5. Você verificará os novos recursos de validação discreto em formulários da Web. Você usará as anotações de dados nas classes de modelo de aplicativo para validação de entrada do usuário e, finalmente, você aprenderá como ativar ou desativar a validação de solicitação para os controles individuais em uma página.

<a id="Task_1_-_Unobtrusive_Validation"></a>
#### <a name="task-1---unobtrusive-validation"></a>Tarefa 1 - validação discreto

Formulários com dados complexos, incluindo validadores tendem a gerar muitos códigos JavaScript na página, que pode representar aproximadamente 60% do código. Com a validação discreta habilitada, o código HTML ficará mais limpa e mais organizada.

Nesta seção, você permitirá a validação discreta no ASP.NET para comparar o código HTML gerado por ambas as configurações.

1. Abra **Visual Studio 2012** e abra o **começar** solução localizada no **Source\Ex2 Validation\Begin** pasta deste laboratório. Como alternativa, você pode continuar trabalhando em sua solução existente do exercício anterior.

   1. Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar. Para fazer isso, no Gerenciador de soluções, clique o **WebFormsLab** projeto **gerenciar pacotes NuGet**.
   2. No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, compile a solução clicando **criar** | **compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto. É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.
2. Pressione **F5** para iniciar o aplicativo web. Vá para os clientes de página e clique no **adicionar um novo cliente** link.
3. Na página do navegador e selecione **Exibir código-fonte** opção para abrir o código HTML gerado pelo aplicativo.

    ![Mostrando a página de código HTML](whats-new-in-web-forms-in-aspnet-45/_static/image9.png "mostrando a página de código HTML")

    *Mostrando a página de código HTML*
4. Percorra o código de origem da página e observe que o ASP.NET tem injetado validadores de dados e o código JavaScript na página para executar validações e exibir a lista de erros.

    ![Código JavaScript de validação na página CustomerDetails](whats-new-in-web-forms-in-aspnet-45/_static/image10.png "código JavaScript de validação na página CustomerDetails")

    *Código JavaScript de validação na página CustomerDetails*
5. Feche o navegador e volte para o Visual Studio.
6. Agora, você habilitará validação discreta. Abra **Web. config** e localize **ValidationSettings:UnobtrusiveValidationMode** chave no **AppSettings** seção **.** Defina o valor da chave como **WebForms**.

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample22.xml)]

    > [!NOTE]
    > Você também pode definir essa propriedade &quot; **página\_carga** &quot; evento caso você deseja habilitar a validação discreto somente para algumas páginas.
7. Abra **CustomerDetails.aspx** e pressione **F5** para iniciar o aplicativo Web.
8. Pressione a tecla F12 para abrir as ferramentas de desenvolvedor do IE. Quando as ferramentas de desenvolvedor é aberto, selecione a guia de script. Selecione **CustomerDetails.aspx** no menu e take Observe que os scripts necessários para executar jQuery em página tiverem sido carregados no navegador do site local.

    ![Carregar o jQuery JavaScript arquivos diretamente do servidor IIS local](whats-new-in-web-forms-in-aspnet-45/_static/image11.png "carregar o jQuery JavaScript arquivos diretamente do servidor IIS local")

    *Carregando os arquivos do JavaScript jQuery diretamente do servidor IIS local*
9. Feche o navegador para retornar ao Visual Studio. Abra o **Site.Master** novamente e localize o **ScriptManager**. Adicione o atributo **EnableCdn** propriedade com o valor **True**. Isso forçará o jQuery para ser carregado da URL online, e não da URL do site local.
10. Abra **CustomerDetails.aspx** no Visual Studio. Pressione a tecla F5 para executar o site. Depois que o Internet Explorer é aberta, pressione a tecla F12 para abrir as ferramentas de desenvolvedor. Selecione o **Script** guia e, em seguida, examine a lista suspensa. Observe que os arquivos do JavaScript jQuery não estão sendo carregados do site local, mas a partir de online jQuery CDN.

    ![Carregar o jQuery JavaScript arquivos da CDN](whats-new-in-web-forms-in-aspnet-45/_static/image12.png "carregar o jQuery JavaScript arquivos da CDN")

    *Carregando os arquivos do JavaScript jQuery da CDN*
11. Abra o código-fonte HTML página novamente usando a opção de fonte de exibição no navegador. Observe que, permitindo que a validação discreta ASP.NET substituiu o código JavaScript injetado com dados \*atributos.

    ![Código de validação discreto](whats-new-in-web-forms-in-aspnet-45/_static/image13.png "código de validação discreto")

    *Código de validação discreto*

    > [!NOTE]
    > Neste exemplo, você viu como um resumo com anotações de dados de validação foi simplificado para apenas algumas HTML e JavaScript linhas. Anteriormente, sem validação discreta, os controles de validação mais adicionar, quanto maior o código de validação de JavaScript aumentam.

<a id="Task_2_-_Validating_the_Model_with_Data_Annotations"></a>
#### <a name="task-2---validating-the-model-with-data-annotations"></a>Tarefa 2: validar o modelo com anotações de dados

ASP.NET 4.5 apresenta validação de anotações de dados para formulários da Web. Em vez de ter um controle de validação em cada entrada, você pode definir restrições em suas classes de modelo e usá-los em todos os seu aplicativo da web. Nesta seção, você aprenderá como usar anotações de dados para validação de um formulário de cliente do novo/editar.

1. Abra **CustomerDetail.aspx** página. Observe que o cliente primeiro nome e o segundo nome no **EditItemTemplate** e **InsertItemTemplate** seções são validadas usando um controle RequiredFieldValidator. Cada validador está associado a uma determinada condição, portanto você precisa incluir validadores tantos como condições para verificar.
2. Adicione anotações de dados para validar a classe de modelo de cliente. Abra **Customer.cs** classe no **modelo** pasta e *decorar* cada propriedade usando atributos de anotação de dados.

    (Código de trecho - *Web Forms anotações de dados de laboratório - Ex02 -*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample23.cs)]

    > [!NOTE]
    > .NET framework 4.5 estendeu a coleção de anotação de dados existente. Estas são algumas das anotações de dados que você pode usar: [CreditCard], [Phone], [EmailAddress], [intervalo], [comparar], [Url], [FileExtensions], [Required], [chave], [RegularExpression].
    > 
    > Alguns exemplos de uso:
    > 
    > [chave]: Specifies that an attribute is the unique identifier
    > 
    > [Range(0.4, 0.5, ErrorMessage=&quot;{Write an error message}&quot;]: Double range
    > 
    > [EmailAddress(ErrorMessage=&quot;Invalid Email&quot;), MaxLength(56)]: Two annotations in the same line.
    > 
    > Você também pode definir suas próprias mensagens de erro dentro de cada atributo.
3. Abra **CustomerDetails.aspx** e remova todos os RequiredFieldvalidators para os campos nome e sobrenome nas seções EditItemTemplate e InsertItemTemplate do controle FormView.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample24.aspx)]

    > [!NOTE]
    > Uma vantagem de usar as anotações de dados é que a lógica de validação não sejam duplicada nas suas páginas de aplicativo. Você defini-lo de uma vez no modelo e usá-lo em todas as páginas de aplicativo que manipulam dados.
4. Abra **CustomerDetails.aspx** code-behind e localize o método SaveCustomer. Esse método é chamado quando inserir um novo cliente e recebe o parâmetro de cliente entre os valores de controle de FormView. Quando o mapeamento entre os controles da página e o objeto de parâmetro ocorre quando o ASP.NET executará a validação do modelo em relação a anotação de dados de atributos e preencher o dicionário ModelState com os erros encontrados, se houver.

    O ModelState só retornará true se todos os campos no modelo são válidos após a execução da validação.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample25.cs)]
5. Adicionar um **ValidationSummary** controle no final da página CustomerDetails para mostrar a lista de erros do modelo.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample26.aspx)]

    O **ShowModelStateErrors** é uma nova propriedade no ValidationSummary controle que, quando definido como **true**, o controle mostrará os erros do dicionário ModelState. Esses erros são provenientes de validação de anotações de dados.
6. Pressione **F5** para executar o aplicativo Web. Preencha o formulário com alguns valores incorretos e clique em **salvar** para executar a validação. Observe o erro resumo na parte inferior.

    ![Validação com anotações de dados](whats-new-in-web-forms-in-aspnet-45/_static/image14.png "validação com anotações de dados")

    *Validação com anotações de dados*

<a id="Task_3_-_Handling_Custom_Database_Errors_with_ModelState"></a>
#### <a name="task-3---handling-custom-database-errors-with-modelstate"></a>Tarefa 3 - tratamento de erros de banco de dados personalizado com ModelState

Na versão anterior do Web Forms, tratamento de erros de banco de dados como uma cadeia de caracteres muito longa ou uma violação de chave exclusiva pode envolver Lançando exceções no código do repositório e, em seguida, tratamento de exceções em seu code-behind para exibir um erro. Uma grande quantidade de código é necessária para fazer algo relativamente simples.

No Web Forms 4.5, o objeto de ModelState pode ser usado para exibir os erros na página do seu modelo ou do banco de dados, de maneira consistente.

Nesta tarefa, você adicionará código para tratar exceções de banco de dados e mostrar a mensagem apropriada para o usuário usando o objeto ModelState corretamente.

1. Enquanto o aplicativo ainda está em execução, tente atualizar o nome de uma categoria de usando um valor duplicado.

    ![Atualizando uma categoria com um nome duplicado](whats-new-in-web-forms-in-aspnet-45/_static/image15.png "atualizando uma categoria com um nome duplicado")

    *Atualizando uma categoria com um nome duplicado*

    Observe que uma exceção será lançada devido ao &quot;exclusivo&quot; restrição a **CategoryName** coluna.

    ![A exceção para nomes de categoria duplicado](whats-new-in-web-forms-in-aspnet-45/_static/image16.png "exceção para nomes de categoria duplicado")

    *Exceção para nomes de categoria duplicado*
2. Pare a depuração. No **Products.aspx.cs** arquivo code-behind, atualizar o **UpdateCategory não** método para lidar com as exceções geradas pelo banco de dados. Método SaveChanges () chamar e adicionar um erro para o **ModelState** objeto.

    O novo **TryUpdateModel** método atualiza o objeto da categoria recuperado do banco de dados usando os dados do formulário fornecidos pelo usuário.

    (Código de trecho - *Web Forms laboratório - Ex02 - UpdateCategory não identificador erros*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample27.cs)]

    > [!NOTE]
    > Idealmente, você precisa identificar a causa do DbUpdateException e verifique se a causa raiz é a violação de restrição de chave exclusiva.
3. Abra **Products. aspx** e adicione um **ValidationSummary** controle abaixo as categorias de GridView para mostrar a lista de erros de modelo.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample28.aspx)]
4. Executar o site e vá para a página de produtos. Tente atualizar o nome de uma categoria de usando um valor duplicado.

    Observe que a exceção foi tratada e a mensagem de erro aparece no **ValidationSummary** controle.

    ![Duplicado categoria erro](whats-new-in-web-forms-in-aspnet-45/_static/image17.png "duplicado categoria erro")

    *Erro de categoria duplicado*

<a id="Task_4_-_Request_Validation_in_ASPNET_Web_Forms_45"></a>
#### <a name="task-4---request-validation-in-aspnet-web-forms-45"></a>Tarefa 4 – solicitação de validação em Web Forms do ASP.NET 4.5

O recurso de validação de solicitação do ASP.NET fornece um certo nível de proteção padrão contra ataques de script entre sites (XSS). Nas versões anteriores do ASP.NET, a validação de solicitação era habilitada por padrão e só pode ser desabilitada para uma página inteira. Com a nova versão do ASP.NET Web Forms pode agora desabilitar a validação de solicitação para um único controle, executar a validação de solicitação lenta ou acessar os dados de solicitação não validadas (cuidado se você fizer isso!).

1. Pressione **Ctrl + F5** para iniciar o site sem depuração e vá para a página de produtos. Selecione uma categoria e, em seguida, clique no **editar** link em qualquer um dos produtos.
2. Digite uma descrição que contém o conteúdo potencialmente perigoso, por exemplo, inclusive marcas HTML. Observar a exceção gerada devido à validação de solicitação.

    ![Edição de um produto com conteúdo potencialmente perigoso](whats-new-in-web-forms-in-aspnet-45/_static/image18.png "edição de um produto com conteúdo potencialmente perigoso")

    *Edição de um produto com conteúdo potencialmente perigoso*

    ![Exceção lançada devido à validação de solicitação](whats-new-in-web-forms-in-aspnet-45/_static/image19.png "exceção devido à validação de solicitação")

    *Exceção lançada devido à validação de solicitação*
3. Feche a página e, no Visual Studio, pressione **SHIFT + F5** para parar a depuração.
4. Abra o **ProductDetails.aspx** página e localize o **descrição** caixa de texto.
5. Adicionar o novo **ValidateRequestMode** propriedade para a caixa de texto e defina seu valor como **desabilitado**.

    O novo **ValidateRequestMode** atributo permite que você desabilite a validação da solicitação forma granular em cada controle. Isso é útil quando você deseja usar uma entrada que pode receber o código HTML, mas deseja manter a validação de trabalho para o restante da página.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample29.aspx)]
6. Pressione **F5** para executar o aplicativo web. Abra a página Editar novamente e concluir uma descrição de produto, incluindo marcas HTML. Observe que agora você pode adicionar conteúdo HTML a descrição.

    ![Desabilitado para a descrição do produto de validação de solicitação](whats-new-in-web-forms-in-aspnet-45/_static/image20.png "desabilitada para a descrição do produto de validação de solicitação")

    *Validação de solicitação desabilitada para a descrição do produto*

    > [!NOTE]
    > Em um aplicativo de produção, você deve limpar o código HTML inserido pelo usuário para garantir seguras somente marcas HTML são inseridas (por exemplo, há nenhum &lt;script&gt; marcas). Para fazer isso, você pode usar [biblioteca de proteção do Microsoft Web](https://www.nuget.org/packages/AntiXSS).
7. Edite o produto novamente. Digite o código HTML no campo nome e clique em **salvar**. Observe que a validação de solicitação é desabilitada somente para o campo de descrição e o restante dos campos re ainda validada em relação ao conteúdo potencialmente perigoso.

    ![Validação habilitada no restante dos campos da solicitação](whats-new-in-web-forms-in-aspnet-45/_static/image21.png "habilitada no restante dos campos de validação de solicitação")

    *Validação de solicitação habilitada no restante dos campos*

    Web Forms do ASP.NET 4.5 inclui um novo modo de validação de solicitação para executar a validação de solicitação lentamente. Com o modo de validação de solicitação definido como **4.5**, se um pedaço de código acessa *Request [&quot;chave&quot;]*, validação de solicitação do disparador de será de validação somente solicitação do ASP.NET 4.5 para esse elemento específico na coleção de formulário.

    Além disso, o ASP.NET 4.5 agora inclui rotinas de codificação de núcleo do v 4.0 biblioteca Anti-XSS de Microsoft. A codificação rotinas são implementadas pelo novo Anti-XSS *AntiXssEncoder* tipo encontrado no novo **System.Web.Security.AntiXss** namespace. Com o **encoderType** parâmetro configurado para usar *AntiXssEncoder*, saída automaticamente a codificação no ASP.NET usa as rotinas de codificação de novo.
8. A validação do ASP.NET 4.5 de solicitação também oferece suporte a acesso não validado para solicitar dados. ASP.NET 4.5 adiciona uma nova propriedade de coleção para o **HttpRequest** objeto chamado **Unvalidated**. Quando você navega em **HttpRequest.Unvalidated** você tem acesso a todas as partes comuns de dados de solicitação, incluindo QueryStrings, formulários, Cookies, URLs e assim por diante.

    ![Objeto Request.Unvalidated](whats-new-in-web-forms-in-aspnet-45/_static/image22.png "Request.Unvalidated objeto")

    *Objeto Request.Unvalidated*

    > [!NOTE]
    > **Use a propriedade HttpRequest.Unvalidated com cuidado!** Verifique se que você executar cuidadosamente a validação personalizada nos dados brutos de solicitação para garantir que o texto perigoso é recuperado e processado novamente para que clientes não!

<a id="Exercise3"></a>

<a id="Exercise_3_Asynchronous_Page_Processing_in_ASPNET_Web_Forms"></a>
### <a name="exercise-3-asynchronous-page-processing-in-aspnet-web-forms"></a>Exercício 3: Página assíncrona de processamento no ASP.NET Web Forms

Neste exercício, você será apresentado para a nova página assíncrona processamento recursos no Web Forms do ASP.NET.

<a id="Task_1_-_Updating_the_Product_Details_Page_to_Upload_and_Show_Images"></a>
#### <a name="task-1---updating-the-product-details-page-to-upload-and-show-images"></a>Tarefa 1: atualização do produto detalhes para carregar e exibir imagens

Nesta tarefa, você atualizará a página de detalhes do produto para permitir que o usuário especifique uma URL de imagem para o produto e exibi-lo no modo de exibição somente leitura. Você criará uma cópia local da imagem especificada, baixando-o de forma síncrona. A próxima tarefa, você irá atualizar essa implementação para torná-lo a trabalhar de forma assíncrona.

1. Abra **Visual Studio 2012** e carregar o **começar** solução localizada no **Source\Ex3 Async\Begin** da pasta neste laboratório. Como alternativa, você pode continuar trabalhando em sua solução existente de exercícios anteriores.

   1. Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar. Para fazer isso, no Gerenciador de soluções, clique o **WebFormsLab** do projeto e selecione **gerenciar pacotes NuGet**.
   2. No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, compile a solução clicando **criar** | **compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto. É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.
2. Abra o **ProductDetails.aspx** página fonte e adicione um campo ItemTemplate de FormView para mostrar a imagem do produto.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample30.aspx)]
3. Adicione um campo para especificar a URL da imagem na EditTemplate de FormView.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample31.aspx)]
4. Abra o **ProductDetails.aspx.cs** de lógica de arquivo e adicione as seguintes diretivas de namespace.

    (Código de trecho - *Web Forms Namespaces de laboratório - Ex03 -*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample32.cs)]
5. Criar um **UpdateProductImage** método para armazenar imagens remotas no local **imagens** pasta e atualizar a entidade de produto com o novo valor de local de imagem.

    (Código de trecho - *Web Forms laboratório - Ex03 - UpdateProductImage*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample33.cs)]
6. Atualização de **UpdateProduct** método para chamar o **UpdateProductImage** método.

    (Código de trecho - *Web Forms laboratório - Ex03 - UpdateProductImage chamada*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample34.cs)]
7. Execute o aplicativo e tente carregar uma imagem de um produto. Por exemplo, você pode usar a seguinte URL de imagem de arte de clipe do Office: [[http://officeimg.vo.msecnd.net/images/MB900437099.jpg](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)

    ![Definindo uma imagem de um produto](whats-new-in-web-forms-in-aspnet-45/_static/image23.png "definindo uma imagem de um produto")

    *Definindo uma imagem de um produto*

<a id="Task_2_-_Adding_Asynchronous_Processing_to_the_Product_Details_Page"></a>
#### <a name="task-2---adding-asynchronous-processing-to-the-product-details-page"></a>Tarefa 2: adicionando assíncrona de processamento para a página de detalhes do produto

Nesta tarefa, você atualizará a página de detalhes do produto para torná-lo a trabalhar de forma assíncrona. Você aumentará uma tarefa demorada - o processo de download da imagem - usando o processamento de página assíncrona do ASP.NET 4.5.

Métodos assíncronos em aplicativos da web podem ser usados para otimizar a forma como os pools de threads do ASP.NET são usados. No ASP.NET existe são solicitações de um número limitado de threads no pool de threads para participar, assim, quando todos os threads estão ocupados, ASP.NET começa a rejeitar novas solicitações, envia mensagens de erro do aplicativo e faz com que seu site não está disponível.

Operações demoradas em seu site da web são ótimos candidatos para programação assíncrona porque eles ocupam o thread atribuído por um longo tempo. Isso inclui solicitações de longa execução, páginas com muitos elementos diferentes e páginas que requerem operações off-line, tais consultar um banco de dados ou acessar um servidor web externo. A vantagem é que, se você usar métodos assíncronos para essas operações, enquanto a página está sendo processada, o thread é liberado e retornado para o thread do pool e pode ser usado para participar de uma nova solicitação de página. Isso significa que, a página será iniciado em um thread do pool de threads de processamento e processamento em um diferente, pode ser concluída após o processamento assíncrono.

1. Abra o **ProductDetails.aspx** página. Adicionar o **Async** atributo o **página** elemento e defina-a como **true**. Esse atributo diz ao ASP.NET para implementar a interface IHttpAsyncHandler.


~~~
[!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample35.aspx)]
~~~
2. Adicione um rótulo na parte inferior da página para mostrar os detalhes dos threads de execução da página.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample36.aspx)]
3. Abra **ProductDetails.aspx.cs** e adicione as seguintes diretivas de namespace.

    (Código de trecho - *Web Forms laboratório - Ex03 - Namespaces 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample37.cs)]
4. Modificar o **UpdateProductImage** método para baixar a imagem com uma tarefa assíncrona. Substitua o **WebClient** **DownloadFile** método com o **DownloadFileTaskAsync** método e incluir o **await** palavra-chave.

    (Código de trecho - *Web Forms laboratório - Ex03 - UpdateProductImage Async*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample38.cs)]

    O RegisterAsyncTask registra uma nova tarefa assíncrona de página a ser executado em um thread diferente. Ele recebe uma expressão lambda com a tarefa (t) a ser executado. O **await** palavra-chave no **DownloadFileTaskAsync** método converte o restante do método em um retorno de chamada é invocado de forma assíncrona depois do **DownloadFileTaskAsync** método foi concluída. ASP.NET continuará a execução do método mantendo automaticamente todos os HTTP solicitação valores originais. O novo modelo de programação assíncrono do .NET 4.5 permite escrever código assíncrono é muito parecido com o código síncrono e deixar que o compilador tratar complicações das funções de retorno de chamada ou código de continuação.
    > [!NOTE]
    > RegisterAsyncTask e PageAsyncTask já estavam disponível desde o .NET 2.0. A palavra-chave await é nova do modelo de programação assíncrona do .NET 4.5 e pode ser usada junto com os novos métodos TaskAsync do objeto .NET WebClient.
5. Adicione código para exibir os threads nos quais o código iniciado e concluído a execução. Para fazer isso, atualize o **UpdateProductImage** método com o código a seguir.

    (Código de trecho - *Web Forms laboratório - Ex03 - Mostrar threads*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample39.cs)]
6. Abra o site **Web. config** arquivo. Adicione a seguinte variável appSetting.

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample40.xml)]
7. Pressione **F5** para executar o aplicativo e carregue uma imagem do produto. Observe a ID de threads em que o código iniciado e concluído pode ser diferente. Isso ocorre porque a execução de tarefas assíncronas em um thread separado do pool de threads do ASP.NET. Quando a tarefa é concluída, o ASP.NET coloca a tarefa novamente na fila e atribui a qualquer um dos threads disponíveis.

    ![Baixar uma imagem de forma assíncrona](whats-new-in-web-forms-in-aspnet-45/_static/image24.png "baixar uma imagem de forma assíncrona")

    *Baixar uma imagem de forma assíncrona*

> [!NOTE]
> Além disso, você pode implantar esse aplicativo do Azure seguinte [apêndice b: publicação um aplicativo ASP.NET MVC 4 usando a implantação da Web](#AppendixB).


* * *

<a id="Summary"></a>
## <a name="summary"></a>Resumo

Neste laboratório prático, os conceitos a seguir foram resolvidos e demonstrados:

- Usar expressões de associação de dados fortemente tipados
- Usar novos recursos de associação de modelo no Web Forms
- Usar provedores de valor para mapear os dados da página para métodos de lógica
- Usar anotações de dados para validação de entrada do usuário
- Demorar advange de validação do lado do cliente unobstrusive com jQuery nos formulários da Web
- Implementar a validação de solicitação granular
- Implementar o processamento em formulários da Web da página assíncrona

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apêndice a: instalar o Visual Studio Express 2012 para Web

Você pode instalar **Microsoft Visual Studio Express 2012 para Web** ou outro &quot;Express&quot; versão usando o **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. As instruções a seguir guiá-lo pelas etapas necessárias para instalar o *Visual studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.

1. Vá para [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Como alternativa, se você já tiver instalado o Web Platform Installer, você pode abri-la e procure o produto &quot; <em>Visual Studio Express 2012 para Web com SDK do Azure</em>&quot;.
2. Clique em **instalar agora**. Se você não tem **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.
3. Uma vez **Web Platform Installer** é aberto, clique em **instalar** para iniciar a instalação.

    ![Instalar Visual Studio Express](whats-new-in-web-forms-in-aspnet-45/_static/image25.png "instalar Visual Studio Express")

    *Instalar Visual Studio Express*
4. Leia os termos e licenças de todos os produtos e clique em **aceito** para continuar.

    ![Aceitar os termos de licença](whats-new-in-web-forms-in-aspnet-45/_static/image26.png)

    *Aceitar os termos de licença*
5. Aguarde até que o processo de download e instalação seja concluída.

    ![Progresso da instalação](whats-new-in-web-forms-in-aspnet-45/_static/image27.png)

    *Progresso da instalação*
6. Quando a instalação for concluída, clique em **concluir**.

    ![Instalação concluída](whats-new-in-web-forms-in-aspnet-45/_static/image28.png)

    *Instalação concluída*
7. Clique em **saída** para fechar o Web Platform Installer.
8. Para abrir o Visual Studio Express para Web, vá para o **iniciar** tela e comece a escrever &quot; **VS Express**&quot;, em seguida, clique no **VS Express para Web** lado a lado.

    ![VS Express para o bloco de Web](whats-new-in-web-forms-in-aspnet-45/_static/image29.png)

    *VS Express para o bloco de Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Apêndice b: publicar um aplicativo ASP.NET MVC 4 usando a implantação da Web

Este apêndice mostram como criar um novo site do Portal do Azure e publicar o aplicativo que você obteve seguindo o laboratório, aproveitando o recurso de publicação de implantação da Web fornecido pelo Azure.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>Tarefa 1: criar um novo Site do Portal do Azure

1. Vá para o [Portal de gerenciamento](https://manage.windowsazure.com/) e entre usando as credenciais da Microsoft associadas à sua assinatura.

    > [!NOTE]
    > Com o Azure, você pode hospedar 10 Sites da Web ASP.NET gratuitamente e, em seguida, dimensione conforme seu tráfego cresce. Você pode inscrever [aqui](http://aka.ms/aspnet-hol-azure).

    ![Faça logon no portal do Windows Azure](whats-new-in-web-forms-in-aspnet-45/_static/image30.png "faça logon no portal do Windows Azure")

    *Faça logon no Portal do*
2. Clique em **novo** na barra de comandos.

    ![Criando um novo Site](whats-new-in-web-forms-in-aspnet-45/_static/image31.png "criar um novo Site")

    *Criando um novo Site*
3. Clique em **de computação** | **Site da Web**. Em seguida, selecione **criação rápida** opção. Forneça uma URL disponível para o novo site e clique em **Criar Site**.

    > [!NOTE]
    > O Azure é o host para um aplicativo web em execução na nuvem que você pode controlar e gerenciar. A opção criação rápida permite que você implante um aplicativo web concluído no Azure de fora do portal. Ele não inclui etapas para configurar um banco de dados.

    ![Criando um novo Site usando a criação rápida](whats-new-in-web-forms-in-aspnet-45/_static/image32.png "criar um novo Site usando a criação rápida")

    *Criando um novo Site usando a criação rápida*
4. Aguarde até que o novo **Site da Web** é criado.
5. Depois que o Site da Web é criado, clique no link sob a **URL** coluna. Verifique se o novo Site da Web está funcionando.

    ![Navegando para o novo site](whats-new-in-web-forms-in-aspnet-45/_static/image33.png "navegando para o novo site")

    *Navegando para o novo site*

    ![Site da Web em execução](whats-new-in-web-forms-in-aspnet-45/_static/image34.png "site em execução")

    *Site da Web em execução*
6. Acesse o portal e clique no nome do site sob o **nome** coluna para exibir as páginas de gerenciamento.

    ![Abrir as páginas de gerenciamento do site da web](whats-new-in-web-forms-in-aspnet-45/_static/image35.png "abrir páginas de gerenciamento do site")

    *Abrir as páginas de gerenciamento do Site da Web*
7. No **painel** página no **visão rápida** seção, clique o **baixar perfil de publicação** link.

    > [!NOTE]
    > O *perfil de publicação* contém todas as informações necessárias para publicar um aplicativo web no Azure para cada método de publicação habilitado. O perfil de publicação contém as URLs, as credenciais do usuário e cadeias de caracteres de banco de dados necessárias para se conectar e autenticar cada um dos pontos de extremidade para o qual um método de publicação está habilitado. **Microsoft WebMatrix 2**, **Microsoft Visual Studio para Web Express** e **Microsoft Visual Studio 2012** oferecem suporte à leitura perfis de publicação para automatizar a configuração desses programas para Publicando aplicativos web no Azure.

    ![Baixando o site da web de perfil de publicação](whats-new-in-web-forms-in-aspnet-45/_static/image36.png "baixando o site da web de perfil de publicação")

    *Baixando o Site da Web de perfil de publicação*
8. Baixe o arquivo de perfil de publicação para um local conhecido. Mais neste exercício, você verá como usar esse arquivo para publicar um aplicativo web no Azure do Visual Studio.

    ![Salvando o arquivo de perfil de publicação](whats-new-in-web-forms-in-aspnet-45/_static/image37.png "salvar o perfil de publicação")

    *Salvando o arquivo de perfil de publicação*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tarefa 2 – Configurando o servidor de banco de dados

Se seu aplicativo utiliza o SQL Server bancos de dados que você precisará criar um servidor de banco de dados SQL. Se você deseja implantar um aplicativo simples que não usa o SQL Server, você pode ignorar esta tarefa.

1. Você precisará de um servidor de banco de dados SQL para armazenar o banco de dados do aplicativo. Você pode exibir os servidores de banco de dados SQL de sua assinatura no portal de gerenciamento do Azure em **bancos de dados Sql** | **servidores** | **painel do servidor**. Se você não tiver um servidor criado, você pode criar um usando o **adicionar** botão na barra de comandos. Anote o **nome do servidor e o URL, o nome de logon de administrador e senha**, pois você usará nas próximas tarefas. Não crie o banco de dados, como ele será criado em um estágio posterior.

    ![Painel do servidor de banco de dados SQL](whats-new-in-web-forms-in-aspnet-45/_static/image38.png "painel de banco de dados do SQL Server")

    *Painel de banco de dados do SQL Server*
2. A próxima tarefa, você testará a conexão de banco de dados do Visual Studio, por esse motivo, você precisa incluir seu endereço IP local na lista do servidor de **endereços IP permitidos**. Para fazer isso, clique em **configurar**, selecione o endereço IP do **endereço IP do cliente atual** e cole-o no **endereço IP inicial** e **oendereçoIPfinal** caixas de texto e clique no ![add-client-ip-address-ok-button](whats-new-in-web-forms-in-aspnet-45/_static/image39.png) botão.

    ![Adicionar endereço IP do cliente](whats-new-in-web-forms-in-aspnet-45/_static/image40.png)

    *Adicionar endereço IP do cliente*
3. Uma vez o **endereço IP do cliente** é adicionado para os endereços IP permitidos, clique em **salvar** para confirmar as alterações.

    ![Confirmar alterações](whats-new-in-web-forms-in-aspnet-45/_static/image41.png)

    *Confirmar alterações*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tarefa 3: publicar um aplicativo ASP.NET MVC 4 usando a implantação da Web

1. Volte para a solução do ASP.NET MVC 4. No **Solution Explorer**, com o botão direito no projeto de site da web e selecione **publicar**.

    ![Publicar o aplicativo](whats-new-in-web-forms-in-aspnet-45/_static/image42.png "a publicação do aplicativo")

    *Publicar o site*
2. Importe o perfil de publicação que você salvou na primeira tarefa.

    ![Importar o perfil de publicação](whats-new-in-web-forms-in-aspnet-45/_static/image43.png "importar o perfil de publicação")

    *Importar o perfil de publicação*
3. Clique em **validar Conexão**. Quando a validação estiver concluída, clique em **próximo**.

    > [!NOTE]
    > A validação é concluída depois que aparecer uma marca de seleção verde ao lado do botão Validar Conexão.

    ![Validando a conexão](whats-new-in-web-forms-in-aspnet-45/_static/image44.png "validação de conexão")

    *Validação de conexão*
4. No **configurações** página no **bancos de dados** seção, clique no botão ao lado da caixa de texto da sua conexão de banco de dados (ou seja, **DefaultConnection**).

    ![Configuração de implantação da Web](whats-new-in-web-forms-in-aspnet-45/_static/image45.png "configuração de implantação da Web")

    *Configuração de implantação da Web*
5. Configure a conexão de banco de dados da seguinte maneira:

   - No **nome do servidor** digite sua URL de servidor de banco de dados SQL usando o *tcp:* prefixo.
   - Em **nome de usuário** digite seu nome de logon de administrador do servidor.
   - Em **senha** digite sua senha de logon de administrador do servidor.
   - Digite um novo nome de banco de dados.

     ![Configurando a cadeia de caracteres de conexão de destino](whats-new-in-web-forms-in-aspnet-45/_static/image46.png "Configurando a cadeia de caracteres de conexão de destino")

     *Configurando a cadeia de caracteres de conexão de destino*
6. Clique em **OK**. Quando solicitado a criar o banco de dados, clique em **Sim**.

    ![Criando o banco de dados](whats-new-in-web-forms-in-aspnet-45/_static/image47.png "criar a cadeia de caracteres do banco de dados")

    *Criando o banco de dados*
7. A cadeia de caracteres de conexão que você usará para se conectar ao banco de dados SQL no Azure é mostrada na caixa de texto de Conexão padrão. Clique em **Avançar**.

    ![Cadeia de caracteres de Conexão que aponta para o banco de dados SQL](whats-new-in-web-forms-in-aspnet-45/_static/image48.png "cadeia de caracteres de Conexão que aponta para o banco de dados SQL")

    *Cadeia de caracteres de Conexão que aponta para o banco de dados SQL*
8. No **visualização** , clique em **publicar**.

    ![Publicar o aplicativo web](whats-new-in-web-forms-in-aspnet-45/_static/image49.png "publicar o aplicativo web")

    *Publicar o aplicativo web*
9. Quando termina o processo de publicação, o navegador padrão abrirá o site da web publicado.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Apêndice c: usar trechos de código

Com trechos de código, você tem todo o código que é necessário ao seu alcance. O documento de laboratório informará exatamente quando você pode usá-los, conforme mostrado na figura a seguir.

![Usando trechos de código do Visual Studio para inserir o código no seu projeto](whats-new-in-web-forms-in-aspnet-45/_static/image50.png "trechos de código com o Visual Studio para inserir código em seu projeto")

*Usando trechos de código do Visual Studio para inserir código em seu projeto*

***Para adicionar um trecho de código usando o teclado (apenas c#)***

1. Posicione o cursor onde você deseja inserir o código.
2. Comece a digitar o nome do trecho (sem espaços ou hifens).
3. Observe como IntelliSense exibe os nomes dos trechos de código correspondentes.
4. Selecione o trecho correto (ou continue digitando até que o nome do trecho de código inteiro é selecionado).
5. Pressione a tecla Tab duas vezes para inserir o trecho de código no local do cursor.

![Comece a digitar o nome do fragmento](whats-new-in-web-forms-in-aspnet-45/_static/image51.png "comece a digitar o nome do trecho de código")

*Comece a digitar o nome do trecho de código*

![Pressione Tab para selecionar o trecho de código realçado](whats-new-in-web-forms-in-aspnet-45/_static/image52.png "pressione Tab para selecionar o trecho de código realçado")

*Pressione Tab para selecionar o trecho de código realçado*

![Pressione Tab novamente e o trecho expandirá](whats-new-in-web-forms-in-aspnet-45/_static/image53.png "expandirá pressione Tab novamente e o trecho de código")

*Pressione Tab novamente e o trecho de código serão expandida*

***Para adicionar um trecho de código usando o mouse (c#, Visual Basic e XML)*** 1. Clique com botão direito em que você deseja inserir o trecho de código.

1. Selecione **Inserir trecho** seguido por **Meus trechos de código**.
2. Selecione o trecho relevante na lista, clicando nele.

![Com o botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho](whats-new-in-web-forms-in-aspnet-45/_static/image54.png "com o botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho")

*Clique com botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho*

![Selecione o trecho relevante na lista, clicando nele](whats-new-in-web-forms-in-aspnet-45/_static/image55.png "escolher o trecho relevante na lista, clicando nele")

*Selecione o trecho relevante na lista, clicando nele*
