---
uid: identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
title: Migrando um site existente da associação do SQL para o ASP.NET Identity | Microsoft Docs
author: Rick-Anderson
description: Este tutorial ilustra as etapas para migrar um aplicativo web existente com o usuário e dados de função criados usando a associação do SQL para a nova identidade do ASP.NET s...
ms.author: aspnetcontent
ms.date: 12/19/2014
ms.assetid: 220d3d75-16b2-4240-beae-a5b534f06419
msc.legacyurl: /identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 4acb8c82c9b05de9d587466170f8fac4ef9b6dde
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812245"
---
<a name="migrating-an-existing-website-from-sql-membership-to-aspnet-identity"></a>Migrando um site existente da associação do SQL para o ASP.NET Identity
====================
por [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)

> Este tutorial ilustra as etapas para migrar um aplicativo web existente com o usuário e dados de função criados usando a associação do SQL para o novo sistema ASP.NET Identity. Essa abordagem envolve a alteração de esquema de banco de dados existentes para aquele necessário para a identidade do ASP.NET e o gancho nas classes antigos/novos a ele. Depois que você adotar essa abordagem, depois que seu banco de dados é migrado, as atualizações futuras identidade serão tratadas sem esforço.


Para este tutorial, vamos utilizar um modelo de aplicativo da web (Web Forms) criado usando o Visual Studio 2010 para criar dados de usuário e a função. Em seguida, usaremos scripts SQL para migrar o banco de dados existente para tabelas necessárias para o sistema de identidade. Em seguida vamos instalar os pacotes NuGet necessários e adicionar novas páginas de gerenciamento de conta que usam o sistema de identidade para o gerenciamento de associação. Como um teste de migração, os usuários criados usando a associação do SQL devem ser capazes de fazer logon e novos usuários devem ser capazes de se registrar. Você pode encontrar o exemplo completo [aqui](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/). Consulte também [migrando da associação do ASP.NET para ASP.NET Identity](http://travis.io/blog/2015/03/24/migrate-from-aspnet-membership-to-aspnet-identity.html).

## <a name="getting-started"></a>Introdução

### <a name="creating-an-application-with-sql-membership"></a>Criando um aplicativo com a associação do SQL

1. É preciso começar com um aplicativo existente que usa a associação do SQL e tem dados de usuário e a função. Com a finalidade deste artigo, vamos criar um aplicativo web no Visual Studio 2010.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.jpg)
2. Usando a ferramenta de configuração do ASP.NET, criar 2 usuários: **oldAdminUser** e **oldUser.**

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.jpg)
3. Criar uma função chamada administrador e adicione 'oldAdminUser' como um usuário nessa função.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.png)
4. Crie uma seção de administração do site com um default. aspx. Defina a marca de autorização no arquivo Web. config para habilitar o acesso somente aos usuários em funções de administrador. Mais informações podem ser encontradas aqui [https://www.asp.net/web-forms/tutorials/security/roles/role-based-authorization-cs](../../../web-forms/overview/older-versions-security/roles/role-based-authorization-cs.md)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.png)
5. Exiba o banco de dados no Gerenciador de servidores para entender as tabelas criadas pelo sistema de associação do SQL. Os dados de logon do usuário são armazenados no aspnet\_usuários e aspnet\_tabelas de associação, enquanto os dados de função são armazenados no aspnet\_funções da tabela. Informações sobre quais usuários estão no quais funções são armazenados no aspnet\_tabela UsersInRoles. Para o gerenciamento de associação básica é suficiente para as informações nas tabelas acima para o sistema ASP.NET Identity da porta.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.png)

### <a name="migrating-to-visual-studio-2013"></a>Migrando para o Visual Studio 2013

1. Instalar o Visual Studio Express 2013 para Web ou o Visual Studio 2013 juntamente com o [atualizações mais recentes](https://www.microsoft.com/download/details.aspx?id=44921).
2. Abra o projeto acima em sua versão instalada do Visual Studio. Se não estiver instalado no computador do SQL Server Express, um prompt será exibido quando você abrir o projeto, uma vez que a cadeia de caracteres de conexão usa o SQL Express. Você pode optar por instalar o SQL Express ou como solução alternativa para alterar a cadeia de caracteres de conexão para o LocalDb. Para este artigo, alteraremos-lo para o LocalDb.
3. Abra o Web. config e altere a cadeia de conexão do. SQLExpess para v11.0 (LocalDb). Remover ' User Instance = true' da cadeia de conexão.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.jpg)
4. Abra o Gerenciador de servidores e verifique se que o esquema da tabela e os dados podem ser observados.
5. O sistema de identidade do ASP.NET funciona com a versão 4.5 ou superior do framework. Redirecione o aplicativo para a versão 4.5 ou superior.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image5.png)

    Compile o projeto para verificar se há erros.

