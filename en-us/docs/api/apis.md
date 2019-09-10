Title: API´s - ESP
Description: CITSmart - ESP API´s

#API´s ESP -Enterprise Service Platform

This section is intended to describe the communication structure REST, established between applications and the back-end server.

!!! warning 
    "**CITSMART_URL**": URL unalterable prefix, so that you can access the services made available to the mobile applications.</br>
    _ALL API THAT REQUIRES A “**sessionID**” WILL NEED TO BE IN A SESSION PROVIDED BY THE “Login” API._

--------

### Login 

!!! example "Login the user to use CITSmart Services."
	```tab="Method"
 	POST
	```

	```HTML tab="URL"
 	<CITSMART_URL>/services/v2/login
	```

	```JSON tab="Request"
	Request { 
	"userName": "mobile", 
	"password": "123456", 
	"token": "API132654ASFE32132121Â­5412", 
	"platform": "android" 
	}
	
	```

	```JSON tab="Response"
	Response {
	"sessionID": "2355A68BF75281B73607EEC1A7191645",
	"rangeAction": 10,
	"locationInterval": 10
	}
	```

	```tab="Fields"
	Request:
		userName: alphanumeric not empty and not null;
		password: alphanumeric not empty and not null;
		token: alphanumeric not empty and not null, referring to the device identifier for sending the push notification;
		platform: referring to the platform type (ios or android) from which the user will log in;
	Response:
		sessionID: cannot be null or empty.
		rangeAction: is integer, not null and can be zero. It represents the action radius of a field user in KM.
		locationInterval: is integer, not null, and greater than zero. Represents, in minutes, the time interval that the App must send the positioning of the attendant.
	```
                




### ListContracts

!!! example "List the contracts accessible to the attendant."
	```tab="Method"
 	POST
	```

	```HTML tab="URL"
 	<CITSMART_URL>/services/v2/contracts
	```

	```JSON tab="Request"
	{
	"sessionID": "2355A68BF75281B73607EEC1A7191645"
	}
	```

	```JSON tab="Response"
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

	```tab="Fields"
	Request :
		sessionID: alphanumeric not null and not empty;
	Response :
		dateTime: timestamp not empty and not null;
		operationID: numeric not empty and not null;
		error: alphanumeric value that can be null but not empty;
		contracts: not empty and not null;
		id: numeric not empty and not null;
		description: alphanumeric not empty and not null	
	```

### ListDeniedReasons

!!! example " List the reasons when refusing a request, like in the check-in."
	```tab="Method"
 	POST
	```

	```HTML tab="URL"
 	<CITSMART_URL>/services/v2/deniedReasons
	```

	```JSON tab="Request"
	{
	"sessionID": "2355A68BF75281B73607EEC1A7191645"
	}
	```

	```JSON tab="Response"
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

	```tab="Fields"
	Request :
		sessionID: alphanumeric not null and not empty;
	Response :
		dateTime: timestamp not empty and not null;
		operationID: numeric not empty and not null;
		error: alphanumeric value that can be null but not empty;
		reasons: not empty and not null;
		id: numeric not empty and not null;
		description: alphanumeric not empty and not null;
	```

### ListSolicitationStatus

!!! example " List status of a request to be used, for example, in the checkout service."
	```tab="Method"
 	POST
	```

	```HTML tab="URL"
 	<CITSMART_URL>/services/v2/status
	```

	```JSON tab="Request"
	{
	"sessionID": "2355A68BF75281B73607EEC1A7191645"
	}
	```


	```JSON tab="Response"
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

	```tab="Fields"
	Request :
		sessionID: alphanumeric not null and not empty;
	Response :
		dateTime: timestamp not empty and not null;
		operationID: numeric not empty and not null;
		error: alphanumeric value that can be null but not empty;
		status: not empty and not null;
		id: numeric not empty and not null;
		description: alphanumeric not empty and not null;
	```

### ListUnits

!!! example " List the units of a contract"
	```tab="Method"
 	POST
	```

	```HTML tab="URL"
 	<CITSMART_URL>/services/v2/units
	```

	```JSON tab="Request"
	{
	"sessionID": "2355A68BF75281B73607EEC1A7191645"
	"contractID": 1233
	}
	```

	```JSON tab="Response"
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

	```tab="Fields"
	Request :
		sessionID: alphanumeric not null and not empty; 
		contractID: numeric not null
	Response :
		dateTime: timestamp not empty and not null;
		operationID: numeric not empty and not null;
		error: alphanumeric value that can be null but not empty;
		units: not empty and not null;
		id: numeric not empty and not null;
		description: alphanumeric not empty and not null;	
	```
