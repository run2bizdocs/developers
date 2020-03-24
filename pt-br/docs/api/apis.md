Title: API
Description: descrever a estrutura de comunicação REST, estabelecida entre aplicativos e o servidor de back-end.

# API´s ESP -Enterprise Service Platform

Esta seção destina-se a descrever a estrutura de comunicação REST, estabelecida entre aplicativos e o servidor de back-end.

!!! warning "Atenção"
    **"4biz_URL"**: prefixo de URL inalterável, para que você possa acessar os serviços disponibilizados para os aplicativos móveis.
    TODO API QUE NECESSITA UM **“sessionID”** PRECISARÁ ESTAR EM UMA SESSÃO FORNECIDA PELO “Login” DA API.


--------

### Login

!!! example "Login do usuário para usar os Serviços 4biz"
	```tab="Método"
 	POST
	```

	```HTML tab="URL"
 	<4biz_URL>/services/v2/login
	```

	```JSON tab="Solicitação"
	Solicitação {
	"userName": "mobile",
	"password": "123456",
	"token": "API132654ASFE32132121Â­5412",
	"platform": "android"
	}

	```

	```JSON tab="Resposta"
	Response {
	"sessionID": "2355A68BF75281B73607EEC1A7191645",
	"rangeAction": 10,
	"locationInterval": 10
	}
	```

	```tab="Campos"
	Solicitação:
		userName: alfanumérico não vazio e nem nulo;
		password: alfanumérico não vazio e nem nulo;
		token: alfanumérico não vazio e nem nulo, refere-se ao identificador do dispositivo para enviar a notificação push;
		platform: refere-se ao tipo de plataforma (ios ou android) do qual o usuário efetuará login;
	Resposta:
		sessionID: não pode ser nulo nem vazio.
		rangeAction: é inteiro, não nulo e não pode ser zero. Representa o raio de ação de um usuário de campo em KM.
		locationInterval: é inteiro, não nulo e maior que zero. Representa, em minutos, o intervalo de tempo que o aplicativo deve enviar o posicionamento do atendente.
	```





### ListContracts

!!! example "Lista dos contratos acessíveis ao atendente."
	```tab="Método"
 	POST
	```

	```HTML tab="URL"
 	<4biz_URL>/services/v2/contracts
	```

	```JSON tab="Solicitação"
	{
	"sessionID": "2355A68BF75281B73607EEC1A7191645"
	}
	```

	```JSON tab="Resposta"
	{
	"dateTime": 1412101947000,
	"operationID": 65,
	"error": null,
	"contracts": [
	{
	"id": 1,
	"description": "Test Name"
	}
	]
	}
	```

	```tab="Campos"
	Solicitação :
		sessionID: alfanumérico não nulo e nem vazio;
	Resposta :
		dateTime: timestamp não vazio nem nulo;
		operationID: numérico não vazio nem nulo;
		error: valor alfanumérico que pode ser nulo mas não pode ser vazio;
		contracts: não vazio nem nulo;
		id: numérico não vazio nem nulo;
		description: alfanumérico não vazio nem nulo.
	```

### ListDeniedReasons

!!! example "Lista de razões quando recusar uma solicitação, como no check-in"
	```tab="Método"
 	POST
	```

	```HTML tab="URL"
 	<4biz_URL>/services/v2/deniedReasons
	```

	```JSON tab="Solicitação"
	{
	"sessionID": "2355A68BF75281B73607EEC1A7191645"
	}
	```

	```JSON tab="Resposta"
	{
	"dateTime": 1412101988000,
	"operationID": 666,
	"error": null,
	"reasons": [
	{
	"id": 1,
	"description": "First Suspension Reason"
	},
	{
	"id": 2,
	"description": "Second Suspension Reason"
	}
	]
	}
	```

	```tab="Campos"
	Solicitação :
		sessionID: alfanumérico não nulo e nem vazio;
	Resposta :
		dateTime: timestamp não vazio nem nulo;
		operationID: numérico não vazio nem nulo;
		error: valor alfanumérico que pode ser nulo mas não pode ser vazio;
		reasons: não vazio nem nulo;
		id: numérico não vazio nem nulo;
		description: alfanumérico não vazio nem nulo.
	```

### ListSolicitationStatus

