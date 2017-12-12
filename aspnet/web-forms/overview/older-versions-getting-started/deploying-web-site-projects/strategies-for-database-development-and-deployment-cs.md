---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/strategies-for-database-development-and-deployment-cs
title: "Estratégias de desenvolvimento de banco de dados e implantação (c#) | Microsoft Docs"
author: rick-anderson
description: "Ao implantar um aplicativo orientado a dados pela primeira vez você cegamente pode copiar o banco de dados no ambiente de desenvolvimento para o ambiente de produção. B..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: 3e8b0627-3eb7-488e-807e-067cba7cec05
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/strategies-for-database-development-and-deployment-cs
msc.type: authoredcontent
ms.openlocfilehash: 21d63b175eb52838ac9a12e33efc59fded4ed87d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="strategies-for-database-development-and-deployment-c"></a>Estratégias de desenvolvimento de banco de dados e implantação (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial10_DBDevel_cs.pdf)

> Ao implantar um aplicativo orientado a dados pela primeira vez você cegamente pode copiar o banco de dados no ambiente de desenvolvimento para o ambiente de produção. Mas, executando um blind cópia em implantações subsequentes substituirá todos os dados inseridos no banco de dados de produção. Em vez disso, implantar um banco de dados envolve a aplicação das alterações feitas no banco de dados de desenvolvimento desde a última implantação para o banco de dados de produção. Este tutorial examina esses desafios e oferece várias estratégias para ajudá-lo que narra e aplicar as alterações feitas no banco de dados desde a última implantação.


## <a name="introduction"></a>Introdução

Como discutido nos tutoriais anteriores, implantar um aplicativo ASP.NET envolve copiar o conteúdo pertinente do ambiente de desenvolvimento para o ambiente de produção. Implantação não é um evento único, mas em vez disso, algo que ocorre sempre que uma nova versão do software é liberada ou quando bugs ou questões de segurança foram identificados e resolvidos. Ao copiar páginas ASP.NET, imagens, arquivos JavaScript e outros esses arquivos para o ambiente de produção, que você não precisa se preocupar com como esses arquivos foram alterados desde a última implantação. Cegamente você pode copiar o arquivo para a produção, substituindo o conteúdo existente. Infelizmente, essa simplicidade não se estende ao implantar o banco de dados.

Ao implantar um aplicativo orientado a dados pela primeira vez você cegamente pode copiar o banco de dados no ambiente de desenvolvimento para o ambiente de produção. Mas, executando um blind cópia em implantações subsequentes substituirá todos os dados inseridos no banco de dados de produção. Em vez disso, a implantação de um banco de dados envolve a aplicação de *alterações* feitas no banco de dados de desenvolvimento desde a última implantação para o banco de dados de produção. Este tutorial examina esses desafios e oferece várias estratégias para ajudá-lo que narra e aplicar as alterações feitas no banco de dados desde a última implantação.

## <a name="the-challenges-of-deploying-a-database"></a>Os desafios de implantação de um banco de dados

Antes de um aplicativo orientado a dados foi implantado pela primeira vez, há apenas um banco de dados, ou seja, o banco de dados no ambiente de desenvolvimento, por isso, ao implantar um aplicativo orientado a dados pela primeira vez você cegamente pode copiar o banco de dados do ambiente de desenvolvimento para o ambiente de produção. Mas quando o aplicativo tiver sido implantado há duas cópias do banco de dados: um no desenvolvimento e outro em produção.

Entre as implantações de bancos de dados de desenvolvimento e produção podem se tornar fora de sincronia. Enquanto o esquema de s do banco de dados de produção permanece inalterado, o esquema de s do banco de dados de desenvolvimento pode mudar conforme novos recursos foram adicionados. Você pode adicionar ou remover colunas, tabelas, exibições ou procedimentos armazenados. Também pode haver dados importantes que são adicionados ao banco de dados de desenvolvimento. Muitos aplicativos controlados por dados incluem tabelas de pesquisa preenchidas com dados embutidos, específicos do aplicativo que não são editáveis pelo usuário. Por exemplo, um site de leilão pode ter uma lista suspensa com as opções que descrevem a condição do item que está sendo auctioned: novo, como novos, BOM e razoável. Em vez de embutir essas opções diretamente na lista suspensa é geralmente melhor colocá-los em uma tabela de banco de dados. Se, durante o desenvolvimento, uma nova condição denominada fraco é adicionada à tabela, em seguida, ao implantar o aplicativo esse mesmo registro precisa ser adicionada à tabela de pesquisa no banco de dados de produção.