### <a name="installing-the-nuget-packages"></a>Instalando os pacotes do Nuget

1. No Gerenciador de soluções, clique com botão direito no projeto &gt; **gerenciar pacotes NuGet**. Na caixa de pesquisa, digite "Asp.net Identity". Selecione o pacote na lista de resultados e clique em instalar. Aceite o contrato de licença clicando no botão "Aceito". Observe que este pacote irá instalar os pacotes de dependência: EntityFramework e identidade do Microsoft ASP.NET Core. Da mesma forma, instale os pacotes a seguir (ignore os últimos 4 pacotes do OWIN se você não quiser habilitar log-in OAuth):

   - Microsoft.AspNet.Identity.Owin
   - Microsoft.Owin.Host.SystemWeb
   - Microsoft.Owin.Security.Facebook
   - Microsoft.Owin.Security.Google
   - Microsoft.Owin.Security.MicrosoftAccount
   - Microsoft.Owin.Security.Twitter

     ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image6.png)

### <a name="migrate-database-to-the-new-identity-system"></a>Migrar o banco de dados para o novo sistema de identidade

A próxima etapa é migrar o banco de dados existente para um esquema exigido pelo sistema de identidade do ASP.NET. Para conseguir isso, podemos executar um SQL script que tem um conjunto de comandos para criar novas tabelas e migrar informações existentes de usuário para as novas tabelas. O arquivo de script pode ser encontrado [aqui](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/Migrations.sql).

Esse arquivo de script é específico para este exemplo. Se o esquema para as tabelas criadas usando a associação do SQL personalizada ou modificado scripts precisarão ser alteradas adequadamente.

### <a name="how-to-generate-the-sql-script-for-schema-migration"></a>Como gerar o script SQL para a migração de esquema

Para classes de identidade do ASP.NET trabalhar fora da caixa com os dados de usuários existentes, é necessário migrar o esquema de banco de dados para aquele necessário pela identidade do ASP.NET. Podemos fazer isso adicionando novas tabelas e copiando as informações existentes nessas tabelas. Por padrão o ASP.NET Identity usa o EntityFramework para mapear as classes de modelo de identidade no banco de dados para armazenamento/recuperação de informações. Essas classes de modelo implementam interfaces de identidade principais, definindo objetos de usuário e função. As tabelas e colunas no banco de dados com base nessas classes de modelo. As classes de modelo do EntityFramework identidade anterior à versão 2.1.0 e suas propriedades são conforme definido abaixo

| **IdentityUser** | **Tipo** | **IdentityRole** | **IdentityUserRole** | **IdentityUserLogin** | **IdentityUserClaim** |
| --- | --- | --- | --- | --- | --- |
| Id | cadeia de caracteres | Id | RoleId | ProviderKey | Id |
| Nome de usuário | cadeia de caracteres | Nome | ID de usuário | ID de usuário | ClaimType |
| PasswordHash | cadeia de caracteres |  |  | LoginProvider | ClaimValue |
| SecurityStamp | cadeia de caracteres |  |  |  | Usuário\_Id |
| Email | cadeia de caracteres |  |  |  |  |
| EmailConfirmed | bool |  |  |  |  |
| PhoneNumber | cadeia de caracteres |  |  |  |  |
| PhoneNumberConfirmed | bool |  |  |  |  |
| LockoutEnabled | bool |  |  |  |  |
| LockoutEndDate | DateTime |  |  |  |  |
| AccessFailedCount | int |  |  |  |  |

