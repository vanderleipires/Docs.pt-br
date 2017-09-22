---
title: "Configurar o HTTPS para o desenvolvimento no núcleo do ASP.NET"
author: Rick-Anderson
description: "Mostra como configurar o HTTPS para o desenvolvimento no ASP.NET 2.0 de núcleo."
keywords: "Núcleo do ASP.NET, SSL, HTTPS"
ms.author: riande
manager: wpickett
ms.date: 05/10/2017
ms.topic: article
ms.assetid: 94f2f1a4-7d46-45e2-a085-a57916e41724
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/https
ms.openlocfilehash: 7913758ffa045dcf884d73eed9bab223b8d2c3fe
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2017
---
# <a name="setting-up-https-for-development-in-aspnet-core"></a>Configurar o HTTPS para o desenvolvimento no núcleo do ASP.NET

> [!NOTE] 
> Este tópico se aplica ao ASP.NET Core 2.0 Preview 1

Você pode configurar seu aplicativo para usar HTTPS durante o desenvolvimento para simular HTTPS em seu ambiente de produção. Habilitando HTTPS pode ser necessárias para habilitar a integração com vários provedores de identidade (como [AD do Azure](https://azure.microsoft.com/services/active-directory) e [do Azure AD B2C](https://azure.microsoft.com/services/active-directory-b2c/)).

<a name="iisxpress"></a>

No Windows, se você instalou o Visual Studio ou o IIS Express, o IIS Express desenvolvimento certificado no repositório de certificados LocalMachine. Você pode atualizar suas propriedades de projeto no Visual Studio para usar este certificado durante a execução por trás do IIS Express.

   * No Gerenciador de soluções, clique com o botão direito e selecione **propriedades**.
   * No painel esquerdo, selecione **depurar**.
   * Verificar **habilitar SSL**
   * Copie a URL de SSL e cole-o no **URL do aplicativo**

![Guia de propriedades do aplicativo web de depuração](enforcing-ssl/_static/ssl.png)

Para o desenvolvimento, você pode usar o certificado de desenvolvimento do IIS Express se ele está disponível ou criar um novo certificado para fins de desenvolvimento. O certificado de desenvolvimento deve ser configurado no `appsettings.Development.json` arquivos de forma que ele não é usado na produção:

```json
{
  "Certificates": {
    "HTTPS": {
      "Source": "Store",
      "StoreLocation": "LocalMachine",
      "StoreName": "My",
      "Subject": "CN=localhost",
      "AllowInvalid": true
    }
  }
}
```

Um aplicativo com essa configuração em execução na produção lançará uma exceção não dizendo "Nenhum certificado denominado 'HTTPS' encontrado na configuração para o ambiente atual (produção)". Para alternar o [ambiente](xref:fundamentals/environments) para `Development`, defina o `ASPNETCORE_ENVIRONMENT` variável de ambiente `Development`.

Se você não tiver o IIS Express desenvolvimento certificado instalado, você pode criar um certificado de desenvolvimento por conta própria. No Windows, você pode criar um certificado de desenvolvimento e adicioná-lo no armazenamento raiz confiável para o usuário atual, executando os seguintes comandos do PowerShell em um prompt com privilégios elevados:

```powershell
$cert = New-SelfSignedCertificate -Subject localhost -DnsName localhost -FriendlyName "ASP.NET Core Development" -KeyUsage DigitalSignature -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.1") 
Export-Certificate -Cert $cert -FilePath cert.cer
Import-Certificate -FilePath cert.cer -CertStoreLocation cert:/CurrentUser/Root
```

<a name="OpenSSL"></a>

## <a name="kestrel-on--macos-and-linux"></a>Kestrel em macOS e Linux

Você pode configurar Kestrel para escutar em HTTPS ao configurar um ponto de extremidade com o endereço desejado de IP, porta e as certificado. O certificado pode ser configurado em linha, ou no nível superior `Certificates` seção e, em seguida, referenciadas por nome:

```json
{
  "Kestrel": {
    "Endpoints": {
      "LocalhostHttps": {
        "Address": "127.0.0.1",
        "Port": "43434",
        "Certificate": "HTTPS"
      }
    }
  }
}

```

Em macOS e Linux, você pode criar um certificado autoassinado para usar HTTPS [OpenSSL](https://www.openssl.org/):

```bash
openssl req -new -x509 -newkey rsa:2048 -keyout localhost.key -out localhost.cer -days 365 -subj /CN=localhost
openssl pkcs12 -export -out certificate.pfx -inkey localhost.key -in localhost.cer
```

Uma vez o `certificate.pfx` arquivo foi gerado, configure o certificado HTTPS em seu `appsettings.Development.json` arquivo:

```json
{
  "Certificates": {
    "HTTPS": {
      "Source": "File",
      "Path": "certificate.pfx"
    }
  }
}
```

Você também precisará especificar a senha para o certificado, definindo a propriedade de configuração "Certificados: HTTPS:Password". As senhas não devem ser armazenadas em texto sem formatação. Consulte [seguro armazenamento de segredos durante o desenvolvimento de aplicativos](app-secrets.md) para manipular apropriado da senha do certificado.

MacOS pode [adicionar o certificado ao seu conjunto de chaves](https://support.apple.com/kb/PH20129?locale=en_US) e [alterar suas configurações de confiança](https://support.apple.com/kb/PH20127?locale=en_US&viewlocale=en_US) para que ele é confiável para HTTPS durante o desenvolvimento. Para adicionar o certificado ao seu conjunto de chaves (o equivalente a `CurrentUser/My` armazenar no Windows) execute o seguinte comando:

```bash
security import certificate.pfx -k ~/Library/Keychains/login.keychain-db
```

E, em seguida, para confiar no certificado:

```bash
security add-trusted-cert localhost.cer
```

Você pode configurar seu aplicativo para usar esse certificado em desenvolvimento, como este:

```json
{
  "Certificates": {
    "HTTPS": {
      "Source": "Store",
      "StoreLocation": "CurrentUser",
      "StoreName": "My",
      "Subject": "CN=localhost",
      "AllowInvalid": true
    }
  }
}
```
