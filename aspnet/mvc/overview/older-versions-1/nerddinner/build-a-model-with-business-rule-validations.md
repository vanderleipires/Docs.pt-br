---
uid: mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
title: Criar um modelo com validações de regra de negócios | Microsoft Docs
author: microsoft
description: Etapa 3 mostra como criar um modelo que podemos usar para ambas as consultas e atualizar o banco de dados de nosso aplicativo NerdDinner.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 0bc191b2-4311-479a-a83a-7f1b1c32e6fe
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
msc.type: authoredcontent
ms.openlocfilehash: b735c414c629b9ff6617dbf80782d57543306c00
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834176"
---
<a name="build-a-model-with-business-rule-validations"></a>Criar um modelo com validações de regra de negócios
====================
por [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Esta é a etapa 3 do grátis [aplicativo "NerdDinner"](introducing-the-nerddinner-tutorial.md) que orienta-through como criar um pequeno, mas concluir, o aplicativo web usando ASP.NET MVC 1.
> 
> Etapa 3 mostra como criar um modelo que podemos usar para ambas as consultas e atualizar o banco de dados de nosso aplicativo NerdDinner.
> 
> Se você estiver usando o ASP.NET MVC 3, recomendamos que você siga a [obtendo iniciado com o MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [Store de música do MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriais.


## <a name="nerddinner-step-3-building-the-model"></a>Etapa 3 do NerdDinner: Criação do modelo

Uma estrutura model-view-controller o termo "modelo" refere-se aos objetos que representam os dados do aplicativo, bem como a lógica de domínio correspondentes que se integra a validação e regras de negócio com ele. O modelo está em muitas formas a "essência" de um aplicativo baseado em MVC e como veremos posteriormente fundamentalmente conduz o comportamento dele.

O ASP.NET MVC framework oferece suporte ao uso de qualquer tecnologia de acesso a dados e os desenvolvedores podem escolher entre uma variedade de opções de dados avançadas do .NET para implementar seus modelos, incluindo: LINQ to entidades, LINQ to SQL, NHibernate, LLBLGen Pro, o SubSonic, WilsonORM ou ADO apenas bruto. NET DataReaders ou conjuntos de dados.

Para nosso aplicativo NerdDinner vamos usar o LINQ to SQL para criar um modelo simples que corresponde bem de perto a nosso design de banco de dados e adiciona algumas regras de negócios e lógica de validação personalizada. Será, em seguida, implementamos uma classe de repositório que ajuda a distância abstrato a implementação de persistência de dados do restante do aplicativo e permite-na unidade facilmente testá-lo.

### <a name="linq-to-sql"></a>LINQ to SQL

O LINQ to SQL é um ORM (mapeador relacional de objetos) que é fornecido como parte do .NET 3.5.

O LINQ to SQL fornece uma maneira fácil para mapear tabelas de banco de dados para classes do .NET que pode codificar. Para nosso aplicativo NerdDinner usaremos-lo para mapear as tabelas de jantares e RSVP dentro do nosso banco de dados para o jantar e RSVP classes. As colunas das tabelas jantares e RSVP corresponderão às propriedades nas classes de Dinner e RSVP. Cada objeto de jantar e RSVP representará uma linha separada dentro das tabelas de jantares ou RSVP no banco de dados.

O LINQ to SQL permite evitar ter que construir manualmente instruções SQL para recuperar e atualizar o jantar e RSVP objetos com o banco de dados. Em vez disso, vamos definir as classes de Dinner e RSVP, como eles são mapeados para/do banco de dados e as relações entre eles. O LINQ to SQL, em seguida, será leva o cuidado de gerar a lógica de execução de SQL apropriada para usar em tempo de execução quando podemos interagir e usá-los.

Podemos usar o suporte de linguagem LINQ em VB e c# para escrever consultas expressivas que recuperam o jantar e RSVP objetos do banco de dados. Isso minimiza a quantidade de código de dados, precisamos escrever, e nos permite criar aplicativos realmente limpas.

### <a name="adding-linq-to-sql-classes-to-our-project"></a>Adicionando Classes LINQ to SQL ao nosso projeto

Vamos começar pelo botão direito do mouse na pasta "Modelos" em nosso projeto e selecionar o **Add -&gt;Novo Item** comando de menu:

![](build-a-model-with-business-rule-validations/_static/image1.png)

Isso abrirá a caixa de diálogo "Adicionar Novo Item". Vamos filtrar por categoria "Dados" e selecione o modelo de "Classes de LINQ to SQL" dentro dele:

![](build-a-model-with-business-rule-validations/_static/image2.png)

Vamos nomear o item "NerdDinner" e clique no botão "Adicionar". Visual Studio irá adicionar um arquivo NerdDinner.dbml em nosso diretório \Models e, em seguida, abra o designer do LINQ to SQL objeto relacional:

![](build-a-model-with-business-rule-validations/_static/image3.png)

### <a name="creating-data-model-classes-with-linq-to-sql"></a>Criando Classes de modelo de dados com LINQ to SQL

O LINQ to SQL nos permite criar rapidamente classes de modelo de dados de esquema de banco de dados existente. Tarefas pendentes isso vamos abrir o banco de dados do NerdDinner no Gerenciador de servidores e selecione as tabelas que queremos modelar nele:

![](build-a-model-with-business-rule-validations/_static/image4.png)

Podemos pode, em seguida, arraste as tabelas do LINQ à superfície de designer do SQL. Quando fazemos essa LINQ to SQL cria automaticamente jantar e classes RSVP usando o esquema das tabelas (com propriedades de classe que são mapeados para as colunas da tabela de banco de dados):

![](build-a-model-with-business-rule-validations/_static/image5.png)

Por padrão, o LINQ to SQL designer automaticamente "pluraliza" nomes de tabela e coluna quando ele cria classes com base em um esquema de banco de dados. Por exemplo: a tabela "Jantares" em nosso exemplo acima resultou em uma classe "Jantar". Essa classe de nomenclatura ajuda a tornar nossos modelos consistente com as convenções de nomenclatura do .NET, e eu geralmente considero que ter a correção designer essa backup conveniente (especialmente ao adicionar grande número de tabelas). Se você não gosta do nome de uma classe ou propriedade que gera o designer, no entanto, você sempre pode substituí-la e alterá-lo para qualquer nome que desejar. Você pode fazer isso editando a propriedade da entidade/nome em linha dentro do designer ou modificá-lo por meio da grade de propriedade.

Por padrão, o designer do LINQ to SQL também inspeciona as relações de chave estrangeira/chave primária das tabelas e com base neles automaticamente cria padrão "associações de relação" entre as classes de modelo diferente, que ele cria. Por exemplo, quando foi arrastado os jantares e RSVP tabelas para o designer do LINQ to SQL, uma associação de relação um-para-muitos entre os dois foi inferido com base no fato de que a tabela RSVP tinha uma chave estrangeira na tabela de jantares (Isso é indicado pela seta no Designer):

![](build-a-model-with-business-rule-validations/_static/image6.png)

A associação acima fará com que o LINQ to SQL para adicionar uma propriedade "Jantar" com rigidez de tipos à classe RSVP que os desenvolvedores podem usar para acessar o jantar associado com um determinado RSVP. Ele também fará com que a classe Dinner ter uma propriedade de coleção "RSVPs" que permite aos desenvolvedores recuperar e atualizar objetos RSVP associados com uma refeição específica.

Abaixo você pode ver um exemplo do intellisense no Visual Studio quando criamos um novo objeto RSVP e adicioná-lo à coleção de confirmações de um jantar. Observe como o LINQ to SQL adicionado automaticamente uma coleção de "RSVPs" no objeto de jantar:

![](build-a-model-with-business-rule-validations/_static/image7.png)

Adicionando o RSVP objeto à coleção de RSVPs do jantar estamos informando o LINQ to SQL para associar uma relação de chave estrangeira entre o jantar e a linha RSVP em nosso banco de dados:

![](build-a-model-with-business-rule-validations/_static/image8.png)

Se você não gostar de como o designer tem modelada ou chamada de uma associação de tabela, você pode substituí-la. Basta clicar na seta para associação dentro do designer e acessar suas propriedades por meio da grade de propriedade para renomear, excluir ou modificá-lo. Em nosso aplicativo NerdDinner, no entanto, as regras de associação padrão funcionam bem para as classes de modelo de dados que estamos criando e podemos apenas usar o comportamento padrão.

### <a name="nerddinnerdatacontext-class"></a>Classe NerdDinnerDataContext

Visual Studio criará automaticamente as classes do .NET que representam os modelos e as relações de banco de dados definidas usando o designer do LINQ to SQL. Uma classe LINQ to SQL DataContext também é gerado para cada arquivo LINQ to SQL designer adicionado à solução. Como nomeamos o nosso LINQ ao item de classe SQL "NerdDinner", a classe DataContext criada será chamada "NerdDinnerDataContext". Essa classe NerdDinnerDataContext é a principal maneira em que podemos irá interagir com o banco de dados.

Nossa classe NerdDinnerDataContext expõe duas propriedades - "Jantares" e "RSVPs -" que representam as duas tabelas que são modeladas no banco de dados. Podemos usar c# para escrever consultas LINQ em relação a essas propriedades para consulta e recuperar objetos de jantar e RSVP do banco de dados.

O código a seguir demonstra como instanciar um objeto NerdDinnerDataContext e executar uma consulta LINQ para obter uma sequência de jantares ocorrer no futuro. O Visual Studio fornece suporte total ao intellisense ao escrever a consulta LINQ, e os objetos retornados dele são fortemente tipadas e também dão suporte a intellisense:

![](build-a-model-with-business-rule-validations/_static/image9.png)

Além do que nos permite consultar objetos de jantar e RSVP, um NerdDinnerDataContext rastreia automaticamente as alterações que fazemos subsequentemente para os objetos de jantar e RSVP que recuperamos através dele. Podemos usar essa funcionalidade para salvar facilmente as alterações no banco de dados – sem precisar escrever nenhum código de atualização explícito do SQL.

Por exemplo, o código a seguir demonstra como usar uma consulta LINQ para recuperar um único objeto de jantar do banco de dados, atualizar duas das propriedades do jantar e, em seguida, salve as alterações no banco de dados:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample1.cs)]

O objeto NerdDinnerDataContext no código acima rastreadas automaticamente as alterações de propriedade feitas para o objeto de Dinner que recuperamos dele. Quando chamamos o método "SubmitChanges()", ele executará uma declaração SQL apropriada de "Atualizar", no banco de dados para manter os valores atualizados de volta.

### <a name="creating-a-dinnerrepository-class"></a>Criando uma classe de DinnerRepository

Para pequenos aplicativos, às vezes, é suficiente ter controladores de trabalhar diretamente em uma classe LINQ to SQL DataContext e inserir consultas LINQ em controladores. Como os aplicativos ficam maiores, no entanto, essa abordagem se torna difícil de manter e testar. Ele também pode levar conosco duplicando as mesmas consultas LINQ em vários lugares.

Uma abordagem que pode tornar mais fácil de manter e testar os aplicativos é usar um padrão de "repositório". Uma classe de repositório ajuda a encapsular a consulta de dados e lógica de persistência e abstrai os detalhes da implementação da persistência de dados do aplicativo. Além de tornar o código do aplicativo mais limpo, usando um padrão de repositório pode tornar mais fácil alterar as implementações de armazenamento de dados no futuro, e ele pode ajudar a facilitar a unidade testando um aplicativo sem a necessidade de um banco de dados real.

Para nosso aplicativo NerdDinner, definiremos uma classe de DinnerRepository com o abaixo de assinatura:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample2.cs)]

