---
uid: mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
title: Criar um modelo de validação de regra de negócios | Microsoft Docs
author: microsoft
description: Etapa 3 mostra como criar um modelo que podemos usar para ambas as consultas e atualizar o banco de dados de nosso aplicativo NerdDinner.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 0bc191b2-4311-479a-a83a-7f1b1c32e6fe
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
msc.type: authoredcontent
ms.openlocfilehash: c5a482474fd2f41836f70952306ada5cd9136455
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874935"
---
<a name="build-a-model-with-business-rule-validations"></a>Criar um modelo de validação de regra de negócios
====================
por [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Esta é a etapa 3 da livremente [aplicativo "NerdDinner"](introducing-the-nerddinner-tutorial.md) que aborda-por meio de como criar um pequeno, mas concluir, o aplicativo web usando o ASP.NET MVC 1.
> 
> Etapa 3 mostra como criar um modelo que podemos usar para ambas as consultas e atualizar o banco de dados de nosso aplicativo NerdDinner.
> 
> Se você estiver usando o ASP.NET MVC 3, recomendamos que você siga o [obtendo iniciado com MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [repositório de música MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriais.


## <a name="nerddinner-step-3-building-the-model"></a>NerdDinner etapa 3: Criar o modelo

Uma estrutura model-view-controller o termo "modelo" refere-se aos objetos que representam os dados de aplicativo, bem como a lógica do domínio correspondente que se integra a validação e regras de negócio com ele. O modelo é de várias maneiras "Centro" de um aplicativo baseado em MVC e como você verá posteriormente fundamentalmente unidades o comportamento do mesmo.

A estrutura ASP.NET MVC oferece suporte ao uso de qualquer tecnologia de acesso a dados e os desenvolvedores podem escolher entre uma variedade de opções de dados avançadas do .NET para implementar seus modelos incluindo: LINQ para entidades, LINQ to SQL, NHibernate, LLBLGen Pro, SubSonic, WilsonORM ou ADO apenas bruto. NET DataReaders ou conjuntos de dados.

Em nosso aplicativo NerdDinner, vamos usar o LINQ to SQL para criar um modelo simple que bem de perto corresponde ao nosso design de banco de dados e adiciona algumas regras de negócios e lógica de validação personalizada. Vamos, em seguida, implementar uma classe de repositório que ajuda a distância abstrato a implementação da persistência de dados do restante do aplicativo e habilita a unidade facilmente testá-lo.

### <a name="linq-to-sql"></a>LINQ to SQL

O LINQ to SQL é um ORM (mapeador relacional de objeto) que é fornecido como parte do .NET 3.5.

O LINQ to SQL fornece uma maneira fácil para mapear tabelas de banco de dados para classes do .NET, codifique contra. Em nosso aplicativo NerdDinner usaremos ele para mapear as tabelas jantares e RSVP em nosso banco de dados para classes refeição e RSVP. As colunas das tabelas jantares e RSVP corresponderá às propriedades nas classes de uma refeição e RSVP. Cada objeto refeição e RSVP representará uma linha separada dentro das tabelas jantares ou RSVP no banco de dados.

O LINQ to SQL permite evitar ter que construir manualmente instruções SQL para recuperar e atualizar uma refeição e RSVP objetos com do banco de dados. Em vez disso, vamos definir as classes refeição e RSVP, como eles serão mapeados para/do banco de dados e as relações entre eles. O LINQ to SQL será assume o cuidado de gerar a lógica de execução SQL apropriada para usar em tempo de execução quando interagir e usá-los.

Podemos usar o suporte de linguagem LINQ em VB e c# para escrever expressivas consultas que recuperam uma refeição e RSVP objetos do banco de dados. Isso minimiza a quantidade de código de dados, precisamos de gravação, e permite a criação de aplicativos realmente limpas.

### <a name="adding-linq-to-sql-classes-to-our-project"></a>Adicionando Classes LINQ to SQL ao nosso projeto

Vamos começar clicando na pasta "Modelos" em nosso projeto e selecione o **Add -&gt;Novo Item** comando de menu:

![](build-a-model-with-business-rule-validations/_static/image1.png)

Isso abrirá a caixa de diálogo "Adicionar Novo Item". Vamos filtrar por categoria de "Dados" e selecione o modelo de "Classes de LINQ to SQL" nele:

![](build-a-model-with-business-rule-validations/_static/image2.png)

Vamos o nome do item "NerdDinner" e clique no botão "Adicionar". O Visual Studio adiciona um arquivo NerdDinner.dbml em nosso diretório \Models e, em seguida, abra o LINQ to SQL designer relacional de objeto:

![](build-a-model-with-business-rule-validations/_static/image3.png)

### <a name="creating-data-model-classes-with-linq-to-sql"></a>Criando Classes de modelo de dados com LINQ to SQL

O LINQ to SQL permite criar rapidamente as classes de modelo de dados de esquema de banco de dados existente. Fazer isso, abra o banco de dados NerdDinner no Gerenciador de servidores e selecione as tabelas deseja modelar nele:

![](build-a-model-with-business-rule-validations/_static/image4.png)

Em seguida, é possível arrastar as tabelas em LINQ à superfície de designer do SQL. Quando fazemos esse LINQ to SQL criará automaticamente uma refeição e RSVP classes usando o esquema de tabelas (com propriedades de classe que mapeiam para as colunas da tabela de banco de dados):

![](build-a-model-with-business-rule-validations/_static/image5.png)

Por padrão, o LINQ to SQL designer pluraliza automaticamente "os" nomes de tabela e coluna quando ele cria classes com base em um esquema de banco de dados. Por exemplo: a tabela "Jantares" em nosso exemplo acima resultou em uma classe "Refeição". Essa classe de nomenclatura ajuda a tornar os nossos modelos consistente com as convenções de nomenclatura do .NET e geralmente localizar que com a correção designer isso backup conveniente (especialmente ao adicionar muitas tabelas). Se você não gosta do nome de uma classe ou propriedade que gera o designer, no entanto, você sempre pode substituí-la e alterá-lo para qualquer nome que você deseja. Você pode fazer isso editando a propriedade de entidade/nome em linha dentro do designer ou para modificá-lo por meio da grade de propriedades.

Por padrão, o LINQ to SQL designer também verifica se a primárias/chave relações de chave estrangeira das tabelas e com base neles automaticamente cria padrão "associações de relação" entre as classes de modelo diferente, que ele cria. Por exemplo, quando é arrastado as jantares e RSVP tabelas em LINQ to SQL designer uma associação de relação um-para-muitos entre os dois foi inferida baseada no fato de que a tabela RSVP tinha uma chave estrangeira à tabela jantares (Isso é indicado pela seta do Designer):

![](build-a-model-with-business-rule-validations/_static/image6.png)

A associação acima fará com que o LINQ to SQL para adicionar uma propriedade de "Refeição" fortemente tipada para a classe RSVP que os desenvolvedores podem usar para acessar a refeição associada a um determinado RSVP. Ela também fará com que a classe refeição ter uma propriedade de coleção "RSVPs" que permite aos desenvolvedores recuperar e atualizar objetos RSVP associados com uma refeição específica.

Abaixo você pode ver um exemplo de como intellisense no Visual Studio quando podemos criar um novo objeto RSVP e adicioná-la à coleção de RSVPs da refeição. Observe como o LINQ to SQL adicionado automaticamente uma coleção de "RSVPs" no objeto refeição:

![](build-a-model-with-business-rule-validations/_static/image7.png)

Adicionando o objeto RSVP à coleção de RSVPs do refeição dizemos LINQ to SQL para associar uma relação de chave estrangeira entre a refeição e a linha RSVP em nosso banco de dados:

![](build-a-model-with-business-rule-validations/_static/image8.png)

Se você não gostar como o designer tem modelada ou denominado uma associação de tabela, você pode substituí-la. Basta clicar na seta para associação dentro do designer e acessar suas propriedades por meio da grade de propriedades para renomear, excluir ou modificá-lo. Em nosso aplicativo NerdDinner, no entanto, as regras de associação padrão funcionam bem para as classes de modelo de dados que estamos criando e podemos usar apenas o comportamento padrão.

### <a name="nerddinnerdatacontext-class"></a>Classe NerdDinnerDataContext

O Visual Studio criará automaticamente as classes do .NET que representam os modelos e as relações de banco de dados definidas usando o LINQ to SQL designer. Uma classe LINQ to SQL DataContext também é gerado para cada arquivo LINQ to SQL designer adicionado à solução. Como podemos nomeado nosso LINQ para item de classe SQL "NerdDinner", a classe DataContext criada será chamada "NerdDinnerDataContext". Essa classe NerdDinnerDataContext é a principal maneira que irão interagir com o banco de dados.

Nossa classe NerdDinnerDataContext expõe duas propriedades - "Jantares" e "RSVPs -" que representam as duas tabelas que são modeladas no banco de dados. Podemos usar c# para escrever consultas LINQ em relação a essas propriedades para consulta e recuperar uma refeição e RSVP objetos do banco de dados.

O código a seguir demonstra como instanciar um objeto NerdDinnerDataContext e executar uma consulta LINQ para obter uma sequência de jantares ocorrer no futuro. O Visual Studio fornece completo intellisense ao escrever a consulta LINQ, e os objetos retornados dele são fortemente tipadas e também oferecem suporte a intellisense:

![](build-a-model-with-business-rule-validations/_static/image9.png)

Além de permitir que a consulta para objetos de uma refeição e RSVP, um NerdDinnerDataContext automaticamente rastreia as alterações que fazemos subsequentemente para os objetos de uma refeição e RSVP que recuperamos através dele. Podemos usar essa funcionalidade para salvar facilmente as alterações no banco de dados - sem precisar gravar qualquer código de atualização SQL explícito.

Por exemplo, o código a seguir demonstra como usar uma consulta LINQ para recuperar um único objeto de uma refeição do banco de dados, atualizar duas propriedades refeição e, em seguida, salvar as alterações no banco de dados:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample1.cs)]

O objeto NerdDinnerDataContext no código acima controladas automaticamente as alterações de propriedade feitas para o objeto de uma refeição que são obtidas. Quando é chamado o método "SubmitChanges()", ele executará uma instrução SQL apropriada de "Atualização", no banco de dados para manter os valores atualizados de volta.

### <a name="creating-a-dinnerrepository-class"></a>Criando uma classe DinnerRepository

Para aplicativos pequenos, às vezes, é bom ter controladores de trabalhar diretamente em uma classe LINQ to SQL DataContext e inserir consultas LINQ em controladores de. Como aplicativos ficam maiores, no entanto, essa abordagem se torna difícil de manter e testar. Isso também poderá resultar em nós duplicando as mesmas consultas LINQ em vários lugares.

Uma abordagem que pode tornar mais fácil de manter e testar aplicativos é usar um padrão de "repository". Uma classe de repositório ajuda encapsular a consulta de dados e lógica de persistência e resumos imediatamente os detalhes de implementação da persistência de dados do aplicativo. Além de tornar o código do aplicativo limpeza, usando um padrão de repositório pode tornar mais fácil alterar as implementações de armazenamento de dados no futuro, e pode ajudar a facilitar o teste de um aplicativo sem a necessidade de um banco de dados real da unidade.

Em nosso aplicativo NerdDinner vamos definir uma classe DinnerRepository com o abaixo de assinatura:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample2.cs)]

