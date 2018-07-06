---
uid: web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
title: Guia de solução de problemas (Razor) de páginas da Web ASP.NET | Microsoft Docs
author: tfitzmac
description: Este artigo descreve problemas que você possa ter ao trabalhar com páginas da Web do ASP.NET (Razor) e algumas soluções sugeridas. Versões de software Pag de Web do ASP.NET...
ms.author: aspnetcontent
ms.date: 02/10/2014
ms.assetid: 2a2c1833-0bfe-4e2e-9cc0-341b52c7b121
msc.legacyurl: /web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
msc.type: authoredcontent
ms.openlocfilehash: 0d0e47ce1b52f0e9ac23a44bbd37ac1a38290dbd
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825157"
---
<a name="aspnet-web-pages-razor-troubleshooting-guide"></a>Guia de solução de problemas (Razor) de páginas da Web ASP.NET
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo descreve problemas que você possa ter ao trabalhar com páginas da Web do ASP.NET (Razor) e algumas soluções sugeridas.
> 
> ## <a name="software-versions"></a>Versões de software
> 
> 
> - Páginas da Web do ASP.NET (Razor) 3
>   
> 
> Este tutorial também funciona com ASP.NET Web Pages 2 e páginas da Web de ASP.NET 1.0.


Esse tópico contém as seguintes seções:

- [Problemas com a execução de páginas](#Issues_Running_.cshtml_Pages)
- [Problemas com o código do Razor](#IssuesWithRazorCode)
- [Problemas com a segurança e associação](#membership)
- [Problemas com o envio de Email](#email)
- [Recursos adicionais](#AdditionalResources)

Para perguntas gerais, consulte [páginas da Web do ASP.NET (Razor) perguntas frequentes sobre](https://go.microsoft.com/fwlink/?LinkId=253000).

<a id="Issues_Running_.cshtml_Pages"></a>
## <a name="issues-with-running-pages"></a>Problemas com a execução de páginas

Uma variedade de problemas pode impedir *. cshtml* e *. vbhtml* páginas sejam executados corretamente. Esta seção lista mensagens de erro comuns e causas mais prováveis.

### <a name="http-error-403---forbidden-access-is-denied"></a>Erro HTTP 403 - Proibido: Acesso negado

*Você não tem permissão para exibir este diretório ou página usando as credenciais que você forneceu.*

Esse erro pode ocorrer se o servidor não está executando a versão correta do .NET Framework. Certifique-se de que o computador que está executando o servidor (local ou remotamente) tenha pelo menos o .NET Framework 4 instalado. Além disso, certifique-se de que o aplicativo em si é configurado para executar a versão correta.

Se você vir esse problema localmente enquanto estiver trabalhando no WebMatrix, clique o **Site** espaço de trabalho e, em que o modo de exibição de árvore, clique em **configurações**. No **selecione a versão do .NET Framework** , selecione **(integrado) do .NET 4**. Se essa versão já está definido, tente executar o WebMatrix como administrador.

Certifique-se de que a raiz do seu site tenha pelo menos um *. cshtml* arquivo nele.

Se você vir esse erro quando o servidor web está em um servidor remoto, entre em contato com o administrador do servidor. Certifique-se de que o servidor tem o .NET Framework 4 ou posterior instalado. Além disso, certifique-se de que o aplicativo é executado em um pool de aplicativos está configurado para usar essa versão do.NET Framework.

Se você tiver controle sobre o servidor, verifique se que ele está executando a versão correta do .NET Framework. Você também pode tentar reparar a instalação executando o `aspnet_regiis -iru` comando. (Por exemplo, se você instalar o IIS depois de instalar o .NET Framework, o IIS não será ser corretamente configurado para executar as páginas ASP.NET.) Para obter mais informações, consulte [ferramenta de registro ASP.NET IIS (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h(v=vs.100).aspx).

### <a name="http-error-40314---forbidden"></a>Erro de HTTP 403.14 - proibido

*O servidor Web está configurado para não listar o conteúdo desse diretório.*

Esse erro pode ocorrer se você solicitar um recurso protegido (como o *Web. config* arquivo) ou em uma pasta protegida (como *App\_dados* ou *aplicativo\_Código*).

### <a name="http-error-40417---not-found"></a>Erro de HTTP 404.17 – não encontrado

*O conteúdo solicitado parece ser o script e não será servido pelo manipulador de arquivo estático.*

Esse erro pode ocorrer se o servidor não está configurado corretamente para usar o .NET Framework 4 ou posterior e, portanto, não reconhece o código em `@{ }` blocos. Consulte a descrição anteriormente por *HTTP Erro 403 - Proibido: acesso negado*.

### <a name="http-error-4047---not-found"></a>Erro de HTTP 404.7 - não encontrado

*O módulo de filtragem de solicitações é configurada para negar a extensão de arquivo*

Esse erro pode ocorrer se *. cshtml* ou *. vbhtml* extensões foram bloqueadas explicitamente no servidor. Um sintoma desse problema é que o trabalho URLs quando elas não incluem a extensão, mas as URLs que incluem *. cshtml* ou *. vbhtml* não funcionam. Uma solução possível é habilitar novamente as extensões do site *Web. config* arquivo. O exemplo a seguir mostra como habilitar o *. cshtml* extensão.

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample1.xml?highlight=5-6)]

### <a name="http-error-4048---not-found"></a>Erro de HTTP 404.8 - não encontrado

*O módulo de filtragem de solicitações está configurada para negar a um caminho da URL que contém uma seção hiddenSegment.*

Esse erro pode ocorrer se você solicitar um recurso protegido (como o *Web. config* arquivo) ou em uma pasta protegida (como *App\_dados* ou *aplicativo\_Código*).

### <a name="this-type-of-page-is-not-served-server-error-in--application"></a>Esse tipo de página não é atendido (erro de servidor no aplicativo '/')

Consulte a descrição anterior de HTTP 404.17 de erro.

<a id="IssuesWithRazorCode"></a>
## <a name="issues-with-razor-code"></a>Problemas com o código do Razor

### <a name="the-name-class-does-not-exist-in-the-current-context"></a>O nome '*classe*' não existe no contexto atual

Geralmente, um motivo que você vir esse erro é que `class` referências de um auxiliar, mas o auxiliar não está instalado. Por exemplo, se você tentar usar um auxiliar, mas se você ainda não instalou o pacote do NuGet, você verá esse erro. Use a galeria no WebMatrix para encontrar e instalar o auxiliar.

Se o auxiliar está instalado, mas a página ainda não reconhecê-lo, tente adicionar adicionar um `using` instrução no código. No `using` instrução, o namespace que inclui o auxiliar de referência. Por exemplo, os auxiliares básicos que estão no pacote auxiliares do ASP.NET Web estão no `System.Web.Helpers` namespace. Na parte superior da página em que você deseja usar o auxiliar, adicione esta linha:

`@using Microsoft.Web.Helpers;`

<a id="membership"></a>
## <a name="issues-with-security-and-membership"></a>Problemas com a segurança e associação

Se você estiver usando o sistema de segurança internas (associação) em páginas da Web do ASP.NET (Razor), você pode encontrar os problemas a seguir.

### <a name="to-call-this-method-the-membershipprovider-property-must-be-an-instance-of-extendedmembershipprovider"></a>Para chamar esse método, a propriedade "Membership.Provider" deve ser uma instância de "ExtendedMembershipProvider"

Esse erro pode indicar que nenhum `AspNetSqlMembershipProvider` classe está configurada. (Um sintoma é que o site funciona bem localmente, mas gera este erro quando você publicá-lo no servidor de um provedor de hospedagem). Uma correção para esse problema é habilitar explicitamente a associação simples, adicionando o seguinte para o site *Web. config* arquivo:

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample2.xml?highlight=6)]

<a id="email"></a>
## <a name="issues-with-sending-email"></a>Problemas com o envio de Email

Problemas com o envio de email podem ser um desafio de depurar. Um problema inicial pode ser que você não pode se conectar ao servidor SMTP. Se a conexão for bem-sucedida, ASP.NET entrega a mensagem para o servidor SMTP. No entanto, pode haver problemas com a mensagem em si que impede que o servidor SMTP enviá-la.

Se seu aplicativo não enviar email com êxito, tente o seguinte:

- O nome do servidor SMTP costuma ser algo como `smtp.provider.com` ou `smtp.provider.net`. No entanto, se você publicar seu site em um provedor de hospedagem, o nome do servidor SMTP nesse ponto pode ser `localhost`. Essa situação ocorre porque depois que você publicou e seu site está em execução no servidor do provedor, o servidor SMTP pode ser local da perspectiva do seu aplicativo. Essa alteração de nomes de servidor pode significar que você precisa alterar o nome do servidor SMTP como parte de seu processo de publicação.
- Geralmente, o número da porta é 25. No entanto, alguns provedores exigem que você usar a porta 587 ou alguma outra porta. Verifique com o proprietário do servidor SMTP de qual número de porta que eles esperam que você use.
- Certifique-se de que você use as credenciais corretas. Se você publicar seu site em um provedor de hospedagem, use as credenciais que o provedor indicou especificamente são para email. Essas credenciais podem ser diferentes das credenciais que usar para publicar.
- Às vezes, você não precisa credenciais em todos os. Se você estiver enviando um email por meio de seu ISP pessoal, seu provedor de email talvez já saiba suas credenciais. Depois de publicar, você talvez precise usar credenciais diferentes quando você testa em seu computador local.
- Se seu provedor de email usa a criptografia, defina `WebMail.EnableSsl` para `true`.

Se houver um erro ao enviar email, você poderá ver uma mensagem de erro padrão do ASP.NET, que tem esta aparência:

![Mensagem de erro do ASP.NET quando há um problema com o email](aspnet-web-pages-razor-troubleshooting-guide/_static/image1.png)

Você também pode depurar problemas com o envio de email usando um `try-catch` bloco, como no exemplo a seguir. Quando você usa um `try-catch` bloco, o ASP.NET não exibe suas mensagens de erro padrão. Em vez disso, você pode capturar o erro no `catch` parte do bloco.

[!code-cshtml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample3.cshtml)]

Substitua os valores apropriados para `your-SMTP-server-name`e assim por diante. Algumas das mensagens de erro, que talvez você veja dessa forma incluem o seguinte:

- *Falha ao enviar email.*

    -ou-

    *Uma tentativa de conexão falhou porque a parte conectada não respondeu corretamente após um período de tempo ou a conexão estabelecida falhou porque o host conectado não respondeu*

    Esse erro geralmente significa que o aplicativo não pôde se conectar ao servidor SMTP. Verifique o nome do servidor e número da porta.
- <em>Caixa de correio indisponível. A resposta do servidor foi: 5.1.0 &lt; someuser@invaliddomain &gt; remetente rejeitado: domínio do remetente inválido</em>

    Essa mensagem pode indicar que o `From` endereço não está correto ou está ausente.
- *A cadeia de caracteres especificada não está no formato necessário para um endereço de email.*

    Esse erro pode indicar que o valor de `To` ou `From` propriedades não são reconhecidas como endereços de email. (ASP.NET não é possível verificar se o endereço de email é válido, apenas que ele do dentro o formato correto, como *name@domain.com*.)

> [!NOTE]
> Remova a marcação que exibe o erro (`@errorMessage`) antes de publicar a página em um site ativo. Não é uma boa ideia permitir que os usuários a ver as mensagens de erro que você obtém de um servidor.


<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Recursos adicionais

[Perguntas frequentes sobre Páginas da Web do ASP.NET (Razor)](https://go.microsoft.com/fwlink/?LinkId=253000)

[O WebMatrix e páginas da Web ASP.NET](https://forums.asp.net/1224.aspx/1?WebMatrix) fórum no site do ASP.NET
