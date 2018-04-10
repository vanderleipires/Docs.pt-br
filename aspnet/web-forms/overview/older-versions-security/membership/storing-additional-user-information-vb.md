---
uid: web-forms/overview/older-versions-security/membership/storing-additional-user-information-vb
title: Armazenando informações de usuário adicionais (VB) | Microsoft Docs
author: rick-anderson
description: Este tutorial é responderá essa pergunta criando um aplicativo muito rudimentares convidados. Dessa forma, examinaremos diferentes opções para modeli...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: ee4b924e-8002-4dc3-819f-695fca1ff867
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/storing-additional-user-information-vb
msc.type: authoredcontent
ms.openlocfilehash: 9a8673e764ae94b12fbc01f81ef12ea4c133b7d5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/10/2018
---
<a name="storing-additional-user-information-vb"></a>Armazenando informações de usuário adicionais (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_08_VB.zip) ou [baixar PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial08_ExtraUserInfo_vb.pdf)

> Este tutorial é responderá essa pergunta criando um aplicativo muito rudimentares convidados. Dessa forma, podemos examinar opções diferentes para modelagem de informações do usuário em um banco de dados e, em seguida, consulte como associar esses dados com as contas de usuário criadas pela estrutura de associação.


## <a name="introduction"></a>Introdução

AS PÁGINAS ASP. Estrutura de associação do NET oferece uma interface flexível de gerenciamento de usuários. A API de associação inclui métodos para validar as credenciais, recuperação de informações sobre o usuário conectado no momento, criar uma nova conta de usuário e excluir uma conta de usuário, entre outros. Cada conta de usuário na estrutura de associação contém apenas as propriedades necessárias para validar as credenciais e executar tarefas relacionadas à conta de usuário essenciais. Isso fica evidente, os métodos e propriedades do [ `MembershipUser` classe](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx), que modela uma conta de usuário na estrutura de associação. Essa classe tem propriedades como [ `UserName` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.username.aspx), [ `Email` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.email.aspx), e [ `IsLockedOut` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx), e métodos como [ `GetPassword` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.getpassword.aspx) e [ `UnlockUser` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx).

Muitas vezes, os aplicativos precisam armazenar informações de usuário adicionais não incluídas na estrutura de associação. Por exemplo, uma loja online, talvez seja necessário permitir que cada usuário armazene seus endereços de envio e de cobrança, informações de pagamento, as preferências de entrega e entre em contato com o número de telefone. Além disso, cada pedido no sistema está associado uma conta de usuário específica.

O `MembershipUser` classe não tem propriedades como `PhoneNumber` ou `DeliveryPreferences` ou `PastOrders`. Assim como controlar as informações de usuário necessitadas para o aplicativo e colocá-lo a integrar com o framework de associação? Este tutorial é responderá essa pergunta criando um aplicativo muito rudimentares convidados. Dessa forma, podemos examinar opções diferentes para modelagem de informações do usuário em um banco de dados e, em seguida, consulte como associar esses dados com as contas de usuário criadas pela estrutura de associação. Vamos começar!

## <a name="step-1-creating-the-guestbook-applications-data-model"></a>Etapa 1: Criando o modelo de dados do aplicativo de convidados

Há uma variedade de técnicas que podem ser utilizadas para capturar informações de usuário em um banco de dados e associá-lo com as contas de usuário criadas pela estrutura de associação. Para ilustrar as técnicas, precisaremos ampliar o aplicativo web tutorial para que ele capture algum tipo de dados relacionados ao usuário. (No momento, o modelo de dados do aplicativo contém apenas o aplicativo serviços tabelas necessárias para o `SqlMembershipProvider`.)

Vamos criar um aplicativo muito simples de convidados em que um usuário autenticado pode deixar um comentário. Além de armazenar comentários de convidados, vamos permitir que cada usuário armazenar sua cidade, a home page e a assinatura. Se fornecido, a cidade do usuário, home page e assinatura aparecerá em cada mensagem que ele tiver saído no livro de convidados.

### <a name="adding-theguestbookcommentstable"></a>Adicionando o`GuestbookComments`tabela

Para capturar os comentários de convidados, precisamos criar uma tabela de banco de dados denominada `GuestbookComments` que tem colunas como `CommentId`, `Subject`, `Body`, e `CommentDate`. Também precisamos ter cada registro no `GuestbookComments` o usuário que saiu do comentário de referência de tabela.

Para adicionar esta tabela para nosso banco de dados, vá para o Gerenciador de banco de dados no Visual Studio e Detalhar o `SecurityTutorials` banco de dados. Com o botão direito na pasta de tabelas e escolha Adicionar nova tabela. Isso abre uma interface que permite definir as colunas para a nova tabela.


[![Adicionar uma nova tabela no banco de dados SecurityTutorials](storing-additional-user-information-vb/_static/image2.png)](storing-additional-user-information-vb/_static/image1.png)

**Figura 1**: adicionar uma nova tabela para o `SecurityTutorials` banco de dados ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image3.png))


Em seguida, defina o `GuestbookComments`de colunas. Comece adicionando uma coluna denominada `CommentId` do tipo `uniqueidentifier`. Essa coluna será identificam exclusivamente cada comentário no livro de convidados, portanto não permitir `NULL` s e marcá-la como chave primária da tabela. Em vez de fornecer um valor para o `CommentId` campo em cada `INSERT`, pode indicar que um novo `uniqueidentifier` valor deve ser gerado automaticamente para esse campo em `INSERT` definindo o valor padrão da coluna como `NEWID()`. Depois de adicioná-lo primeiro, marcá-lo como a chave primária e as configurações de seu valor padrão, sua tela deve ser semelhante ao mostrado na Figura 2 de captura de tela.


[![Adicionar uma coluna principal chamada CommentId](storing-additional-user-information-vb/_static/image5.png)](storing-additional-user-information-vb/_static/image4.png)

**Figura 2**: adicionar uma coluna denominada primário `CommentId` ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image6.png))


Em seguida, adicione uma coluna denominada `Subject` do tipo `nvarchar(50)` e uma coluna denominada `Body` do tipo `nvarchar(MAX)`, desabilitação `NULL` s em ambas as colunas. Depois disso, adicione uma coluna denominada `CommentDate` do tipo `datetime`. Não permitir `NULL` s e defina o `CommentDate` valor padrão da coluna `getdate()`.

Tudo o que resta é para adicionar uma coluna que associa uma conta de usuário a cada comentário convidados. Uma opção seria adicionar uma coluna denominada `UserName` do tipo `nvarchar(256)`. Isso é uma escolha adequada ao usar um provedor de associação que o `SqlMembershipProvider`. Mas ao usar o `SqlMembershipProvider`, já que estamos na série tutorial, o `UserName` coluna o `aspnet_Users` tabela não é garantia de exclusividade. O `aspnet_Users` chave primária da tabela é `UserId` e é do tipo `uniqueidentifier`. Portanto, o `GuestbookComments` tabela precisa de uma coluna denominada `UserId` do tipo `uniqueidentifier` (desabilitação `NULL` valores). Vá em frente e adicione esta coluna.

> [!NOTE]
> Conforme abordado no [ *criar o esquema de associação no SQL Server* ](creating-the-membership-schema-in-sql-server-vb.md) tutorial, a estrutura de associação foi projetada para permitir que vários aplicativos web com diferentes contas de usuário compartilhar o mesmo repositório do usuário. Ele faz isso por meio do particionamento contas de usuário em diferentes aplicativos. E, enquanto cada nome de usuário é garantido para ser exclusivo dentro de um aplicativo, o mesmo nome de usuário pode ser usada em diferentes aplicativos usando o mesmo repositório do usuário. Há uma composição `UNIQUE` restrição no `aspnet_Users` tabela o `UserName` e `ApplicationId` campos, mas não uma em apenas o `UserName` campo. Consequentemente, é possível que o aspnet\_tabela de usuários para ter dois (ou mais) registros com o mesmo `UserName` valor. Há, no entanto, um `UNIQUE` restrição a `aspnet_Users` da tabela `UserId` campo (já que ela é a chave primária). Um `UNIQUE` restrição é importante porque sem ele, não é possível estabelecer uma restrição de chave estrangeira entre a `GuestbookComments` e `aspnet_Users` tabelas.


Depois de adicionar o `UserId` coluna, salve a tabela clicando no ícone Salvar na barra de ferramentas. Nomeie a nova tabela `GuestbookComments`.

