---
uid: web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
title: Perfis, temas e Web Parts | Microsoft Docs
author: microsoft
description: Há grandes alterações na configuração e instrumentação no ASP.NET 2.0. A nova API de configuração do ASP.NET permite alterações de configuração a serem feitas pr...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 92df4051-77c6-492c-bd34-23d24189cea4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
msc.type: authoredcontent
ms.openlocfilehash: b749ed093fbaacf45b60f2826a2c20bac219a5c7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="profiles-themes-and-web-parts"></a>Perfis, temas e Web Parts
====================
por [Microsoft](https://github.com/microsoft)

> Há grandes alterações na configuração e instrumentação no ASP.NET 2.0. A nova API de configuração do ASP.NET permite alterações de configuração sejam feitas por meio de programação. Além disso, existem a muitas novas configurações permitem que as novas configurações e instrumentação.


O ASP.NET 2.0 representa uma melhoria significativa na área de sites personalizados. O recursos de associação weve já abordada, além de perfis do ASP.NET, temas e Web parts significativamente aprimoram a personalização em sites da Web.

## <a name="aspnet-profiles"></a>Perfis do ASP.NET

Perfis do ASP.NET são semelhantes às sessões. A diferença é que um perfil é persistente, enquanto uma sessão é perdida quando o navegador for fechado. Outra grande diferença entre as sessões e de perfis é que os perfis são fortemente tipados, portanto, oferecendo IntelliSense durante o processo de desenvolvimento.

Um perfil é definido no arquivo de configuração de máquinas ou o arquivo Web. config para o aplicativo. (Você não pode definir um perfil em um arquivo Web. config de subpastas). O código a seguir define um perfil para armazenar os visitantes do site da Web primeiro e último nome.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample1.xml)]

O tipo de dados padrão para uma propriedade de perfil é System. String. No exemplo acima, nenhum tipo de dados foi especificado. Portanto, as propriedades de FirstName e LastName são do tipo cadeia de caracteres. Como mencionado anteriormente, perfil de propriedades são fortemente tipadas. O código a seguir adiciona uma nova propriedade para a idade é do tipo Int32.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample2.xml)]

Perfis são geralmente usados com autenticação de formulários do ASP.NET. Quando usado em combinação com a autenticação de formulários, cada usuário tem um perfil separado associado com a ID de usuário. No entanto, também é possível permitir o uso de perfis em um aplicativo anônimo usando o &lt;anonymousIdentification&gt; elemento no arquivo de configuração, juntamente com o **allowAnonymous** atributo como como mostrado abaixo:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample3.xml)]

Quando um usuário anônimo procura o site, o ASP.NET cria uma instância de **ProfileCommon** para o usuário. Esse perfil usa uma ID exclusiva armazenada em um cookie no navegador para identificar o usuário como um visitante exclusivo. Dessa forma, você pode armazenar informações de perfil para os usuários que estão acessando anonimamente.

## <a name="profile-groups"></a>Grupos de perfis

É possível para propriedades do grupo de perfis. Por propriedades de agrupamento, é possível simular vários perfis para um aplicativo específico.

A configuração a seguir define uma propriedade FirstName e LastName para dois grupos; Compradores e potenciais.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample4.xml)]

Em seguida, é possível definir propriedades em um determinado grupo, da seguinte maneira:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample5.cs)]

## <a name="storing-complex-objects"></a>Armazenando objetos complexos

Até agora, os exemplos que abordamos armazenou os tipos de dados simples em um perfil. Também é possível armazenar tipos de dados complexos em um perfil, especificando o método de serialização usando o **serializeAs** atributo da seguinte maneira:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample6.xml)]

Nesse caso, o tipo é PurchaseInvoice. A classe PurchaseInvoice precisa ser marcado como serializável e pode conter qualquer número de propriedades. Por exemplo, se PurchaseInvoice tem uma propriedade chamada **NumItemsPurchased**, você pode fazer referência a essa propriedade no código da seguinte maneira:

[!code-css[Main](profiles-themes-and-web-parts/samples/sample7.css)]

## <a name="profile-inheritance"></a>Herança de perfil

É possível criar um perfil para uso em vários aplicativos. Criando uma classe de perfil que deriva de ProfileBase, você pode reutilizar um perfil em vários aplicativos usando o **herda** atributo conforme mostrado abaixo:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample8.xml)]

Nesse caso, a classe **PurchasingProfile** ficaria assim:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample9.cs)]

## <a name="profile-providers"></a>Provedores de perfil

Perfis do ASP.NET usam o modelo de provedor. O provedor padrão armazena as informações em um banco de dados do SQL Server Express no aplicativo\_a pasta dados do aplicativo Web usando o provedor SqlProfileProvider. Se o banco de dados não existir, ASP.NET será criá-lo automaticamente quando o perfil de tentativas armazenar informações.

Em alguns casos, no entanto, você talvez queira desenvolver seu próprio provedor de perfil. O recurso de perfil do ASP.NET permite que você facilmente usar provedores diferentes.

Criar um provedor de perfil personalizado quando:

- Você precisa armazenar informações de perfil em uma fonte de dados, como em um banco de dados FoxPro ou em um banco de dados Oracle, que não é suportado pelos provedores de perfil incluídos com o .NET Framework.
- Você precisará gerenciar informações de perfil usando um esquema de banco de dados que é diferente do esquema do banco de dados usado pelos provedores incluídos com o .NET Framework. Um exemplo comum é que você deseja integrar informações de perfil de dados do usuário em um banco de dados existente do SQL Server.

### <a name="required-classes"></a>Classes necessárias

Para implementar um provedor de perfil, crie uma classe que herda da classe abstrata System.Web.Profile.ProfileProvider. O **ProfileProvider** classe abstrata, por sua vez, herda a classe abstrata System.Configuration.SettingsProvider, que herda da classe abstrata ProviderBase. Devido a esta cadeia de herança, além dos membros necessários do **ProfileProvider** classe, você deve implementar os membros necessários do **SettingsProvider** e  **ProviderBase** classes.

As tabelas a seguir descrevem as propriedades e métodos que você deve implementar do **ProviderBase**, **SettingsProvider**, e **ProfileProvider** abstrato classes.

### <a name="providerbase-members"></a>Membros ProviderBase

| **Membro** | **Descrição** |
| --- | --- |
| Método Initialize | Aceita como entrada o nome da instância do provedor e NameValueCollection de definições de configuração. Usado para definir opções e os valores de propriedade para a instância do provedor, incluindo valores específicos de implementação e opções especificadas na configuração da máquina ou no arquivo Web. config. |

### <a name="settingsprovider-members"></a>Membros SettingsProvider

