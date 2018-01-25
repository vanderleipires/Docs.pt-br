---
uid: identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
title: "Migrando um site existente da associação SQL para a identidade do ASP.NET | Microsoft Docs"
author: Rick-Anderson
description: "Este tutorial ilustra as etapas para migrar um aplicativo web existente com o usuário e os dados de função criados usando a associação de SQL para a nova identidade do ASP.NET s..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/19/2014
ms.topic: article
ms.assetid: 220d3d75-16b2-4240-beae-a5b534f06419
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 3638c6779a0fcedaaa49623126b28ecf09a4954f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="migrating-an-existing-website-from-sql-membership-to-aspnet-identity"></a>Migrando um site existente da associação SQL para a identidade do ASP.NET
====================
por [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)

> Este tutorial ilustra as etapas para migrar um aplicativo web existente com dados de função criados usando a associação do SQL para o novo sistema de identidade do ASP.NET e do usuário. Essa abordagem envolve a alteração do esquema de banco de dados existente para um necessária para a identidade do ASP.NET e gancho nas classes antiga e nova para ele. Depois que você adote essa abordagem, depois que o banco de dados é migrado, futuras atualizações a identidade serão tratadas com facilidade.


Neste tutorial, obtemos um modelo de aplicativo da web (Web Forms) criado usando o Visual Studio 2010 para criar dados de usuário e a função. Em seguida, usaremos scripts SQL para migrar o banco de dados existente para as tabelas necessárias para o sistema de identidade. Em seguida vamos instalar os pacotes do NuGet necessários e adicionar novas páginas de gerenciamento de conta que usa o sistema de identidade para o gerenciamento de associação. Como um teste de migração, os usuários criados usando a participação do SQL devem ser capazes de fazer logon e novos usuários devem ser capazes de se registrar. Você pode encontrar o exemplo completo [aqui](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/). Consulte também [migrando de associação do ASP.NET para a identidade do ASP.NET](http://travis.io/blog/2015/03/24/migrate-from-aspnet-membership-to-aspnet-identity.html).

## <a name="getting-started"></a>Introdução

### <a name="creating-an-application-with-sql-membership"></a>Criando um aplicativo com a associação do SQL

1. É preciso iniciar com um aplicativo existente que usa associação do SQL e tem dados de usuário e a função. Com a finalidade deste artigo, vamos criar um aplicativo web no Visual Studio 2010.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.jpg)
2. Usando a ferramenta de configuração do ASP.NET, criar 2 usuários: **oldAdminUser** e **oldUser.**

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.jpg)
3. Criar uma função chamada administrador e adicione 'oldAdminUser' como um usuário nessa função.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.png)
4. Crie uma seção de administração do site com um default. aspx. Defina a marca de autorização no arquivo Web. config para habilitar o acesso somente aos usuários em funções de administrador. Mais informações podem ser encontradas aqui [https://www.asp.net/web-forms/tutorials/security/roles/role-based-authorization-cs](../../../web-forms/overview/older-versions-security/roles/role-based-authorization-cs.md)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.png)
5. Exiba o banco de dados no Gerenciador de servidores para entender as tabelas criadas pelo sistema de associação do SQL. Os dados de logon do usuário são armazenados no aspnet\_usuários e aspnet\_tabelas de associação, enquanto os dados de função são armazenados no aspnet\_tabela de funções. Informações sobre quais usuários estão em quais funções são armazenados no aspnet\_tabela UsersInRoles. Para o gerenciamento de associação básica é suficiente para a porta as informações nas tabelas acima para o sistema de identidade do ASP.NET.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.png)

### <a name="migrating-to-visual-studio-2013"></a>Migrando para o Visual Studio 2013