Temos uma última saída necessária com a `GuestbookComments` tabela: é necessário criar um [restrição de chave estrangeira](https://msdn.microsoft.com/library/ms175464.aspx) entre o `GuestbookComments.UserId` coluna e o `aspnet_Users.UserId` coluna. Para fazer isso, clique no ícone de relação na barra de ferramentas para iniciar a caixa de diálogo relações de chave estrangeira. (Como alternativa, você pode iniciar esta caixa de diálogo, no menu Designer de tabela e escolha relações).

Clique no botão Adicionar no canto inferior esquerdo da caixa de diálogo relações de chave estrangeira. Isso adicionará uma nova restrição de chave estrangeira, embora ainda assim será preciso definir as tabelas que participam da relação.


[![Use a caixa de diálogo relações de chave estrangeira para gerenciar as restrições de chave estrangeira da tabela](storing-additional-user-information-vb/_static/image8.png)](storing-additional-user-information-vb/_static/image7.png)

**Figura 3**: Use a caixa de diálogo de relações de chave estrangeira para gerenciar as restrições de chave estrangeira da tabela ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image9.png))


Em seguida, clique no ícone de reticências na linha "Especificações de tabela e colunas" à direita. Isso abrirá a caixa de diálogo de tabelas e colunas, da qual podemos especificar a tabela de chave primária e a coluna e a coluna de chave estrangeira do `GuestbookComments` tabela. Em particular, selecione `aspnet_Users` e `UserId` como a tabela de chave primária e a coluna, e `UserId` do `GuestbookComments` tabela como coluna de chave estrangeira (consulte a Figura 4). Depois de definir as colunas e tabelas de chave primárias e estrangeiras, clique em Okey para retornar à caixa de diálogo relações de chave estrangeira.


[![Estabelecer uma Foreign Key restrição entre o aspnet_Users e GuesbookComments tabelas](storing-additional-user-information-vb/_static/image11.png)](storing-additional-user-information-vb/_static/image10.png)

**Figura 4**: estabelecer uma Foreign Key restrição entre o `aspnet_Users` e `GuesbookComments` tabelas ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image12.png))


Neste momento a restrição de chave estrangeira foi estabelecida. A presença dessa restrição garante [integridade relacional](http://en.wikipedia.org/wiki/Referential_integrity) entre as duas tabelas, garantindo que nunca haverá uma entrada de convidados referindo-se a uma conta de usuário inexistente. Por padrão, uma restrição de chave estrangeira não permitirá um registro pai a ser excluído se há registros filho correspondentes. Ou seja, se um usuário faz com que um ou mais comentários de convidados e, em seguida, podemos tentar excluir essa conta de usuário, a exclusão falhará a menos que os comentários de convidados são excluídos primeiro.

Restrições de chave estrangeira podem ser configuradas para excluir automaticamente os registros filho associado quando um registro pai é excluído. Em outras palavras, é possível configurar esta restrição de chave estrangeira para que as entradas do livro de visitas do usuário são excluídas automaticamente quando a conta de usuário é excluída. Para fazer isso, expanda a seção "Especificação de inserção e atualização" e defina a propriedade de "Excluir regra" em cascata.


[![Configurar a restrição de chave estrangeira para exclusões em cascata](storing-additional-user-information-vb/_static/image14.png)](storing-additional-user-information-vb/_static/image13.png)

**Figura 5**: configurar a restrição de chave estrangeira para exclusões em cascata ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image15.png))


Para salvar a restrição de chave estrangeira, clique no botão Fechar para sair as relações de chave estrangeira. Em seguida, clique no ícone Salvar na barra de ferramentas para salvar a tabela e essa relação.

### <a name="storing-the-users-home-town-homepage-and-signature"></a>Armazenar Home cidade, home page e assinatura do usuário

O `GuestbookComments` tabela ilustra como armazenar informações que compartilha uma relação um-para-muitos com contas de usuário. Como cada conta de usuário pode ter um número arbitrário de comentários associados, essa relação é modelada por meio da criação de uma tabela para armazenar o conjunto de comentários que inclui uma coluna que vincula cada comentário de volta para um usuário específico. Ao usar o `SqlMembershipProvider`, esse link for estabelecido melhor por meio da criação de uma coluna denominada `UserId` do tipo `uniqueidentifier` e uma restrição de chave estrangeira entre essa coluna e `aspnet_Users.UserId`.

Agora é preciso associar três colunas a cada conta de usuário para armazenar a assinatura, que será exibido nos seus comentários de convidados, home page e cidade do usuário. Não há número de diferentes maneiras de fazer isso:

- <strong>Adicionar novas colunas a</strong><strong>`aspnet_Users`</strong><strong>ou</strong><strong>`aspnet_Membership`</strong><strong>tabelas.</strong> Eu não recomendamos essa abordagem porque ela modifica o esquema usado pelo `SqlMembershipProvider`. Essa decisão pode voltar contra você no futuro. Por exemplo, se uma versão futura do ASP.NET usa outro `SqlMembershipProvider` esquema. Microsoft pode incluir uma ferramenta para migrar o ASP.NET 2.0 `SqlMembershipProvider` dados para o novo esquema, mas se você tiver modificado o ASP.NET 2.0 `SqlMembershipProvider` esquema, tal conversão pode não ser possível.

- **Use o ASP. Estrutura de perfil da rede, definindo uma propriedade de perfil para a cidade, a home page e a assinatura.** O ASP.NET inclui uma estrutura de perfil que foi projetada para armazenar dados específicos de usuário adicionais. Como o framework de associação, estrutura de perfil é criada sobre o modelo de provedor. O .NET Framework vem com um `SqlProfileProvider` que armazena dados em um banco de dados do SQL Server do perfil. Na verdade, nosso banco de dados já tem a tabela usada pelo `SqlProfileProvider` (`aspnet_Profile`), como ele foi adicionado ao adicionamos os serviços do aplicativo de volta a [ *criar o esquema de associação no SQL Server* ](creating-the-membership-schema-in-sql-server-vb.md)tutorial.   
  O principal benefício do framework perfil é que ele permite para desenvolvedores definir as propriedades de perfil no `Web.config` – nenhum código precisa ser gravada para serializar os dados de perfil de e para o armazenamento de dados subjacente. Em resumo, é extremamente fácil para definir um conjunto de propriedades de perfil e trabalhar com eles no código. No entanto, o sistema de perfil deixa muito a desejar quando se trata de controle de versão, portanto, se você tiver um aplicativo em que você espera que novas propriedades específicas do usuário a ser adicionado em um momento posterior, ou arquivos existentes sejam removidas ou modificadas, em seguida, a estrutura de perfil não pode ser o  melhor opção. Além disso, o `SqlProfileProvider` armazena as propriedades do perfil de maneira altamente desnormalizada, facilitando praticamente impossível executar consultas diretamente os dados de perfil (por exemplo, quantos usuários têm uma cidade de Nova York inicial).   
  Para obter mais informações sobre a estrutura de perfil, consulte a seção "Leituras adicionais" no final deste tutorial.

- <strong>Adicione essas três colunas em uma nova tabela no banco de dados e estabelecer uma relação um para um entre essa tabela e</strong><strong>`aspnet_Users`</strong><strong>.</strong> Essa abordagem envolve um pouco mais trabalho do que com a estrutura de perfil, mas oferece a máxima flexibilidade em como as propriedades de usuário adicionais são modeladas no banco de dados. Essa é a opção que vamos usar neste tutorial.

Vamos criar uma nova tabela chamada `UserProfiles` para salvar a cidade, a home page e a assinatura para cada usuário. Com o botão direito na pasta de tabelas na janela do Gerenciador de banco de dados e optar por criar uma nova tabela. Nome da primeira coluna `UserId` e defina seu tipo como `uniqueidentifier`. Não permitir `NULL` valores e marque a coluna como uma chave primária. Em seguida, adicione colunas nomeadas: `HomeTown` do tipo `nvarchar(50)`; `HomepageUrl` do tipo `nvarchar(100)`; e assinatura do tipo `nvarchar(500)`. Cada uma dessas três colunas pode aceitar um `NULL` valor.


[![Criar a tabela de UserProfiles](storing-additional-user-information-vb/_static/image17.png)](storing-additional-user-information-vb/_static/image16.png)

**Figura 6**: criar o `UserProfiles` tabela ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image18.png))


