---
uid: web-api/overview/security/working-with-ssl-in-web-api
title: Trabalhando com SSL na API da Web | Microsoft Docs
author: MikeWasson
description: Mostra como usar o SSL com a API da Web do ASP.NET, incluindo o uso de certificados de cliente SSL.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 97f6164f-59cf-45c0-b820-e4aa29b45396
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/working-with-ssl-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 127b336cb628e55bd59481ecb1c4df83960dc25b
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2018
ms.locfileid: "28036158"
---
<a name="working-with-ssl-in-web-api"></a>Trabalhando com SSL na API da Web
====================
por [Mike Wasson](https://github.com/MikeWasson)

Vários esquemas de autenticação comuns não são seguras por HTTP sem formatação. Em particular, a autenticação básica e autenticação de formulários enviam credenciais sem criptografia. Para ser seguro, esses esquemas de autenticação *deve* usar SSL. Além disso, os certificados de cliente SSL podem ser usados para autenticar clientes.

## <a name="enabling-ssl-on-the-server"></a>Habilitar o SSL no servidor

Para configurar o SSL no IIS 7 ou posterior:

- Criar ou obter um certificado. Para testar, você pode criar um certificado autoassinado.
- Adicione uma associação HTTPS.

Para obter detalhes, consulte [como configurar o SSL no IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).

Para testes locais, você pode habilitar o SSL no IIS Express do Visual Studio. Na janela Propriedades, defina **SSL habilitado** para **True**. Observe o valor de **SSL URL**; usar essa URL para testar conexões HTTPS.

![](working-with-ssl-in-web-api/_static/image1.png)

### <a name="enforcing-ssl-in-a-web-api-controller"></a>Imposição de SSL em um controlador de API da Web

Se você tiver um HTTPS e uma associação HTTP, os clientes ainda podem usar HTTP para acessar o site. Você pode permitir que alguns recursos estejam disponíveis por meio de HTTP, enquanto outros recursos exigem SSL. Nesse caso, use um filtro de ação para exigir SSL para os recursos protegidos. O código a seguir mostra um filtro de autenticação de API da Web que verifica o SSL:

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample1.cs)]

Adicione esse filtro para as ações de API da Web que exigem SSL:

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample2.cs)]

## <a name="ssl-client-certificates"></a>Certificados de cliente SSL

O SSL fornece autenticação usando certificados de infraestrutura de chave pública. O servidor deve fornecer um certificado que autentica o servidor para o cliente. É menos comum para o cliente fornecer um certificado para o servidor, mas essa é uma opção para autenticação de clientes. Para usar certificados de cliente com SSL, você precisa de uma maneira de distribuir certificados autoassinados para seus usuários. Para muitos tipos de aplicativo, isso não será uma boa experiência do usuário, mas em alguns ambientes (por exemplo, enterprise) pode ser viável.

| Vantagens | Desvantagens |
| --- | --- |
| -As credenciais de certificado são mais seguras do que o nome de usuário e senha. -SSL fornece um canal seguro concluído, com autenticação, integridade e criptografia de mensagens. | -Você deve obter e gerenciar certificados PKI. -A plataforma do cliente deve oferecer suporte a certificados de cliente SSL. |

Para configurar o IIS para aceitar certificados de cliente, abra o Gerenciador do IIS e execute as seguintes etapas:

1. Clique no nó do site no modo de exibição de árvore.
2. Clique duas vezes o **configurações de SSL** recurso no painel central.
3. Em **certificados de cliente**, selecione uma destas opções: 

    - **Aceitar**: IIS aceitará um certificado do cliente, mas não requer um.
    - **Exigir**: exigem um certificado de cliente. (Para habilitar essa opção, você também deve selecionar "Exigir SSL")

Você também pode definir essas opções no arquivo applicationHost. config:

[!code-xml[Main](working-with-ssl-in-web-api/samples/sample3.xml)]

O **SslNegotiateCert** sinalizador significa IIS aceitará um certificado do cliente, mas não requer um (equivalente à opção "Aceitar" no Gerenciador do IIS). Para solicitar um certificado, defina o **SslRequireCert** sinalizador. Para testar, você também pode definir essas opções no IIS Express, em que o host de aplicativos local. Arquivo de configuração, localizado em "Documents\IISExpress\config".

### <a name="creating-a-client-certificate-for-testing"></a>Criar um certificado de cliente para teste

Para fins de teste, você pode usar [MakeCert.exe](https://msdn.microsoft.com/library/bfsktky3.aspx) para criar um certificado de cliente. Primeiro, crie uma autoridade raiz de teste:

[!code-console[Main](working-with-ssl-in-web-api/samples/sample4.cmd)]

Makecert solicitará que você insira uma senha para a chave privada.

Em seguida, adicione o certificado para o teste "Autoridades de certificação confiáveis" do servidor armazenar, da seguinte maneira:

1. Abra o MMC.
2. Em **arquivo**, selecione **Adicionar/Remover Snap-In**.
3. Selecione **conta de computador**.
4. Selecione **computador Local** e conclua o assistente.
5. No painel de navegação, expanda o nó de "Autoridades de certificação confiáveis".
6. Sobre o **ação** , aponte para **todas as tarefas**e, em seguida, clique em **importação** para iniciar o Assistente de importação de certificado.
7. Navegue até o arquivo de certificado, TempCA.cer.
8. Clique em **abrir**, em seguida, clique em **próximo** e conclua o assistente. (Você será solicitado a inserir novamente a senha.)

Agora, crie um certificado de cliente que está assinado pelo certificado primeiro:

[!code-console[Main](working-with-ssl-in-web-api/samples/sample5.cmd)]

### <a name="using-client-certificates-in-web-api"></a>Usando certificados de cliente na API da Web

No lado do servidor, você pode obter o certificado de cliente chamando [GetClientCertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) na mensagem de solicitação. O método retornará nulo se não houver nenhum certificado do cliente. Caso contrário, ele retorna um **X509Certificate2** instância. Use esse objeto para obter informações do certificado, como o assunto e emissor. Em seguida, você pode usar essas informações para autenticação ou autorização.

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample6.cs)]