!!! example "Lista os status de uma solicitação a ser usada, por exemplo, no serviço de checkout."
	```tab="Método"
 	POST
	```

	```HTML tab="URL"
 	<4biz_URL>/services/v2/status
	```

	```JSON tab="Solicitação"
	{
	"sessionID": "2355A68BF75281B73607EEC1A7191645"
	}
	```


	```JSON tab="Resposta"
	{
	"dateTime": 1412101662000,
	"operationID": 64,
	"error": null,
	"status": [
	{
	"id": 1,
	"description": "In progress"
	},
	{
	"id": 2,
	"description": "Suspended",
	"justifications": [
	{
	"id": 1,
	"description": "First Suspension Reason"
	},
	{
	"id": 2,
	"description": "Second Suspension Reason"
	}
	]
	},
	{
	"id": 3,
	"description": "Canceled"
	},
	{
	"id": 4,
	"description": "Solved",
	"justifications": [
	{
	"id": 1,
	"description": "Reconfigured database user",
	"parentId": null
	},
	{
	"id": 2,
	"description": "Any other solution here",
	"parentId": 1
	}
	]
	}
	]
	}
	```

	```tab="Campos"
	Solicitação :
		sessionID: alfanumérico não vazio nem nulo;
	Resposta :
		dateTime: timestamp não vazio nem nulo;
		operationID: numérico não vazio nem nulo;
		error: valor alfanumérico que pode ser nulo mas não pode ser vazio;
		status: não vazio nem nulo;
		id: numérico não vazio nem nulo;
		description: alfanumérico não vazio nem nulo;
	```

### ListUnits

!!! example " Lista as unidades de um contrato"
	```tab="Método"
 	POST
	```

	```HTML tab="URL"
 	<4biz_URL>/services/v2/units
	```

	```JSON tab="Solicitação"
	{
	"sessionID": "2355A68BF75281B73607EEC1A7191645"
	"contractID": 1233
	}
	```

	```JSON tab="Resposta"
	{
	"dateTime": 1412101988000,
	"operationID": 66,
	"error": null,
	"units": [
	{
	"id": 1,
	"description": "Default"
	},
	{
	"id": 2,
	"description": "Unit Test"
	}
	]
	}
	```

	```tab="Campos"
	Solicitação:
		sessionID: alfanumérico não vazio nem nulo;
		contractID: numérico não nulo.
	Resposta:
		dateTime: timestamp não vazio nem nulo;
		operationID: numérico não vazio nem nulo;
		error: valor alfanumérico que pode ser nulo mas não pode ser vazio;
		units: não vazio nem nulo;
		id: numérico não vazio nem nulo;
		description: alfanumérico não vazio nem nulo;   	
	```
### SendCoordinates

!!! example "Atualiza as coordenadas geográficas de uma unidade."
	```tab="Método"
 	POST
	```

	```HTML tab="URL"
 	<4biz_URL>/services/v2/coordinates
	```

	```JSON tab="Solicitação"
	{
	"unitID": 222,
	"sessionID": "2355A68BF75281B73607EEC1A7191645",
	"latitude": -19.369852147,
	"longitude": -49.369852147
	}
	```

	```JSON tab="Resposta"
	{
	"dateTime": 1412102841000,
	"operationID": 68,
	"error": null,
	"success": true
	}
	```

	```tab="Campos"
	Solicitação:
		unitID: numérico não nulo;
		sessionID: alfanumérico não vazio nem nulo;
		latitude: numérico não nulo;
		longitude: numérico não nulo;
	Resposta:
		dateTime: timestamp não vazio nem nulo;
		operationID: numérico não vazio nem nulo;
		error: valor alfanumérico que pode ser nulo mas não pode ser vazio;
		success: boolean;
	```
### DeviceDisassociate