### SendCoordinates

!!! example " Update the geographical coordinates of a unit."
	```tab="Method"
 	POST
	```

	```HTML tab="URL"
 	<CITSMART_URL>/services/v2/coordinates
	```

	```JSON tab="Request"
	{
	"unitID": 222,
	"sessionID": "2355A68BF75281B73607EEC1A7191645",
	"latitude": -19.369852147,
	"longitude": -49.369852147
	}
	```

	```JSON tab="Response"
	{
	"dateTime": 1412102841000,
	"operationID": 68,
	"error": null,
	"success": true
	}
	```

	```tab="Fields"
	Request :
		unitID: numeric not null;
		sessionID: alphanumeric not empty and not null;
		latitude: numeric not null;
		longitude: numeric not null;
	Response :
		dateTime: timestamp not empty and not null;
		operationID: numeric not empty and not null;
		error: alphanumeric value that can be null but not empty;
		success: boolean;
	```
### DeviceDisassociate

!!! example "Disassociate a user from a device, so that when a user deletes a connection, the user no longer receives push notification from the deleted connection."
	```tab="Method"
 	POST
	```

	```HTML tab="URL"
 	<CITSMART_URL>/services/v2/disassociate
	```

	```JSON tab="Request"
	{
	"connection": "http://citsmart.centralit.com.br&quot;,
	"sessionID": "2355A68BF75281B73607EEC1A7191645",
	"token": "API132654ASFE32132121¬5412"
	}
	```

	```JSON tab="Response"
	{
	"dateTime": 1412102841000,
	"operationID": 68,
	"error": null,
	"success": true
	}
	```

	```tab="Fields"
	Request :
		connection: alphanumeric not empty and not null, referring to the connection deleted by the user;
		sessionID: alphanumeric not empty and not null;
		token: alphanumeric not empty and not null, referring to the token to be disassociated;
	Response :
		dateTime: timestamp not empty and not null;
		operationID: numeric not empty and not null;
		error: alphanumeric value that can be null but not empty;
		success: boolean;
	```

### GetNewest

