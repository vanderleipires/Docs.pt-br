## <a name="register-the-database-context"></a>Registrar o contexto de banco de dados

Para injetar o contexto de banco de dados no controlador, precisamos registrá-lo com o contêiner [injeção de dependência](xref:fundamentals/dependency-injection). Registre o contexto de banco de dados com o contêiner de serviço usando o suporte interno para [injeção de dependência](xref:fundamentals/dependency-injection). Substitua o conteúdo do arquivo *Startup.cs* pelo seguinte:

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Startup.cs?highlight=2,4,12)]

O código anterior:

* Remove o código que não estamos usando.
* Especifica que um banco de dados em memória é injetado no contêiner de serviço.
