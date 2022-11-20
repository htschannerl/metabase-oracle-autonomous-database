# How to configure Metabase with Oracle Cloud Autonomus Database

Instructions to connect the Metabase to Oracle Autonomous Database

I decided to write this instruction because I had difficulty connecting my Docker Metabase instance with the Oracle Autonomous Database using SSL.
Basically, my difficult was discovery how use the Keyfile in the docker container. 

As a reference I use the following links:

- [Oracle connecting with SSL](https://www.metabase.com/docs/latest/databases/connections/oracle#connecting-with-ssl)
- [Oracle Keytool](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html)

1) First of all you need to create your Oracle Autonomous database.
2) Go to the database details.
   ![Database details](/img/img01.png)
3) Access DB Connection.
   ![DB Connection](/img/img02.png)
4) Download client credentials (Wallet).
	1) Inform one password. This password will be used to create the Keyfile and will be used in the docker container of metabase.
    ![Keystore Password](/img/img03.png)
5) Extract the Wallet.
6) Copy the file keystore.jks where you going store the metabase configuration data.
7) Use the environment `JAVA_OPTS` to define the following `JVM` options:
````
-Djavax.net.ssl.keyStore=/path/to/keystore.jks
-Djavax.net.ssl.keyStoreType=JKS \
-Djavax.net.ssl.keyStorePassword=<keyStorePassword>
```` 
For example `JAVA_OPTS: "-Djavax.net.ssl.keyStore=/scripts/keystore.jks -Djavax.net.ssl.keyStoreType=JKS -Djavax.net.ssl.keyStorePassword=<keyStorePassword>"`

8) Open the file tnsnames.ora extracted from the Wallet and search the information about `host`, `port` and `service_name`, and use it in the database configuration in the metabase.
![Database Configuration](/img/img04.png)

If you have some question, please open a issue here and ask me.

Thanks!!!