1. Instalar o Visual Studio Express 2013 para Web ou o Visual Studio 2013 junto com o [atualizações mais recentes](https://www.microsoft.com/download/details.aspx?id=44921).
2. Abra o projeto acima na sua versão instalada do Visual Studio. Se o SQL Server Express não está instalado no computador, um prompt será exibido quando você abrir o projeto, desde que a cadeia de caracteres de conexão usa o SQL Express. Você pode escolher instalar o SQL Express ou como solução alternativa para alterar a cadeia de caracteres de conexão para o LocalDb. Para este artigo, alteraremos-lo para o LocalDb.
3. Abra Web. config e altere a cadeia de caracteres de conexão. SQLExpess para v 11.0 do (LocalDb). Remover ' User Instance = true' da cadeia de conexão.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.jpg)
4. Abra o Gerenciador de servidores e verifique se que o esquema de tabela e os dados podem ser observados.
5. O sistema de identidade do ASP.NET funciona com a versão 4.5 ou posterior do framework. Redirecione o aplicativo 4.5 ou superior.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image5.png)

    Crie o projeto para verificar se não houver nenhum erro.

### <a name="installing-the-nuget-packages"></a>Instalando os pacotes do Nuget

1. No Gerenciador de soluções, clique com botão direito no projeto &gt; **gerenciar pacotes NuGet**. Na caixa de pesquisa, digite "Asp.net Identity". Selecione o pacote na lista de resultados e clique em instalar. Aceite o contrato de licença clicando no botão "Aceito". Observe que este pacote irá instalar os pacotes de dependência: EntityFramework e Microsoft ASP.NET Identity Core. Da mesma forma, instale os pacotes a seguir (ignorar os últimos 4 pacotes OWIN se você não quiser habilitar o log no OAuth):

    - Microsoft.AspNet.Identity.Owin
    - Microsoft.Owin.Host.SystemWeb
    - Microsoft.Owin.Security.Facebook
    - Microsoft.Owin.Security.Google
    - Microsoft.Owin.Security.MicrosoftAccount
    - Microsoft.Owin.Security.Twitter

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image6.png)

### <a name="migrate-database-to-the-new-identity-system"></a>Migrar o banco de dados para o novo sistema de identidade

A próxima etapa é migrar o banco de dados existente para um esquema exigido pelo sistema de identidade do ASP.NET. Para conseguir isso executamos um SQL script que tem um conjunto de comandos para criar novas tabelas e migrar informações de usuário existentes para novas tabelas. O arquivo de script pode ser encontrado [aqui](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/Migrations.sql).

Esse arquivo de script é específico para este exemplo. Se o esquema para as tabelas criadas usando a associação SQL personalizada ou modificado a necessidade de scripts a serem alterados de acordo.

### <a name="how-to-generate-the-sql-script-for-schema-migration"></a>Como gerar o script SQL para a migração de esquema

Para classes de identidade do ASP.NET trabalhar fora da caixa com os dados de usuários existentes, é necessário migrar o esquema de banco de dados para aquele necessário por identidade do ASP.NET. Podemos fazer isso adicionando novas tabelas e copiando as informações existentes nessas tabelas. Por padrão a identidade do ASP.NET usa EntityFramework para mapear as classes de modelo de identidade no banco de dados para armazenar/recuperar informações. Essas classes de modelo implementam as interfaces de identidade de núcleo definindo objetos de usuário e função. As tabelas e colunas no banco de dados com base nessas classes de modelo. As classes de modelo EntityFramework v 2.1.0 de identidade e suas propriedades são conforme definido abaixo

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