| **Membro** | **Descrição** |
| --- | --- |
| Propriedade ApplicationName | O nome do aplicativo que é armazenado com cada perfil. O provedor de perfil usa o nome do aplicativo para armazenar informações de perfil separadamente para cada aplicativo. Isso permite que vários aplicativos ASP.NET usar a mesma fonte de dados sem um conflito se o mesmo nome de usuário for criado em diferentes aplicativos. Como alternativa, vários aplicativos ASP.NET podem compartilhar uma fonte de dados de perfil, especificando o mesmo nome do aplicativo. |
| Método GetPropertyValues | Aceita como entrada um SettingsContext e um objeto SettingsPropertyCollection. O **SettingsContext** fornece informações sobre o usuário. Você pode usar as informações como uma chave primária para recuperar informações de propriedade de perfil do usuário. Use o **SettingsContext** objeto para obter o nome de usuário e se o usuário é autenticado ou anônimo. O **SettingsPropertyCollection** contém uma coleção de objetos SettingsProperty. Cada **SettingsProperty** objeto fornece o nome e o tipo de propriedade, bem como informações adicionais como o valor padrão para a propriedade e se a propriedade é somente leitura. O **GetPropertyValues** método popula um SettingsPropertyValueCollection com SettingsPropertyValue objetos com base no **SettingsProperty** objetos fornecidos como entrada. Os valores da fonte de dados para o usuário especificado são atribuídos às propriedades PropertyValue para cada **SettingsPropertyValue** objeto e toda a coleção é retornado. Também é chamar o método atualiza o valor de LastActivityDate para o perfil de usuário especificada para a data e hora atuais. |
| Método SetPropertyValues | Aceita como entrada uma **SettingsContext** e um **SettingsPropertyValueCollection** objeto. O **SettingsContext** fornece informações sobre o usuário. Você pode usar as informações como uma chave primária para recuperar informações de propriedade de perfil do usuário. Use o **SettingsContext** objeto para obter o nome de usuário e se o usuário é autenticado ou anônimo. O **SettingsPropertyValueCollection** contém uma coleção de **SettingsPropertyValue** objetos. Cada **SettingsPropertyValue** objeto fornece o nome, tipo e valor da propriedade, bem como informações adicionais como o valor padrão para a propriedade e se a propriedade é somente leitura. O **SetPropertyValues** método atualiza os valores de propriedade de perfil na fonte de dados para o usuário especificado. Chamar o método também atualizações do **LastActivityDate** e LastUpdatedDate valores para o perfil de usuário especificada para a data e hora atuais. |

### <a name="profileprovider-members"></a>Membros ProfileProvider

| **Membro** | **Descrição** |
| --- | --- |
| Método DeleteProfiles | Aceita como entrada uma matriz de cadeia de caracteres do usuário, nomes e exclui da fonte de dados todos os valores de propriedade e informações de perfil para os nomes especificados em que o nome do aplicativo corresponde a **ApplicationName** valor da propriedade. Se sua fonte de dados oferece suporte a transações, é recomendável que você inclua todas as operações de exclusão em uma transação e que você reverte a transação e lançar uma exceção se qualquer operação de exclusão falhar. |
| Método DeleteProfiles | Aceita como entrada uma coleção de ProfileInfo objetos e exclui da fonte de dados todos os valores de propriedade e informações de perfil para cada perfil em que o nome do aplicativo corresponde a **ApplicationName** valor da propriedade. Se sua fonte de dados oferece suporte a transações, é recomendável que você inclua todas as operações de exclusão em uma transação e reverte a transação e lançar uma exceção se qualquer operação de exclusão falhar. |
| Método DeleteInactiveProfiles | Utiliza como entrada um valor de ProfileAuthenticationOption e todas as informações de perfil de origem de um objeto DateTime e exclusões de dados e valores de propriedade onde a data da última atividade é menor ou igual de data e hora especificadas e o nome do aplicativo corresponde a **ApplicationName** valor da propriedade. O **ProfileAuthenticationOption** parâmetro especifica se somente perfis anônimos, apenas perfis autenticados, ou todos os perfis serão excluídos. Se sua fonte de dados oferece suporte a transações, é recomendável que você inclua todas as operações de exclusão em uma transação e reverte a transação e lançar uma exceção se qualquer operação de exclusão falhar. |
| Método GetAllProfiles | Aceita como entrada uma **ProfileAuthenticationOption** valor, um inteiro que especifica o índice de página, um número inteiro que especifica o tamanho da página e uma referência a um número inteiro que será definida como a contagem total de perfis. Retorna um ProfileInfoCollection que contém **ProfileInfo** objetos para todos os perfis na fonte de dados onde o nome do aplicativo corresponde a **ApplicationName** valor da propriedade. O **ProfileAuthenticationOption** parâmetro especifica se somente perfis anônimos, apenas perfis autenticados, ou todos os perfis serão retornados. Os resultados retornados pelo **GetAllProfiles** método são restritas pelo índice da página e os valores de tamanho de página. O valor de tamanho de página especifica o número máximo de **ProfileInfo** objetos para retornar o **ProfileInfoCollection**. O valor de índice da página especifica qual página de resultados para retornar, onde 1 identifica a primeira página. O parâmetro de total de registros é um parâmetro de saída (você pode usar **ByRef** no Visual Basic) que é definido como o número total de perfis. Por exemplo, se o repositório de dados contém 13 perfis para o aplicativo e o valor de índice de página é 2 com um tamanho de página de 5, o **ProfileInfoCollection** retornado contém o perfis sexto até os décimo. O valor total de registros é definido como 13 quando o método retorna. |
| Método GetAllInactiveProfiles | Aceita como entrada uma **ProfileAuthenticationOption** valor, uma **DateTime** objeto, um inteiro que especifica o índice de página, um número inteiro que especifica o tamanho da página e uma referência a um número inteiro que será definida como a contagem total de perfis. Retorna um **ProfileInfoCollection** que contém **ProfileInfo** objetos para todos os perfis na fonte de dados onde a data da última atividade é menor ou igual a especificado **DateTime**  e onde o nome do aplicativo corresponde a **ApplicationName** valor da propriedade. O **ProfileAuthenticationOption** parâmetro especifica se somente perfis anônimos, apenas perfis autenticados, ou todos os perfis serão retornados. Os resultados retornados pelo **GetAllInactiveProfiles** método são restritas pelo índice da página e os valores de tamanho de página. O valor de tamanho de página especifica o número máximo de **ProfileInfo** objetos para retornar o **ProfileInfoCollection**. O valor de índice da página especifica qual página de resultados para retornar, onde 1 identifica a primeira página. O parâmetro de total de registros é um parâmetro de saída (você pode usar **ByRef** no Visual Basic) que é definido como o número total de perfis. Por exemplo, se o repositório de dados contém 13 perfis para o aplicativo e o valor de índice de página é 2 com um tamanho de página de 5, o **ProfileInfoCollection** retornado contém o perfis sexto até os décimo. O valor total de registros é definido como 13 quando o método retorna. |
| Método FindProfilesByUserName | Aceita como entrada uma **ProfileAuthenticationOption** valor, uma cadeia de caracteres que contém um nome de usuário, um número inteiro que especifica o índice de página, um número inteiro que especifica o tamanho da página e uma referência a um número inteiro que será definida como a contagem total de perfis. Retorna um **ProfileInfoCollection** que contém **ProfileInfo** objetos para todos os perfis de dados de origem onde o nome de usuário corresponde ao nome de usuário especificado e onde o nome do aplicativo corresponde a **ApplicationName** valor da propriedade. O **ProfileAuthenticationOption** parâmetro especifica se somente perfis anônimos, apenas perfis autenticados, ou todos os perfis serão retornados. Se sua fonte de dados oferece suporte a recursos de pesquisa adicionais, como caracteres curinga, você pode fornecer recursos de pesquisa mais abrangentes para nomes de usuário. Os resultados retornados pelo **FindProfilesByUserName** método são restritas pelo índice da página e os valores de tamanho de página. O valor de tamanho de página especifica o número máximo de **ProfileInfo** objetos para retornar o **ProfileInfoCollection**. O valor de índice da página especifica qual página de resultados para retornar, onde 1 identifica a primeira página. O parâmetro de total de registros é um parâmetro de saída (você pode usar **ByRef** no Visual Basic) que é definido como o número total de perfis. Por exemplo, se o repositório de dados contém 13 perfis para o aplicativo e o valor de índice de página é 2 com um tamanho de página de 5, o **ProfileInfoCollection** retornado contém o perfis sexto até os décimo. O valor total de registros é definido como 13 quando o método retorna. |
| Método FindInactiveProfilesByUserName | Aceita como entrada uma **ProfileAuthenticationOption** valor, uma cadeia de caracteres que contém um nome de usuário, uma **DateTime** objeto, um inteiro que especifica o índice de página, um número inteiro que especifica o tamanho da página e um referência a um número inteiro que será definido como a contagem total de perfis. Retorna um **ProfileInfoCollection** que contém **ProfileInfo** objetos para todos os perfis na fonte de dados onde o nome de usuário corresponde ao nome de usuário especificado, onde a data da última atividade é menor que ou igual ao especificado **DateTime**, e onde o nome do aplicativo corresponde a **ApplicationName** valor da propriedade. O **ProfileAuthenticationOption** parâmetro especifica se somente perfis anônimos, apenas perfis autenticados, ou todos os perfis serão retornados. Se sua fonte de dados oferece suporte a recursos de pesquisa adicionais, como caracteres curinga, você pode fornecer recursos de pesquisa mais abrangentes para nomes de usuário. Os resultados retornados pelo **FindInactiveProfilesByUserName** método são restritas pelo índice da página e os valores de tamanho de página. O valor de tamanho de página especifica o número máximo de **ProfileInfo** objetos para retornar o **ProfileInfoCollection**. O valor de índice da página especifica qual página de resultados para retornar, onde 1 identifica a primeira página. O parâmetro de total de registros é um parâmetro de saída (você pode usar **ByRef** no Visual Basic) que é definido como o número total de perfis. Por exemplo, se o repositório de dados contém 13 perfis para o aplicativo e o valor de índice de página é 2 com um tamanho de página de 5, o **ProfileInfoCollection** retornado contém o perfis sexto até os décimo. O valor total de registros é definido como 13 quando o método retorna. |
| Método GetNumberOfInActiveProfiles | Aceita como entrada uma **ProfileAuthenticationOption** valor e um **DateTime** do objeto e retorna uma contagem de todos os perfis na fonte de dados onde a data da última atividade é menor ou igual ao especificado **DateTime** e onde o nome do aplicativo corresponde a **ApplicationName** valor da propriedade. O **ProfileAuthenticationOption** parâmetro especifica se somente perfis anônimos, apenas perfis autenticados, ou todos os perfis serão contados. |