!!! example "Desassociar um usuário de um dispositivo para que, quando um usuário excluir uma conexão, o usuário não receba mais a notificação push da conexão excluída."
	```tab="Método"
 	POST
	```

	```HTML tab="URL"
 	<4biz_URL>/services/v2/disassociate
	```

	```JSON tab="Solicitação"
	{
	"connection": "http://4biz.example.com.br&quot;,
	"sessionID": "2355A68BF75281B73607EEC1A7191645",
	"token": "API132654ASFE32132121¬5412"
	}
	```

	```JSON tab="Resposta"
	{
	"dateTime": 1412102841000,
	"operationID": 68,
	"error": null,
	"success": true
	}
	```

	```tab="Campos"
	Solicitação:
		connection: alfanumérico não vazio nem nulo, referindo-se à conexão excluída pelo usuário;
		sessionID: alfanumérico não vazio nem nulo;
		token: alfanumérico não vazio nem nulo, referindo-se ao token a ser desassociado;
	Resposta:
		dateTime: timestamp não vazio nem nulo;
		operationID: numérico não vazio nem nulo;
		error: valor alfanumérico que pode ser nulo mas não vazio;
		success: boolean;
	```

### GetNewest

!!! example "Recupera a solicitação mais recente para o usuário no grupo, a partir da última (newestNumber) no aplicativo"
	```tab="Método"
 	POST
	```

	```HTML tab="URL"
 	<4biz_URL>/services/v2/getNewest
	```

	```JSON tab="Solicitação"
	{
	"notificationType": 0,
	"onlyApproval": 1,
	"newestNumber": 322,
	"sessionID": "2355A68BF75281B73607EEC1A7191645"
	}
	```

	```JSON tab="Resposta"
	{
	"dateTime": 1377618318000,
	"operationID": 336,
	"error": null,
	"notifications": [
	{
	"number": 95933,
	"taskId": 22737,
	"type": 1,
	"date": "27/08/2013",
	"timeFlag": 12345,
	"timeSLA": 12,
	"latitude": 32.9984730,
	"longitude": 29.9984730,
	"endSLA": "null",
	"task": "Validar requisição",
	"personal": true,
	"inService": true,
	"inCkeckin": 0,
	"typeRequest": 0,
	"waiting": 1,
	"service": "SERVICES AND PRODUCTS REQUEST",
	"contract": 1,
	"unit": 15,
	"priorityorder": 1
	}
	]
	}
	```

	```tab="Campos"
	Solicitação:
		notificationType: numérico não nulo;
		onlyApproval: numérico não nulo;
		sessionID: alfanumérico não vazio nem nulo;
		newestNumber: numérico não nulo;
	Resposta:
		dateTime: timestamp não vazio nem nulo;
		operationID: numérico não vazio nem nulo;
		error: valor alfanumérico que pode ser nulo mas não pode ser vazio;
		notifications: não nulo;
		number: numérico não nulo;
		taskId: numérico não nulo;
		type: numérico não vazio nem nulo;
			0 - Tudo
			1 - Compras
			2 - Viagens
			3 - RH
			4 - Incidentes
			5 - Outros
			6 - Esperando
			data: string de data no formato dd/mm/aaaa;
		timeFlag: Número que define o período;
			0 - Em tempo
			1 - Menos que uma hora
			2 - Atrasado
		timeSLA: numérico inteiro não nulo, em minutos;
		latitude: numérico;
		longitude: numérico;
		endSLA: data em alfanumérico no padrão dd/MM/aaaa HH:mm ou MM/dd/aaaa HH:mm, dependendo da localidade, que pode ter valor nulo.
		task: alfanumérico não vazio nem nulo;
		personal: boolean que identifica se a solicitação é destinada para o usuário ou se é do grupo;
		inService: Boolean não nulo que identifica se a solicitação está em atendimento;
		inCheckin: Flag que identifica se a solicitação está em check Pessoal (true ou false);
			0 – Esperando checkin
			1 – Em Checkin
		service: alfanumérico não vazio nem nulo;
		typeRequest: Flag que identifica o tipo de execução da solicitação
			0 - Execução da Solicitação
			1 - Aprovação da Solicitação
		waiting: Flag que define o status da solicitação.
			0 - Acompanhando
			1 - Execução disponível
		contract: numérico não nulo;
		unit: numérico não nulo;
		priorityorder: valor numérico que pode apresentar valor nulo, número de sequência que identifica se o gerente solicitou o atendimento;    
	```

### GetOldest