Salve a tabela e nomeie-o `UserProfiles`. Por fim, estabelecer uma restrição de chave estrangeira entre a `UserProfiles` da tabela `UserId` campo e o `aspnet_Users.UserId` campo. Como foi feito com a restrição de chave estrangeira entre a `GuestbookComments` e `aspnet_Users` tabelas, têm essa restrição Propagar exclusões. Como o `UserId` campo `UserProfiles` é o primário chave, isso garante que haverá não mais de um registro no `UserProfiles` tabela para cada conta de usuário. Esse tipo de relação é chamado como um para um.

Agora que temos o modelo de dados criado, você está pronto para usá-lo. Nas etapas 2 e 3 examinaremos como o usuário conectado no momento pode exibir e editar suas informações de cidade, a home page e a assinatura principal. Na etapa 4, criaremos a interface para usuários autenticados enviar novos comentários ao livro de convidados e exibir os existentes.

## <a name="step-2-displaying-the-users-home-town-homepage-and-signature"></a>Etapa 2: Exibir Home cidade, home page e assinatura do usuário

Há várias maneiras para permitir que o usuário conectado no momento exibir e editar suas informações de cidade, a home page e a assinatura principal. Foi possível criar manualmente a interface do usuário com a caixa de texto e controles de rótulo ou use um dos dados de controles da Web, como o controle DetailsView. Para executar o banco de dados `SELECT` e `UPDATE` instruções poderíamos escrever ADO.NET na classe de code-behind da nossa página de código ou, Alternativamente, empregam uma abordagem declarativa com SqlDataSource. O ideal é nosso aplicativo conteria uma arquitetura em camadas, que podemos ou pode invocar por meio de programação de classe de code-behind da página ou declarativamente por meio do controle ObjectDataSource.

Desde que esta série de tutorial concentra-se na autenticação de formulários, autorização, contas de usuário e funções, não haverá uma discussão completa sobre essas opções de acesso de dados diferente ou por uma arquitetura hierárquica é preferível executar instruções SQL diretamente Na página ASP.NET. Vou percorrer usando um DetailsView e SqlDataSource – a opção mais rápida e fácil – mas os conceitos abordados certamente podem ser aplicados a alternativa lógica de acesso dados e controles da Web. Para obter mais informações sobre como trabalhar com dados no ASP.NET, consulte o meu *[trabalhando com dados no ASP.NET 2.0](../../data-access/index.md)* série de tutoriais.

Abra o `AdditionalUserInfo.aspx` página o `Membership` pasta e adicionar um controle DetailsView para a página, definindo sua propriedade ID como `UserProfile` e limpar seu `Width` e `Height` propriedades. Expanda a marca inteligente de DetailsView e escolha para associá-lo a um novo controle de fonte de dados. Isso iniciará o Assistente de configuração de fonte de dados (consulte a Figura 7). A primeira etapa solicitará que você especifique o tipo de fonte de dados. Já que vamos se conectar diretamente ao `SecurityTutorials` banco de dados, escolha o ícone de banco de dados, especificando o `ID` como `UserProfileDataSource`.


[![Adicionar um novo controle SqlDataSource denominado UserProfileDataSource](storing-additional-user-information-vb/_static/image20.png)](storing-additional-user-information-vb/_static/image19.png)

**Figura 7**: adicionar um controle SqlDataSource novo chamado `UserProfileDataSource` ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image21.png))


A próxima tela solicita que o banco de dados a ser usado. Nós já definiu uma cadeia de caracteres de conexão em `Web.config` para o `SecurityTutorials` banco de dados. Esse nome de cadeia de caracteres de conexão – `SecurityTutorialsConnectionString` – deve estar na lista suspensa. Selecione esta opção e clique em Avançar.


[![Escolha SecurityTutorialsConnectionString na lista suspensa](storing-additional-user-information-vb/_static/image23.png)](storing-additional-user-information-vb/_static/image22.png)

**Figura 8**: escolha `SecurityTutorialsConnectionString` na lista suspensa ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image24.png))


A tela subsequente pede para especificar a tabela e colunas à consulta. Escolha o `UserProfiles` da tabela na lista suspensa e verificar todas as colunas.


[![Trazer de volta todas as colunas da tabela UserProfiles](storing-additional-user-information-vb/_static/image26.png)](storing-additional-user-information-vb/_static/image25.png)

**Figura 9**: colocar novamente todas as colunas do `UserProfiles` tabela ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image27.png))


A consulta atual na Figura 9 retorna *todos os* dos registros em `UserProfiles`, mas só estamos interessados no registro do usuário conectado no momento. Para adicionar um `WHERE` cláusula, clique o `WHERE` botão para exibir a adicionar `WHERE` caixa de diálogo de cláusula (consulte a Figura 10). Aqui você pode selecionar a coluna para filtrar, o operador e a origem do parâmetro de filtro. Selecione `UserId` como a coluna e "=" como o operador.

Infelizmente não há nenhuma fonte de parâmetro internas para retornar o usuário conectado no momento `UserId` valor. Precisamos obter esse valor por meio de programação. Portanto, defina a lista suspensa de origem para "None", clique em Adicionar botão para adicionar o parâmetro e, em seguida, clique em Okey.


[![Adicionar um parâmetro de filtro na coluna de ID de usuário](storing-additional-user-information-vb/_static/image29.png)](storing-additional-user-information-vb/_static/image28.png)

**Figura 10**: adicionar um parâmetro de filtro de `UserId` coluna ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image30.png))


Depois de clicar em Okey, você voltará para a tela mostrada na Figura 9. Neste momento, no entanto, a consulta SQL na parte inferior da tela deve incluir um `WHERE` cláusula. Clique em Avançar para ir para a tela de "Consulta de teste". Aqui você pode executar a consulta e ver os resultados. Clique em Concluir para concluir o assistente.

Após concluir o Assistente de configuração de fonte de dados, o Visual Studio cria o controle SqlDataSource com base nas configurações especificadas no assistente. Além disso, ele adiciona manualmente BoundFields a DetailsView para cada coluna retornada pelo SqlDataSource `SelectCommand`. Não é necessário para mostrar o `UserId` campo a DetailsView, desde que o usuário não precisa saber esse valor. Você pode remover este campo diretamente de marcação declarativa do controle DetailsView ou clicando em "Editar campos" link de marca inteligente.

Neste ponto declarativo da página deve ser semelhante ao seguinte:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample1.aspx)]

É preciso definir programaticamente do controle SqlDataSource `UserId` parâmetro para o usuário conectado no momento `UserId` antes que os dados são selecionados. Isso pode ser feito por meio da criação de um manipulador de eventos para o SqlDataSource `Selecting` eventos e adicionando o seguinte código existe:

[!code-vb[Main](storing-additional-user-information-vb/samples/sample2.vb)]

O código acima começa obtendo uma referência para o usuário conectado no momento chamando o `Membership` da classe `GetUser` método. Isso retorna um `MembershipUser` objeto cujo `ProviderUserKey` propriedade contém o `UserId`. O `UserId` valor é então atribuído para o SqlDataSource `@UserId` parâmetro.

> [!NOTE]
> O `Membership.GetUser()` método retorna informações sobre o usuário conectado no momento. Se um usuário anônimo está visitando a página, ela retornará um valor de `Nothing`. Nesse caso, isso levará a uma `NullReferenceException` na linha seguinte do código ao tentar ler o `ProviderUserKey` propriedade. Obviamente, não temos preocupar `Membership.GetUser()` retornando nada a `AdditionalUserInfo.aspx` página porque configuramos autorização de URL em um tutorial anterior para que somente usuários autenticados podem acessar os recursos do ASP.NET nesta pasta. Se você precisar acessar informações sobre o usuário conectado no momento em uma página em que o acesso anônimo é permitido, certifique-se de verificar se o `MembershipUser` objeto retornado do `GetUser()` método não é nada antes de fazer referência a suas propriedades.


Se você visitar o `AdditionalUserInfo.aspx` página através de um navegador você verá uma página em branco porque ainda precisamos adicionar linhas a serem o `UserProfiles` tabela. Na etapa 6, examinaremos como personalizar o controle CreateUserWizard para adicionar automaticamente uma nova linha para o `UserProfiles` tabela quando uma nova conta de usuário é criada. Agora, no entanto, precisaremos criar manualmente um registro na tabela.

Navegue até o Gerenciador de banco de dados no Visual Studio e expanda a pasta de tabelas. Clique com botão direito no `aspnet_Users` tabela e escolha "Mostrar dados da tabela" para ver os registros na tabela; fazer o mesmo o `UserProfiles` tabela. A Figura 11 mostra esses resultados ao lado a lado verticalmente. No meu banco de dados existem atualmente `aspnet_Users` registros para Bruce Fred e Tito, mas nenhum registro de `UserProfiles` tabela.


