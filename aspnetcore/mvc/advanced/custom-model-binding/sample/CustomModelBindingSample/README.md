# <a name="custom-model-binding-demo"></a>Demonstração de associação de modelo personalizado

Você pode testar o `ByteArrayModelBinder` executando o aplicativo e uma cadeia de caracteres codificada em base64 para o ponto de extremidade ImageController de lançamento (/ api/imagem /). Você deve especificar o arquivo e o nome de arquivo proparties na solicitação de corpo como dados de formulário (usando carteiro ou uma ferramenta semelhante). Você pode usar [essa cadeia de caracteres de exemplo](Base64String.txt). O resultado será salvo na pasta wwwroot/imagens/carregar com o nome de arquivo especificado.

Para testar o exemplo de associação personalizada, tente os seguintes pontos de extremidade: /api/authors/1 /api/authors/2 (não encontrado) /api/boundauthors/1 /api/boundauthors/2 (não encontrado) /api/boundauthors/get/1 /api/boundauthors/get/2 (sem conteúdo -), essa ação não verifica para nulo e retornar um não encontrado
