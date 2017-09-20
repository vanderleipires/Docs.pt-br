---
title: "Páginas de ajuda da API Web ASP.NET Core usando o Swagger"
author: spboyer
description: "Este tutorial fornece um passo a passo da adição de Swagger para gerar a documentação e páginas Ajuda para um aplicativo de API Web."
keywords: "ASP.NET Core, Swagger, Swashbuckle, páginas de ajuda, API Web"
ms.author: spboyer
manager: wpickett
ms.date: 09/01/2017
ms.topic: article
ms.assetid: 54bb961d-29d9-4dee-8e2c-a93fc33c16f2
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/web-api-help-pages-using-swagger
ms.openlocfilehash: 647ab48fb83c5e2c79b5de371173bc644c65d831
ms.sourcegitcommit: 98ecb0f1bae4886507b090c84ecd99ff1e5c46ed
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/17/2017
---
# <a name="aspnet-web-api-help-pages-using-swagger"></a>Páginas de ajuda da API Web ASP.NET usando o Swagger

<a name=web-api-help-pages-using-swagger></a>

Por [Shayne Boyer](https://twitter.com/spboyer) e [Scott Addie](https://twitter.com/Scott_Addie)

Compreender os vários métodos de uma API pode ser um desafio para um desenvolvedor durante a criação de um aplicativo de consumo.

A geração de boa documentação e páginas de ajuda para a API Web, usando [Swagger](https://swagger.io/) com a implementação do .NET Core [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore), é tão fácil quanto adicionar alguns pacotes NuGet e modificar o *Startup.cs*.

* O [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) é um projeto de software livre para geração de documentos do Swagger para APIs Web ASP.NET Core.

* O [Swagger](https://swagger.io/) é uma representação legível por computador de uma API RESTful que habilita o suporte para documentação interativa, geração de SDK do cliente e detectabilidade.

Este tutorial se baseia no exemplo [Criando sua primeira API Web com ASP.NET Core MVC e Visual Studio](xref:tutorials/first-web-api). Se você quiser acompanhar, baixe a amostra em [https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample).

## <a name="getting-started"></a>Guia de Introdução

Há três componentes principais bo Swashbuckle:

* `Swashbuckle.AspNetCore.Swagger`: um modelo de objeto de Swagger e middleware para expor objetos `SwaggerDocument` como pontos de extremidade JSON.

* `Swashbuckle.AspNetCore.SwaggerGen`: um gerador de Swagger cria objetos `SwaggerDocument` diretamente dos modelos, controladores e rotas. Normalmente, ele é combinado com o middleware de ponto de extremidade do Swagger para expor automaticamente o JSON do Swagger.

* `Swashbuckle.AspNetCore.SwaggerUI`: uma versão inserida da ferramenta de interface do usuário Swagger, que interpreta JSON do Swagger a fim de criar uma experiência rica e personalizável para descrever a funcionalidade da API Web. Ela inclui o agente de teste interno para os métodos públicos.

## <a name="nuget-packages"></a>Pacotes NuGet

O Swashbuckle pode ser adicionado com as seguintes abordagens:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Da janela **Console do Gerenciador de Pacotes**:

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* Da caixa de diálogo **Gerenciar Pacotes NuGet**:

     * Clique com o botão direito do mouse no projeto em **Gerenciador de Soluções** > **Gerenciar Pacotes NuGet**
     * Defina a **Origem do pacote** para "nuget.org"
     * Insira "Swashbuckle.AspNetCore" na caixa de pesquisa
     * Selecione o pacote "Swashbuckle.AspNetCore" na guia **Procurar** e clique em **Instalar**

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

* Clique com o botão direito do mouse na pasta *Pacotes* em **Painel de Soluções** > **Adicionar Pacotes...**
* Defina a lista suspensa **Origem** da janela **Adicionar Pacotes** para "nuget.org"
* Insira Swashbuckle.AspNetCore na caixa de pesquisa
* Selecione o pacote "Swashbuckle.AspNetCore" no painel de resultados e clique em **Adicionar Pacote**

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Execute o comando a seguir do **Terminal Integrado**:

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[CLI do .NET Core](#tab/netcore-cli)

Execute o seguinte comando:

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-to-the-middleware"></a>Adicionar e configurar o Swagger para o middleware

Adicione o gerador de Swagger à coleção de serviços no método `ConfigureServices` de *Startup.cs*:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_ConfigureServices&highlight=7-10)]

Adicione a seguinte instrução using da classe `Info`:

```csharp
using Swashbuckle.AspNetCore.Swagger;
```

No método `Configure` de *Startup.cs*, habilite o middleware para servir o documento JSON gerado e o SwaggerUI:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_Configure&highlight=4,7-10)]

