---
uid: web-forms/overview/older-versions-security/membership/storing-additional-user-information-vb
title: Armazenar informações adicionais do usuário (VB) | Microsoft Docs
author: rick-anderson
description: Neste tutorial será respondemos a essa pergunta, criando um aplicativo de livro de visitas muito rudimentares. Dessa forma, vamos examinar as diferentes opções para modeli...
ms.author: riande
ms.date: 01/18/2008
ms.assetid: ee4b924e-8002-4dc3-819f-695fca1ff867
msc.legacyurl: /web-forms/overview/older-versions-security/membership/storing-additional-user-information-vb
msc.type: authoredcontent
ms.openlocfilehash: 33e686cc3b977c6c740dfaf1057e1e399d5a298b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830643"
---
<a name="storing-additional-user-information-vb"></a>Armazenar informações adicionais do usuário (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_08_VB.zip) ou [baixar PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial08_ExtraUserInfo_vb.pdf)

> Neste tutorial será respondemos a essa pergunta, criando um aplicativo de livro de visitas muito rudimentares. Dessa forma, podemos examinar opções diferentes para modelagem de informações do usuário em um banco de dados e, em seguida, consulte como associar esses dados com as contas de usuário criadas pela estrutura de associação.


## <a name="introduction"></a>Introdução

ASP. Estrutura de associação da rede oferece uma interface flexível para gerenciar usuários. A API Membership inclui métodos para validar as credenciais, recuperando informações sobre o usuário conectado no momento, criando uma nova conta de usuário e a exclusão de uma conta de usuário, entre outros. Cada conta de usuário na estrutura associação contém apenas as propriedades necessárias para validar as credenciais e executar tarefas relacionadas a contas de usuário essencial. Isso fica evidente através de métodos e propriedades do [ `MembershipUser` classe](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx), que modela uma conta de usuário na estrutura de associação. Essa classe tem propriedades como [ `UserName` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.username.aspx), [ `Email` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.email.aspx), e [ `IsLockedOut` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx), e métodos como [ `GetPassword` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.getpassword.aspx) e [ `UnlockUser` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx).

Muitas vezes, os aplicativos precisam armazenar informações de usuário adicionais não incluídas na estrutura associação. Por exemplo, uma loja online, talvez seja necessário permitir que cada usuário armazenar seus endereços de envio e de cobrança, informações de pagamento, as preferências de entrega e entre em contato com o número de telefone. Além disso, cada pedido no sistema está associado uma conta de usuário específica.

O `MembershipUser` classe não inclui propriedades como `PhoneNumber` ou `DeliveryPreferences` ou `PastOrders`. Então, como podemos acompanhar as informações de usuário necessitadas para o aplicativo e fazer com que ele se integram com a estrutura de associação? Neste tutorial será respondemos a essa pergunta, criando um aplicativo de livro de visitas muito rudimentares. Dessa forma, podemos examinar opções diferentes para modelagem de informações do usuário em um banco de dados e, em seguida, consulte como associar esses dados com as contas de usuário criadas pela estrutura de associação. Vamos começar!

## <a name="step-1-creating-the-guestbook-applications-data-model"></a>Etapa 1: Criando o modelo de dados do aplicativo de livro de visitas

Há uma variedade de técnicas que podem ser empregadas para capturar informações de usuário em um banco de dados e associá-la com as contas de usuário criadas pela estrutura de associação. Para ilustrar essas técnicas, é necessário aumentar o aplicativo web do tutorial, de modo que ele captura algum tipo de dados relacionados ao usuário. (No momento, o modelo de dados do aplicativo contém apenas o aplicativo de serviços tabelas necessárias para o `SqlMembershipProvider`.)

Vamos criar um aplicativo de livro de visitas muito simples em que um usuário autenticado pode deixar um comentário. Além de armazenar comentários de livro de visitas, vamos permitir que cada usuário armazenar sua cidade, a home page e a assinatura. Se fornecido, cidade do usuário, home page e a assinatura serão exibido em cada mensagem que ele saiu no livro de visitas.

### <a name="adding-theguestbookcommentstable"></a>Adicionando o`GuestbookComments`tabela

Para capturar os comentários do livro de visitas, precisamos criar uma tabela de banco de dados denominada `GuestbookComments` que tem colunas como `CommentId`, `Subject`, `Body`, e `CommentDate`. Também precisamos ter cada registro no `GuestbookComments` o usuário que deixou o comentário de referência de tabela.

Para adicionar essa tabela para nosso banco de dados, vá para o Gerenciador de banco de dados no Visual Studio e Detalhar o `SecurityTutorials` banco de dados. Clique com botão direito na pasta tabelas e escolha Adicionar nova tabela. Isso abre uma interface que permite definir as colunas para a nova tabela.


[![Adicionar uma nova tabela no banco de dados SecurityTutorials](storing-additional-user-information-vb/_static/image2.png)](storing-additional-user-information-vb/_static/image1.png)

**Figura 1**: adicionar uma nova tabela para o `SecurityTutorials` banco de dados ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image3.png))


Em seguida, defina o `GuestbookComments`da colunas. Comece adicionando uma coluna denominada `CommentId` do tipo `uniqueidentifier`. Esta coluna será identificam exclusivamente cada comentário no livro de visitas, portanto, não permitir `NULL` s e marcá-la como chave primária da tabela. Em vez de fornecer um valor para o `CommentId` campo em cada `INSERT`, podemos pode indicar que uma nova `uniqueidentifier` valor deve ser gerado automaticamente para esse campo em `INSERT` definindo o valor padrão da coluna como `NEWID()`. Depois de adicionar esse campo primeiro, marcando-o como a chave primária e as configurações de seu valor padrão, sua tela deve ser semelhante à mostrada na Figura 2 de captura de tela.


[![Adicionar uma coluna principal chamada CommentId](storing-additional-user-information-vb/_static/image5.png)](storing-additional-user-information-vb/_static/image4.png)

**Figura 2**: adicionar uma coluna denominada primário `CommentId` ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image6.png))


Em seguida, adicione uma coluna denominada `Subject` do tipo `nvarchar(50)` e uma coluna denominada `Body` do tipo `nvarchar(MAX)`, desativando `NULL` s em ambas as colunas. Depois disso, adicione uma coluna denominada `CommentDate` do tipo `datetime`. Não permitir `NULL` s e defina o `CommentDate` valor de padrão da coluna a ser `getdate()`.

Tudo o que resta é adicionar uma coluna que associa uma conta de usuário a cada comentário do livro de visitas. Uma opção seria adicionar uma coluna denominada `UserName` do tipo `nvarchar(256)`. Isso é uma escolha adequada ao uso de um provedor de associação diferente a `SqlMembershipProvider`. Mas ao usar o `SqlMembershipProvider`, pois estamos nessa série de tutoriais, o `UserName` coluna o `aspnet_Users` tabela não é garantida para ser exclusivo. O `aspnet_Users` é de chave primária da tabela `UserId` e é do tipo `uniqueidentifier`. Portanto, o `GuestbookComments` tabela precisa de uma coluna denominada `UserId` do tipo `uniqueidentifier` (desautorizando `NULL` valores). Vá em frente e adicione esta coluna.

> [!NOTE]
> Como discutimos na [ *criação do esquema de associação no SQL Server* ](creating-the-membership-schema-in-sql-server-vb.md) tutorial, a estrutura de associação foi projetada para permitir vários aplicativos web com contas de usuário diferentes compartilhar o mesmo repositório do usuário. Ele faz isso por meio do particionamento de contas de usuário em aplicativos diferentes. E enquanto cada nome de usuário é garantido que seja exclusivo dentro de um aplicativo, o mesmo nome de usuário pode ser usado em aplicativos diferentes usando o mesmo repositório de usuário. Há uma composição `UNIQUE` restrição na `aspnet_Users` tabela o `UserName` e `ApplicationId` campos, mas não uma em apenas o `UserName` campo. Consequentemente, é possível que o aspnet\_tabela de usuários para ter dois (ou mais) registros com o mesmo `UserName` valor. No entanto, há uma `UNIQUE` restrição na `aspnet_Users` da tabela `UserId` campo (já que é a chave primária). Um `UNIQUE` restrição é importante porque sem ela, não é possível estabelecer uma restrição de chave estrangeira entre as `GuestbookComments` e `aspnet_Users` tabelas.


Depois de adicionar o `UserId` coluna, salve a tabela clicando no ícone de salvar na barra de ferramentas. Nomeie a nova tabela `GuestbookComments`.