O ideal é implantar o banco de dados envolve a copiar o banco de dados de desenvolvimento para produção. Mas tenha em mente que, após você ter implantado o aplicativo e retomada de desenvolvimento, o banco de dados de produção está sendo populado com dados reais de usuários reais. Portanto, se você simplesmente copiar o banco de dados de desenvolvimento para produção na próxima implantação substituir o banco de dados de produção e perder seus dados existentes. O resultado é que a implantação de banco de dados resume a aplicar as alterações feitas no banco de dados de desenvolvimento desde a última implantação.

Como implantar um banco de dados envolve a aplicação das alterações no esquema e, possivelmente, os dados desde a última implantação, um histórico de alterações deve ser mantido (ou determinado em tempo de implantação) para que essas alterações podem ser aplicadas em produção. Há uma variedade de técnicas para gerenciar e aplicar as alterações ao modelo de dados.

### <a name="defining-the-baseline"></a>Definir a linha de base

Para manter as alterações no banco de dados do aplicativo s, você precisa ter algum estado inicial, para que as alterações são aplicadas a uma linha de base. De um lado, o estado inicial pode ser um banco de dados vazio com nenhuma tabelas, exibições ou procedimentos armazenados. Uma linha de base resulta em um log grande alteração porque ele deve incluir a criação de todas as tabelas do banco de dados s, exibições e procedimentos armazenados, junto com todas as alterações feitas após a implantação inicial. Na outra extremidade do espectro, você pode definir a linha de base da versão do banco de dados que é implantado inicialmente no ambiente de produção. Essa opção resulta em um log de alteração muito menor, porque ela inclui apenas as alterações feitas no banco de dados após a implantação. Essa é a abordagem que preferir. E, obviamente você pode escolher um meio mais a abordagem de estrada, definindo a linha de base como algum ponto entre a criação inicial do banco de dados e quando o banco de dados é implantado pela primeira vez.

Depois que você tiver escolhido uma linha de base considere gerar um script SQL que pode ser executado para recriar a versão de linha de base. Esse script torna possível recriar rapidamente a versão de linha de base do banco de dados. Essa funcionalidade é especialmente útil em projetos grandes, onde pode haver vários desenvolvedores trabalhando no projeto ou ambientes adicionais, como teste ou preparo, cada precisa de sua própria cópia do banco de dados.

Há uma variedade de ferramentas à sua disposição para gerar um script SQL da versão de linha de base. Do SQL Server Management Studio (SSMS) clique no banco de dados, vá para o submenu de tarefas e escolha a opção de gerar Scripts. Isso inicia o Assistente de Script, você pode instruir para gerar um arquivo que contém os comandos SQL para criar objetos de s de seu banco de dados. Outra opção é o Assistente de publicação de banco de dados, que pode gerar os comandos SQL não apenas para criar o esquema de banco de dados, mas também os dados nas tabelas de banco de dados. O Assistente de publicação de banco de dados foi examinado detalhadamente no *Implantando um banco de dados* tutorial. Independentemente de qual ferramenta usar, no final, você deve ter um arquivo de script que você pode usar para recriar a versão de linha de base do seu banco de dados, caso seja preciso surgir.

## <a name="documenting-the-database-changes-in-prose"></a>As alterações do banco de dados em um texto de documentação

É a maneira mais simples para manter um log de alterações para o modelo de dados durante a fase de desenvolvimento registrar as alterações no texto. Por exemplo, se durante o desenvolvimento de um aplicativo já implantado, você adicionar uma nova coluna para o `Employees` da tabela, remover uma coluna do `Orders` de tabela e adicionar uma nova tabela (`ProductCategories`), você deve manter um arquivo de texto ou documento do Microsoft Word com o seguinte histórico:

<a id="0.4_table01"></a>


