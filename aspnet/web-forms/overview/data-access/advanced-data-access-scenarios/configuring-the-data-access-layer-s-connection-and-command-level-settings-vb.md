---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-vb
title: "Definindo configurações de nível de Conexão e comando da camada de acesso a dados (VB) | Microsoft Docs"
author: rick-anderson
description: Os TableAdapters dentro de um conjunto de dados tipado automaticamente cuidar de se conectar ao banco de dados, emitir comandos e preencher uma DataTable com os resultados...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: d57dfa2b-d627-45cb-b5b1-abbf3159d770
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-vb
msc.type: authoredcontent
ms.openlocfilehash: f2da69ba1b7511e8659ab7212785e8148b438a4b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="configuring-the-data-access-layers-connection--and-command-level-settings-vb"></a>Definindo configurações de nível de Conexão e comando da camada de acesso a dados (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_72_VB.zip) ou [baixar PDF](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/datatutorial72vb1.pdf)

> Os TableAdapters dentro de um conjunto de dados tipado automaticamente cuidar de se conectar ao banco de dados, emitir comandos e preencher uma DataTable com os resultados. Há ocasiões, no entanto, quando queremos lidar com esses detalhes nos e neste tutorial, saber como acessar as configurações de nível de conexão e comando de banco de dados no TableAdapter.


## <a name="introduction"></a>Introdução

Em toda a série de tutoriais usamos DataSets tipados para implementar a camada de acesso a dados e objetos de negócios da nossa arquitetura em camadas. Como discutido o [primeiro tutorial](../introduction/creating-a-data-access-layer-vb.md), o s do conjunto de dados tipado DataTables servem como repositórios de dados enquanto os TableAdapters atuar como wrappers para se comunicar com o banco de dados para recuperar e modificar os dados subjacentes. Os TableAdapters encapsular a complexidade envolvida ao trabalhar com o banco de dados e economiza da necessidade de escrever código para se conectar ao banco de dados, execute um comando ou preencher os resultados em uma DataTable.

Há ocasiões, entretanto, quando é preciso burrow dentro da profundidade do TableAdapter e gravar o código que trabalha diretamente com os objetos do ADO.NET. No [encapsulamento modificações de banco de dados em uma transação](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md) tutorial, por exemplo, adicionamos métodos TableAdapter para início, confirmação e reversão de transações do ADO.NET. Esses métodos usados interno, criados manualmente `SqlTransaction` objeto que foi atribuído ao TableAdapter s `SqlCommand` objetos.

Neste tutorial, examinaremos como acessar as configurações de nível de conexão e comando de banco de dados no TableAdapter. Em particular, vamos adicionar funcionalidade a `ProductsTableAdapter` que permite acessar à cadeia de conexão subjacente e configurações de tempo limite de comando.

## <a name="working-with-data-using-adonet"></a>Trabalhando com dados usando o ADO.NET