Temos uma última questão participar com o `GuestbookComments` tabela: é necessário criar um [restrição de chave estrangeira](https://msdn.microsoft.com/library/ms175464.aspx) entre o `GuestbookComments.UserId` coluna e o `aspnet_Users.UserId` coluna. Para fazer isso, clique no ícone de relação na barra de ferramentas para iniciar a caixa de diálogo relações de chave estrangeira. (Como alternativa, você pode iniciar esta caixa de diálogo, no menu Designer de tabela e escolhendo as relações).

Clique no botão Adicionar no canto inferior esquerdo da caixa de diálogo relações de chave estrangeira. Isso adicionará uma nova restrição foreign key, embora ainda assim será preciso definir as tabelas que participam na relação.


[![Use a caixa de diálogo relações de chave estrangeira para gerenciar as restrições de chave estrangeira da tabela](storing-additional-user-information-vb/_static/image8.png)](storing-additional-user-information-vb/_static/image7.png)

**Figura 3**: Use a caixa de diálogo de relações de chave estrangeira para gerenciar as restrições de chave estrangeira da tabela ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image9.png))


Em seguida, clique no ícone de reticências na linha "Especificações de tabela e colunas" à direita. Isso iniciará a caixa de diálogo tabelas e colunas, dos quais podemos especificar a tabela de chave primária e a coluna e a coluna de chave estrangeira do `GuestbookComments` tabela. Em particular, selecione `aspnet_Users` e `UserId` como a tabela de chave primária e a coluna, e `UserId` da `GuestbookComments` tabela como a coluna de chave estrangeira (consulte a Figura 4). Depois de definir as colunas e tabelas de chave primárias e estrangeiras, clique em Okey para retornar à caixa de diálogo relações de chave estrangeira.


[![Estabeleça uma Foreign Key restrição entre o aspnet_Users e GuesbookComments tabelas](storing-additional-user-information-vb/_static/image11.png)](storing-additional-user-information-vb/_static/image10.png)

**Figura 4**: estabeleça um Foreign Key restrição entre o `aspnet_Users` e `GuesbookComments` tabelas ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image12.png))


Neste momento a restrição de chave estrangeira foi estabelecida. A presença dessa restrição garante [integridade relacional](http://en.wikipedia.org/wiki/Referential_integrity) entre as duas tabelas, garantindo que nunca haverá uma entrada de livro de visitas referindo-se a uma conta de usuário inexistente. Por padrão, uma restrição foreign key não permitirá um registro pai a ser excluído se existem registros filho correspondentes. Ou seja, se um usuário faz um ou mais comentários do livro de visitas e, em seguida, podemos tentar excluir essa conta de usuário, a exclusão falhará, a menos que os comentários do livro de visitas são excluídos primeiro.

Restrições de chave estrangeira podem ser configuradas para excluir automaticamente os registros filho associado quando um registro pai é excluído. Em outras palavras, podemos pode configurar essa restrição de chave estrangeira para que as entradas de livro de visitas do usuário são excluídas automaticamente quando sua conta de usuário é excluída. Para fazer isso, expanda a seção "Especificação de inserção e atualização" e defina a propriedade de "Excluir a regra" em cascata.


[![Configurar a restrição de chave estrangeira para exclusões em cascata](storing-additional-user-information-vb/_static/image14.png)](storing-additional-user-information-vb/_static/image13.png)

**Figura 5**: configurar a restrição de chave estrangeira para exclusões em cascata ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image15.png))


Para salvar a restrição de chave estrangeira, clique no botão Fechar para sair das relações de chave estrangeira. Em seguida, clique no ícone Salvar na barra de ferramentas para salvar a tabela e essa relação.

### <a name="storing-the-users-home-town-homepage-and-signature"></a>Armazenar Home Town, home page e assinatura do usuário

O `GuestbookComments` tabela ilustra como armazenar informações que compartilha uma relação um-para-muitos com as contas de usuário. Como cada conta de usuário pode ter um número arbitrário de comentários associados, essa relação é modelada com a criação de uma tabela para armazenar o conjunto de comentários que inclui uma coluna que vincula cada comentário de volta para um determinado usuário. Ao usar o `SqlMembershipProvider`, esse link melhor é estabelecido com a criação de uma coluna denominada `UserId` do tipo `uniqueidentifier` e uma restrição de chave estrangeira entre esta coluna e `aspnet_Users.UserId`.

Agora, precisamos associar três colunas a cada conta de usuário para armazenar a cidade, home page e assinatura, que será exibido em seu livro de visitas comentários do usuário. Há várias maneiras diferentes para fazer isso:

- <strong>Adicionar novas colunas para o</strong><strong>`aspnet_Users`</strong><strong>ou</strong><strong>`aspnet_Membership`</strong><strong>tabelas.</strong> Não recomendo essa abordagem porque ele modifica o esquema usado pelo `SqlMembershipProvider`. Essa decisão pode se voltar contra você ao longo do caminho. Por exemplo, se uma versão futura do ASP.NET usa outro `SqlMembershipProvider` esquema. A Microsoft pode incluir uma ferramenta para migrar do ASP.NET 2.0 `SqlMembershipProvider` dados para o novo esquema, mas se você tiver modificado o ASP.NET 2.0 `SqlMembershipProvider` esquema, essa conversão pode não ser possível.

- **Use o ASP. Framework de perfil da rede, definindo uma propriedade de perfil para a cidade, a home page e a assinatura.** O ASP.NET inclui uma estrutura de perfil que foi projetada para armazenar dados específicos de usuário adicionais. Como a estrutura de associação, a estrutura de perfil foi criada sobre o modelo de provedor. O .NET Framework vem com um `SqlProfileProvider` que armazena dados em um banco de dados do SQL Server do perfil. Na verdade, nosso banco de dados já tem a tabela usada pelo `SqlProfileProvider` (`aspnet_Profile`), como ele foi adicionado quando adicionamos os serviços de aplicativo de volta a [ *criando o esquema de associação no SQL Server* ](creating-the-membership-schema-in-sql-server-vb.md)tutorial.   
  O principal benefício do framework de perfil é que ela permite aos desenvolvedores definir as propriedades de perfil no `Web.config` – nenhum código precisa ser escrito para serializar os dados de perfil de e para o armazenamento de dados subjacente. Em resumo, é incrivelmente fácil definir um conjunto de propriedades de perfil e trabalhar com eles no código. No entanto, o sistema de perfil deixa muito a desejar quando se trata de controle de versão, portanto, se você tiver um aplicativo em que você espera que novas propriedades específicas do usuário a ser adicionado em um momento posterior, ou os existentes para ser removido ou modificado, em seguida, a estrutura de perfil pode não ser o  melhor opção. Além disso, o `SqlProfileProvider` armazena as propriedades de perfil de maneira altamente desnormalizada, tornando praticamente impossível executar consultas diretamente nos dados de perfil (por exemplo, quantos usuários têm uma cidade de Nova York doméstica).   
  Para obter mais informações sobre a estrutura de perfil, consulte a seção de "Leituras adicionais" no final deste tutorial.

- <strong>Adicione essas três colunas em uma nova tabela no banco de dados e estabelecer uma relação um para um entre essa tabela e</strong><strong>`aspnet_Users`</strong><strong>.</strong> Essa abordagem envolve um pouco mais trabalho do que com o framework de perfil, mas oferece a máxima flexibilidade em como as propriedades adicionais do usuário são modeladas no banco de dados. Essa é a opção que usaremos neste tutorial.

Vamos criar uma nova tabela chamada `UserProfiles` para salvar a cidade, a home page e a assinatura para cada usuário. Clique com botão direito na pasta tabelas na janela do Gerenciador de banco de dados e optar por criar uma nova tabela. Nome da coluna da primeira `UserId` e defina seu tipo como `uniqueidentifier`. Não permitir `NULL` valores e marque a coluna como uma chave primária. Em seguida, adicione colunas nomeadas: `HomeTown` do tipo `nvarchar(50)`; `HomepageUrl` do tipo `nvarchar(100)`; e a assinatura de tipo `nvarchar(500)`. Cada uma dessas três colunas pode aceitar um `NULL` valor.


[![Criar a tabela UserProfiles](storing-additional-user-information-vb/_static/image17.png)](storing-additional-user-information-vb/_static/image16.png)

**Figura 6**: criar o `UserProfiles` tabela ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image18.png))


Salve a tabela e nomeie- `UserProfiles`. Por fim, estabelecer uma restrição de chave estrangeira entre a `UserProfiles` da tabela `UserId` campo e o `aspnet_Users.UserId` campo. Como fizemos com a restrição de chave estrangeira entre a `GuestbookComments` e `aspnet_Users` tabelas, têm essa restrição Propagar exclusões. Uma vez que o `UserId` campo `UserProfiles` é o primário chave, isso garante que haverá não mais do que um registro no `UserProfiles` tabela para cada conta de usuário. Esse tipo de relação é chamado como um para um.