É necessário ter tabelas para cada um desses modelos com colunas que correspondem às propriedades. O mapeamento entre classes e tabelas é definido na `OnModelCreating` método da `IdentityDBContext`. Isso é conhecido como o método da API fluente da configuração e obter mais informações podem ser encontradas [aqui](https://msdn.microsoft.com/data/jj591617.aspx). A configuração das classes é mencionado abaixo

| **Class** | **Tabela** | **Chave primária** | **Chave estrangeira** |
| --- | --- | --- | --- |
| IdentityUser | AspnetUsers | Id |  |
| IdentityRole | AspnetRoles | Id |  |
| IdentityUserRole | AspnetUserRole | ID de usuário + RoleId | Usuário\_Id -&gt;AspnetUsers RoleId -&gt;AspnetRoles |
| IdentityUserLogin | AspnetUserLogins | ID de usuário + ProviderKey + LoginProvider | ID de usuário -&gt;AspnetUsers |
| IdentityUserClaim | AspnetUserClaims | Id | Usuário\_Id -&gt;AspnetUsers |

Com essas informações, podemos criar instruções SQL para criar novas tabelas. Podemos pode gravar cada instrução individualmente ou gerar todo o script usando os comandos do PowerShell do EntityFramework que, em seguida, podemos Editar conforme necessário. Para fazer isso, no VS open a **Package Manager Console** da **exibição** ou **ferramentas** menu

- Execute o comando "Enable-Migrations" para habilitar migrações do EntityFramework.
- Execute o comando "Add-migration initial", que cria o código de configuração inicial para criar o banco de dados em c# / VB.
- A etapa final é executar "Update-Database – Script" comando que gera o script SQL baseado nas classes de modelo.

[!INCLUDE[](../../../includes/identity/alter-command-exception.md)]

Esse script de geração de banco de dados pode ser usado como ponto de partida, onde podemos estará fazendo alterações adicionais para adicionar novas colunas e copiar dados. A vantagem disso é que nós geramos a `_MigrationHistory` tabela que é usada pelo EntityFramework para modificar o esquema de banco de dados quando o modelo de classes de alteração para versões futuras dos lançamentos de identidade. 

As informações de usuário SQL tinham outras propriedades além na classe de modelo de usuário identidade ou seja, email, a tentativas de senha, a data do último logon, a última data limite de bloqueio etc. Essas informações são úteis e gostaríamos que ele ser transferidos para o sistema de identidade. Isso pode ser feito adicionando propriedades adicionais para o modelo de usuário e mapeando-os de volta para as colunas da tabela no banco de dados. Podemos fazer isso adicionando uma classe que pode efetuar subclasses de `IdentityUser` modelo. Podemos adicionar as propriedades para essa classe personalizada e edite o script SQL para adicionar as colunas correspondentes ao criar a tabela. O código para essa classe é descrito posteriormente neste artigo. O script SQL para criar o `AspnetUsers` depois de adicionar as novas propriedades seria de tabela

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample1.sql)]

Em seguida, é necessário copiar as informações existentes do banco de dados de associação SQL às tabelas recém-adicionado para a identidade. Isso pode ser feito por meio do SQL, copiando os dados diretamente de uma tabela para outra. Para adicionar dados em linhas de tabela, usamos o `INSERT INTO [Table]` construir. Para copiar de outra tabela, podemos usar o `INSERT INTO` instrução juntamente com o `SELECT` instrução. Para obter todas as informações de usuário, precisamos consultar as *aspnet\_os usuários* e *aspnet\_associação* tabelas e copiar os dados para o *AspNetUsers*tabela. Podemos usar o `INSERT INTO` e `SELECT` juntamente com `JOIN` e `LEFT OUTER JOIN` instruções. Para obter mais informações sobre como consultar e copiar dados entre tabelas, consulte [isso](https://technet.microsoft.com/library/ms190750%28v=sql.105%29.aspx) link. Além das tabelas AspnetUserLogins e AspnetUserClaims estão vazias para começar, pois não há nenhuma informação de associação do SQL que é mapeado para isso por padrão. É a única informação que copiou para usuários e funções. Para o projeto criado nas etapas anteriores, a consulta SQL para copiar informações para a tabela de usuários deve ser

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample2.sql)]

Na instrução SQL acima, informações sobre cada usuário da *aspnet\_os usuários* e *aspnet\_associação* tabelas é copiado para as colunas da  *AspnetUsers* tabela. A única modificação feita aqui é quando podemos copiar a senha. Uma vez que o algoritmo de criptografia de senhas na associação do SQL usado 'PasswordSalt' e 'PasswordFormat', copiamos que muito junto com a senha com hash para que possa ser usado para descriptografar a senha por identidade. Isso é explicado com mais detalhes no artigo quando vinculando hasher uma senha personalizada. 