!!! example "Recupere a solicitação mais recente para o usuário no grupo, do mais antigo."
	```tab="Método"
 	POST
	```

	```HTML tab="URL"
 	<4biz_URL>/services/v2/getOldest
	```

	```JSON tab="Solicitação"
	{
	"notificationType": 0,
	"onlyApproval": 1,
	"oldestNumber": 322,
	"sessionID": "2355A68BF75281B73607EEC1A7191645"
	}
	```

	```JSON tab="Resposta"
	{
	"dateTime": 1377618318000,
	"operationID": 336,
	"error": null,
	"notifications": [
	{
	"number": 95933,
	"taskId": 22737,
	"type": 1,
	"date": "27/08/2013",
	"timeFlag": 12345,
	"timeSLA": 12,
	"latitude": 32.9984730,
	"longitude": 29.9984730,
	"endSLA": "null",
	"task": "Validate request",
	"personal": true,
	"inService": true,
	"inCheckin": 1,
	"typeRequest": 0,
	"waiting": 1
	"service": "SERVICES AND PRODUCTS REQUEST",
	"contract": 1,
	"unit": 15,
	"priorityorder": 1
	}
	]
	}
	```

	```tab="Campos"
	Solicitação:
		notificationType: numérico não nulo;
		onlyApproval: numérico não nulo;
		sessionID: alfanumérico não vazio nem nulo;
		oldestNumber: numérico não nulo;
	Resposta:
		dateTime: timestamp não vazio nem nulo;
		operationID: numérico não vazio nem nulo;
		error: valor alfanumérico que pode ser nulo mas não pode ser vazio;
		notifications: não nulo;
		number: numérico não nulo;
		taskId: numérico não nulo;
		type: numérico não vazio nem nulo;
			0 - Tudo
			1 - Compras
			2 - Viagens
			3 - RH
			4 - Incidentes
			5 - Outros
			6 - Esperando
		date: data no formato dd/mm/aaaa;
		timeFlag: Número que define o período;
			0 - Em tempo
			1 - Menos de uma hora
			2 - Atrasado
		timeSLA: numérico inteiro não nulo, em minutos;
		latitude: numérico;
		longitude: numérico;
		endSLA: data em alfanumérico nos padrões dd/MM/aaaa HH:mm ou MM/dd/aaaa HH:mm, dependendo do locale, que pode ter valor nulo.
		task: alfanumérico não vazio nem nulo;
		personal: boolean que identifica se a solicitação está destinada para o usuário ou para o grupo;
		inService: Boolean não nulo que identifica se a solicitação está em atendimento;
		inCheckin: Flag que identifica se a solicitação está em checkin Pessoal (true ou false);
			0 – Esperando checkin
			1 – Em Cleckin
		service: alfanumérico não vazio nem nulo;
		typeRequest: Flag que identifica o tipo de execução de uma solicitação
			0 - Execução da Solicitação
			1 - Aprovação da Solicitação
		waiting: Flag que define o status da solicitação
			0 - Acompanhando
			1 - Execução disponível
		contract: numérico não nulo;
		unit: numérico não nulo;
		priorityorder: valor numérico que pode apresentar valor nulo, número de sequência que identifica se o gerente solicitou o serviço;
	```

### GetByCoordinates