Agora que temos o modelo de dados criado, estamos prontos para usá-lo. Nas etapas 2 e 3 vamos examinar como o usuário conectado no momento pode exibir e editar suas informações de cidade, a home page e a assinatura principal. Etapa 4, vamos criar a interface para usuários autenticados enviar novos comentários para o livro de visitas e exibir os existentes.

## <a name="step-2-displaying-the-users-home-town-homepage-and-signature"></a>Etapa 2: Exibindo Home Town, home page e assinatura do usuário

Há várias maneiras para permitir que o usuário conectado no momento exibir e editar suas informações de cidade, a home page e a assinatura iniciais. Podemos pode criar manualmente a interface do usuário com a caixa de texto e controles de rótulo ou, use um dos controles da Web, como o controle DetailsView de dados. Para executar o banco de dados `SELECT` e `UPDATE` instruções poderíamos escrever ADO.NET na classe de code-behind da nossa página de código ou, Alternativamente, empregar uma abordagem declarativa com o SqlDataSource. O ideal é que o nosso aplicativo conteria uma arquitetura em camadas, o que podemos pode invocar por meio de programação de classe de code-behind da página, ou declarativamente por meio do controle ObjectDataSource.

Uma vez que esta série de tutoriais se concentra em formulários de autenticação, autorização, contas de usuário e funções, não haverá uma discussão completa sobre essas opções de acesso de dados diferente ou por que uma arquitetura em camadas é preferível executar instruções SQL diretamente Na página ASP.NET. Vou percorrer usando um DetailsView e SqlDataSource – a opção mais rápida e fácil – mas os conceitos discutidos certamente podem ser aplicados a alternativa lógica de acesso dados e controles da Web. Para obter mais informações sobre como trabalhar com dados no ASP.NET, consulte a minha *[trabalhando com dados no ASP.NET 2.0](../../data-access/index.md)* série de tutoriais.

Abra o `AdditionalUserInfo.aspx` página na `Membership` pasta e adicione um controle DetailsView para a página, definindo sua propriedade de ID como `UserProfile` e limpeza dos seus `Width` e `Height` propriedades. Expanda de marca de DetailsView inteligente e escolha vinculá-la a um novo controle de fonte de dados. Isso iniciará o Assistente de configuração de fonte de dados (veja a Figura 7). A primeira etapa solicitará que você especifique o tipo de fonte de dados. Já que vamos para se conectar diretamente para o `SecurityTutorials` banco de dados, escolha o ícone de banco de dados, especificando o `ID` como `UserProfileDataSource`.


[![Adicionar um novo controle SqlDataSource chamado UserProfileDataSource](storing-additional-user-information-vb/_static/image20.png)](storing-additional-user-information-vb/_static/image19.png)

**Figura 7**: adicionar um controle SqlDataSource novo chamado `UserProfileDataSource` ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image21.png))


A próxima tela solicita o banco de dados usar. Nós já definimos uma cadeia de caracteres de conexão no `Web.config` para o `SecurityTutorials` banco de dados. Esse nome de cadeia de caracteres de conexão – `SecurityTutorialsConnectionString` – devem estar na lista suspensa. Selecione esta opção e clique em Avançar.


[![Escolha SecurityTutorialsConnectionString na lista suspensa](storing-additional-user-information-vb/_static/image23.png)](storing-additional-user-information-vb/_static/image22.png)

**Figura 8**: escolha `SecurityTutorialsConnectionString` na lista suspensa ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image24.png))


A tela subsequente nos pede para especificar a tabela e colunas à consulta. Escolha o `UserProfiles` de tabela na lista suspensa e verificar todas as colunas.


[![Trazer de volta todas as colunas da tabela UserProfiles](storing-additional-user-information-vb/_static/image26.png)](storing-additional-user-information-vb/_static/image25.png)

**Figura 9**: colocar novamente todas as das colunas dos `UserProfiles` tabela ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image27.png))


A consulta atual na Figura 9 retorna *todos os* dos registros no `UserProfiles`, mas estamos interessados apenas no registro do usuário conectado no momento. Para adicionar um `WHERE` cláusula, clique em de `WHERE` botão para abrir a adicionar `WHERE` caixa de diálogo de cláusula (veja a Figura 10). Aqui você pode selecionar a coluna para filtrar, o operador e a origem do parâmetro de filtro. Selecione `UserId` como a coluna e "=" como o operador.

Infelizmente não há nenhuma fonte de parâmetro interno para retornar o usuário conectado no momento `UserId` valor. Precisamos pegar esse valor por meio de programação. Portanto, defina a lista suspensa de origem para "None", clique em Adicionar botão para adicionar o parâmetro e, em seguida, clique em Okey.


[![Adicionar um parâmetro de filtro na coluna de ID de usuário](storing-additional-user-information-vb/_static/image29.png)](storing-additional-user-information-vb/_static/image28.png)

**Figura 10**: adicionar um parâmetro de filtro sobre a `UserId` coluna ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image30.png))


Depois de clicar em Okey, você voltará para a tela mostrada na Figura 9. Neste momento, no entanto, a consulta SQL na parte inferior da tela deve incluir um `WHERE` cláusula. Clique em Avançar para passar para a tela de "Consulta de teste". Aqui você pode executar a consulta e ver os resultados. Clique em Concluir para concluir o assistente.

Após concluir o Assistente de configuração de fonte de dados, o Visual Studio cria o controle SqlDataSource com base nas configurações especificadas no assistente. Além disso, ele adiciona manualmente BoundFields a DetailsView para cada coluna retornada pelo SqlDataSource `SelectCommand`. Não é necessário para mostrar o `UserId` campo DetailsView, uma vez que o usuário não precisa saber esse valor. Você pode remover esse campo diretamente na marcação declarativa do controle DetailsView ou clicando em "Editar campos" link da sua marca inteligente.

Neste ponto marcação declarativa de sua página deve ser semelhante ao seguinte:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample1.aspx)]

Precisamos definir programaticamente do controle SqlDataSource `UserId` parâmetro para o usuário conectado no momento `UserId` antes que os dados são selecionados. Isso pode ser feito criando um manipulador de eventos para o SqlDataSource `Selecting` eventos e adicionando o seguinte código lá:

[!code-vb[Main](storing-additional-user-information-vb/samples/sample2.vb)]

O código acima começa obtendo uma referência para o usuário conectado no momento chamando o `Membership` da classe `GetUser` método. Isso retorna um `MembershipUser` do objeto, cuja `ProviderUserKey` propriedade contém o `UserId`. O `UserId` valor é então atribuído para o SqlDataSource `@UserId` parâmetro.

> [!NOTE]
> O `Membership.GetUser()` método retorna informações sobre o usuário conectado no momento. Se um usuário anônimo está visitando a página, ela retornará um valor de `Nothing`. Nesse caso, isso levará a um `NullReferenceException` na linha seguinte do código ao tentar ler o `ProviderUserKey` propriedade. Obviamente, não precisamos se preocupar sobre `Membership.GetUser()` retornando nada o `AdditionalUserInfo.aspx` página como configuramos a autorização de URL em um tutorial anterior para que somente usuários autenticados poderiam acessar os recursos do ASP.NET nesta pasta. Se você precisar acessar informações sobre o usuário conectado no momento em uma página em que o acesso anônimo é permitido, certifique-se de verificar que o `MembershipUser` objeto retornado do `GetUser()` método não é nada antes de fazer referência a suas propriedades.


Se você visitar o `AdditionalUserInfo.aspx` página por meio de um navegador você verá uma página em branco porque ainda precisamos adicionar linhas a serem o `UserProfiles` tabela. Na etapa 6, vamos examinar como personalizar o controle CreateUserWizard para adicionar automaticamente uma nova linha para o `UserProfiles` tabela quando uma nova conta de usuário é criada. Por enquanto, no entanto, precisamos criar manualmente um registro na tabela.

Navegue até o Gerenciador de banco de dados no Visual Studio e expanda a pasta de tabelas. Clique com botão direito no `aspnet_Users` tabela e escolha "Mostrar dados da tabela" para ver os registros na tabela; para fazer a mesma coisa o `UserProfiles` tabela. Figura 11 mostra estes resultados quando o lado a lado verticalmente. No meu banco de dados atualmente, há `aspnet_Users` registros de Bruce, Fred e Tito, mas não há registros no `UserProfiles` tabela.


[![O conteúdo do aspnet_Users e UserProfiles tabelas são exibidas](storing-additional-user-information-vb/_static/image32.png)](storing-additional-user-information-vb/_static/image31.png)