!!! example " Recover the most recent request for the user in the group, from the latest (newestNumber) in the App "
	```tab="Method"
 	POST
	```

	```HTML tab="URL"
 	<CITSMART_URL>/services/v2/getNewest
	```

	```JSON tab="Request"
	{
	"notificationType": 0,
	"onlyApproval": 1,
	"newestNumber": 322,
	"sessionID": "2355A68BF75281B73607EEC1A7191645"
	}
	```

	```JSON tab="Response"
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

	```tab="Fields"
	Request :
		notificationType: numeric not null;
		onlyApproval: numeric not null;
		sessionID: alphanumeric not null and not empty;
		newestNumber: numeric not null;
	Response :
		dateTime: timestamp not empty and not null;
		operationID: numeric not empty and not null;
		error: alphanumeric value that can be null but not empty;
		notifications: not null;
		number: numeric not null;
		taskId: numeric not null;
		type: numeric not empty and not null;
			0 - All
			1 - Purchases
			2 - Travels
			3 - HR
			4 - Incidents
			5 - Others
			6 - Waiting
			date: date string in dd/mm/yyyy format;
		timeFlag: Number that defines the period;
			0 - In time
			1 - Less than an hour
			2 - Overdue
		timeSLA: numeric integer not null, in minutes;
		latitude: numeric;
		longitude: numeric;
		endSLA: date in alphanumeric in the patterns dd/MM/ yyyy HH:mm or MM/dd/yyyy HH:mm, depending on the locale, which may have null value.
		task: alphanumeric not empty and not null;
		personal: boolean that identifies whether the request is signed for the user or if it's from the group;
		inService: non-null boolean that identifies whether the request is in attendance;
		inCheckin: Flag that identifies whether a request is in Personal check (true or false);
			0 – Waiting checkin
			1 – In Cleckin
		service: alphanumeric not empty and not null;
		typeRequest: Flag that identifies the execution type of a request
			0 - Request Execution
			1 - Request Approval
		waiting: Flag that defines the status of the request.
			0 - Monitoring
			1 - Execution available
		contract: numeric not null;
		unit: numeric not null;
		priorityorder: numeric value that can present null value, sequence number that identifies if the manager ordered the attendance;	
	```

### GetOldest

!!! example " Recover the most recent request for the user in the group, from the oldest."
	```tab="Method"
 	POST
	```

	```HTML tab="URL"
 	<CITSMART_URL>/services/v2/getOldest
	```

	```JSON tab="Request"
	{
	"notificationType": 0,
	"onlyApproval": 1,
	"oldestNumber": 322,
	"sessionID": "2355A68BF75281B73607EEC1A7191645"
	}
	```

	```JSON tab="Response"
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

	```tab="Fields"
	Request :
		notificationType: numeric not null;
		onlyApproval: numeric not null;
		sessionID: alphanumeric not null and not empty;
		oldestNumber: numeric not null;
	Response :
		dateTime: timestamp not empty and not null;
		operationID: numeric not empty and not null;
		error: alphanumeric value that can be null but not empty;
		notifications: not null;
		number: numeric not null;
		taskId: numeric not null;
		type: numeric not empty and not null;
			0 - All
			1 - Purchases
			2 - Travels
			3 - HR
			4 - Incidents
			5 - Others
			6 - Waiting
		date: date string in the format dd/mm/yyyy;
		timeFlag: Number that defines the period;
			0 - In time
			1 - Less than an hour
			2 - Overdue
		timeSLA: numeric integer not null, in minutes;
		latitude: numeric;
		longitude: numeric;
		endSLA: date in alphanumeric in the patterns dd/MM/ yyyy HH:mm or MM/dd/yyyy HH:mm, depending on the locale, which may have null value.
		task: alphanumeric not empty and not null;
		personal: boolean that identifies whether the request is signed for the user or if it's from the group;
		inService: non-null boolean that identifies whether the request is in service;
		inCheckin: Flag that identifies whether a request is in the checkin Personal (true or false);
			0 – Waiting checkin
			1 – In Cleckin
		service: alphanumeric not empty and not null;
		typeRequest: Flag that identifies the type of execution of a request
			0 - Request Execution
			1 - Request Approval
		waiting: Flag that defines the status of the request
			0 - Monitoring
			1 - Execution available
		contract: numeric not null;
		unit: numeric not null;
		priorityorder: numeric value that can present null value, sequence number that identifies if the manager ordered the service;	
	```

### GetByCoordinates

