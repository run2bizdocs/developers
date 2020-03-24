Title: Data Export
Description:  Webservices in 4biz

#Webservice - Data Export
Through this functionality it is possible, via request to the WebService, to obtain data from the 4biz Database tables for an XML file.  
The query conditions for the records that will be returned are passed as parameters via URL as the example below:  

!!! expample ""
    http://localhost/4biz/services/data/cargos/19  

This request returns the records of the positions table where the idcargo, that is, the primary key of the charges table, is equal to 19. The query generated with this request is this:  

<font size=2>
``` mysql
 SELECT IDCARGO, NOMECARGO, DATAINICIO, DATAFIM, IDDESCRICAOCARGO FROM CARGOS WHERE IDCARGO = 19  
```
</font>

Below are listed the possibilities and parameters that can be used for data recovery using this feature:  

##PK Consultation

!!! example "It is possible to obtain a record according to a Primary Key field of the reported table."
	```HTML tab="URL"
 	http://<SERVER ADDRESS>/4biz/services/data/<TABLE>/<PK VALUE>
	```

	```MYSQL tab="SQL"
	SELECT * FROM <TABLE_NAME> WHERE <PK_FIELD> = <PK_VALUE>
	```

	```tab="Example"
	http://localhost/4biz/services/data/process/19
	   It will search the table PROCESS with the PK Key equal to 19.
	```

##Query by Pk and Where

!!! example "It is possible to obtain a record according to a Primary Key field of the given table and that meets the conditions of a where clause."
	```HTML tab="URL"
 	http://<SERVER ADDRESS*>/4biz/services/data/<TABLE>/<PK_VALUE>?cond=<ADDITIONAL_CLAUSES>
	```

	```MYSQL tab="SQL"
	SELECT * FROM <TABLE_NAME> WHERE <PK_FIELD> = <PK_VALUE> AND (<ADDITIONAL_CLAUSES>)
	```

	```tab="Example"
        http://localhost/4biz/services/data/cargos/22?cond=nomecargo like "Diretor"
	```

##Where And Ordered Consultation

!!! example "It is possible to obtain ordered records of a table according to the conditions of a where clause and field for sorted ordering."
	```HTML tab="URL"
 	http://<SERVER ADDRESS*>/4biz/services/data/<TABLE>?cond=<WHERE_CRITERIA>&order=<ORDERING FIELD>
	```

	```MYSQL tab="SQL"
	SELECT * FROM <TABLE_NAME> WHERE (<WHERE CRITERIA>) ORDER BY <ORDERING FIELD>
	```

	```tab="Example"
	http://localhost/4biz/services/data/cargos?cond=idcargo<15 and idcargo > 10&order=nomecargo
	```



##Query with links to Where and Ordering

!!! example "It is possible to obtain ordered records of a table, along with the records with which they are linked, according to the conditions of a where clause and field for ordering."

	```HTML tab="URL"
	http://<SERVER ADDRESS*>/4biz/services/data/<TABLE>?cond=<WHERE_CRITERIA>&order=<ORDERING FIELD>&links=<s_ou_n>
	```

	```MYSQL tab="SQL"
	SELECT * FROM <TABLE_NAME> WHERE (<WHERE CRITERIA>) ORDER BY <ORDERING FIELD>
	```

	```tab="Example"
   	http://localhost/4biz/services/data/localidade?cond=idlocalidade=1&order=idlocalidade&links=s
	```

	```tab="Fields"
	cond =Refers to the conditions of the where clause;
	order = Field that will be used in the order by clause;
	links = "S", or "N". When S, the query will also export foreign key bindings. That is, the records of other tables that reference this record. By default, when this option is not entered, the value is N.
	```

<hr>
<font  Size=2><b>Produto/Versão:</b> 4biz | 8.00</font> &nbsp; &nbsp;
<font  Size=2><b>Atualização:</b>13/12/2018 - Andre Luiz de Oliveira Fernandes</font>