**Figura 11**: O conteúdo do `aspnet_Users` e `UserProfiles` tabelas são exibidas ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image33.png))


Adicione um novo registro para o `UserProfiles` tabela digitando manualmente valores para o `HomeTown`, `HomepageUrl`, e `Signature` campos. A maneira mais fácil de obter válida `UserId` valor no novo `UserProfiles` registro é selecionar o `UserId` campo de uma conta de usuário específico no `aspnet_Users` de tabela e copie e cole-o na `UserId` campo `UserProfiles`. A Figura 12 mostra o `UserProfiles` depois que foi adicionado a um novo registro de Bruce de tabela.


[![Um registro foi adicionado ao UserProfiles para Bruce](storing-additional-user-information-vb/_static/image35.png)](storing-additional-user-information-vb/_static/image34.png)

**Figura 12**: um registro foi adicionado à `UserProfiles` de Bruce ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image36.png))


Volte para o `AdditionalUserInfo.aspx page`, conectado como Bruce. Como mostra a Figura 13, configurações de Bruce são exibidas.


[![O usuário no momento, visitando é mostrado His configurações](storing-additional-user-information-vb/_static/image38.png)](storing-additional-user-information-vb/_static/image37.png)

**Figura 13**: O atualmente visitando usuário é mostrado His configurações ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image39.png))


> [!NOTE]
> Vá em frente e manualmente adicione registros no `UserProfiles` tabela para cada usuário da associação. Na etapa 6, vamos examinar como personalizar o controle CreateUserWizard para adicionar automaticamente uma nova linha para o `UserProfiles` tabela quando uma nova conta de usuário é criada.


## <a name="step-3-allowing-the-user-to-edit-his-home-town-homepage-and-signature"></a>Etapa 3: Permitir que o usuário edite sua cidade inicial, a home page e a assinatura

Neste momento o usuário conectado no momento pode exibir sua cidade, home page e configuração de assinatura, mas eles ainda não é possível modificá-los. Vamos atualizar o controle DetailsView para que os dados podem ser editados.

A primeira coisa que precisamos fazer é adicionar um `UpdateCommand` para o SqlDataSource, especificando o `UPDATE` instrução a ser executada e seus parâmetros correspondentes. Selecione o SqlDataSource e, na janela Propriedades, clique na elipse ao lado da propriedade UpdateQuery para abrir a caixa de diálogo Editor de comando e parâmetro. Insira o seguinte `UPDATE` instrução na caixa de texto:

[!code-sql[Main](storing-additional-user-information-vb/samples/sample3.sql)]

Em seguida, clique no botão "Atualizar parâmetros", que criará um parâmetro no controle do SqlDataSource `UpdateParameters` coleção para cada um dos parâmetros no `UPDATE` instrução. Deixe a fonte para todos os parâmetros como None e clique no botão Okey para concluir a caixa de diálogo.


[![Especifique o SqlDataSource UpdateCommand e UpdateParameters](storing-additional-user-information-vb/_static/image41.png)](storing-additional-user-information-vb/_static/image40.png)

**Figura 14**: especifique o SqlDataSource `UpdateCommand` e `UpdateParameters` ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image42.png))


Devido às adições que fizemos para o controle SqlDataSource, DetailsView controle pode oferecer suporte a edição. Na marca inteligente de DetailsView, marque a caixa de seleção "Habilitar edição". Isso adiciona um CommandField para o controle `Fields` coleção com seu `ShowEditButton` propriedade definida como True. Isso renderiza um botão de edição quando DetailsView é exibido no modo somente leitura e atualização e botões de cancelamento quando exibido no modo de edição. Em vez de exigir que o usuário clique em Editar, no entanto, podemos ter renderização DetailsView em um estado "sempre editável", definindo o controle de DetailsView [ `DefaultMode` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode.aspx) para `Edit`.

Com essas alterações, a marcação declarativa do seu controle DetailsView deve ser semelhante ao seguinte:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample4.aspx)]

Observe a adição do CommandField e o `DefaultMode` propriedade.

Vá em frente e teste essa página por meio de um navegador. Ao visitar com um usuário que tem um registro correspondente na `UserProfiles`, as configurações do usuário são exibidas em uma interface editável.


[![DetailsView renderiza uma Interface editável](storing-additional-user-information-vb/_static/image44.png)](storing-additional-user-information-vb/_static/image43.png)

**Figura 15**: DetailsView renderiza uma Interface editável ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image45.png))


Tente alterar os valores e clicando no botão de atualização. Ele aparece como se nada acontecerá. Há um postback e os valores são salvos no banco de dados, mas não há nenhum feedback visual que ocorreu o salvamento.

Para corrigir isso, retorne ao Visual Studio e adicione um controle de rótulo acima DetailsView. Defina suas `ID` para `SettingsUpdatedMessage`, sua `Text` propriedade como "suas configurações foram atualizadas," e seu `Visible` e `EnableViewState` propriedades para `False`.

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample5.aspx)]

É necessário exibir o `SettingsUpdatedMessage` rotular sempre que DetailsView é atualizado. Para fazer isso, crie um manipulador de eventos para o DetailsView `ItemUpdated` eventos e adicione o seguinte código:

[!code-vb[Main](storing-additional-user-information-vb/samples/sample6.vb)]

Volte para o `AdditionalUserInfo.aspx` página por meio de um navegador e atualizar os dados. Desta vez, será exibida uma mensagem de status úteis.


[![Uma mensagem curta é exibido quando as configurações são atualizadas](storing-additional-user-information-vb/_static/image47.png)](storing-additional-user-information-vb/_static/image46.png)

**Figura 16**: uma pequena mensagem é exibida quando as configurações são atualizadas ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image48.png))


> [!NOTE]
> O controle DetailsView de edição do interface deixa muito a desejar. Ele usa o tamanho padrão caixas de texto, mas o campo de assinatura provavelmente deve ser uma caixa de texto de várias linha. Um RegularExpressionValidator deve ser usado para garantir que a URL da home page, se inserido, comece com "http://" ou "https://". Além disso, desde DetailsView controle tem seu `DefaultMode` propriedade definida como `Edit`, o botão Cancelar não faz nada. Ele o devem ser removido ou, quando clicado, redirecione o usuário para alguma outra página (como `~/Default.aspx`). Posso deixar essas melhorias como um exercício para o leitor.


### <a name="adding-a-link-to-theadditionaluserinfoaspxpage-in-the-master-page"></a>Adicionando um Link para o`AdditionalUserInfo.aspx`página na página mestra

Atualmente, o site não oferece quaisquer links para o `AdditionalUserInfo.aspx` página. É a única maneira de acessá-lo inserir a URL da página diretamente na barra de endereços do navegador. Vamos adicionar um link para esta página no `Site.master` página mestra.

Lembre-se de que a página mestra contém um controle LoginView Web no seu `LoginContent` ContentPlaceHolder que exibe a marcação diferente para os visitantes autenticados e anônimos. Atualizar do controle LoginView `LoggedInTemplate` para incluir um link para o `AdditionalUserInfo.aspx` página. Depois de fazer essas alterações a LoginView marcação declarativa do controle deve ser semelhante ao seguinte:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample7.aspx)]

Observe a adição do `lnkUpdateSettings` controle de hiperlink para o `LoggedInTemplate`. Com este link em vigor, os usuários autenticados podem ir rapidamente para a página para exibir e modificar suas configurações de cidade, a home page e a assinatura iniciais.

## <a name="step-4-adding-new-guestbook-comments"></a>Etapa 4: Adicionando novos comentários de livro de visitas

O `Guestbook.aspx` página é onde os usuários autenticados podem exibir o livro de visitas e deixe um comentário. Vamos começar criando a interface para adicionar novos comentários de livro de visitas.

Abra o `Guestbook.aspx` página no Visual Studio e construir uma interface do usuário consiste em dois controles TextBox, um para o assunto do novo comentário e outra para seu corpo. Defina o controle de caixa de texto primeiro `ID` propriedade para `Subject` e seu `Columns` propriedade para 40; do segundo conjunto `ID` para `Body`, sua `TextMode` para `MultiLine`e seu `Width` e `Rows` as propriedades para "95%" e 8, respectivamente. Para concluir a interface do usuário, adicione um controle da Web de botão chamado `PostCommentButton` e defina seu `Text` propriedade como "Postar seu comentário".