*Observação: Neste capítulo vamos extrair uma interface de IDinnerRepository dessa classe e habilitar a injeção de dependência com ele em nosso controladores. Para começar, no entanto, vamos iniciar simples e apenas trabalhar diretamente com a classe de DinnerRepository.*

Para implementar essa classe, vamos com o botão direito em nossa pasta de "Modelos" e escolha o **Add -&gt;Novo Item** comando de menu. Na caixa de diálogo "Adicionar Novo Item" Estamos vai selecionar o modelo "Class" e nomeie o arquivo "DinnerRepository.cs":

![](build-a-model-with-business-rule-validations/_static/image10.png)

Em seguida, podemos implementar nossa classe DinnerRespository usando o código a seguir:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample3.cs)]

### <a name="retrieving-updating-inserting-and-deleting-using-the-dinnerrepository-class"></a>Recuperar, atualizar, inserir e excluir usando a classe de DinnerRepository

Agora que criamos nossa classe DinnerRepository, vamos examinar alguns exemplos de código que demonstram tarefas comuns que podemos fazer com ele:

#### <a name="querying-examples"></a>Exemplos de consultas

O código a seguir recupera um jantar único usando o valor de DinnerID:


[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample4.cs)]

O código a seguir recupera todos os jantares futuros e loops sobre eles:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample5.cs)]

