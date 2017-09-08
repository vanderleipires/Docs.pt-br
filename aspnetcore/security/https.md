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
ms.openlocfilehash: e06f4194d496b5b11aa867e66563bec317e735ff
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="setting-up-https-for-development-in-aspnet-core"></a><span data-ttu-id="40dfe-104">Configurar o HTTPS para o desenvolvimento no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="40dfe-104">Setting up HTTPS for development in ASP.NET Core</span></span>

> [!NOTE] 
> <span data-ttu-id="40dfe-105">Este tópico se aplica ao ASP.NET Core 2.0 Preview 1</span><span class="sxs-lookup"><span data-stu-id="40dfe-105">This topic applies to ASP.NET Core 2.0 Preview 1</span></span>

<span data-ttu-id="40dfe-106">Você pode configurar seu aplicativo para usar HTTPS durante o desenvolvimento para simular HTTPS em seu ambiente de produção.</span><span class="sxs-lookup"><span data-stu-id="40dfe-106">You can configure your application to use HTTPS during development to simulate HTTPS in your production environment.</span></span> <span data-ttu-id="40dfe-107">Habilitando HTTPS pode ser necessárias para habilitar a integração com vários provedores de identidade (como [AD do Azure](https://azure.microsoft.com/services/active-directory) e [do Azure AD B2C](https://azure.microsoft.com/services/active-directory-b2c)).</span><span class="sxs-lookup"><span data-stu-id="40dfe-107">Enabling HTTPS may be required to enable integration with various identity providers (like [Azure AD](https://azure.microsoft.com/services/active-directory) and [Azure AD B2C](https://azure.microsoft.com/services/active-directory-b2c)).</span></span>

<a name="iisxpress"></a>

<span data-ttu-id="40dfe-108">No Windows, se você instalou o Visual Studio ou o IIS Express, o IIS Express desenvolvimento certificado no repositório de certificados LocalMachine.</span><span class="sxs-lookup"><span data-stu-id="40dfe-108">On Windows if you’ve installed Visual Studio or IIS Express, the IIS Express Development Certificate will be in your LocalMachine certificate store.</span></span> <span data-ttu-id="40dfe-109">Você pode atualizar suas propriedades de projeto no Visual Studio para usar este certificado durante a execução por trás do IIS Express.</span><span class="sxs-lookup"><span data-stu-id="40dfe-109">You can update your project properties in Visual Studio to use this certificate when running behind IIS Express.</span></span>

   * <span data-ttu-id="40dfe-110">No Gerenciador de soluções, clique com o botão direito e selecione **propriedades**.</span><span class="sxs-lookup"><span data-stu-id="40dfe-110">In Solution Explorer, right-click the project and select **Properties**.</span></span>
   * <span data-ttu-id="40dfe-111">No painel esquerdo, selecione **depurar**.</span><span class="sxs-lookup"><span data-stu-id="40dfe-111">On the left pane, select **Debug**.</span></span>
   * <span data-ttu-id="40dfe-112">Verificar **habilitar SSL**</span><span class="sxs-lookup"><span data-stu-id="40dfe-112">Check **Enable SSL**</span></span>
   * <span data-ttu-id="40dfe-113">Copie a URL de SSL e cole-o no **URL do aplicativo**</span><span class="sxs-lookup"><span data-stu-id="40dfe-113">Copy the SSL URL and paste it into the **App URL**</span></span>

![Guia de propriedades do aplicativo web de depuração](enforcing-ssl/_static/ssl.png)

<span data-ttu-id="40dfe-115">Para o desenvolvimento, você pode usar o certificado de desenvolvimento do IIS Express se ele está disponível ou criar um novo certificado para fins de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="40dfe-115">For development you can use the IIS Express Development Certificate if it is available, or create a new certificate for development purposes.</span></span> <span data-ttu-id="40dfe-116">O certificado de desenvolvimento deve ser configurado no `appsettings.Development.json` arquivos de forma que ele não é usado na produção:</span><span class="sxs-lookup"><span data-stu-id="40dfe-116">The development certificate should be configured in the `appsettings.Development.json` file so that it is not used in production:</span></span>

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

<span data-ttu-id="40dfe-117">Um aplicativo com essa configuração em execução na produção lançará uma exceção não dizendo "Nenhum certificado denominado 'HTTPS' encontrado na configuração para o ambiente atual (produção)".</span><span class="sxs-lookup"><span data-stu-id="40dfe-117">An app with this configuration running in production will throw an exception saying "No certificate named 'HTTPS' found in configuration for the current environment (Production)".</span></span> <span data-ttu-id="40dfe-118">Para alternar o [ambiente](xref:fundamentals/environments) para `Development`, defina o `ASPNETCORE_ENVIRONMENT` variável de ambiente `Development`.</span><span class="sxs-lookup"><span data-stu-id="40dfe-118">To switch the [environment](xref:fundamentals/environments) to `Development`, set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development`.</span></span>

<span data-ttu-id="40dfe-119">Se você não tiver o IIS Express desenvolvimento certificado instalado, você pode criar um certificado de desenvolvimento por conta própria.</span><span class="sxs-lookup"><span data-stu-id="40dfe-119">If you do not have the IIS Express Development Certificate installed, you can create a development certificate yourself.</span></span> <span data-ttu-id="40dfe-120">No Windows, você pode criar um certificado de desenvolvimento e adicioná-lo no armazenamento raiz confiável para o usuário atual, executando os seguintes comandos do PowerShell em um prompt com privilégios elevados:</span><span class="sxs-lookup"><span data-stu-id="40dfe-120">On Windows you can create a development certificate and add it to the trusted root store for the current user by running the following PowerShell commands in an elevated prompt:</span></span>

```powershell
$cert = New-SelfSignedCertificate -Subject localhost -DnsName localhost -FriendlyName "ASP.NET Core Development" -KeyUsage DigitalSignature -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.1") 
Export-Certificate -Cert $cert -FilePath cert.cer
Import-Certificate -FilePath cert.cer -CertStoreLocation cert:/CurrentUser/Root
```

<a name="OpenSSL"></a>

## <a name="kestrel-on--macos-and-linux"></a><span data-ttu-id="40dfe-121">Kestrel em macOS e Linux</span><span class="sxs-lookup"><span data-stu-id="40dfe-121">Kestrel on  macOS and Linux</span></span>

<span data-ttu-id="40dfe-122">Você pode configurar Kestrel para escutar em HTTPS ao configurar um ponto de extremidade com o endereço desejado de IP, porta e as certificado.</span><span class="sxs-lookup"><span data-stu-id="40dfe-122">You can  configure Kestrel to listen over HTTPS by configuring an endpoint with the desired IP address, port, and certificate.</span></span> <span data-ttu-id="40dfe-123">O certificado pode ser configurado em linha, ou no nível superior `Certificates` seção e, em seguida, referenciadas por nome:</span><span class="sxs-lookup"><span data-stu-id="40dfe-123">The certificate can be configured inline, or in the top-level `Certificates` section and then referenced by name:</span></span>

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

<span data-ttu-id="40dfe-124">Em macOS e Linux, você pode criar um certificado autoassinado para usar HTTPS [OpenSSL](https://www.openssl.org/):</span><span class="sxs-lookup"><span data-stu-id="40dfe-124">On macOS and Linux you can create a self-signed certificate for HTTPS using [OpenSSL](https://www.openssl.org/):</span></span>

```bash
openssl req -new -x509 -newkey rsa:2048 -keyout localhost.key -out localhost.cer -days 365 -subj /CN=localhost
openssl pkcs12 -export -out certificate.pfx -inkey localhost.key -in localhost.cer
```

<span data-ttu-id="40dfe-125">Uma vez o `certificate.pfx` arquivo foi gerado, configure o certificado HTTPS em seu `appsettings.Development.json` arquivo:</span><span class="sxs-lookup"><span data-stu-id="40dfe-125">Once the `certificate.pfx` file has been generated, configure the HTTPS certificate in your `appsettings.Development.json` file:</span></span>

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

<span data-ttu-id="40dfe-126">Você também precisará especificar a senha para o certificado, definindo a propriedade de configuração "Certificados: HTTPS:Password".</span><span class="sxs-lookup"><span data-stu-id="40dfe-126">You will also need to specify the passphrase for the certificate by setting the “Certificates:HTTPS:Password” config property.</span></span> <span data-ttu-id="40dfe-127">As senhas não devem ser armazenadas em texto sem formatação.</span><span class="sxs-lookup"><span data-stu-id="40dfe-127">Passwords should not be stored in plain text.</span></span> <span data-ttu-id="40dfe-128">Consulte [seguro armazenamento de segredos durante o desenvolvimento de aplicativos](app-secrets.md) para manipular apropriado da senha do certificado.</span><span class="sxs-lookup"><span data-stu-id="40dfe-128">See [Safe Storage of App Secrets During Development](app-secrets.md) for appropriate handling of the certificate passphrase.</span></span>

<span data-ttu-id="40dfe-129">MacOS pode [adicionar o certificado ao seu conjunto de chaves](https://support.apple.com/kb/PH20129?locale=en_US) e [alterar suas configurações de confiança](https://support.apple.com/kb/PH20127?locale=en_US&viewlocale=en_US) para que ele é confiável para HTTPS durante o desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="40dfe-129">On macOS you can [add the certificate to your keychain](https://support.apple.com/kb/PH20129?locale=en_US) and [change its trust settings](https://support.apple.com/kb/PH20127?locale=en_US&viewlocale=en_US) so that it is trusted for HTTPS during development.</span></span> <span data-ttu-id="40dfe-130">Para adicionar o certificado ao seu conjunto de chaves (o equivalente a `CurrentUser/My` armazenar no Windows) execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="40dfe-130">To add the certificate to your keychain (the equivalent of the `CurrentUser/My` store on Windows) run the following command:</span></span>

```bash
security import certificate.pfx -k ~/Library/Keychains/login.keychain-db
```

<span data-ttu-id="40dfe-131">E, em seguida, para confiar no certificado:</span><span class="sxs-lookup"><span data-stu-id="40dfe-131">And then to trust the certificate:</span></span>

```bash
security add-trusted-cert localhost.cer
```

<span data-ttu-id="40dfe-132">Você pode configurar seu aplicativo para usar esse certificado em desenvolvimento, como este:</span><span class="sxs-lookup"><span data-stu-id="40dfe-132">You can then configure your app to use this certificate in development like this:</span></span>

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