!!! example "Recupera a solicitação mais recente para o usuário no grupo, a partir das coordenadas atuais do usuário."
	```tab="Método"
 	POST
	```

	```HTML tab="URL"
 	<4biz_URL>/services/v2/getByCoordinates
	```

	```JSON tab="Solicitação"
	{
	"notificationType": 0,
	"onlyApproval": 1,
	"latitude": 32.9984730,
	"longitude": 29.9984730,
	"sessionID": "2355A68BF75281B73607EEC1A7191645",
	"pager": {
	"page": 1,
	"size": 10
	}
	```

	```JSON tab="Resposta"
	{
	"dateTime": 1377618318000,
	"operationID": 336,
	"error": null,
	"notifications": [
	{
	"number": 95933,
	"taskId": 22737,
	"type": 1,
	"date": "27/08/2013",
	"timeFlag": 12345,
	"timeSLA": 12,
	"latitude": 32.9984730,
	"longitude": 29.9984730,
	"endSLA": "null",
	"task": "Validate request",
	"typeRequest": 0,
	"waiting": 1
	"personal": true,
	"inService": true,
	"inCheckin": 1,
	"service": "SERVICES AND PRODUCTS REQUEST",
	"contract": 1,
	"unit": 15,
	"priorityorder": 1
	}
	],
	"paging": {
	"page": 1,
	"size": 1,
	"totalElements": 5,
	"totalPages": 5
	}
	}
	```

	```tab="Campos"
	Solicitação:
		notificationType: numérico não nulo;
		onlyApproval: numérico não nulo;
		latitude: numérico não nulo;
		longitude: numérico não nulo;
		sessionID: alfanumérico não vazio nem nulo;
		pager: informação de paginação:
			page: número de página a ser consultado, a partir de 1;
			size: número de elementos a retornar;
	Resposta:
		dateTime: timestamp não vazio nem nulo;
		operationID: numérico não vazio nem nulo;
		error: valor alfanumérico que pode ser nulo nem vazio;
		notifications: não nulo;
		number: numérico não nulo;
		taskId: numérico não nulo;
		type: numérico não vazio nem nulo;
			0 - Tudo
			1 - Compras
			2 - Viagens
			3 - RH
			4 - Incidentes
			5 - Outros
			6 - Esperando
		date: data no formato dd/mm/aaaa;
		timeFlag: Número que define o período;
			0 - Em tempo
			1 - Menos de uma hora
			2 - Atrasado
		timeSLA: inteiro numérico não nulo, em minutos;
		latitude: numérico não nulo;
		longitude: numérico não nulo;
		endSLA: date em alfanumérico nos padrões dd/MM/aaaa HH:mm ou MM/dd/aaaa HH:mm, dependendo do locale, que pode ter valor nulo.
		task: alfanumérico não vazio nem nulo;
		personal: boolean que identifica se a solicitação foi destinada ao usuário ou para o grupo;
		inService: Boolean não nulo que identifica se a solicitação está em atendimento;
		inCheckin: Flag que identifica se a solicitação está em checkin Pessoal (true ou false);
			0 – Esperando checkin
			1 – Em Cleckin
		service: alfanumérico não vazio nem nulo;
		typeRequest: Flag que identifica o tipo de execução de uma solicitação
			0 - Execução da Solicitação
			1 - Aprovação da Solicitação
		waiting: Flag que define o status da solicitação.
			0 - Acompanhando
			1 - Execução disponível
		contract: numérico não nulo;
		unit: numérico não nulo;
		priorityorder: valor numérico que pode apresentar valor nulo, número de sequência que identifica se o gerente solicitou o serviço;
		paging: informação de paginação
			1.	page: número da página atual
			2.	size: tamanho da página retornada
			3.	totalElements: número de elementos de acordo com a consulta
		totalPages: número de páginas de acordo com a consulta
	```

### UpdateNotification

!!! example "Recupera as tarefas de um serviço."
	```tab="Método"
 	POST
	```

	```HTML tab="URL"
 	<4biz_URL>/services/v2/update
	```

	```JSON tab="Solicitação"
	{
	number": 322,
	"sessionID": "2355A68BF75281B73607EEC1A7191645"
	}
	```

	```JSON tab="Resposta"
	{
	"dateTime": 1377618318000,
	"operationID": 336,
	"error": null,
	"notifications": [
	{
	"number": 95933,
	"taskId": 22737,
	"type": 1,
	"date": "27/08/2013",
	"timeFlag": 12345,
	"timeSLA": 12,
	"latitude": 32.9984730,
	"longitude": 29.9984730,
	"endSLA": "null",
	"task": "Validate request",
	"personal": true,
	"inService": true,
	"inCheckin": 1,
	"typeRequest": 0,
	"waiting": 1
	"service: "SERVICES AND PRODUCTS REQUEST",
	"contract": 1,
	"unit": 15,
	"priorityorder": 1
	}
	]
	}
	```

	```tab="Campos"
	Solicitação:
		notificationType: numérico não nulo;
		onlyApproval: numérico não nulo;
		sessionID: alfanumérico não vazio nem nulo;
		oldestNumber: numérico não nulo;
	Resposta:
		dateTime: timestamp não vazio nem nulo;
		operationID: numérico não vazio nem nulo;
		error: valor alfanumérico que pode ser nulo nem vazio;
		notifications: não nulo;
		number: numérico não nulo;
		taskId: numérico não nulo;
		type: numérico não vazio nem nulo;
			0 - Tudo
			1 - Compras
			2 - Viagens
			3 - RH
			4 - Incidentes
			5 - Outros
			6 - Esperando
		date: data no formato dd/mm/aaaa;
		timeFlag: Número que define o período;
			0 - Em tempo
			1 - Menos de uma hora
			2 - Atrasado
		timeSLA: inteiro numérico não nulo, em minutos;
		latitude: numérico não nulo;
		longitude: numérico não nulo;
		endSLA: date em alfanumérico nos padrões dd/MM/aaaa HH:mm ou MM/dd/aaaa HH:mm, dependendo do locale, que pode ter valor nulo.
		task: alfanumérico não vazio nem nulo;
		personal: Boolean que identifica se a solicitação foi destinada ao usuário ou para o grupo;
		inService: Boolean não nulo que identifica se a solicitação está em atendimento;
		inCheckin: Flag que identifica se a solicitação está em checkin Pessoal (true ou false);
			0 – Esperando checkin
			1 – Em Cleckin
		service: alfanumérico não vazio nem nulo;
		typeRequest: Flag que identifica o tipo de execução de uma solicitação
			0 - Execução de Solicitação
			1 - Aprovação de Solicitação
		waiting: Flag que define o status da solicitação.
			0 - Acompanhando
			1 - Execução disponível
		contract: numérico não nulo;
		unit: numérico não nulo;
		priorityorder: valor numérico que pode apresentar valor nulo, número de sequência que identifica se o gerente solicitou o serviço;
	```

