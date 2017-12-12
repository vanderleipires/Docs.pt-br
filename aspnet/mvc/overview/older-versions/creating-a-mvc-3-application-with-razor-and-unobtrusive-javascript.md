---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: Criando um MVC 3 aplicativo com Razor e o JavaScript discreto | Microsoft Docs
author: microsoft
description: "O aplicativo de web de exemplo da lista de usuários demonstra como é simples para criar aplicativos ASP.NET MVC 3 usando o mecanismo de exibição Razor. S de aplicativo de exemplo..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/01/2010
ms.topic: article
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: 68870caf1608e596962650cf653e5b455b82382a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a>Criando um MVC 3 aplicativo com Razor e o JavaScript discreto
====================
por [Microsoft](https://github.com/microsoft)

> O aplicativo de web de exemplo da lista de usuários demonstra como é simples para criar aplicativos ASP.NET MVC 3 usando o mecanismo de exibição Razor. O aplicativo de exemplo mostra como usar o novo mecanismo de exibição Razor com o ASP.NET MVC versão 3 e o Visual Studio 2010 para criar um site de lista de usuários fictício que inclui a funcionalidade de como criar, exibir, editar e excluir usuários.
> 
> Este tutorial descreve as etapas que foram usadas para criar o aplicativo do ASP.NET MVC 3 de exemplo lista de usuários. Um projeto do Visual Studio com o código-fonte c# e VB está disponível para acompanhar este tópico: [baixar](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114). Se você tiver dúvidas sobre este tutorial, poste-os para o [Fórum do MVC](https://forums.asp.net/1146.aspx).


## <a name="overview"></a>Visão Geral

O aplicativo que você criará é um site da lista de usuário simples. Os usuários podem inserir, exibir e atualizar informações de usuário.

![Site de exemplo](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

Você pode baixar o projeto concluído VB e c# [aqui](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).

## <a name="creating-the-web-application"></a>Criando o aplicativo Web

Para iniciar o tutorial, abra o Visual Studio 2010 e crie um novo projeto usando o *aplicativo Web do ASP.NET MVC 3* modelo. Nomeie o aplicativo &quot;Mvc3Razor&quot;.

[![Novo projeto MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)

No **novo projeto do ASP.NET MVC 3** caixa de diálogo, selecione **aplicativo de Internet**, selecione o mecanismo de exibição Razor e, em seguida, clique em **Okey**.

![Caixa de diálogo Nova projeto do ASP.NET MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

Neste tutorial você não usará o provedor de associação do ASP.NET, portanto você pode excluir todos os arquivos associados ao logon e associação. Em **Solution Explorer**, remova os arquivos e diretórios a seguir:

- *Controllers\AccountController*
- *Models\AccountModels*
- *Exibições \ compartilhadas\\_LogOnPartial*
- *Views\Account* (e todos os arquivos neste diretório)

![Soln Exp](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

Editar o  *\_cshtml* de arquivo e substitua a marcação dentro de `<div>` elemento chamado `logindisplay` com a mensagem  *&quot;* logon desabilitado&quot;. O exemplo a seguir mostra a marcação novo:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a>Adicionar modelo

Em **Solution Explorer**, com o botão direito do *modelos* pasta, selecione **adicionar**e, em seguida, clique em **classe**.

![Nova classe de usuário Mdl](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

Nomeie a classe `UserModel`. Substitua o conteúdo do *UserModel* arquivo com o código a seguir:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

O `UserModel` classe representa os usuários. Cada membro da classe é anotado com a [necessário](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.requiredattribute.aspx) de atributo do [DataAnnotations](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx) namespace. Os atributos de [DataAnnotations](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx) namespace fornecem validação automática de lado cliente e servidor para aplicativos da web.

Abra o `HomeController` classe e adicione um `using` diretiva para que você possa acessar o `UserModel` e `Users` classes:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

Logo após o `HomeController` declaração, adicione o seguinte comentário e a referência a um `Users` classe:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

O `Users` classe é um repositório de dados simplificada, de memória que você usará neste tutorial. Em um aplicativo real, você usaria um banco de dados para armazenar informações do usuário. As primeiras linhas do `HomeController` arquivo são mostradas no exemplo a seguir:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

Compile o aplicativo para que o modelo de usuário estarão disponível para o Assistente de scaffolding na próxima etapa.

## <a name="creating-the-default-view"></a>Criar modo de exibição padrão

A próxima etapa é adicionar um método de ação e o modo de exibição para exibir os usuários.

Exclua o existente *Views\Home\Index* arquivo. Você criará um novo *índice* arquivo para exibir os usuários.

No `HomeController` classe, substitua o conteúdo do `Index` método com o código a seguir:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

Clique dentro do `Index` método e depois clique em **adicionar exibição**.

![Adicionar modo de exibição](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

Selecione o **criar uma exibição fortemente tipada** opção. Para **exibir dados de classe**, selecione **Mvc3Razor.Models.UserModel**. (Se você não vir **Mvc3Razor.Models.UserModel** no **exibir dados de classe** caixa, você precisa compilar o projeto.) Certifique-se de que o mecanismo de exibição está definido como **Razor**. Definir **exibir conteúdo** para **lista** e, em seguida, clique em **adicionar**.

![Adicionar exibição de índice](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

A nova exibição scaffolds automaticamente os dados do usuário que são passados para o `Index` exibição. Examine o recém-gerado *Views\Home\Index* arquivo. O **criar novo**, **editar**, **detalhes**, e **excluir** links não funcionam, mas o restante da página é funcional. Execute a página. Você verá uma lista de usuários.

![Página de índice](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

Abra o *cshtml* de arquivo e substitua o `ActionLink` marcação para **editar**, **detalhes**, e **excluir** com o código a seguir :

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

O nome de usuário é usado como a ID para localizar o registro selecionado no **editar**, **detalhes**, e **excluir** links.

## <a name="creating-the-details-view"></a>Criar a exibição de detalhes

A próxima etapa é adicionar um `Details` método de ação e o modo de exibição para exibir detalhes do usuário.

![Detalhes](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

Adicione o seguinte `Details` método para o controlador principal:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

Clique dentro do `Details` método e, em seguida, selecione **adicionar exibição**. Verifique o **exibir dados de classe** caixa contém **Mvc3Razor.Models.UserModel***.* Definir **exibir conteúdo** para **detalhes** e, em seguida, clique em **adicionar**.

![Adicionar exibição de detalhes](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

Execute o aplicativo e selecione um link de detalhes. O scaffolding automática mostra cada propriedade no modelo.

![Detalhes](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a>Criar a exibição de edição

Adicione o seguinte `Edit` método para o controlador principal.

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

Adicionar um modo de exibição como as etapas anteriores, mas definir **exibir conteúdo** para **editar**.

![Adicionar exibição de edição](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

Execute o aplicativo e edite o nome e sobrenome de um dos usuários. Se você violar qualquer `DataAnnotation` restrições que foram aplicadas a `UserModel` classe, ao enviar o formulário, você verá erros de validação que são produzidos pelo código do servidor. Por exemplo, se você alterar o nome &quot;Ann&quot; para &quot;um&quot;, quando você enviar o formulário, o seguinte erro é exibido no formulário:

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

Neste tutorial, você está tratando o nome de usuário como a chave primária. Portanto, a propriedade de nome de usuário não pode ser alterada. No *Edit.cshtml* arquivo, logo após o `Html.BeginForm` instrução, defina o nome de usuário para ser um campo oculto. Isso faz com que a propriedade a ser passado no modelo. O fragmento de código a seguir mostra a colocação do `Hidden` instrução:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

Substitua o `TextBoxFor` e `ValidationMessageFor` marcação para o nome de usuário com um `DisplayFor` chamar. O `DisplayFor` método exibe a propriedade como um elemento somente leitura. O exemplo a seguir mostra a marcação concluída. O original `TextBoxFor` e `ValidationMessageFor` chamadas são comentadas com os caracteres de comentário begin e end comentário do Razor (`@* *@`)

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a>Habilitar a validação do lado do cliente

Para habilitar a validação do lado do cliente no ASP.NET MVC 3, você deve definir dois sinalizadores e você deve incluir três arquivos JavaScript.

Abra o aplicativo *Web. config* arquivo. Verifique se `that ClientValidationEnabled` e `UnobtrusiveJavaScriptEnabled` são definidas como true nas configurações do aplicativo. O seguinte fragmento de raiz *Web. config* arquivo mostra as configurações corretas:

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

Definindo `UnobtrusiveJavaScriptEnabled` como true Ajax discreto habilita e validação de cliente discreto. Quando você usar a validação discreta, as regras de validação são transformadas em atributos do HTML5. Nomes de atributo do HTML5 podem consistir apenas letras minúsculas, números e traços.

Definindo `ClientValidationEnabled` para true habilita a validação do lado do cliente. Definindo essas chaves no aplicativo *Web. config* arquivo, habilitar a validação do cliente e o JavaScript discreto para todo o aplicativo. Você também pode habilitar ou desabilitar essas configurações em exibições individuais ou em métodos do controlador usando o seguinte código:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

Você também precisa incluir vários arquivos JavaScript no modo de exibição renderizado. É uma maneira fácil de incluir o JavaScript em todas as exibições para adicioná-los para o *exibições \ compartilhadas\\cshtml* arquivo. Substitua o `<head>` elemento o  *\_cshtml* arquivo com o código a seguir:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

Os primeiros dois scripts de jQuery são hospedados do Microsoft Ajax Content Delivery Network (CDN). Aproveitando a CDN do Microsoft Ajax, você pode melhorar significativamente o desempenho de primeira visita de seus aplicativos.

Execute o aplicativo e clique em um link de edição. Exiba código-fonte da página no navegador. A origem do navegador mostra muitos atributos do formulário `data-val` (para validação de dados). Quando a validação do cliente e o JavaScript discreto está habilitado, os campos de entrada com uma regra de validação do cliente contém o `data-val="true"` atributo para disparar a validação do cliente discreto. Por exemplo, o `City` campo no modelo foi decorado com o [necessário](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.requiredattribute.aspx) atributo, o que resulta em HTML mostrado no exemplo a seguir:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

Para cada regra de validação do cliente, um atributo é adicionado, que tem a forma `data-val-rulename="message"`. Usando o `City` campo exemplo mostrado anteriormente, a regra de validação de cliente necessário gera o `data-val-required` atributo e a mensagem &quot;cidade o campo é obrigatório&quot;. Executar o aplicativo, edite os usuários e limpar o `City` campo. Ao pressionar a tecla tab para fora do campo, você vê uma mensagem de erro de validação do lado do cliente.

![Cidade necessária](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

Da mesma forma, para cada parâmetro na regra de validação do cliente, um atributo é adicionado que tem o formato `data-val-rulename-paramname=paramvalue`. Por exemplo, o `FirstName` propriedade está anotada com o [StringLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) de atributo e especifica um comprimento mínimo de 3 e um comprimento máximo de 8. A regra de validação de dados denominada `length` tem o nome do parâmetro `max` e o valor do parâmetro 8. A seguir mostra o HTML que é gerado para o `FirstName` campo quando você edita um dos usuários:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

Para obter mais informações sobre a validação do cliente discreto, consulte a entrada [discreto validação do cliente no ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) no blog de Brad Wilson.

> [!NOTE]
> No ASP.NET MVC 3 Beta, às vezes você precisa enviar o formulário para iniciar a validação do lado do cliente. Isso pode ser alterado para a versão final.


## <a name="creating-the-create-view"></a>Criando o Create View

A próxima etapa é adicionar um `Create` método de ação e o modo de exibição para permitir que o usuário crie um novo usuário. Adicione o seguinte `Create` método para o controlador principal:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

Adicionar um modo de exibição como as etapas anteriores, mas definir **exibir conteúdo** para **criar**.

![Criar modo de exibição](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

Executar o aplicativo, selecione o **criar** link e adicionar um novo usuário. O `Create` método automaticamente tira proveito da validação do lado do cliente e do servidor. Tente digitar um nome de usuário que contém espaço em branco, como &quot;Ben X&quot;. Quando você guia fora do campo de nome de usuário, um erro de validação do lado do cliente (`White space is not allowed`) é exibido.

## <a name="add-the-delete-method"></a>Adicione o método Delete

Para concluir o tutorial, adicione o seguinte `Delete` método para o controlador principal:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

Adicionar um `Delete` exibição como as etapas anteriores, definindo **exibir conteúdo** para **excluir**.

![Excluir exibição](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

Agora você tem um aplicativo ASP.NET MVC 3 simple, mas totalmente funcional com a validação.
