---
uid: web-pages/overview/data/5-working-with-data
title: Introdução ao trabalho com um banco de dados da Web do ASP.NET (Razor) Sites de páginas | Microsoft Docs
author: tfitzmac
description: Este capítulo descreve como acessar dados de um banco de dados e exibi-lo usando as páginas da Web ASP.NET.
ms.author: aspnetcontent
ms.date: 02/18/2014
ms.assetid: 673d502f-2c16-4a6f-bb63-dbfd9a77ef47
msc.legacyurl: /web-pages/overview/data/5-working-with-data
msc.type: authoredcontent
ms.openlocfilehash: 5185769530cf78c301f2ac43b25dba6e77ca75a2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808500"
---
<a name="introduction-to-working-with-a-database-in-aspnet-web-pages-razor-sites"></a>Introdução ao trabalho com um banco de dados da Web do ASP.NET (Razor) Sites de páginas
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo descreve como usar as ferramentas do Microsoft WebMatrix para criar um banco de dados em um site de páginas da Web do ASP.NET (Razor) e como criar páginas que permitem a você exibir, adicionar, editar e excluir dados.
> 
> **O que você aprenderá:** 
> 
> - Como criar um banco de dados.
> - Como se conectar a um banco de dados.
> - Como exibir dados em uma página da web.
> - Como inserir, atualizar e excluir registros do banco de dados.
> 
> Estes são os recursos introduzidos no artigo:
> 
> - Trabalhando com um banco de dados do Microsoft SQL Server Compact Edition.
> - Trabalhando com consultas SQL.
> - O `Database` classe.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 2
> - O WebMatrix 2
>   
> 
> Este tutorial também funciona com o WebMatrix 3. Você pode usar 3 de páginas da Web do ASP.NET e Visual Studio 2013 (ou o Visual Studio Express 2013 para Web); No entanto, a interface do usuário será diferente.


## <a name="introduction-to-databases"></a>Introdução aos bancos de dados

Imagine um catálogo de endereços típico. Para cada entrada no catálogo de endereços (ou seja, para cada pessoa) você tem várias partes de informações como nome, sobrenome, endereço, endereço de email e número de telefone.

Uma maneira comum de dados de imagem como esta é uma tabela com linhas e colunas. Em termos de banco de dados, cada linha é conhecida como um registro. Cada coluna (também conhecida como campos) contém um valor para cada tipo de dados: nome, nome do último e assim por diante.

| **ID** | **FirstName** | **LastName** | **Endereço** | **Email** | **Telefone** |
| --- | --- | --- | --- | --- | --- |
| 1 | Jim | Abrus | 210 100th St SE Orcas WA 98031 | jim@contoso.com | 555 0100 |
| 2 | Diogo | Adams | 1234 Main St. Seattle WA 99011 | terry@cohowinery.com | 555 0101 |

Para a maioria das tabelas de banco de dados, a tabela deve ter uma coluna que contém um identificador exclusivo, como um número de cliente, o número da conta, etc. Isso é conhecido como a tabela *chave primária*, e usá-lo para identificar cada linha na tabela. No exemplo, a coluna ID é a chave primária para o catálogo de endereços.

Com essa compreensão básica dos bancos de dados, você está pronto para aprender a criar um banco de dados simple e executar operações como adicionar, modificar e excluir dados.

> [!TIP] 
> 
> **Bancos de dados relacionais**
> 
> Você pode armazenar dados de várias formas, incluindo planilhas e arquivos de texto. Para a maioria dos usos de negócios, no entanto, dados são armazenados no banco de dados relacional.
> 
> Este artigo não vai muito profundamente em bancos de dados. No entanto, talvez seja útil para entender um pouco sobre eles. Em um banco de dados relacional, as informações é logicamente divididas em tabelas separadas. Por exemplo, um banco de dados de uma escola pode conter tabelas separadas para os alunos em ofertas de classe. Os banco de dados software (como o SQL Server) oferece suporte a comandos poderosos que permitem que você dinamicamente estabelecem relações entre as tabelas. Por exemplo, você pode usar o banco de dados relacional para estabelecer uma relação lógica entre classes e os alunos para criar uma agenda. Armazenando dados em tabelas separadas reduz a complexidade da estrutura de tabela e reduz a necessidade de manter dados redundantes em tabelas.