### GetById

!!! example "Recupera detalhes de uma solicitação, de acordo com seu identificador"
	```tab="Método"
 	POST
	```

	```HTML tab="URL"
 	<4biz_URL>/services/v2/getById
	```

	```JSON tab="Solicitação"
	{
	"taskId": 22737,
	"sessionID": "2355A68BF75281B73607EEC1A7191645"
	}
	```

	```JSON tab="Resposta"
	{
	"dateTime": 1377618392000,
	"operationID": 337,
	"error": null,
	"notification": {
	"endSLA": "null",
	"task": "Validate request",
	"service": "SERVICES AND PRODUCTS REQUEST",
	"description": " test",
	"status": "In progress",
	"taskStatus": "Available",
	"timeSLA": 12
	}
	}
	```

	```tab="Campos"
	Solicitação:
		taskId: numérico não nulo;
		sessionID: alfanumérico não vazio nem nulo;
	Resposta:
		dateTime: timestamp não vazio nem nulo;
		operationID: numérico não vazio nem nulo;
		error: valor alfanumérico que pode ser nulo mas não pode ser vazio;
		notification; não vazio nem nulo;
		endSLA: data em alfanumérico pelos padrões dd/MM/aaaa HH:mm ou MM/dd/aaaa HH:mm, dependendo do locale.
		task: alfanumérico não vazio nem nulo;
		service: alfanumérico não vazio nem nulo;
		description: alfanumérico não vazio nem nulo;
		status: alfanumérico não vazio nem nulo;
		taskStatus: alfanumérico não vazio nem nulo;
		timeSLA: numérico inteiro não nulo, em minutos;
	```

### GetReasons

!!! example "Recuperar os motivos a serem utilizados na aprovação de uma solicitação, de acordo com a solicitação."
	```tab="Método"
 	POST
	```

	```HTML tab="URL"
 	<4biz_URL>/services/v2/getReasons
	```

	```JSON tab="Solicitação"
	{
	"taskId": 22737,
	"sessionID": "2355A68BF75281B73607EEC1A7191645"
	}
	```

	```JSON tab="Resposta"
	{
	"dateTime": 1377618482000,
	"operationID": 339,
	"error": null,
	"amount": 4,
	"reasons": [
	{
	"id": 1,
	"desc": "Non-refundable authorization"
	},
	{
	"id": 4,
	"desc": "Purchase necessary to the business"
	},
	{
	"id": 6,
	"desc": "Purchase need supplied with another solution"
	},
	{
	"id": 5,
	"desc": "Value of quotation above the budget of the contract"
	}
	]
	}
	```

	```tab="Campos"
	Solicitação:
		taskId: numérico não nulo;
		sessionID: alfanumérico não vazio nem nulo;
	Resposta:
		dateTime: timestamp não vazio nem nulo;
		operationID: numérico não vazio nem nulo;
		error: valor alfanumérico que pode ser nulo mas não pode ser vazio;
		amount: numérico não vazio nem nulo;
		reasons; não vazio nem nulo;
		id: numérico não vazio nem nulo;
		desc: alfanumérico não vazio nem nulo;
	```

