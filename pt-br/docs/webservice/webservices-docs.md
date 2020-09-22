Title: Webservices - 4biz Docs
Description: 4biz - Docs Webservices

# Webservices 4biz Docs

Esta seção destina-se a descrever a estrutura de comunicação REST, estabelecida entre aplicativos e o servidor back-end.

!!! warning "Atenção"
    "4biz_URL": prefixo de URL inalterável, para que você possa acessar os serviços disponibilizados para os aplicativos de mobile</br>
    TODO API QUE NECESSITA DE “sessionID” PRECISARÁ ESTAR NA SEÇÃO FORNECIDA PELO “Login” DA API.

--------

### LOGIN

!!! example "Serviço de autentificação do usuário"
	```tab="Método"
 	GET
	```

	```HTML tab="URL"
 	<url_base>/cit-portal-web/rest/usuario/getToken
	```

	```JSON tab="Solicitação"
	{
	"username":"rogerio.cassimiro",
	"password":"123456"
	}
	```

	```JSON tab="Resposta"
	{
	"id":25080,
	"dateEdition":"2017-09-12T11:06:28.907-0300",
	"dateCreation":"2017-08-10T10:00:51.274-0300",
	"username":"rogerio.cassimiro",
	"token":"TGT-17-BI06OJWapCune4uamf6zUDOcyf0GNPPxjrOSDJ66ZxtcthZGhf-CITDFSRV074",
	"name":"rogerio.cassimiro",
	"version":":59"
	}
	```

	```tab="Campos"
	Solicitação:
	   username: alfanumérico não vazio nem nulo;
	   password: alfanumérico não vazio nem nulo;
	Resposta:
	   id: numérico não vazio nem nulo;
	   dateEdition: timestamp não vazio nem nulo;
	   dateCreation: timestamp não vazio nem nulo;
	   username: alfanumérico não vazio nem nulo;
	   token: alfanumérico não vazio nem nulo;
	   name: alfanumérico não vazio nem nulo;
	   version: numérico não vazio nem nulo;
	```


###LISTA DEPARTMENTO

!!! example "Lista de serviços dos departamentos a serem usados."
	```tab="Método"
 	GET
	```

	```HTML tab="URL"
 	<url_base>/cit-ecm-web/integracao /listUnidade
	```

	```JSON tab="Solicitação"
	 {
	"name":"Department 1",
	"token":"TGT-17-BI06OJWapCune4uamf6zUDOcyf0GNPPxjrOSDJ66ZxtcthZGhf-CITDFSRV074"
	}
	```

	```JSON tab="Resposta"

	{
	"units":[
	{
	"id":93794,
	"name":"Department 1",
	"code":"000001"
	}
	]
	}
	```

	```tab="Campos"
	Solicitação:
	   name: alfanumérico não vazio nem nulo
	   token: alfanumérico não vazio nem nulo
	Resposta:
	   units: Lista vazia e não nula;
	   id: numérico não vazio nem nulo
	   name: alfanumérico não vazio nem nulo
	   code: alfanumérico não vazio nem nulo
	```

###LISTA TIPO DE PROCESSO

!!! example "Serviço de listagem do tipo de processo de um processo, a ser usado"
	```tab="Método"
	GET
	```

	```HTML tab="URL"
	<url_base>/cit-ecm-web/integracao/listTipoProcesso

	```

	```JSON tab="Solicitação"
	{
	"name":"Department 1",
	"token":"TGT-17-BI06OJWapCune4uamf6zUDOcyf0GNPPxjrOSDJ66ZxtcthZGhf-CITDFSRV074"
	}
	```

	```JSON tab="Resposta"
	{
	"typeProcess":[
	{
	"id":6967,
	"name":"Pattern process",
	"description":"Pattern process detailing"
	}
	]
	}
	```

	```tab="Campos"
	Solicitação:
	   name: alfanumérico não vazio nem nulo
	   token: alfanumérico não vazio nem nulo
	Resposta:
	  typeProcess: Lista vazia e não nula;
	  id: numérico não vazio nem nulo
	  name: alfanumérico não vazio nem nulo
	  description: alfanumérico não vazio nem nulo
	```
