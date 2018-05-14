## <a name="register-the-database-context"></a>Registrar o contexto de banco de dados

Nesta etapa, o contexto do banco de dados é registrado com o contêiner [injeção de dependência](xref:fundamentals/dependency-injection). Os serviços (como o contexto do banco de dados) que são registrados com o contêiner de injeção de dependência (DI) estão disponíveis para os controladores.

Registre o contexto de banco de dados com o contêiner de serviço usando o suporte interno para [injeção de dependência](xref:fundamentals/dependency-injection). Substitua o conteúdo do arquivo *Startup.cs* pelo seguinte código:

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Startup.cs?highlight=2,4,12)]

O código anterior:

* Remove o código que não é usado.
* Especifica que um banco de dados em memória é injetado no contêiner de serviço.