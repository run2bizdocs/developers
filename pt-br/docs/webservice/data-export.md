Title: Exportação de Dados
Description:  Webservices no 4biz para Exportação de Dados

# Webservice – Exportação de Dados
Por meio dessa funcionalidade, é possível, por solicitação ao WebService, obter dados das tabelas do Banco de Dados do 4biz para um arquivo XML.
As condições de consulta para os registros que serão retornados são passadas como parâmetros via URL, conforme o exemplo abaixo:

!!! example ""
    http://localhost/4biz/services/data/cargos/19  

Essa solicitação retorna os registros da tabela de posições em que o idcargo, ou seja, a chave primária da tabela de cobranças, é igual a 19. A consulta gerada com essa solicitação é esta:  

<font size=2>
``` mysql
 SELECT IDCARGO, NOMECARGO, DATAINICIO, DATAFIM, IDDESCRICAOCARGO FROM CARGOS WHERE IDCARGO = 19  
```
</font>

Abaixo estão listadas as possibilidades e parâmetros que podem ser usados para recuperação de dados usando esta funcionalidade:  

## Consulta PK

!!! example "É possível obter um registro de acordo com o campo Chave Primária da tabela relatada."
	```HTML tab="URL"
 	http://<SERVER ADDRESS>/4biz/services/data/<TABLE>/<PK VALUE>
	```

	```MYSQL tab="SQL"
	SELECT * FROM <TABLE_NAME> WHERE <PK_FIELD> = <PK_VALUE>
	```

	```tab="Exemplo"
	http://localhost/4biz/services/data/process/19
	   Ele irá procurar na tabela PROCESS com a chave PK igual a 19.
	```

## Consulta por PK e Where

!!! example "É possível obter um registro de acordo com um campo de Chave Primária da tabela fornecida e que atenda às condições de uma cláusula where."
	```HTML tab="URL"
 	http://<SERVER ADDRESS*>/4biz/services/data/<TABLE>/<PK_VALUE>?cond=<ADDITIONAL_CLAUSES>
	```

	```MYSQL tab="SQL"
	SELECT * FROM <TABLE_NAME> WHERE <PK_FIELD> = <PK_VALUE> AND (<ADDITIONAL_CLAUSES>)
	```

	```tab="Exemplo"
        http://localhost/4biz/services/data/cargos/22?cond=nomecargo like "Diretor"
	```

## Where e Consulta Ordenada

!!! example "É possível obter registros ordenados de uma tabela de acordo com as condições de uma cláusula where e campo para ordenação."
	```HTML tab="URL"
 	http://<SERVER ADDRESS*>/4biz/services/data/<TABLE>?cond=<WHERE_CRITERIA>&order=<ORDERING FIELD>
	```

	```MYSQL tab="SQL"
	SELECT * FROM <TABLE_NAME> WHERE (<WHERE CRITERIA>) ORDER BY <ORDERING FIELD>
	```

	```tab="Exemplo"
	http://localhost/4biz/services/data/cargos?cond=idcargo<15 and idcargo > 10&order=nomecargo
	```



## Consulta com vínculos ao Where e Ordenação

!!! example "É possível obter registros ordenados de uma tabela, juntamente com os registros com os quais eles estão vinculados, de acordo com as condições de uma cláusula where e de um campo para ordenação."

	```HTML tab="URL"
	http://<SERVER ADDRESS*>/4biz/services/data/<TABLE>?cond=<WHERE_CRITERIA>&order=<ORDERING FIELD>&links=<s_ou_n>
	```

	```MYSQL tab="SQL"
	SELECT * FROM <TABLE_NAME> WHERE (<WHERE CRITERIA>) ORDER BY <ORDERING FIELD>
	```

	```tab="Exemplo"
   	http://localhost/4biz/services/data/localidade?cond=idlocalidade=1&order=idlocalidade&links=s
	```

	```tab="Fields"
	cond = Refere-se às condições da cláusula where;
	order = Campo que será usado na ordem por cláusula;
	links = "S" ou "N". Quando S, a consulta também exportará ligações de chaves estrangeiras. Ou seja, os registros de outras tabelas que fazem referência a esse registro. Por padrão, quando essa opção não é inserida, o valor é N.
	```

<hr>
<font  Size=2><b>Produto/Versão:</b> 4biz | Helium</font> &nbsp; &nbsp;