[![O conteúdo a aspnet_Users e UserProfiles tabelas são exibidas](storing-additional-user-information-vb/_static/image32.png)](storing-additional-user-information-vb/_static/image31.png)

**Figura 11**: O conteúdo do `aspnet_Users` e `UserProfiles` tabelas são exibidas ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image33.png))


Adicione um novo registro para o `UserProfiles` tabela digitando manualmente os valores para o `HomeTown`, `HomepageUrl`, e `Signature` campos. A maneira mais fácil de obter um válido `UserId` valor no novo `UserProfiles` registro é selecionar o `UserId` campo de uma conta de usuário específico no `aspnet_Users` de tabela e copie e cole-o no `UserId` campo `UserProfiles`. A Figura 12 mostra o `UserProfiles` depois da adição de um novo registro para Bruce de tabela.


[![Um registro foi adicionado para UserProfiles Bruce](storing-additional-user-information-vb/_static/image35.png)](storing-additional-user-information-vb/_static/image34.png)

**Figura 12**: um registro foi adicionado à `UserProfiles` para Bruce ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image36.png))


Volte para o `AdditionalUserInfo.aspx page`, conectado como Bruce. Como mostra a Figura 13, configurações de Bruce são exibidas.


[![O usuário no momento visitando é mostrado His configurações](storing-additional-user-information-vb/_static/image38.png)](storing-additional-user-information-vb/_static/image37.png)

**Figura 13**: O momento visitando usuário é mostrado His configurações ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image39.png))


> [!NOTE]
> Vá em frente e manualmente adicionar registros de `UserProfiles` tabela para cada usuário da associação. Na etapa 6, examinaremos como personalizar o controle CreateUserWizard para adicionar automaticamente uma nova linha para o `UserProfiles` tabela quando uma nova conta de usuário é criada.


## <a name="step-3-allowing-the-user-to-edit-his-home-town-homepage-and-signature"></a>Etapa 3: Permitindo que o usuário edite sua Home cidade, home page e assinatura

Agora o usuário conectado no momento pode exibir seus cidade, a home page e a configuração de assinatura, mas eles ainda não é possível modificá-las. Vamos atualizar o controle DetailsView para que os dados podem ser editados.

A primeira coisa que precisamos fazer é adicionar um `UpdateCommand` para SqlDataSource, especificando o `UPDATE` instrução a ser executada e seus parâmetros correspondentes. Selecione o SqlDataSource e, na janela Propriedades, clique na reticências ao lado da propriedade UpdateQuery para abrir a caixa de diálogo Editor de comando e parâmetro. Digite o seguinte `UPDATE` instrução na caixa de texto:

[!code-sql[Main](storing-additional-user-information-vb/samples/sample3.sql)]

Em seguida, clique no botão "Atualizar parâmetros", que criará um parâmetro no controle SqlDataSource do `UpdateParameters` cada um dos parâmetros na coleção de `UPDATE` instrução. Deixar a fonte para todos os parâmetros para nenhuma e clique no botão Okey para concluir a caixa de diálogo.


[![Especifique o SqlDataSource UpdateCommand e UpdateParameters](storing-additional-user-information-vb/_static/image41.png)](storing-additional-user-information-vb/_static/image40.png)

**Figura 14**: especifique o SqlDataSource `UpdateCommand` e `UpdateParameters` ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image42.png))


Devido a adições fizemos ao controle SqlDataSource, o controle pode oferecer suporte a edição de DetailsView. De marca inteligente de DetailsView, marque a caixa de seleção "Habilitar edição". Isso adiciona um CommandField para o controle `Fields` coleção com seu `ShowEditButton` propriedade definida como True. Isso apresenta um botão de edição quando DetailsView é exibido no modo somente leitura e atualização e botões Cancelar quando exibido no modo de edição. Em vez de exigir que o usuário clique em Editar, no entanto, podemos ter a renderização de DetailsView em um estado "sempre editável" definindo o controle de DetailsView [ `DefaultMode` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode.aspx) para `Edit`.

Com essas alterações, declarativo do seu controle DetailsView deve ser semelhante ao seguinte:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample4.aspx)]

Observe a adição do CommandField e `DefaultMode` propriedade.

Vá em frente e testar esta página por meio de um navegador. Ao visitar um usuário que tem um registro correspondente na `UserProfiles`, as configurações do usuário são exibidas em uma interface editável.


[![O DetailsView renderiza uma Interface editável](storing-additional-user-information-vb/_static/image44.png)](storing-additional-user-information-vb/_static/image43.png)

**Figura 15**: DetailsView renderiza uma Interface editável ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image45.png))


Tente alterar os valores e clicando no botão de atualização. Ele aparece como se nada acontece. Há um postback e os valores são salvos no banco de dados, mas não há nenhum comentários visuais que salvar ocorreu.

Para corrigir este problema, retorne ao Visual Studio e adicionar um controle de rótulo acima DetailsView. Definir seu `ID` para `SettingsUpdatedMessage`, sua `Text` propriedade como "suas configurações foram atualizadas," e seu `Visible` e `EnableViewState` propriedades `False`.

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample5.aspx)]

É necessário exibir o `SettingsUpdatedMessage` rótulo sempre que o DetailsView é atualizado. Para fazer isso, crie um manipulador de eventos para o DetailsView `ItemUpdated` evento e adicione o seguinte código:

[!code-vb[Main](storing-additional-user-information-vb/samples/sample6.vb)]

Volte para o `AdditionalUserInfo.aspx` página através de um navegador e atualizar os dados. Neste momento, será exibida uma mensagem de status úteis.


[![Uma mensagem curta é exibido quando as configurações são atualizadas](storing-additional-user-information-vb/_static/image47.png)](storing-additional-user-information-vb/_static/image46.png)

**Figura 16**: uma mensagem curta é exibida quando as configurações são atualizadas ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image48.png))


> [!NOTE]
> O controle DetailsView de edição do interface deixa muito a desejar. Ele usa o tamanho padrão caixas de texto, mas o campo de assinatura provavelmente deve ser uma caixa de texto de várias linha. Um RegularExpressionValidator deve ser usado para garantir que a URL da home page, se inserido, comece com "http://" ou "https://". Além disso, desde que o controle tem de DetailsView seu `DefaultMode` propriedade definida como `Edit`, o botão Cancelar não faz nada. -Um deve ser removido ou, quando clicado, redirecione o usuário para alguma outra página (como `~/Default.aspx`). Deixe esses melhorias como um exercício para o leitor.


### <a name="adding-a-link-to-theadditionaluserinfoaspxpage-in-the-master-page"></a>Adicionar um Link para o`AdditionalUserInfo.aspx`página na página mestra

No momento, o site não fornece todos os links para o `AdditionalUserInfo.aspx` página. É a única maneira de alcançá-lo inserir a URL da página diretamente na barra de endereços do navegador. Vamos adicionar um link para esta página no `Site.master` página mestra.

Lembre-se de que a página mestra contém um controle LoginView Web no seu `LoginContent` ContentPlaceHolder que exibe a marcação diferente para os visitantes anônimos e autenticados. Atualização do controle LoginView `LoggedInTemplate` para incluir um link para o `AdditionalUserInfo.aspx` página. Após fazer essas alterações a LoginView declarativo do controle deve ser semelhante ao seguinte:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample7.aspx)]

Observe a adição do `lnkUpdateSettings` controle de hiperlink para o `LoggedInTemplate`. Com este link no local, os usuários autenticados podem ir rapidamente para a página para exibir e modificar as configurações de cidade, a home page e a assinatura inicial.

## <a name="step-4-adding-new-guestbook-comments"></a>Etapa 4: Adicionando novos comentários de convidados

O `Guestbook.aspx` página é onde os usuários autenticados podem exibir o registro de visitantes e deixar um comentário. Vamos começar a criar a interface para adicionar novos comentários de convidados.

Abra o `Guestbook.aspx` página no Visual Studio e construir uma interface de usuário que consiste em dois controles de caixa de texto, uma para o assunto do comentário novo e outra para seu corpo. Definir o controle de caixa de texto primeiro `ID` propriedade `Subject` e sua `Columns` propriedade para 40; do segundo conjunto `ID` para `Body`, sua `TextMode` para `MultiLine`e sua `Width` e `Rows` Propriedades "95%" e 8, respectivamente. Para concluir a interface do usuário, adicione um controle de botão Web chamado `PostCommentButton` e defina seu `Text` propriedade como "Post seu comentário".