### <a name="applicationname"></a>ApplicationName

Como provedores de perfil armazenam informações de perfil separadamente para cada aplicativo, você deve garantir que o esquema de dados inclua o nome do aplicativo e que as consultas e atualizações também incluam o nome do aplicativo. Por exemplo, o comando a seguir é usado para recuperar um valor de propriedade de um banco de dados com base no nome de usuário e se o perfil é anônimo e garante que o **ApplicationName** valor é incluído na consulta.

[!code-sql[Main](profiles-themes-and-web-parts/samples/sample10.sql)]

## <a name="aspnet-themes"></a>Temas do ASP.NET

## <a name="what-are-aspnet-20-themes"></a>Quais são os temas do ASP.NET 2.0?

Um dos aspectos mais importantes de um aplicativo Web é uma aparência consistente em todo o site. Geralmente, os desenvolvedores do ASP.NET 1. x usam folhas de estilo em cascata (CSS) para implementar uma aparência consistente. O ASP.NET 2.0 temas melhorar significativamente no CSS porque eles oferecem ao desenvolvedor ASP.NET a capacidade de definir a aparência de controles de servidor ASP.NET, bem como elementos HTML. Temas do ASP.NET podem ser aplicados para os controles individuais, uma página da Web específica ou um aplicativo Web inteiro. Temas usam uma combinação de arquivos CSS, um arquivo de capa opcional e uma pasta de imagens opcional se houver necessidade de imagens. O arquivo de capa controla a aparência visual de controles de servidor ASP.NET.

## <a name="where-are-themes-stored"></a>Onde estão os temas armazenados?

O local de armazenamento de temas varia com base em seu escopo. Temas que podem ser aplicados a qualquer aplicativo são armazenados na seguinte pasta:

`C:\WINDOWS\Microsoft.NET\Framework\v2.x.xxxxx\ASP.NETClientFiles\Themes\<Theme_Name>`

Um tema que é específico para um determinado aplicativo é armazenado em um `App\_Themes\<Theme\_Name>` diretório na raiz do site da Web.

> [!NOTE]
> Um arquivo de capa só deve modificar as propriedades de controle de servidor que afetam a aparência.

Um tema global é um tema que pode ser aplicado a qualquer aplicativo ou site da Web em execução no servidor Web. Esses temas são armazenados por padrão no diretório ASP.NETClientfiles\Themes dentro do diretório v2.x.xxxxx. Como alternativa, você pode mover os arquivos de tema para o aspnet\_sistema do cliente\_web / /Themes/ [versão] [tema\_nome] pasta na raiz do seu site da Web.

Temas específicos do aplicativo só podem ser aplicados para o aplicativo no qual residem os arquivos. Esses arquivos são armazenados no `App\_Themes/<theme\_name>` diretório na raiz do site da Web.

## <a name="the-components-of-a-theme"></a>Os componentes de um tema

Um tema é composto de um ou mais arquivos CSS, um arquivo de capa opcional e uma pasta de imagens opcional. Os arquivos CSS podem ser qualquer nome que você deseja (por exemplo, Default ou theme.css, etc.) e deve estar na raiz da pasta de temas. Os arquivos CSS que são usados para definir classes CSS comuns e os atributos de seletores específicos. Para aplicar uma das classes CSS para um elemento de página, o **CSSClass** propriedade é usada.

O arquivo de capa é um arquivo XML que contém definições de propriedade para controles de servidor ASP.NET. O código abaixo é um exemplo de arquivo de capa.

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample11.aspx)]

**Figura 1** abaixo mostra uma página ASP.NET pequena procurados sem um tema. **Figura 2** mostra o mesmo arquivo com um tema. As cores de plano de fundo e texto são configuradas por meio de um arquivo CSS. A aparência do botão e caixa de texto são configurados usando o arquivo de capa listado acima.