#### <a name="insert-and-update-examples"></a>INSERT e Update exemplos

O código a seguir demonstra como adicionar dois jantares de novo. Adições/modificações para o repositório não são confirmadas no banco de dados até que o método "Save ()" é chamado nela. LINQ to SQL é quebrado automaticamente todas as alterações em uma transação de banco de dados – portanto, todas as alterações acontecem ou nenhuma delas fazer ao nosso repositório salva:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample6.cs)]

O código a seguir recupera um objeto existente do jantar e modifica duas propriedades nele. As alterações são confirmadas no banco de dados quando o método "Save ()" é chamado em nosso repositório:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample7.cs)]

O código a seguir recupera um jantar e, em seguida, adiciona uma confirmação de presença a ele. Ele faz isso usando a coleção de confirmações no objeto de Dinner que LINQ to SQL criado para nós (porque não há uma relação primary key/foreign key entre os dois no banco de dados). Essa alteração é mantida no banco de dados como uma nova linha na tabela RSVP quando o método "Save ()" é chamado no repositório:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample8.cs)]

#### <a name="delete-example"></a>Exemplo-excluir

O código a seguir recupera um objeto existente do jantar e, em seguida, a marca a ser excluído. Quando o método "Save ()" é chamado no repositório confirmará a exclusão no banco de dados:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample9.cs)]