###LISTA NÍVEL DE ACESSO DO TIPO DE PROCESSO

!!! example "O serviço de listagem de assunto de um processo e / ou documento, a ser usado."
	```tab="Método"
	GET
	```

	```HTML tab="URL"
	<4biz_URL>/cit-ecm-web/integracao/listNivelAcessoTipoProcesso
	```

	```JSON tab="Solicitação"
	{
	"idTypeProcess":"54654",
	"token":"TGT-17-BI06OJWapCune4uamf6zUDOcyf0GNPPxjrOSDJ66ZxtcthZGhf-CITDFSRV074"
	}
	```

	```JSON tab="Resposta"
	{
	"levelAccessTypeProcess":[
	{
	"idLevelAccessTypeProcess":97942,
	"idLevelAccess:":"23121",
	"name":"PUBLIC"
	},
	{
	"idLevelAccessTypeProcess":97947,
	" idLevelAccess":91426,
	"name":"RESTRICT"
	}
	]
	}
	```

	```tab="Campos"
	Solicitação:
 	   idTypeProcess: numérico não vazio nem nulo
	   token: alfanumérico não vazio nem nulo
	Resposta:
	  levelAccessTypeProcess: lista não vazia nem nula;
	  idLevelAccess: numérico não vazio nem nulo
	  idLevelAccessTypeProcess: numérico não vazio nem nulo
 	  name: alfanumérico não vazio nem nulo
	```
### LISTA NÍVEL DE ACESSO DO TIPO DE DOCUMENTO

!!! example "O serviço de listagem de assunto de um processo e / ou documento, a ser usado."
	```tab="Método"
 	GET
	```

	```HTML tab="URL"
 	<4biz_URL>/cit-ecm-web/integracao/listNivelAcessoTipoDocumento

	```

	```JSON tab="Solicitação"
	{
	"idTypeDocument":"54654",
	"token":"TGT-17-BI06OJWapCune4uamf6zUDOcyf0GNPPxjrOSDJ66ZxtcthZGhf-CITDFSRV074"
	}
	```

	```JSON tab="Resposta"
	{
	"levelAccessTypeDocument":[
	{
	"idLevelAccessTypeDocument":97942,
	" idLevelAccess:":"23121",
	"name":"PUBLIC"
	},
	{
	"idLevelAccessTypeDocument ":97947,
	"idLevelAccess":91426,
	"name":"RESTRICTED"
	}
	]
	}
	```

	```tab="Campos"
	Solicitação:
	  idTypeDocument: numérico não vazio nem nulo
	  token: alfanumérico não vazio nem nulo
	Resposta:
	  levelAccesTypeDocument: lista não vazia nem nula;
	  idLevelAccess: numérico não vazio nem nulo
	  idLevelAccessTypeDocument: numérico não vazio nem nulo
  	  name: alfanumérico não vazio nem nulo
	```
###LISTA DE ASSUNTO


!!! example "O serviço de listagem de assunto de um processo e / ou documento, a ser usado."
	```tab="Método"
 	GET
	```

	```HTML tab="URL"
  	<4biz_URL>/cit-ecm-web/integracao/listAssunto

	```

	```JSON tab="Solicitação"
	{
	"name":"HEARING.MEETINGS"
	"code":"010.3",
	"token":"TGT-17-BI06OJWapCune4uamf6zUDOcyf0GNPPxjrOSDJ66ZxtcthZGhf-CITDFSRV074"
	}
	```

	```JSON tab="Resposta" 	
	{
	"subjects":[
	{
	"id":6982,
	"code":"010.3",
	"name":"HEARING.MEETINGS",
	"subject":"010.3 – ADMINISTRATION.GENERAL "
	}
	]
	}
	```

	```tab="Campos"
	Solicitação:
	  name: alfanumérico não vazio nem nulo
	  code: alfanumérico não vazio nem nulo
	  token: alfanumérico não vazio nem nulo
 	Resposta:
	  subjects: Lista vazia e não nula;
	  id: numérico não vazio nem nulo
	  code: alfanumérico não vazio nem nulo
	  name: alfanumérico não vazio nem nulo
	  subject: alfanumérico não vazio nem nulo
	```