Inicie o aplicativo e navegue até `http://localhost:<random_port>/swagger/v1/swagger.json`. O documento gerado que descreve os pontos de extremidade é exibido.

**Observação:** Microsoft Edge, Google Chrome e Firefox exibem documentos JSON nativamente. Há extensões para o Chrome que formatam o documento para facilitar a leitura. *O exemplo a seguir é reduzido para fins de brevidade.*

```json
{
   "swagger": "2.0",
   "info": {
       "version": "v1",
       "title": "API V1"
   },
   "basePath": "/",
   "paths": {
       "/api/Todo": {
           "get": {
               "tags": [
                   "Todo"
               ],
               "operationId": "ApiTodoGet",
               "consumes": [],
               "produces": [
                   "text/plain",
                   "application/json",
                   "text/json"
               ],
               "responses": {
                   "200": {
                       "description": "Success",
                       "schema": {
                           "type": "array",
                           "items": {
                               "$ref": "#/definitions/TodoItem"
                           }
                       }
                   }
                }
           },
           "post": {
               ...
           }
       },
       "/api/Todo/{id}": {
           "get": {
               ...
           },
           "put": {
               ...
           },
           "delete": {
               ...
   },
   "definitions": {
       "TodoItem": {
           "type": "object",
            "properties": {
                "id": {
                    "format": "int64",
                    "type": "integer"
                },
                "name": {
                    "type": "string"
                },
                "isComplete": {
                    "default": false,
                    "type": "boolean"
                }
            }
       }
   },
   "securityDefinitions": {}
}
```

Este documento orienta sobre a interface do usuário do Swagger, que pode ser exibida navegando para `http://localhost:<random_port>/swagger`:

![Interface do usuário do Swagger](web-api-help-pages-using-swagger/_static/swagger-ui.png)

Cada método de ação pública em `TodoController` pode ser testado da interface do usuário. Clique em um nome de método para expandir a seção. Adicione quaisquer parâmetros necessários e clique em "Experimente!".

![Teste GET de Swagger de exemplo](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

## <a name="customization--extensibility"></a>Personalização e extensibilidade

O Swagger fornece opções para documentar o modelo de objeto e personalizar a interface do usuário para corresponder ao seu tema.

### <a name="api-info-and-description"></a>Informações e descrição da API

A ação de configuração passada para o método `AddSwaggerGen` pode ser usada para adicionar informações como o autor, a licença e a descrição:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?range=20-30,36)]

A imagem a seguir representa a interface do usuário do Swagger exibindo as informações de versão:

![A interface do usuário do Swagger com informações de versão: descrição, autor e link veja mais](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a>Comentários XML

Comentários XML podem ser habilitados com as seguintes abordagens:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Clique com o botão direito do mouse no projeto no **Gerenciador de Soluções** e selecione **Propriedades**
* Verifique a caixa **Arquivo de documentação XML** sob a seção **Saída** da guia **Build**:

![Compile a guia das propriedades do projeto](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

* Abra a caixa de diálogo **Opções do Projeto** > **Compilar** > **Compilador**
* Verifique a caixa **Gerar documentação XML** sob a seção **Opções Gerais**:

![Seção Opções Gerais das opções do projeto](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Adicione manualmente o trecho a seguir ao arquivo *.csproj*:

[!code-xml[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/TodoApi.csproj?range=7-9)]

---

Configure o Swagger para usar o arquivo XML gerado. Para sistemas operacionais Linux ou não Windows, caminhos e nomes de arquivo podem diferenciar maiúsculas de minúsculas. Por exemplo, um arquivo *ToDoApi.XML* pode ser encontrado no Windows, mas não em CentOS.

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_ConfigureServices&highlight=20-22)]

No código anterior, `ApplicationBasePath` obtém o caminho base do aplicativo. O caminho base é usado para localizar o arquivo de comentários XML. *TodoApi.xml* funciona apenas para este exemplo, desde que o nome do arquivo de comentários XML gerado seja baseado no nome do aplicativo.

Adicionar comentários de barra tripla ao método aprimora a interface do usuário do Swagger adicionando a descrição ao cabeçalho da seção:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete&highlight=2)]