É preciso ter tabelas para cada um desses modelos com colunas que correspondem às propriedades. O mapeamento entre classes e tabelas é definido no `OnModelCreating` método o `IdentityDBContext`. Isso é conhecido como o método de API fluente de configuração e mais informações podem ser encontradas [aqui](https://msdn.microsoft.com/data/jj591617.aspx). A configuração das classes é mencionado abaixo

| **Class** | **Tabela** | **Chave primária** | **Chave estrangeira** |
| --- | --- | --- | --- |
| IdentityUser | AspnetUsers | Id |  |
| IdentityRole | AspnetRoles | Id |  |
| IdentityUserRole | AspnetUserRole | UserId + RoleId | Usuário\_Id -&gt;AspnetUsers RoleId -&gt;AspnetRoles |
| IdentityUserLogin | AspnetUserLogins | UserId + ProviderKey + LoginProvider | UserId-&gt;AspnetUsers |
| IdentityUserClaim | AspnetUserClaims | Id | User\_Id-&gt;AspnetUsers |

Com essas informações, podemos criar instruções SQL para criar novas tabelas. Podemos pode gravar cada instrução individualmente ou gerar o script usando comandos do PowerShell de EntityFramework que, em seguida, podemos Editar conforme necessário. Para fazer isso, no VS abrir o **Package Manager Console** do **exibição** ou **ferramentas** menu

- Execute o comando "Enable-Migrations" para permitir as migrações de EntityFramework.
- Execute o comando "Add-migration inicial", que cria o código de configuração inicial para criar o banco de dados em c# / VB.
- A etapa final é executar "Atualizar banco de dados – Script" comando que gera o script SQL baseado nas classes de modelo.

Este script de geração de banco de dados pode ser usado como um início onde podemos estará fazendo alterações adicionais para adicionar novas colunas e copiar dados. A vantagem disso é que podemos gerar o `_MigrationHistory` tabela que é usada pelo EntityFramework para modificar o esquema de banco de dados quando o modelo de classes de alteração para versões futuras de versões de identidade. 

As informações de usuário de associação SQL tinham outras propriedades além na classe de modelo de usuário como email identidade tentativas de senha, data do último logon, última data de bloqueio etc. Essas informações são úteis e gostaríamos de receber a ser transferida para o sistema de identidade. Isso pode ser feito adicionando propriedades adicionais para o modelo de usuário e mapeando-os para as colunas da tabela no banco de dados. Podemos fazer isso adicionando uma classe que as subclasses de `IdentityUser` modelo. Podemos adicionar as propriedades para esta classe personalizada e edite o script SQL para adicionar as colunas correspondentes ao criar a tabela. O código para essa classe é descrito posteriormente no artigo. O script SQL para criar o `AspnetUsers` tabela depois de adicionar as novas propriedades seria

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample1.sql)]

Em seguida, precisamos copiar as informações de existentes do banco de dados de associação SQL às tabelas recém-adicionada para identidade. Isso pode ser feito por meio do SQL, copiando os dados diretamente de uma tabela para outra. Para adicionar dados em linhas de tabela, usamos o `INSERT INTO [Table]` construir. Para copiar de outra tabela, podemos usar o `INSERT INTO` instrução junto com o `SELECT` instrução. Para obter todas as informações de usuário é necessário consultar o *aspnet\_usuários* e *aspnet\_associação* tabelas e copiar os dados para o *AspNetUsers*tabela. Usamos o `INSERT INTO` e `SELECT` juntamente com `JOIN` e `LEFT OUTER JOIN` instruções. Para obter mais informações sobre como consultar e copiando dados entre tabelas, consulte [isso](https://technet.microsoft.com/library/ms190750%28v=sql.105%29.aspx) link. Além das tabelas AspnetUserLogins e AspnetUserClaims estão vazias em primeiro lugar porque não há nenhuma informação sobre a associação SQL que é mapeado para isso, por padrão. É a única informação que copiou para usuários e funções. Para o projeto criado nas etapas anteriores, a consulta SQL para copiar informações para a tabela de usuários deve ser

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample2.sql)]

Na instrução SQL acima, informações sobre cada usuário a *aspnet\_usuários* e *aspnet\_associação* tabelas são copiados para as colunas da  *AspnetUsers* tabela. A única modificação feita aqui é quando a senha são copiados. Como o algoritmo de criptografia de senhas na associação SQL usado 'PasswordSalt' e 'PasswordFormat', podemos copiar que muito junto com a senha com hash para que ele pode ser usado para descriptografar a senha pela identidade. Isso é explicado com mais detalhes no artigo quando gancho hasher uma senha personalizada. 

Esse arquivo de script é específico para este exemplo. Para aplicativos que têm tabelas adicionais, os desenvolvedores podem seguir uma abordagem semelhante para adicionar outras propriedades na classe de modelo de usuário e mapeá-los para colunas na tabela AspnetUsers. Para executar o script