### LISTA DE HIPÓTESES LEGAIS

!!! example "Lista de hipóteses legais de um processo ou documento a ser utilizado."
	```tab="Método"
	GET
	```

	```HTML tab="URL"
	<4biz_URL>/cit-ecm-web/integracao/listHipoteseLegal

	```

	```JSON tab="Solicitação"
	{
	"idLevelAccess":"97947",
	"name":"Information name",
	"token":"TGT-17-BI06OJWapCune4uamf6zUDOcyf0GNPPxjrOSDJ66ZxtcthZGhf-CITDFSRV074"
	}
	```

	```JSON tab="Resposta"
	{
	"hypoteseLegal":[
	{
	"id":7340,
	"description":"Law description",
	"name":"Information name"
	}
	]
	}
	```

	```tab="Campos"
	Solicitação:
	  idLevelAccess: numérico não vazio nem nulo
	  name: alfanumérico não vazio nem nulo
	  token: alfanumérico não vazio nem nulo
	Resposta:
	  hypoteseLegal: Lista vazia e não nula;
	  id: numérico não vazio nem nulo
   	  description: alfanumérico não vazio nem nulo
	  name: alfanumérico não vazio nem nulo
	```
### LISTA DE PESSOAS


!!! example "Lista de pessoas interessadas em um processo ou documento a ser utilizado."
	```tab="Método"
	GET
	```

	```HTML tab="URL"
	<4biz_URL>/cit-ecm-web/integracao/listPessoas

	```

	```JSON tab="Solicitação"
	{
	"name":"Maycon",
	"token":"TGT-17-BI06OJWapCune4uamf6zUDOcyf0GNPPxjrOSDJ66ZxtcthZGhf-CITDFSRV074"
	}	 
	```

	```JSON tab="Resposta"
	{
	"people":[
	{
	"id":98064,
	"name":"Maycon"
	}
	]
	}
	```

	```tab="Campos"
	Solicitação:
	  name: alfanumérico não vazio nem nulo
	  token: alfanumérico não vazio nem nulo
	Resposta:
	  people: Lista vazia e não nula;
	  id: numérico não vazio nem nulo
  	  name: alfanumérico não vazio nem nulo
	```
###LISTA DA FORMA DE CONFERÊNCIA

!!! example "Lista dos formulários de conferência de um processo e / ou documento, a ser usado"
	```tab="Método"
	GET
	```

	```HTML tab="URL"
	<4biz_URL>/cit-ecm-web/integracao/listFormaConferencia

	```

	```JSON tab="Solicitação"
	 {
	"name":"Administrative certified copy",
	"token":"TGT-17-BI06OJWapCune4uamf6zUDOcyf0GNPPxjrOSDJ66ZxtcthZGhf-CITDFSRV074"
	}
	```

	```JSON tab="Resposta"
	{
	"formConference":[
	{
	"id":97484,
	"name":"Administrative certified copy"
	}
	]
	}
	```

	```tab="Campos"
	Solicitação:
	  name: alfanumérico não vazio nem nulo
	  token: alfanumérico não vazio nem nulo
	Resposta:
	  formaConference: Lista vazia e não nula;
	  id: numérico não vazio nem nulo
	  name: alfanumérico não vazio nem nulo
	```
### LISTA DE TIPOS DE MÍDIA

!!! example "Lista de tipos de mídia para um documento a ser utilizado."
	```tab="Método"
	GET
	```

	```HTML tab="URL"
	<4biz_URL>/cit-ecm-web/integracao/listTipoSuporte

	```

	```JSON tab="Solicitação"
	{
	"name":"Name type media",
	"token":"TGT-17-BI06OJWapCune4uamf6zUDOcyf0GNPPxjrOSDJ66ZxtcthZGhf-CITDFSRV074"
	}
	```

	```JSON tab="Resposta"
	{
	"typeSupport":[
	{
	"id":97975,
	"name":"Name type media"
	}
	]
	}

	```

	```tab="Campos"
	Solicitação:
	  name: alfanumérico não vazio nem nulo
	  token: alfanumérico não vazio nem nulo
	Resposta:
	  typeSupport: Lista vazia e não nula;
	  id: numérico não vazio nem nulo
	  name: alfanumérico não vazio nem nulo
	```
