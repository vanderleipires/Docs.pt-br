---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
title: "Introdução a páginas da Web ASP.NET - exibindo dados | Microsoft Docs"
author: tfitzmac
description: "Este tutorial mostra como criar um banco de dados no WebMatrix e como exibir dados de banco de dados em uma página quando você usa páginas da Web do ASP.NET (Razor). Ele assume y..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: b3a006a0-3ea2-4d45-b833-e20e3a3c0a1a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
msc.type: authoredcontent
ms.openlocfilehash: fdb9af0ba87c7802c63451ac7aa422e0020b5719
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="introducing-aspnet-web-pages---displaying-data"></a>Introdução a páginas da Web ASP.NET - exibindo dados
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial mostra como criar um banco de dados no WebMatrix e como exibir dados de banco de dados em uma página quando você usa páginas da Web do ASP.NET (Razor). Ele pressupõe que você tenha concluído a série por meio de [Introdução à programação de páginas da Web do ASP.NET](../introducing-razor-syntax-c.md).
> 
> O que você aprenderá:
> 
> - Como usar as ferramentas do WebMatrix para criar um banco de dados e tabelas de banco de dados.
> - Como usar ferramentas do WebMatrix para adicionar dados a um banco de dados.
> - Como exibir dados do banco de dados em uma página.
> - Como executar comandos SQL em páginas da Web do ASP.NET.
> - Como personalizar o `WebGrid` auxiliar para alterar a exibição de dados e adicionar paginação e classificação.
>   
> 
> Recursos/tecnologias abordadas:
> 
> - Ferramentas de banco de dados do WebMatrix.
> - `WebGrid`auxiliar.


## <a name="what-youll-build"></a>O que você vai criar

No tutorial anterior, foram introduzidos para páginas da Web do ASP.NET (*. cshtml* arquivos), as Noções básicas da sintaxe do Razor e auxiliares. Neste tutorial, você começará a criar o aplicativo web real que você usará para o restante da série. O aplicativo é um aplicativo de filme simples que permite exibir, adicionar, alterar e excluir informações sobre filmes.

Quando você concluir este tutorial, você poderá exibir uma lista de filmes que se parece com esta página:

![Exibição WebGrid que inclui o conjunto de parâmetros para nomes de classe CSS](displaying-data/_static/image1.png)

Mas, para começar, você precisa criar um banco de dados.

## <a name="a-very-brief-introduction-to-databases"></a>Uma breve introdução aos bancos de dados

Este tutorial fornecerá apenas a introdução dado aos bancos de dados. Se você tiver experiência de banco de dados, você poderá ignorar esta seção curta.

Um banco de dados contém uma ou mais tabelas que contêm informações &mdash; por exemplo, tabelas de clientes, pedidos e fornecedores ou para os alunos, professores, classes e notas. Estruturalmente, uma tabela de banco de dados é como uma planilha. Imagine um catálogo de endereços típico. Para cada entrada no catálogo de endereços (ou seja, para cada pessoa) você tem várias partes de informações como nome, sobrenome, endereço, endereço de email e número de telefone.

![Tabela de banco de dados de exemplo como uma grade simple](displaying-data/_static/image2.png)

(Linhas às vezes são chamadas de *registros*, e colunas são às vezes chamadas de *campos*.)

Para a maioria das tabelas de banco de dados, a tabela deve ter uma coluna que contém um valor exclusivo, como um número de cliente, número de conta e assim por diante. Esse valor é conhecido como a tabela *chave primária*, e você pode usá-lo para identificar cada linha na tabela. No exemplo, a coluna ID é a chave primária para o catálogo de endereços, mostrado no exemplo anterior.

A maioria do trabalho que faria em aplicativos da web consiste em ler informações do banco de dados e exibi-lo em uma página. Geralmente você coletar informações de usuários e adicioná-lo a um banco de dados, ou você modificará os registros que já estão no banco de dados. (Abordaremos todas essas operações no decorrer deste tutorial conjunto.)

Trabalho de banco de dados pode ser muito complexo e pode exigir conhecimento especializado. No entanto, para obter este tutorial, você precisa compreender apenas os conceitos básicos, que serão todas explicados, conforme você avança.

## <a name="creating-a-database"></a>Criando um banco de dados

O WebMatrix inclui ferramentas que tornam mais fácil para criar um banco de dados e criar tabelas no banco de dados. (A estrutura de um banco de dados é conhecida como o banco de dados *esquema*.) Para obter este tutorial, você criará um banco de dados que contém somente uma tabela de &mdash; filmes.

