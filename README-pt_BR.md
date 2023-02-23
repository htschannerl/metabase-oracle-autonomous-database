# Como configurar Metabase para conectar em um Banco de Dados Oracle na Nuvem (Oracle Autonomus Database)

## Objetivo
O objetivo final desse artigo é explicar como fiz para conectar o Metabase que é uma ferramenta de BI, com um banco de dados Oracle em Nuvem.

Como sempre gostei de trabalhar com banco de dados e já utilizei o banco de dados Oracle e o Metabase como ferramenta de BI, eu decidi utilizar uma assinatura gratuita que tenho da Oracle e criar um banco de dados para controlar minhas finanças e para não precisar desenvolver nenhuma ferramenta para relatórios e analises eu decidi utilizar o Metabase.

Mas ai que veio o desafio. A instancia do Metabase eu instalei em um desktop que tenho em casa através do Docker e como já foi dito, o banco de dados estava na nuvem da Oracle e que por questões de segurança utiliza SSL para conexão.

Em todas as pesquisas que realizei, a informação que encontrei foi de alterar as definições da JVM do Metabase afim de informar os parâmetros do certificado e chave, mas como então realizar essas configurações no container? Sendo que os parâmetros necessários não estavam na relação de variáveis de ambiente possíveis. 

Vamos então para as intruções de como realizei essas configurações.

1) Primeiro de tudo temos que criar o banco de dados Oracle (Ver documentação Oracle).
2) Acesse os detalhes do banco de dados criado.
   ![Database details](/img/img01.png)
3) Acesse a opção *DB Connection*.
   ![DB Connection](/img/img02.png)
4) Acesse *Download client credentials (Wallet)*.
	1) Informe uma senha. Essa senha será utilizada para a criação do *Keyfile* que será utilizado pelo container do Metabase.
    ![Keystore Password](/img/img03.png)
5) Extraia o arquivo do *Wallet*.
6) Copie o arquivo `keystore.jks` onde esta armazenado os dados de configuração container do Metabase.
7) Use a variavel de ambiente `JAVA_OPTS` para definir as opções de `JVM` a seguir:
   ````
   -Djavax.net.ssl.keyStore=/path/to/keystore.jks
   -Djavax.net.ssl.keyStoreType=JKS \
   -Djavax.net.ssl.keyStorePassword=<keyStorePassword>
   ```` 
   Por exemplo `JAVA_OPTS: "-Djavax.net.ssl.keyStore=/keystore.jks -Djavax.net.ssl.keyStoreType=JKS -Djavax.net.ssl.keyStorePassword=<keyStorePassword>"`

8) Abra o arquivo `tnsnames.ora` extraido do arquivo de *Wallet* e busque a informação de `host`, `port` e `service_name`, e use essas informações na configuração da base de dados dentro das configurações do Metabase.
![Database Configuration](/img/img04.png)

## Referencias:

- [Oracle connecting with SSL](https://www.metabase.com/docs/latest/databases/connections/oracle#connecting-with-ssl)
- [Oracle Keytool](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html)