![Nenhum tema](profiles-themes-and-web-parts/_static/image1.gif)

**Figura 1**: nenhum tema


![Tema aplicado](profiles-themes-and-web-parts/_static/image2.gif)

**Figura 2**: tema


O arquivo de capa listado acima define uma aparência padrão para todos os controles de caixa de texto e controles de botão. Que significa que cada controle TextBox e inseridos em uma página de controle de botão entrarão nessa aparência. Você também pode definir uma aparência que pode ser aplicada a instâncias específicas desses controles usando a **SkinID** propriedade do controle.

O código a seguir define uma aparência para um controle de botão. Controles de botão somente com um **SkinID** propriedade **goButton** terá a aparência da capa.

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample12.aspx)]

Você só pode ter uma aparência padrão por tipo de controle de servidor. Se você precisar de capas adicionais, você deve usar a propriedade SkinID.

## <a name="applying-themes-to-pages"></a>Aplicar temas a páginas

Um tema pode ser aplicado usando qualquer um dos seguintes métodos:

- No &lt;páginas&gt; elemento do arquivo Web. config
- No @Page diretiva de uma página
- Por meio de programação

## <a name="applying-a-theme-in-the-configuration-file"></a>Aplicar um tema no arquivo de configuração

Para aplicar um tema no arquivo de configuração do aplicativo, use a seguinte sintaxe:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample13.xml)]

O nome do tema especificado aqui deve corresponder ao nome da pasta de temas. Essa pasta pode existir em qualquer um dos locais mencionados anteriormente no curso. Se você tentar aplicar um tema que não exista, ocorrerá um erro de configuração.

## <a name="applying-a-theme-in-the-page-directive"></a>Aplicar um tema na diretiva de página

Você também pode aplicar um tema na diretiva @ Page. Esse método permite que você use um tema para uma página específica.

Para aplicar um tema a @Page diretiva, use a seguinte sintaxe:

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample14.aspx)]

Novamente, o tema especificado aqui deve corresponder a pasta de tema, conforme mencionado anteriormente. Se você tentar aplicar um tema que não exista, ocorrerá uma falha de compilação. O Visual Studio também realçar o atributo e notificá-lo a existência de nenhum esse tema.

## <a name="applying-a-theme-programmatically"></a>Aplicar um tema programaticamente

Para aplicar um tema programaticamente, você deve especificar o **tema** propriedade para a página de **página\_PreInit** método.

Para aplicar um tema programaticamente, use a seguinte sintaxe:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample15.cs)]

É necessário aplicar o tema no método PreInit devido ao ciclo de vida da página. Se você aplicá-lo a partir desse ponto, o tema de páginas será já foram aplicado pelo tempo de execução e uma alteração nesse momento é muito tarde no ciclo de vida. Se você aplicar um tema que não existe, uma **HttpException** ocorre. Quando um tema é aplicado por meio de programação, um aviso de compilação ocorrerá se os controles de servidor tem uma propriedade de SkinID especificada. Esse aviso destina-se para informar que nenhum tema declarativamente é aplicado, e pode ser ignorado.

## <a name="exercise-1--applying-a-theme"></a>Exercício 1: Aplicar um tema

Neste exercício, você aplicará um tema ASP.NET para um site da Web.

> [!IMPORTANT]
> Se você estiver usando o Microsoft Word para inserir informações em um arquivo de capa, certifique-se de que você não estiver substituindo normais com aspas inteligentes. Aspas inglesas irá causar problemas com arquivos de capa.

1. Crie um novo site da Web do ASP.NET.
2. Clique com botão direito no projeto no Gerenciador de soluções e escolha Adicionar Novo Item.
3. Escolha o arquivo de configuração da Web da lista de arquivos e clique em Adicionar.
4. Clique com botão direito no projeto no Gerenciador de soluções e escolha Adicionar Novo Item.
5. Escolha o arquivo de capa e clique em Adicionar.
6. Clique em Sim quando perguntado se youd colocar o arquivo dentro do aplicativo\_pasta temas.
7. Com o botão direito na pasta SkinFile dentro do aplicativo\_pasta temas no Gerenciador de soluções e escolha Adicionar Novo Item.
8. Escolha a folha de estilo da lista de arquivos e clique em Adicionar. Agora você tem todos os arquivos necessários para implementar o novo tema. No entanto, o Visual Studio atribuiu sua pasta de temas SkinFile. Com o botão direito na pasta e altere o nome para CoolTheme.
9. Abra o arquivo SkinFile.skin e adicione o seguinte código final do arquivo: 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample16.aspx)]
10. Salve o arquivo SkinFile.skin.
11. Abra o stylesheet.
12. Substitua todo o texto com o seguinte: 

    [!code-css[Main](profiles-themes-and-web-parts/samples/sample17.css)]
13. Salve o arquivo stylesheet.
14. Abra a página Default.aspx.
15. Adicione um controle de caixa de texto e um controle de botão.
16. Salve a página. Agora, procure a página Default.aspx. Deve ser exibida como um formulário da Web normal.
17. Abra o arquivo Web. config.
18. Adicione o seguinte diretamente sob a abertura `<system.web>` marca: 

    [!code-xml[Main](profiles-themes-and-web-parts/samples/sample18.xml)]
19. Salve o arquivo Web. config. Agora, procure a página Default.aspx. Ele deve exibir com o tema aplicado.
20. Se ainda não estiver aberto, abra a página Default.aspx no Visual Studio.
21. Selecione o botão.
22. Alterar o **SkinID** propriedade goButton. Observe que o Visual Studio fornece uma lista suspensa com SkinID os valores válidos para um controle de botão.
23. Salve a página. Agora, visualize a página em seu navegador novamente. O botão deve agora dizer "go" e deve ser maior aparência.

Usando o **SkinID** propriedade, você pode facilmente configurar aparências diferentes para instâncias diferentes de um determinado tipo de controle de servidor.

## <a name="the-stylesheettheme-property"></a>A propriedade StyleSheetTheme

Até agora, falamos apenas sobre como aplicar temas usando a propriedade de tema. Ao usar a propriedade de tema, o arquivo de capa substituirão qualquer configuração declarativa para controles de servidor. Por exemplo, no Exercício 1, especificado um SkinID da "goButton" para o controle de botão e que alterar o texto do botão "Ir". Você pode ter observado que a propriedade de texto do botão no designer foi definida como "Button", mas o tema substitui que. O tema sempre substituirão as configurações de propriedade no designer.

Se você deseja ser capaz de substituir as propriedades definidas no arquivo de capa do tema com propriedades especificadas no designer, você pode usar o **StyleSheetTheme** propriedade em vez da propriedade de tema. A propriedade StyleSheetTheme é o mesmo que a propriedade de tema, exceto que não substitui todas as configurações de propriedade explícita como propriedade do tema.

Para ver isso em ação, abra o arquivo Web. config do projeto no Exercício 1 e altere o `<pages>` elemento para o seguinte:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample19.xml)]

Agora, procurar a página Default.aspx e você verá que o controle de botão tem uma propriedade de texto de "Button" novamente. Isso ocorre porque a configuração da propriedade explícita no designer está substituindo a propriedade de texto definida pelo goButton SkinID.