*Observação: Neste capítulo vamos extrair uma interface IDinnerRepository desta classe e habilitar a injeção de dependência com ele em nosso controladores. Em primeiro lugar, porém, vamos começar simples e apenas trabalhar diretamente com a classe DinnerRepository.*

Para implementar essa classe vamos com o botão direito em nossa pasta de "Modelos" e escolha o **Add -&gt;Novo Item** comando de menu. A caixa de diálogo "Adicionar Novo Item" Vamos selecionar o modelo de "Classe" e nomeie o arquivo "DinnerRepository.cs":

![](build-a-model-with-business-rule-validations/_static/image10.png)

Em seguida, podemos implementar nossa classe DinnerRespository usando o código a seguir:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample3.cs)]

### <a name="retrieving-updating-inserting-and-deleting-using-the-dinnerrepository-class"></a>Recuperar, atualizar, inserir e excluir usando a classe DinnerRepository

Agora que criamos nossa classe DinnerRepository, vamos examinar alguns exemplos de código que demonstram as tarefas comuns que podem fazer com ele:

#### <a name="querying-examples"></a>Exemplos de consultando

O código a seguir recupera uma refeição único usando o valor de DinnerID:


[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample4.cs)]

O código a seguir recupera todos os futuros jantares e loops sobre eles:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample5.cs)]

