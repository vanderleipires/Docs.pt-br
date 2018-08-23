---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
title: Introdução ao ASP.NET Web Pages – exibindo dados | Microsoft Docs
author: tfitzmac
description: Este tutorial mostra como criar um banco de dados no WebMatrix e como exibir dados de banco de dados em uma página, quando você usa o ASP.NET Web Pages (Razor). Ele pressupõe que y...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: b3a006a0-3ea2-4d45-b833-e20e3a3c0a1a
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
msc.type: authoredcontent
ms.openlocfilehash: 864b9f7826763e307368706116458678abf50d3b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824816"
---
<a name="introducing-aspnet-web-pages---displaying-data"></a>Introdução ao ASP.NET Web Pages - exibindo dados
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial mostra como criar um banco de dados no WebMatrix e como exibir dados de banco de dados em uma página, quando você usa o ASP.NET Web Pages (Razor). Ele pressupõe que você tenha concluído a série por meio [Introdução à programação de páginas da Web do ASP.NET](../introducing-razor-syntax-c.md).
> 
> O que você aprenderá:
> 
> - Como usar as ferramentas do WebMatrix para criar um banco de dados e tabelas de banco de dados.
> - Como usar as ferramentas do WebMatrix para adicionar dados a um banco de dados.
> - Como exibir dados do banco de dados em uma página.
> - Como executar comandos SQL em páginas da Web do ASP.NET.
> - Como personalizar o `WebGrid` auxiliar para alterar a exibição de dados e adicionar paginação e classificação.
>   
> 
> Recursos/tecnologias abordadas:
> 
> - Ferramentas de banco de dados do WebMatrix.
> - `WebGrid` auxiliar.


## <a name="what-youll-build"></a>O que você vai criar

No tutorial anterior, você foi apresentado para páginas da Web do ASP.NET (*. cshtml* arquivos), os conceitos básicos da sintaxe do Razor e auxiliares. Neste tutorial, você começará criando o aplicativo web real que você usará para o restante da série. O aplicativo é um aplicativo de filme simples que lhe permite exibir, adicionar, alterar e excluir informações sobre filmes.

Quando terminar este tutorial, você poderá exibir uma lista de filmes que se parece com esta página:

![Exibição do WebGrid que inclui o conjunto de parâmetros para nomes de classes CSS](displaying-data/_static/image1.png)

Mas, para começar, você precisa criar um banco de dados.

## <a name="a-very-brief-introduction-to-databases"></a>Uma introdução muito breve para bancos de dados

Este tutorial fornecerá apenas a Everybody Introdução aos bancos de dados. Se você tiver experiência de banco de dados, você pode ignorar esta seção curta.

Um banco de dados contém uma ou mais tabelas que contêm informações &mdash; tabelas, por exemplo, para clientes, pedidos e fornecedores ou para os alunos, professores, classes e notas. Estruturalmente, uma tabela de banco de dados é como uma planilha. Imagine um catálogo de endereços típico. Para cada entrada no catálogo de endereços (ou seja, para cada pessoa) você tem várias partes de informações como nome, sobrenome, endereço, endereço de email e número de telefone.

![Tabela de banco de dados de exemplo como uma grade simple](displaying-data/_static/image2.png)

(Linhas são às vezes chamadas de *registros*, e colunas são às vezes denominadas *campos*.)

Para a maioria das tabelas de banco de dados, a tabela deve ter uma coluna que contém um valor exclusivo, como um número de cliente, o número de conta e assim por diante. Esse valor é conhecido como a tabela *chave primária*, e usá-lo para identificar cada linha na tabela. No exemplo, a coluna ID é a chave primária para o catálogo de endereços, mostrado no exemplo anterior.

Grande parte do trabalho que você pode fazer em aplicativos da web consiste em ler as informações do banco de dados e exibi-lo em uma página. Você geralmente também coletar informações de usuários e adicioná-lo a um banco de dados ou modificará registros que já estão no banco de dados. (Abordaremos todas essas operações durante este conjunto de tutoriais.)

