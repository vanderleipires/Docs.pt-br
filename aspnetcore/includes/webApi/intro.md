## <a name="overview"></a>Visão Geral

Este tutorial cria a seguinte API:

|API | Descrição | Corpo da solicitação | Corpo da resposta |
|--- | ---- | ---- | ---- |
|GET /api/todo | Obter todos os itens de tarefas pendentes | Nenhum | Matriz de itens de tarefas pendentes|
|GET /api/todo/{id} | Obter um item por ID | Nenhum | Item de tarefas pendentes|
|POST /api/todo | Adicionar um novo item | Item de tarefas pendentes | Item de tarefas pendentes |
|PUT /api/todo/{id} | Atualizar um item &nbsp; existente | Item de tarefas pendentes | Nenhum |
|DELETE /api/todo/{id} &nbsp; &nbsp; | Excluir um item &nbsp; &nbsp; | Nenhum | Nenhum|

<br>

O diagrama a seguir mostra o design básico do aplicativo.

![O cliente é representado por uma caixa à esquerda e envia uma solicitação e recebe uma resposta do aplicativo, uma caixa desenhada à direita. Dentro da caixa do aplicativo, três caixas representam o controlador, o modelo e a camada de acesso a dados. A solicitação é recebida no controlador do aplicativo e as operações de leitura/gravação ocorrem entre o controlador e a camada de acesso a dados. O modelo é serializado e retornado para o cliente na resposta.](../../tutorials/first-web-api/_static/architecture.png)

* O cliente é tudo o que consome a API Web (aplicativo móvel, navegador, etc.). Este tutorial não cria um cliente. [Postman](https://www.getpostman.com/) ou [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html) são usados como clientes para testar o aplicativo.

* Um *modelo* é um objeto que representa os dados no aplicativo. Nesse caso, o único modelo é um item de tarefas pendentes. Modelos são representados como classes c#, também conhecidos como **P**lain **O**ld **C**# **O**bjeto (POCOs).

* Um *controlador* é um objeto que manipula solicitações HTTP e cria a resposta HTTP. Este aplicativo tem um único controlador.

* Para manter o tutorial simples, o aplicativo não usa um banco de dados persistente. O aplicativo de exemplo armazena os itens de tarefas pendentes em um banco de dados em memória.