## <a name="overriding-themes"></a>Substituindo temas

Um tema global pode ser substituído pela aplicação de um tema com o mesmo nome no aplicativo\_pasta temas do aplicativo. No entanto, o tema não é aplicado em um cenário de substituição verdadeiro. Se o tempo de execução encontrar arquivos de tema no aplicativo\_pasta temas, ele aplicará o tema usando esses arquivos e ignorará o tema global.

A propriedade StyleSheetTheme é substituível e pode ser substituída no código da seguinte maneira:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample20.cs)]

## <a name="web-parts"></a>Web Parts

Web Parts do ASP.NET é um conjunto integrado de controles para criar sites da Web que permitem aos usuários finais modificar o conteúdo, a aparência e o comportamento das páginas da Web diretamente de um navegador. As modificações podem ser aplicadas a todos os usuários no site ou a usuários individuais. Quando os usuários modificam páginas e controles, as configurações podem ser salvas para reter preferências pessoais do usuário entre sessões futuras do navegador, um recurso chamado de personalização. Esses recursos de Web Parts significam que os desenvolvedores podem habilitar usuários finais a personalizar um aplicativo da Web dinamicamente, sem a intervenção do desenvolvedor ou administrador.

Usando o conjunto de controles de Web Parts, você como um desenvolvedor pode habilitar os usuários finais:

- Personalize o conteúdo da página. Os usuários podem adicionar novos controles de Web Parts a uma página, removê-los, ocultá-los ou minimizá-los como janelas comuns.
- Personalize o layout de página. Usuários podem arrastar um controle de Web Parts para uma zona diferente em uma página ou alterar sua aparência, propriedades e comportamento.
- Exportar e importar controles. Os usuários podem importar ou exportar configurações de controle de Web Parts para uso em outras páginas ou sites, mantendo as propriedades de aparência e até mesmo os dados em controles. Isso reduz as demandas de entrada e a configuração de dados sobre os usuários finais.
- Crie conexões. Os usuários podem estabelecer conexões entre os controles para que, por exemplo, um controle de gráfico pode exibir um gráfico para os dados em um controle de cotações da bolsa. Os usuários podem personalizar não apenas a conexão em si, mas a aparência e detalhes de como o controle de gráfico exibe os dados.
- Gerenciar e personalizar configurações no nível do site. Os usuários autorizados podem definir configurações no nível do site, determinam quem pode acessar um site ou uma página, defina o acesso baseado em função para controles e assim por diante. Por exemplo, um usuário em uma função administrativa foi definido um controle de Web Parts para ser compartilhado por todos os usuários e impedir que os usuários que não são administradores de personalizar o controle compartilhado.

Você normalmente trabalha com Web Parts de uma destas três maneiras: Criando páginas que usam controles de Web Parts, criação de controles de Web Parts individuais ou criando aplicativos da Web completos e personalizáveis, como um portal.

## <a name="page-development"></a>Desenvolvimento de página

Os desenvolvedores de página podem usar ferramentas de design visual, como Microsoft Visual Studio 2005 para criar páginas que usam Web Parts. Uma vantagem em usar uma ferramenta como o Visual Studio é o conjunto de controles de Web Parts fornece recursos para criação de arrastar e soltar e configuração de controles de Web Parts em um designer visual. Por exemplo, você pode usar o designer para arrastar uma zona de Web Parts, ou um controle de editor de Web Parts, na superfície de design e, em seguida, configurar o controle à direita no designer usando a interface do usuário fornecida pelo Web Parts conjunto de controles. Isso pode agilizar o desenvolvimento de aplicativos Web Parts e reduzir a quantidade de código que você precisa escrever.

## <a name="control-development"></a>Desenvolvimento de controle

Você pode usar qualquer controle ASP.NET existente como um controle de Web Parts, incluindo controles padrão de servidor Web, controles personalizados de servidor e controles de usuário. Para máximo controle programático de seu ambiente, você também pode criar controles personalizados de Web Parts que derivam da classe de Web Part. Para o desenvolvimento de controle de Web Parts individual, você será normalmente crie um controle de usuário e usá-lo como um controle de Web Parts ou desenvolver um controle de Web Parts personalizado.

Como um exemplo de desenvolvimento de um controle de Web Parts personalizado, você pode criar um controle para fornecer qualquer um dos recursos fornecidos por outros controles de servidor ASP.NET que podem ser úteis ao pacote como um controle de Web Parts personalizável: calendários, listas, informações financeiras, notícias, calculadoras, controles de rich text para atualizar a grade editável de conteúdo que se conectam a bancos de dados, gráficos que dinamicamente atualizar suas exibições, ou tempo e informações de viagem. Se você fornecer um designer visual com seu controle, em seguida, qualquer desenvolvedor de página usando o Visual Studio pode simplesmente arrastar o controle para uma zona de Web Parts e configurá-lo em tempo de design sem precisar gravar código adicional.

Personalização é a base do recurso Web Parts. Ele permite que os usuários modifiquem ou personalizem o layout, a aparência e o comportamento de controles de Web Parts em uma página. As configurações personalizadas são vida longa: elas são mantidas não apenas durante a sessão atual do navegador (como com estado de exibição), mas também em armazenamento de longo prazo, para que as configurações do usuário são salvas para sessões futuras do navegador. Personalização é ativada por padrão para páginas de Web Parts.

Os componentes estruturais de interface do usuário dependem de personalização e fornecem a estrutura principal e os serviços necessários por todos os controles de Web Parts. Um componente estrutural de interface do usuário necessário em cada página de Web Parts é o controle em WebPartManager. Embora nunca visível, este controle tem a tarefa crítica de coordenar todos os controles de Web Parts em uma página. Por exemplo, ele controla todos os controles de Web Parts individuais. Ele gerencia zonas de Web Parts (regiões que contêm controles de Web Parts em uma página) e quais controles estão em quais regiões. Ele também rastreia e controla os modos de exibição diferentes que uma página pode estar, tais como procurar, conectar, editar ou modo de catálogo e se as alterações de personalização se aplicam a todos os usuários ou a usuários individuais. Finalmente, ele inicia e controla as conexões e comunicação entre os controles de Web Parts.

O segundo tipo de componente estrutural de interface do usuário é a zona. Zonas atuam como gerentes de layout em uma página de Web Parts. Eles contêm e organizam os controles que derivam da classe parte (controles parciais) e fornecem a capacidade de fazer o layout de página modular na orientação horizontal ou vertical. Zonas também oferecem comuns e consistentes elementos de interface do usuário (como o estilo de cabeçalho e rodapé, título, estilo de borda, botões de ação e assim por diante) para cada controle que eles contêm; Esses elementos comuns são conhecidos como cromo de um controle. Vários tipos especializados de zonas são usados nos modos de exibição diferentes e com vários controles. Os tipos diferentes de zonas estão descritos na seção de Web Parts Essential Controls abaixo.