1. Abra o Gerenciador de servidores. Expanda a conexão 'ApplicationServices' para exibir as tabelas. Clique com o botão direito no nó tabelas e selecione a opção 'Nova consulta'

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image7.png)
2. Na janela de consulta, copie e cole o script SQL inteiro do arquivo Migrations.sql. Execute o arquivo de script, pressionar o botão de seta 'Executar'.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.jpg)

    Atualize a janela do Gerenciador de servidores. Cinco novas tabelas são criadas no banco de dados.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image8.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image9.png)

    Abaixo está a como as informações nas tabelas de associação do SQL são mapeados para o novo sistema de identidade.

    aspnet\_Roles --&gt; AspNetRoles

    ASP\_netUsers e asp\_netMembership -&gt; AspNetUsers

    aspnet\_UserInRoles --&gt; AspNetUserRoles

    Conforme explicado na seção acima, as tabelas AspNetUserClaims e AspNetUserLogins estão vazias. O campo 'Discriminador' na tabela AspNetUser deve corresponder ao nome de classe de modelo que está definido como uma próxima etapa. Também a coluna PasswordHash está no formato ' senha criptografada | salt de senha | formatação da senha '. Isso permite que você use SQL associação criptografia uma lógica especial para que você pode reutilizar as senhas antigas. Que é explicado em posteriormente neste artigo.

### <a name="creating-models-and-membership-pages"></a>Criar modelos e páginas de associação

Como mencionado anteriormente, o recurso de identidade usa o Entity Framework para se comunicar com o banco de dados para armazenar informações de conta por padrão. Para trabalhar com os dados existentes na tabela, é preciso criar classes de modelo que mapeiam para as tabelas e conectá-las no sistema de identidade. Como parte do contrato de identidade, as classes de modelo devem implementar as interfaces definidas na dll Identity.Core ou podem estender a implementação dessas interfaces disponíveis no ASPNET.

Em nosso exemplo, as tabelas de AspNetRoles, AspNetUserClaims, AspNetLogins e AspNetUserRole têm colunas que são semelhantes à implementação existente do sistema de identidade. Portanto, podemos reutilizar as classes existentes para mapear para essas tabelas. A tabela AspNetUser tem algumas colunas adicionais que são usadas para armazenar informações adicionais das tabelas de associação do SQL. Isso pode ser mapeado, criando uma classe de modelo que estendem a implementação de 'IdentityUser' e adicione as propriedades adicionais.

1. Pasta de modelos de criar um no projeto e adicionar uma classe de usuário. O nome da classe deve corresponder os dados adicionados na coluna 'Discriminador' da tabela 'AspnetUsers'.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image10.png)

    A classe de usuário deve estender a classe IdentityUser encontrada no *ASPNET* dll. Declare as propriedades na classe que mapeiam para as colunas AspNetUser. As propriedades ID, nome de usuário, PasswordHash e SecurityStamp são definidas no IdentityUser e isso são omitidas. Abaixo está o código para a classe de usuário que tem todas as propriedades

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample3.cs)]
2. Uma classe DbContext do Entity Framework é necessária para manter dados em modelos de volta para tabelas e recuperar dados de tabelas para preencher os modelos. *ASPNET* dll define a classe do IdentityDbContext que interage com as tabelas de identidade para recuperar e armazenar informações. IdentityDbContext&lt;tuser&gt; usa uma classe 'TUser', que pode ser qualquer classe que estende a classe IdentityUser.

    Criar uma nova classe ApplicationDBContext que estende o IdentityDbContext na pasta 'Modelos', passando na classe 'User' criado na etapa 1

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample4.cs)]
3. Gerenciamento de usuários no novo sistema de identidade é feito usando o UserManager&lt;tuser&gt; classe definida no *ASPNET* dll. É preciso criar uma classe personalizada que estende o UserManager, passando na classe 'User' criado na etapa 1.

    Na pasta de modelos, criar uma nova classe que estende o UserManager UserManager&lt;usuário&gt;

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample5.cs)]
4. As senhas de usuários do aplicativo são criptografadas e armazenadas no banco de dados. O algoritmo de criptografia usado na associação do SQL é diferente no novo sistema de identidade. A reutilização de senhas antigas, precisamos seletivamente descriptografar senhas quando os usuários fazem logon usando o algoritmo de associações do SQL usando o algoritmo de criptografia de identidade para os novos usuários.

    A classe UserManager tem uma propriedade 'PasswordHasher', que armazena uma instância de uma classe que implementa a interface 'IPasswordHasher'. Isso é usado para criptografar/descriptografar senhas durante as transações de autenticação de usuário. Na classe UserManager definido na etapa 3, crie uma nova classe SQLPasswordHasher e copie o código abaixo.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample6.cs)]

    Resolva os erros de compilação, importe os namespaces System. Text e Cryptography.

    O método EncodePassword criptografa a senha de acordo com a implementação de criptografia de associação do SQL padrão. Isso é obtido da dll System. Web. Se o aplicativo antigo usado uma implementação personalizada, em seguida, ele deve ser refletido aqui. É preciso definir dois outros métodos *HashPassword* e *VerifyHashedPassword* que usam o *EncodePassword* método hash de uma senha fornecida ou verificar um texto sem formatação senha com um existente no banco de dados.

    O sistema de associação do SQL usado PasswordHash, PasswordSalt e PasswordFormat para hash da senha inserida por usuários quando eles registrarem ou alterar sua senha. Durante a migração de todos os três campos são armazenados na coluna PasswordHash na tabela AspNetUser separada pela ' |' caracteres. Quando um usuário faz logon e a senha tem esses campos, usamos a criptografia de associação do SQL para verificar a senha; Caso contrário, usamos padrão de criptografia do sistema de identidade para verificar a senha. Dessa forma os usuários não precisa alterar suas senhas depois que o aplicativo é migrado.