!!! example "Recover the most recent request for the user in the group, from the user's current coordinates."
	```tab="Method"
 	POST
	```

	```HTML tab="URL"
 	<CITSMART_URL>/services/v2/getByCoordinates
	```

	```JSON tab="Request"
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

	```JSON tab="Response"
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

	```tab="Fields"
	Request :
		notificationType: numeric not null;
		onlyApproval: numeric not null;
		latitude: numeric not null;
		longitude: numeric not null;
		sessionID: alphanumeric not null and not empty;
		pager: pagination information:
			page: page number to be queried, starting from 1;
			size: number of elements to return;
	Response :
		dateTime: timestamp not empty and not null;
		operationID: numeric not empty and not null;
		error: alphanumeric value that can be null but not empty;
		notifications: not null;
		number: numeric not null;
		taskId: numeric not null;
		type: numeric not empty and not null;
			0 - All
			1 - Purchases
			2 - Travels
			3 - HR
			4 - Incidents
			5 - Others
			6 - Waiting
		date: date string in the format dd/mm/yyyy;
		timeFlag: Number that defines the period;
			0 - In time
			1 - Less than an hour
			2 - Overdue
		timeSLA: numeric integer not null, in minutes;
		latitude: numeric not null;
		longitude: numeric not null;
		endSLA: date in alphanumeric in the patterns dd/MM/ yyyy HH:mm or MM/dd/yyyy HH:mm, depending on the locale, which may have null value.
		task: alphanumeric not empty and not null;
		personal: boolean that identifies whether the request is signed for the user or if it's from the group;
		inService: non-null boolean that identifies whether the request is in service;
		inCheckin: Flag that identifies whether a request is in the checkin Personal (true or false);
			0 – Waiting checkin
			1 – In Cleckin
		service: alphanumeric not empty and not null;
		typeRequest: Flag that identifies the type of execution of a request
			0 - Request Execution
			1 - Request Approval
		waiting: Flag that defines the status of the request.
			0 - Monitoring
			1 - Execution available
		contract: numeric not null;
		unit: numeric not null;
		priorityorder: numeric value that can present null value, sequence number that identifies if the manager ordered the service;
		paging: pagination information
			1.	page: number of the current page
			2.	size: size of the page returned
			3.	totalElements: total of elements according to the query
		totalPages: total of pages according to the query
	```

### UpdateNotification

!!! example " Retrieve the tasks of a service."
	```tab="Method"
 	POST
	```

	```HTML tab="URL"
 	<CITSMART_URL>/services/v2/update
	```

	```JSON tab="Request"
	{
	number": 322,
	"sessionID": "2355A68BF75281B73607EEC1A7191645"
	}
	```

	```JSON tab="Response"
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

	```tab="Fields"
	Request :
		notificationType: numeric not null;
		onlyApproval: numeric not null;
		sessionID: alphanumeric not null and not empty;
		oldestNumber: numeric not null;
	Response :
		dateTime: timestamp not empty and not null;
		operationID: numeric not empty and not null;
		error: alphanumeric value that can be null but not empty;
		notifications: not null;
		number: numeric not null;
		taskId: numeric not null;
		type: numeric not empty and not null;
			0 - All
			1 - Purchases
			2 - Travels
			3 - HR
			4 - Incidents
			5 - Others
			6 - Waiting
		date: date string in format dd/mm/yyyy;
		timeFlag: Number that defines the period;
			0 - In time
			1 - Less than an hour
			2 - Overdue
		timeSLA: numeric integer not null, in minutes;
		latitude: numeric;
		longitude: numeric;
		endSLA: date in alphanumeric in the patterns dd/MM/ yyyy HH:mm or MM/dd/yyyy HH:mm, depending on the locale, which may have null value.
		task: alphanumeric not empty and not null;
		personal: boolean that identifies whether the request is signed for the user or if it's from the group;
		inService: non-null boolean that identifies whether the request is in service;
		inCheckin: Flag that identifies whether a request is in the checkin Personal (true or false);
			0 – Waiting checkin
			1 – In Cleckin
		service: alphanumeric not empty and not null;
		typeRequest: Flag that identifies the type of execution of a request
			0 - Request Execution
			1 - Request Approval
		waiting: Flag that defines the status of the request.
			0 - Monitoring
			1 - Execution available
		contract: numeric not null;
		unit: numeric not null;
		priorityorder: numeric value that can present null value, sequence number that identifies if the manager ordered the service;	
	```

### GetById

!!! example "Retrieves details of a request, according to its identifier"
	```tab="Method"
 	POST
	```

	```HTML tab="URL"
 	<CITSMART_URL>/services/v2/getById
	```

	```JSON tab="Request"
	{
	"taskId": 22737,
	"sessionID": "2355A68BF75281B73607EEC1A7191645"
	}
	```

	```JSON tab="Response"
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

	```tab="Fields"
	Request :
		taskId: numeric not null;
		sessionID: alphanumeric not null and not empty;
	Response :
		dateTime: timestamp not empty and not null;
		operationID: numeric not empty and not null;
		error: alphanumeric value that can be null but not empty;
		notification; not empty and not null;
		endSLA: date in alphanumeric by the patterns dd/MM/yyyy HH:mm or MM/dd/yyyy HH:mm, depending on the locale.
		task: alphanumeric not empty and not null;
		service: alphanumeric not empty and not null;
		description: alphanumeric not empty and not null;
		status: alphanumeric not empty and not null;
		taskStatus: alphanumeric not empty and not null;
		timeSLA: numeric integer not null, in minutes;
	```