#### <a name="insert-and-update-examples"></a>Exemplos de atualização e inserção

O código a seguir demonstra como adicionar dois jantares novo. Adições/modificações para o repositório não são confirmadas no banco de dados até que o método "Save ()" é chamado nela. O LINQ to SQL ajusta automaticamente todas as alterações em uma transação de banco de dados – para todas as alterações ocorrem ou nenhum deles ao nosso repositório salva:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample6.cs)]

O código a seguir recupera um objeto de uma refeição existente e modifica as duas propriedades nele. As alterações são confirmadas no banco de dados quando o método "Save ()" é chamado em nosso repositório:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample7.cs)]

O código a seguir recupera uma refeição e, em seguida, adiciona um RSVP a ele. Ele faz isso usando a coleção de RSVPs no objeto de uma refeição criado LINQ to SQL (porque não há uma relação primary key/foreign key entre os dois no banco de dados). Essa alteração é mantida no banco de dados como uma nova linha na tabela RSVP quando o método "Save ()" é chamado no repositório:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample8.cs)]

#### <a name="delete-example"></a>Exemplo-excluir

O código a seguir recupera um objeto de uma refeição existente e, em seguida, a marca a ser excluído. Quando o método "Save ()" é chamado no repositório, ela confirmará a exclusão no banco de dados:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample9.cs)]