Como cada comentário do livro de visitas requer um assunto e corpo, adicione um RequiredFieldValidator para cada uma das caixas de texto. Defina a `ValidationGroup` propriedade desses controles para "EnterComment"; da mesma forma, defina a `PostCommentButton` do controle `ValidationGroup` propriedade como "EnterComment". Para obter mais informações sobre o ASP. Controles de validação da rede, check-out [validação do formulário no ASP.NET](http://www.4guysfromrolla.com/webtech/090200-1.shtml), [dissecando os controles de validação no ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)e o [Tutorial de controles de servidor de validação](http://www.w3schools.com/aspnet/aspnet_refvalidationcontrols.asp) na [W3Schools](http://www.w3schools.com/).

Depois de elaborar a interface do usuário marcação declarativa de sua página deve ser algo semelhante ao seguinte:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample8.aspx)]

Com a interface de usuário completa, nossa próxima tarefa é inserir um novo registro para o `GuestbookComments` tabela quando o `PostCommentButton` é clicado. Isso pode ser feito de várias maneiras: podemos escrever código ADO.NET no botão `Click` manipulador de eventos; podemos pode adicionar um controle SqlDataSource para a página, configurar seu `InsertCommand`e, em seguida, chame seu `Insert` método do `Click` evento manipulador; ou poderíamos criar uma camada intermediária que era responsável por inserir novos comentários de livro de visitas e invocar essa funcionalidade no `Click` manipulador de eventos. Uma vez que analisamos usando um SqlDataSource na etapa 3, vamos usar código ADO.NET aqui.

> [!NOTE]
> As classes ADO.NET usadas para acessar programaticamente os dados de um banco de dados do Microsoft SQL Server estão localizadas no `System.Data.SqlClient` namespace. Talvez seja necessário importar esse namespace para classe de code-behind da sua página (ou seja, `Imports System.Data.SqlClient`).


Crie um manipulador de eventos para o `PostCommentButton`do `Click` evento e adicione o seguinte código:

[!code-vb[Main](storing-additional-user-information-vb/samples/sample9.vb)]

O `Click` manipulador de eventos é iniciado, verificando se os dados fornecidos pelo usuário são válidos. Se não for, o manipulador de eventos é encerrado antes de inserir um registro. Supondo que os dados fornecidos são válidos, o usuário conectado no momento `UserId` valor é recuperado e armazenado no `currentUserId` variável local. Esse valor é necessário porque estamos deve fornecer um `UserId` valor ao inserir um registro em `GuestbookComments`.

Depois disso, a conexão de cadeia de caracteres para o `SecurityTutorials` banco de dados é recuperado do `Web.config` e o `INSERT` instrução SQL é especificada. Um `SqlConnection` objeto, em seguida, é criado e aberto. Em seguida, uma `SqlCommand` objeto é construído e os valores para os parâmetros usados no `INSERT` da consulta são atribuídos. O `INSERT` instrução será executada e a conexão é fechada. No final do manipulador de eventos, o `Subject` e `Body` caixas de texto `Text` propriedades são limpos para que os valores do usuário não persistem entre o postback.

Vá em frente e testar esta página em um navegador. Uma vez que essa página está no `Membership` pasta não está acessível para visitantes anônimos. Portanto, você precisará fazer logon pela primeira vez (se você ainda não tiver). Insira um valor para o `Subject` e `Body` caixas de texto e clique no `PostCommentButton` botão. Isso fará com que um novo registro a ser adicionado ao `GuestbookComments`. No postback, o assunto e corpo fornecido por você são apagados de TextBoxes.

Depois de clicar o `PostCommentButton` botão lá está sem comentários visuais que o comentário foi adicionado ao livro de visitas. Ainda precisamos atualizar esta página para exibir os comentários de livro de visitas existente, o que vamos fazer na etapa 5. Depois que podemos fazer isso, o comentário adicionado apenas será exibida na lista de comentários, fornecer comentários visuais adequados. Por enquanto, confirme se o seu comentário do livro de visitas foi salvo, examinando o conteúdo do `GuestbookComments` tabela.

Figura 17 mostra o conteúdo do `GuestbookComments` após dois comentários foram deixados de tabela.


[![Você pode ver os comentários do livro de visitas na tabela GuestbookComments](storing-additional-user-information-vb/_static/image50.png)](storing-additional-user-information-vb/_static/image49.png)

**Figura 17**: você pode ver comentários no livro de visitas a `GuestbookComments` tabela ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image51.png))


> [!NOTE]
> Se um usuário tentar inserir um comentário de livro de visitas contém potencialmente perigosa marcação – como HTML – ASP.NET irá acionar uma `HttpRequestValidationException`. Para saber mais sobre essa exceção, por que é gerada, e como permitir que os usuários enviem valores potencialmente perigosos, consulte o [white paper de validação de solicitação](../../../../whitepapers/request-validation.md).


## <a name="step-5-listing-the-existing-guestbook-comments"></a>Etapa 5: Listando os comentários existentes do livro de visitas

Além de deixar comentários, um usuário visitar o `Guestbook.aspx` página também deve ser capaz de exibir os comentários existentes do livro de visitas. Para fazer isso, adicione um controle ListView chamado `CommentList` na parte inferior da página.

> [!NOTE]
> O controle ListView é novo para o ASP.NET versão 3.5. Ele é projetado para exibir uma lista de itens em um layout bastante personalizável e flexível, e ainda oferecem ainda internos editando, inserindo, excluindo, paginação e classificação funcionalidades como GridView. Se você estiver usando o ASP.NET 2.0, você precisará usar o controle DataList ou Repeater em vez disso. Para obter mais informações sobre como usar o ListView, consulte [Scott Guthrie](https://weblogs.asp.net/scottgu/)da entrada de blog [controle asp: ListView](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)e o meu artigo [exibindo dados com o controle ListView](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx).


Abrir de marca do ListView inteligente e, na lista suspensa Escolher fonte de dados, associar o controle para uma nova fonte de dados. Como vimos na etapa 2, isso iniciará o Assistente de configuração de fonte de dados. Selecione o ícone de banco de dados, nomeie o SqlDataSource resultante `CommentsDataSource`e clique em Okey. Em seguida, selecione o `SecurityTutorialsConnectionString` conexão da cadeia de caracteres da lista suspensa e clique em Avançar.

Nesse ponto na etapa 2 especificamos os dados para consulta pela separação de `UserProfiles` de tabela na lista suspensa e selecionar as colunas para retornar (consulte novamente a Figura 9). Neste momento, no entanto, queremos criar uma instrução SQL que efetua pull de não apenas os registros de volta `GuestbookComments`, mas também do autor do comentário cidade, home page, assinatura e nome de usuário. Portanto, selecione o botão de rádio "Especificar uma instrução SQL personalizada ou procedimento armazenado" e clique em Avançar.

Isso abrirá a tela de "Definir personalizado instruções ou procedimentos armazenados". Clique no botão Construtor de consultas para compilar graficamente a consulta. O construtor de consultas começa solicitando que você especifique as tabelas que desejamos consultar. Selecione o `GuestbookComments`, `UserProfiles`, e `aspnet_Users` tabelas e clique em Okey. Isso adicionará todas as três tabelas à superfície de design. Como não há restrições de chave estrangeira entre a `GuestbookComments`, `UserProfiles`, e `aspnet_Users` tabelas, o construtor de consultas automaticamente `JOIN` s essas tabelas.

Tudo o que resta fazer é especificar as colunas para retornar. Do `GuestbookComments` select da tabela o `Subject`, `Body`, e `CommentDate` colunas; retornar o `HomeTown`, `HomepageUrl`, e `Signature` colunas da `UserProfiles` tabela; e retornar `UserName` do `aspnet_Users`. Além disso, adicione "`ORDER BY CommentDate DESC`" ao final do `SELECT` consulta para que as postagens mais recentes são retornadas pela primeira vez. Depois de fazer essas seleções, sua interface do construtor de consultas deve ser semelhante para a tela na Figura 18.


[![A consulta de Constructed une as tabelas de aspnet_Users, UserProfiles e GuestbookComments](storing-additional-user-information-vb/_static/image53.png)](storing-additional-user-information-vb/_static/image52.png)

**Figura 18**: A consulta construída `JOIN` s do `GuestbookComments`, `UserProfiles`, e `aspnet_Users` tabelas ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image54.png))


Clique em Okey para fechar a janela do construtor de consulta e retornar à tela "Definir personalizado instruções ou procedimentos armazenados". Clique em Avançar para ir à tela de "Consulta de teste", onde você pode exibir os resultados da consulta clicando no botão consulta de teste. Quando você estiver pronto, clique em Concluir para concluir o Assistente Configurar fonte de dados.