###LISTA TIPOS DE DOCUMENTO

!!! example "Serviço de listagem de tipos de documentos de um documento a ser utilizado"
	```tab="Método"
 	GET
	```

	```HTML tab="URL"
 	<4biz_URL>/cit-ecm-web/integracao/listTipoDocumento

	```

	```JSON tab="Solicitação"
	 {
	"name":"Document type name",
	"token":"TGT-17-BI06OJWapCune4uamf6zUDOcyf0GNPPxjrOSDJ66ZxtcthZGhf-CITDFSRV074"
	}
	```

	```JSON tab="Resposta"
	{
	"typeDocument":[
	{
	"id":97949,
	"name":"Document type name"
	}
	]
	}
	```

	```tab="Campos"
	Solicitação:
	  name: alfanumérico não vazio nem nulo
	  token: alfanumérico não vazio nem nulo
	Resposta:
	  typeDocument: Lista vazia e não nula;
	  id: numérico não vazio nem nulo
	  name: alfanumérico não vazio nem nulo 	  
	```
###CRIAR PROCESSO

!!! example "Serviço de criação de um processo"
	```tab="Método"
 	POST
	```

	```HTML tab="URL"
 	<4biz_URL>/cit-ecm-web/integracao/processo

	```

	```JSON tab="Solicitação"
	{
	"assuntoComplementar":"Subject Detail",
	"observacaoGeral":"General observation",
	"unidade":{
	"id":40358
	},
	"tipoProcesso":{
	"id":40394
	},
	"nivelAcesso":{
	"idNivelAcessoTipoProcesso":40395
	},
	"assunto":{
	"id":40393
	},
	"interessados":[
	{
	"interessado":{
	"id":40355
	}
	}
	],
	"token":"TGT-17-BI06OJWapCune4uamf6zUDOcyf0GNPPxjrOSDJ66ZxtcthZGhf-CITDFSRV074"
	}
	```

	```JSON tab="Resposta"
	{
	"id":"25353",
	"nup":"00001.00002/2016-58"
	}	 
	```

	```tab="Campos"
	Solicitação:
 	  subjectComplementary: alfanumérico não vazio nem nulo
	  observationGeneral: alfanumérico não vazio nem nulo
	  unit: não vazio nem nulo;
	  id: não vazio nem nulo;
	  typeProcess: não vazio nem nulo;
	     id: não vazio nem nulo;
	  levelAccess: não vazio nem nulo;
	     idLevelAccessTypeProcess: não vazio nem nulo;
	  hypoteseLegal: não vazio nem nulo se o nível de acesso difere de Público.
	     id: númerico não vazio nem nulo
	  subject: não vazio nem nulo;
  	     id: não vazio nem nulo;
	  interested: pode ser vazio e nulo;
	    pessoal
	  token: alfanumérico não vazio nem nulo
	Resposta:
	  id: numérico não vazio nem nulo
	  nup: alfanumérico não vazio nem nulo
	```
###CRIAR DOCUMENTO