Trabalho de banco de dados pode ser extremamente complexo e pode exigir um conhecimento especializado. No entanto, para este conjunto de tutoriais, você precisa compreender apenas os conceitos básicos, que serão todas ser explicados conforme você avança.

## <a name="creating-a-database"></a>Criando um banco de dados

O WebMatrix inclui ferramentas que tornam mais fácil de criar um banco de dados e criar tabelas no banco de dados. (A estrutura de um banco de dados é conhecida como o banco de dados *esquema*.) Para este conjunto de tutoriais, você criará um banco de dados que contém somente uma tabela de &mdash; filmes.

Abrir o WebMatrix, se você ainda não fez isso e abra o site de WebPagesMovies que você criou no tutorial anterior.

No painel esquerdo, clique no **banco de dados** espaço de trabalho.

![Guia do espaço de trabalho de banco de dados do WebMatrix](displaying-data/_static/image3.png)

As alterações de faixa de opções para mostrar as tarefas relacionadas ao banco de dados. Na faixa de opções, clique em **novo banco de dados**.

!['Novo banco de dados' botão na faixa de opções do WebMatrix](displaying-data/_static/image4.png)

O WebMatrix cria um banco de dados do SQL Server CE (um *sdf* arquivo) que tem o mesmo nome como seu site &mdash; *WebPagesMovies.sdf*. (Você não fará isso aqui, mas você pode renomear o arquivo para que desejar, desde que ela tenha um *sdf* extensão.)

## <a name="creating-a-table"></a>Criando uma tabela

Na faixa de opções, clique em **nova tabela**. O WebMatrix abre o designer de tabela em uma nova guia. (Se a opção de nova tabela não estiver disponível, verifique se que o novo banco de dados está selecionado na exibição de árvore à esquerda.)

!['Nova tabela' botão na faixa de opções do WebMatrix](displaying-data/_static/image5.png)