Quando é concluído o Assistente Configurar fonte de dados do etapa 2, o controle DetailsView associado `Fields` coleção foi atualizada para incluir um BoundField para cada coluna retornada pela `SelectCommand`. No entanto, o ListView, permanece inalterado; ainda precisamos definir seu layout. Layout do ListView pode ser construído manualmente por meio de sua marcação declarativa ou na opção "Configurar ListView" em sua marca inteligente. Eu geralmente prefiro definir a marcação manualmente, mas usar qualquer método que é mais natural para você.

Eu acabei usando o seguinte `LayoutTemplate`, `ItemTemplate`, e `ItemSeparatorTemplate` do meu controle ListView:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample10.aspx)]

O `LayoutTemplate` define a marcação emitidos pelo controle, enquanto o `ItemTemplate` processa cada item retornado pela SqlDataSource. O `ItemTemplate`da marcação resultante é colocada na `LayoutTemplate`do `itemPlaceholder` controle. Além de `itemPlaceholder`, o `LayoutTemplate` inclui um controle DataPager, o que limita o ListView para mostrar apenas 10 comentários de livro de visitas por página (o padrão) e renderiza uma interface de paginação.

Minha `ItemTemplate` exibe o assunto do livro de visitas de cada comentário em um `<h4>` elemento com o corpo situado abaixo o assunto. Observe que a sintaxe usada para exibir o corpo leva os dados retornados pela `Eval("Body")` declaração de associação de dados, converte-o em uma cadeia de caracteres e quebras de linha de substitui com o `<br />` elemento. Essa conversão é necessária para mostrar as quebras de linha inseridas ao enviar o comentário, pois o espaço em branco é ignorado por HTML. Assinatura do usuário é exibida abaixo do corpo em itálico, seguido cidade do usuário, um link para sua home page, a data e hora em que o comentário foi feito e o nome de usuário da pessoa que deixou o comentário.

Reserve um tempo para exibir a página por meio de um navegador. Você deve ver os comentários que você adicionou ao livro de visitas na etapa 5 exibidas aqui.


[![GuestBook agora exibe comentários do livro de visitas](storing-additional-user-information-vb/_static/image56.png)](storing-additional-user-information-vb/_static/image55.png)

**Figura 19**: `Guestbook.aspx` agora exibe comentários do livro de visitas ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image57.png))


Tente adicionar um novo comentário para o livro de visitas. Ao clicar o `PostCommentButton` botão a página é postada e o comentário for adicionado ao banco de dados, mas o controle ListView não é atualizado para mostrar o novo comentário. Isso pode ser corrigido por qualquer um:

- Atualizando o `PostCommentButton` do botão `Click` manipulador de eventos para que ele invoca o controle de ListView `DataBind()` método depois de inserir o novo comentário para o banco de dados, ou
- Configuração do controle ListView `EnableViewState` propriedade para `False`. Essa abordagem funciona porque, ao desabilitar o estado de exibição do controle, ele deve associar novamente os dados subjacentes em cada postagem.

O site do tutorial disponível para download deste tutorial ilustra ambas as técnicas. Do controle ListView `EnableViewState` propriedade para `False` e o código necessário para reassociar programaticamente os dados para o ListView está presente no `Click` manipulador de eventos, mas é comentado.

> [!NOTE]
> Atualmente, o `AdditionalUserInfo.aspx` página permite que o usuário exibir e editar suas configurações de cidade, a home page e a assinatura iniciais. Seria bom atualizar `AdditionalUserInfo.aspx` para exibir o log em comentários de livro de visitas do usuário. Ou seja, além de examinar e modificar suas informações, um usuário poderá visitar o `AdditionalUserInfo.aspx` página para ver quais livro de visitas comentários, ela é feita no passado. Vou deixar isso como um exercício para os leitores interessados.


## <a name="step-6-customizing-the-createuserwizard-control-to-include-an-interface-for-the-home-town-homepage-and-signature"></a>Etapa 6: Personalizando o controle CreateUserWizard para incluir uma Interface para a cidade inicial, home page e assinatura

O `SELECT` consulta usada pelo `Guestbook.aspx` página usa um `INNER JOIN` para combinar os registros relacionados entre os `GuestbookComments`, `UserProfiles`, e `aspnet_Users` tabelas. Se um usuário que não tem nenhum registro na `UserProfiles` possibilita um livro de visitas de comentário, o comentário não será exibido o ListView porque o `INNER JOIN` retorna apenas `GuestbookComments` registros quando há registros correspondentes na `UserProfiles` e `aspnet_Users`. Conforme observamos na etapa 3, se um usuário não tem um registro em `UserProfiles` ela não é possível exibir ou editar suas configurações no `AdditionalUserInfo.aspx` página.

Desnecessário dizer, é importante que cada conta de usuário no sistema de associação tem uma correspondência de decisões devido ao nosso design registram no `UserProfiles` tabela. O que queremos é um registro correspondente a ser adicionado ao `UserProfiles` sempre que uma nova conta de usuário de associação é criada por meio de CreateUserWizard.

Conforme discutido na [ *criando contas de usuário* ](creating-user-accounts-vb.md) tutorial, depois que a nova conta de usuário de associação é criada o controle CreateUserWizard aciona seu [ `CreatedUser` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx). Podemos criar um manipulador de eventos para esse evento, obter a ID de usuário para o usuário recém-criada e, em seguida, inserir um registro para o `UserProfiles` tabela com valores padrão para o `HomeTown`, `HomepageUrl`, e `Signature` colunas. Além disso, é possível solicitar ao usuário para obter esses valores, personalizando a interface do controle CreateUserWizard para incluir caixas de texto adicionais.

Vamos primeiro examinar como adicionar uma nova linha para o `UserProfiles` na tabela a `CreatedUser` manipulador de eventos com valores padrão. Depois disso, veremos como personalizar a interface do usuário do controle CreateUserWizard para incluir campos adicionais para coletar o novo usuário cidade, home page e assinatura.

### <a name="adding-a-default-row-touserprofiles"></a>Adição de uma linha padrão para`UserProfiles`

No [ *criando contas de usuário* ](creating-user-accounts-vb.md) tutorial adicionamos um controle CreateUserWizard para o `CreatingUserAccounts.aspx` página o `Membership` pasta. Para ter o CreateUserWizard controle Adicionar um registro a `UserProfiles` tabela no momento da criação da conta de usuário, precisamos atualizar a funcionalidade do controle CreateUserWizard. Em vez de fazer essas alterações para o `CreatingUserAccounts.aspx` página, em vez disso, adicionaremos um novo controle CreateUserWizard para `EnhancedCreateUserWizard.aspx` página e faça as modificações para este tutorial lá.

Abra o `EnhancedCreateUserWizard.aspx` página no Visual Studio e arraste um controle CreateUserWizard Toolbox para a página. Defina o controle de CreateUserWizard `ID` propriedade para `NewUserWizard`. Como discutimos na [ *criando contas de usuário* ](creating-user-accounts-vb.md) tutorial, a interface do usuário padrão do CreateUserWizard solicita que o visitante para as informações necessárias. Depois que essas informações foram fornecidas, o controle internamente cria uma nova conta de usuário na estrutura de associação, tudo isso sem nós ter que escrever uma única linha de código.

O controle CreateUserWizard eleva um número de eventos durante o fluxo de trabalho. Depois que um visitante fornece as informações de solicitação e envia o formulário, o controle CreateUserWizard inicialmente dispara seu [ `CreatingUser` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx). Se houver um problema durante o processo de criação, o [ `CreateUserError` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx) é acionado; no entanto, se o usuário é criado com êxito, o [ `CreatedUser` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx) é gerado. No [ *criando contas de usuário* ](creating-user-accounts-vb.md) tutorial, criamos um manipulador de eventos para o `CreatingUser` evento para garantir que o nome de usuário fornecido não continha qualquer à esquerda ou à direita de espaços e que o nome de usuário não foram exibidos em qualquer lugar na senha.

Para adicionar uma linha na `UserProfiles` tabela de usuário recém-criada, precisamos criar um manipulador de eventos para o `CreatedUser` eventos. No momento o `CreatedUser` evento foi disparado, a conta de usuário já tiver sido criada na estrutura de associação, que permite recuperar o valor de ID de usuário da conta.

Crie um manipulador de eventos para o `NewUserWizard`do `CreatedUser` evento e adicione o seguinte código:

[!code-vb[Main](storing-additional-user-information-vb/samples/sample11.vb)]

Os seres código acima, recuperando a ID do usuário da conta de usuário just-adicionado. Isso é feito usando o `Membership.GetUser(username)` método para retornar informações sobre um usuário específico e, em seguida, usando o `ProviderUserKey` propriedade a recuperar sua ID de usuário. O nome de usuário inserido pelo usuário no controle CreateUserWizard está disponível por meio de seu [ `UserName` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.username.aspx).