O Microsoft .NET Framework contém uma grande quantidade de classes criada especificamente para trabalhar com dados. Essas classes, encontradas na [ `System.Data` namespace](https://msdn.microsoft.com/en-us/library/system.data.aspx), são chamados a *ADO.NET* classes. Algumas das classes sob a proteção do ADO.NET estão vinculadas a um determinado *provedor de dados*. Você pode pensar em um provedor de dados como um canal de comunicação que permite que as informações entre as classes ADO.NET e o armazenamento de dados subjacente. Há provedores generalizadas, como OLE DB e ODBC, bem como provedores são projetados especialmente para um sistema de banco de dados específico. Por exemplo, embora seja possível se conectar a um banco de dados do Microsoft SQL Server usando o provedor OLE DB, o provedor SqlClient é muito mais eficiente, pois ele foi projetado e otimizado especificamente para o SQL Server.

Quando acessar programaticamente os dados, geralmente é usado o seguinte padrão:

1. Estabelece uma conexão ao banco de dados.
2. Emita um comando.
3. Para `SELECT` consultas, trabalhar com os registros resultantes.

Existem classes ADO.NET separadas para executar cada uma dessas etapas. Para se conectar a um banco de dados usando o provedor SqlClient, por exemplo, use o [ `SqlConnection` classe](https://msdn.microsoft.com/en-us/library/system.data.sqlclient.sqlconnection(VS.80).aspx). Para emitir um `INSERT`, `UPDATE`, `DELETE`, ou `SELECT` comando no banco de dados, use o [ `SqlCommand` classe](https://msdn.microsoft.com/en-us/library/system.data.sqlclient.sqlcommand.aspx).

Exceto para o [encapsulamento modificações de banco de dados em uma transação](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md) tutorial, não foi necessário gravar qualquer código ADO.NET de baixo nível nos porque o código a TableAdapters gerada automaticamente inclui a funcionalidade necessária para Conecte-se ao banco de dados, execute comandos, recuperar dados e preencher esses dados em tabelas de dados. No entanto, pode haver ocasiões em que precisa para personalizar essas configurações de nível inferior. Sobre as próximas etapas, examinaremos como aproveitar os objetos do ADO.NET usados internamente pelo qual os TableAdapters.

## <a name="step-1-examining-with-the-connection-property"></a>Etapa 1: Examinando com a propriedade de Conexão

Cada classe TableAdapter tem um `Connection` propriedade que especifica informações de conexão de banco de dados. Esse tipo de dados de propriedade s e `ConnectionString` valor são determinadas pelas seleções feitas no Assistente de configuração do TableAdapter. Lembre-se de que quando o primeiro adicionamos um TableAdapter para um conjunto de dados tipado esse assistente faz nos para o banco de dados de origem (consulte a Figura 1). A lista suspensa essa primeira etapa inclui os bancos de dados especificados no arquivo de configuração, bem como quaisquer outros bancos de dados em Server Explorer s conexões de dados. Se o banco de dados que você deseja usar não existe na lista suspensa, uma nova conexão de banco de dados pode ser especificado ao clicar no botão Nova Conexão e fornecer as informações de conexão necessárias.


[![A primeira etapa do Assistente de configuração do TableAdapter](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image2.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image1.png)

**Figura 1**: A primeira etapa do Assistente de configuração TableAdapter ([clique para exibir a imagem em tamanho normal](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image3.png))


Permitem s levar alguns instantes para inspecionar o código para o s TableAdapter `Connection` propriedade. Conforme observado no [criando uma camada de acesso a dados](../introduction/creating-a-data-access-layer-vb.md) tutorial, podemos pode exibir o código de TableAdapter geradas automaticamente indo para a janela de exibição de classe, detalhar para a classe apropriada e, em seguida, duas vezes no nome do membro.

Navegue até a janela de exibição de classe, no menu de exibição e escolhendo o modo de exibição de classe (ou digitando Ctrl + Shift + C). Na metade superior da janela de exibição de classe, fazer drill down até o `NorthwindTableAdapters` namespace e selecione o `ProductsTableAdapter` classe. Isso exibirá a `ProductsTableAdapter` membros s na parte inferior metade da exibição de classe, conforme mostrado na Figura 2. Clique duas vezes o `Connection` propriedade para ver seu código.


![Clique duas vezes a propriedade de Conexão no modo de exibição de classe para exibir o código gerado automaticamente](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image4.png)

**Figura 2**: clique duas vezes a propriedade de Conexão no modo de exibição de classe para exibir o código gerado automaticamente


O TableAdapter s `Connection` propriedade e outra maneira de código relacionadas à conexão:


[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample1.vb)]

Quando a classe do TableAdapter é instanciada, a variável de membro `_connection` é igual a `Nothing`. Quando o `Connection` propriedade é acessada, ele primeiro verifica se o `_connection` variável de membro foi instanciado. Se não tiver o `InitConnection` método é chamado, o que cria uma instância `_connection` e define seu `ConnectionString` propriedade para o valor de cadeia de caracteres de conexão especificado na etapa configuração TableAdapter Assistente s primeiro.

O `Connection` propriedade também pode ser atribuída a um `SqlConnection` objeto. Isso associa o novo `SqlConnection` objeto com cada TableAdapter s `SqlCommand` objetos.

## <a name="step-2-exposing-connection-level-settings"></a>Etapa 2: Expor as configurações de nível de Conexão

As informações de conexão devem permanecer encapsuladas em TableAdapter e não estar acessível a outras camadas na arquitetura do aplicativo. No entanto, pode haver situações quando as informações de nível de conexão do TableAdapter s devem ser acessível ou personalizável para uma consulta, um usuário ou uma página ASP.NET.

Permitem s estender o `ProductsTableAdapter` no `Northwind` conjunto de dados para incluir um `ConnectionString` propriedade que pode ser usada pela camada de lógica de negócios para ler ou alterar a cadeia de conexão usada pelo TableAdapter.

> [!NOTE]
> Um *cadeia de caracteres de conexão* é uma cadeia de caracteres que especifica as informações de conexão de banco de dados, como o provedor a ser usado, o local do banco de dados, as credenciais de autenticação e outras configurações de banco de dados. Para obter uma lista de padrões de cadeia de caracteres de conexão usada por uma variedade de armazenamentos de dados e provedores, consulte [ConnectionStrings.com](http://www.connectionstrings.com/).


Como discutido o [criando uma camada de acesso a dados](../introduction/creating-a-data-access-layer-vb.md) tutorial, as classes geradas automaticamente do conjunto de dados tipado s podem ser estendidas com o uso de classes parciais. Primeiro, crie uma nova subpasta no projeto chamado `ConnectionAndCommandSettings` sob o `~/App_Code/DAL` pasta.


![Adicionar uma subpasta chamada ConnectionAndCommandSettings](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image5.png)

**Figura 3**: adicionar uma subpasta chamada`ConnectionAndCommandSettings`


Adicionar um novo arquivo de classe chamado `ProductsTableAdapter.ConnectionAndCommandSettings.vb` e insira o código a seguir:


[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample2.vb)]

Essa classe parcial adiciona um `Public` propriedade denominada `ConnectionString` para o `ProductsTableAdapter` classe que permite que qualquer camada ler ou atualizar a cadeia de conexão para o s TableAdapter subjacente a conexão.

Com essa classe parcial criado (e salvo), abra o `ProductsBLL` classe. Ir para um dos métodos existentes e digite `Adapter` e, em seguida, pressione a tecla Períoda ao ativar o IntelliSense. Você deve ver o novo `ConnectionString` propriedade disponível no IntelliSense, que significa que você pode ler ou ajustar este valor de BLL programaticamente.

## <a name="exposing-the-entire-connection-object"></a>Expondo o objeto de Conexão inteira

Essa classe parcial expõe apenas uma propriedade do objeto de conexão subjacente: `ConnectionString`. Se desejar disponibilizar o objeto de conexão inteira além dos limites do TableAdapter, você também pode alterar o `Connection` nível de proteção de propriedade s. O código gerado automaticamente, examinamos na etapa 1 mostrou que o s TableAdapter `Connection` propriedade for marcada como `Friend`, que significa que ele somente pode ser acessado por classes no mesmo assembly. Isso pode ser alterado, no entanto, por meio do TableAdapter s `ConnectionModifier` propriedade.

Abra o `Northwind` conjunto de dados, clique no `ProductsTableAdatper` no Designer e navegue até a janela Propriedades. Lá você verá o `ConnectionModifier` definido como seu valor padrão, `Assembly`. Para fazer o `Connection` disponível fora do assembly do conjunto de dados tipado s, alteração de propriedade de `ConnectionModifier` propriedade `Public`.


[![O nível de acessibilidade de s de propriedade de Conexão pode ser configurado por meio da propriedade ConnectionModifier](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image7.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image6.png)

**Figura 4**: O `Connection` propriedade s acessibilidade nível pode ser configurado por meio de `ConnectionModifier` propriedade ([clique para exibir a imagem em tamanho normal](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image8.png))


Salvar o conjunto de dados e, em seguida, retornar para o `ProductsBLL` classe. Como antes, vá para um dos métodos existentes e digite `Adapter` e, em seguida, pressione a tecla Períoda ao ativar o IntelliSense. A lista deve conter um `Connection` propriedade, o que significa que você pode agora programaticamente ler ou atribuir as configurações de nível de conexão do BLL.

## <a name="step-3-examining-the-command-related-properties"></a>Etapa 3: Examinando as propriedades de comando

Um TableAdapter consiste em uma consulta principal que, por padrão, foi gerado automaticamente `INSERT`, `UPDATE`, e `DELETE` instruções. Esta consulta principal s `INSERT`, `UPDATE`, e `DELETE` instruções são implementadas no código TableAdapter s como um objeto de adaptador de dados do ADO.NET por meio de `Adapter` propriedade. Como com seus `Connection` propriedade, o `Adapter` tipo de dados de propriedade s é determinado pelo provedor de dados usado. Como esses tutoriais usam o provedor SqlClient, o `Adapter` é de propriedade do tipo [ `SqlDataAdapter` ](https://msdn.microsoft.com/en-us/library/system.data.sqlclient.sqldataadapter(VS.80).aspx).

O TableAdapter s `Adapter` propriedade tem três propriedades do tipo `SqlCommand` que ele usa para problema de `INSERT`, `UPDATE`, e `DELETE` instruções:

- `InsertCommand`
- `UpdateCommand`
- `DeleteCommand`

Um `SqlCommand` objeto é responsável por enviar uma consulta específica ao banco de dados e tem propriedades como: [ `CommandText` ](https://msdn.microsoft.com/en-us/library/system.data.sqlclient.sqlcommand.commandtext.aspx), que contém a instrução de SQL ad hoc ou procedimento armazenado a ser executada; e [ `Parameters` ](https://msdn.microsoft.com/en-us/library/system.data.sqlclient.sqlcommand.parameters.aspx), que é uma coleção de `SqlParameter` objetos. Como vimos no [criando uma camada de acesso a dados](../introduction/creating-a-data-access-layer-vb.md) tutorial, esses comando objetos podem ser personalizados usando a janela Propriedades.

Além de sua consulta principal, o TableAdapter pode incluir um número variável de métodos que, quando chamado, enviar um comando especificado no banco de dados. O objeto de comando de consulta principal s e os objetos de comando para todos os métodos adicionais são armazenados no TableAdapter s `CommandCollection` propriedade.

Permitem s dedicar um tempo para examinar o código gerado pelo `ProductsTableAdapter` no `Northwind` conjunto de dados para essas duas propriedades e suas variáveis de membro de suporte e os métodos auxiliares:


[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample3.vb)]

O código para o `Adapter` e `CommandCollection` propriedades imita de perto o `Connection` propriedade. Há variáveis de membro que contêm objetos usados pelas propriedades. As propriedades `Get` acessadores comece verificando se a variável de membro correspondente é `Nothing`. Nesse caso, um método de inicialização é chamado, que cria uma instância da variável de membro e atribui o núcleo propriedades relacionadas ao comando.

## <a name="step-4-exposing-command-level-settings"></a>Etapa 4: Expor as configurações de nível de comando

Idealmente, as informações de nível de comando devem permanecer encapsuladas na camada de acesso a dados. Essa informação será necessária em outras camadas da arquitetura, no entanto, ele pode ser exposto por meio de uma classe parcial, assim como com as configurações de nível de conexão.

Como o TableAdapter tem apenas um único `Connection` propriedade, o código para expor as configurações de nível de conexão é bastante simples. As coisas são um pouco mais complicadas ao modificar as configurações de nível de comando porque o TableAdapter pode ter vários objetos de comando - um `InsertCommand`, `UpdateCommand`, e `DeleteCommand`, junto com um número variável de objetos de comando na `CommandCollection` propriedade. Ao atualizar configurações de nível de comando, essas configurações precisará ser propagadas para todos os objetos de comando.

Por exemplo, imagine que havia determinadas consultas TableAdapter que levou um tempo longo extraordinário para executar. Ao usar o TableAdapter para executar uma das consultas, queremos aumentar o objeto de comando s [ `CommandTimeout` propriedade](https://msdn.microsoft.com/en-us/library/system.data.sqlclient.sqlcommand.commandtimeout.aspx). Essa propriedade especifica o número de segundos de espera para executar o comando e o padrão é 30.

Para permitir que o `CommandTimeout` propriedade deve ser ajustado BLL, adicione o seguinte `Public` método para o `ProductsDataTable` usando o arquivo de classe parcial criada na etapa 2 (`ProductsTableAdapter.ConnectionAndCommandSettings.vb`):


[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample4.vb)]

Esse método pode ser invocado da BLL ou camada de apresentação para definir o tempo limite do comando para todos os problemas de comandos por aquela instância do TableAdapter.

> [!NOTE]
> O `Adapter` e `CommandCollection` propriedades são marcadas como `Private`, que significa que eles só podem ser acessados de código dentro do TableAdapter. Ao contrário do `Connection` propriedade, esses modificadores de acesso não são configuráveis. Portanto, se você precisar expor as propriedades de nível de comando para outras camadas da arquitetura deve usar a abordagem de classe parcial discutida acima para fornecer uma `Public` método ou propriedade que lê ou grava o `Private` objetos de comando.


## <a name="summary"></a>Resumo

Os TableAdapters dentro de um conjunto de dados tipado servem para encapsular os detalhes de acesso de dados e complexidade. Usando TableAdapters, não temos se preocupar sobre como escrever código ADO.NET para conectar-se ao banco de dados, emitir um comando ou preencher os resultados em uma DataTable. É tudo tratado automaticamente para nós.

No entanto, pode haver ocasiões em que precisa para personalizar as especificações ADO.NET de baixo nível, como alterar a cadeia de caracteres de conexão ou os valores padrão de tempo limite de conexão ou o comando. O TableAdapter foi gerado automaticamente `Connection`, `Adapter`, e `CommandCollection` propriedades, mas eles são `Friend` ou `Private`, por padrão. Essas informações internas podem ser expostas ao estender o TableAdapter usando classes parciais para incluir `Public` métodos ou propriedades. Como alternativa, o TableAdapter s `Connection` modificador de acesso de propriedade pode ser configurado por meio do TableAdapter s `ConnectionModifier` propriedade.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Gerar revisores para este tutorial foram Burnadette Leigh, S ren Jacob Lauritsen, Teresa Murphy e Geisenow Hilton. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](working-with-computed-columns-vb.md)
[Próximo](protecting-connection-strings-and-other-configuration-information-vb.md)
