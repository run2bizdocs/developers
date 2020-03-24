Title: Webservices - Docs
Description: 4biz - Docs Webservices

#Webservices 4biz Docs

This section is intended to describe the communication structure REST, established between applications and the back-end server.

!!! warning
    "**4biz_URL**": URL unalterable prefix, so that you can access the services made available to the mobile applications.</br>
    _ALL API THAT REQUIRES A “**sessionID**” WILL NEED TO BE IN A SESSION PROVIDED BY THE “Login” API._

--------

### Login

!!! example "User Authentication Service"
	```tab="Method"
 	GET
	```

	```HTML tab="URL"
 	<url_base>/cit-portal-web/rest/usuario/getToken
	```

	```JSON tab="Request"
	{
	"username":"rogerio.cassimiro",
	"password":"123456"
	}
	```

	```JSON tab="Response"
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

	```tab="Fields"
	Request:
	   username: alphanumeric not empty and not null;
	   password :alphanumeric not empty and not null;
	Response:
	   id: numeric not empty and not null;
	   dateEdition: timestamp not empty and not null;
	   dateCreation: timestamp not empty and not null;
	   username: alphanumeric not empty and not null;
	   token: alphanumeric not empty and not null;
	   name: alphanumeric not empty and not null;
	   version: numeric not empty and not null;
	```


###LIST DEPARTMENT

!!! example "Service list of departments to be used."
	```tab="Method"
 	GET
	```

	```HTML tab="URL"
 	<url_base>/cit-ecm-web/integracao /listUnidade
	```

	```JSON tab="Request"
	 {
	"name":"Department 1",
	"token":"TGT-17-BI06OJWapCune4uamf6zUDOcyf0GNPPxjrOSDJ66ZxtcthZGhf-CITDFSRV074"
	}
	```

	```JSON tab="Response"

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

	```tab="Fields"
	Request:
	   name: alphanumeric not empty and not null
	   token: alphanumeric not empty and not null
	Response:
	   units: List empty and not null;
	   id: numeric not empty and not null
	   name: alphanumeric not empty and not null
	   code: alphanumeric not empty and not null
	```

###LIST PROCESS TYPE

!!! example "Service of listing the process type of a process, to be used"
	```tab="Method"
	GET
	```

	```HTML tab="URL"
	<url_base>/cit-ecm-web/integracao/listTipoProcesso

	```

	```JSON tab="Request"
	{
	"name":"Department 1",
	"token":"TGT-17-BI06OJWapCune4uamf6zUDOcyf0GNPPxjrOSDJ66ZxtcthZGhf-CITDFSRV074"
	}
	```

	```JSON tab="Response"
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

	```tab="Fields"
	Request:
	   name: alphanumeric not empty and not null
	   token: alphanumeric not empty and not null
	Response:
	  typeProcess: List empty and not null;
	  id: numeric not empty and not null
	  name: alphanumeric not empty and not null
	  description: alphanumeric not empty and not null
	```
###LIST LEVEL ACCESS TYPE PROCESS

!!! example "The subject listing service of a process and / or document, to be used."
	```tab="Method"
	GET
	```

	```HTML tab="URL"
	<4biz_URL>/cit-ecm-web/integracao/listNivelAcessoTipoProcesso
	```

	```JSON tab="Request"
	{
	"idTypeProcess":"54654",
	"token":"TGT-17-BI06OJWapCune4uamf6zUDOcyf0GNPPxjrOSDJ66ZxtcthZGhf-CITDFSRV074"
	}
	```

	```JSON tab="Response"
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

	```tab="Fields"
	Request:
 	   idTypeProcess: numeric not empty and not null
	   token: alphanumeric not empty and not null
	Response:
	  levelAccessTypeProcess: list not empty and not null;
	  idLevelAccess: numeric not empty and not null
	  idLevelAccessTypeProcess: numeric not empty and not null
 	  name: alphanumeric not empty and not null
	```
### LIST LEVEL ACCESS TYPE DOCUMENT