### <a name="integrating-validation-and-business-rule-logic-with-model-classes"></a>Integração de validação e lógica de regra de negócios com Classes de modelo

Integração de validação e a lógica é uma parte fundamental de qualquer aplicativo que funciona com os dados de regra de negócio.

#### <a name="schema-validation"></a>Validação de esquema

Quando as classes de modelo são definidas usando o designer do LINQ to SQL, os tipos de dados das propriedades nas classes de modelo de dados correspondem aos tipos de dados da tabela de banco de dados. Por exemplo: se a coluna "EventDate" na tabela de jantares "datetime", a classe de modelo de dados criada pelo LINQ to SQL será do tipo "DateTime" (que é um tipo de dados interno do .NET). Isso significa que você receberá erros de compilação se você tentar atribuir um inteiro ou um valor booliano a ele no código, e ele irá gerar um erro automaticamente se você tentar converter implicitamente um tipo de cadeia de caracteres não é válida para ele em tempo de execução.

O LINQ to SQL também automaticamente manipula valores de escape do SQL para você ao usar cadeias de caracteres, que ajuda a protegê-lo contra ataques de injeção de SQL ao usá-lo.

#### <a name="validation-and-business-rule-logic"></a>Validação e lógica de regra de negócios

Validação de esquema é útil como uma primeira etapa, mas é raramente é suficiente. A maioria dos cenários do mundo real exigem a capacidade de especificar a lógica de validação mais avançada que pode abranger várias propriedades, executar o código e geralmente têm reconhecimento de um estado de modelo (por exemplo: é ele que está sendo criado/atualizado/excluído, ou dentro de um estado específico do domínio como "arquivados"). Há uma variedade de diferentes padrões e estruturas que podem ser usadas para definir e aplicar regras de validação a classes de modelo, e há várias estruturas .NET com base por aí que podem ser usadas para ajudar com isso. Você pode usar praticamente qualquer um deles dentro de aplicativos ASP.NET MVC.

Para fins de nosso aplicativo NerdDinner, vamos usar um padrão relativamente simple e direta onde podemos expor uma propriedade IsValid e um método GetRuleViolations() no nosso objeto de modelo do jantar. A propriedade IsValid retornará true ou false dependendo se a validação e regras de negócios são válidas. O método GetRuleViolations() retornará uma lista de erros de regra.

Implementaremos IsValid e GetRuleViolations() para nosso modelo de jantar, adicionando uma "classe parcial" ao nosso projeto. Classes parciais podem ser usadas para adicionar métodos/propriedades/eventos às classes mantidas por um designer do VS (como a classe Dinner gerado pelo designer do LINQ to SQL) e ajudar a evitar a ferramenta de complicação com nosso código. Podemos adicionar uma nova classe parcial para nosso projeto clicando na pasta \Models e, em seguida, selecione o comando de menu "Adicionar Novo Item". Podemos, em seguida, escolha o modelo de "Classe" na caixa de diálogo "Adicionar Novo Item" e nomeie-o Dinner.cs.

![](build-a-model-with-business-rule-validations/_static/image11.png)