Na caixa de texto na parte superior (em que a marca d'água diz "Nome da tabela Enter"), insira "Filmes".

![Inserir um nome de tabela no designer de banco de dados do WebMatrix](displaying-data/_static/image6.png)

O painel sob o nome da tabela é onde você define as colunas individuais. Para o *filmes* tabela neste tutorial, você criará apenas algumas colunas: *ID*, *título*, *gênero*, e *ano*.

No **nome** , digite "ID". Inserir um valor aqui ativa todos os controles para a nova coluna.

Alterne para o **tipo de dados** lista e escolha **int**. Esse valor Especifica que a coluna de ID conterá dados integer (número).

> [!NOTE]
> Não chamamos o qualquer mais aqui (muito), mas você pode usar gestos de teclado padrão do Windows para navegar nessa grade. Por exemplo, você pode pressionar tab entre os campos, você apenas pode começar a digitar para selecionar um item em uma lista e assim por diante.


Guia após o **valor padrão** caixa (ou seja, deixe em branco). Alterne para o **é a chave primária** caixa de seleção e selecione-o. Essa opção informa ao banco de dados que o *ID* coluna conterá os dados que identificam as linhas individuais. (Ou seja, cada linha terá um valor exclusivo na coluna de ID que você pode usar para encontrar aquela linha.)

Escolha o **é identidade** opção. Essa opção informa ao banco de dados que ele deve gerar automaticamente o próximo número sequencial para cada nova linha. (O **é identidade** opção funciona somente se você tiver selecionado também a **chave primária é** opção.)

Clique na próxima linha de grade ou pressione a tecla Tab duas vezes para concluir a linha atual. O gesto salva a linha atual e inicia o próximo. Observe que o **valor padrão** coluna agora diz **nulo**. (Null é o valor padrão para o valor padrão, por assim dizer.)

Quando você tiver terminado de definir o novo **ID** coluna, o designer será semelhante a esta ilustração:

![O WebMatrix designer de banco de dados depois de definir a coluna de ID para a tabela de filmes](displaying-data/_static/image7.png)

Para criar a próxima coluna, clique na caixa de **nome** coluna. Insira "Title" para a coluna e, em seguida, selecione **nvarchar** para o **tipo de dados** valor. A parte "var" de **nvarchar** informa que o banco de dados que os dados para essa coluna será uma cadeia de caracteres cujo tamanho pode variar de registro em registro. (O prefixo "n" representa "national", que indica que o campo pode conter dados de caractere para qualquer alfabeto ou sistema de gravação — ou seja, o campo contém dados Unicode.)

Quando você escolhe **nvarchar**, outra caixa é exibida, onde você pode inserir o comprimento máximo do campo. Digite 50, supondo que nenhum título do filme que você usará para trabalhar com este tutorial será mais de 50 caracteres.

Skip **valor padrão** e desmarque as **Allow Nulls** opção. Você não deseja que o banco de dados para permitir que qualquer filmes sejam inseridos no banco de dados que não têm um título.

Quando você estiver pronto e move para a próxima linha, o designer se parece com esta ilustração:

![O WebMatrix designer de banco de dados depois de definir a coluna de título para a tabela de filmes](displaying-data/_static/image8.png)

Repita estas etapas para criar uma coluna denominada "Gênero", exceto para o comprimento, defina-o como apenas 30.

Criar outra coluna denominada "Ano". Para o tipo de dados, escolha **nchar** (não **nvarchar**) e defina o tamanho a 4. Para o ano, você vai usar um número de 4 dígitos, como "1995" ou "2010", portanto, você não precisar de uma coluna de tamanho variável.

O design concluído é semelhante ao seguinte:

![O WebMatrix designer de banco de dados depois que todos os campos são definidos para a tabela de filmes](displaying-data/_static/image9.png)

Pressione Ctrl + S ou clique a **salvar** botão na barra de ferramentas de acesso rápido. Feche o designer de banco de dados fechando a guia.

## <a name="adding-some-example-data"></a>Adicionando alguns dados de exemplo

Mais tarde nessa série de tutoriais, você criará uma página onde você pode inserir novos filmes em um formulário. No entanto, por enquanto, você pode adicionar alguns dados de exemplo que você pode exibir em uma página.

No **banco de dados** espaço de trabalho no WebMatrix, observe que há uma árvore que mostra a *sdf* arquivo que você criou anteriormente. Abra o nó para o seu novo *sdf* do arquivo e, em seguida, abra o **tabelas** nó.

![Espaço de trabalho de banco de dados do WebMatrix com árvore de abrir a tabela de filmes](displaying-data/_static/image10.png)

Clique com botão direito do **filmes** nó e, em seguida, escolha **dados**. O WebMatrix é aberto em uma grade onde você pode inserir dados para o *filmes* tabela:

![Grade de entrada de banco de dados no WebMatrix (vazio)](displaying-data/_static/image11.png)

Clique o **título** coluna e digite "Quando Jaime atendidos Sally". Mover para o **gênero** coluna (você pode usar a tecla Tab) e insira "Românticas comédia". Mover para o **ano** coluna e digite "1989":

![Grade de entrada do banco de dados no WebMatrix com um registro](displaying-data/_static/image12.png)

Pressione Enter e o WebMatrix salva o novo filme. Observe que o **ID** coluna tiver sido preenchida.

![Grade de entrada do banco de dados no WebMatrix com um registro e a ID gerada automaticamente](displaying-data/_static/image13.png)

Insira outro filme (por exemplo, "e o vento", "Drama", "1939"). A coluna ID é preenchida novamente:

![Grade de entrada do banco de dados no WebMatrix com dois registros e IDs geradas automaticamente](displaying-data/_static/image14.png)

Insira um filme de terceiro (por exemplo, "Ghostbusters", "Comédia"). Para experimentar, deixe o **ano** coluna em branco e, em seguida, pressione Enter. Porque você desmarcou a **Allow Nulls** opção, o banco de dados mostrará um erro:

![Erro de 'Dados inválidos' exibido se um valor de coluna necessária é deixado em branco](displaying-data/_static/image15.png)

Clique em **Okey** para voltar e corrigir a entrada (o ano para "Ghostbusters" é 1984) e, em seguida, pressione Enter.

Preencha vários filmes até que você tenha de 8 ou forma. (Inserir 8 torna mais fácil trabalhar com paginação mais tarde. Mas se esse for um número excessivo, inserir alguns por enquanto.) Não importam os dados reais.

![Grade de entrada do banco de dados no WebMatrix com qualquer um dos registros](displaying-data/_static/image16.png)

Se você inseriu todos os filmes sem erros, os valores de ID são sequenciais. Se você tentou salvar um registro de filme incompleto, os números de identificação podem não ser sequenciais. Nesse caso, que é okey. Os números não têm nenhum significado inerente e a única coisa que é importante é que eles são exclusivos na *filmes* tabela.

Feche a guia que contém o designer de banco de dados.

Agora você pode ativar para exibir esses dados em uma página da web.

## <a name="displaying-data-in-a-page-by-using-the-webgrid-helper"></a>Exibindo dados em uma página usando o auxiliar do WebGrid

Para exibir dados em uma página, você vai usar o `WebGrid` auxiliar. Esse auxiliar produz uma exibição em uma grade ou uma tabela (linhas e colunas). Como você verá, você será capaz de refinar a grade com formatação e outros recursos.

Para executar a grade, você precisará escrever algumas linhas de código. Algumas dessas linhas servirá como um tipo de padrão para quase todos o acesso a dados que você pode fazer neste tutorial.

> [!NOTE]
> Você realmente tem muitas opções para exibir dados em uma página. o `WebGrid` auxiliar é apenas um. Optamos por ele para este tutorial porque é a maneira mais fácil de exibir dados e porque ele é razoavelmente flexível. O próximo conjunto de tutoriais, você verá como usar um modo "manual" mais para trabalhar com dados na página, que lhe dá controle mais direto sobre como exibir os dados.


No painel esquerdo no WebMatrix, clique o **arquivos** espaço de trabalho.

O novo banco de dados que você criou está no *App\_dados* pasta. Se a pasta ainda não existir, o WebMatrix criou para seu novo banco de dados. (A pasta pode ter existido se anteriormente você tiver instalado os auxiliares.)

Na exibição de árvore, selecione a raiz do site. Você deve selecionar a raiz do site da Web; Caso contrário, o novo arquivo pode ser adicionado ao aplicativo\_pasta de dados.

Na faixa de opções, clique em **New**. No **escolher um tipo de arquivo** , escolha **CSHTML**.

No **nome** caixa, nomeie a nova página "Movies.cshtml":

![Caixa de diálogo 'Escolher um tipo de arquivo' mostrando a página 'Filmes'](displaying-data/_static/image17.png)

Clique o **Okey** botão. O WebMatrix abre um novo arquivo com alguns elementos de esqueleto. Primeiro, você escreverá um código para obter os dados do banco de dados. Em seguida, você adicionará a marcação para a página para exibir os dados.

### <a name="writing-the-data-query-code"></a>Escrever o código de consulta de dados

Na parte superior da página, entre o `@{` e `}` caracteres, insira o código a seguir. (Certifique-se de que você inserir este código entre as chaves de abertura e fechamento).

[!code-csharp[Main](displaying-data/samples/sample1.cs)]

A primeira linha abre o banco de dados que você criou anteriormente, que é sempre a primeira etapa antes de fazer algo com o banco de dados. Você informa o `Database.Open` nome do método do banco de dados para abrir. Observe que você não incluir *sdf* no nome. O `Open` método pressupõe que ele está procurando por um *. sdf* arquivo (ou seja, *WebPagesMovies.sdf*) e que o *. sdf* arquivo está sendo o *aplicativo\_ Dados* pasta. (Anteriormente, percebemos que o *App\_dados* pasta é reservada; nesse cenário é um dos locais em que o ASP.NET faz suposições sobre esse nome.)

Quando o banco de dados é aberto, uma referência a ele é colocada na variável nomeada `db`. (Que poderia ser qualquer nome.) O `db` variável é como você vai acabar interagindo com o banco de dados.

A segunda linha, na verdade, busca os dados de banco de dados usando o `Query` método. Observe como esse código funciona: o `db` variável tem uma referência para o banco de dados aberto e você invocar o `Query` método usando o `db` variável (`db.Query`).

A consulta em si é um SQL `Select` instrução. (Para algumas informações básicas sobre o SQL, consulte a explicação mais tarde). Na instrução, `Movies` identifica a tabela à consulta. O `*` caractere Especifica que a consulta deve retornar todas as colunas da tabela. (Você pode também listar colunas individualmente, separados por vírgulas.)

Os resultados da consulta, se houver, são retornados e disponibilizados no `selectedData` variável. Novamente, a variável pode ser qualquer nome.

Por fim, a terceira linha informa o ASP.NET que você deseja usar uma instância da `WebGrid` auxiliar. Você cria (*instanciar*) o objeto auxiliar usando o `new` palavra-chave e passá-lo os resultados da consulta por meio do `selectedData` variável. O novo `WebGrid` objeto, juntamente com os resultados da consulta de banco de dados, são disponibilizados no `grid` variável. Você precisará que resultam em um momento para exibir os dados na página.

Nesse estágio, o banco de dados foi aberto, você já tem os dados você quer, e que você preparou a `WebGrid` auxiliar com os dados. Em seguida é criar a marcação na página.

> [!TIP] 
> 
> **Linguagem de consulta estruturada (SQL)**
> 
> SQL é uma linguagem que é usada na maioria dos bancos de dados relacionais para o gerenciamento de dados em um banco de dados. Ele inclui comandos que permitem que você recuperar dados e atualizá-lo e que permitem que você criar, modificar e gerenciar dados em tabelas de banco de dados. SQL é diferente de uma linguagem de programação (como c#). Com SQL, você informa o banco de dados que você deseja e trabalho do banco de dados é descobrir como obter os dados ou executar a tarefa. Aqui estão exemplos de alguns comandos SQL e o que fazer:
> 
> `Select * From Movies`
> 
> `SELECT ID, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> A primeira `Select` instrução obtém todas as colunas (especificado por `*`) da *filmes* tabela.
> 
> A segunda `Select` instrução busca as colunas de ID, o nome e o preço dos registros a *produto* tabela cujo valor de coluna de preço é maior que 10. O comando retorna os resultados em ordem alfabética com base nos valores da coluna nome. Se nenhum registro corresponde aos critérios de preço, o comando retorna um conjunto vazio.
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ('Croissant', 'A flaky delight', 1.99)`
> 
> Este comando insere um novo registro para o *produto* tabela, definindo a coluna de nome "Croissant", a coluna de descrição para "Um instável a felicidade" e o preço para 1.99.
> 
> Observe que, quando você estiver especificando um valor não numérico, o valor é colocado entre aspas simples (não as aspas duplas, como em c#). Você usar essas aspas ao redor de valores de texto ou data, mas não em torno de números.
> 
> `DELETE FROM Product WHERE ExpirationDate < '01/01/2008'`
> 
> Esse comando exclui registros na *produto* tabela cuja coluna de data de vencimento é anterior a 1º de janeiro de 2008. (O comando supõe que o *produto* tabela tem uma coluna desse tipo, é claro.) A data é inserida aqui no formato MM/DD/AAAA, mas ele deve ser inserido no formato que é usado para sua localidade.
> 
> O `Insert` e `Delete` comandos não retornam conjuntos de resultados. Em vez disso, eles retornam um número que informa quantos registros foram afetado pelo comando.
> 
> Para algumas dessas operações (como inserindo e excluindo registros), o processo que está solicitando a operação deve ter as permissões apropriadas no banco de dados. Esse é o motivo para bancos de dados de produção que muitas vezes, é preciso fornecer um nome de usuário e senha ao se conectar ao banco de dados.
> 
> Existem dezenas de comandos SQL, mas todos eles seguem um padrão como os comandos que você vê aqui. Você pode usar comandos SQL para criar tabelas de banco de dados, contar o número de registros em uma tabela, calcular preços e realizar várias operações mais.


### <a name="adding-markup-to-display-the-data"></a>Adicionando marcação para exibir os dados

Dentro de `<head>` elemento, o conteúdo do conjunto do `<title>` elemento a ser "Filmes":

[!code-html[Main](displaying-data/samples/sample2.html?highlight=3)]

Dentro de `<body>` elemento da página, adicione o seguinte:

[!code-html[Main](displaying-data/samples/sample3.html)]

Isso é tudo. O `grid` variável é o valor que você criou ao criar o `WebGrid` objeto no código anterior.

Na exibição de árvore do WebMatrix, a página com o botão direito e selecione **iniciar no navegador**. Você verá algo semelhante esta página:

![Saída do auxiliar de WebGrid padrão da tabela de filmes](displaying-data/_static/image18.png)

Clique em um link de título de coluna para classificar por essa coluna. Ser capaz de classificar, clique em um título é um recurso que está incorporado a **WebGrid** auxiliar.

O `GetHtml` método, como o nome sugere, gera marcação que exibe os dados. Por padrão, o `GetHtml` método gera um HTML `<table>` elemento. (Se você quiser, você pode verificar o processamento, observando a código-fonte da página no navegador.)

## <a name="modifying-the-look-of-the-grid"></a>Modificando a aparência da grade

Usando o `WebGrid` auxiliar, como acabou de fazer é fácil, mas a exibição resultante é simples. O `WebGrid` auxiliar tem inúmeras opções que permitem controlar como os dados são exibidos. Há muito mais para explorar neste tutorial, mas esta seção lhe dará uma ideia de algumas dessas opções. Algumas opções adicionais serão abordadas em tutoriais posteriores nesta série.

### <a name="specifying-individual-columns-to-display"></a>Especificando colunas individuais para exibição

Para começar, você pode especificar que você deseja exibir somente certas colunas. Por padrão, como você viu, a grade mostra todas as quatro colunas do *filmes* tabela.

No *Movies.cshtml* do arquivo, substitua o `@grid.GetHtml()` marcação que você acabou de adicionar o seguinte:

[!code-css[Main](displaying-data/samples/sample4.css)]

Para informar o auxiliar de quais colunas serão exibidas, você deve incluir um `columns` parâmetro para o `GetHtml` método e passar em uma coleção de colunas. Na coleção, você deve especificar cada coluna a incluir. Você especifica uma coluna individual para exibir, incluindo um `grid.Column` de objeto e passe no nome da coluna de dados que você deseja. (Essas colunas devem ser incluídas nos resultados da consulta SQL — o `WebGrid` auxiliar não é possível exibir as colunas que não foram retornadas pela consulta.)

Inicie o *Movies.cshtml* página no navegador novamente e, desta vez, você obtém uma exibição como a mostrada a seguir (Observe que nenhuma coluna de ID é exibida):

![Exibição do WebGrid mostrando apenas as colunas selecionadas](displaying-data/_static/image19.png)

### <a name="changing-the-look-of-the-grid"></a>Alterar a aparência da grade

Há muitos mais opções para a exibição de colunas, algumas das quais serão exploradas nos tutoriais posteriores neste conjunto. Por enquanto, esta seção apresenta a você maneiras em que você pode definir o estilo de grade como um todo.

Dentro de `<head>` seção da página, logo antes do fechamento `</head>` marca, adicione o seguinte `<style>` elemento:

[!code-css[Main](displaying-data/samples/sample5.css)]

Essa marcação CSS define classes chamadas `grid`, `head`e assim por diante. Você também pode colocar essas definições de estilo em um separado *. CSS* de arquivo e vinculá-lo para a página. (Na verdade, terá de fazer isso posteriormente neste conjunto de tutoriais.) Mas para facilitar as coisas para este tutorial, eles estão dentro da mesma página que exibe os dados.

Agora você pode obter o `WebGrid` auxiliar para usar essas classes de estilo. O auxiliar tem um número de propriedades (por exemplo, `tableStyle`) para essa finalidade — você atribuir um nome de classe de estilo CSS a eles e esse nome de classe é processado como parte da marcação que é processada pelo auxiliar.

Alterar o `grid.GetHtml` marcação para que ele agora se pareça com este código:

[!code-css[Main](displaying-data/samples/sample6.css)]

O que há de novo aqui é que você adicionou `tableStyle`, `headerStyle`, e `alternatingRowStyle` parâmetros para o `GetHtml` método. Esses parâmetros foram definidos como os nomes dos estilos de CSS que você adicionou um momento atrás.

Execute a página e, neste momento, você verá uma grade que se parece muito menos simples do que antes:

![Exibição do WebGrid que inclui o conjunto de parâmetros para nomes de classes CSS](displaying-data/_static/image20.png)

Para ver o que o `GetHtml` método gerado, você pode examinar a código-fonte da página no navegador. Não entraremos em detalhes aqui, mas o ponto importante é que, com a especificação de parâmetros como `tableStyle`, você fez a grade gerar marcas HTML semelhante ao seguinte:

`<table class="grid">`

O `<table>` marca teve um `class` atributo adicionado a ele que faz referência a uma das regras de CSS que você adicionou anteriormente. Este código mostra o padrão básico &mdash; parâmetros diferentes para o `GetHtml` permitem que você referência de método CSS classes que o método, em seguida, gera juntamente com a marcação. O que fazer com as classes CSS cabe a você.

## <a name="adding-paging"></a>Adicionando paginação

Como a última tarefa para este tutorial, você adicionará a paginação à grade. No momento, não é problema para exibir todos os seus filmes ao mesmo tempo. Mas, se você adicionou a centenas de filmes, a exibição de página obteria longo.

Na página de código, altere a linha que cria o `WebGrid` objeto para o código a seguir:

[!code-csharp[Main](displaying-data/samples/sample7.cs)]

A única diferença do antes é que você adicionou um `rowsPerPage` parâmetro que é definido como 3.

Execute a página. A grade exibe 3 linhas em uma hora, além de links de navegação que permitem que você percorrer os filmes no banco de dados:

![Exibição do WebGrid com paginação](displaying-data/_static/image21.png)

## <a name="coming-up-next"></a>Próximo

No próximo tutorial, você aprenderá como usar o código do Razor e c# para obter a entrada do usuário em um formulário. Você adicionará uma caixa de pesquisa para a página Movies para que você possa encontrar filmes por título ou gênero.

## <a name="complete-listing-for-movies-page"></a>Listagem completa para a página de filmes

[!code-cshtml[Main](displaying-data/samples/sample8.cshtml)]

## <a name="additional-resources"></a>Recursos adicionais

- [Introdução à programação Web do ASP.NET usando a sintaxe Razor](https://go.microsoft.com/fwlink/?LinkID=202890)

> [!div class="step-by-step"]
> [Anterior](intro-to-web-pages-programming.md)
> [Próximo](form-basics.md)