![A interface do usuário do Swagger, mostrando o comentário XML 'Exclui um TodoItem específico'. para o método DELETE](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

A interface do usuário é controlada pelo arquivo JSON gerado, que também contém estes comentários:

```json
"delete": {
    "tags": [
        "Todo"
    ],
    "summary": "Deletes a specific TodoItem.",
    "operationId": "ApiTodoByIdDelete",
    "consumes": [],
    "produces": [],
    "parameters": [
        {
            "name": "id",
            "in": "path",
            "description": "",
            "required": true,
            "type": "integer",
            "format": "int64"
        }
    ],
    "responses": {
        "200": {
            "description": "Success"
        }
    }
}
```

Adicione uma marca [<remarks>](https://docs.microsoft.com/dotnet/csharp/programming-guide/xmldoc/remarks) à documentação do método de ação `Create`. Ele suplementa as informações especificadas na marca `<summary>` e fornece uma interface do usuário do Swagger mais robusta. O conteúdo de marca `<remarks>` pode consistir em texto, JSON ou XML.

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

Observe os aprimoramentos de interface do usuário com esses comentários adicionais.

![Interface do usuário do Swagger com comentários adicionais mostrados](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a>Anotações de dados

Decore o modelo com atributos encontrados em `System.ComponentModel.DataAnnotations` para ajudar a controlar os componentes de interface do usuário do Swagger.

Adicione o atributo `[Required]` à propriedade `Name` da classe `TodoItem`:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Models/TodoItem.cs?highlight=10)]

A presença desse atributo altera o comportamento da interface do usuário e altera o esquema JSON subjacente:

```json
"definitions": {
    "TodoItem": {
        "required": [
            "name"
        ],
        "type": "object",
        "properties": {
            "id": {
                "format": "int64",
                "type": "integer"
            },
            "name": {
                "type": "string"
            },
            "isComplete": {
                "default": false,
                "type": "boolean"
            }
        }
    }
},
```

Adicione o atributo `[Produces("application/json")]` ao controlador da API. A finalidade dele é declarar que as ações do controlador dão suporte a retornar um tipo de conteúdo de *application/json*:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_TodoController&highlight=3)]

A lista suspensa **Tipo de Conteúdo de Resposta** seleciona esse tipo de conteúdo como o padrão para ações GET do controlador:

![Interface do usuário do Swagger com o tipo de conteúdo de resposta padrão](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

À medida que aumenta o uso de anotações de dados na API Web, a interface do usuário e as páginas de ajuda da API se tornam mais descritivas e úteis.

### <a name="describing-response-types"></a>Descrevendo os tipos de resposta

Os desenvolvedores de consumo estão mais preocupados com o que é retornado &mdash; especificamente, tipos de resposta e códigos de erro (se não padrão). Eles são manipulados nos comentários XML e anotações de dados.

A ação `Create` retorna `201 Created` em caso de êxito ou `400 Bad Request` quando o corpo da solicitação postada é nulo. Sem a documentação adequada na interface do usuário do Swagger, o consumidor não tem conhecimento desses resultados esperados. Esse problema é corrigido adicionando as linhas destacadas no exemplo a seguir:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

A interface do usuário do Swagger agora documenta claramente os códigos de resposta HTTP esperados:

![A interface do usuário do Swagger mostra a descrição da classe de resposta POST, 'Retorna o item de tarefa pendente recém-criado' e '400 – se o item for nulo' para o código de status e o motivo em Mensagens de Resposta](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customizing-the-ui"></a>Personalizando a interface do usuário

A interface do usuário de estoque é funcional e também apresentável; no entanto, ao criar páginas de documentação para sua API, você deseja que ela represente sua marca ou tema. A realização dessa tarefa com os componentes de Swashbuckle requer a adição de recursos para servir arquivos estáticos e, em seguida, criar a estrutura de pastas para hospedar esses arquivos.

Se você usar o .NET Framework como destino, adicione o pacote NuGet `Microsoft.AspNetCore.StaticFiles` ao projeto:

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

Habilite o middleware de arquivos estáticos:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_Configure&highlight=3)]

Adquira o conteúdo da pasta *dist* do [repositório GitHub da interface do usuário do Swagger](https://github.com/swagger-api/swagger-ui/tree/2.x/dist). Essa pasta contém os ativos necessários para a página da interface do usuário do Swagger. Copie o conteúdo dessa pasta para a pasta *wwwroot/swagger/ui*.

Criar um arquivo *wwwroot/swagger/ui/css/custom.css* com o CSS a seguir para personalizar o cabeçalho da página:

[!code-css[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/css/custom.css)]

Referencie *custom.css* no arquivo *index.html*:

[!code-html[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/index.html?range=14)]

Navegue até a página *index.html* em `http://localhost:<random_port>/swagger/ui/index.html`. Digite `http://localhost:<random_port>/swagger/v1/swagger.json` na caixa de texto do cabeçalho e clique no botão **Explorar**. A página resultante será semelhante ao seguinte:

![Interface do usuário do Swagger com o título do cabeçalho personalizado](web-api-help-pages-using-swagger/_static/custom-header.png)

Há muito mais que você pode fazer com a página. Consulte as funcionalidades completas para os recursos de interface do usuário no [repositório GitHub da interface do usuário do Swagger](https://github.com/swagger-api/swagger-ui).