Abrir o WebMatrix, se você ainda não fez isso e abra o site WebPagesMovies que você criou no tutorial anterior.

No painel esquerdo, clique no **banco de dados** espaço de trabalho.

![Guia de espaço de trabalho do banco de dados do WebMatrix](displaying-data/_static/image3.png)

As alterações de faixa de opções para mostrar as tarefas relacionadas ao banco de dados. Na faixa de opções, clique em **novo banco de dados**.

![Botão 'Novo banco de dados' na faixa de opções do WebMatrix](displaying-data/_static/image4.png)

O WebMatrix cria um banco de dados do SQL Server CE (um *. sdf* arquivo) que tem o mesmo nome como seu site &mdash; *WebPagesMovies.sdf*. (Não fazer isso aqui, mas você pode renomear o arquivo que desejar, desde que ela tenha um *. sdf* extensão.)

## <a name="creating-a-table"></a>Criando uma tabela

Na faixa de opções, clique em **nova tabela**. O WebMatrix abre o designer de tabela em uma nova guia. (Se a opção de nova tabela não estiver disponível, verifique se o novo banco de dados está selecionado na exibição de árvore à esquerda.)

!['Nova tabela' botão na faixa de opções do WebMatrix](displaying-data/_static/image5.png)

Na caixa de texto na parte superior (em que a marca d'água dizendo "Insira nome de tabela"), digite "Filmes".

![Inserir um nome de tabela no designer de banco de dados do WebMatrix](displaying-data/_static/image6.png)

O painel sob o nome da tabela é onde você define as colunas individuais. Para o *filmes* tabela neste tutorial, você criará apenas algumas colunas: *ID*, *título*, *gênero*, e *ano*.

No **nome** , digite "ID". Inserir um valor aqui ativa todos os controles para a nova coluna.

Alterne para o **tipo de dados** lista e escolha **int**. Esse valor Especifica que a coluna ID conterá dados integer (número).

> [!NOTE]
> Não chamamos out qualquer mais aqui (muito), mas você pode usar atalhos de teclado do Windows para navegar desta grade. Por exemplo, você pode usar tab entre os campos, você pode simplesmente comece a digitar para selecionar um item em uma lista e assim por diante.


Guia após o **valor padrão** caixa (ou seja, deixe em branco). Alterne para o **chave primária é** caixa de seleção e selecione-o. Essa opção informa o banco de dados que o *ID* coluna conterá os dados que identificam as linhas individuais. (Ou seja, cada linha terá um valor exclusivo na coluna de ID que você pode usar para localizar a linha.)

Escolha o **é identidade** opção. Essa opção informa o banco de dados que ele deve gerar automaticamente o próximo número sequencial para cada nova linha. (O **é identidade** opção funciona somente se você também tiver selecionado o **chave primária é** opção.)

Clique na próxima linha de grade ou pressione Tab duas vezes para concluir a linha atual. Um gesto salva a linha atual e inicia o outro. Observe que o **valor padrão** coluna agora diz **nulo**. (Null é o valor padrão para o valor padrão, por assim dizer.)

Quando terminar de definir o novo **ID** coluna, o designer será semelhante a esta ilustração:

![O WebMatrix designer de banco de dados depois de definir a coluna de ID para a tabela de filmes](displaying-data/_static/image7.png)

Para criar a próxima coluna, clique na caixa de **nome** coluna. Insira "Title" para a coluna e, em seguida, selecione **nvarchar** para o **tipo de dados** valor. A parte "var" de **nvarchar** informa que o banco de dados que os dados para essa coluna será uma cadeia de caracteres cujo tamanho pode variar de um registro para outro. (O prefixo "n" representa "national", que indica que o campo pode conter dados de caractere para qualquer alfabeto ou gravar sistema — ou seja, o campo contém dados Unicode.)

Quando você escolhe **nvarchar**, outra caixa é exibida, onde você pode inserir o comprimento máximo do campo. Insira a 50, supondo que nenhum título do filme que você trabalhará com este tutorial será mais de 50 caracteres.

Ignorar **valor padrão** e desmarque o **permitir nulos** opção. Você não deseja que o banco de dados para permitir que qualquer filmes sejam inseridos no banco de dados que não têm um título.

Quando você terminar e move para a próxima linha, o designer se parece com esta ilustração:

![O WebMatrix designer de banco de dados depois de definir a coluna de título para a tabela de filmes](displaying-data/_static/image8.png)

Repita estas etapas para criar uma coluna denominada "Gênero", exceto o comprimento, defina-o para apenas 30.

Criar outra coluna denominada "Ano". O tipo de dados, escolha **nchar** (não **nvarchar**) e defina o comprimento de 4. Para o ano, você irá usar um número de 4 dígitos como "1995" ou "2010", para que você não precise de uma coluna de tamanho variável.

O design concluído é semelhante ao seguinte:

![O WebMatrix designer de banco de dados depois que todos os campos são definidos para a tabela de filmes](displaying-data/_static/image9.png)

Pressione Ctrl + S ou clique no **salvar** botão na barra de ferramentas de acesso rápido. Fechar a guia para fechar o designer de banco de dados.

## <a name="adding-some-example-data"></a>Adicionar alguns dados de exemplo

Mais tarde nesta série de tutoriais, você criará uma página onde você pode inserir novos filmes em um formulário. Agora, no entanto, você pode adicionar alguns dados de exemplo que você pode exibir em uma página.

No **banco de dados** espaço de trabalho no WebMatrix, observe que há uma árvore que mostra a *. sdf* arquivo que você criou anteriormente. Abra o nó para o seu novo *. sdf* de arquivo e, em seguida, abra o **tabelas** nó.

![Espaço de trabalho de banco de dados do WebMatrix com árvore abrir a tabela de filmes](displaying-data/_static/image10.png)

Clique com botão direito do **filmes** nó e, em seguida, escolha **dados**. O WebMatrix abre uma grade onde você pode inserir dados para o *filmes* tabela:

![Grade de entrada do banco de dados no WebMatrix (vazio)](displaying-data/_static/image11.png)

Clique o **título** coluna e digite "Quando Harry atendidos Sally". Mover para o **gênero** coluna (você pode usar a tecla Tab) e digite "Românticas comédia". Mover para o **ano** coluna e digite "1989":

![Grade de entrada do banco de dados no WebMatrix com um registro](displaying-data/_static/image12.png)

Pressione Enter e o WebMatrix salva o novo filme. Observe que o **ID** coluna tiver sido preenchida.

![Grade de entrada do banco de dados no WebMatrix com um registro e a ID gerada automaticamente](displaying-data/_static/image13.png)

Insira outro filme (por exemplo, "e o vento", "Drama", "1939"). A coluna ID é preenchida novamente:

![Grade de entrada do banco de dados no WebMatrix com dois registros e IDs geradas automaticamente](displaying-data/_static/image14.png)

Insira um terceiro filme (por exemplo, "Ghostbusters", "Comédia"). Como uma experiência, deixe o **ano** coluna em branco e, em seguida, pressione Enter. Porque você desmarcou a **permitir nulos** opção, o banco de dados exibe o erro:

![Erro de 'Dados inválidos' exibido se um valor de coluna necessária é deixado em branco](displaying-data/_static/image15.png)

Clique em **Okey** para voltar e corrigir a entrada (o ano para "Ghostbusters" é 1984) e, em seguida, pressione Enter.

Preencha vários filmes até que você tenha 8 ou para. (Inserir 8 torna mais fácil trabalhar com paginação mais tarde. Mas se houver muitos, insira apenas alguns agora). Não importam os dados reais.

![Grade de entrada do banco de dados no WebMatrix com qualquer um dos registros](displaying-data/_static/image16.png)

Se você inseriu filmes sem erros, os valores de ID são sequenciais. Se você tentar salvar um registro de filme incompletos, os números de identificação podem não ser sequenciais. Nesse caso, que é okey. Os números não tem nenhum significado inerente e a única coisa que é importante é que eles são exclusivos no *filmes* tabela.

Feche a guia que contém o designer de banco de dados.

Agora você pode ativar para exibir esses dados em uma página da web.

## <a name="displaying-data-in-a-page-by-using-the-webgrid-helper"></a>Exibindo dados em uma página usando o auxiliar WebGrid

Para exibir dados em uma página, você vai usar o `WebGrid` auxiliar. Este assistente gera uma exibição em uma grade ou tabela (linhas e colunas). Como você pode ver, você será capaz de refinar a grade com formatação e outros recursos.

Para executar a grade, você precisará escrever algumas linhas de código. Essas linhas alguns servirá como um tipo de padrão para quase todo o acesso a dados que faz neste tutorial.

> [!NOTE]
> Você realmente tem muitas opções para exibir dados em uma página. o `WebGrid` auxiliar é apenas um. Escolhemos a ele para este tutorial porque é a maneira mais fácil de exibir dados e porque é razoavelmente flexível. O próximo conjunto de tutorial, você verá como usar uma maneira mais "manual" para trabalhar com dados da página, que lhe dá controle mais direto sobre como exibir os dados.


No painel esquerdo no WebMatrix, clique o **arquivos** espaço de trabalho.

É o novo banco de dados que você criou no *aplicativo\_dados* pasta. Se a pasta não existir, o WebMatrix criou para seu novo banco de dados. (A pasta pode ter existido se você tiver instalado anteriormente auxiliares.)

Na exibição de árvore, selecione a raiz do site. Você deve selecionar a raiz do site. Caso contrário, o novo arquivo pode ser adicionado ao aplicativo\_pasta de dados.

Na faixa de opções, clique em **novo**. No **escolher um tipo de arquivo** caixa, escolha **CSHTML**.

No **nome** caixa, nomeie a nova página "Movies.cshtml":

![Caixa de diálogo 'Escolher um tipo de arquivo' mostrando a página 'Filmes'](displaying-data/_static/image17.png)

Clique o **Okey** botão. O WebMatrix abre um novo arquivo com alguns elementos de esqueleto. Primeiro, você escreverá código para obter os dados do banco de dados. Em seguida, você adicionará marcação para a página para exibir os dados realmente.

### <a name="writing-the-data-query-code"></a>Escrevendo o código de consulta de dados

Na parte superior da página, entre o `@{` e `}` caracteres, insira o código a seguir. (Certifique-se de que este código entre as chaves de abertura e fechamento).

[!code-csharp[Main](displaying-data/samples/sample1.cs)]

A primeira linha abre o banco de dados que você criou anteriormente, que é sempre a primeira etapa antes de fazer algo com o banco de dados. Você informar o `Database.Open` nome do método do banco de dados para abrir. Observe que você não incluir *. sdf* no nome. O `Open` método pressupõe que está procurando uma *. sdf* arquivo (ou seja, *WebPagesMovies.sdf*) e que o *. sdf* arquivo está no *aplicativo\_ Dados* pasta. (Anteriormente observado que a *aplicativo\_dados* pasta está reservada; esse cenário é um dos locais em que o ASP.NET faz suposições sobre esse nome.)

Quando o banco de dados é aberto, uma referência a ele é colocada na variável nomeada `db`. (Que pode ser qualquer nome.) O `db` variável é como você acabará interagir com o banco de dados.

A segunda linha, na verdade, busca do banco de dados usando o `Query` método. Observe como funciona esse código: o `db` variável tem uma referência para o banco de dados aberto e você chamar o `Query` método usando o `db` variável (`db.Query`).

A consulta em si é um SQL `Select` instrução. (Plano de fundo um pouco sobre SQL, consulte a explicação posterior.) Na instrução, `Movies` identifica a tabela à consulta. O `*` caractere Especifica que a consulta deve retornar todas as colunas da tabela. (Você pode também lista colunas individualmente, separados por vírgulas.)

Os resultados da consulta, se houver, são retornados e disponibilizados no `selectedData` variável. Novamente, a variável pode ser qualquer nome.

Por fim, a terceira linha informa ASP.NET que você deseja usar uma instância de `WebGrid` auxiliar. Criar (*instanciar*) o objeto auxiliar usando o `new` palavra-chave e passá-lo os resultados da consulta por meio de `selectedData` variável. O novo `WebGrid` objeto junto com os resultados da consulta de banco de dados, são disponibilizados no `grid` variável. Você precisará que resultam em um momento para exibir os dados realmente na página.

Neste estágio, o banco de dados tiver sido aberto, você obteve os dados desejados e que você preparou a `WebGrid` auxiliar com esses dados. A segunda é criar a marcação na página.

> [!TIP] 
> 
> **Linguagem de consulta estruturada (SQL)**
> 
> SQL é uma linguagem que é usada na maioria dos bancos de dados relacionais para gerenciar dados em um banco de dados. Ele inclui comandos que permitem que você recupere dados e atualizá-lo, e que permitem criar, modificar e gerenciar dados em tabelas de banco de dados. SQL é diferente de uma linguagem de programação (como c#). Com o SQL, você informar o banco de dados que você deseja e trabalho do banco de dados para descobrir como obter os dados ou executar a tarefa. Aqui estão exemplos de alguns comandos SQL e o que fazer:
> 
> `Select * From Movies`
> 
> `SELECT ID, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> A primeira `Select` instrução obtém todas as colunas (especificado por `*`) da *filmes* tabela.
> 
> A segunda `Select` instrução busca as colunas de ID, o nome e o preço dos registros de *produto* tabela cujo valor de coluna de preço é maior que 10. O comando retorna os resultados em ordem alfabética com base nos valores da coluna de nome. Se nenhum registro corresponde aos critérios de preço, o comando retorna um conjunto vazio.
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ('Croissant', 'A flaky delight', 1.99)`
> 
> Este comando insere um novo registro para o *produto* tabela, definir a coluna de nome "Croissant", a coluna Descrição "Felicidade instável de um" e o preço para 1.99.
> 
> Observe que, quando você estiver especificando um valor não numérico, o valor está entre aspas simples (não aspas duplas, como em c#). Use essas aspas em torno de valores de data ou de texto, mas não em torno de números.
> 
> `DELETE FROM Product WHERE ExpirationDate < '01/01/2008'`
> 
> Esse comando exclui registros no *produto* tabela cuja coluna de data de vencimento anterior a 1º de janeiro de 2008. (O comando supõe que o *produto* tabela tem uma coluna desse tipo, é claro.) A data é inserida aqui no formato DD/MM/AAAA, mas ele deve ser inserido no formato que é usado para sua localidade.
> 
> O `Insert` e `Delete` comandos não retornam conjuntos de resultados. Em vez disso, elas retornam um número que indica quantos registros foram afetados pelo comando.
> 
> Para algumas dessas operações (como inserir e excluir registros), o processo que está solicitando a operação deve ter as permissões apropriadas no banco de dados. Que é por isso para bancos de dados de produção muitas vezes você precisa fornecer um nome de usuário e senha quando você se conectar ao banco de dados.
> 
> Há dezenas de comandos SQL, mas todas elas seguem um padrão como os comandos que você vê aqui. Você pode usar comandos SQL para criar tabelas de banco de dados, contar o número de registros em uma tabela, calcular preços e executar muitas operações mais.


### <a name="adding-markup-to-display-the-data"></a>Adicionando marcação para exibir os dados

Dentro de `<head>` elemento, o conteúdo do conjunto do `<title>` elemento "Filmes":

[!code-html[Main](displaying-data/samples/sample2.html?highlight=3)]

Dentro de `<body>` elemento da página, adicione o seguinte:

[!code-html[Main](displaying-data/samples/sample3.html)]

É isso. O `grid` variável é o valor que é criada quando você criou o `WebGrid` objeto no código anterior.

Na exibição de árvore do WebMatrix, a página e selecione **iniciar no navegador**. Você verá algo parecido com esta página:

![Saída de auxiliar WebGrid padrão da tabela de filmes](displaying-data/_static/image18.png)

Clique em um link de título de coluna para classificar por essa coluna. Ser capaz de classificar, clicando em um título é um recurso incorporado a **WebGrid** auxiliar.

O `GetHtml` método, como o nome sugere, gera uma marcação que exibe os dados. Por padrão, o `GetHtml` método gera um HTML `<table>` elemento. (Se desejar, você pode verificar a renderização examinando a fonte da página no navegador.)

## <a name="modifying-the-look-of-the-grid"></a>Modificando a aparência da grade

Usando o `WebGrid` auxiliar que você acabou de fazer é fácil, mas a exibição resultante é simples. O `WebGrid` auxiliar tem todos os tipos de opções que permitem controlar como os dados são exibidos. Há muito mais para explorar neste tutorial, mas esta seção lhe dará uma ideia de algumas dessas opções. Algumas opções adicionais serão abordadas em tutoriais subsequentes na série.

### <a name="specifying-individual-columns-to-display"></a>Especificar colunas individuais para exibição

Para iniciar, você pode especificar que você deseja exibir somente certas colunas. Por padrão, como visto, a grade mostra as quatro colunas do *filmes* tabela.

No *Movies.cshtml* de arquivo, substitua o `@grid.GetHtml()` marcação que você acabou de adicionar com o seguinte:

[!code-css[Main](displaying-data/samples/sample4.css)]

Para informar o auxiliar quais colunas serão exibidas, você deve incluir um `columns` parâmetro para o `GetHtml` método e passar em uma coleção de colunas. Na coleção, você deve especificar cada coluna a incluir. Especifique uma coluna individual para exibir, incluindo um `grid.Column` objeto e, em seguida, passa o nome da coluna de dados que você deseja. (Essas colunas devem ser incluídas nos resultados da consulta SQL — o `WebGrid` auxiliar não pode exibir as colunas que não foram retornadas pela consulta.)

Inicie o *Movies.cshtml* página no navegador novamente e desta vez você obter uma exibição como a seguir (Observe que nenhuma coluna de ID é exibida):

![Exibição de WebGrid mostrando apenas colunas selecionadas](displaying-data/_static/image19.png)

### <a name="changing-the-look-of-the-grid"></a>Alterar a aparência da grade

Há algumas mais opções para a exibição de colunas, alguns dos quais serão explorados em tutoriais subsequentes neste conjunto. No momento, esta seção apresenta a você maneiras em que você pode definir o estilo de grade como um todo.

Dentro de `<head>` seção da página, antes do fechamento `</head>` marca, adicione o seguinte `<style>` elemento:

[!code-css[Main](displaying-data/samples/sample5.css)]

Essa marcação CSS define classes chamadas `grid`, `head`e assim por diante. Você também pode colocar essas definições de estilo em uma separada *. CSS* de arquivo e que estão vinculadas à página. (Na verdade, você realizará que posteriormente nesse conjunto de tutorial.) Mas, para facilitar as coisas para este tutorial, está dentro da mesma página que exibe os dados.

Agora você pode obter o `WebGrid` auxiliar para usar essas classes de estilo. O auxiliar tem um número de propriedades (por exemplo, `tableStyle`) para essa finalidade, atribua um nome de classe de estilo CSS a eles, e esse nome de classe é processado como parte da marcação que é processada pelo auxiliar de.

Alteração de `grid.GetHtml` marcação para que ele agora parece com este código:

[!code-css[Main](displaying-data/samples/sample6.css)]

O que há de novo aqui é que você adicionou `tableStyle`, `headerStyle`, e `alternatingRowStyle` parâmetros para o `GetHtml` método. Esses parâmetros foram definidos para os nomes dos estilos de CSS que você adicionou um momento atrás.

Execute a página e, neste momento, você verá uma grade que se parece muito mais simples do que antes:

![Exibição WebGrid que inclui o conjunto de parâmetros para nomes de classe CSS](displaying-data/_static/image20.png)

Para ver o que o `GetHtml` método gerado, você pode examinar a fonte da página no navegador. Não entraremos em detalhes aqui, mas o ponto importante é que, com a especificação de parâmetros, como `tableStyle`, causou a grade gerar marcas HTML com o seguinte:

`<table class="grid">`

O `<table>` marca teve um `class` atributo adicionado a ele que faz referência a uma das regras de CSS que você adicionou anteriormente. Este código mostra o padrão básico &mdash; parâmetros diferentes para o `GetHtml` permitem que você referência de método CSS que o método gera juntamente com a marcação de classes. O que fazer com as classes CSS fica a seu critério.

## <a name="adding-paging"></a>Adicionando paginação

Como a última tarefa para este tutorial, você adicionará paginação à grade. No momento, não é um problema para exibir todos os seus filmes por vez. Mas, se você adicionou centenas de filmes, a exibição da página obteria longo.

Na página de código, altere a linha que cria o `WebGrid` objeto para o código a seguir:

[!code-csharp[Main](displaying-data/samples/sample7.cs)]

A única diferença de antes é que você adicionou um `rowsPerPage` parâmetro que é definido como 3.

Execute a página. A grade exibe 3 linhas em um tempo, além de links de navegação que permitem percorrer os filmes em seu banco de dados:

![Exibição WebGrid com paginação](displaying-data/_static/image21.png)

## <a name="coming-up-next"></a>Próximo

O seguinte tutorial, você aprenderá como usar código Razor e c# para obter a entrada do usuário em um formulário. Você adicionará uma caixa de pesquisa para a página de filmes, para que você possa encontrar filmes por título ou gênero.

## <a name="complete-listing-for-movies-page"></a>Listagem completa para a página de filmes

[!code-cshtml[Main](displaying-data/samples/sample8.cshtml)]

## <a name="additional-resources"></a>Recursos adicionais

- [Introdução à programação da Web do ASP.NET usando a sintaxe do Razor](https://go.microsoft.com/fwlink/?LinkID=202890)

>[!div class="step-by-step"]
[Anterior](intro-to-web-pages-programming.md)
[Próximo](form-basics.md)