Esse arquivo de script é específico para este exemplo. Para aplicativos que têm tabelas adicionais, os desenvolvedores podem seguir uma abordagem semelhante para adicionar propriedades adicionais na classe de modelo de usuário e mapeá-las para colunas na tabela AspnetUsers. Para executar o script

1. Abra o Gerenciador de servidores. Expanda a conexão 'ApplicationServices' para exibir as tabelas. Clique com o botão direito no nó tabelas e selecione a opção 'Nova consulta'

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image7.png)
2. Na janela de consulta, copie e cole o script SQL inteiro do arquivo Migrations.sql. Execute o arquivo de script pressionando o botão de seta 'Executar'.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.jpg)

    Atualize a janela do Gerenciador de servidores. Cinco novas tabelas são criadas no banco de dados.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image8.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image9.png)

    Abaixo é como as informações nas tabelas de associação SQL são mapeados para o novo sistema de identidade.

    ASPNET\_funções –&gt; AspNetRoles

    ASP\_netUsers e asp\_netMembership –&gt; AspNetUsers

    ASPNET\_UserInRoles –&gt; AspNetUserRoles

    Conforme explicado na seção acima, as tabelas AspNetUserClaims e AspNetUserLogins estão vazias. O campo 'Discriminador' na tabela AspNetUser deve corresponder ao nome de classe de modelo que é definido como uma próxima etapa. Também a coluna PasswordHash está no formato ' senha criptografada | sal de senha | formato da senha '. Isso permite que você use o SQL associação crypto uma lógica especial para que você pode reutilizar as senhas antigas. Que é explicado posteriormente neste artigo.

### <a name="creating-models-and-membership-pages"></a>Criar modelos e páginas de associação

Como mencionado anteriormente, o recurso de identidade usa o Entity Framework para se comunicar com o banco de dados para armazenar informações de conta por padrão. Para trabalhar com dados existentes na tabela, precisamos criar classes de modelo que mapeiam para as tabelas e conectá-las no sistema de identidade. Como parte do contrato de identidade, as classes de modelo devem implementam as interfaces definidas na dll Identity.Core ou podem estender a implementação existente dessas interfaces disponíveis no EntityFramework.

Em nosso exemplo, as tabelas de AspNetRoles, AspNetUserClaims, AspNetLogins e AspNetUserRole têm colunas que são semelhantes para a implementação existente do sistema de identidade. Portanto, podemos reutilizar as classes existentes para mapear a essas tabelas. A tabela de AspNetUser tem algumas colunas adicionais que são usadas para armazenar informações adicionais das tabelas de associação do SQL. Isso pode ser mapeado, criando uma classe de modelo que estendem a implementação existente do 'IdentityUser' e adicione as propriedades adicionais.

1. Pasta de modelos de criar um no projeto e adicionar uma classe de usuário. O nome da classe deve corresponder os dados adicionados na coluna 'Discriminador' da tabela 'AspnetUsers'.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image10.png)

    A classe de usuário deve estender a classe IdentityUser encontrada na *ASPNET* dll. Declare as propriedades na classe que mapeiam para as colunas AspNetUser. As propriedades ID, nome de usuário, PasswordHash e SecurityStamp são definidas no IdentityUser e isso são omitidas. Abaixo está o código para a classe de usuário que tem todas as propriedades

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample3.cs)]
2. Uma classe DbContext do Entity Framework é necessária para manter os dados em modelos de volta para tabelas e recuperar dados de tabelas para preencher os modelos. *ASPNET* dll define a classe do IdentityDbContext que interage com as tabelas de identidade para recuperar e armazenar informações. IdentityDbContext&lt;tuser&gt; usa uma classe 'TUser', que pode ser qualquer classe que estende a classe IdentityUser.

    Crie uma nova classe ApplicationDBContext que estende IdentityDbContext sob a pasta 'Modelos', passando na classe 'User' criado na etapa 1

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample4.cs)]
3. Gerenciamento de usuários no novo sistema de identidade é feito usando o UserManager&lt;tuser&gt; classe definida na *ASPNET* dll. É necessário criar uma classe personalizada que estende o UserManager, passando na classe 'User' criado na etapa 1.

    Na pasta Models, crie uma nova classe UserManager que estende o UserManager&lt;usuário&gt;

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample5.cs)]
4. As senhas dos usuários do aplicativo são criptografadas e armazenadas no banco de dados. O algoritmo de criptografia usado na associação do SQL é diferente no novo sistema de identidade. A reutilização de senhas antigas, que é necessário descriptografar seletivamente as senhas quando usuários antigos logon usando o algoritmo de associações SQL usando o algoritmo de criptografia em identidade para os novos usuários.

    A classe UserManager tem uma propriedade 'PasswordHasher', que armazena uma instância de uma classe que implementa a interface 'IPasswordHasher'. Isso é usado para criptografar/descriptografar senhas durante as transações de autenticação do usuário. Na classe UserManager definida na etapa 3, crie uma nova classe SQLPasswordHasher e copie o código abaixo.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample6.cs)]

    Resolva os erros de compilação, importe os namespaces System. Text e Cryptography.

    O método EncodePassword criptografa a senha de acordo com a implementação de criptografia de associação do SQL padrão. Isso é obtido da dll System. Web. Se o aplicativo antigo usado uma implementação personalizada, em seguida, ele deve ser refletido aqui. Precisamos definir dois outros métodos *HashPassword* e *VerifyHashedPassword* que usam o *EncodePassword* método para uma determinada senha de hash ou verificar um texto sem formatação senha com um existente no banco de dados.

    O sistema de associação do SQL usado PasswordHash e PasswordSalt de PasswordFormat para hash da senha inserida pelos usuários quando eles se registrar ou alterarem sua senha. Durante a migração, todos os três campos são armazenados na coluna PasswordHash na tabela AspNetUser separada pelo ' |' caracteres. Quando um usuário faz logon e a senha tem esses campos, podemos usar a criptografia de associação do SQL para verificar a senha; Caso contrário, usamos a criptografia de padrão do sistema de identidade para verificar a senha. Dessa forma usuários antigos não precisaria alterar suas senhas depois que o aplicativo é migrado.