Como cada comentário convidados requer um assunto e corpo, adicione um controle RequiredFieldValidator para cada uma das caixas de texto. Definir o `ValidationGroup` propriedade desses controles para "EnterComment"; da mesma forma, defina o `PostCommentButton` do controle `ValidationGroup` propriedade como "EnterComment". Para obter mais informações sobre o ASP. Controles de validação do NET, check-out [validação do formulário no ASP.NET](http://www.4guysfromrolla.com/webtech/090200-1.shtml), [dissecando os controles de validação no ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)e o [Tutorial de controles de servidor de validação](http://www.w3schools.com/aspnet/aspnet_refvalidationcontrols.asp) em [W3Schools](http://www.w3schools.com/).

Depois de criar a interface do usuário declarativo da página deve ser semelhante ao seguinte:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample8.aspx)]

Com a interface de usuário completa, nossa próxima tarefa é inserir um novo registro para o `GuestbookComments` tabela quando o `PostCommentButton` é clicado. Isso pode ser feito de várias maneiras: podemos escrever código ADO.NET do botão `Click` manipulador de eventos; pode adicionar um controle SqlDataSource para a página, configure seu `InsertCommand`e, em seguida, chame seu `Insert` método do `Click` evento manipulador; é possível criar uma camada intermediária que era responsável por inserir novos comentários de convidados e chamar essa funcionalidade no `Click` manipulador de eventos. Como vimos usando um SqlDataSource na etapa 3, vamos usar código ADO.NET aqui.

> [!NOTE]
> As classes ADO.NET usadas para acessar dados programaticamente de um banco de dados do Microsoft SQL Server estão localizadas no `System.Data.SqlClient` namespace. Talvez seja necessário importar esse namespace para a classe de code-behind da página (ou seja, `Imports System.Data.SqlClient`).


Criar um manipulador de eventos para o `PostCommentButton`do `Click` evento e adicione o seguinte código:

[!code-vb[Main](storing-additional-user-information-vb/samples/sample9.vb)]

O `Click` manipulador de eventos é iniciado, verificando se os dados fornecidos pelo usuário são válidos. Se não for, o manipulador de eventos será encerrado antes de inserir um registro. Supondo que os dados fornecidos são válidos, o usuário conectado no momento `UserId` valor é recuperado e armazenado no `currentUserId` variável local. Esse valor é necessário porque é necessário fornecer um `UserId` valor ao inserir um registro em `GuestbookComments`.

Depois disso, a conexão de cadeia de caracteres para o `SecurityTutorials` banco de dados é recuperado do `Web.config` e `INSERT` instrução SQL é especificada. Um `SqlConnection` objeto é criado e aberto. Em seguida, um `SqlCommand` objeto é construído e os valores para os parâmetros usados no `INSERT` da consulta são atribuídos. O `INSERT` instrução será executada e a conexão é fechada. No final do manipulador de eventos, o `Subject` e `Body` caixas de texto `Text` propriedades são limpo para que os valores do usuário não são persistidos em postback.

Vá em frente e testar esta página em um navegador. Como essa página está no `Membership` pasta não está acessível aos visitantes anônimos. Portanto, você precisará primeiro fazer logon (se você ainda não tiver feito). Insira um valor para o `Subject` e `Body` caixas de texto e clique no `PostCommentButton` botão. Isso fará com que um novo registro a ser adicionado ao `GuestbookComments`. Em um postback, o assunto e corpo fornecidos são apagados de caixas de texto.

Depois de clicar no `PostCommentButton` botão lá está sem comentários visuais que o comentário foi adicionado ao livro de convidados. Ainda é preciso atualizar esta página para exibir os comentários de convidados existente, o que faremos na etapa 5. Depois que podemos fazer isso, o comentário adicionado apenas será exibida na lista de comentários, fornecendo feedback visual adequado. Por enquanto, confirme que o seu comentário convidados foi salvo examinando o conteúdo do `GuestbookComments` tabela.

A Figura 17 mostra o conteúdo do `GuestbookComments` após dois comentários foram deixados de tabela.


[![Você pode ver os comentários de convidados na tabela GuestbookComments](storing-additional-user-information-vb/_static/image50.png)](storing-additional-user-information-vb/_static/image49.png)

**Figura 17**: você pode ver os comentários de convidados no `GuestbookComments` tabela ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image51.png))


> [!NOTE]
> Se um usuário tenta inserir um comentário de convidados que contém potencialmente perigosa marcação – como HTML – ASP.NET lançará um `HttpRequestValidationException`. Para saber mais sobre esta exceção, por que é gerada, e como permitir que os usuários enviem valores potencialmente perigosos, consulte o [white paper de validação de solicitação](../../../../whitepapers/request-validation.md).


## <a name="step-5-listing-the-existing-guestbook-comments"></a>Etapa 5: Listando os comentários de convidados existente

Além de deixar comentários, um usuário visitar o `Guestbook.aspx` página também deve ser capaz de exibir comentários existentes do livro de convidados. Para fazer isso, adicione um controle ListView chamado `CommentList` até a parte inferior da página.

> [!NOTE]
> O controle ListView é novo para o ASP.NET versão 3.5. Ele é projetado para exibir uma lista de itens em um layout muito personalizável e flexível, mas ainda oferecer internos edição, inserir, excluir, paginação e classificando funcionalidades como GridView. Se você estiver usando o ASP.NET 2.0, você precisará usar o controle DataList ou repetidor em vez disso. Para obter mais informações sobre como usar o ListView, consulte [Scott Guthrie](https://weblogs.asp.net/scottgu/)da entrada de blog, [controle asp: ListView](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)e o artigo [exibindo dados com o controle ListView](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx).


Abra a marca inteligente de ListView e, na lista suspensa Escolher fonte de dados, associar o controle a uma nova fonte de dados. Como vimos na etapa 2, isso iniciará o Assistente de configuração de fonte de dados. Selecione o ícone de banco de dados, nomeie o SqlDataSource resultante `CommentsDataSource`e clique em Okey. Em seguida, selecione a `SecurityTutorialsConnectionString` conexão de cadeia de caracteres da lista suspensa e clique em Avançar.

Nesse ponto na etapa 2 especificamos os dados para consulta selecionando o `UserProfiles` tabela da lista suspensa e selecionando as colunas retornem (consulte novamente a Figura 9). Neste momento, no entanto, você deseja criar uma instrução SQL que extrai não apenas os registros de `GuestbookComments`, mas também do autor do comentário cidade, home page, assinatura e nome de usuário. Portanto, selecione o botão de opção "Especificar uma instrução SQL personalizada ou procedimento armazenado" e clique em Avançar.

Isso abrirá a tela "Definir personalizado instruções ou procedimentos armazenados". Clique no botão Construtor de consultas para criar graficamente a consulta. Inicia o construtor de consultas, solicitando que você especifique as tabelas que deseja consultar na. Selecione o `GuestbookComments`, `UserProfiles`, e `aspnet_Users` tabelas e clique em Okey. Isso adicionará todas as três tabelas à superfície de design. Como há restrições de chave estrangeira entre a `GuestbookComments`, `UserProfiles`, e `aspnet_Users` tabelas, o construtor de consultas automaticamente `JOIN` s essas tabelas.

Tudo o que resta é para especificar as colunas para retornar. Do `GuestbookComments` select da tabela de `Subject`, `Body`, e `CommentDate` colunas; return a `HomeTown`, `HomepageUrl`, e `Signature` colunas do `UserProfiles` tabela; e retornar `UserName` de `aspnet_Users`. Além disso, adicione "`ORDER BY CommentDate DESC`" ao final do `SELECT` consulta para que as postagens mais recentes são retornadas primeiro. Depois de fazer essas seleções, sua interface do construtor de consultas deve ser semelhante para a tela na Figura 18.


[![A consulta Constructed une as tabelas de aspnet_Users, UserProfiles e GuestbookComments](storing-additional-user-information-vb/_static/image53.png)](storing-additional-user-information-vb/_static/image52.png)

**Figura 18**: A consulta construída `JOIN` s a `GuestbookComments`, `UserProfiles`, e `aspnet_Users` tabelas ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image54.png))


Clique Okey para fechar a janela do construtor de consulta e retornar à tela "Definir personalizado instruções ou procedimentos armazenados". Clique em Avançar para a tela de "Consulta de teste", onde você pode exibir os resultados da consulta clicando no botão consulta de teste. Quando estiver pronto, clique em Concluir para concluir o Assistente Configurar fonte de dados.