Os controles de Web Parts de UI, todos os que derivam de **parte** classe, compõem interface primária em uma página de Web Parts. O conjunto de controles de Web Parts é flexível e inclusive nas opções de assim como para a criação de controles de parte. Além de criar seus próprios controles personalizados de Web Parts, você também pode usar controles de servidor ASP.NET existentes, controles de usuário ou controles personalizados de servidor como controles Web Parts. Os controles essenciais que são mais comumente usados para criar páginas de Web Parts são descritos na próxima seção.

## <a name="web-parts-essential-controls"></a>Controles essenciais Web Parts

O conjunto de controles de Web Parts é abrangente, mas alguns controles são essenciais porque eles são necessários para Web Parts funcionem ou porque eles são os controles usados com mais frequência nas páginas de Web Parts. Como começar a usar o Web Parts e criar páginas de Web Parts básicas, é útil estar familiarizado com os controles de Web Parts essenciais descritos na tabela a seguir.

| **Controlam de Web Parts** | **Descrição** |
| --- | --- |
| WebPartManager | Gerencia todos os controles de Web Parts em uma página. Um (e apenas um) **WebPartManager** controle é necessário para cada página de Web Parts. |
| CatalogZone | Contém controles CatalogPart. Use esta zona para criar um catálogo de controles de Web Parts do qual os usuários podem selecionar controles para adicionar a uma página. |
| EditorZone | Contém os controles de EditorPart. Use esta zona para permitir que os usuários editem e personalizem controles de Web Parts em uma página. |
| WebPartZone | Contém e fornece o layout geral para os controles de Web Part que compõem a interface do usuário principal de uma página. Use esta zona sempre que você criar páginas com controles de Web Parts. Páginas podem conter uma ou mais zonas. |
| ConnectionsZone | Contém controles WebPartConnection e fornece uma interface do usuário para gerenciar as conexões. |
| WebPart (GenericWebPart) | Renderiza a interface do usuário principal; a maioria dos controles de Web Parts de UI entram nessa categoria. Para máximo controle programático, você pode criar controles personalizados de Web Parts que derivam da base de **WebPart** controle. Você também pode usar controles de servidor existentes, controles de usuário ou controles personalizados, como controles Web Parts. Sempre que qualquer desses controles são colocados em uma zona, o **WebPartManager** controle automaticamente quebra-o com **GenericWebPart** controles em tempo de execução para que você pode usá-los com a funcionalidade de Web Parts. |
| CatalogPart | Contém uma lista de controles de Web Parts disponíveis que os usuários podem adicionar à página. |
| WebPartConnection | Cria uma conexão entre dois controles de Web Parts em uma página. A conexão define um dos controles de Web Parts como um provedor (de dados) e o outro como um consumidor. |
| EditorPart | Serve como a classe base para os controles de editor especializados. |
| Controles de EditorPart (AppearanceEditorPart LayoutEditorPart, BehaviorEditorPart e PropertyGridEditorPart) | Permitir que usuários personalizem vários aspectos dos controles de Web Parts de UI em uma página |

## <a name="lab-create-a-web-part-page"></a>Laboratório: Criar uma página de Web Part

Neste laboratório, você criará uma página de Web part que persistirá informações por meio de perfis do ASP.NET.

### <a name="creating-a-simple-page-with-web-parts"></a>Criando uma página Simple com Web Parts

Nesta parte do passo a passo, você deve criar uma página que usa os controles de Web Parts para mostrar conteúdo estático. É a primeira etapa ao trabalhar com Web Parts criar uma página com dois elementos estruturais necessários. Primeiro, uma página Web parts precisa de um controle WebPartManager para controlar e coordenar todos os controles de Web Parts. Segundo, uma página de Web Parts precisa de uma ou mais zonas, que são controles compostos que contêm WebPart ou outros controles de servidor e ocupam uma região especificada de uma página.

> [!NOTE]
> Você não precisa fazer nada para permitir personalização de Web Parts; ele é habilitado por padrão para o conjunto de controles de Web Parts. Quando você executa pela primeira vez uma página de Web Parts em um site, o ASP.NET configura um provedor de personalização padrão para armazenar configurações personalizadas do usuário. Para obter mais informações sobre personalização, consulte Visão geral de personalização do Web Parts.


### <a name="to-create-a-page-for-containing-web-parts-controls"></a>Para criar uma página que contém os controles de Web Parts

1. Feche a página padrão e adicione uma nova página ao site, chamada WebPartsDemo.
2. Alternar para **Design** exibição.
3. Do **exibição** menu, verifique se o **controles não visuais** e **detalhes** opções são selecionadas para que você possa ver marcas de layout e controles que não têm uma interface do usuário.
4. Coloque o ponto de inserção antes do `<div>` marcas na superfície de design e pressione ENTER para adicionar uma nova linha. Posicione o ponto de inserção antes do caractere de nova linha, clique no **formato de bloco** lista suspensa menu de controle e selecione o **título 1** opção. No título, adicione o texto **página de Web Parts demonstração**.
5. Do **WebParts** guia da caixa de ferramentas, arraste um **WebPartManager** até a página, posicionando-o logo após o caractere de nova linha e antes do `<div>`marcas.   
  
   O **WebPartManager** controle não gera nenhuma saída, então ele aparece como uma caixa cinza na superfície do designer.
6. Posicione o ponto de inserção dentro de `<div>` marcas.
7. No **Layout** menu, clique em **Inserir tabela**e criar uma nova tabela que tenha uma linha e três colunas. Clique o **propriedades de célula** botão, selecione **superior** do **Alinhamento Vertical** lista suspensa, clique em **Okey**e clique em **Okey** novamente para criar a tabela.
8. Arraste um controle WebPartZone para a coluna de tabela esquerda. Clique com botão direito do **WebPartZone** de controle, escolha **propriedades**e defina as seguintes propriedades:   
  
   ID: SidebarZone   
  
   HeaderText: barra lateral
9. Arraste uma segunda **WebPartZone** controlar para a coluna do meio da tabela e defina as seguintes propriedades:   
  
   ID: MainZone   
  
   HeaderText: principal
10. Salve o arquivo.

Sua página agora tem duas zonas diferentes que você pode controlar separadamente. No entanto, nenhuma zona tem qualquer conteúdo, para que criar conteúdo é a próxima etapa. Para este passo a passo, você trabalha com controles de Web Parts que exibem somente conteúdo estático.

O layout de uma zona de Web Parts é especificado por uma &lt;zonetemplate&gt; elemento. Dentro do modelo de zona, você pode adicionar qualquer controle ASP.NET, se ele é um controle de Web Parts personalizado, um controle de usuário ou um controle de servidor existente. Observe que aqui você está usando o controle de rótulo e ao qual você está simplesmente adicionando texto estático. Quando você coloca um controle de servidor em um **WebPartZone** zona, ASP.NET trata o controle como um controle de Web Parts em tempo de execução, o que habilita recursos de Web Parts no controle.

**Para criar conteúdo para a zona principal**