## <a name="creating-a-database"></a>Criando um banco de dados

Este procedimento mostra como criar um banco de dados denominado SmallBakery usando a ferramenta de design de banco de dados do SQL Server Compact está incluída no WebMatrix. Embora você possa criar um banco de dados usando o código, é mais comum para criar o banco de dados e tabelas de banco de dados usando uma ferramenta de design como o WebMatrix.

1. Inicie o WebMatrix e, na página de início rápido, clique em **Site a partir do modelo**.
2. Selecione **Site vazio**e, nas **nome do Site** caixa Insira "SmallBakery" e, em seguida, clique em **Okey**. O site é criado e exibido no WebMatrix.
3. No painel esquerdo, clique no **bancos de dados** espaço de trabalho.
4. Na faixa de opções, clique em **novo banco de dados**. Um banco de dados vazio é criado com o mesmo nome de seu site.
5. No painel esquerdo, expanda o **SmallBakery.sdf** nó e clique **tabelas**.
6. Na faixa de opções, clique em **nova tabela**. O WebMatrix abre o designer de tabela.

    ![[imagem]](5-working-with-data/_static/image1.jpg)
7. Clique na **nome** coluna e digite &quot;Id&quot;.
8. No **tipo de dados** coluna, selecione **int**.
9. Defina as **é a chave primária?** e **é identificar?** opções para **Sim**.

    Como o nome sugere, **é a chave primária** informa que o banco de dados que isso será a chave primária da tabela. **É identidade** informa ao banco de dados para criar automaticamente um número de identificação para cada novo registro e atribuí-lo o próximo número sequencial (começando em 1).
