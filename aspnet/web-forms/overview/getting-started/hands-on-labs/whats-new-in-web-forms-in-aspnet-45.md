---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
title: O que há de novo no Web Forms no ASP.NET 4.5 | Microsoft Docs
author: rick-anderson
description: A nova versão do ASP.NET Web Forms apresenta uma série de melhorias e concentrados em melhorar a experiência do usuário ao trabalhar com dados. Nas versões anteriores do...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 0a1f88bd-97da-4ed1-86f1-605199dc75a4
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: 4e8c8f303851b7f1a01744cab58e27a9b37127a6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389225"
---
<a name="whats-new-in-web-forms-in-aspnet-45"></a>O que há de novo no Web Forms no ASP.NET 4.5
====================
por [Web Camps equipe](https://twitter.com/webcamps)

> A nova versão do ASP.NET Web Forms apresenta uma série de melhorias e concentrados em melhorar a experiência do usuário ao trabalhar com dados.
> 
> Nas versões anteriores do Web Forms, ao usar a vinculação de dados para emitir o valor de um membro de objeto, você usou as expressões de associação de dados BIND () ou Eval (). Na nova versão do ASP.NET, é possível declarar o tipo de dados de um controle vai deve ser vinculado por meio de uma nova propriedade ItemType. A definição dessa propriedade permitirá que você usar uma variável fortemente tipado para receber os benefícios da experiência de desenvolvimento do Visual Studio, como IntelliSense, navegação de membro e verificação de tempo de compilação.
> 
> Com os controles de associação de dados, agora você pode também especificar seus próprios métodos personalizados para selecionando, atualização, exclusão e inserção de dados, simplificando a interação entre os controles de página e a lógica do aplicativo. Além disso, os recursos de ligação de modelo foram adicionados para o ASP.NET, o que significa que você pode mapear os dados da página diretamente para os parâmetros de tipo de método.
> 
> Validação de entrada do usuário também deve ser mais fácil com a versão mais recente do Web Forms. Agora você pode anotar suas classes de modelo com atributos de validação do **DataAnnotations** namespace e a solicitação que controla todos os seu site validam a entrada do usuário usando essas informações. Validação do lado do cliente em formulários da Web agora está integrada com o jQuery, fornecem limpador código do lado do cliente e os recursos de JavaScript discretos.
> 
> Na área de validação de solicitação, foram feitas melhorias para tornar mais fácil desativar a validação de solicitação para partes específicas de seus aplicativos ou ler dados de solicitação invalidados seletivamente.
> 
> Algumas melhorias foram feitas ao Web Forms controles de servidor para tirar proveito dos novos recursos do HTML5:
> 
> - A propriedade TextMode do controle TextBox foi atualizada para dar suporte os novos tipos de entrada do HTML5, como email, datetime e assim por diante.
> - O controle FileUpload agora dá suporte a vários carregamentos de arquivos de navegadores que oferecem suporte a esse recurso do HTML5.
> - Controles de validação agora suporte Validando entrados elementos do HTML5.
> - Novos elementos do HTML5 que têm atributos que representam uma URL agora dão suporte a runat =&quot;server&quot;. Como resultado, você pode usar as convenções do ASP.NET em caminhos de URL, como o ~ operador para representar a raiz do aplicativo (por exemplo, &lt;vídeo runat =&quot;server&quot; src =&quot;~/myVideo.wmv&quot; &gt; &lt;/vídeo&gt;).
> - O controle UpdatePanel foi corrigido para dar suporte a campos de entrada do HTML5 do lançamento.
> 
> No portal oficial do ASP.NET, você pode encontrar mais exemplos dos novos recursos no ASP.NET 4.5 do WebForms: [o que há de novo no ASP.NET 4.5 e do Visual Studio 2012](../../../../whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012.md#_Toc318097385)
> 
> Todo o código de exemplo e trechos de código são incluídos no Web Camps treinamento Kit, disponível em [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

Neste laboratório prático, você aprenderá como:

- Usar expressões de associação de dados fortemente tipados
- Usar os novos recursos de associação de modelo no Web Forms
- Usar provedores de valor para o mapeamento de dados da página para métodos de lógica
- Use anotações de dados para validação de entrada do usuário
- Levar advange de validação do lado do cliente unobstrusive com o jQuery em formulários da Web
- Implementar a validação de solicitação granular
- Implementar o processamento em formulários da Web de página assíncrona

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Pré-requisitos

Você deve ter os seguintes itens para concluir este laboratório:

- [Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou superior (leia [Apêndice A](#AppendixA) para obter instruções sobre como instalá-lo).

<a id="Setup"></a>
### <a name="setup"></a>Configuração

**Instalando os trechos de código**

Para sua conveniência, grande parte do código que você estiver gerenciando ao longo deste laboratório está disponível como trechos de código do Visual Studio. Para instalar os trechos de código executados **.\Source\Setup\CodeSnippets.vsi** arquivo.

Se você não estiver familiarizado com os trechos de código do Visual Studio e aprender como usá-los, consulte o Apêndice deste documento &quot; [trechos de código usando do apêndice c:](#AppendixC)&quot;.

<a id="Exercises"></a>
## <a name="exercises"></a>Exercícios

Este laboratório prático inclui os seguintes exercícios:

1. [Exercício 1: Associação de modelos no Web Forms do ASP.NET](#Exercise1)
2. [Exercício 2: Validação de dados](#Exercise2)
3. [Exercício 3: Formulários de processamento na Web ASP.NET de página assíncrona](#Exercise3)

> [!NOTE]
> Cada exercício é acompanhado por um **final** pasta que contém a solução resultante, você deve obter depois de concluir os exercícios. Você pode usar essa solução como um guia se você precisar trabalhar com os exercícios de ajuda adicional.


Tempo estimado para concluir este laboratório: **60 minutos**.

<a id="Exercise1"></a>

<a id="Exercise_1_Model_Binding_in_ASPNET_Web_Forms"></a>
### <a name="exercise-1-model-binding-in-aspnet-web-forms"></a>Exercício 1: Associação de modelos no Web Forms do ASP.NET

A nova versão do ASP.NET Web Forms apresenta uma série de aprimoramentos e concentrados em melhorar a experiência ao trabalhar com dados. Ao longo deste exercício, você aprenderá sobre controles de dados com rigidez de tipos e associação de modelos.

<a id="Task_1_-_Using_Strongly-Typed_Data-Bindings"></a>
#### <a name="task-1---using-strongly-typed-data-bindings"></a>Tarefa 1 - usando associações de dados fortemente tipados

Nesta tarefa, você descobrirá as novas associações rigidez de tipos disponíveis no ASP.NET 4.5.

1. Abra o **começar** solução localizado em **origem/Ex1-ModelBinding/início/** pasta.

   1. Você precisará baixar alguns pacotes do NuGet ausentes antes de continuar. Para fazer isso, clique o **Project** menu e selecione **Manage NuGet Packages**.
   2. No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, compile a solução clicando **construir** | **compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você será capaz de baixar todas as bibliotecas necessárias na primeira vez em que você executar o projeto. É por isso você terá que executar essas etapas depois de abrir uma solução existente neste laboratório.
2. Abra o **Customers.aspx** página. Colocar uma lista não numerada no controle principal e incluem um controle repeater dentro para listar cada cliente. Defina o nome do repetidor **customersRepeater** conforme mostrado no código a seguir.

    Nas versões anteriores do Web Forms, ao usar a vinculação de dados para emitir o valor de um membro em um objeto tiver associação de dados, você usaria uma expressão de associação de dados, juntamente com uma chamada para o método Eval, passando no nome do membro como uma cadeia de caracteres.

    No tempo de execução dessas chamadas Eval usar reflexão no objeto ligado no momento para ler o valor do membro com o nome fornecido e exibir o resultado no HTML. Essa abordagem facilita muito para associação de dados em relação aos dados arbitrários, unshaped.

    Infelizmente, você perderá muitos dos recursos ótima experiência de tempo de desenvolvimento no Visual Studio, incluindo IntelliSense para nomes de membro, o suporte para navegação (como ir para definição) e a verificação de tempo de compilação.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample1.aspx)]
3. Abra o **Customers.aspx.cs** arquivo.
4. Adicione a seguinte instrução using.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample2.cs)]
5. No **página\_carga** método, adicione código para preencher o repeater com a lista de clientes.

    (Código de trecho de código – *Web de fonte de dados de clientes do Forms laboratório - Ex01 - Bind*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample3.cs)]

    A solução usa o EntityFramework junto com CodeFirst para criar e acessar o banco de dados. No código a seguir, o customersRepeater está associado a uma consulta materializada que retorna todos os clientes do banco de dados.
6. Pressione **F5** para executar a solução e vá para o **clientes** página para ver o repeater em ação. Como a solução está usando CodeFirst, o banco de dados será criado e preenchido na instância do SQL Express local ao executar o aplicativo.

    ![Listando os clientes com um repetidor](whats-new-in-web-forms-in-aspnet-45/_static/image1.png "listando os clientes com um repetidor")

    *Listando os clientes com um repetidor*

    > [!NOTE]
    > No Visual Studio 2012, o IIS Express é o servidor de desenvolvimento de Web padrão.
7. Feche o navegador e volte para o Visual Studio.
8. Agora, substitua a implementação para usar associações com rigidez de tipos. Abra o **Customers.aspx** da página e use a nova **ItemType** atributo no repetidor para definir o **cliente** tipo como o tipo de associação.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample4.aspx)]

    A propriedade ItemType permite que você declare qual tipo de dados, o controle é vai ser associado a e permite que você use fortemente tipada de associação dentro do controle associado a dados.
9. Substitua o ItemTemplate conteúdo com o código a seguir.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample5.aspx)]

    Uma desvantagem com as abordagens acima é que as chamadas para Eval () e BIND () são a associação tardia – que significa que você passe sequências de caracteres para representar os nomes de propriedade. Isso significa que você não obtém o Intellisense para os nomes de membro, suporte para navegação de código (como ir para definição), nem suporte à verificação de tempo de compilação.

    Definir a propriedade ItemType faz com que duas novas variáveis de tipo a ser gerado no escopo das expressões de associação de dados: **Item** e **BindItem**. Você pode usar essas variáveis com rigidez de tipos nas expressões de associação de dados e obter os benefícios da experiência de desenvolvimento do Visual Studio.

    O &quot; **:** &quot; usado na expressão será automaticamente a codificação HTML a saída para evitar problemas de segurança (por exemplo, ataques script entre sites). Essa notação estava disponível desde o .NET 4 para resposta de escrever, mas agora também está disponível em expressões de associação de dados.

    > [!NOTE]
    > O membro de Item funciona para a associação unidirecional. Se você quiser realizar a associação bidirecional use o **BindItem** membro.

    ![Suporte do IntelliSense na associação fortemente tipadas](whats-new-in-web-forms-in-aspnet-45/_static/image2.png "suporte do IntelliSense na associação fortemente tipadas")

    *Suporte do IntelliSense na associação fortemente tipadas*
10. Pressione **F5** para executar a solução e vá para a página de clientes para garantir que as alterações funcionem conforme o esperado.

    ![Listando os detalhes do cliente](whats-new-in-web-forms-in-aspnet-45/_static/image3.png "listando os detalhes do cliente")

    *Listando os detalhes do cliente*
11. Feche o navegador e volte para o Visual Studio.

<a id="Task_2_-_Introducing_Model_Binding_in_Web_Forms"></a>
#### <a name="task-2---introducing-model-binding-in-web-forms"></a>Tarefa 2 - Introdução ao modelo de associação no Web Forms

Nas versões anteriores do Web Forms do ASP.NET, quando você desejava executar associação de dados bidirecional, tanto recuperar e atualizar dados, você precisava usar um objeto de fonte de dados. Isso pode ser um objeto de fonte de dados, uma fonte de dados SQL, uma fonte de dados do LINQ e assim por diante. No entanto se seu cenário necessário código personalizado para manipular os dados, você precisava usar o objeto de fonte de dados e isso trouxe algumas desvantagens. Por exemplo, é necessário para evitar tipos complexos e necessários para lidar com exceções ao executar a lógica de validação.

A nova versão do ASP.NET Web Forms os controles de associação de dados dão suporte a associação de modelos. Isso significa que você pode especificar selecionar, atualizar, inserir e excluir os métodos diretamente em um controle associado a dados para chamar a lógica de seu arquivo code-behind ou de outra classe.

Para saber mais sobre isso, você usará um GridView para listar as categorias de produto usando a nova **SelectMethod** atributo. Este atributo permite que você especifique um método para recuperar os dados de GridView.

1. Abra o **Products. aspx** página e inclui uma **GridView**. Configure o GridView, conforme mostrado abaixo para usar associações fortemente tipados e habilitar a classificação e paginação.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample6.aspx)]
2. Use a nova **SelectMethod** atributo para configurar o GridView para chamar um **GetCategories** método para selecionar os dados.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample7.aspx)]
3. Abra o **Products.aspx.cs** de lógica de arquivo e adicione o seguinte usando instruções.

    (Código de trecho de código – *Web Forms laboratório - Ex01 - Namespaces*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample8.cs)]
4. Adicionar um membro privado na **produtos** classe e atribuir uma nova instância da **ProductsContext**. Essa propriedade armazena o contexto de dados do Entity Framework que permite que você se conecte ao banco de dados.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample9.cs)]
5. Criar uma **GetCategories** método para recuperar a lista de categorias usando LINQ. A consulta incluirá o **produtos** propriedade para que o GridView pode mostrar a quantidade de produtos para cada categoria. Observe que o método retorna um objeto IQueryable bruto que representam a consulta seja executada mais tarde o ciclo de vida da página.

    (Código de trecho de código – *Web Forms laboratório - Ex01 - GetCategories*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample10.cs)]

    > [!NOTE]
    > Nas versões anteriores do Web Forms do ASP.NET, Habilitar classificação e paginação usando sua própria lógica de repositório em um contexto de objeto fonte de dados, necessárias para escrever seu próprio código personalizado e receber todos os parâmetros necessários. Agora, como os métodos de associação de dados podem retornar IQueryable e isso representa uma consulta ainda para ser executado, ASP.NET pode cuidar de modificar a consulta para adicionar a classificação correta e parâmetros de paginação.
6. Pressione **F5** para iniciar a depuração de site e vá para a página de produtos. Você deve ver que o GridView é preenchido com as categorias retornadas pelo método GetCategories.

    ![Preencher um GridView usando associação de modelos](whats-new-in-web-forms-in-aspnet-45/_static/image4.png "preencher um GridView usando associação de modelos")

    *Preencher um GridView usando associação de modelos*
7. Pressione **SHIFT**+**F5** parar a depuração.

<a id="Task_3_-_Value_Providers_in_Model_Binding"></a>
#### <a name="task-3---value-providers-in-model-binding"></a>Tarefa 3: provedores de valor na associação de modelos

Associação de modelos não apenas permite que você especifique métodos personalizados para trabalhar com seus dados diretamente em um controle associado a dados, mas também permite que você mapeie os dados da página em parâmetros desses métodos. No parâmetro de método, você pode usar atributos de provedor de valor para especificar a fonte de dados do valor. Por exemplo:

- Controles da página
- Valores de cadeia de caracteres de consulta
- Exibir dados
- Estado de sessão
- Cookies
- Dados de formulário postados
- Estado de exibição
- Provedores de valor personalizados também têm suporte

Se você tiver usado o ASP.NET MVC 4, você observará que o suporte à associação de modelo é semelhante. Na verdade, esses recursos foram obtidos do ASP.NET MVC e movidos para o **System. Web** assembly para ser capaz de usá-los também no Web Forms.

Nesta tarefa, você atualizará o GridView para filtrar os resultados pela quantidade de produtos para cada categoria, recebendo o parâmetro de filtro com a associação de modelo.

1. Volte para o **Products. aspx** página.
2. Na parte superior de GridView, adicione uma **etiqueta** e uma **ComboBox** para selecionar o número de produtos para cada categoria, conforme mostrado abaixo.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample11.aspx)]
3. Adicionar um **EmptyDataTemplate** a GridView para mostrar uma mensagem quando não houver nenhuma categoria com o número selecionado de produtos.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample12.aspx)]
4. Abra o **Products.aspx.cs** code-behind e adicione a seguinte instrução using.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample13.cs)]
5. Modificar a **GetCategories** método para receber um número inteiro **minProductsCount** argumento e filtrar os resultados retornados. Para fazer isso, substitua o método com o código a seguir.

    (Código de trecho de código – *Web Forms laboratório - Ex01 - GetCategories 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample14.cs)]

    O novo **[controle]** atributo as **minProductsCount** argumento informará ASP.NET seu valor deve ser preenchido usando um controle na página. ASP.NET irá procurar qualquer controle que corresponde ao nome do argumento (minProductsCount) e executar a conversão para preencher o parâmetro com o valor do controle e o mapeamento necessário.

    Como alternativa, o atributo fornece um construtor sobrecarregado que permite que você especifique o controle de onde obter o valor.

    > [!NOTE]
    > É uma meta dos recursos de vinculação de dados reduzir a quantidade de código que precisa ser escrito para interação de página. Além do provedor de valor [controle], você pode usar outros provedores de associação de modelos em seus parâmetros de método. Alguns deles são listados na introdução de tarefa.
6. Pressione **F5** para iniciar a depuração de site e vá para a página de produtos. Selecione um número de produtos na lista suspensa e observe como o GridView está atualizado.

    ![Filtragem de GridView com um valor de lista suspensa](whats-new-in-web-forms-in-aspnet-45/_static/image5.png "filtragem GridView com um valor de lista suspensa")

    *Filtragem de GridView com um valor de lista suspensa*
7. Pare a depuração.

<a id="Task_4_-_Using_Model_Binding_for_Filtering"></a>
#### <a name="task-4---using-model-binding-for-filtering"></a>Tarefa 4 - usando o modelo de associação para filtragem

Nesta tarefa, você irá adicionar um segundo, o filho GridView para mostrar os produtos na categoria selecionada.

1. Abra o **Products. aspx** página e atualizar as categorias GridView para gerar automaticamente o botão de seleção.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample15.aspx)]
2. Adicione um segundo **GridView** denominado **productsGrid** na parte inferior. Defina as **ItemType** para **WebFormsLab.Model.Product**, o **DataKeyNames** para **ProductId** e o **SelectMethod**  à **GetProducts**. Definir **AutoGenerateColumns** à **falso** e adicione as colunas ProductId, ProductName, descrição e UnitPrice.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample16.aspx)]
3. Abra o **Products.aspx.cs** arquivo code-behind. Implemente a **GetProducts** método para receber a ID da categoria da categoria de GridView e filtrar os produtos. Associação de modelos definirá o valor do parâmetro usando a linha selecionada na **categoriesGrid**. Uma vez que o nome do argumento e o nome do controle não corresponderem, você deve especificar o nome do controle no provedor de valor de controle.

    (Código de trecho de código – *Web Forms laboratório - Ex01 - GetProducts*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample17.cs)]

    > [!NOTE]
    > Essa abordagem facilita a unidade esses métodos de teste. Em um contexto de teste de unidade, em que os formulários da Web não está em execução, o atributo [controle] não executará nenhuma ação específica.
4. Abra o **Products. aspx** página e localize os produtos GridView. Atualize os produtos GridView para mostrar um link para editar o produto selecionado.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample18.aspx)]
5. Abra o **ProductDetails.aspx** página de lógica e substitua o **SelectProduct** método com o código a seguir.

    (Código de trecho de código – *Web Forms laboratório - Ex01 - SelectProduct método*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample19.cs)]

    > [!NOTE]
    > Observe que o **[QueryString]** atributo é usado para preencher o parâmetro do método de um parâmetro productId na cadeia de caracteres de consulta.
6. Pressione **F5** para iniciar a depuração de site e vá para a página de produtos. Selecione qualquer categoria nas categorias GridView e observe que os produtos GridView é atualizado.

    ![Mostrando produtos da categoria selecionada](whats-new-in-web-forms-in-aspnet-45/_static/image6.png "mostrando produtos da categoria selecionada")

    *Mostrando produtos da categoria selecionada*
7. Clique o **exibição** link em um produto para abrir a página ProductDetails.aspx.

    Observe que a página está recuperando o produto com SelectMethod usando o parâmetro productId da cadeia de consulta.

    ![Exibindo os detalhes do produto](whats-new-in-web-forms-in-aspnet-45/_static/image7.png "exibindo os detalhes do produto")

    *Exibindo os detalhes do produto*

    > [!NOTE]
    > A capacidade de digitar uma descrição em HTML será implementada no próximo exercício.

<a id="Task_5_-_Using_Model_Binding_for_Update_Operations"></a>
#### <a name="task-5---using-model-binding-for-update-operations"></a>Tarefa 5 – usando o modelo de associação para operações de atualização

Na tarefa anterior, você usou a associação de modelo, principalmente para selecionar os dados, essa tarefa, você aprenderá como usar a associação de modelos em operações de atualização.

Você atualizará as categorias de GridView para permitir que o usuário categorias de atualização.

1. Abra o **Products. aspx** da página e atualizar as categorias GridView para gerar automaticamente o botão Editar e usar a nova **UpdateMethod** atributo para especificar um **UpdateCategory não**método para atualizar o item selecionado.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample20.aspx)]

    O atributo DataKeyNames no GridView definem quais são os membros que identificam exclusivamente o objeto de associação de modelo e, portanto, quais são os parâmetros que do método update deverá receber pelo menos.
2. Abra o **Products.aspx.cs** arquivo code-behind e implementar o **UpdateCategory não** método. O método deve receber a ID da categoria para carregar a categoria atual, preencher os valores de GridView e, em seguida, atualizar a categoria.

    (Código de trecho de código – *Web UpdateCategory não laboratório - Ex01 - de formulários*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample21.cs)]

    O novo **TryUpdateModel** método na classe de página é responsável de preencher o objeto de modelo usando os valores dos controles na página. Nesse caso, ele substituirá os valores atualizados da linha de GridView atual que está sendo editado para o **categoria** objeto.

    > [!NOTE]
    > O próximo exercício explica o uso do ModelState para validar os dados inseridos pelo usuário ao editar o objeto.
3. Executar o site e vá para a página de produtos. Edite uma categoria. Digite um novo nome e, em seguida, clique em **atualização** para manter as alterações.

    ![Edição de categorias](whats-new-in-web-forms-in-aspnet-45/_static/image8.png "editando categorias")

    *Categorias de edição*

<a id="Exercise2"></a>

<a id="Exercise_2_Data_Validation"></a>
### <a name="exercise-2-data-validation"></a>Exercício 2: Validação de dados

Neste exercício, você aprenderá sobre os novos recursos de validação de dados no ASP.NET 4.5. Você irá conferir os novos recursos de validação não invasiva do Web Forms. Você usará as anotações de dados nas classes de modelo de aplicativo para validação de entrada do usuário e, finalmente, você aprenderá como ativar ou desativar a validação de solicitação para controles individuais em uma página.

<a id="Task_1_-_Unobtrusive_Validation"></a>
#### <a name="task-1---unobtrusive-validation"></a>Tarefa 1 - validação não invasiva

Formulários com dados complexos, incluindo validadores tendem a gerar muito código JavaScript na página, que pode representar a cerca de 60% do código. Com a validação não invasiva habilitada, o código HTML ficará mais limpo e mais organizada.

Nesta seção, você habilitará a validação não invasiva no ASP.NET para comparar o código HTML gerado por ambas as configurações.

1. Abra **Visual Studio 2012** e abra o **Begin** solução localizada no **Source\Ex2 Validation\Begin** pasta deste laboratório. Como alternativa, você pode continuar trabalhando em sua solução existente do exercício anterior.

   1. Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar. Para fazer isso, no Gerenciador de soluções, clique o **WebFormsLab** project **Manage NuGet Packages**.
   2. No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, compile a solução clicando **construir** | **compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você será capaz de baixar todas as bibliotecas necessárias na primeira vez em que você executar o projeto. É por isso você terá que executar essas etapas depois de abrir uma solução existente neste laboratório.
2. Pressione **F5** para iniciar o aplicativo web. Vá para os clientes de página e clique no **adicionar um novo cliente** link.
3. Clique com botão direito na página do navegador e, em seguida, selecione **Exibir código-fonte** opção para abrir o código HTML gerado pelo aplicativo.

    ![Mostrando a página de código HTML](whats-new-in-web-forms-in-aspnet-45/_static/image9.png "mostrando a página de código HTML")

    *Mostrando a página de código HTML*
4. Percorra o código de origem da página e observe que o ASP.NET tem injetado validadores de dados e código JavaScript na página para executar as validações e mostrar a lista de erros.

    ![Código de JavaScript de validação na página CustomerDetails](whats-new-in-web-forms-in-aspnet-45/_static/image10.png "código JavaScript de validação na página CustomerDetails")

    *Código de JavaScript de validação na página CustomerDetails*
5. Feche o navegador e volte para o Visual Studio.
6. Agora, você habilitará a validação não invasiva. Abra **Web. config** e localize **ValidationSettings:UnobtrusiveValidationMode** chave no **AppSettings** seção **.** Defina o valor da chave como **WebForms**.

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample22.xml)]

    > [!NOTE]
    > Você também pode definir essa propriedade &quot; **página\_Load** &quot; evento caso você deseja habilitar a validação não invasiva somente para algumas páginas.
7. Abra **CustomerDetails.aspx** e pressione **F5** para iniciar o aplicativo Web.
8. Pressione a tecla F12 para abrir as ferramentas de desenvolvedor do IE. Quando as ferramentas de desenvolvedor é aberto, selecione a guia do script. Selecione **CustomerDetails.aspx** no menu e take, observe que os scripts necessários para executar jQuery na página tiverem sido carregados no navegador de site local.

    ![Carregar o jQuery JavaScript arquivos diretamente do servidor local do IIS](whats-new-in-web-forms-in-aspnet-45/_static/image11.png "carregar o jQuery JavaScript arquivos diretamente do servidor IIS local")

    *Ao carregar os arquivos de JavaScript jQuery diretamente do servidor IIS local*
9. Feche o navegador para retornar ao Visual Studio. Abra o **Master** novamente e localize o **ScriptManager**. Adicione o atributo **EnableCdn** propriedade com o valor **verdadeiro**. Isso forçará o jQuery para ser carregado da URL on-line, não de URL do site local.
10. Abra **CustomerDetails.aspx** no Visual Studio. Pressione a tecla F5 para executar o site. Depois que o Internet Explorer é aberta, pressione a tecla F12 para abrir as ferramentas de desenvolvedor. Selecione o **Script** guia e, em seguida, examine a lista suspensa. Observe que os arquivos de JavaScript jQuery não estão sendo carregados do site local, mas em vez do on-line jQuery da CDN.

    ![Arquivos de carregar o JavaScript jQuery da CDN](whats-new-in-web-forms-in-aspnet-45/_static/image12.png "arquivos de carregar o JavaScript jQuery da CDN")

    *Ao carregar os arquivos de JavaScript jQuery da CDN*
11. Abra o código-fonte HTML página novamente usando a opção de exibição de código-fonte no navegador. Observe que, ao habilitar a validação não invasiva ASP.NET tiver substituído o código JavaScript injetado com dados - \*atributos.

    ![Código de validação não invasiva](whats-new-in-web-forms-in-aspnet-45/_static/image13.png "código de validação não invasiva")

    *Código de validação não invasiva*

    > [!NOTE]
    > Neste exemplo, você viu como um resumo com anotações de dados de validação foi simplificado para apenas algumas HTML e JavaScript linhas. Anteriormente, sem validação não invasiva, os controles de validação mais adicionar, quanto maior o código de validação de JavaScript aumentará.

<a id="Task_2_-_Validating_the_Model_with_Data_Annotations"></a>
#### <a name="task-2---validating-the-model-with-data-annotations"></a>Tarefa 2: validar o modelo com anotações de dados

ASP.NET 4.5 introduz a validação de anotações de dados para formulários da Web. Em vez de ter um controle de validação em cada entrada, você pode definir restrições em suas classes de modelo e usá-los em todo o aplicativo web. Nesta seção, você aprenderá como usar anotações de dados para validar um formulário de novo/Editar cliente.

1. Abra **CustomerDetail.aspx** página. Observe que o cliente primeiro nome e o segundo nome na **EditItemTemplate** e **InsertItemTemplate** seções são validadas usando um RequiredFieldValidator controles. Cada validador está associado a uma determinada condição, portanto, você precisará incluir tantos validadores como condições a serem verificadas.
2. Adicione anotações de dados para validar a classe de modelo de cliente. Abra **Customer.cs** classe a **modelo** pasta e *decorar* cada propriedade usando atributos de anotação de dados.

    (Código de trecho de código – *Web Forms laboratório - Ex02 - Data Annotations*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample23.cs)]

    > [!NOTE]
    > .NET framework 4.5 estendeu a coleção de anotação de dados existente. Estas são algumas das anotações de dados que você pode usar: [CreditCard], [Phone], [EmailAddress], [intervalo], [comparar], [Url], [FileExtensions], [Required], [chave], [RegularExpression].
    > 
    > Alguns exemplos de uso:
    > 
    > [Chave]: Specifies that an attribute is the unique identifier
    > 
    > [Range(0.4, 0.5, ErrorMessage=&quot;{Write an error message}&quot;]: Double range
    > 
    > [EmailAddress(ErrorMessage=&quot;Invalid Email&quot;), MaxLength(56)]: Two annotations in the same line.
    > 
    > Você também pode definir suas próprias mensagens de erro dentro de cada atributo.
3. Abra **CustomerDetails.aspx** e remover todos os RequiredFieldvalidators para os campos nome e sobrenome nas seções em EditItemTemplate e InsertItemTemplate do controle FormView.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample24.aspx)]

    > [!NOTE]
    > Uma vantagem de usar anotações de dados é que a lógica de validação não está duplicada em páginas do seu aplicativo. Você defini-lo uma vez no modelo e usá-lo em todas as páginas de aplicativo que manipulam dados.
4. Abra **CustomerDetails.aspx** code-behind e localize o método SaveCustomer. Esse método é chamado quando a inserção de um novo cliente e recebe o parâmetro de cliente entre os valores de controle FormView. Quando o mapeamento entre os controles de página e o parâmetro objeto ocorre, o ASP.NET executará a validação do modelo em relação a anotação de dados de atributos e preencher o dicionário ModelState com os erros encontrados, se houver.

    O ModelState só retornará true se todos os campos no modelo são válidos depois de executar a validação.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample25.cs)]
5. Adicionar um **ValidationSummary** controle no final da página CustomerDetails para mostrar a lista de erros do modelo.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample26.aspx)]

    O **ShowModelStateErrors** é uma nova propriedade no ValidationSummary controlar que quando definido como **verdadeiro**, o controle mostrará os erros do dicionário ModelState. Esses erros provenientes de validação de anotações de dados.
6. Pressione **F5** para executar o aplicativo Web. Preencha o formulário com alguns valores incorretos e clique em **salvar** para executar a validação. Observe o erro resumo na parte inferior.

    ![Validação com anotações de dados](whats-new-in-web-forms-in-aspnet-45/_static/image14.png "validação com anotações de dados")

    *Validação com anotações de dados*

<a id="Task_3_-_Handling_Custom_Database_Errors_with_ModelState"></a>
#### <a name="task-3---handling-custom-database-errors-with-modelstate"></a>Tarefa 3: tratamento de erros de banco de dados personalizado com ModelState

Na versão anterior do Web Forms, manipulação de erros de banco de dados como uma cadeia de caracteres muito longa ou uma violação de chave exclusiva pode envolver a gerar exceções em seu código de repositório e, em seguida, manipular as exceções em seu code-behind para exibir um erro. Uma grande quantidade de código é necessário para fazer algo relativamente simples.

No Web Forms 4.5, o objeto de ModelState pode ser usado para exibir os erros na página do seu modelo ou do banco de dados, de maneira consistente.

Nesta tarefa, você adicionará código para tratar exceções de banco de dados e mostre a mensagem apropriada para o usuário usando o objeto ModelState corretamente.

1. Enquanto o aplicativo ainda está em execução, tente atualizar o nome de uma categoria de uso de um valor duplicado.

    ![Atualizando uma categoria com um nome duplicado](whats-new-in-web-forms-in-aspnet-45/_static/image15.png "atualizando uma categoria com um nome duplicado")

    *Atualizando uma categoria com um nome duplicado*

    Observe que uma exceção é gerada devido à &quot;exclusivos&quot; restrição da **CategoryName** coluna.

    ![A exceção para nomes de categoria duplicada](whats-new-in-web-forms-in-aspnet-45/_static/image16.png "exceção para nomes de categoria duplicada")

    *Exceção para nomes de categoria duplicada*
2. Pare a depuração. No **Products.aspx.cs** arquivo code-behind, atualize o **UpdateCategory não** método para manipular as exceções geradas pelo BD. Método SaveChanges () chama e adicione um erro para o **ModelState** objeto.

    O novo **TryUpdateModel** método atualiza o objeto de categoria recuperado do banco de dados usando os dados do formulário fornecidos pelo usuário.

    (Código de trecho de código – *Web Forms laboratório - Ex02 - UpdateCategory não identificador erros*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample27.cs)]

    > [!NOTE]
    > Idealmente, você teria que identificar a causa do DbUpdateException e verificar se a causa raiz é a violação de restrição de chave exclusiva.
3. Abra **Products. aspx** e adicione um **ValidationSummary** controle abaixo as categorias de GridView para mostrar a lista de erros de modelo.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample28.aspx)]
4. Executar o site e vá para a página de produtos. Tente atualizar o nome de uma categoria usando um valor duplicado.

    Observe que a exceção foi identificada e a mensagem de erro é exibido na **ValidationSummary** controle.

    ![Duplicado categoria erro](whats-new-in-web-forms-in-aspnet-45/_static/image17.png "duplicado categoria erro")

    *Erro de categoria duplicada*

<a id="Task_4_-_Request_Validation_in_ASPNET_Web_Forms_45"></a>
#### <a name="task-4---request-validation-in-aspnet-web-forms-45"></a>Tarefa 4 - solicitação de validação no Web Forms do ASP.NET 4.5

O recurso de validação de solicitação no ASP.NET fornece um certo nível de proteção padrão contra ataques de script de entre sites (XSS). Nas versões anteriores do ASP.NET, a validação de solicitação foi habilitada por padrão e só pode ser desabilitada para uma página inteira. Com a nova versão do Web Forms do ASP.NET pode agora desabilitar a validação de solicitação para um único controle, executar a validação de solicitação lenta ou acessar os dados de solicitação não validadas (tenha cuidado se você fizer isso!).

1. Pressione **Ctrl + F5** para iniciar o site sem depuração e vá para a página de produtos. Selecione uma categoria e, em seguida, clique no **editar** link em qualquer um dos produtos.
2. Digite uma descrição que contém conteúdo potencialmente perigoso, por exemplo, incluindo marcas HTML. Observe a exceção lançada devido à validação de solicitação.

    ![Edição de um produto com conteúdo potencialmente perigoso](whats-new-in-web-forms-in-aspnet-45/_static/image18.png "edição de um produto com conteúdo potencialmente perigoso")

    *Edição de um produto com conteúdo potencialmente perigoso*

    ![Exceção lançada devido à validação de solicitação](whats-new-in-web-forms-in-aspnet-45/_static/image19.png "exceção lançada devido à validação de solicitação")

    *Exceção lançada devido à validação de solicitação*
3. Feche a página e, no Visual Studio, pressione **SHIFT+F5** para parar a depuração.
4. Abra o **ProductDetails.aspx** da página e localize o **descrição** caixa de texto.
5. Adicione a nova **ValidateRequestMode** propriedade à caixa de texto e defina seu valor como **desabilitado**.

    O novo **ValidateRequestMode** atributo permite que você desabilite a validação de solicitação granularmente em cada controle. Isso é útil quando você deseja usar uma entrada que pode receber o código HTML, mas deseja manter a validação funcionando para o restante da página.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample29.aspx)]
6. Pressione **F5** para executar o aplicativo web. Abra a página de produto da edição novamente e concluir uma descrição de produto, incluindo marcas HTML. Observe que agora você pode adicionar conteúdo HTML para a descrição.

    ![Desabilitado para a descrição do produto de validação de solicitação](whats-new-in-web-forms-in-aspnet-45/_static/image20.png "desabilitada para a descrição do produto de validação de solicitação")

    *Validação de solicitação de desabilitado para a descrição do produto*

    > [!NOTE]
    > Em um aplicativo de produção, você deve limpar o código HTML inserido pelo usuário para certificar-se de seguras somente marcas HTML são inseridas (por exemplo, há nenhuma &lt;script&gt; marcas). Para fazer isso, você pode usar [Microsoft Web Protection Library](https://www.nuget.org/packages/AntiXSS).
7. Edite o produto novamente. Digite o código HTML no campo nome e clique em **salvar**. Aviso de validação de solicitação é desabilitada somente para o campo de descrição e o restante dos campos re ainda validados em relação o conteúdo potencialmente perigoso.

    ![Habilitado no restante dos campos de validação de solicitação](whats-new-in-web-forms-in-aspnet-45/_static/image21.png "habilitada no restante dos campos de validação de solicitação")

    *Validação de solicitação habilitada no restante dos campos*

    Web Forms do ASP.NET 4.5 inclui um novo modo de validação de solicitação para executar a validação de solicitação lentamente. Com o modo de validação de solicitação definido como **4.5**, se uma parte do código acessos *Request. Form [&quot;chave&quot;]*, validação de solicitação do gatilho de será de validação somente solicitação do ASP.NET 4.5 para esse elemento específico na coleção de formulário.

    Além disso, o ASP.NET 4.5 agora inclui rotinas de codificação de núcleo do v4.0 a biblioteca do Microsoft Anti-XSS. As rotinas de codificação são implementadas pelo novo de Anti-XSS *AntiXssEncoder* tipo encontrado no novo **AntiXSS** namespace. Com o **encoderType** parâmetro configurado para usar *AntiXssEncoder*, toda a saída codificação dentro do ASP.NET automaticamente usa as rotinas de codifica de novo.
8. Validação de solicitação 4.5 ASP.NET também dá suporte a acesso não validado para solicitar dados. ASP.NET 4.5 adiciona uma nova propriedade de coleção para o **HttpRequest** objeto chamado **Unvalidated**. Quando você navega até **HttpRequest.Unvalidated** você tem acesso a todas as partes comuns de dados de solicitação, incluindo formulários, as QueryStrings, Cookies, URLs e assim por diante.

    ![Objeto Request.Unvalidated](whats-new-in-web-forms-in-aspnet-45/_static/image22.png "Request.Unvalidated objeto")

    *Objeto Request.Unvalidated*

    > [!NOTE]
    > **Use a propriedade de HttpRequest.Unvalidated com cuidado!** Verifique se que você executa cuidadosamente a validação personalizada nos dados brutos de solicitação para garantir que o texto perigoso não é recuperado e processado para que clientes!

<a id="Exercise3"></a>

<a id="Exercise_3_Asynchronous_Page_Processing_in_ASPNET_Web_Forms"></a>
### <a name="exercise-3-asynchronous-page-processing-in-aspnet-web-forms"></a>Exercício 3: Formulários de processamento na Web ASP.NET de página assíncrona

Neste exercício, você será apresentado para a nova página assíncrona, processamento de recursos no Web Forms do ASP.NET.

<a id="Task_1_-_Updating_the_Product_Details_Page_to_Upload_and_Show_Images"></a>
#### <a name="task-1---updating-the-product-details-page-to-upload-and-show-images"></a>Tarefa 1 - atualização do produto fornece detalhes sobre a página para carregar e exibir imagens

Nesta tarefa, você atualizará a página de detalhes do produto para permitir que o usuário especifique uma URL de imagem para o produto e exibi-lo no modo de exibição somente leitura. Você criará uma cópia local da imagem especificada, baixando-o de forma síncrona. Na próxima tarefa, você irá atualizar essa implementação para que ele funcione de forma assíncrona.

1. Abra **Visual Studio 2012** e carregar o **começar** solução localizada no **Source\Ex3 Async\Begin** da pasta neste laboratório. Como alternativa, você pode continuar trabalhando em sua solução existente de exercícios anteriores.

   1. Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar. Para fazer isso, no Gerenciador de soluções, clique o **WebFormsLab** do projeto e selecione **Manage NuGet Packages**.
   2. No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, compile a solução clicando **construir** | **compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você será capaz de baixar todas as bibliotecas necessárias na primeira vez em que você executar o projeto. É por isso você terá que executar essas etapas depois de abrir uma solução existente neste laboratório.
2. Abra o **ProductDetails.aspx** página de código-fonte e adicione um campo no ItemTemplate de FormView para mostrar a imagem do produto.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample30.aspx)]
3. Adicione um campo para especificar a URL da imagem na EditTemplate de FormView.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample31.aspx)]
4. Abra o **ProductDetails.aspx.cs** de lógica de arquivo e adicione as seguintes diretivas de namespace.

    (Código de trecho de código – *Web Forms laboratório - Ex03 - Namespaces*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample32.cs)]
5. Criar uma **UpdateProductImage** método para armazenar imagens remotas no local **imagens** pasta e atualize a entidade de produto com o novo valor de local de imagem.

    (Código de trecho de código – *Web Forms laboratório - Ex03 - UpdateProductImage*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample33.cs)]
6. Atualizar o **UpdateProduct** método para chamar o **UpdateProductImage** método.

    (Código de trecho de código – *formulários laboratório - Ex03 - UpdateProductImage chamada Web*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample34.cs)]
7. Execute o aplicativo e tentar carregar uma imagem de um produto. Por exemplo, você pode usar a seguinte URL de imagem do Office Clip Arts: [[http://officeimg.vo.msecnd.net/images/MB900437099.jpg](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)

    ![Definindo uma imagem de um produto](whats-new-in-web-forms-in-aspnet-45/_static/image23.png "definindo uma imagem de um produto")

    *Definindo uma imagem de um produto*

<a id="Task_2_-_Adding_Asynchronous_Processing_to_the_Product_Details_Page"></a>
#### <a name="task-2---adding-asynchronous-processing-to-the-product-details-page"></a>Tarefa 2 - adicionando assíncrona de processamento para a página de detalhes do produto

Nesta tarefa, você atualizará a página de detalhes do produto para torná-lo a trabalhar de forma assíncrona. Você aumentará uma tarefa demorada – o processo de download da imagem - usando o processamento de página assíncrona do ASP.NET 4.5.

Métodos assíncronos em aplicativos da web podem ser usados para otimizar a forma como os pools de threads do ASP.NET são usados. No ASP.NET lá são solicita um número limitado de threads no pool de threads para participar, assim, quando todos os threads estiverem ocupados, ASP.NET começa a rejeitar novas solicitações, envia mensagens de erro do aplicativo e torna o seu site não está disponível.

Operações demoradas em seu site da web são bons candidatos para programação assíncrona porque eles ocupam o thread atribuído por um longo tempo. Isso inclui solicitações de longa execução, as páginas com muitos elementos diferentes e páginas que requerem operações offline, tal consulta um banco de dados ou acesso a um servidor web externo. A vantagem é que, se você usar métodos assíncronos para essas operações, enquanto a página é processada, o thread é liberado e retornado para o thread de pool e pode ser usado para participar de uma nova solicitação de página. Isso significa que, a página será iniciado em um thread do pool de threads de processamento e processamento em um diferente, pode ser concluída após o processamento assíncrono é concluído.

1. Abra o **ProductDetails.aspx** página. Adicione a **Async** atributo na **página** elemento e defini-lo como **true**. Esse atributo diz ao ASP.NET para implementar a interface IHttpAsyncHandler.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample35.aspx)]
2. Adicione um rótulo na parte inferior da página para mostrar os detalhes dos threads de execução da página.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample36.aspx)]
3. Abra **ProductDetails.aspx.cs** e adicione as seguintes diretivas de namespace.

    (Código de trecho de código – *Web Forms laboratório - Ex03 - Namespaces 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample37.cs)]
4. Modificar a **UpdateProductImage** método para baixar a imagem com uma tarefa assíncrona. Você substituirá a **WebClient** **DownloadFile** método com o **DownloadFileTaskAsync** método e incluem o **await** palavra-chave.

    (Código de trecho de código – *Web Forms laboratório - Ex03 - UpdateProductImage Async*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample38.cs)]

    O RegisterAsyncTask registra uma nova tarefa assíncrona página a ser executado em um thread diferente. Ele recebe uma expressão lambda com a tarefa (t) a ser executado. O **await** palavra-chave na **DownloadFileTaskAsync** método converte o restante do método em um retorno de chamada é invocado de forma assíncrona após o **DownloadFileTaskAsync** método for concluído. ASP.NET retomará a execução do método, mantendo automaticamente todos os os originais valores de solicitação HTTP. O novo modelo de programação assíncrono no .NET 4.5 permite que você escrever código assíncrono que é muito parecido com o código síncrono e deixar que o compilador lidar com as complicações de funções de retorno de chamada ou código de continuação.
    > [!NOTE]
    > RegisterAsyncTask e PageAsyncTask já estavam disponíveis desde o .NET 2.0. A palavra-chave await é nova a partir do modelo de programação assíncrona .NET 4.5 e pode ser usada junto com os novos métodos TaskAsync do objeto .NET WebClient.
5. Adicione código para exibir os threads nos quais o código iniciado e concluído a execução. Para fazer isso, atualize o **UpdateProductImage** método com o código a seguir.

    (Código de trecho de código – *threads de Web Forms laboratório - Ex03 - Show*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample39.cs)]
6. Abra o site **Web. config** arquivo. Adicione a seguinte variável de appSetting.

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample40.xml)]
7. Pressione **F5** para executar o aplicativo e carregue uma imagem para o produto. Observe a ID de threads em que o código iniciado e concluído pode ser diferente. Isso ocorre porque a execução de tarefas assíncronas em um thread separado do pool de threads do ASP.NET. Quando a tarefa for concluída, o ASP.NET coloca a tarefa novamente na fila e atribui qualquer um dos threads disponíveis.

    ![Baixar uma imagem de forma assíncrona](whats-new-in-web-forms-in-aspnet-45/_static/image24.png "baixar uma imagem de forma assíncrona")

    *Baixar uma imagem de forma assíncrona*

> [!NOTE]
> Além disso, você pode implantar esse aplicativo do Azure seguinte [apêndice b: publicando um aplicativo ASP.NET MVC 4 usando a implantação da Web](#AppendixB).


* * *

<a id="Summary"></a>
## <a name="summary"></a>Resumo

Neste laboratório prático, os conceitos a seguir foram resolvidos e demonstrados:

- Usar expressões de associação de dados fortemente tipados
- Usar os novos recursos de associação de modelo no Web Forms
- Usar provedores de valor para o mapeamento de dados da página para métodos de lógica
- Use anotações de dados para validação de entrada do usuário
- Levar advange de validação do lado do cliente unobstrusive com o jQuery em formulários da Web
- Implementar a validação de solicitação granular
- Implementar o processamento em formulários da Web de página assíncrona

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apêndice a: instalação do Visual Studio Express 2012 para Web

Você pode instalar **Microsoft Visual Studio Express 2012 para Web** ou outra &quot;Express&quot; versão usando o **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. As instruções a seguir guia você pelas etapas necessárias para instalar *Visual studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.

1. Vá para [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Como alternativa, se você já tiver instalado o Web Platform Installer, você pode abri-lo e pesquisar o produto &quot; <em>Visual Studio Express 2012 para Web com o SDK do Azure</em>&quot;.
2. Clique em **instalar agora**. Se você não tiver **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.
3. Uma vez **Web Platform Installer** é aberto, clique em **instalar** para iniciar a instalação.

    ![Instalar o Visual Studio Express](whats-new-in-web-forms-in-aspnet-45/_static/image25.png "instalar o Visual Studio Express")

    *Instalar o Visual Studio Express*
4. Leia os termos e licenças de todos os produtos e clique em **aceito** para continuar.

    ![Aceitar os termos de licença](whats-new-in-web-forms-in-aspnet-45/_static/image26.png)

    *Aceitar os termos de licença*
5. Aguarde até que o processo de download e instalação for concluído.

    ![Progresso da instalação](whats-new-in-web-forms-in-aspnet-45/_static/image27.png)

    *Progresso da instalação*
6. Quando a instalação for concluída, clique em **concluir**.

    ![Instalação concluída](whats-new-in-web-forms-in-aspnet-45/_static/image28.png)

    *Instalação concluída*
7. Clique em **Exit** para fechar o Web Platform Installer.
8. Para abrir o Visual Studio Express para Web, vá para o **iniciar** de tela e comece a escrever &quot; **VS Express**&quot;, em seguida, clique no **VS Express para Web** lado a lado.

    ![VS Express para o bloco da Web](whats-new-in-web-forms-in-aspnet-45/_static/image29.png)

    *VS Express para o bloco da Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Apêndice b: publicando um aplicativo ASP.NET MVC 4 usando a implantação da Web

Este apêndice mostram como criar um novo site do Portal do Azure e publicar o aplicativo que você obteve seguindo o ambiente de laboratório, aproveitando o recurso de publicação de implantação da Web fornecido pelo Azure.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>Tarefa 1 - criar um novo Site do Portal do Azure

1. Vá para o [Portal de gerenciamento](https://manage.windowsazure.com/) e entre usando as credenciais da Microsoft associadas à sua assinatura.

    > [!NOTE]
    > Com o Azure, você pode hospedar 10 Sites da Web ASP.NET gratuitamente e, em seguida, dimensione conforme seu tráfego aumenta. Você pode se inscrever [aqui](http://aka.ms/aspnet-hol-azure).

    ![Faça logon no portal do Windows Azure](whats-new-in-web-forms-in-aspnet-45/_static/image30.png "faça logon no portal do Windows Azure")

    *Faça logon no Portal*
2. Clique em **New** na barra de comandos.

    ![Criando um novo Site](whats-new-in-web-forms-in-aspnet-45/_static/image31.png "criando um novo Site")

    *Criando um novo Site*
3. Clique em **Compute** | **Site da Web**. Em seguida, selecione **criação rápida** opção. Forneça uma URL disponível para o novo site da web e clique em **Criar Site**.

    > [!NOTE]
    > O Azure é o host para um aplicativo web em execução na nuvem que você pode controlar e gerenciar. A opção criação rápida permite que você implante um aplicativo web concluído no Azure de fora do portal. Ele não inclui as etapas para configurar um banco de dados.

    ![Criando um novo Site usando a criação rápida](whats-new-in-web-forms-in-aspnet-45/_static/image32.png "criando um novo Site usando a criação rápida")

    *Criando um novo Site usando a criação rápida*
4. Aguarde até que o novo **Site da Web** é criado.
5. Depois que o Site da Web é criado, clique no link sob a **URL** coluna. Verifique se o novo Site da Web está funcionando.

    ![Navegando até o novo site](whats-new-in-web-forms-in-aspnet-45/_static/image33.png "navegando até o novo site da web")

    *Navegando até o novo site da web*

    ![Site da Web em execução](whats-new-in-web-forms-in-aspnet-45/_static/image34.png "site da Web em execução")

    *Site da Web em execução*
6. Volte para o portal e clique no nome do site da web sob o **nome** coluna para exibir as páginas de gerenciamento.

    ![Abrir as páginas de gerenciamento do site da web](whats-new-in-web-forms-in-aspnet-45/_static/image35.png "abrir as páginas de gerenciamento do site da web")

    *Abrir as páginas de gerenciamento do Site da Web*
7. No **Dashboard** página na **visão geral rápida** seção, clique no **baixar perfil de publicação** link.

    > [!NOTE]
    > O *perfil de publicação* contém todas as informações necessárias para publicar um aplicativo web no Azure para cada método de publicação habilitado. O perfil de publicação contém as URLs, as credenciais do usuário e cadeias de caracteres de banco de dados necessárias para conectar e autenticar em relação a cada um dos pontos de extremidade para o qual um método de publicação está habilitado. **Microsoft WebMatrix 2**, **Microsoft Visual Studio para Web Express** e **Microsoft Visual Studio 2012** oferecem suporte à leitura perfis de publicação para automatizar a configuração desses programas para publicação de aplicativos web do Azure.

    ![Baixando o site da web de perfil de publicação](whats-new-in-web-forms-in-aspnet-45/_static/image36.png "baixando o site da web de perfil de publicação")

    *Baixando o Site da Web de perfil de publicação*
8. Baixe o arquivo de perfil de publicação em um local conhecido. Ainda mais neste exercício, você verá como usar esse arquivo para publicar um aplicativo web do Azure do Visual Studio.

    ![Salvando o arquivo de perfil de publicação](whats-new-in-web-forms-in-aspnet-45/_static/image37.png "salvar o perfil de publicação")

    *Salvando o arquivo de perfil de publicação*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tarefa 2 – Configurando o servidor de banco de dados

Se seu aplicativo faz uso do SQL Server você precisará criar um servidor de banco de dados SQL de bancos de dados. Se você quiser implantar um aplicativo simples que não usa o SQL Server, você pode ignorar esta tarefa.

1. Você precisará de um servidor de banco de dados SQL para armazenar o banco de dados do aplicativo. Você pode exibir os servidores de banco de dados SQL na sua assinatura no portal de gerenciamento do Azure em **bancos de dados Sql** | **servidores** | **painel do servidor**. Se você não tiver um servidor criado, você pode criar uma usando o **adicionar** botão na barra de comandos. Anote o **nome do servidor e o URL, o nome de logon de administrador e senha**, pois você usará nas próximas tarefas. Não crie o banco de dados ainda, ele será criado em um estágio posterior.

    ![Painel do servidor de banco de dados SQL](whats-new-in-web-forms-in-aspnet-45/_static/image38.png "painel de banco de dados do SQL Server")

    *Painel de banco de dados do SQL Server*
2. A próxima tarefa, você testará a conexão de banco de dados do Visual Studio, por esse motivo, você precisa incluir seu endereço IP local na lista do servidor **endereços IP permitidos**. Para fazer isso, clique em **configurar**, selecione o endereço IP do **endereço de IP do cliente atual** e cole-o na **endereço IP inicial** e **oendereçoIPfinal** caixas de texto e clique em de ![add-client-ip-address-ok-button](whats-new-in-web-forms-in-aspnet-45/_static/image39.png) botão.

    ![Adicionar endereço IP do cliente](whats-new-in-web-forms-in-aspnet-45/_static/image40.png)

    *Adicionar endereço IP do cliente*
3. Uma vez a **endereço IP do cliente** é adicionado para endereços IP, clique em **salvar** para confirmar as alterações.

    ![Confirmar alterações](whats-new-in-web-forms-in-aspnet-45/_static/image41.png)

    *Confirmar alterações*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tarefa 3 – publicar um aplicativo ASP.NET MVC 4 usando a implantação da Web

1. Volte para a solução do ASP.NET MVC 4. No **Gerenciador de soluções**, clique com botão direito no projeto de site da web e selecione **publicar**.

    ![O aplicativo de publicação](whats-new-in-web-forms-in-aspnet-45/_static/image42.png "publicando o aplicativo")

    *Publicar o site da web*
2. Importe o perfil de publicação que você salvou na primeira tarefa.

    ![Importando o perfil de publicação](whats-new-in-web-forms-in-aspnet-45/_static/image43.png "importando o perfil de publicação")

    *Importando o perfil de publicação*
3. Clique em **validar Conexão**. Depois que a validação estiver concluída, clique em **próxima**.

    > [!NOTE]
    > A validação é concluída quando você vir uma marca de seleção verde aparecer ao lado do botão Validar Conexão.

    ![Validar conexão](whats-new-in-web-forms-in-aspnet-45/_static/image44.png "validar conexão")

    *Validando a conexão*
4. No **as configurações** página na **bancos de dados** seção, clique no botão ao lado da caixa de texto do sua conexão banco de dados (ou seja, **DefaultConnection**).

    ![Configuração de implantação da Web](whats-new-in-web-forms-in-aspnet-45/_static/image45.png "configuração de implantação da Web")

    *Configuração de implantação da Web*
5. Configure a conexão de banco de dados da seguinte maneira:

   - No **nome do servidor** digite sua URL de servidor de banco de dados SQL usando o *tcp:* prefixo.
   - Na **nome de usuário** digite seu nome de logon de administrador do servidor.
   - Na **senha** digite sua senha de logon de administrador do servidor.
   - Digite um novo nome de banco de dados.

     ![Configurando a cadeia de caracteres de conexão de destino](whats-new-in-web-forms-in-aspnet-45/_static/image46.png "Configurando a cadeia de caracteres de conexão de destino")

     *Configurando a cadeia de caracteres de conexão de destino*
6. Clique em **OK**. Quando for solicitado a criar o banco de dados, clique em **Sim**.

    ![Criando o banco de dados](whats-new-in-web-forms-in-aspnet-45/_static/image47.png "criando a cadeia de caracteres de banco de dados")

    *Criando o banco de dados*
7. A cadeia de caracteres de conexão que você usará para se conectar ao banco de dados SQL no Azure é mostrada na caixa de texto de Conexão padrão. Clique em **Avançar**.

    ![Cadeia de caracteres de Conexão que aponta para o banco de dados SQL](whats-new-in-web-forms-in-aspnet-45/_static/image48.png "cadeia de caracteres de Conexão que aponta para o banco de dados SQL")

    *Cadeia de caracteres de Conexão que aponta para o banco de dados SQL*
8. No **versão prévia** , clique em **publicar**.

    ![Publicando o aplicativo web](whats-new-in-web-forms-in-aspnet-45/_static/image49.png "publicando o aplicativo web")

    *Publicar o aplicativo da web*
9. Quando o processo de publicação for concluído, o navegador padrão abrirá o site publicado.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Apêndice c: usar trechos de código

Com trechos de código, você tem todo o código que necessário ao seu alcance. O documento de laboratório lhe dirá exatamente quando você pode usá-los, conforme mostrado na figura a seguir.

![Usar trechos de código do Visual Studio para inserir código em seu projeto](whats-new-in-web-forms-in-aspnet-45/_static/image50.png "trechos de código usando o Visual Studio para inserir código em seu projeto")

*Usar trechos de código do Visual Studio para inserir código em seu projeto*

***Para adicionar um trecho de código usando o teclado (somente c#)***

1. Coloque o cursor onde você deseja inserir o código.
2. Comece a digitar o nome do trecho de código (sem espaços ou hifens).
3. Assista como o IntelliSense exibe os nomes dos trechos de código correspondentes.
4. Selecione o trecho de código correto (ou continue digitando até que o nome do trecho toda está selecionado).
5. Pressione a tecla Tab duas vezes para inserir o trecho de código no local do cursor.

![Comece a digitar o nome do trecho](whats-new-in-web-forms-in-aspnet-45/_static/image51.png "comece a digitar o nome do trecho de código")

*Comece a digitar o nome do trecho de código*

![Pressione Tab para selecionar o trecho de código realçado](whats-new-in-web-forms-in-aspnet-45/_static/image52.png "pressione Tab para selecionar o trecho de código realçado")

*Pressione Tab para selecionar o trecho de código realçado*

![Pressione Tab novamente e o trecho de código serão expandido](whats-new-in-web-forms-in-aspnet-45/_static/image53.png "pressione Tab novamente e o trecho de código serão expandido")

*Pressione Tab novamente e o trecho de código serão expandido*

***Para adicionar um trecho de código usando o mouse (c#, Visual Basic e XML)*** 1. Clique com botão direito no qual você deseja inserir o trecho de código.

1. Selecione **Inserir trecho** seguido **Meus trechos de código**.
2. Selecione o trecho relevante na lista, clicando nele.

![Clique com botão direito no qual você deseja inserir o trecho de código e selecione Insert Snippet](whats-new-in-web-forms-in-aspnet-45/_static/image54.png "botão direito do mouse em que você deseja inserir o trecho de código e selecione Insert Snippet")

*Clique com botão direito no qual você deseja inserir o trecho de código e selecione Insert Snippet*

![Escolher o trecho relevante na lista, clicando nele](whats-new-in-web-forms-in-aspnet-45/_static/image55.png "escolher o trecho relevante na lista, clicando nele")

*Escolher o trecho relevante na lista, clicando nele*
