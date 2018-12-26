# ¿Cómo configurar la cadena de conexión con la base de datos? 

`Processa.GuessWho.Data.dll` busca una cadena de conexión en el siguiente orden. La búsqueda se detendrá en la primera coincidencia. 

> Una entrada en `AppSettings` del archivo de configuración con el nombre **DefaultConnectionString**. Si se encuentra, se busca en la sección `ConnectionStrings` del archivo de configuración una entrada con este nombre y se utiliza como cadena de conexión. 

Ejemplo:

```xml
<connectionStrings>
	<add name="MyConnectionStringName" connectionString="YourConnectionStringHere" providerName="System.Data.SqlClient" />
</connectionStrings>
<appSettings>
	<add key="DefaultConnectionString" value="MyConnectionStringName" />
</appSettings>
```

> Una cadena de conexión con el nombre **SQLServer:GuessWho** en la sección `ConnectionStrings` del archivo de configuración.

Ejemplo:

```xml
<connectionStrings>
	<add name="SQLServer:GuessWho" connectionString="YourConnectionStringHere" providerName="System.Data.SqlClient" />
</connectionStrings>
```