### <a name="integrating-validation-and-business-rule-logic-with-model-classes"></a>Integrar a validação e lógica de regra de negócios com Classes de modelo

Integração de validação e lógica é uma parte importante de qualquer aplicativo que trabalha com dados de regra de negócio.

#### <a name="schema-validation"></a>Validação de esquema

Quando as classes de modelo são definidas usando o LINQ to SQL designer, os tipos de dados das propriedades em classes de modelo de dados correspondem aos tipos de dados da tabela de banco de dados. Por exemplo: se a coluna "Data doevento" na tabela jantares "datetime", a classe de modelo de dados criada pelo LINQ to SQL será do tipo "DateTime" (que é um tipo de dados interno do .NET). Isso significa que você receberá erros de compilação se você tentar atribuir um inteiro ou um valor booleano para ele do código, e ele irá gerar um erro automaticamente se você tentar converter implicitamente um tipo de cadeia de caracteres não é válida para ele em tempo de execução.

O LINQ to SQL automaticamente manipula valores escape do SQL para você durante o uso de cadeias de caracteres, que ajuda a protegê-lo contra ataques de injeção SQL quando usá-lo.

#### <a name="validation-and-business-rule-logic"></a>Validação e lógica de regra de negócios

Validação de esquema é útil como uma primeira etapa, mas é raramente suficiente. A maioria dos cenários do mundo real exigem a capacidade de especificar a lógica de validação mais avançada que pode abranger várias propriedades, execute o código e geralmente têm reconhecimento de estado do modelo (por exemplo: está ele sendo criado/atualizada/excluídos, ou em um estado específico do domínio como "arquivados"). Há uma variedade de diferentes padrões e estruturas que podem ser usadas para definir e aplicar regras de validação a classes de modelo, e há várias estruturas .NET com base por aí que podem ser usadas para ajudá-lo com isso. Você pode usar praticamente qualquer um em aplicativos ASP.NET MVC.

Para fins de nosso aplicativo NerdDinner, usaremos um padrão relativamente simple e direta onde podemos expõe uma propriedade IsValid e um método GetRuleViolations() no objeto modelo refeição. A propriedade IsValid retorna true ou false dependendo se a validação e regras de negócios são válidas. O método GetRuleViolations() retornará uma lista de erros de regra.

Implementaremos IsValid e GetRuleViolations() para nosso modelo refeição adicionando uma "classe parcial" ao nosso projeto. Classes parciais podem ser usadas para adicionar métodos e propriedades/eventos para classes mantidas por um designer VS (como a classe refeição gerado pelo LINQ to SQL designer) e ajudar a evitar a ferramenta de misturado com nosso código. Podemos adicionar uma nova classe parcial para nosso projeto clicando na pasta \Models e, em seguida, selecione o comando de menu "Adicionar Novo Item". Podemos, em seguida, escolha o modelo de "Classe" na caixa de diálogo "Adicionar Novo Item" e nomeie-o Dinner.cs.

![](build-a-model-with-business-rule-validations/_static/image11.png)

Clicar no botão "Adicionar" adicionar um arquivo Dinner.cs ao nosso projeto e abra-o no IDE. Podemos, em seguida, implementar uma regra básica/validação imposição framework usando o código abaixo:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample10.cs)]

