# <a name="custom-model-binding-demo"></a>Demonstração de associação de modelos personalizada

Teste o `ByteArrayModelBinder` executando o aplicativo e executando POST em uma cadeia de caracteres codificada em Base64 para o ponto de extremidade ImageController (/api/image/). Você deve especificar as propriedades de arquivo e de nome de arquivo no Corpo da solicitação como dados de formulário (usando o Postman ou uma ferramenta semelhante). Use [esta cadeia de caracteres de exemplo](Base64String.txt). O resultado será salvo na pasta wwwroot/images/upload com o nome de arquivo especificado.

Para testar o exemplo de associação personalizada, tente os seguintes pontos de extremidade: /api/authors/1 /api/authors/2 (NÃO ENCONTRADO) /api/boundauthors/1 /api/boundauthors/2 (NÃO ENCONTRADO) /api/boundauthors/get/1 /api/boundauthors/get/2 (SEM CONTEÚDO) – essa ação não verifica a presença de nulo e retorna um Não Encontrado