Quando é concluído o Assistente Configurar fonte de dados do etapa 2, o controle DetailsView associado `Fields` coleção foi atualizada para incluir um BoundField para cada coluna retornada pelo `SelectCommand`. No entanto, ListView, permanece inalterado; ainda é preciso definir seu layout. O layout de ListView pode ser construído manualmente por meio de sua marcação declarativa ou a opção "Configurar ListView" na sua marca inteligente. Geralmente, prefira definindo a marcação manualmente, mas use qualquer método que é mais natural.

Acabei usando o seguinte `LayoutTemplate`, `ItemTemplate`, e `ItemSeparatorTemplate` para o controle ListView:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample10.aspx)]

O `LayoutTemplate` define a marcação emitidos pelo controle, enquanto o `ItemTemplate` renderiza cada item retornado por SqlDataSource. O `ItemTemplate`da marcação resultante será colocada no `LayoutTemplate`do `itemPlaceholder` controle. Além de `itemPlaceholder`, o `LayoutTemplate` inclui um controle DataPager, o que limita a ListView para mostrar apenas 10 comentários do livro de visitas por página (o padrão) e apresenta uma interface de paginação.

Meu `ItemTemplate` exibe o assunto do comentário de cada livro de convidados em um `<h4>` elemento com o corpo situado abaixo o assunto. Observe que a sintaxe usada para exibir o corpo leva os dados retornados pelo `Eval("Body")` instrução de associação de dados, converte-o em uma cadeia de caracteres e as quebras de linha substitui com o `<br />` elemento. Essa conversão é necessária para mostrar as quebras de linha inseridas ao enviar o comentário, desde que o espaço em branco é ignorado por HTML. Assinatura do usuário é exibida sob o corpo em itálico, seguido cidade do usuário, um link para sua home page, a data e hora em que o comentário foi feito e o nome de usuário da pessoa que saiu do comentário.

Dedicar um tempo para exibir a página por meio de um navegador. Você deve ver os comentários que você adicionou ao livro de convidados na etapa 5 exibidos aqui.


[![GuestBook agora exibe comentários do livro de convidados](storing-additional-user-information-vb/_static/image56.png)](storing-additional-user-information-vb/_static/image55.png)

**Figura 19**: `Guestbook.aspx` agora exibe comentários do livro de convidados ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image57.png))


Tente adicionar um novo comentário ao livro de convidados. Após clicar no `PostCommentButton` botão página envia de volta e o comentário é adicionado ao banco de dados, mas o controle ListView não é atualizado para mostrar o novo comentário. Isso pode ser corrigido pelo:

- Atualizando o `PostCommentButton` do botão `Click` manipulador de eventos para que ele invoca o controle de ListView `DataBind()` método depois de inserir o novo comentário para o banco de dados, ou
- Configuração do controle ListView `EnableViewState` propriedade `False`. Essa abordagem funciona porque desabilitando o estado de exibição do controle, ele deve associar novamente os dados subjacentes em cada postback.

O site do tutorial para download deste tutorial ilustra ambas as técnicas. Do controle ListView `EnableViewState` propriedade `False` e o código necessário para reassociar programaticamente os dados para ListView está presente no `Click` manipulador de eventos, mas está marcado como ignorado.

> [!NOTE]
> Atualmente o `AdditionalUserInfo.aspx` página permite que o usuário exibir e editar suas configurações de cidade, a home page e a assinatura inicial. Talvez seja adequado atualizar `AdditionalUserInfo.aspx` para exibir o log de comentários do livro de visitas do usuário. Ou seja, além de examinar e modificar suas informações, um usuário pode visitar o `AdditionalUserInfo.aspx` para ver quais convidados comentários, ela é feita no passado. Posso deixar isso como um exercício para o leitor de interesse.


## <a name="step-6-customizing-the-createuserwizard-control-to-include-an-interface-for-the-home-town-homepage-and-signature"></a>Etapa 6: Personalizar o controle CreateUserWizard para incluir uma Interface para a cidade de casa, home page e assinatura

O `SELECT` consulta usada pelo `Guestbook.aspx` página usa um `INNER JOIN` para combinar os registros relacionados entre o `GuestbookComments`, `UserProfiles`, e `aspnet_Users` tabelas. Se um usuário que não tem registro de `UserProfiles` faz uma convidados de comentário, o comentário não será exibido em ListView, porque o `INNER JOIN` retorna apenas `GuestbookComments` registros quando há registros correspondentes em `UserProfiles` e `aspnet_Users`. E, conforme vimos na etapa 3, se um usuário não tem um registro em `UserProfiles` ela não é possível exibir ou editar suas configurações no `AdditionalUserInfo.aspx` página.

Obviamente, é importante que cada conta de usuário no sistema de associação tem uma correspondência de decisões devido ao nosso design registram no `UserProfiles` tabela. O que queremos é um registro correspondente a ser adicionado ao `UserProfiles` sempre que uma nova conta de usuário de associação é criada por meio de CreateUserWizard.

Como discutido o [ *criando contas de usuário* ](creating-user-accounts-vb.md) tutorial, depois que a nova conta de usuário de associação é criada o controle CreateUserWizard gera seu [ `CreatedUser` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx). Podemos criar um manipulador de eventos para esse evento, obter a ID de usuário para o usuário recém-criada e, em seguida, inserir um registro para o `UserProfiles` tabela com valores padrão para o `HomeTown`, `HomepageUrl`, e `Signature` colunas. Além disso, é possível solicitar ao usuário para esses valores, personalizando a interface do controle CreateUserWizard para incluir caixas de texto adicionais.

Vamos primeiro examinar como adicionar uma nova linha para o `UserProfiles` tabela o `CreatedUser` manipulador de eventos com valores padrão. Depois disso, veremos como personalizar a interface do usuário do controle CreateUserWizard para incluir campos adicionais para coletar o novo usuário cidade, home page e assinatura.

### <a name="adding-a-default-row-touserprofiles"></a>Adicionar uma linha padrão`UserProfiles`

No [ *criando contas de usuário* ](creating-user-accounts-vb.md) tutorial adicionamos um controle CreateUserWizard para o `CreatingUserAccounts.aspx` página o `Membership` pasta. Para ter CreateUserWizard controle Adicionar um registro a `UserProfiles` tabela após a criação da conta de usuário, é necessário atualizar a funcionalidade do controle CreateUserWizard. Em vez de fazer essas alterações para o `CreatingUserAccounts.aspx` página, vamos em vez disso, adicione um novo controle CreateUserWizard para `EnhancedCreateUserWizard.aspx` página e faça as alterações para este tutorial existe.

Abra o `EnhancedCreateUserWizard.aspx` página no Visual Studio e arraste um controle CreateUserWizard da caixa de ferramentas para a página. Definir o controle de CreateUserWizard `ID` propriedade `NewUserWizard`. Conforme abordado no [ *criando contas de usuário* ](creating-user-accounts-vb.md) tutorial, a interface de usuário padrão do CreateUserWizard solicita que o visitante para as informações necessárias. Depois que essas informações foram fornecidas, o controle internamente cria uma nova conta de usuário na estrutura de associação, todos os sem nos precisar escrever uma única linha de código.

O controle CreateUserWizard eleva um número de eventos durante o fluxo de trabalho. Depois que um visitante fornece as informações de solicitação e envia o formulário, o controle CreateUserWizard inicialmente dispara seu [ `CreatingUser` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx). Se houver um problema durante o processo de criação, o [ `CreateUserError` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx) é acionado; no entanto, se o usuário é criado com êxito, o [ `CreatedUser` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx) é gerado. No [ *criando contas de usuário* ](creating-user-accounts-vb.md) tutorial criamos um manipulador de eventos para o `CreatingUser` evento para garantir que o nome de usuário fornecido não contém qualquer à esquerda ou à direita de espaços e que o nome de usuário não foi exibida em qualquer lugar na senha.

Para adicionar uma linha a `UserProfiles` tabela de usuário recém-criada, precisamos criar um manipulador de eventos para o `CreatedUser` evento. No momento o `CreatedUser` evento foi disparado, a conta de usuário já foi criada na estrutura de associação, que permite recuperar o valor de ID de usuário da conta.

Criar um manipulador de eventos para o `NewUserWizard`do `CreatedUser` evento e adicione o seguinte código:

[!code-vb[Main](storing-additional-user-information-vb/samples/sample11.vb)]