5. Declare o construtor para a classe UserManager e a passa como o SQLPasswordHasher à propriedade no construtor.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample7.cs)]

### <a name="create-new-account-management-pages"></a>Criar nova conta de páginas de gerenciamento

A próxima etapa da migração é adicionar páginas de gerenciamento de conta que permitirá que um usuário registre e faça logon. As páginas de conta antiga da associação do SQL usam controles que não funcionam com o novo sistema de identidade. Para adicionar o novo usuário páginas de gerenciamento siga o tutorial neste link [ https://www.asp.net/identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project ](../getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project.md) começando da etapa 'Adicionar formulários da Web para registrar usuários para seu aplicativo', pois estamos já criou o projeto e adicionou o NuGet pacotes.

Precisamos fazer algumas alterações para o exemplo trabalhar com o projeto que temos aqui.

- O código de Register.aspx.cs e Login.aspx.cs por trás do uso de classes de `UserManager` dos pacotes de identidade para criar um usuário. Para este exemplo use o UserManager adicionado à pasta de modelos, seguindo as etapas mencionadas anteriormente.
- Use a classe de usuário criada em vez do IdentityUser no Register.aspx.cs e Login.aspx.cs code-behind de classes. Isso conecta em nossa classe de usuário personalizada para o sistema de identidade.
- A parte para criar o banco de dados pode ser ignorada.
- O desenvolvedor precisa definir o ApplicationId para o novo usuário para corresponder à ID do aplicativo. Isso pode ser feito consultando o ApplicationId para este aplicativo antes de um objeto de usuário é criado na classe Register.aspx.cs e defini-lo antes de criar o usuário. 

    Exemplo:

    Definir um método na página Register.aspx.cs para consultar o aspnet\_aplicativos de tabela e obter a Id do aplicativo acordo com o nome do aplicativo

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample8.cs)]

    Agora Obtenha defini-la no objeto do usuário

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample9.cs)]

Use o nome de usuário antigo e a senha para fazer logon de um usuário existente. Use a página de registro para criar um novo usuário. Também verifique se os usuários em funções conforme o esperado.

Portabilidade para o sistema de identidade ajuda o usuário adicione autenticação aberta (OAuth) para o aplicativo. Consulte a amostra [aqui](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/) que tenha o OAuth habilitado.

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, mostramos como os usuários da associação do SQL para o ASP.NET Identity da porta, mas nós não dados de perfil de porta. No próximo tutorial, estamos examinará a portabilidade de dados de perfil da associação do SQL para o novo sistema de identidade.

Você pode deixar comentários na parte inferior deste artigo.

*Graças ao Tom Dykstra e Rick Anderson revisão do artigo.*