!!! example "Serviço de ciração de documento"
	```tab="Método"
 	POST
	```

	```HTML tab="URL"
 	<4biz_URL>/cit-ecm-web/integracao/documento

	```

	```JSON tab="Solicitação"
	{
	"numero":"9999",
	"assuntoComplementar":"Complementary Subject",
	"dataReferencia":"2016-08-19T14:45:25.360-0300",
	"unidade":{
	"id":40358
	},
	"processo":{
	"id":40503
	},
	"tipoDocumento":{
	"id":40426
	},
	"nivelAcesso":{
	"idNivelAcessoTipoDocumento":40427
	},
	"hipoteseLegal":{
	"id": 5658
	},
	"assunto":{
	"id":40393
	},
	"destinatario":{
	"id":40355
	},
	"tipoSuporteDocumento":{
	"id":40430
	},
	"interessados":[
	{
	"interessado":{
	"id":40355
	}
	}
	],
	"tipoConferencia":{
	"id":39950
	},
	"localizacao":"Physical location",
	"token":"TGT-17-BI06OJWapCune4uamf6zUDOcyf0GNPPxjrOSDJ66ZxtcthZGhf-CITDFSRV074"
	}
	```

	```JSON tab="Resposta"		
	{
	"id":"25353",
	"numero":"00001.00002/2016-58"
	}
	```

	```tab="Campos"
	Solicitação:
	  numero: alfanumérico não vazio nem nulo;
	  assuntoComplementar: alfanumérico não vazio nem nulo;
	  localização: alfanumérico não vazio nem nulo;
	  conteudo: não vazio nem nulo;
	     O conteúdo do documento deve ser enviado em base64
	  dataReferencia: timestamp não vazio nem nulo;
	  unidade: não vazio nem nulo;
	     id: numérico não vazio nem nulo
	  processo: não vazio nem nulo;
	     id: numérico não vazio nem nulo
	  tipoDocumento: não vazio nem nulo;
	     id: numérico não vazio nem nulo
	  nivelAcesso: não vazio nem nulo;
	     idNivelAcessoTipoProcesso: numérico não vazio nem nulo
	  hipoteseLegal: não vazio nem nulo, se o nível de acesso for diferente de Público;
	     id: numérico não vazio nem nulo
	  assunto: não vazio nem nulo;
	     id: numérico não vazio nem nulo
	  destinatario: não vazio nem nulo;
	     pessoa: não vazio nem nulo;
	        id: numérico não vazio nem nulo
	  tipoSuporteDocumento: não vazio nem nulo;
	  interessados: pode ser vazio e nulo;
	     pessoa: não vazio nem nulo;
	        id: numérico não vazio nem nulo
	  tipoConferencia: Boolean não vazio nem nulo;
	  localização: alfanumérico não vazio nem nulo, se o atributo tipoConferencia for true;
	  token: alfanumérico não vazio nem nulo;
	Resposta:
	  id: numérico não vazio nem nulo;
	  numero: não vazio nem nulo;
	```
###DOCUMENTO UPLOAD