Clicando no botão "Adicionar" adicionar um arquivo de Dinner.cs ao nosso projeto e abra-o dentro do IDE. Podemos, em seguida, implementar um framework de imposição de regra/validação básica usando o código abaixo:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample10.cs)]

Algumas observações sobre o código acima:

- A classe de jantar é precedida por uma palavra-chave "parcial" – que significa que o código contido nele será combinado com a classe gerada/mantidos pelo designer do LINQ to SQL e compilado em uma única classe.
- A classe RuleViolation é uma classe auxiliar que vamos adicionar ao projeto que nos permite fornecer mais detalhes sobre uma violação de regra.
- O método Dinner.GetRuleViolations() faz com que nosso validação e regras de negócios a ser avaliado (implementaremos-los em breve). Em seguida, ele volta retorna uma sequência de objetos RuleViolation que fornecem mais detalhes sobre os erros de regra.
- A propriedade Dinner.IsValid fornece uma propriedade de auxiliares convenientes que indica se o objeto de jantar tem qualquer RuleViolations Active Directory. Ele pode ser verificado de forma proativa por um desenvolvedor usando o objeto de jantar a qualquer momento (e não gera uma exceção).
- O método parcial de Dinner.OnValidate() é um gancho que fornece o LINQ to SQL que nos permite ser notificado sempre que o objeto de jantar está prestes a ser persistido no banco de dados. Nossa implementação OnValidate() acima garante que o jantar não tenha nenhum RuleViolations antes de ser salvo. Se ele estiver em um estado inválido, ele gera uma exceção, o que fará com que o LINQ to SQL para anular a transação.

Essa abordagem fornece uma estrutura simple que podemos pode integrar a validação e regras de negócio em. Agora vamos adicionar as regras a seguir para nosso método GetRuleViolations():

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample11.cs)]

Estamos usando o recurso "yield return" do c# para retornar uma sequência de qualquer RuleViolations. O primeiro verifica seis regra acima simplesmente impõe que as propriedades de cadeia de caracteres em nosso jantar não podem ser nulo ou vazio. A última regra é um pouco mais interessante e chama um método auxiliar de PhoneValidator.IsValidNumber() que podemos adicionar ao nosso projeto para verificar se o ContactPhone número país do formato correspondências o jantar.

Podemos usar. Suporte a expressões regulares do NET para implementar esse suporte de validação do telefone. Abaixo está uma implementação de PhoneValidator simples que podemos adicionar ao nosso projeto que nos permite adicionar verificações de padrão de Regex específicas do país:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample12.cs)]

#### <a name="handling-validation-and-business-logic-violations"></a>Tratamento de violações de lógica de negócios e de validação

Agora que adicionamos o código da regra validação e negócios acima, sempre que podemos tentar criar ou atualizar um jantar, nossas regras de lógica de validação serão avaliadas e impostas.

Os desenvolvedores podem escrever código, como abaixo proativamente determinar se um objeto de jantar é válido e recuperar uma lista de todas as violações nela sem gerar qualquer exceção:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample13.cs)]

Se podemos tentar salvar um jantar em um estado inválido, uma exceção será gerada quando chamamos o método Save () de DinnerRepository. Isso ocorre porque o LINQ to SQL chama nosso método parcial de Dinner.OnValidate() automaticamente antes que ele salva as alterações do jantar e adicionamos código para Dinner.OnValidate() para gerar uma exceção se qualquer violação de regra existir no jantar. Podemos capturar essa exceção e de forma reativa recuperar uma lista das violações corrigir:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample14.cs)]

Como nosso validação e regras de negócios são implementadas em nossa camada de modelo e não dentro da camada de interface do usuário, serão aplicadas e usados em todos os cenários dentro do nosso aplicativo. Podemos posteriormente alterar ou adicionar regras de negócio e ter todo o código que funciona com nossos objetos jantar honra-los.

Ter a flexibilidade para alterar as regras de negócio em um só lugar, sem a necessidade de ondulação em todo o aplicativo e a lógica de interface do usuário, essas alterações é um sinal de um aplicativo bem escrito e um benefício que uma estrutura MVC ajuda a incentivar.

### <a name="next-step"></a>Próxima etapa

Agora temos um modelo que podemos usar tanto consultar e atualizar nosso banco de dados.

Vamos agora adicionar alguns controladores e modos de exibição para o projeto que podemos usar para criar uma experiência de interface do usuário HTML ao redor dele.

> [!div class="step-by-step"]
> [Anterior](create-a-database.md)
> [Próximo](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