10. Clique na próxima linha. O editor inicia uma nova definição de coluna.
11. Para o valor de nome, digite &quot;nome&quot;.
12. Para **tipo de dados**, escolha &quot;nvarchar&quot; e defina o comprimento como 50. O *var* fazem parte do `nvarchar` informa que o banco de dados que os dados para essa coluna será uma cadeia de caracteres cujo tamanho pode variar de registro em registro. (O *n* prefixo representa *national*, que indica que o campo pode conter dados de caracteres que representa qualquer alfabeto ou sistema de gravação &#8212; , ou seja, que o campo contém dados Unicode.)
13. Defina a **permitir nulos** opção **não**. Isso irá impor que o *nome* coluna não for deixada em branco.
14. Usando esse mesmo processo, crie uma coluna chamada *descrição*. Definir **tipo de dados** "nvarchar" e 50 para o comprimento e o conjunto **Allow Nulls** como false.
15. Criar uma coluna denominada *preço*. Definir **tipo de dados "money"** e defina **Allow Nulls** como false.
16. Na caixa na parte superior, nomeie a tabela &quot;produto&quot;.

    Quando terminar, a definição terá esta aparência:

    ![[imagem]](5-working-with-data/_static/image2.jpg)
17. Pressione Ctrl + S para salvar a tabela.

## <a name="adding-data-to-the-database"></a>Adicionando dados ao banco de dados

Agora você pode adicionar alguns dados de exemplo para seu banco de dados que você trabalhará com posteriormente neste artigo.

1. No painel esquerdo, expanda o **SmallBakery.sdf** nó e clique **tabelas**.
2. A tabela de produto com o botão direito e, em seguida, clique em **dados**.
3. No painel de edição, digite os seguintes registros:

    | **Nome** | **Descrição** | **Preço** |
    | --- | --- | --- |
    | Pão | Novo incorporada todos os dias. | 2.99 |
    | Shortcake morango | Feita com Morangos orgânicos do nosso jardim. | 9.99 |
    | Pizza da Apple | Segundo, somente a pizza da sua mãe. | 12.99 |
    | Pizza pecan | Se você gosta pecans, isso é para você. | 10.99 |
    | Torta de limão | Feitas com os Limões melhor do mundo. | 11.99 |
    | Cupcakes | Seus filhos e a criança em que você vai adorar esses. | 7.99 |

    Lembre-se de que você não precisa inserir nada para o *Id* coluna. Quando você criou o *identificação* coluna, defina seu **é identidade** propriedade como true, o que faz com que ele ser preenchidos automaticamente.

    Quando você terminar de inserir os dados, o designer de tabela terá esta aparência:

    ![[imagem]](5-working-with-data/_static/image3.jpg)
4. Feche a guia que contém os dados do banco de dados.

## <a name="displaying-data-from-a-database"></a>Exibir dados de um banco de dados

Depois que você tem um banco de dados com os dados contidos nela, você pode exibir os dados em uma página da web ASP.NET. Para selecionar as linhas da tabela para exibir, você deve usar uma instrução SQL, que é um comando que você passa para o banco de dados.

1. No painel esquerdo, clique no **arquivos** espaço de trabalho.
2. Na raiz do site, crie uma nova página CSHTML chamada *ListProducts.cshtml*.
3. Substitua a marcação existente pelo seguinte:

    [!code-cshtml[Main](5-working-with-data/samples/sample1.cshtml)]

    O primeiro bloco de código, você abre o *SmallBakery.sdf* arquivo (banco de dados) que você criou anteriormente. O `Database.Open` método pressupõe que o *sdf* arquivo está em seu site *App\_dados* pasta. (Observe que você não precisa especificar o *sdf* extensão &#8212; na verdade, se você fizer isso, o `Open` método não funcionará.)

    > [!NOTE]
    > O *App\_dados* pasta é uma pasta especial no ASP.NET que é usado para armazenar arquivos de dados. Para obter mais informações, consulte [conectando-se a um banco de dados](#SB_ConnectingToADatabase) mais adiante neste artigo.

    Em seguida, fazer uma solicitação para consultar o banco de dados usando o SQL a seguir `Select` instrução:

    [!code-sql[Main](5-working-with-data/samples/sample2.sql)]

    Na instrução, `Product` identifica a tabela à consulta. O `*` caractere Especifica que a consulta deve retornar todas as colunas da tabela. (Você pode também listar colunas individualmente, separados por vírgulas, se você quiser ver somente algumas das colunas.) O `Order By` cláusula indica como os dados devem ser classificados &#8212; nesse caso, pelo *nome* coluna. Isso significa que os dados são classificados em ordem alfabética com base no valor da *nome* coluna para cada linha.

    No corpo da página, a marcação cria uma tabela HTML que será usada para exibir os dados. Dentro de `<tbody>` elemento, use um `foreach` loop obter individualmente cada linha de dados que é retornada pela consulta. Para cada linha de dados, você cria uma linha da tabela HTML (`<tr>` elemento). Em seguida, você cria células da tabela HTML (`<td>` elementos) para cada coluna. Cada vez que você percorre o loop, a próxima linha disponível do banco de dados está no `row` variável (você fazê-lo no `foreach` instrução). Para obter uma coluna individual da linha, você pode usar `row.Name` ou `row.Description` ou é de seja qual for o nome da coluna você deseja.
4. Execute a página em um navegador. (Certifique-se de que a página está selecionada na **arquivos** espaço de trabalho antes de executá-lo.) A página exibe uma lista semelhante ao seguinte:

    ![[imagem]](5-working-with-data/_static/image4.jpg)

> [!TIP] 
> 
> **Linguagem de consulta estruturada (SQL)**
> 
> SQL é uma linguagem que é usada na maioria dos bancos de dados relacionais para o gerenciamento de dados em um banco de dados. Ele inclui comandos que permitem que você recuperar dados e atualizá-lo e que permitem que você criar, modificar e gerenciar tabelas de banco de dados. SQL é diferente de uma linguagem de programação (como aquele que você está usando no WebMatrix) porque com SQL, a ideia é que você informar o banco de dados que você deseja e trabalho do banco de dados é descobrir como obter os dados ou executar a tarefa. Aqui estão exemplos de alguns comandos SQL e o que fazer:
> 
> `SELECT Id, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> Isso busca a *identificação*, *nome*, e *preço* colunas de registros no *produto* tabela se o valor de *preço* for maior que 10 e retorna os resultados em ordem alfabética com base nos valores da *nome* coluna. Esse comando retornará um conjunto de resultados que contém os registros que satisfazem os critérios ou um conjunto vazio se nenhum registro correspondente.
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ("Croissant", "A flaky delight", 1.99)`
> 
> Isso insere um novo registro na *produto* tabela, definindo o *nome* coluna para &quot;Croissant&quot;, o *descrição* coluna &quot; A felicidade instável&quot;e o preço para 1.99.
> 
> `DELETE FROM Product WHERE ExpirationDate < "01/01/2008"`
> 
> Esse comando exclui registros na *produto* tabela cuja coluna de data de vencimento é anterior a 1º de janeiro de 2008. (Isso pressupõe que o *produto* tabela tem uma coluna desse tipo, é claro.) A data é inserida aqui no formato MM/DD/AAAA, mas ele deve ser inserido no formato que é usado para sua localidade.
> 
> O `Insert Into` e `Delete` comandos não retornam conjuntos de resultados. Em vez disso, eles retornam um número que informa quantos registros foram afetado pelo comando.
> 
> Para algumas dessas operações (como inserindo e excluindo registros), o processo que está solicitando a operação deve ter as permissões apropriadas no banco de dados. É por isso que para bancos de dados de produção que muitas vezes, é preciso fornecer um nome de usuário e uma senha ao se conectar ao banco de dados.
> 
> Existem dezenas de comandos SQL, mas todos eles seguem um padrão como este. Você pode usar comandos SQL para criar tabelas de banco de dados, contar o número de registros em uma tabela, calcular preços e realizar várias operações mais.


## <a name="inserting-data-in-a-database"></a>Inserindo dados em um banco de dados

Esta seção mostra como criar uma página que permite aos usuários adicionar um novo produto para o *produto* tabela de banco de dados. Depois de inserir um novo registro de produto, a página exibe a tabela atualizada usando o *ListProducts.cshtml* página que você criou na seção anterior.

A página inclui validação para certificar-se de que os dados que o usuário insere são válidos para o banco de dados. Por exemplo, o código na página torna-se de que tenha sido inserido um valor para todas as colunas necessárias.

1. No site, criar um novo arquivo CSHTML chamado *InsertProducts.cshtml*.
2. Substitua a marcação existente pelo seguinte:

    [!code-cshtml[Main](5-working-with-data/samples/sample3.cshtml)]

    O corpo da página contém um formulário HTML com três caixas de texto que permitem aos usuários inserir um nome, descrição e preço. Quando os usuários clicarem o **inserir** botão, o código na parte superior da página abre uma conexão para o *SmallBakery.sdf* banco de dados. Você não terá os valores que o usuário ter enviado usando o `Request` do objeto e atribuir esses valores a variáveis locais.

    Para validar que o usuário inseriu um valor para cada coluna necessária, registre cada `<input>` elemento que você deseja validar:

    [!code-csharp[Main](5-working-with-data/samples/sample4.cs)]

    O `Validation` auxiliar verifica que há um valor em cada um dos campos que você registrou. Você pode testar se todos os campos passou na validação, verificando `Validation.IsValid()`, que você costuma fazer antes de processar as informações que você pode obter do usuário:

    [!code-csharp[Main](5-working-with-data/samples/sample5.cs)]

    (O `&&` significa que o operador AND — esse teste é *se esse for o envio de um formulário e todos os campos passaram na validação*.)

    Se todas as colunas validado (nenhum for vazia), vá em frente e crie uma instrução SQL para inserir os dados e, em seguida, executá-lo, conforme mostrado a seguir:

    [!code-csharp[Main](5-working-with-data/samples/sample6.cs)]

    Para os valores a serem inseridos, incluem espaços reservados de parâmetro (`@0`, `@1`, `@2`).

    > [!NOTE]
    > Como uma precaução de segurança, sempre passe valores para uma instrução SQL usando parâmetros, como você pode ver no exemplo anterior. Isso lhe dá a oportunidade de validar os dados do usuário, além de ajuda a proteger contra tentativas de enviar comandos mal-intencionados para seu banco de dados (também conhecido como ataques de injeção de SQL).

    Para executar a consulta, use esta instrução, passando a ele as variáveis que contêm os valores para substituir os espaços reservados:

    [!code-csharp[Main](5-working-with-data/samples/sample7.cs)]

    Após o `Insert Into` instrução foi executada, você enviar o usuário para a página que lista os produtos usando esta linha:

    [!code-javascript[Main](5-working-with-data/samples/sample8.js)]

    Se a validação não for bem-sucedida, você pode ignorar a inserção. Em vez disso, você tem um auxiliar na página que pode exibir as mensagens de erro acumulado (se houver):

    [!code-cshtml[Main](5-working-with-data/samples/sample9.cshtml)]

    Observe que o bloco de estilo na marcação inclui uma definição de classe CSS chamada `.validation-summary-errors`. Esse é o nome da classe CSS que é usado por padrão para o `<div>` elemento que contém erros de validação. Nesse caso, a classe CSS especifica que erros de resumo de validação são exibidos em vermelho e em negrito, mas você pode definir o `.validation-summary-errors` classe para exibir qualquer formatação que você deseja.

### <a name="testing-the-insert-page"></a>Testando a página de inserção

1. Exiba a página em um navegador. A página exibe um formulário que é semelhante ao que é mostrado na ilustração a seguir.

    ![[imagem]](5-working-with-data/_static/image5.jpg)
2. Insira valores para todas as colunas, mas certifique-se de que você deixe o *preço* coluna em branco.
3. Clique em **Inserir**. A página exibe uma mensagem de erro, conforme mostrado na ilustração a seguir. (Nenhum novo registro é criado.)

    ![[imagem]](5-working-with-data/_static/image6.jpg)
4. Preencha o formulário totalmente e, em seguida, clique em **inserir**. Neste momento, o *ListProducts.cshtml* página é exibida e mostra o novo registro.

## <a name="updating-data-in-a-database"></a>Atualizando dados em um banco de dados

Depois que dados foram inseridos em uma tabela, talvez seja necessário atualizá-lo. Este procedimento mostra como criar duas páginas que são semelhantes aos que você criou para a inserção de dados anteriormente. A primeira página exibe os produtos e permite aos usuários selecionar um para alterar. A segunda página permite que os usuários, na verdade, faça as edições e salvá-los.

> [!NOTE] 
> 
> **Importante** em um site de produção, você normalmente restringir quem tem permissão para fazer alterações aos dados. Para obter informações sobre como configurar a associação e sobre as maneiras de autorizar usuários a executar tarefas no site, consulte [adicionando segurança e associação a um Site de páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202904).


1. No site, criar um novo arquivo CSHTML chamado *EditProducts.cshtml*.
2. Substitua a marcação existente no arquivo pelo seguinte:

    [!code-cshtml[Main](5-working-with-data/samples/sample10.cshtml)]

    A única diferença entre essa página e o *ListProducts.cshtml* página de versões anteriores é que a tabela HTML nesta página inclui uma coluna extra que exibe um **editar** link. Quando você clica nesse link, será levado para o *UpdateProducts.cshtml* página (o que você criará em seguida) onde você pode editar o registro selecionado.

    Examinar o código que cria o **editar** link:

    [!code-cshtml[Main](5-working-with-data/samples/sample11.cshtml)]

    Isso cria um HTML `<a>` elemento cujo `href` atributo é definido dinamicamente. O `href` atributo especifica a página a ser exibido quando o usuário clica no link. Ela também passa a `Id` valor da linha atual para o link. Quando a página é executada, a origem da página pode conter links como estes:

    [!code-html[Main](5-working-with-data/samples/sample12.html)]

    Observe que o `href` atributo é definido como `UpdateProducts/n`, onde *n* é um número de produto. Quando um usuário clica em um desses links, a URL resultante será algo parecido com isso:

    `http://localhost:18816/UpdateProducts/6`

    Em outras palavras, o número do produto a ser editada será passado na URL.
3. Exiba a página em um navegador. A página exibe os dados em um formato como este:

    ![[imagem]](5-working-with-data/_static/image7.jpg)

    Em seguida, você criará a página que permite que os usuários realmente atualizar os dados. A página de atualização inclui validação para validar os dados que o usuário insere. Por exemplo, o código na página torna-se de que tenha sido inserido um valor para todas as colunas necessárias.
4. No site, criar um novo arquivo CSHTML chamado *UpdateProducts.cshtml*.
5. Substitua a marcação existente no arquivo pelo seguinte.

    [!code-cshtml[Main](5-working-with-data/samples/sample13.cshtml)]

    O corpo da página contém um formulário HTML em que um produto é exibido e onde os usuários podem editá-lo. Para obter o produto para exibir, você deve usar essa instrução SQL:

    [!code-sql[Main](5-working-with-data/samples/sample14.sql)]

    Isso selecionará o produto cuja identificação corresponde ao valor que é passado a `@0` parâmetro. (Porque *Id* é a chave primária e, portanto, deve ser exclusivo, registro de apenas um produto nunca pode ser selecionado dessa forma.) Para obter o valor de ID para passar a este `Select` instrução, você pode ler o valor que é passado para a página como parte da URL, usando a seguinte sintaxe:

    [!code-csharp[Main](5-working-with-data/samples/sample15.cs)]

    Para buscar, na verdade, o registro do produto, você deve usar o `QuerySingle` método, que retornará apenas um registro:

    [!code-csharp[Main](5-working-with-data/samples/sample16.cs)]

    A única linha é retornada para o `row` variável. Você pode obter dados para fora de cada coluna e atribuí-lo a variáveis locais como este:

    [!code-csharp[Main](5-working-with-data/samples/sample17.cs)]

    A marcação para o formulário, esses valores são exibidos automaticamente nas caixas de texto individuais usando o código inserido com o seguinte:

    [!code-html[Main](5-working-with-data/samples/sample18.html)]

    Essa parte do código exibe o registro de produto a ser atualizado. Depois que o registro foi exibido, o usuário pode editar colunas individuais.

    Quando o usuário envia o formulário clicando o **atualização** botão, o código a `if(IsPost)` execução do bloco. Isso obtém os valores do usuário da `Request` armazena os valores nas variáveis de objeto e valida que cada coluna tiver sido preenchida. Se a validação é bem-sucedida, o código cria a seguinte instrução SQL Update:

    [!code-sql[Main](5-working-with-data/samples/sample19.sql)]

    Em um SQL `Update` instrução, que você especificar cada coluna para a atualização e defini-lo como o valor. Nesse código, os valores são especificados usando os espaços reservados de parâmetro `@0`, `@1`, `@2`e assim por diante. (Conforme observado anteriormente, para segurança, você sempre deve passar valores para uma instrução SQL usando parâmetros.)

    Quando você chama o `db.Execute` método, você passa as variáveis que contêm os valores na ordem em que corresponde aos parâmetros na instrução SQL:

    [!code-csharp[Main](5-working-with-data/samples/sample20.cs)]

    Após o `Update` instrução tiver sido executada, você chama o método a seguir para redirecionar o usuário voltar à página Editar:

    [!code-cshtml[Main](5-working-with-data/samples/sample21.cshtml)]

    O efeito é que o usuário vê uma lista atualizada dos dados no banco de dados e pode editar o outro produto.
6. Salve a página.
7. Execute o *EditProducts.cshtml* página (não a página de atualização) e depois clique em **editar** para selecionar um produto para editar. O *UpdateProducts.cshtml* página for exibida, mostrando o registro selecionado.

    ![[imagem]](5-working-with-data/_static/image8.jpg)
8. Faça uma alteração e clique em **atualização**. A lista de produtos é mostrada novamente com seus dados atualizados.

## <a name="deleting-data-in-a-database"></a>Exclusão de dados em um banco de dados

Esta seção mostra como permitir que os usuários a excluir um produto do *produto* tabela de banco de dados. O exemplo consiste em duas páginas. Na primeira página, os usuários selecionar um registro a ser excluído. O registro a ser excluído, em seguida, é exibido em uma segunda página que permite que eles confirmar que deseja excluir o registro.

> [!NOTE] 
> 
> **Importante** em um site de produção, você normalmente restringir quem tem permissão para fazer alterações aos dados. Para obter informações sobre como configurar a associação e sobre as maneiras de autorizar o usuário executar tarefas no site, consulte [adicionando segurança e associação a um Site de páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202904).


1. No site, criar um novo arquivo CSHTML chamado *ListProductsForDelete.cshtml*.
2. Substitua a marcação existente pelo seguinte:

    [!code-cshtml[Main](5-working-with-data/samples/sample22.cshtml)]

    Esta página é semelhante para o *EditProducts.cshtml* página anterior. No entanto, em vez de exibir uma **edite** link para cada produto, ele exibe um **excluir** link. O **excluir** link é criado usando o seguinte código incorporado na marcação:

    [!code-cshtml[Main](5-working-with-data/samples/sample23.cshtml)]

    Isso cria uma URL que se parece com isso, quando os usuários clicarem no link:

    `http://<server>/DeleteProduct/4`

    A URL que chama uma página chamada *DeleteProduct.cshtml* (que você criará em seguida) e o passa a ID do produto a ser excluído (aqui, 4).
3. Salve o arquivo, mas deixá-la aberta.
4. Crie outro arquivo CHTML denominado *DeleteProduct.cshtml*. Substitua o conteúdo existente pelo seguinte:

    [!code-cshtml[Main](5-working-with-data/samples/sample24.cshtml)]

    Esta página é chamada pelo *ListProductsForDelete.cshtml* e permite aos usuários confirmar que deseja excluir um produto. Para listar o produto a ser excluído, obtenha a identificação do produto para excluir a URL usando o seguinte código:

    [!code-csharp[Main](5-working-with-data/samples/sample25.cs)]

    A página, em seguida, solicita que o usuário clicar em um botão para excluir, na verdade, o registro. Isso é uma medida de segurança importante: quando você executa operações confidenciais em seu site como atualização ou exclusão de dados, essas operações sempre devem ser feitas usando uma operação POST, não é uma operação GET. Se seu site estiver configurado para que uma operação de exclusão pode ser executada usando uma operação GET, qualquer pessoa pode passar uma URL como `http://<server>/DeleteProduct/4` e excluir qualquer coisa que eles querem do seu banco de dados. Adicionando a confirmação e a página de codificação para que a exclusão pode ser executada usando uma POSTAGEM, você pode adicionar uma medida de segurança ao seu site.

    A operação de exclusão real é executada usando o código a seguir, que primeiro confirma que se trata de uma operação post e se a ID não está vazia:

    [!code-csharp[Main](5-working-with-data/samples/sample26.cs)]

    O código executa uma instrução SQL que exclui o registro especificado e, em seguida, redireciona o usuário para a página de listagem.
5. Execute *ListProductsForDelete.cshtml* em um navegador.

    ![[imagem]](5-working-with-data/_static/image9.jpg)
6. Clique o **excluir** link para um dos produtos. O *DeleteProduct.cshtml* página é exibida para confirmar que você deseja excluir esse registro.
7. Clique o **excluir** botão. O registro do produto será excluído e a página é atualizada com uma listagem de produtos atualizada.

> [!TIP]
> 
> <a id="SB_ConnectingToADatabase"></a>
> ### <a name="connecting-to-a-database"></a>Conectar-se a um banco de dados
> 
> Você pode se conectar a um banco de dados de duas maneiras. A primeira é usar o `Database.Open` método e para especificar o nome do arquivo de banco de dados (menos o *sdf* extensão):
> 
> `var db = Database.Open("SmallBakery");`
> 
> O `Open` método pressupõe que o. *sdf* arquivo está em um site do *App\_dados* pasta. Essa pasta foi projetada especificamente para armazenar dados. Por exemplo, ele tem as permissões apropriadas para permitir que o site para ler e gravar dados e como uma medida de segurança, o WebMatrix não permite acesso aos arquivos dessa pasta.
> 
> A segunda maneira é usar uma cadeia de caracteres de conexão. Uma cadeia de caracteres de conexão contém informações sobre como se conectar a um banco de dados. Isso pode incluir um caminho de arquivo, ou ele pode incluir o nome de um banco de dados do SQL Server em um servidor local ou remoto, juntamente com um nome de usuário e senha para se conectar a esse servidor. (Se você mantiver os dados em uma versão gerenciada centralmente do SQL Server, como no site de um provedor de hospedagem, você sempre use uma cadeia de caracteres de conexão para especificar as informações de conexão de banco de dados.)
> 
> No WebMatrix, cadeias de caracteres de conexão são geralmente armazenadas em um arquivo XML denominado *Web. config*. Como o nome implica, você pode usar um *Web. config* arquivo na raiz do seu site para armazenar informações de configuração do site, incluindo quaisquer cadeias de caracteres de conexão que pode exigir a seu site. Um exemplo de uma cadeia de caracteres de conexão em um *Web. config* arquivo pode parecer com o seguinte:
> 
> [!code-xml[Main](5-working-with-data/samples/sample27.xml)]
> 
> No exemplo, a cadeia de conexão aponta para um banco de dados em uma instância do SQL Server que está em execução em um servidor em algum lugar (em vez de um local *sdf* arquivo). Você precisará substituir os nomes apropriados para `myServer` e `myDatabase`e especifique os valores de logon do SQL Server para `username` e `password`. (Os valores de nome de usuário e senha não são necessariamente os mesmos como suas credenciais do Windows ou como os valores que seu provedor de hospedagem tenha dado a você para fazer logon em seus servidores. Verifique com o administrador para os valores exatos que necessários.)
> 
> O `Database.Open` método é flexível, porque ele permite que você passe o nome de um banco de dados *sdf* arquivo ou o nome de uma cadeia de caracteres de conexão é armazenada no *Web. config* arquivo. O exemplo a seguir mostra como se conectar ao banco de dados usando a cadeia de caracteres de conexão ilustrada no exemplo anterior:
> 
> [!code-cshtml[Main](5-working-with-data/samples/sample28.cshtml)]
> 
> Conforme observado, o `Database.Open` método permite que você passe um nome de banco de dados ou uma cadeia de caracteres de conexão, e ele vai descobrir qual deles usar. Isso é muito útil quando você implantar (publicar) seu site. Você pode usar um *. sdf* arquivo na *App\_dados* pasta quando você está desenvolvendo e testando o seu site. Quando você move o seu site para um servidor de produção, você pode usar uma cadeia de caracteres de conexão na *Web. config* arquivo que tem o mesmo nome que sua *. sdf* de arquivos, mas que aponta para o provedor de hospedagem de banco de dados &#8212;tudo isso sem precisar alterar seu código.
> 
> Por fim, se você quiser trabalhar diretamente com uma cadeia de caracteres de conexão, você pode chamar o `Database.OpenConnectionString` método e passar ele a conexão real da cadeia de caracteres em vez de apenas o nome de um em de *Web. config* arquivo. Isso pode ser útil em situações em que, por algum motivo, você não tem acesso à cadeia de conexão (ou valores, como o *sdf* nome de arquivo) até que a página está em execução. No entanto, na maioria dos cenários, você pode usar `Database.Open` conforme descrito neste artigo.


## <a name="additional-resources"></a>Recursos adicionais

- [SQL Server Compact](https://www.microsoft.com/sqlserver/2008/en/us/compact.aspx)
- [Conectar-se ao SQL Server ou banco de dados MySQL no WebMatrix](https://go.microsoft.com/fwlink/?LinkId=208661)
- [Validação da entrada do usuário em sites de Páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=253002)