### AttendRequest

!!! example "Informar o início do atendimento de uma solicitação de serviço."
	```tab="Método"
 	POST
	```

	```HTML tab="URL"
 	<4biz_URL>/services/v2/attendRequest
	```

	```JSON tab="Solicitação"
	{
	"number": 89647,
	"latitude": -19.369852147,
	"longitude": -49.369852147,
	"dateTime": 1377618517000,
	"sessionID": "2355A68BF75281B73607EEC1A7191645"
	}
	```

	```JSON tab="Resposta"
	{
	"dateTime": 1377618517000,
	"operationID": 341,
	"error": null,
	"success": true
	}
	```

	```tab="Campos"
	Solicitação:
		number: numérico não nulo;
		latitude: numérico não nulo;
		longitude: numérico não nulo;
		dateTime: timestamp não vazio nem nulo;
		sessionID: alfanumérico não vazio nem nulo;
	Resposta:
		dateTime: timestamp não vazio nem nulo;
		operationID: numérico não vazio nem nulo;
		error: valor alfanumérico que pode ser nulo mas não pode ser vazio;
		success: Boolean que identifica a conclusão bem sucedida da operação;  
	```

### AttendantLocation

!!! example "Informa a localização automática da posição do atendente."
	```tab="Método"
 	POST
	```

	```HTML tab="URL"
 	<4biz_URL>/services/v2/ location
	```

	```JSON tab="Solicitação"
	{
	"sessionID": "2355A68BF75281B73607EEC1A7191645",
	"latitude": -19.369852147,
	"longitude": -49.369852147,
	"dateTime": 1412102841000
	}
	```

	```JSON tab="Resposta"
	{
	"dateTime": 1412102841000,
	"operationID": 68,
	"error": null,
	"success": true
	}
	```

	```tab="Campos"
	N/A
	```

### Feedback

!!! example "Registra uma aprovação ou negação de uma solicitação de serviço."
	```tab="Método"
 	POST
	```

	```HTML tab="URL"
 	<4biz_URL>/services/v2/feedback
	```

	```JSON tab="Solicitação"
	{
	"taskId": 22736,
	"feedback": 0,
	"reasonId": 1,
	"comments": "Comments",
	"sessionID": "2355A68BF75281B73607EEC1A7191645}
	```

	```JSON tab="Resposta"
	{
	"dateTime": 1377618517000,
	"operationID": 341,
	"error": null
	}
	```

	```tab="Campos"
	Solicitação:
		taskId: numérico não nulo;
		feedback: numérico não nulo;
			0 - rejeição da solicitação;
			1 - aprovação da solicitação;
		reasonId: numérico não nulo;
		comments: alfanumérico não vazio nem nulo;
		sessionID: alfanumérico não vazio nem nulo;
	Resposta:
		dateTime: timestamp não vazio nem nulo;
		operationID: numérico não vazio nem nulo;
		error: valor alfanumérico que pode ser nulo mas não pode ser vazio;
	```
### New

!!! example "Cria uma nova solicitação"
	```tab="Método"
 	POST
	```

	```HTML tab="URL"
 	<4biz_URL>/services/v2/new
	```

	```JSON tab="Solicitação"
	{
	"description": "Teste mobile",
	"sessionID": "2355A68BF75281B73607EEC1A7191645",
	"latitude": -19.369852147,
	"longitude": -49.369852147}
	```

	```JSON tab="Resposta"
	{
	"dateTime": 1377618543000,
	"operationID": 342,
	"error": null,
	"number": 95935
	}
	```

	```tab="Campos"
	Solicitação:
		description: alfanumérico não vazio nem nulo;
		sessionID: alfanumérico não vazio nem nulo;
		latitude: numérico não nulo;
		longitude: numérico não nulo;
	Resposta:
		dateTime: timestamp não vazio nem nulo;
		operationID: numérico não vazio nem nulo;
		error: valor alfanumérico que pode ser nulo mas não pode ser vazio;
		number: alfanumérico não vazio nem nulo;
	```

