## <a name="call-the-web-api-with-jquery"></a>Chamar a API Web com o jQuery

Nesta seção, é adicionada uma página HTML que usa o jQuery para chamar a API Web. O jQuery inicia a solicitação e atualiza a página com os detalhes da resposta da API.

Configure o projeto para atender arquivos estáticos e habilitar o mapeamento de arquivo padrão. Isso é feito chamando os métodos de extensão [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) e [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) em *Startup.Configure*. Para obter mais informações, veja [Arquivos estáticos](xref:fundamentals/static-files).

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Startup2.cs?name=snippet_Configure&highlight=3-4)]

Adicione um arquivo HTML, chamado *index.html*, ao diretório *wwwroot* do projeto. Substitua seu conteúdo pela seguinte marcação:

[!code-html[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/index.html)]

Adicione um arquivo JavaScript, chamado *site.js*, ao diretório *wwwroot* do projeto. Substitua seu conteúdo pelo código a seguir:

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

Pode ser necessário realizar uma alteração nas configurações de inicialização do projeto do ASP.NET Core para testar a página HTML localmente. Abra *launchSettings.json* no diretório *Propriedades* do projeto. Remova a propriedade `launchUrl` para forçar o aplicativo a ser aberto em *index.html*, o arquivo padrão do projeto.

Há várias maneiras de obter o jQuery. No trecho de código anterior, a biblioteca é carregada de uma CDN. Este é um exemplo completo de CRUD ao chamar a API com o jQuery. Existem recursos adicionais neste exemplo para enriquecer a experiência. Abaixo estão as explicações relacionadas às chamadas à API.

### <a name="get-a-list-of-to-do-items"></a>Obter uma lista de itens pendentes

Para obter uma lista de itens pendentes, envie uma solicitação HTTP GET para */api/todo*.

A função [ajax](https://api.jquery.com/jquery.ajax/) do jQuery envia uma solicitação AJAX para a API, que retorna o JSON que representa um objeto ou uma matriz. Essa função pode lidar com todas as formas de interação de HTTP, enviando uma solicitação HTTP à `url` especificada. `GET` é usado como o `type`. A função de retorno de chamada `success` será invocada se a solicitação for bem-sucedida. No retorno de chamada, o DOM é atualizado com as informações do item pendente.

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a>Adicionar um item pendente

Para adicionar um item pendente, envie uma solicitação HTTP POST para */api/todo/*. O corpo da solicitação deve conter um objeto pendente. A função [ajax](https://api.jquery.com/jquery.ajax/) está usando o `POST` para chamar a API. Para as solicitações `POST` e `PUT`, o corpo da solicitação representa os dados enviados para a API. A API está esperando um corpo de solicitação JSON. As opções `accepts` e `contentType` são definidas como `application/json` para classificar o tipo de mídia que está sendo recebido e enviado, respectivamente. Os dados são convertidos em um objeto JSON usando [`JSON.stringify`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify). Quando a API retorna um código de status de êxito, a função `getData` é invocada para atualizar a tabela HTML.

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a>Atualizar um item pendente

Atualizar um item pendente é muito semelhante a adicionar um item pendente, pois ambas as ações contam com um corpo da solicitação. Nesse caso, a única diferença real entre as duas é que a `url` é alterada para adicionar o identificador exclusivo do item e o `type` é `PUT`.

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a>Excluir um item pendente

A exclusão de um item pendente é feita definindo o `type` na chamada do AJAX como `DELETE` e especificando o identificador exclusivo do item na URL.

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]