As seres código acima, recuperando a ID do usuário da conta de usuário adicionada apenas. Isso é feito usando o `Membership.GetUser(username)` método para retornar informações sobre um usuário específico e, em seguida, usando o `ProviderUserKey` propriedade a recuperar sua ID de usuário. O nome de usuário inserido pelo usuário no controle CreateUserWizard está disponível por meio de seu [ `UserName` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.username.aspx).

Em seguida, a cadeia de caracteres de conexão é recuperada do `Web.config` e `INSERT` instrução é especificada. Os objetos necessários do ADO.NET são instanciados e o comando executado. O código atribui um [ `DBNull` ](https://msdn.microsoft.com/library/system.dbnull.aspx) de instância para o `@HomeTown`, `@HomepageUrl`, e `@Signature` parâmetros, que tem o efeito de inserção de banco de dados `NULL` valores para o `HomeTown`, `HomepageUrl`, e `Signature` campos.

Visite o `EnhancedCreateUserWizard.aspx` página através de um navegador e criar uma nova conta de usuário. Depois de fazer isso, retorne ao Visual Studio e examine o conteúdo do `aspnet_Users` e `UserProfiles` tabelas (como na Figura 12). Você deve ver a nova conta de usuário no `aspnet_Users` e correspondente `UserProfiles` linha (com `NULL` valores para `HomeTown`, `HomepageUrl`, e `Signature`).


[![Uma nova conta de usuário e registro UserProfiles foi adicionados](storing-additional-user-information-vb/_static/image59.png)](storing-additional-user-information-vb/_static/image58.png)

**Figura 20**: A nova conta de usuário e `UserProfiles` registro ter sido adicionado ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image60.png))


Depois que o visitante tiver fornecido suas novas informações de conta e clicar no botão "Create User", a conta de usuário é criada e uma linha é adicionada para o `UserProfiles` tabela. Em seguida, exibe o CreateUserWizard seu `CompleteWizardStep`, que exibe uma mensagem de êxito e um botão de continuação. Clicar no botão continuar causa um postback, mas nenhuma ação é executada, deixar o usuário presas no `EnhancedCreateUserWizard.aspx` página.

Podemos especificar uma URL para enviar o usuário para quando o botão continuar for clicado por meio do controle CreateUserWizard [ `ContinueDestinationPageUrl` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx). Definir o `ContinueDestinationPageUrl` propriedade como "~ / Membership/AdditionalUserInfo.aspx". Isso leva o novo usuário `AdditionalUserInfo.aspx`, onde eles podem exibir e atualizar suas configurações.

### <a name="customizing-the-createuserwizards-interface-to-prompt-for-the-new-users-home-town-homepage-and-signature"></a>Personalizando a Interface do CreateUserWizard para solicitar o novo usuário Home cidade, home page e assinatura

Interface de padrão de controle CreateUserWizard é suficiente para cenários de criação de conta simples, onde somente informações de conta de usuário de núcleo como nome de usuário, senha e email precisam ser coletadas. Mas e se desejarmos solicitar o visitante insira a cidade, a home page e a assinatura ao criar sua conta? É possível personalizar a interface do controle CreateUserWizard para coletar informações adicionais na inscrição, e essas informações podem ser usadas no `CreatedUser` manipulador de eventos para inserir registros adicionais no banco de dados subjacente.

O controle CreateUserWizard estende o controle do Assistente do ASP.NET, que é um controle que permite que um desenvolvedor de página definir uma série de ordenada `WizardSteps`. O controle de assistente processa a etapa ativa e fornece uma interface de navegação que permite que o visitante percorrer as seguintes etapas. O controle Wizard é ideal para dividir uma tarefa longa em várias etapas. Para obter mais informações sobre o controle do assistente, consulte [criando uma Interface de usuário do passo a passo com o controle de Assistente do ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx).

Marcação de padrão do controle CreateUserWizard define dois `WizardSteps`: `CreateUserWizardStep` e `CompleteWizardStep`.

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample12.aspx)]

A primeira `WizardStep`, `CreateUserWizardStep`, renderiza a interface que solicita o nome de usuário, senha, email e assim por diante. Depois que o visitante fornecerá essas informações e clica em "Create User", ela é mostrada a `CompleteWizardStep`, que mostra a mensagem de êxito e um botão de continuação.

Para personalizar a interface do controle CreateUserWizard para incluir campos adicionais, podemos:

- <strong>Criar um ou mais novos</strong><strong>`WizardStep`</strong><strong>s para conter os elementos da interface de usuário adicionais</strong>. Para adicionar uma nova `WizardStep` CreateUserWizard, clique o "Adicionar ou remover `WizardStep` s" link de marca inteligente para iniciar o `WizardStep` Editor de coleção. Lá você pode adicionar, remover ou reordenar as etapas no assistente. Essa é a abordagem que usaremos para este tutorial.

- <strong>Converter o</strong><strong>`CreateUserWizardStep`</strong><strong>em um editável</strong><strong>`WizardStep`</strong><strong>.</strong> Isso substitui o `CreateUserWizardStep` com um equivalente `WizardStep` cuja marcação define uma interface de usuário que corresponda a `CreateUserWizardStep`' s. Convertendo o `CreateUserWizardStep` em um `WizardStep` podemos reposicionar os controles ou adicionar elementos de interface de usuário adicionais para esta etapa. Para converter o `CreateUserWizardStep` ou `CompleteWizardStep` em um editável `WizardStep`, clique o "Personalizar criar usuário etapa" ou "Personalizar concluir a etapa" vincular de marca inteligente do controle.

- **Use uma combinação das duas opções acima.**

Uma coisa importante para ter em mente é que o controle CreateUserWizard executa seu processo de criação de conta de usuário, ao clicar no botão "Criar usuário" no seu `CreateUserWizardStep`. Não importa se há adicionais `WizardStep` s após o `CreateUserWizardStep` ou não.

Ao adicionar um personalizado `WizardStep` para o controle CreateUserWizard para coletar a entrada do usuário adicional, personalizado `WizardStep` pode ser colocado antes ou depois de `CreateUserWizardStep`. Se ele antecede o `CreateUserWizardStep` , em seguida, a entrada adicional do usuário coletados de personalizado `WizardStep` está disponível para o `CreatedUser` manipulador de eventos. No entanto, se personalizado `WizardStep` vem depois `CreateUserWizardStep` , em seguida, pelo tempo personalizado `WizardStep` é exibida a nova conta de usuário já foi criada e o `CreatedUser` evento já foi disparado.

Figura 21 mostra o fluxo de trabalho quando adicionado `WizardStep` precede o `CreateUserWizardStep`. Desde que as informações de usuário adicionais foram coletadas pelo tempo de `CreatedUser` evento ser acionado, precisamos fazer é atualização o `CreatedUser` manipulador de eventos para recuperar essas entradas e usá-las para o `INSERT` valores de parâmetro da instrução (em vez de `DBNull.Value`).


[![O fluxo de trabalho CreateUserWizard quando um WizardStep adicional precede o CreateUserWizardStep](storing-additional-user-information-vb/_static/image62.png)](storing-additional-user-information-vb/_static/image61.png)

**Figura 21**: O CreateUserWizard fluxo de trabalho quando um adicionais `WizardStep` Precedes o `CreateUserWizardStep` ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image63.png))


Se personalizado `WizardStep` é colocado *depois* o `CreateUserWizardStep`, no entanto, o processo de conta de usuário de criar ocorre antes do usuário teve a chance de inserir sua cidade, home page ou assinatura. Nesse caso, devem ser inserido no banco de dados depois que a conta de usuário foi criada, conforme ilustra a Figura 22 essas informações adicionais.


[![O fluxo de trabalho CreateUserWizard quando um WizardStep adicional segue o CreateUserWizardStep](storing-additional-user-information-vb/_static/image65.png)](storing-additional-user-information-vb/_static/image64.png)

**A Figura 22**: O CreateUserWizard fluxo de trabalho quando um adicionais `WizardStep` vem após o `CreateUserWizardStep` ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image66.png))


O fluxo de trabalho mostrado na Figura 22 aguarda para inserir um registro para o `UserProfiles` tabela até após a conclusão da etapa 2. Se o visitante fechar seu navegador após a etapa 1, no entanto, teremos atingido um estado em que uma conta de usuário foi criada, mas nenhum registro foi adicionado ao `UserProfiles`. Uma solução alternativa é ter um registro com `NULL` ou valores inseridos padrão `UserProfiles` no `CreatedUser` manipulador de eventos (que é acionado após a etapa 1) e atualize esse registro após a conclusão da etapa 2. Isso garante que um `UserProfiles` registro será adicionado para a conta de usuário mesmo se o usuário fecha no processo de registro meio.