### Check-In

!!! example "Check-in do atendente em uma solicitação. Em outras palavras, inicia o cumprimento da solicitação."
	```tab="Método"
 	POST
	```

	```HTML tab="URL"
 	<4biz_URL>/services/v2/checkin
	```

	```JSON tab="Solicitação"
	{
	"taskId": 22778,
	"sessionID": "2355A68BF75281B73607EEC1A7191645",
	"latitude": -19.369852147,
	"longitude": -49.369852147,
	"startTime": 1377618543000}
	```

	```JSON tab="Resposta"
	{
	"dateTime": 1377618543000,
	"operationID": 342,
	"error": null,
	"number": 95935
	}
	```

	```tab="Campos"
	Solicitação:
		taskId:numérico não nulo;
		sessionID: alfanumérico não vazio nem nulo;
		latitude: numérico não nulo;
		longitude: numérico não nulo;
		startTime: timestamp não vazio nem nulo do tempo que o atendente solicita o "Check-in"
	Resposta:
		dateTime: timestamp não vazio nem nulo;
		operationID: numérico não vazio nem nulo;
		error: valor alfanumérico que pode ser nulo mas não pode ser vazio;
		number: alfanumérico não vazio nem nulo;
	```

### Check-Out

!!! example "Verifica um atendente em uma solicitação, atualizando seu status ("Suspenso", "Resolvido", etc.)."
	```tab="Método"
 	POST
	```

	```HTML tab="URL"
 	<4biz_URL>/services/v2/checkout
	```

	```JSON tab="Solicitação"
	{
	"taskId": 22778,
	"sessionID": "2355A68BF75281B73607EEC1A7191645",
	"latitude": -19.369852147,
	"longitude": -49.369852147,
	"status": 5,
	"solution": 5,
	"descSolution": "Descrição da Solução"
	}
	```

	```JSON tab="Resposta"
	{
	"dateTime": 1377618517000,
	"operationID": 342,
	"error": null,
	"number": 95935
	}
	```

	```tab="Campos"
	Solicitação:
		taskId: numérico não nulo;
		sessionID: alfanumérico não vazio nem nulo;
		latitude: numérico não nulo;
		longitude: numérico não nulo;
		status: numérico não nulo, de acordo com o retorno do serviço "ListNotivicationStatus"
		solution: numérico, informado apenas quando o status for “Solucionado (4)” or “Suspenso (2)”
		descSolution: alfanumérico, não vazio nem nulo, informado apenas quando o status for “Solucionado (4)” or “Suspenso (2)”
	Resposta:
		dateTime: timestamp não vazio nem nulo;
		operationID: numérico não vazio nem nulo;
		error: valor alfanumérico que pode ser nulo mas não pode ser vazio;
		number: alfanumérico não vazio nem nulo;
	```

### Check-InDenied

!!! example "Registrar a negação de uma solicitação de atendimento pelo atendente, nos casos de recusa ao check-in."
	```tab="Método"
 	POST
	```

	```HTML tab="URL"
 	<4biz_URL>/services/v2/denied
	```

	```JSON tab="Solicitação"
	{
	"taskId": 22778,
	"sessionID": "2355A68BF75281B73607EEC1A7191645",
	"latitude": -19.369852147,
	"longitude": -49.369852147,
	"dateTime": 1377618543000,
	"reasonId": 1377618543000
	}
	```

	```JSON tab="Resposta"
	{
	"dateTime": 1377618543000,
	"operationID": 342,
	"error": null
	}
	```

	```tab="Campos"
	Solicitação:
		taskId: numérico não nulo;
		sessionID: alfanumérico não vazio nem nulo;
		latitude: numérico não nulo;
		longitude: numérico não nulo;
		reasonId: numérico e não nulo. Obtido através do chamado de serviço "GetDeniedReasons";
	Resposta:
		dateTime: timestamp não vazio nem nulo;
		operationID: numérico não vazio nem nulo;
		error: valor alfanumérico que pode ser nulo mas não pode ser vazio;
		number: alfanumérico não vazio nem nulo;
	```