1. Em **Design** exibir, arraste um **rótulo** controlar do **padrão** guia da caixa de ferramentas para a área de conteúdo da zona cuja **ID** propriedade é definido como MainZone.
2. Alternar para **fonte** exibição. Observe que uma &lt;zonetemplate&gt; elemento foi adicionado para ajustar o **rótulo** controle na MainZone.
3. Adicione um atributo chamado **título** para o &lt;asp: label&gt; elemento e defina seu valor para o conteúdo. Remova o texto = atributo "Rótulo" do &lt;asp: label&gt; elemento. Entre as marcas de abertura e fechamento do &lt;asp: label&gt; elemento, adicione um texto como **bem-vindo à minha Home Page** dentro de um par de &lt;h2&gt; marcas de elemento. O código deve ser da seguinte maneira. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample21.aspx)]
4. Salve o arquivo.

Em seguida, crie um controle de usuário também pode ser adicionado à página como um controle de Web Parts.

### <a name="to-create-a-user-control"></a>Para criar um controle de usuário

1. Adicione um novo controle de usuário da Web ao seu site para servir como um controle de pesquisa. Desmarque a opção de **coloque o código-fonte em um arquivo separado**. Adicione-o no mesmo diretório que a página WebPartsDemo e nomeie-a como SearchUserControl.   
  
    > [!NOTE]
    > O controle de usuário para este passo a passo não implementa a funcionalidade de pesquisa real; ele é usado somente para demonstrar os recursos de Web Parts.
2. Alternar para **Design** exibição. Do **padrão** guia da caixa de ferramentas, arraste um controle de caixa de texto para a página.
3. Coloque o ponto de inserção depois da caixa de texto que você acabou de adicionar e pressione ENTER para adicionar uma nova linha.
4. Arraste um controle de botão para a página na nova linha abaixo da caixa de texto que você acabou de adicionar.
5. Alternar para **fonte** exibição. Certifique-se de que o código-fonte para o controle de usuário parece com o exemplo a seguir. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample22.aspx)]
6. Salve e feche o arquivo.

Agora você pode adicionar controles de Web Parts para a zona de barra lateral. Você está adicionando dois controles à zona Sidebar, um contendo uma lista de links e outro que é o controle de usuário que você criou no procedimento anterior. Os links são adicionados como um padrão **rótulo** controle de servidor, semelhante à forma como você criou o texto estático para a zona principal. No entanto, embora os controles de servidor individuais contidos no controle de usuário pode ser contidos diretamente na zona (como o controle de rótulo), nesse caso eles não são. Em vez disso, eles fazem parte do controle de usuário que você criou no procedimento anterior. Isso demonstra uma maneira comum de empacotar qualquer controle e a funcionalidade adicional que você deseja em um controle de usuário e, em seguida, fazer referência a esse controle em uma zona como um controle de Web Parts.

Em tempo de execução, o conjunto de controles de Web Parts envolve ambos os controles com GenericWebPart. Quando um **GenericWebPart** controle envolve um controle de servidor Web, o controle da parte genérica é o controle pai e você pode acessar o controle de servidor por meio da propriedade ChildControl do controle pai. Esse uso de controles de parte genérica permite controles de servidor Web padrão ter o mesmo comportamento básico e atributos como controles de Web Parts que derivam de **WebPart** classe.

### <a name="to-add-web-parts-controls-to-the-sidebar-zone"></a>Para adicionar controles de Web Parts para a zona sidebar

1. Abra a página WebPartsDemo.
2. Alternar para **Design** exibição.
3. Arraste a usuário controle página você criou, SearchUserControl, de **Solution Explorer** na zona cujo **ID** propriedade é definida como SidebarZone e soltá-lo lá.
4. Salve a página WebPartsDemo.
5. Alternar para **fonte** exibição.
6. Dentro de &lt;asp: webpartzone&gt; elemento para SidebarZone, logo acima de referência para o controle de usuário, adicione um &lt;asp: label&gt; elemento contido links, conforme mostrado no exemplo a seguir. Além disso, adicionar um **título** a marca de controle de usuário, com um valor de atributo **pesquisa**, conforme mostrado. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample23.aspx)]
7. Salve e feche o arquivo.

Agora você pode testar sua página navegando para ele em seu navegador. A página exibe as duas zonas. A captura de tela a seguir mostra a página.

**Página de demonstração de partes da Web com duas zonas**


![Captura de tela de instruções passo a passo 1 Web Parts do VS](profiles-themes-and-web-parts/_static/image3.gif)

**Figura 3**: Web Parts do VS captura de tela de instruções passo a passo 1


No título da barra de cada controle é uma seta para baixo que fornece acesso a um menu de ações disponíveis, que você pode executar em um controle. Clique no menu de verbos para um dos controles, clique o **minimizar** verbo e observe que o controle é minimizado. No menu de verbos, clique em **restauração**, e o controle retorna ao seu tamanho normal.

### <a name="enabling-users-to-edit-pages-and-change-layout"></a>Permite aos usuários editar páginas e alterar o Layout

Web Parts fornece a capacidade dos usuários podem alterar o layout dos controles de Web Parts, arrastando-os de uma zona para outro. Além de permitir que os usuários movam **WebPart** controles de uma zona para outro, você pode permitir que usuários editem várias características dos controles, inclusive sua aparência, o layout e o comportamento. O conjunto de controles de Web Parts fornece a funcionalidade básica para a edição **WebPart** controles. Embora você não faça isso neste passo a passo, você também pode criar controles de edição personalizados que permitem aos usuários editar os recursos de **WebPart** controles. Como alterar a localidade de um **WebPart** controle, editar propriedades de um controle depende da personalização do ASP.NET para salvar as alterações feitas pelos usuários.

Nesta parte do passo a passo, você adicionar a capacidade dos usuários editar as características básicas de qualquer **WebPart** controle na página. Para habilitar esses recursos, você adiciona outro controle de usuário personalizado para a página, junto com um &lt;asp: editorzone&gt; elemento e dois controles de edição.

### <a name="to-create-a-user-control-that-enables-changing-page-layout"></a>Para criar um controle de usuário que permite alterar layout da página

1. No Visual Studio, no **arquivo** menu, selecione o **novo** submenu e clique no **arquivo** opção.
2. No **Adicionar Novo Item** caixa de diálogo, selecione **controle de usuário da Web**. Nomeie o novo arquivo Displaymodemenu. Desmarque a opção de **coloque o código-fonte no arquivo separado**.
3. Clique em Adicionar para criar o novo controle.
4. Alternar para **fonte** exibição.
5. Remova todo o código existente no novo arquivo e, em seguida, cole o código a seguir. Esse código de controle de usuário usa recursos do conjunto de controle de Web Parts que permitem que uma página Alterar o modo de exibição ou modo de exibição e também permite que você altere a aparência física e layout da página enquanto você estiver em certos modos de exibição. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample24.aspx)]
6. Salve o arquivo clicando em Salvar ícone na barra de ferramentas ou selecionando **salvar** no **arquivo** menu.

### <a name="to-enable-users-to-change-the-layout"></a>Para permitir aos usuários alterar o layout