Em seguida, a cadeia de caracteres de conexão é recuperada do `Web.config` e o `INSERT` instrução é especificada. Os objetos necessários do ADO.NET são instanciados e o comando executado. O código atribui uma [ `DBNull` ](https://msdn.microsoft.com/library/system.dbnull.aspx) da instância para o `@HomeTown`, `@HomepageUrl`, e `@Signature` parâmetros, que tem o efeito de inserção de banco de dados `NULL` os valores para o `HomeTown`, `HomepageUrl`, e `Signature` campos.

Visite o `EnhancedCreateUserWizard.aspx` página por meio de um navegador e criar uma nova conta de usuário. Depois de fazer isso, retorne ao Visual Studio e examinar o conteúdo do `aspnet_Users` e `UserProfiles` tabelas (como fizemos na Figura 12). Você deve ver a nova conta de usuário na `aspnet_Users` e um correspondente `UserProfiles` linha (com `NULL` os valores para `HomeTown`, `HomepageUrl`, e `Signature`).


[![Uma nova conta de usuário e o registro UserProfiles foram adicionados](storing-additional-user-information-vb/_static/image59.png)](storing-additional-user-information-vb/_static/image58.png)

**Figura 20**: uma nova conta de usuário e `UserProfiles` registro foram adicionados ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image60.png))


Depois que o visitante tenha fornecido as informações de sua nova conta e clicou no botão "Criar usuário", a conta de usuário é criada e uma linha é adicionada para o `UserProfiles` tabela. O CreateUserWizard, em seguida, exibe seu `CompleteWizardStep`, que exibe uma mensagem de êxito e um botão continuar. Clicar no botão continuar faz com que um postback, mas nenhuma ação é executada, deixando o usuário preso no `EnhancedCreateUserWizard.aspx` página.

Podemos especificar uma URL para enviar o usuário para quando o botão continuar é clicado por meio do controle CreateUserWizard [ `ContinueDestinationPageUrl` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx). Defina o `ContinueDestinationPageUrl` propriedade como "~ / Membership/AdditionalUserInfo.aspx". Isso leva o novo usuário `AdditionalUserInfo.aspx`, onde eles podem exibir e atualizar suas configurações.

### <a name="customizing-the-createuserwizards-interface-to-prompt-for-the-new-users-home-town-homepage-and-signature"></a>Personalizando a Interface do CreateUserWizard para solicitar o novo usuário cidade Home, home page e assinatura

Interface de padrão do controle CreateUserWizard é suficiente para cenários de criação de conta simples onde somente informações de conta de usuário de núcleo, como email, senha e nome de usuário precisam ser coletadas. Mas e se quiséssemos solicitar que o visitante a inserir sua cidade, home page e assinatura durante a criação de sua conta? É possível personalizar a interface do controle CreateUserWizard para coletar informações adicionais na inscrição, e essas informações podem ser usadas no `CreatedUser` manipulador de eventos para inserir registros adicionais no banco de dados subjacente.

O controle CreateUserWizard estende o controle Wizard do ASP.NET, que é um controle que permite que um desenvolvedor de página definir uma série de ordenada `WizardSteps`. O controle de assistente renderiza a etapa ativa e fornece uma interface de navegação que permite que o visitante a percorrer essas etapas. O controle Wizard é ideal para dividir uma tarefa longa em várias etapas. Para obter mais informações sobre o controle Wizard, consulte [criando uma Interface do usuário passo a passo com o controle Wizard do ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx).

Marcação de padrão do controle CreateUserWizard define dois `WizardSteps`: `CreateUserWizardStep` e `CompleteWizardStep`.

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample12.aspx)]

A primeira `WizardStep`, `CreateUserWizardStep`, renderiza a interface que solicita o nome de usuário, senha, email e assim por diante. Depois que o visitante fornece essas informações e clica em "Criar usuário", ela será mostrada a `CompleteWizardStep`, que mostra a mensagem de êxito e um botão continuar.

Para personalizar a interface do controle CreateUserWizard para incluir campos adicionais, podemos:

- <strong>Criar um ou mais novos</strong><strong>`WizardStep`</strong><strong>s para conter os elementos de interface de usuário adicionais</strong>. Para adicionar uma nova `WizardStep` para o CreateUserWizard, clique no "Adicionar/remover `WizardStep` s" link da sua marca inteligente para iniciar o `WizardStep` Editor de coleção. A partir daí você pode adicionar, remover ou reordenar as etapas no assistente. Essa é a abordagem que usaremos para este tutorial.

- <strong>Converter o</strong><strong>`CreateUserWizardStep`</strong><strong>em um editável</strong><strong>`WizardStep`</strong><strong>.</strong> Isso substitui o `CreateUserWizardStep` com um equivalente `WizardStep` cuja marcação define uma interface do usuário que corresponde a `CreateUserWizardStep`' s. Convertendo o `CreateUserWizardStep` em um `WizardStep` podemos reposicionar os controles ou adicionar elementos da interface do usuário adicionais a essa etapa. Para converter o `CreateUserWizardStep` ou `CompleteWizardStep` em um editável `WizardStep`, clique no "Personalizar criar usuário Step" ou "Personalizar conclua a etapa" vincular de Smart Tag do controle.

- **Use uma combinação das duas opções acima.**

Uma coisa importante para ter em mente é que o controle CreateUserWizard executa seu processo de criação de conta de usuário, quando se clica no botão de "Criar usuário" no seu `CreateUserWizardStep`. Não importa se há adicionais `WizardStep` s após o `CreateUserWizardStep` ou não.

Ao adicionar um personalizado `WizardStep` para o controle CreateUserWizard para coletar entrada do usuário adicionais, personalizados `WizardStep` pode ser colocado antes ou depois do `CreateUserWizardStep`. Se ele vem antes do `CreateUserWizardStep` , em seguida, a entrada do usuário adicionais coletados personalizado `WizardStep` está disponível para o `CreatedUser` manipulador de eventos. No entanto, se personalizado `WizardStep` vem depois `CreateUserWizardStep` , em seguida, o tempo personalizada `WizardStep` é exibida a nova conta de usuário já tiver sido criada e o `CreatedUser` evento já foi disparado.

A Figura 21 mostra o fluxo de trabalho quando adicionado `WizardStep` precede o `CreateUserWizardStep`. Desde que as informações de usuário adicionais foram coletadas no momento a `CreatedUser` evento é acionado, só precisamos fazer é atualizar o `CreatedUser` manipulador de eventos para recuperar essas entradas e usá-las para o `INSERT` valores de parâmetro da instrução (em vez de `DBNull.Value`).


[![O fluxo de trabalho CreateUserWizard quando um WizardStep adicional precede o CreateUserWizardStep](storing-additional-user-information-vb/_static/image62.png)](storing-additional-user-information-vb/_static/image61.png)

**Figura 21**: O CreateUserWizard fluxo de trabalho quando um adicional `WizardStep` Precedes as `CreateUserWizardStep` ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image63.png))


Se o personalizado `WizardStep` é colocado *após* o `CreateUserWizardStep`, no entanto, o processo de conta de usuário de criação ocorre antes que o usuário tenha tido a oportunidade de inserir sua cidade, a home page ou a assinatura. Nesse caso, essa informação adicional precisa ser inserido no banco de dados depois que a conta de usuário tiver sido criada, como mostra a Figura 22.


[![O fluxo de trabalho CreateUserWizard quando um WizardStep adicional vem após o CreateUserWizardStep](storing-additional-user-information-vb/_static/image65.png)](storing-additional-user-information-vb/_static/image64.png)

**Figura 22**: O CreateUserWizard fluxo de trabalho quando um adicional `WizardStep` vem após o `CreateUserWizardStep` ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image66.png))


O fluxo de trabalho mostrado na Figura 22 aguarda para inserir um registro para o `UserProfiles` tabela até após a conclusão da etapa 2. Se o visitante fechar seu navegador após a etapa 1, no entanto, teremos atingido um estado em que uma conta de usuário foi criada, mas nenhum registro foi adicionado à `UserProfiles`. Uma solução alternativa é ter um registro com `NULL` ou padrão de valores inseridos nas `UserProfiles` no `CreatedUser` manipulador de eventos (que é acionado após a etapa 1) e atualize esse registro após a conclusão da etapa 2. Isso garante que um `UserProfiles` registro será adicionado para a conta de usuário, mesmo que o usuário fecha no processo de registro meio.