| **Data da alteração** | **Alterar detalhes** |
| --- | --- |
| 2009-02-03: | Coluna adicionada `DepartmentID` (`int`, não NULL) para o `Employees` tabela. Adicionar uma restrição de chave estrangeira de `Departments.DepartmentID` para `Employees.DepartmentID`. |
| 2009-02-05: | Coluna removida `TotalWeight` do `Orders` tabela. O associado de dados já capturados no `OrderDetails` registros. |
| 2009-02-12: | Criado o `ProductCategories` tabela. Há três colunas: `ProductCategoryID` (`int`, `IDENTITY`, `NOT NULL`), `CategoryName` (`nvarchar(50)`, `NOT NULL`), e `Active` (`bit`, `NOT NULL`). Adicionar uma restrição de chave primária para `ProductCategoryID`e um valor padrão de 1 para `Active`. |


Há várias desvantagens dessa abordagem. Para começar, não há nenhuma esperança para automação. A qualquer momento essas alterações precisam ser aplicadas a um banco de dados -, como quando o aplicativo é implantado - um desenvolvedor deve implementar manualmente cada alteração, um de cada vez. Além disso, se você precisar reconstruir uma versão específica do banco de dados da linha de base usando o log de alterações, fazer assim levará mais tempo à medida que aumenta o tamanho do log. Outra desvantagem desse método é que a clareza e o nível de detalhes de cada entrada de log de alteração é da esquerda para a pessoa que registra a alteração. Em uma equipe com vários desenvolvedores alguns podem criar entradas mais detalhadas, mais legíveis ou mais precisas que outros. Além disso, erros de digitação e outros erros de entrada de dados relacionados a humanos são possíveis.

O principal benefício de documentar as alterações de banco de dados em um texto é a simplicidade. Você não familiaridade da necessidade de t com a sintaxe SQL para criar e alterar objetos de banco de dados. Em vez disso, você pode registrar as alterações no texto e implementá-las por meio da interface gráfica do usuário do SQL Server Management Studio s.

Manter o log de alterações em um texto é, de fato, não funcionam muito sofisticados e ganha bem com determinados projetos, como aqueles que são grandes no escopo, têm alterações frequentes para o modelo de dados ou envolver vários desenvolvedores. Mas vi essa abordagem funciona bem em projetos de pequenos, com que têm apenas alterações ocasionais ao modelo de dados e em que o desenvolvedor solo não tem um plano de fundo forte na sintaxe SQL criando e alterando objetos de banco de dados.

> [!NOTE]
> Enquanto as informações no log de alteração são, tecnicamente, só é necessária até implantar-hora, é recomendável manter um histórico das alterações. Mas, em vez de manter uma única, crescendo cada vez mais o arquivo de log de alteração, considere ter um arquivo de log de alteração diferentes para cada versão do banco de dados. Geralmente você desejará versão o banco de dados cada vez que ele é implantado. Ao manter um registro de log de alteração você pode, a partir da linha de base, recrie o qualquer versão do banco de dados, executando os scripts de log de alteração a partir da versão 1 e continuar até que a versão que você precisa recriar.


## <a name="recording-the-sql-change-statements"></a>Gravando as instruções de alteração do SQL

A principal desvantagem de manter o log de alterações em um texto é a falta de automação. Idealmente, implementar as alterações do banco de dados para o banco de dados de produção em tempo de implantação seria tão fácil quanto clicar em um botão para executar um script em vez de ter que executar manualmente uma lista de instruções. Essa automação é possível, mantendo um log de alteração que contém os comandos SQL usados para alterar o modelo de dados.