1. Abra a página WebPartsDemo e alternar para **Design** exibição.
2. Posicione o ponto de inserção de **Design** exibir logo após o **WebPartManager** controle que você adicionou anteriormente. Adicionar um retorno de carro depois do texto para que haja uma linha em branco após o **WebPartManager** controle. Coloque o ponto de inserção na linha em branco.
3. Arraste o controle de usuário que você acabou de criar (o arquivo é nomeado DisplayModeMenu) para o WebPartsDemo página e solte-o na linha em branco.
4. Arraste um controle EditorZone do **WebParts** seção da caixa de ferramentas para a célula abertas restantes na página WebPartsDemo.
5. Do **WebParts** seção da caixa de ferramentas, arraste um controle AppearanceEditorPart e um controle LayoutEditorPart para o **EditorZone** controle.
6. Alternar para **fonte** exibição. O código resultante na célula da tabela deve ser semelhante ao seguinte código. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample25.aspx)]
7. Salve o arquivo WebPartsDemo. Você criou um controle de usuário que permite que você altere os modos de exibição e layout de página e você referenciou o controle na página da Web principal.

Agora você pode testar a capacidade de editar páginas e alterar o layout.

### <a name="to-test-layout-changes"></a>Para testar as alterações de layout

1. Carregar a página em um navegador.
2. Clique o **modo de exibição** menu suspenso e selecione **editar**. Os títulos de zona são exibidos.
3. Arraste o **Meus Links** controle por sua barra de título da zona Sidebar para a parte inferior da zona Main. Sua página deve parecer com a captura de tela a seguir.

### <a name="web-parts-demo-page-with-my-links-control-moved"></a>Página de demonstração de partes da Web com o controle My Links movido


![Captura de tela de instruções passo a passo 2 Web Parts do VS](profiles-themes-and-web-parts/_static/image4.gif)

**Figura 4**: Web Parts do VS captura de tela de instruções passo a passo 2


1. Clique o **modo de exibição** menu suspenso e selecione **procurar**. A página for atualizada, os nomes de zona desaparecem e o **Meus Links** controlar permanece onde é posicionado.
2. Para demonstrar que a personalização está funcionando, feche o navegador e, em seguida, carregue a página novamente. As alterações feitas são salvas para sessões futuras do navegador.
3. Do **modo de exibição** menu, selecione **editar**.   
  
   Agora, cada controle na página é exibido com uma seta para baixo na sua barra de título, que contém o menu suspenso de verbos.
4. Clique na seta para exibir o menu de verbos sobre o **Meus Links** controle. Clique o **editar** verbo.   
  
   O **EditorZone** controle aparece, exibindo os controles EditorPart adicionados.
5. No **aparência** seção do controle de edição, altere o **título** em Meus Favoritos, use o **tipo de Cromado** lista suspensa para selecionar **somente título**e, em seguida, clique em **aplicar**. A captura de tela a seguir mostra a página no modo de edição.

### <a name="web-parts-demo-page-in-edit-mode"></a>Página de demonstração de partes da Web no modo de edição


![Captura de tela de instruções passo a passo 3 Web Parts do VS](profiles-themes-and-web-parts/_static/image5.gif)

**Figura 5**: Web Parts do VS captura de tela de instruções passo a passo 3


1. Clique o **modo de exibição** menu e selecione **procurar** para retornar para o modo de procura.
2. O controle agora tem um título atualizado e nenhuma borda, como mostrado na captura de tela a seguir.

### <a name="edited-web-parts-demo-page"></a>Página de Web Parts demonstração editada


![Captura de tela de instruções passo a passo 4 Web Parts do VS](profiles-themes-and-web-parts/_static/image6.gif)

**Figura 4**: Web Parts do VS captura de tela de instruções passo a passo 4


### <a name="adding-web-parts-at-run-time"></a>Adicionar Web Parts em tempo de execução

Você também pode permitir que os usuários a adicionar controles de Web Parts para sua página em tempo de execução. Para fazer isso, configure a página com um catálogo de Web Parts, que contém uma lista de controles de Web Parts que você deseja disponibilizar para os usuários.

**Para permitir que usuários adicionem Web Parts em tempo de execução**

1. Abra a página WebPartsDemo e alternar para **Design** exibição.
2. Do **WebParts** guia da caixa de ferramentas, arraste um controle CatalogZone para a coluna direita da tabela, abaixo de **EditorZone** controle.   
  
   Ambos os controles podem ser na mesma célula da tabela porque eles não serão exibidos ao mesmo tempo.
3. No painel Propriedades, atribua a cadeia de caracteres **adicionar Web Parts** para a propriedade HeaderText do **CatalogZone** controle.
4. Do **WebParts** seção da caixa de ferramentas, arraste um controle DeclarativeCatalogPart para a área de conteúdo do **CatalogZone** controle.
5. Clique na seta no canto superior direito do **DeclarativeCatalogPart** controle para expor seu menu de tarefas e, em seguida, selecione **editar modelos**.
6. Do **padrão** seção da caixa de ferramentas, arraste um **FileUpload** controle e um **calendário** controle para o **WebPartsTemplate** seção de **DeclarativeCatalogPart** controle.
7. Alternar para **fonte** exibição. Inspecione o código-fonte do &lt;asp: catalogzone&gt; elemento. Observe que o **DeclarativeCatalogPart** controle contém um &lt;webpartstemplate&gt; elemento com os dois controles de servidor incluídos que você poderá adicionar à sua página do catálogo.
8. Adicionar um **título** propriedade para cada um dos controles adicionados ao catálogo, usando o valor de cadeia de caracteres mostrado para cada título no exemplo de código abaixo. Embora o título não é uma propriedade você normalmente possa definir esses dois controles de servidor em tempo de design, quando um usuário adiciona esses controles a uma **WebPartZone** zona do catálogo de tempo de execução, eles são empacotados com um  **GenericWebPart** controle. Isso permite que ele atue como controles de Web Parts, portanto eles poderão exibir títulos de.   
  
   O código para os dois controles contidos no **DeclarativeCatalogPart** controle se assemelhar ao seguinte. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample26.aspx)]
9. Salve a página.

Agora você pode testar o catálogo.

### <a name="to-test-the-web-parts-catalog"></a>Para testar o catálogo de Web Parts

1. Carregar a página em um navegador.
2. Clique o **modo de exibição** menu suspenso e selecione **catálogo**.   
  
   O catálogo intitulado **adicionar Web Parts** é exibido.
3. Arraste o **Meus Favoritos** controlar da zona Main voltar ao início da zona Sidebar e soltá-lo lá.
4. No **adicionar Web Parts** do catálogo, selecione ambas as caixas de seleção e, em seguida, selecione **principal** na lista suspensa que contém as zonas disponíveis.
5. Clique em **adicionar** no catálogo. Os controles são adicionados à zona Main. Se desejar, você pode adicionar várias instâncias de controles do catálogo para a página.   
  
   A captura de tela a seguir mostra a página com o controle de carregamento de arquivo e o calendário na zona Main. 

![Controles adicionados à zona Main do catálogo](profiles-themes-and-web-parts/_static/image7.gif)

    **Figure 5**: Controls added to Main zone from the catalog
6. Clique o **modo de exibição** menu suspenso e selecione **procurar**. O catálogo desaparece e a página for atualizada.
7. Feche o navegador. Carregue a página novamente. As alterações feitas persistem.