!!! example "The subject listing service of a process and / or document, to be used."
	```tab="Method"
 	GET
	```

	```HTML tab="URL"
 	<4biz_URL>/cit-ecm-web/integracao/listNivelAcessoTipoDocumento

	```

	```JSON tab="Request"
	{
	"idTypeDocument":"54654",
	"token":"TGT-17-BI06OJWapCune4uamf6zUDOcyf0GNPPxjrOSDJ66ZxtcthZGhf-CITDFSRV074"
	}
	```

	```JSON tab="Response"
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

	```tab="Fields"
	Request:
	  idTypeDocument: numeric not empty and not null
	  token: alphanumeric not empty and not null
	Response:
	  levelAccesTypeDocument: list not empty and not null;
	  idLevelAccess: numeric not empty and not null
	  idLevelAccessTypeDocument: numeric not empty and not null
  	  name: alphanumeric not empty and not null
	```
###LIST SUBJECT


!!! example "The subject listing service of a process and / or document, to be used."
	```tab="Method"
 	GET
	```

	```HTML tab="URL"
  	<4biz_URL>/cit-ecm-web/integracao/listAssunto

	```

	```JSON tab="Request"
	{
	"name":"HEARING.MEETINGS"
	"code":"010.3",
	"token":"TGT-17-BI06OJWapCune4uamf6zUDOcyf0GNPPxjrOSDJ66ZxtcthZGhf-CITDFSRV074"
	}
	```

	```JSON tab="Response" 	
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

	```tab="Fields"
	Request:
	  name: alphanumeric not empty and not null
	  code: alphanumeric not empty and not null
	  token: alfanumeric not empty and not null
 	Response:
	  subjects: List empty and not null;
	  id: numeric not empty and not null
	  code: alphanumeric not empty and not null
	  name: alphanumeric not empty and not null
	  subject: alphanumeric not empty and not null
	```
### LIST LEGAL HYPOTHESES

!!! example "List of legal hypotheses of a process or document, to be used."
	```tab="Method"
	GET
	```

	```HTML tab="URL"
	<4biz_URL>/cit-ecm-web/integracao/listHipoteseLegal

	```

	```JSON tab="Request"
	{
	"idLevelAccess":"97947",
	"name":"Information name",
	"token":"TGT-17-BI06OJWapCune4uamf6zUDOcyf0GNPPxjrOSDJ66ZxtcthZGhf-CITDFSRV074"
	}
	```

	```JSON tab="Response"
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

	```tab="Fields"
	Request:
	  idLevelAccess: numeric not empty and not null
	  name: alphanumeric not empty and not null
	  token: alphanumeric not empty and not null
	Response:
	  hypoteseLegal: List empty and not null;
	  id: numeric not empty and not null
   	  description: alphanumeric not empty and not null
	  name: alphanumeric not empty and not null
	```
### LIST PEOPLE


!!! example "List of interested persons of a process or document, to be used.."
	```tab="Method"
	GET
	```

	```HTML tab="URL"
	<4biz_URL>/cit-ecm-web/integracao/listPessoas

	```

	```JSON tab="Request"
	{
	"name":"Maycon",
	"token":"TGT-17-BI06OJWapCune4uamf6zUDOcyf0GNPPxjrOSDJ66ZxtcthZGhf-CITDFSRV074"
	}	 
	```

	```JSON tab="Response"
	{
	"people":[
	{
	"id":98064,
	"name":"Maycon"
	}
	]
	}
	```

	```tab="Fields"
	Request:
	  name: alphanumeric not empty and not null
	  token: alphanumeric not empty and not null
	Response:
	  people: List empty and not null;
	  id: numeric not empty and not null
  	  name: alphanumeric not empty and not null
	```
###LIST FORM OF CONFERENCE

!!! example "List of the conference forms of a process and / or document, to be used"
	```tab="Method"
	GET
	```

	```HTML tab="URL"
	<4biz_URL>/cit-ecm-web/integracao/listFormaConferencia

	```

	```JSON tab="Request"
	 {
	"name":"Administrative certified copy",
	"token":"TGT-17-BI06OJWapCune4uamf6zUDOcyf0GNPPxjrOSDJ66ZxtcthZGhf-CITDFSRV074"
	}
	```

	```JSON tab="Response"
	{
	"formConference":[
	{
	"id":97484,
	"name":"Administrative certified copy"
	}
	]
	}
	```

	```tab="Fields"
	Request:
	  name: alphanumeric not empty and not null
	  token: alphanumeric not empty and not null
	Response:
	  formaConference: List empty and not null;
	  id: numeric not empty and not null
	  name: alphanumeric not empty and not null
	```
### LIST MEDIA TYPES

!!! example "List of media types for a document to be used"
	```tab="Method"
	GET
	```

	```HTML tab="URL"
	<4biz_URL>/cit-ecm-web/integracao/listTipoSuporte

	```

	```JSON tab="Request"
	{
	"name":"Name type media",
	"token":"TGT-17-BI06OJWapCune4uamf6zUDOcyf0GNPPxjrOSDJ66ZxtcthZGhf-CITDFSRV074"
	}
	```

	```JSON tab="Response"
	{
	"typeSupport":[
	{
	"id":97975,
	"name":"Name type media"
	}
	]
	}

	```

	```tab="Fields"
	Request:
	  name: alphanumeric not empty and not null
	  token: alphanumeric not empty and not null
	Response:
	  typeSupport: List empty and not null;
	  id: numeric not empty and not null
	  name: alphanumeric not empty and not null  
	```
###LIST DOCUMENT TYPES

!!! example "Listing service of document types of a document, to be used."
	```tab="Method"
 	GET
	```

	```HTML tab="URL"
 	<4biz_URL>/cit-ecm-web/integracao/listTipoDocumento

	```

	```JSON tab="Request"
	 {
	"name":"Document type name",
	"token":"TGT-17-BI06OJWapCune4uamf6zUDOcyf0GNPPxjrOSDJ66ZxtcthZGhf-CITDFSRV074"
	}
	```

	```JSON tab="Response"
	{
	"typeDocument":[
	{
	"id":97949,
	"name":"Document type name"
	}
	]
	}
	```

	```tab="Fields"
	Request:
	  name: alphanumeric not empty and not null
	  token: alphanumeric not empty and not null
	Response:
	  typeDocument: List empty and not null;
	  id: numeric not empty and not null
	  name: alphanumeric not empty and not null	  
	```
###CREATE PROCESS

!!! example "Service creating a process."
	```tab="Method"
 	POST
	```

	```HTML tab="URL"
 	<4biz_URL>/cit-ecm-web/integracao/processo

	```

	```JSON tab="Request"
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

	```JSON tab="Response"
	{
	"id":"25353",
	"nup":"00001.00002/2016-58"
	}	 
	```

	```tab="Fields"
	Request:
 	  subjectComplementary: alphanumeric not empty and not null
	  observationGeneral: alphanumeric not empty and not null
	  unit: not empty and not null;
	  id: not empty and not null ;
	  typeProcess: not empty and not null;
	     id: not empty and not null ;
	  levelAccess: not empty and not null
	     idLevelAccessTypeProcess: not empty and not null ;
	  hypoteseLegal: not empty and not null if the access level differs from Public.
	     id: numeric not empty and not null
	  subject: not empty and not null ;
  	     id: not empty and not null ;
	  interested: can be empty and null;
	    personal
	  token: alphanumeric not empty and not null
	Response:
	  id: numeric not empty and not null
	  nup: alphanumérico not empty and not null;
	```