Para este tutorial vamos criar um novo `WizardStep` que ocorre após o `CreateUserWizardStep` , mas antes de `CompleteWizardStep`. Vamos primeiro obtenha o WizardStep em colocar e configurado e, em seguida, podemos examinar o código.

Na Smart Tag do controle CreateUserWizard, selecione o "Adicionar/remover `WizardStep` s", que abre o `WizardStep` caixa de diálogo do Editor de coleção. Adicione um novo `WizardStep`, definindo seu `ID` ao `UserSettings`, sua `Title` para "Configurações de seu" e seu `StepType` para `Step`. Em seguida, posicioná-lo para que ele vem depois o `CreateUserWizardStep` ("Sign Up for Your New Account") e antes do `CompleteWizardStep` ("concluído"), conforme mostrado na Figura 23.


[![Adicionar um novo WizardStep para o controle CreateUserWizard](storing-additional-user-information-vb/_static/image68.png)](storing-additional-user-information-vb/_static/image67.png)

**Figura 23**: adicionar um novo `WizardStep` para o controle CreateUserWizard ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image69.png))


Clique em Okey para fechar o `WizardStep` caixa de diálogo do Editor de coleção. O novo `WizardStep` é evidenciada pelo marcação declarativa do atualizada do controle CreateUserWizard:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample13.aspx)]

Observe o novo `<asp:WizardStep>` elemento. Precisamos adicionar a interface do usuário para coletar o novo usuário cidade, home page e assinatura aqui. Você pode inserir esse conteúdo na sintaxe declarativa ou por meio do Designer. Para usar o Designer, selecione a etapa de "Configurações de seu" na lista suspensa na marca inteligente para ver a etapa no Designer.

> [!NOTE]
> Seleção de uma etapa por meio na lista suspensa da marca inteligente de atualizações do controle CreateUserWizard [ `ActiveStepIndex` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.activestepindex.aspx), que especifica o índice da etapa inicial. Portanto, se você usar essa lista suspensa para editar a etapa de "Configurações de seu" no Designer, certifique-se para defini-lo para "Sign Up for Your New Account", para que essa etapa é exibido quando os usuários visitam primeiro o `EnhancedCreateUserWizard.aspx` página.


Criar uma interface do usuário dentro da etapa de "Configurações de seu" que contém três controles de caixa de texto denominados `HomeTown`, `HomepageUrl`, e `Signature`. Depois de construir essa interface, marcação declarativa do CreateUserWizard deve ser semelhante ao seguinte:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample14.aspx)]

Vá em frente e visitar esta página por meio de um navegador e criar uma nova conta de usuário, especificando valores para a cidade, a home page e a assinatura. Depois de concluir a `CreateUserWizardStep` a conta de usuário é criada na estrutura de associação e o `CreatedUser` execuções de manipulador de eventos, que adiciona uma nova linha à `UserProfiles`, mas com um banco de dados `NULL` valor para `HomeTown`, `HomepageUrl`, e `Signature`. Os valores inseridos para a cidade, a home page e a assinatura nunca são usados. O resultado é uma nova conta de usuário com uma `UserProfiles` registre cuja `HomeTown`, `HomepageUrl`, e `Signature` campos ainda precisam ser especificados.

É preciso executar código após a etapa de "Configurações de seu" que usa os valores de cidade, honepage e assinatura iniciais inseridos pelo usuário e atualiza o apropriada `UserProfiles` registro. Cada vez que o usuário move entre as etapas em um Assistente de controle, o assistente [ `ActiveStepChanged` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.wizard.activestepchanged.aspx) é acionado. Podemos criar um manipulador de eventos para esse evento e a atualização de `UserProfiles` tabela quando a etapa de "Configurações de seu" foi concluída.

Adicionar um manipulador de eventos para o CreateUserWizard `ActiveStepChanged` eventos e adicione o seguinte código:

[!code-vb[Main](storing-additional-user-information-vb/samples/sample15.vb)]

O código acima inicia determinando se alcançamos apenas a etapa de "Concluído". Como a etapa de "Concluído" ocorre imediatamente após a etapa de "Configurações de seu", Avançar quando atinge o visitante "Concluído" passo a passo que significa que ela acabou de concluir a etapa de "Configurações de seu".

Nesse caso, precisamos referenciar programaticamente os controles de caixa de texto dentro de `UserSettings WizardStep`. Isso é feito primeiro usando o `FindControl` método para referenciar programaticamente o `UserSettings WizardStep`e, em seguida, novamente para fazer referência a caixas de texto de dentro a `WizardStep`. Depois que as caixas de texto foram mencionadas, estamos prontos para executar o `UPDATE` instrução. O `UPDATE` instrução tem o mesmo número de parâmetros como o `INSERT` instrução no `CreatedUser` manipulador de eventos, mas aqui, usamos os valores de cidade, a home page e a assinatura iniciais fornecidos pelo usuário.

Com este manipulador de eventos em vigor, visite o `EnhancedCreateUserWizard.aspx` página por meio de um navegador e criar uma nova conta de usuário, especificando valores para a cidade, a home page e a assinatura. Depois de criar a nova conta, você deverá ser redirecionado para o `AdditionalUserInfo.aspx` página, onde as informações de cidade, a home page e a assinatura iniciais just-inserido são exibidas.

> [!NOTE]
> Nosso site atualmente tem duas páginas do qual um visitante pode criar uma nova conta: `CreatingUserAccounts.aspx` e `EnhancedCreateUserWizard.aspx`. Mapa do site do site e a página de logon apontam para o `CreatingUserAccounts.aspx` página, mas o `CreatingUserAccounts.aspx` página não solicitar ao usuário para suas informações de cidade, a home page e a assinatura iniciais e não adicionará uma linha correspondente à `UserProfiles`. Portanto, atualizar o `CreatingUserAccounts.aspx` página para que ele oferece essa funcionalidade ou atualizar a página de logon e o mapa do site para fazer referência a `EnhancedCreateUserWizard.aspx` em vez de `CreatingUserAccounts.aspx`. Se você escolher a última opção, certifique-se de atualizar o `Membership` da pasta `Web.config` arquivo de modo a permitir que usuários anônimos acessem o `EnhancedCreateUserWizard.aspx` página.


## <a name="summary"></a>Resumo

Neste tutorial vimos técnicas para modelar dados relacionados a contas de usuário dentro da estrutura de associação. Em particular, analisamos a modelagem de entidades que compartilham uma relação um-para-muitos com as contas de usuário, bem como os dados que compartilha uma relação um para um. Além disso, vimos como isso relacionados informações poderiam ser exibidas, inseridas e atualizadas, com alguns exemplos usando o controle SqlDataSource e outros usando o código do ADO.NET.

Esse tutorial conclui nossa aparência em contas de usuário. Começando com o próximo tutorial será transformamos nossas atenções para funções. Nos próximos vários tutoriais, vamos examinar a estrutura de funções, consulte como criar novas funções, como atribuir funções aos usuários, como determinar as funções que um usuário pertence e para aplicar a autorização baseada em função.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Acessar e atualizar dados no ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Controle Wizard 2.0 do ASP.NET](https://weblogs.asp.net/scottgu/archive/2006/02/21/438732.aspx)
- [Criando uma Interface do usuário passo a passo com o controle do ASP.NET 2.0 Assistente](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [Criando parâmetros de controle de fonte de dados personalizado](http://aspnet.4guysfromrolla.com/articles/110106-1.aspx)
- [Personalizando o controle CreateUserWizard](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx)
- [Guias de início rápido controle DetailsView](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/detailsview.aspx)
- [Exibindo dados com o controle ListView](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [Dissecando os controles de validação no ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)
- [Edição de inserção e exclusão de dados](../../data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)
- [Validação do formulário no ASP.NET](http://www.4guysfromrolla.com/webtech/090200-1.shtml)
- [Coletando informações de registro de usuário personalizada](https://weblogs.asp.net/scottgu/archive/2006/07/05/Tip_2F00_Trick_3A00_-Gathering-Custom-User-Registration-Information.aspx)
- [Perfis no ASP.NET 2.0](http://www.odetocode.com/Articles/440.aspx)
- [O controle asp: ListView](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)
- [Início rápido de perfis de usuário](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/profile/default.aspx)

### <a name="about-the-author"></a>Sobre o autor

Scott Mitchell, autor de vários livros sobre ASP/ASP.NET e fundador da 4GuysFromRolla.com, trabalha com tecnologias Web Microsoft desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é  *[Sams Teach por conta própria ASP.NET 2.0 em 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott pode ser contatado pelo [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) ou por meio de seu blog em [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a...

Esta série de tutoriais foi revisada por muitos revisores úteis. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Anterior](user-based-authorization-vb.md)