!!! example "Serviço de criação de documento."
	```tab="Método"
 	POST
	```

	```HTML tab="URL"
 	<4biz_URL>/cit-ecm-web/integracao/uploadAnexo

	```

	```JSON tab="Solicitação"
	{
	"file":"9999",
	"idDocumento":"25353",
	"token":"TGT-17-BI06OJWapCune4uamf6zUDOcyf0GNPPxjrOSDJ66ZxtcthZGhf-CITDFSRV074"
	}	 
	```

	```JSON tab="Campos"
	N/A
	```tab="Resposta"
	Solicitação:
 	  idDocumento: numérico não vazio nem nulo
	  file: não vazio nem nulo
	  token: alfanumérico não vazio nem nulo
	```



###GET BY ID PROCESS

!!! example "Serviço que recupera detalhes de um processo, de acordo com seu identificador."
	```tab="Método"
 	GET
	```

	```HTML tab="URL"
 	<4biz_URL>/cit-ecm-web/integracao/getByIdProcesso

	```

	```JSON tab="Solicitação" 	
	{
	"id":6967
	}
	```

	```JSON tab="Resposta"
	{
	"id":"5252",
	"assuntoComplementar":"Complementary Subject",
	"observacaoGeral":"General observation",
	"dataReferencia":null,
	"situacao":"In progress",
	"tipoProcesso":{
	"id":97930,
	"nome":"Type reserved process"
	},
	"nivelAcesso":{
	" idNivelAcessoTipoProcesso ":91428,
	"descricao":"Public"
	},
	"hipoteseLegal":{
	"id":97841,
	"nome":"Information name"
	},
	"assunto":{
	"id":97839,
	"nome":"ADM",
	"assunto":"0001 - ADM "
	},
	"interessados":[
	{
	"pessoas":{
	"id":98064,
	"nome":"Maycon"
	}
	}
	],
	"token":"TGT-17-BI06OJWapCune4uamf6zUDOcyf0GNPPxjrOSDJ66ZxtcthZGhf-CITDFSRV074"
	}
	```

	```tab="Campos"
	Solicitação:
 	  id: numérico não vazio nem nulo
	  token: alfanumérico não vazio nem nulo
	Resposta:
	  assuntoComplementar: alfanumérico não vazio nem nulo
	  observacaoGeral: alfanumérico não vazio nem nulo
	  dataReferencia:
	  unidade: não vazio nem nulo;
	     id: numérico não vazio nem nulo
	  nome: alfanumérico não vazio nem nulo
	  tipoProcesso: não vazio nem nulo;
	     id: numérico não vazio nem nulo
	  nome: alfanumérico não vazio nem nulo
	     nivelAcesso: não vazio nem nulo;
	     idNivelAcessoTipoProcesso: numérico não vazio nem nulo
	     descricao: alfanumérico não vazio nem nulo
	  hipoteseLegal: não vazio nem nulo, se o nível de acesso for diferente de Público.
	     id: numérico não vazio nem nulo
	     nome: alfanumérico não vazio nem nulo
	assunto: não vazio nem nulo;
	     id: numérico não vazio nem nulo
	     assunto: alfanumérico não vazio nem nulo
	interessados: pode ser vazio e nulo;
	     pessoa: não vazio nem nulo;
	        id: numérico não vazio nem nulo
	        nome: : alfanumérico não vazio nem nulo
	```
### LISTA DE PROCESSO

!!! example "Listagem de processos a serem usados por um documento"
	```tab="Método"
	GET
	```

	```HTML tab="URL"
 	<4biz_URL>/cit-ecm-web/integracao/listProcesso
	```

	```JSON tab="Solicitação"
	{
	"nup":"00010.000012/2016-94",
	"token":"TGT-17-BI06OJWapCune4uamf6zUDOcyf0GNPPxjrOSDJ66ZxtcthZGhf-CITDFSRV074"
	}
	```

	```JSON tab="Resposta"
	{
	"processo":[
	{
	"id":"5252",
	"assuntoComplementar":"Complementary Subject",
	"nup":"00010.000012/2016-94",
	"status":"Complementary Subject",
	"tipoProcesso":{
	"id":97930,
	"nome":"type reserved process"
	},
	"nivelAcesso":{
	"descricao":"Público"
	},
	"assunto":{
	"id":97839,
	"nome":"ADM",
	"assunto":"0001 - ADM"
	},
	"hipoteseLegal":{
	"id":97841,
	"nome":"Information name hypothesis"
	}
	}
	]
	}
	```

	```tab="Campos"
	Solicitação:
 	  nup: alfanumérico podendo ser vazio ou nulo;
	  token: alfanumérico podendo ser vazio ou nulo
	Resposta:
	  id: numérico não vazio nem nulo
	  assuntoComplementar: alfanumérico podendo ser vazio ou nulo
	  status: alfanumérico podendo ser vazio ou nulo
  	     CONCLUIDO: "Concluído";
	     EM_ANDAMENTO: "Em andamento";
	     AGUARDANDO_VALIDACAO: "Aguardando validação "
	     VALIDADO: "Válido";
	     ANEXADO: "Anexado";
	  tipoProcesso: não vazio nem nulo;
	     id: numérico não vazio nem nulo
	     nome: alfanumérico podendo ser vazio ou nulo
	  nivelAcesso: não vazio nem nulo
	     idNivelAcessoTipoProcesso: numérico não vazio nem nulo
	     descricao: alfanumérico podendo ser vazio ou nulo
	  hipoteseLegal: não vazio nem nulo, se o nível de acesso for diferente de Público.
	     id: numérico não vazio nem nulo
	     nome: alfanumérico podendo ser vazio ou nulo
	  assunto: não vazio nem nulo;
	     id: numérico não vazio nem nulo
	     assunto: alfanumérico podendo ser vazio ou nulo
	  hipoteseLegal: não vazio nem nulo, se o nível de acesso for diferente de Público.
	     id: numérico não vazio nem nulo
	     name: alfanumérico podendo ser vazio ou nulo
	```
<hr>
<font  Size=2><b>Produto/Versão:</b> 4biz | Helium</font> &nbsp; &nbsp;