###CREATE DOCUMENT

!!! example "Document creation service"
	```tab="Method"
 	POST
	```

	```HTML tab="URL"
 	<4biz_URL>/cit-ecm-web/integracao/documento

	```

	```JSON tab="Request"
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

	```JSON tab="Response"		
	{
	"id":"25353",
	"numero":"00001.00002/2016-58"
	}
	```

	```tab="Fields"
	Request:
	  numero: alphanumeric not empty and not null;
	  assuntoComplementar: alphanumeric not empty and not null
	  localização: alphanumeric may be empty or null;
	  conteudo: not empty and not null;
	     The content of the document should be sent in base64
	  dataReferencia: timestamp not empty and not null;
	  unidade: not empty and not null;
	     id: numeric not empty and not null
	  processo: not empty and not null;
	     id: numeric not empty and not null
	  tipoDocumento: not empty and not null;
	     id: numeric not empty and not null
	  nivelAcesso: not empty and not null;
	     idNivelAcessoTipoProcesso: numeric not empty and not null
	  hipoteseLegal: not empty and not null, if the nível de acesso differs from Público;
	     id: numeric not empty and not null
	  assunto: not empty and not null;
	     id: numeric not empty and not null
	  destinatario: not empty and not null;
	     pessoa: not empty and not null;
	        id: numeric not empty and not null;
	  tipoSuporteDocumento: not empty and not null;
	  interessados: can be empty and null;
	     pessoa: not empty and not null;
	        id: numeric not empty and not null;
	  tipoConferencia: boolean not empty and not null;
	  localização: alphanumeric not null and non-empty, if the tipoConferencia attribute is true;
	  token: alfanumeric not empty and not null
	Response:
	  id: numeric not empty and not null
	  numero: not empty and not null
	```
###UPLOAD DOCUMENT

!!! example "Document creation service"
	```tab="Method"
 	POST
	```

	```HTML tab="URL"
 	<4biz_URL>/cit-ecm-web/integracao/uploadAnexo

	```

	```JSON tab="Request"
	{
	"file":"9999",
	"idDocumento":"25353",
	"token":"TGT-17-BI06OJWapCune4uamf6zUDOcyf0GNPPxjrOSDJ66ZxtcthZGhf-CITDFSRV074"
	}	 
	```

	```JSON tab="Response"
	N/A
	```tab="Fields"
	Request:
 	  idDocumento: numeric not empty and not null
	  file: not empty and not null
	  token: alphanumeric not empty and not null
	```