Para este tutorial vamos criar um novo `WizardStep` que ocorre após o `CreateUserWizardStep` mas antes de `CompleteWizardStep`. Vamos primeiro get WizardStep em colocar e configurado e, em seguida, podemos examinar o código.

De marca do controle CreateUserWizard inteligente, selecione o "Adicionar ou remover `WizardStep` s", que abre o `WizardStep` caixa de diálogo Editor de coleção. Adicionar um novo `WizardStep`, configuração seu `ID` para `UserSettings`, sua `Title` até "Configurações do" e seu `StepType` para `Step`. Em seguida, posicione-o para que ele segue o `CreateUserWizardStep` ("inscrever-se para sua nova conta") e antes do `CompleteWizardStep` ("concluído"), conforme mostrado na Figura 23.


[![Adicionar um novo WizardStep ao controle CreateUserWizard](storing-additional-user-information-vb/_static/image68.png)](storing-additional-user-information-vb/_static/image67.png)

**Figura 23**: adicionar um novo `WizardStep` para o controle CreateUserWizard ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-vb/_static/image69.png))


Clique em Okey para fechar o `WizardStep` caixa de diálogo Editor de coleção. O novo `WizardStep` fica evidente através de marcação de declarativa atualizada do controle CreateUserWizard:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample13.aspx)]

Observe o novo `<asp:WizardStep>` elemento. É preciso adicionar a interface do usuário para coletar o novo usuário cidade, home page e assinatura aqui. Você pode inserir esse conteúdo na sintaxe declarativa ou por meio do Designer. Para usar o Designer, selecione a etapa de "Configurações do" na lista suspensa na marca inteligente para ver a etapa no Designer.

> [!NOTE]
> Seleção de uma etapa por meio lista suspensa da marca inteligente-atualizações do controle CreateUserWizard [ `ActiveStepIndex` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.activestepindex.aspx), que especifica o índice da etapa inicial. Portanto, se você usar essa lista suspensa para editar a etapa de "Configurações do" no Designer, certifique-se para defini-lo para "Sign Up for sua nova conta", para que essa etapa é mostrada quando os usuários visitam primeiro o `EnhancedCreateUserWizard.aspx` página.


Criar uma interface de usuário dentro da etapa de "Configurações do" que contém três controles de caixa de texto denominados `HomeTown`, `HomepageUrl`, e `Signature`. Após a criação desta interface, declarativo do CreateUserWizard deve ser semelhante ao seguinte:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample14.aspx)]

Vá em frente e visite esta página por meio de um navegador e criar uma nova conta de usuário, especificando valores para a cidade, a home page e a assinatura. Depois de concluir o `CreateUserWizardStep` a conta de usuário é criada na estrutura de associação e o `CreatedUser` execuções de manipulador de eventos, que adiciona uma nova linha à `UserProfiles`, mas com um banco de dados `NULL` valor para `HomeTown`, `HomepageUrl`, e `Signature`. Os valores inseridos para a cidade, a home page e a assinatura nunca são usados. O resultado é uma nova conta de usuário com um `UserProfiles` registro cujo `HomeTown`, `HomepageUrl`, e `Signature` campos ainda precisam ser especificados.

É preciso executar código após a etapa de "Configurações do" que aceita os valores base de cidade, honepage e assinatura inseridos pelo usuário e atualiza as `UserProfiles` registro. Cada vez que o usuário move entre as etapas em um Assistente de controle, o assistente [ `ActiveStepChanged` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.wizard.activestepchanged.aspx) é acionado. Podemos criar um manipulador de eventos para este evento e a atualização do `UserProfiles` tabela quando a etapa de "Configurações do" foi concluída.

Adicionar um manipulador de eventos para o CreateUserWizard `ActiveStepChanged` evento e adicione o seguinte código:

[!code-vb[Main](storing-additional-user-information-vb/samples/sample15.vb)]

Inicia o código acima, determinando se estamos apenas atingiu a etapa "Completo". Como a etapa "Completo" ocorre imediatamente após a etapa de "Configurações do", em seguida, quando atinge o visitante "Concluído" etapa que significa que ele acabou de concluir a etapa de "Configurações do".

Nesse caso, precisamos fazer referência programaticamente os controles de caixa de texto dentro de `UserSettings WizardStep`. Isso é feito usando primeiro o `FindControl` método programaticamente referenciando o `UserSettings WizardStep`e, em seguida, novamente para fazer referência a caixas de texto de dentro a `WizardStep`. Depois que a referência de caixas de texto, estamos prontos para executar o `UPDATE` instrução. O `UPDATE` instrução tem o mesmo número de parâmetros como o `INSERT` instrução o `CreatedUser` manipulador de eventos, mas aqui usamos os valores de cidade, a home page e a assinatura inicial fornecidos pelo usuário.

Com este manipulador de eventos em vigor, visite o `EnhancedCreateUserWizard.aspx` página através de um navegador e criar uma nova conta de usuário especificar valores para a cidade, a home page e a assinatura. Depois de criar a nova conta, você deverá ser redirecionado para a `AdditionalUserInfo.aspx` página, em que as informações de cidade, a home page e a assinatura inicial inserido apenas são exibidas.

> [!NOTE]
> Nosso site atualmente tem duas páginas do qual um visitante pode criar uma nova conta: `CreatingUserAccounts.aspx` e `EnhancedCreateUserWizard.aspx`. Ponto de mapa de site do site e a página de logon para o `CreatingUserAccounts.aspx` página, mas o `CreatingUserAccounts.aspx` página não solicitar ao usuário as informações de cidade, a home page e a assinatura inicial e não adicionará uma linha correspondente ao `UserProfiles`. Portanto, atualizar o `CreatingUserAccounts.aspx` para que ele oferece essa funcionalidade de página ou atualizar a página de mapa do site e o logon para fazer referência a `EnhancedCreateUserWizard.aspx` em vez de `CreatingUserAccounts.aspx`. Se você escolher a última opção, certifique-se de atualizar o `Membership` da pasta `Web.config` arquivo de modo a permitir que usuários anônimos acessem o `EnhancedCreateUserWizard.aspx` página.


## <a name="summary"></a>Resumo

Neste tutorial vimos técnicas para modelagem de dados relacionados a contas de usuário dentro da estrutura de associação. Em particular, examinamos modelagem entidades que compartilham uma relação um-para-muitos com contas de usuário, bem como dados que compartilha uma relação um para um. Além disso, vimos como isso relacionado foi exibidas, inseridas e atualizadas, com alguns exemplos que usam o controle SqlDataSource e outras informações usando o código do ADO.NET.

Este tutorial conclui nossa aparência em contas de usuário. Começando com o seguinte tutorial no transformaremos nossa atenção às funções. Sobre a próxima vários tutoriais, vamos examinar a estrutura de funções, consulte como criar novas funções, como atribuir funções aos usuários, como ao determinar quais funções um usuário pertence e para aplicar a autorização baseada em função.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Acessar e atualizar dados no ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [ASP.NET 2.0 Assistente controle](https://weblogs.asp.net/scottgu/archive/2006/02/21/438732.aspx)
- [Criando uma Interface de usuário passo a passo com o controle do ASP.NET 2.0 de Assistente](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [Criando parâmetros de controle de fonte de dados personalizados](http://aspnet.4guysfromrolla.com/articles/110106-1.aspx)
- [Personalizando o controle CreateUserWizard](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx)
- [Início rápido do controle DetailsView](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/detailsview.aspx)
- [Exibindo dados com o controle ListView](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [Dissecando os controles de validação no ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)
- [Edição de inserção e exclusão de dados](../../data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)
- [Validação do formulário no ASP.NET](http://www.4guysfromrolla.com/webtech/090200-1.shtml)
- [Coletando informações de registro de usuário personalizada](https://weblogs.asp.net/scottgu/archive/2006/07/05/Tip_2F00_Trick_3A00_-Gathering-Custom-User-Registration-Information.aspx)
- [Perfis no ASP.NET 2.0](http://www.odetocode.com/Articles/440.aspx)
- [Controle asp: ListView](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)
- [Início rápido de perfis de usuário](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/profile/default.aspx)

### <a name="about-the-author"></a>Sobre o autor

Scott Mitchell, autor de vários livros sobre ASP/ASP.NET e fundador da 4GuysFromRolla. com, trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é  *[Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott pode ser contatado pelo [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) ou em seu blog [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a...

Esta série de tutoriais foi revisado por vários revisores úteis. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Anterior](user-based-authorization-vb.md)