Algumas observações sobre o código acima:

- A classe refeição antecedida com uma palavra-chave "parcial" – que significa que o código contido nele será combinado com a classe gerada/mantidos pelo LINQ to SQL designer e compilado em uma única classe.
- A classe RuleViolation é uma classe auxiliar que vamos adicionar ao projeto que nos permite fornecer mais detalhes sobre uma violação de regra.
- O método Dinner.GetRuleViolations() faz com que a nossas validação e regras de negócios a ser avaliada (implementaremos-los em breve). Em seguida, ele volta retorna uma sequência de objetos RuleViolation que fornecem mais detalhes sobre os erros de regra.
- A propriedade Dinner.IsValid fornece uma propriedade de auxiliar conveniente que indica se o objeto de uma refeição tem qualquer RuleViolations ativo. Ele pode ser verificado de maneira proativa por um desenvolvedor usando o objeto de uma refeição a qualquer momento (e não gerará uma exceção).
- O método parcial Dinner.OnValidate() é um gancho LINQ to SQL tem que nos permite a ser notificado sempre que o objeto de uma refeição está prestes a ser mantido no banco de dados. Nossa implementação OnValidate() acima garante que a refeição não tenha nenhum RuleViolations, antes de ser salvo. Se ele estiver em um estado inválido, ele gera uma exceção, o que fará com que o LINQ to SQL para anular a transação.

Essa abordagem fornece uma estrutura simple que podemos pode se integrar a validação e regras de negócio em. Agora vamos adicionar as regras a seguir para nosso método GetRuleViolations():

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample11.cs)]

Estamos usando o recurso "yield return" do c# para retornar uma sequência de qualquer RuleViolations. O primeiro verifica seis regra acima simplesmente impor que propriedades de cadeia de caracteres em nosso refeição não podem ser nulo ou vazio. A última regra é um pouco mais interessante e chama um método auxiliar de PhoneValidator.IsValidNumber() que podemos adicionar à nosso projeto para verificar se o ContactPhone número país do formato corresponde uma refeição.

Podemos usar. Suporte a expressões regulares do NET para implementar o suporte de validação do telefone. Abaixo está uma simple implementação PhoneValidator que podemos adicionar ao nosso projeto que permite adicionar verificações de padrão Regex específico do país:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample12.cs)]

#### <a name="handling-validation-and-business-logic-violations"></a>Tratamento de validação e violações de lógica de negócios

Agora que adicionamos o código de regra comercial e validação acima, sempre que tentarmos para criar ou atualizar uma refeição, nossas regras de lógica de validação serão avaliadas e aplicadas.

Os desenvolvedores podem escrever código como abaixo proativamente determinar se um objeto de uma refeição é válido e recuperar uma lista de todas as violações nele sem gerar exceções:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample13.cs)]

Se tentar salvar uma refeição em um estado inválido, uma exceção será gerada quando podemos chamar o método Save () no DinnerRepository. Isso ocorre porque o LINQ to SQL chama automaticamente nosso método parcial Dinner.OnValidate() antes de ele salva as alterações da refeição e adicionamos código para Dinner.OnValidate() para gerar uma exceção se existirem quaisquer violações de regra na refeição. Podemos pegar essa exceção e reativo recuperar uma lista das violações de corrigir:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample14.cs)]

Porque nossas validação e regras de negócios são implementadas em nossa camada de modelo e não dentro da camada de interface do usuário, eles serão aplicados e usados em todos os cenários em nosso aplicativo. Podemos posteriormente alterar ou adicionar regras de negócio e ter todo o código que funciona com nossos objetos refeição honrá-los.

Com a flexibilidade para alterar as regras de negócio em um só lugar, sem ter que essas alterações ondulação em todo o aplicativo e a lógica da interface do usuário, é um sinal de um aplicativo bem escrito e um benefício que uma estrutura MVC ajuda incentivamos.

### <a name="next-step"></a>Próxima etapa

Agora temos um modelo que podemos usar tanto consultar e atualizar nosso banco de dados.

Vamos agora adicionar alguns controladores e exibições para o projeto que podemos usar para criar uma experiência de IU em HTML ao redor dele.

> [!div class="step-by-step"]
> [Anterior](create-a-database.md)
> [Próximo](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