###GET BY ID PROCESS

!!! example "Service that retrieves details of a process, according to its identifier."
	```tab="Method"
 	GET
	```

	```HTML tab="URL"
 	<4biz_URL>/cit-ecm-web/integracao/getByIdProcesso

	```

	```JSON tab="Request" 	
	{
	"id":6967
	}
	```

	```JSON tab="Response"
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

	```tab="Fields"
	Request:
 	  id: numeric not empty and not null
	  token: alphanumeric not empty and not null
	Response:
	  assuntoComplementar: alphanumeric not empty and not null
	  observacaoGeral: alphanumeric not empty and not null
	  dataReferencia:
	  unidade: not empty and not null;
	     id: numeric not empty and not null
	  nome: alfphanumeric not empty and not null
	  tipoProcesso: not empty and not null;
	     id: numeric not empty and not null
	  nome: alphanumeric not empty and not null
	     nivelAcesso: not empty and not null
	     idNivelAcessoTipoProcesso: numeric not empty and not null
	     descricao: alphanumeric not empty and not null
	  hipoteseLegal: not empty and not null, if the access level differs from Public.
	     id: numeric not empty and not null
	     nome: alphanumeric not empty and not null
	assunto: not empty and not null;
	     id: numeric not empty and not null
	     assunto: alphanumeric not empty and not null
	interessados: can be empty and null;
	     pessoa: not empty and not null;
	        id: numeric not empty and not null;
	        nome: : alphanumeric not empty and not null  
	```
### LIST PROCESS

!!! example "Listing of processes to be used by a document"
	```tab="Method"
	GET
	```

	```HTML tab="URL"
 	<4biz_URL>/cit-ecm-web/integracao/listProcesso
	```

	```JSON tab="Request"
	{
	"nup":"00010.000012/2016-94",
	"token":"TGT-17-BI06OJWapCune4uamf6zUDOcyf0GNPPxjrOSDJ66ZxtcthZGhf-CITDFSRV074"
	}
	```

	```JSON tab="Response"
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

	```tab="Fields"
	Request:
 	  nup: alphanumeric may be empty or null;
	  token: alphanumeric not empty and not null
	Response:
	  id: numeric not empty and not null
	  assuntoComplementar: alphanumeric not empty and not null
	  status: alphanumeric not empty and not null
  	     CONCLUIDO: "Completed";
	     EM_ANDAMENTO: "In progress";
	     AGUARDANDO_VALIDACAO: "Waiting for Validation"
	     VALIDADO: "Validated";
	     ANEXADO: "Attached";
	  tipoProcesso: not empty and not null;
	     id: numeric not empty and not null
	     nome: alphanumeric not empty and not null
	  nivelAcesso: not empty and not null
	     idNivelAcessoTipoProcesso: numeric not empty and not null
	     descricao: alphanumeric not empty and not null
	  hipoteseLegal: not empty and not null, if the accessLevel differs from Public.
	     id: numeric not empty and not null
	     nome: alphanumeric not empty and not null
	  assunto: not empty and not null ;
	     id: numeric not empty and not null
	     assunto: alphanumeric not empty and not null
	  hipoteseLegal: not empty and not null, if the levelAccess differs from Public.
	     id: numeric not empty and not null
	     name: alphanumeric not empty and not null  
	```
<hr>
<font  Size=2><b>Produto/Versão:</b> 4biz | 8.00</font> &nbsp; &nbsp;
<font  Size=2><b>Atualização:</b>13/12/2018 - Andre Luiz de Oliveira Fernandes</font>