5. Declare o construtor para a classe UserManager e a passa como o SQLPasswordHasher para a propriedade no construtor.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample7.cs)]

### <a name="create-new-account-management-pages"></a>Criar nova conta de páginas de gerenciamento

A próxima etapa da migração é adicionar a páginas de gerenciamento de conta que permitirá que um usuário registrar e entrar. As páginas de conta antiga da associação SQL usam controles que não funcionam com o novo sistema de identidade. Para adicionar o novo usuário páginas de gerenciamento siga o tutorial neste link [https://www.asp.net/identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project](../getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project.md) desde a etapa ' Adição de Web Forms para registrar usuários para o aplicativo ' porque já criou o projeto e adicionar os pacotes do NuGet.

Precisamos fazer algumas alterações para o exemplo trabalhar com o projeto que temos aqui.

- O código Register.aspx.cs e Login.aspx.cs por trás do uso de classes de `UserManager` dos pacotes de identidade para criar um usuário. Para este exemplo usa o UserManager adicionado à pasta de modelos, seguindo as etapas mencionadas anteriormente.
- Use a classe de usuário criada em vez de IdentityUser no código Register.aspx.cs e Login.aspx.cs por trás de classes. Isso conecta em nossa classe de usuário personalizada para o sistema de identidade.
- A parte para criar o banco de dados pode ser ignorada.
- O desenvolvedor precisa definir ApplicationId para o novo usuário para coincidir com a ID do aplicativo atual. Isso pode ser feito consultando ApplicationId para este aplicativo antes de um objeto de usuário é criado na classe Register.aspx.cs e configurá-la antes de criar o usuário. 

    Exemplo:

    Definir um método na página Register.aspx.cs para consultar o aspnet\_aplicativos de tabela e obter o aplicativo de Id de acordo com o nome do aplicativo

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample8.cs)]

    Agora Obtenha defini-la no objeto do usuário

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample9.cs)]

Use o antigo nome de usuário e a senha de logon de um usuário existente. Use a página de registro para criar um novo usuário. Também verifique se os usuários em funções conforme o esperado.

Movimentando para o sistema de identidade ajuda o usuário adicionar autenticação aberta (OAuth) para o aplicativo. Consulte o exemplo [aqui](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/) que tenha o OAuth habilitado.

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, mostramos como transferir os usuários da associação SQL para a identidade do ASP.NET, mas nós não dados de perfil de porta. O tutorial Avançar vamos para a portabilidade de dados de perfil da associação do SQL para o novo sistema de identidade.

Você pode deixar os comentários na parte inferior deste artigo.

*Agradecimentos Tom Dykstra e Rick Anderson para revisar o artigo.*