### GetReasons

!!! example "Recover motives to be used in the approval of a request, according to the request."
	```tab="Method"
 	POST
	```

	```HTML tab="URL"
 	<CITSMART_URL>/services/v2/getReasons
	```

	```JSON tab="Request"
	{
	"taskId": 22737,
	"sessionID": "2355A68BF75281B73607EEC1A7191645"
	}
	```

	```JSON tab="Response"
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

	```tab="Fields"
	Request :
		taskId: numeric not null;
		sessionID: alphanumeric not null and not empty;
	Response :
		dateTime: timestamp not empty and not null;
		operationID: numeric not empty and not null;
		error: alphanumeric value that can be null but not empty;
		amount: numeric not empty and not null;
		reasons; not empty and not null;
		id: numeric not empty and not null;
		desc: alphanumeric not empty and not null;
	```

### AttendRequest

!!! example " Inform the beginning of attendance of a service request "
	```tab="Method"
 	POST
	```

	```HTML tab="URL"
 	<CITSMART_URL>/services/v2/attendRequest
	```

	```JSON tab="Request"
	{
	"number": 89647,
	"latitude": -19.369852147,
	"longitude": -49.369852147,
	"dateTime": 1377618517000,
	"sessionID": "2355A68BF75281B73607EEC1A7191645"
	}
	```

	```JSON tab="Response"
	{
	"dateTime": 1377618517000,
	"operationID": 341,
	"error": null,
	"success": true
	}
	```

	```tab="Fields"
	Request :
		number: numeric not null;
		latitude: numeric not null;
		longitude: numeric not null;
		dateTime: timestamp not empty and not null;
		sessionID: alphanumeric not null and not empty;
	Response :
		dateTime: timestamp not empty and not nul;
		operationID: numeric not empty and not null;
		error: alphanumeric value that can be null but not empty;
		success: boolean that identifies the successful completion of the operation;
	```

### AttendantLocation

!!! example "Reports the automatic location of the attendant position."
	```tab="Method"
 	POST
	```

	```HTML tab="URL"
 	<CITSMART_URL>/services/v2/ location 
	```

	```JSON tab="Request"
	{
	"sessionID": "2355A68BF75281B73607EEC1A7191645",
	"latitude": -19.369852147,
	"longitude": -49.369852147,
	"dateTime": 1412102841000
	}
	```

	```JSON tab="Response"
	{
	"dateTime": 1412102841000,
	"operationID": 68,
	"error": null,
	"success": true
	}
	```

	```tab="Fields"
	N/A
	```

### Feedback 

!!! example "Register an approval or denial of a service request."
	```tab="Method"
 	POST
	```

	```HTML tab="URL"
 	<CITSMART_URL>/services/v2/feedback
	```

	```JSON tab="Request"
	{
	"taskId": 22736,
	"feedback": 0,
	"reasonId": 1,
	"comments": "Comments",
	"sessionID": "2355A68BF75281B73607EEC1A7191645}
	```

	```JSON tab="Response"
	{
	"dateTime": 1377618517000,
	"operationID": 341,
	"error": null
	}
	```

	```tab="Fields"
	Request :
		taskId: numeric not null;
		feedback: numeric not null:
			0 - request rejection;
			1 - request approval;
		reasonId: numeric not null;
		comments: alphanumeric not null and not empty;
		sessionID: alphanumeric not null and not empty;
	Response :
		dateTime: timestamp not empty and not null;
		operationID: numeric not empty and not null;
		error: alphanumeric value that can be null but not empty;
	```
### New

!!! example " Create a new request."
	```tab="Method"
 	POST
	```

	```HTML tab="URL"
 	<CITSMART_URL>/services/v2/new
	```

	```JSON tab="Request"
	{
	"description": "Teste mobile",
	"sessionID": "2355A68BF75281B73607EEC1A7191645",
	"latitude": -19.369852147,
	"longitude": -49.369852147}
	```

	```JSON tab="Response"
	{
	"dateTime": 1377618543000,
	"operationID": 342,
	"error": null,
	"number": 95935
	}
	```

	```tab="Fields"
	Request :
		description: alphanumeric not null and not empty;
		sessionID: alphanumeric not null and not empty;
		latitude: numeric not null;
		longitude: numeric not null;
	Response :
		dateTime: timestamp not empty and not null;
		operationID: numeric not empty and not null;
		error: alphanumeric value that can be null but not empty;
		number: alphanumeric not empty and not null;
	```

### Check-In

!!! example "Check-in of the attendant in a request. In other words, it initiates the fulfillment of the request."
	```tab="Method"
 	POST
	```

	```HTML tab="URL"
 	<CITSMART_URL>/services/v2/checkin 
	```

	```JSON tab="Request"
	{
	"taskId": 22778,
	"sessionID": "2355A68BF75281B73607EEC1A7191645",
	"latitude": -19.369852147,
	"longitude": -49.369852147,
	"startTime": 1377618543000}
	```

	```JSON tab="Response"
	{
	"dateTime": 1377618543000,
	"operationID": 342,
	"error": null,
	"number": 95935
	}
	```

	```tab="Fields"
	Request :
		taskId: numeric not null;
		sessionID: alphanumeric not null and not empty;
		latitude: numeric not null;
		longitude: numeric not null;
		startTime: timestamp not empty and not null of the time that the attendant requests "Check-in"
	Response :
		dateTime: timestamp not empty and not null;
		operationID: numeric not empty and not null;
		error: alphanumeric value that can be null but not empty;
		number: alphanumeric not empty and not null;
	```

### Check-Out

!!! example "Checks out an attendant in a request, updating their status ("Suspended", "Solved", etc.)."
	```tab="Method"
 	POST
	```

	```HTML tab="URL"
 	<CITSMART_URL>/services/v2/checkout
	```

	```JSON tab="Request"
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

	```JSON tab="Response"
	{
	"dateTime": 1377618517000,
	"operationID": 342,
	"error": null,
	"number": 95935
	}
	```

	```tab="Fields"
	Request :
		taskId: numeric not null;
		sessionID: alphanumeric not null and not empty;
		latitude: numeric not null;
		longitude: numeric not null;
		status: numeric not null, according to return of service "ListNotivicationStatus"
		solution: numeric, informed only when a status of "Solved (4)" or "Suspended (2)"
		descSolution: alphanumeric, non-null and non-empty, reported only when status equal to "Solved (4)" or "Suspended (2)"
	Response :
		dateTime: timestamp not empty and not null;
		operationID: numeric not empty and not null;
		error: alphanumeric value that can be null but not empty;
		number: alphanumeric not empty and not null;
	```

### Check-InDenied

!!! example "Register the denial of a  request attendance by the attendant, in cases of refusal to the check-in. "
	```tab="Method"
 	POST
	```

	```HTML tab="URL"
 	<CITSMART_URL>/services/v2/denied
	```

	```JSON tab="Request"
	{
	"taskId": 22778,
	"sessionID": "2355A68BF75281B73607EEC1A7191645",
	"latitude": -19.369852147,
	"longitude": -49.369852147,
	"dateTime": 1377618543000,
	"reasonId": 1377618543000
	}
	```

	```JSON tab="Response"
	{
	"dateTime": 1377618543000,
	"operationID": 342,
	"error": null
	}
	```

	```tab="Fields"
	Request :
		taskId: numeric not null;
		sessionID: alphanumeric not empty and not null;
		latitude: numeric not null;
		longitude: numeric not null;
		reasonId: numeric and not null. Obtained by calling the "GetDeniedReasons" service;
	Response :
		dateTime: timestamp not empty and not null;
		operationID: numeric not empty and not null;
		error: alphanumeric value that can be null but not empty;
		number: alphanumeric not empty and not null;
	```

