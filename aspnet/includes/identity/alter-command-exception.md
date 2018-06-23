> Alguns comandos não têm suporte se o aplicativo usa SQLite como armazenamento de dados de identidade. Devido às limitações no mecanismo de banco de dados, `Alter` comandos geram a seguinte exceção:
>
> "System. NotSupportedException: SQLite não oferece suporte a essa operação de migração." 
>
> Como uma solução alternativa, execute migrações do Code First no banco de dados para alterar as tabelas.