A sintaxe SQL inclui um número de instruções para criar e modificar vários objetos de banco de dados. Por exemplo, o [ *instrução CREATE TABLE*](https://msdn.microsoft.com/en-us/library/ms174979.aspx), quando executado, cria uma nova tabela com as restrições e colunas especificadas. O [ *instrução ALTER TABLE* ](https://msdn.microsoft.com/en-us/library/ms190273.aspx) modifica uma tabela existente, adicionando, removendo ou modificando suas colunas ou restrições. Também há instruções para criar, modificar e remover índices, exibições, funções definidas pelo usuário, procedimentos armazenados, disparadores e outros objetos de banco de dados.

Retornar ao nosso exemplo anterior, a imagem que durante o desenvolvimento de um aplicativo já implantado, você adicionar uma nova coluna para o `Employees` da tabela, remover uma coluna do `Orders` de tabela e adicionar uma nova tabela (`ProductCategories`). Essas ações pode resultar em um arquivo de log de alteração com os comandos SQL a seguir:

[!code-sql[Main](strategies-for-database-development-and-deployment-cs/samples/sample1.sql)]

Enviar por push as alterações no banco de dados de produção no momento da implantação é uma operação de um clique: Abra o SQL Server Management Studio, conecte-se ao banco de dados de produção, abra uma janela nova consulta, colar o conteúdo do log de alteração e clique em executar para executar o script.

## <a name="using-a-comparison-tool-to-synchronize-the-data-models"></a>Usando uma ferramenta de comparação para sincronizar os modelos de dados

Alterações de banco de dados em um texto de documentação é fácil, mas implementar as alterações requer um desenvolvedor fazer cada alteração do banco de dados de produção um por vez; Documentar os comandos SQL a alteração torna implementar essas alterações no banco de dados de produção mais fácil e rápida, como clicar em um botão, mas exige aprendizado e dominar as instruções SQL e sintaxe para criação e alteração de objetos de banco de dados. Ferramentas de comparação do banco de dados tirar o melhor de ambas as abordagens e descartar o pior.

Uma ferramenta de comparação do banco de dados compara o esquema ou os dados de dois bancos de dados e exibe um relatório de resumo mostrando a diferença entre os bancos de dados. Em seguida, com o clique de um botão, você pode gerar os comandos SQL para a sincronização de um ou mais objetos de banco de dados. Em resumo, você pode usar uma ferramenta de comparação do banco de dados para comparar o desenvolvimento e comandos de bancos de dados de produção durante a implantação, gerando um arquivo que contém o SQL que, quando executada, aplicará as alterações no esquema de banco de dados s produção assim que ele reflete o esquema de banco de dados s de desenvolvimento.

Há uma variedade de ferramentas de comparação do banco de dados de terceiros oferecidos por fornecedores diferentes. Um exemplo é [ *SQL comparar*](http://www.red-gate.com/products/SQL_Compare/), por [ *Red Gate Software*](http://www.red-gate.com/). Permitir que o s percorrer o processo de usar a comparação do SQL para comparar e sincronizar os esquemas de bancos de dados de desenvolvimento e produção.

> [!NOTE]
> No momento da redação deste artigo, a versão atual de comparação do SQL foi versão 7.1, com a edição Standard custos de US $395. Você pode acompanhar baixando uma avaliação gratuita de 14 dias.


Quando a comparação do SQL inicia a caixa de diálogo de projetos de comparação é aberta, mostrando os projetos de comparação do SQL salvos. Crie um novo projeto. Isso inicia o Assistente de configuração de projeto, que solicita informações sobre os bancos de dados a ser comparado (consulte a Figura 1). Insira as informações para os bancos de dados de ambiente de desenvolvimento e produção.


[![Compare o desenvolvimento e os bancos de dados de produção](strategies-for-database-development-and-deployment-cs/_static/image2.jpg)](strategies-for-database-development-and-deployment-cs/_static/image1.jpg)

**Figura 1**: comparar o desenvolvimento e os bancos de dados de produção ([clique para exibir a imagem em tamanho normal](strategies-for-database-development-and-deployment-cs/_static/image3.jpg))


> [!NOTE]
> Se seu banco de dados do ambiente de desenvolvimento é um arquivo de banco de dados do SQL Express Edition no `App_Data` pasta do seu site, você precisará registrar o banco de dados no servidor de banco de dados do SQL Server Express para selecioná-lo na caixa de diálogo mostrada na Figura 1. A maneira mais fácil de fazer isso é abrir o SQL Server Management Studio (SSMS), conecte-se ao servidor de banco de dados SQL Server Express e anexar o banco de dados. Se você não tiver o SSMS instalado em seu computador, você pode baixar e instalar a versão gratuita [ *versão do SQL Server 2008 Management Studio Basic*](https://www.microsoft.com/downloads/details.aspx?FamilyId=7522A683-4CB2-454E-B908-E805E9BD4E28&amp;displaylang=en).


Além de selecionar os bancos de dados a ser comparado, você também pode especificar uma variedade de configurações na guia Opções de comparação. Uma opção, que talvez você queira ativar é os "Ignorar restrição e índice nomes." Lembre-se de que o tutorial anterior adicionamos que o aplicativo de serviços de objetos de banco de dados para os bancos de dados de desenvolvimento e produção. Se você usou o `aspnet_regsql.exe` ferramenta para criar esses objetos no banco de dados de produção, em seguida, você descobrirá que a chave primária e nomes de restrição exclusiva diferem entre os bancos de dados de desenvolvimento e produção. Consequentemente, a comparação do SQL marcará todas as tabelas de serviços de aplicativo como diferentes. Você pode deixar os "Ignorar restrição e índice nomes" está desmarcada e sincronizar os nomes de restrição ou instruir a comparação do SQL para ignorar essas diferenças.

Depois de selecionar os bancos de dados comparar (e revisar as opções de comparação), clique no botão comparar agora para começar a comparação. Em alguns segundos, a comparação do SQL examina os esquemas de dois bancos de dados e gera um relatório de como eles diferem. Posso salvar intencionalmente feitas algumas modificações no banco de dados de desenvolvimento para mostrar como tais discrepâncias são observadas na interface de comparação do SQL. Como mostra a Figura 2, ve adicionado um `BirthDate` coluna para o `Authors` tabela, removida o `ISBN` coluna a partir o `Books` de tabela e adicionar uma nova tabela, `Ratings`, que é destinado para permitir que os usuários que visitam a taxa de site os livros revisados.

> [!NOTE]
> As alterações do modelo de dados feitas neste tutorial foram feitas para ilustrar o uso de uma ferramenta de comparação do banco de dados. Você não encontrará essas alterações no banco de dados em tutoriais futuros.


[![Comparação SQL lista as diferenças entre o desenvolvimento e os bancos de dados de produção](strategies-for-database-development-and-deployment-cs/_static/image5.jpg)](strategies-for-database-development-and-deployment-cs/_static/image4.jpg)

**Figura 2**: lista de SQL comparar as diferenças entre o desenvolvimento e os bancos de dados de produção ([clique para exibir a imagem em tamanho normal](strategies-for-database-development-and-deployment-cs/_static/image6.jpg))


Comparar SQL divide os objetos de banco de dados em grupos, rapidamente, mostrando quais objetos existe em ambos os bancos de dados, mas são diferente, que existem objetos em um banco de dados, mas não o outro e quais objetos são idênticos. Como você pode ver, há dois objetos que existem em ambos os bancos de dados, mas são diferentes: a `Authors` tabela, que tem uma coluna adicionada, e o `Books` tabela, que tinha um removido. Há um objeto que existe somente no desenvolvimento banco de dados, ou seja, recém-criado `Ratings` tabela. E há 117 objetos que são idênticos em ambos os bancos de dados.

Selecionando um objeto de banco de dados exibe a janela de diferenças de SQL, que mostra a diferença entre esses objetos. A janela de diferenças de SQL, exibida na parte inferior na Figura 2, realça que o `Authors` tabela no banco de dados de desenvolvimento tem o `BirthDate` coluna, que não foi encontrada no `Authors` tabela no banco de dados de produção.

Depois de revisar as diferenças e selecionar quais objetos você deseja sincronizar, a próxima etapa é gerar os comandos SQL necessários para atualizar o esquema de s do banco de dados de produção para coincidir com o banco de dados de desenvolvimento. Isso é feito por meio do Assistente de sincronização. O Assistente para sincronização confirma que os objetos para sincronizar e resume a ação planejar (consulte a Figura 3). Você pode sincronizar os bancos de dados imediatamente ou gerar um script com os comandos SQL que pode ser executado quando quiser.


[![Use o Assistente de sincronização para sincronizar os esquemas de bancos de dados](strategies-for-database-development-and-deployment-cs/_static/image8.jpg)](strategies-for-database-development-and-deployment-cs/_static/image7.jpg)

**Figura 3**: usar o Assistente de sincronização para sincronizar seus esquemas de bancos de dados ([clique para exibir a imagem em tamanho normal](strategies-for-database-development-and-deployment-cs/_static/image9.jpg))


Ferramentas de comparação do banco de dados como Red Gate Software s SQL comparar tornar a aplicar as alterações de esquema de banco de dados de desenvolvimento para o banco de dados de produção tão fácil quanto apontar e clicar.

> [!NOTE]
> Comparação do SQL compara e sincroniza dois bancos de dados *esquemas*. Infelizmente, ele não comparar e sincronizar os dados nas tabelas de dois bancos de dados. Red Gate Software oferece um produto chamado [ *comparação de dados do SQL* ](http://www.red-gate.com/products/SQL_Data_Compare/) que compara e sincroniza os dados entre dois bancos de dados, mas é um produto separado de comparação do SQL e custos de outro $395.


## <a name="taking-the-application-offline-during-deployment"></a>Colocar o aplicativo Offline durante a implantação

Como podemos ve visto durante esses tutoriais, implantação é um processo que envolve várias etapas: copiar as páginas ASP.NET, páginas mestras, CSS arquivos, JavaScript arquivos, imagens e outro conteúdo necessário no ambiente de desenvolvimento para produção ambiente; copiando as informações de configuração específicos ao ambiente de produção, se necessário. e aplicar as alterações ao modelo de dados desde a última implantação. Dependendo do número de arquivos e a complexidade das alterações de banco de dados, essas etapas podem levar de alguns segundos a vários minutos para concluir. Durante a janela de aplicativo da web está em fluxo e os usuários que visitam o site podem experimentar erros ou um comportamento inesperado.

Ao implantar um site é aconselhável colocar o aplicativo web "offline" até que a implantação for concluída. Colocar o aplicativo offline (e colocá-lo a fazer backup quando terminar o processo de implantação) é tão fácil quanto carregando um arquivo e, em seguida, excluí-lo. Começando com o ASP.NET 2.0, simples presença de um arquivo chamado `app_offline.htm` em aplicativo s diretório raiz leva a todo o site "offline". Qualquer solicitação para uma página ASP.NET no site é automaticamente respondeu com o conteúdo do `app_offline.htm` arquivo. Depois que esse arquivo é removido, o aplicativo de volta a ficar online.

Colocar um aplicativo offline durante a implantação, em seguida, é tão simple quanto carregando um `app_offline.htm` arquivo para o ambiente de produção s raiz do diretório antes de começar o processo de implantação e, em seguida, excluí-lo (ou renomeá-lo para outra coisa) uma vez implantação foi concluída. Para obter mais informações sobre essa técnica, consulte o artigo de s de John Peterson, fazer uma [ *ASP.NET aplicativo Offline*](http://www.15seconds.com/issue/061207.htm).

## <a name="summary"></a>Resumo

O principal desafio na implantação de um aplicativo orientado a dados gira em torno de implantar o banco de dados. Porque há duas versões do banco de dados - um no ambiente de desenvolvimento e outro no ambiente de produção esses esquemas de dois bancos de dados podem se tornar fora de sincronia conforme novos recursos foram adicionados em desenvolvimento. O que é mais, porque o banco de dados de produção como sendo populado com dados reais de usuários reais, não é possível substituir o banco de dados de produção com o banco de dados de desenvolvimento modificado como você pode fazer ao implantar os arquivos que compõem o aplicativo (páginas ASP.NET, arquivos de imagem e assim por diante). Em vez disso, a implantação de um banco de dados envolve a implementar o conjunto exato das alterações feitas no banco de dados de desenvolvimento do banco de dados de produção desde a última implantação.

Este tutorial analisamos as três técnicas de manutenção e a aplicação de um log de alterações do banco de dados. É a abordagem mais simples registrar as alterações no texto. Enquanto essa tática facilita a implementação dessas alterações em um processo manual de dados de produção, ele não requer conhecimento dos comandos SQL para criar e alterar objetos de banco de dados. Uma abordagem mais sofisticada e um que é muito mais agradável no maiores ou projetos com vários desenvolvedores, é registrar as alterações como uma série de comandos SQL. Isso acelera consideravelmente muito distribuindo essas alterações no banco de dados de destino. O melhor de ambas as abordagens pode ser obtido usando uma ferramenta de comparação do banco de dados, como Red Gate Software s comparação do SQL.

Este tutorial conclui nosso foco sobre como implantar um aplicativo orientado a dados. O próximo conjunto de tutoriais examina como responder a erros no ambiente de produção. Vamos examinar como exibir uma página de erro amigável em vez disso, em vez da tela de morte amarelo. E veremos como registrar os detalhes do erro s e alertar você quando tais erros ocorrerem.

Boa programação!

>[!div class="step-by-step"]
[Anterior](configuring-a-website-that-uses-application-services-cs.md)
[Próximo](displaying-a-custom-error-page-cs.md)
