Title: Webservices for use on 4biz
Description: 4biz Webservices

# Webservices for use on 4biz

This document is intended to provide guidance regarding the Web Services made available for integration with 4biz Service Management.

Web Services have been created in 4biz for inclusion, updating, consultation and cancellation of service requests (incidents and requisitions).


## Before getting started


1.  Before any 4biz REST operation is used, the user must be authenticated.
2.  Authentication is done through the REST login operation in the URL **/services/login**, which receives a **CtLogin** object containing the **userName**, **password**, and **platform** attributes.
3.  The platform attribute must contain the ID of the site that is requesting the service.
4.  The login operation returns an alphanumeric value in the **SessionID** attribute. This same **SessionID** must be used in the other REST calls. The returned object contains the code and description of the error in case of problems in executing the login operation.
5.  The authenticated user composes the key for data synchronization, when the **synchronize** attribute is set to **true**.
6.  Request inclusion and update services rely on the **synchronize** attribute. When this attribute is **true**, the user registration and catalog services are automatically created or updated in 4biz from the information sent in the Web Service request.

!!! Abstract "RULE"

    All REST services created in 4biz receive an input object and return an
    object. In case of error, the return object contains the code and
    description of the error. When there is no error, in addition to the
    attributes defined for each service, the return object contains the date and
    time of execution and the id of the operation. 4biz ensures that every
    request is recorded in its database and an operation ID is returned to the
    requester, even in case of error.


## Actions


### Create a Ticket

!!! example "Creating a Request /Incident"
    ```tab="URL"
    /services/request/create
    ```

    ```tab="Input Attributes"
    -synchronize - indicates whether the user and / or service information will be synchronized.
    -sourceRequest - request information from the CtRequest class, containing:
     -numberOrigin - the request number in the source system (mandatory. This attribute is required for 4biz to keep FROM-TO between its database and the original source system number.
     -type - request type (required). Possible values: I = Incident or R = Request.
     -description - description of the incident or request (required).
     -userID - applicant's user ID (required). It will be included if it does not exist in the 4biz database and the synchronize attribute is equal to true.
     -contact - data of the applicant. Required when the requestor does not exist in 4biz and the synchronize attribute is equal to true).
     -name - applicant name (required).
      -phoneNumber - applicant's phone number (required).
      -e-mail - applicant's e-mail (required).
      -contractID - contract number in 4biz (optional). If not informed, 4biz will include the request linked to the default contract parameterized in the service.
      -service - service data (optional). If not informed, 4biz will include the request linked to the default service parameterized in the WebService registry.
          -code - service code. Optional, if service name is given).
          -name - service Name. Required when the service does not exist in 4biz and the synchronize attribute is true.
          -category - category of service. Required when the service does not exist in 4biz and the synchronize attribute is true.
             -code - category code.
             -name - category name.
      -urgency - urgency of the request (optional). Possible values: H = High, M = Average, L = Low. If not informed, the urgency will be calculated from the 4biz service catalog parameters.
      -impact of request (optional). Possible values: H = High, M = Medium, L = Low. If not informed, the impact will be calculated from the 4biz service catalog parameters.
      -groupId - execution group acronym in 4biz (optional). If not informed, the executor group will be obtained from the 4biz service catalog parameters.
    ```

    ```tab="Output Attributes"
    Output Attributes are composed of all Input Attributes of the CtRequest class plus the following information:
      number - number of the request created in 4biz.
      startSLA - SLA start date and time.
      endSLA - SLA end date and time.
      status - status of the request, containing:
         code - code of the situation.
         name - name of the situation.
      currentTask - current task, containing:
         number - task number
         name - name of the task.
         startDateTime - start date and time
         status - task status, containing:
            code: code of the situation.
            name: name of the situation.
      userId - login of the user responsible for the task.
      groupId - acronym of the group responsible for the task.
    ```

    ```JSON tab="JSON Example"
    {"Synchronize": true,
    "sourceRequest": {"numberOrigin": "9999",
    "type": "R",
    "userID": " 61 84460708 ",
    " email ":" fulano.de.tal@centralit.com.br ",
    " department ":" Department of the So-and-so ",
    " name ":" So-and-so "},
    " description ":" REST v3 ",
    " service ": {" name ":" SERVICE.TEST.1 ",
    " category ": {" name ":" Category 1 "}},
    " contractID ":" 1 ",
    " urgency " : "H",
    "impact": "H"}
    }
    Assuming that the platform attribute in the login was informed by user and considering the synchronize attribute equal to true, the 4biz will:
      - Check if there is an FROM-TO of contract 1 for the "user";
      - Include the applicant in the user registry, if it does not exist in the database;
      - Include the service in the service catalog of contract 1, if it does not exist in the database and register the service FROM-TO for the client;
      - Include request with source number 9999;
      - Register the DEPUT from the 9999 source request to the client.
    ```
### Change ticket information

!!! example "Changing Information from Requests/Incidents"
    ```tab="URL"
    /services/request/create
    ```

    ```tab="Input Attributes"
    ​​synchronize - indicates whether the user and / or service information will be synchronized.
    request - request information from the CtRequest class, containing:
        numberOrigin - the request number on the source system. Required when the number attribute is not entered.
        description - description of the incident or request (optional).
        userID - applicant's user ID (required). It will be included if it does not exist in the 4biz database and the synchronize attribute is equal to true.
        number - request number in 4biz (required).
        contact - data of the applicant. Required when the requestor does not exist in 4biz and the synchronize attribute is equal to true).
            name - applicant name (required).
            phoneNumber - applicant's phone number (required).
            e-mail - applicant's e-mail (required)).
    ​    service - service data (optional).
            code - service code. Optional, if service name is given).
            name - service Name. Required when the service does not exist in 4biz and the synchronize attribute is true.
            category -category of service. Required when the service does not exist in 4biz and the synchronize attribute is true.
                Code - category code.
                Name - category name.
         Urgency - urgency of the request (optional). Possible values: H = High, M = Average, L = Low. If not informed, the urgency will be calculated from the 4biz service catalog parameters.
         Impact of request (optional). Possible values: H = High, M = Medium, L = Low. If not informed, the impact will be calculated from the 4biz service catalog parameters.
    ```

    ```tab="Output Attributes"
    Same as the Input
    ```

    ```JSON tab="JSON Example"
    {"Synchronize": true,
    "request": {"numberOrigin": "9999",
    "userID": "ciclano.de.tal",
    "contact": {"phoneNumber": "61 84460709",
    "email" "Cyclone",
    "name": "Cyclone of such"},
    "description": "Inclusion of request using REST v3 - Changed",
    "cyclano.de.tal@centralit.com.br",
    "department" "Service": {"name": "SERVICO.TESTE.2",
    "category": {"name": "Category 2"}}}}

    Assuming that the platform attribute in the login was informed by "user" and considering the synchronize attribute equal to true, the 4biz will:
        Include the applicant in the user registry, if it does not exist in the database;
        Include the service in the service catalog of contract 1, if it does not exist in the database and register the service FROM-TO for the client;
        Change the requestor and service from the request with source number 9999.
    ```

### Change ticket status


!!! example "Changing the Status of an Incidents/Requests"
    ```tab="URL"
    /services/request/updateStatus
    ```

    ```tab="Input Attributes"
    ​​number - request number in 4biz. Required when the numberOrigin attribute is not informed.
    numberOrigin - the request number on the source system. Required when the number attribute is not entered.
    status - status of the request, containing:
        code - situation code (required). Possible values: On End, Suspended, Canceled, Resolved, Reopened, Closed.
        details - complement of justification to change the situation (optional).
    ```

    ```tab="Output Attributes"
        Same as the Input
    ```

    ```JSON tab="JSON Example"
       {"numberOrigin": "9999",
       "status": {"code": "Suspended",
       "details": "Integration Testing"}}
    ```

###INQUIRE THE APPLICANT's INCIDENTS AND REQUESTS

!!! example "Inquire the applicant's Incidents and Requests"
    ```tab="URL"
    /services/request/getByUser
    ```

    ```tab="Input Attributes"
    userID - applicant's user ID (required).
    description - description of the incident or request (optional).
    startDate - start date of the request (optional).
    endDate - end date of the request (optional).
    service - service data (optional).
       code - service code.
       name - name of the service.
    contractID - contract number in 4biz (optional).
    status - request status (optional), containing:
       code - Location code. Possible values: Ending, Suspended, Canceled, Resolved, Reopened, Closed, Reclassified.
    ```

    ```tab="Output Attributes"
    Collection of objects of class CtRequest containing:
       number - request number in 4biz.
       numberOrigin - the request number on the source system.
       type - type of request. Possible values: I = Incident or R = Request
       description - description of the incident or request.
       userID - user ID of the requester.
       urgency - urgency of the request (optional). Possible values: H = High, M = Average, L = Low. If not informed, the urgency will be calculated from the 4biz service catalog parameters.
       impact of request (optional). Possible values: H = High, M = Medium, L = Low. If not informed, the impact will be calculated from the 4biz service catalog parameters.
       groupId - execution group acronym in 4biz (optional). If not informed, the executor group will be obtained from the 4biz service catalog parameters.
       startDateTime - the start date and time of the request.
       startSLA - SLA start date and time.
       endSLA - end date and time of the SLA.
       status - status of the request, containing:
          code - code of the situation.
          name -name of the situation.
    ```

    ```JSON tab="JSON Example"
    {"userID": "john elliot ",
    "startDate": "2015-09-16T03:00:00.000Z",
    "endDate": "2015-09-19T03:00:00.000Z"}
    ```
### Datails of ticket applicant

!!! example "Details of The Request/Incident"
    ```tab="URL"
    /services/request/getById
    ```

    ```tab="Input Attributes"
    number - request number in 4biz. Required when the numberOrigin attribute is not informed.
    numberOrigin - request number in the source system. Required when the number attribute is not entered.
    ```

    ```tab="Output Attributes"
    Output Attributes are composed of all Input Attributes of the CtRequest class plus the following information:
      number - number of the request created in 4biz.
      startSLA - SLA start date and time.
      endSLA - SLA end date and time.
      status - status of the request, containing:
         code - code of the situation.
         name - name of the situation.
      currentTask - current task, containing:
         number - task number
         name - name of the task.
         startDateTime - start date and time
         status - task status, containing:
            code: code of the situation.
            name: name of the situation.
      userId - login of the user responsible for the task.
      groupId - acronym of the group responsible for the task.
    ```

    ```JSON tab="JSON Example"
    {"numberOrigin":"9999"}
    ```


### Include occurrence in ticket

!!! example "Include an occourrence on a Request"
    ```tab="URL"
    /services/request/createOccurrence
    ```

    ```tab="Input Attributes"
    requestNumber - request number in 4biz. Required when the requestNumberOrigin attribute is not informed.
    requestNumberOrigin - request number in the source system. Required when the requestNumber attribute is not informed.
    ocurrence - object of class CtOccurrence, containing:
        numberOrigin - occurrence number in the source system (optional).
        description - occurrence description.
        date - date of record of occurrence.
        hour - time to record the occurrence in the format HH: MM.
        category - category of occurrence. Possible values: Monitoring, Update, Diagnostics, Investigation, Memo, Information, Return, Symptom, Outline, Scheduling.
        reason - reason for occurrence.
    ```

    ```tab="Output Attributes"
    Object of class CtOcurrence containing:
        number - event number in 4biz.
        numberOrigin - occurrence number in the source system.
        description - occurrence description.
        date - date of record of occurrence.
        Hour - time to record the occurrence in the format HH: MM.
        userID - identification of the user responsible for recording the occurrence.
        origin - origin of occurrence. Possible values: EMAIL, FONE_FAX, VOICE_MAIL, PERSONALLY, OTHERS.
        category - category of occurrence. Possible Values: Creation, Monitoring, Update, Diagnosis, Investigation, Memo, Information, Return, Symptom, Outline, Executing, Exchanging, Reclaiming, Reclassification, Schedule, Suspend, Reopen, Targeting, Sharing, Cancellation Task, HomeSLA, SuspendedSLA, Approval, ReactivationSLA
        elapsedTime - elapsed time (for Category of Executing type)
        reason – reason for occurrence.
        task - the task associated with the occurrence, containing:
           number - task number.
           name - name of the task.
           startDateTime - start date and time.
           endDateTime - date and time of execution.
           status - status of the task, containing:
              code - location code.
              name - name of the situation.
          userId - login of the user responsible for the execution of the task.
    ```

    ```JSON tab="JSON Example"
        {"requestNumberOrigin": "9999",
        "occurrence": {"description": "Occurrence test","category": {"code": "Workaround solution"},
        "date": "2015-08-20T03:00:00.000Z",
        "hour": "2219"}}
    ```

### Query ticket occurences

!!! example "Querying Information from Requests/Incidents"
    ```tab="URL"
    /services/request/listOccurrences
    ```

    ```tab="Input Attributes"
    requestNumber - request number in 4biz. Required when the requestNumberOrigin attribute is not informed.
    requestNumberOrigin - request number in the source system. Required when the requestNumber attribute is not informed.
    ```

    ```tab="Output Attributes"
    Object of class CtOcurrence containing:
        number - event number in 4biz.
        numberOrigin - occurrence number in the source system.
        description - occurrence description.
        date - date of record of occurrence.
        Hour - time to record the occurrence in the format HH: MM.
        userID - identification of the user responsible for recording the occurrence.
        origin - origin of occurrence. Possible values: EMAIL, FONE_FAX, VOICE_MAIL, PERSONALLY, OTHERS.
        category - category of occurrence. Possible Values: Creation, Monitoring, Update, Diagnosis, Investigation, Memo, Information, Return, Symptom, Outline, Executing, Exchanging, Reclaiming, Reclassification, Schedule, Suspend, Reopen, Targeting, Sharing, Cancellation Task, HomeSLA, SuspendedSLA, Approval, ReactivationSLA
        elapsedTime - elapsed time (for Category of Executing type)
        reason – reason for occurrence.
        task - the task associated with the occurrence, containing:
           number - task number.
           name - name of the task.
           startDateTime - start date and time.
           endDateTime - date and time of execution.
           status - status of the task, containing:
              code - location code.
              name - name of the situation.
          userId - login of the user responsible for the execution of the task.
    ```

    ```JSON tab="JSON Example"
    {"requestNumberOrigin":"9999"}
    ```
    
### Listing tickets to be attended

This webservice should be used to list the users who can be requesters when opening a ticket.

- Prerequisites: The requester must be linked to a group that is allowed to create in a workflow.

!!! example "Listing requests/incidents to be attended"
    ```tab="URL"
    /services/request/createOccurrence 
    /webmvc/servicerequestincident/searchTickets
    ```
    
    ```tab="Possible return codes"
    200 – Request was successful
    401 - Invalid authentication token or user without access to the resource
    406 - Not Acceptable - Business Exception
    ```
    
    ```tab="Input attributes"
    sessionID: Mandatory attribute that receives the session code;
    name: Non-mandatory attribute that receives the name of the requester, part of the name;
	    If the user gives the %%% information, the system will return all requesters in the system.
        •	userId: Mandatory attribute that receives the logged user code;
        •	selectedPage: Mandatory attribute that passes the page to be returned;
        •	Put the sessionID in the Header;
        •	Filters allowed in this webservice. The attributes described below are not mandatory:
            o	attendantId: Attribute that allows filtering by the name of the attendant;
            o	groupId: Attribute that allows filtering by group name;
            o	orderBy: Attribute that allows ordering the records by the following parameters:
                	TICKET_ID: Ticket number, 
                	CREATE_DATE: Creation date, 
                	LIMIT_DATE: Deadline for attending the ticket,
            o	orderDirection: Attribute that allows directing tickets in two ways:
                	ASC: Directs by the number of the ticket in an ascending way, 
                	DESC: Directs by the number of the ticket in a descending way
            o	requesterId: Attribute that allows filtering by the name of the requester;
            o	status: Attribute that allows filtering by ticket status:
                	IN_PROGRESS,
                	SUSPENDED, 
                	CANCELED, 
                	SOLVED, 
                	CLOSED 
            o	ticketId: Attribute that allows filtering by ticket number;
            o	unitId: Attribute that allows filtering by unit name;
    Example of inputs in the webservice
    {
        "userId": 4,
        "selectedPage": 1
    }

    Example of a webservice input with non-mandatory attributes
    {
        "userId": 4,
        "selectedPage": 1,
        "attendantId": 0,
        "groupId": 0,
        "orderBy": "TICKET_ID",
        "orderDirection": "ASC",
        "requesterId": 0,
        "selectedPage": 0,
        "status": "IN_PROGRESS",
        "ticketId": 0,
        "unitId": 0
    }


    ```tab="JSON Example"
    { 
    "sessionID": "39738a39-836d-4940-94d5-c9235035bb29",

    "name" : "poll" 
    }  
    Example of webservice input with non-mandatory attributes
    {
    "userId": 4,
    "selectedPage": 1,
    "attendantId": 0,
    "groupId": 0,
    "orderBy": "TICKET_ID",
    "orderDirection": "ASC",
    "requesterId": 0,
    "selectedPage": 0,
    "status": "IN_PROGRESS",
    "ticketId": 0,
    "unitId": 0
    }
    ```
    
    ```tab="Output attributes"
	" id " – Response that returns the requester code found by the search;
	“name” - Response that returns the requester's name;
	"email" – Response that returns the requester's email address;
	“Unit” – Response that returns
    	Id: Unit code;
        Name: Unit name;
    “places” – Response that returns:
    	Id: Location code;
    	Name: Location name;
    “phone” – Response that returns the requester's phone number;
    •	"id" – Response that returns the ticket number;
    •	“tipo” - Response that returns the type of demand, that is, if it is a Request (R), Incident (I) or Procedure (P);
    •	"nomePrioridade" – Response that returns the Priority name given to the ticket;
    •	“solicitacao” – Response that returns the description of the requested activity;
    •	“tarefa” – Response that returns the task of the flow that is the ticket;
    •	“status” – Response that returns the status of the listed ticket task;
    •	“dataLimite” – Response that returns the service request closing date and time according to the SLA and calendar linked to the activityxcontract
    •	“statusFluxoNome” - Response that returns the status of the SLA, which can be: Normal, To be expired, Expired, Suspended.
    Example of valid response of the webservice
    "code": "200",
    "message": "Request processed successfully",
    "payload":{
    "initialNumber": 1,
    "lastPage": 1.0,
    "finalNumber": 20,
    "totalRequests": 168,
    "result":
    [
    "id": 1251,
    "tipo": "Incident",
    "nomePrioridade": "Medium",
    "solicitacao": "Incident",
    "tarefa": "Attend request",
    "status": "NORMAL",
    "dataLimite": "2020-06-09 09:18:00 AM UTC"
    ]
    }
    ```

    ```tab="JSON Example"
    {
	    "status": "SUCCESS",
	    "code": "200",
	    "message": "Request processed successfully",
	    "payload": [
	        {
	            "id": 904,
	            "name": "Lucas Novais",
	            "email": "lucas@rogalabs.com",
	            "unit": {
	                "id": 1,
	                "name": "Default",
	                "places": [
	                    {
	                        "id": 1,
	                        "name": "Brasilia"
	                    }
	                ]
	            },
	            "phone": "(676) 76868-7687"
	        }
	    ]
	}
    ```

### Saving ticket in progress

This webservice should be used to return tickets to the be attended by the analysts.  
- Prerequests: Have access to the system and permission of execution in the workflow.

    ```tab="URL"
    To only save the ticket: webmvc/servicerequestincident/save  
    To save and advance the ticket: webmvc/servicerequestincident/next
    ```  
    
    ```tab="Possible return codes"
    200 – Request was successful  
    401 - Invalid authentication token or user without access to the resource  
    406 - Not Acceptable - Business Exception
    ```      
    
    ```tab="Input attributes"    
    authentication-token: Mandatory attribute that receives the authentication token
    	Put the authentication-token in the Header;
    serviceRequestId: Mandatory attribute that receives the ticket number; 
    taskId: Mandatory attribute that receives the ticket task number;
    contactEmail: Mandatory attribute that receives the requester's email address;  
    contactExtension: Non-mandatory attribute that receives the requester's phone extension number;  
    contactPhone: Non-mandatory attribute that receives the requester's phone number;  
    goupId: Non-mandatory attribute that receives the group code number that can receive the ticket;  
    statusId: Mandatory attribute that receives the status code number that the ticket will receive after recording the ticket. The status are:  
	In progress (1), Canceled (3) or Resolved (4)  
    originId: Non-mandatory attribute that receives the source code number that the ticket will receive after recording the ticket;  
    causeId: Non-mandatory attribute that receives the solution cause code number that the ticket will receive after recording the ticket;  
    idSolutionCategory: Non-mandatory attribute that receives the solution category code number that the ticket will receive after recording the ticket;  
    response: Non-mandatory attribute that receives the description of the response solution that the ticket will receive after recording the ticket;  
    builderObjects: Non-mandatory attribute that receives the captured responses if the ticket has a form;  
    quiz: Non-mandatory attribute that receives the captured responses if the ticket has a questionnaire;  
    flowAction: Attribute that receives the user action designed in the flow and answered by the attendant to solve the ticket;  
    reasonFlowAction: Attribute that receives the answer given to the Reason field, when it is designed in the flow as mandatory;  
      
    Example of input in the webservice:  
    { 
    	"serviceRequestId": 5409, 
	"taskId": 8342, 
	"contactEmail": "test@test.com", 
	"contactExtension": "4979",
	"contactPhone": "(64)999177012", 
	"goupId": 21, 
	"statusId": 1, 
	"originId": 59, 
	"causeId": 7, 
	"idSolutionCategory": 13, 
	"response": "Recording test via webservice", 
    }  
    ```
    
    ```tab="Output attribut"  
    " idGrupoAtual " – Response that returns the current ticket group code;  
    “idTarefa” - Response that returns the ticket task code;  
    " idStatus" – Response that returns the current ticket status code;  
    “status” – Response that returns the ticket status description;  
    “dataHoraInicio” – Response that returns the date and time the ticket was created;  
    “dataHoraInicioSLA” – Response that returns the ticket SLA start date and time;
    “dataHoraLimite” – Response that returns the closing date and time of the ticket SLA;  
    “dataHoraSolicitacao” - Response that returns the date and time the ticket was created;  
    Descrição: Response that returns the ticket description;  
    idCategoriaSolucao: Response that returns the ticket solution category code;  
    idCausaIncidente: Response that returns the ticket solution cause code;  
    idContrato: Response that returns the ticket contract code;  
    idServico: Response that returns the business service code or ticket support;  
    idSolicitacaoServico: Response that returns the ticket number;  
    idSolicitante: Response that returns the ticket requester code;  
    idUnidade: Response that returns the ticket unit code,  
    impacto: Response that returns the acronym of the impact;  
    resposta: Response that returns from the words of the answer solution;  
    siglaGrupo: Response that returns the group's acronym;  
    tarefa: Response that returns the task description;  
    urgencia: Response that returns the urgency of the ticket;  
    idUsuarioResponsavelAtual: Response that returns the code of the person responsible for the ticket;  
    nomeGrupoAtual: Response that returns the description of the current ticket group;  
    solicitanteVip: Response that returns whether the applicant is a VIP or not. Possible responses:  
    	false ou true
    ```  

    ```tab="JSON Example"
    {
    	"status": "SUCCESS", 
	"code": "200", 
	"message": "Request processed successfully", 
	"payload": {
		"idGrupoAtual": 171, 
		"idTarefa": 8809, 
		"idStatus": 1, 
		"status": "In Progress", 
		"dataHoraInicio": "2020-09-10 11:58:28 AM BRT", 
		"dataHoraInicioSLA": "2020-09-10 11:58:29 AM BRT", 
		"dataHoraLimite": "2020-09-10 17:08:00 PM BRT", 
		"dataHoraSolicitacao": "2020-09-10 11:58:28 AM BRT", 
		"descricao": "<div>test</div>", 
		"idCategoriaSolucao": 13, 
		"idCausaIncidente": 7, 
		"idContrato": 52, 
		"idServico": 670, 
		"idSolicitacaoServico": 5712, 
		"idSolicitante": 456, 
		"idUnidade": 2, 
		"impacto": "A", 
		"resposta": "Recording test via webservice", 
		"siglaGrupo": "LEVEL1", 
		"tarefa": "Answer Ticket", 
		"urgencia": "A", 
		"idUsuarioResponsavelAtual": 254,
		"nomeGrupoAtual": "Level 1", 
		"solicitanteVip": false
	}  
    }
    ```    

### Receive Units

This webservice must be used to return the existing active units in the system for selection when creating a ticket.

- Preconditions: This webservice changes results if parameter 61 - Link contracts to the unit (Eg.: Y or N) is active.

!!! example "Receive Units"
    ```tab="URL"
    webmvc/ v1/unit
    Method: GET
    Possible return codes
    200 – Successful request
    401 - Invalid authentication token or user without access to the resource
    404 – Justification not found
    ```

    ```tab="Input Attributes"
    •	authentication-token: Mandatory attribute that receives the authentication token
        o	Pass the authentication-token on the Header;
    •	employeeId: Mandatory attribute that receives the user code requesting the ticket:
        o	Pass the attribute as a parameter;
    •	Limit: Attribute that returns the number of units to be returned in the search;
        o	Pass the attribute as a parameter;
    •	unitQuery: Non-mandatory attribute that allows searching by part of the name;
        o	Pass the attribute as a parameter;
    Example of webservice input
    {
        It doesn't apply since the URL will pass all necessary attributes
    }
    ```

    ```tab="Output Attributes"
    •	id:  Returns the unit code;
    •	name: Returns the unit description;
    Example of webservice output
    {
        "status": "SUCCESS",
        "code": "200",
        "message": "Request processed successfully",
        "payload": [
            {
                "id": 52,
                "name": "---Human Resourcer"
            }
        ]
    }
    ```

### Receive justification for suspension

This webservice must be used to return the justification for suspension registered and active in the system.

- Preconditions: The user who is passed on the webservice must have permission to suspend in the workflow.

!!! example "
Receive justification for suspension"
    ```tab="URL"
    webmvc/v1/ticket/justification
    Method: GET
    Possible return codes
    200 – Successful request
    401 - Invalid authentication token or user without access to the resource
    404 – Justification not found
    ```

    ```tab="Input Attributes"
    •	authentication-token: Mandatory attribute that receives the authentication token
        o	Put the authentication-token on the Header;
    •	type: Mandatory attribute that receives one of the acronyms below:
        o	Put the type as parameter;
        o	Type (S = Suspension, A = Approval)
            Available values : S, A
            Default value : S
    Example of webservice input
    {
        It doesn't apply since the URL will give all necessary attributes
    }
    ```

    ```tab="Output Attributes"
    •	id:  Returns the code of Justification for suspension;
    •	description: Returns the description of the Justification for suspension
    Example of webservice output
    {
        "status": "SUCCESS",
        "code": "200",
        "message": "Request processed successfully",
        "payload": [
            {
                "id": 1,
                "description": "Default"
            }
        ]
    }
    ```
### List tickets to be attended

This webservice must be used to return the options allowed in the flow in a given group.

- Preconditions: The user who is passed on the webservice must have access to the system.
The user who is passed on the webservice must have permission in the workflow.

!!! example "List tickets to be attended"
    ```tab="URL"
    /webmvc/v1/ticket/{ticketId}/permissions
    Method: GET
    Possible return codes
    200 – Successful request
    401 - Invalid authentication token or user without access to the resource
    404 – Ticket not found
    ```

    ```tab="Input Attributes"
    •	authentication-token: Mandatory attribute that receives the authentication token
        o	Put the authentication-token on the Header;
    •	taskId: Mandatory attribute that receives the ticket task number;
        o	Put the taskId as parameter in the url;
    •	ticketId: Mandatory attribute that receives the ticket number;
    Example of webservice input
    {
        It doesn't apply since the URL will give all necessary attributes 
    }
    ```

    ```tab="Output Attributes"
    •	"id" – Response that returns the ticket number;
    •	“type” - Response that returns the type of demand, that is, if it's a Request (R), Incident (I) or Procedure (P);
    •	"namePriority" – Response that returns the Priority name given to the ticket;
    •	“request” – Response that returns the description of the requested activity;
    •	“task” – Response that returns the task of the flow that is the ticket;
    •	“status” – Response that returns the status of the listed ticket task;
    •	“dataLimite” – Response that returns the service request closing date and time according to the SLA and calendar linked to the activityxcontract
    •	“statusFlowName” - Response that returns the status of the SLA, which can be: Normal, To Be Expired, Expired, Suspended.
    Example of a valid webservice response
    "code": "200",
    "message": "Request processed successfully",
    "payload":{
    "initialNumber": 1,
    "lastPage": 1.0,
    "finalNumber": 20,
    "totalRequests": 168,
    "result":
    [
    "id": 1251,
    "type": "Incident",
    "namePriority": "Medium",
    "request": "Service of Incident",
    "task": "Attend request",
    "status": "NORMAL",
    "dataLimite": "2020-06-09 09:18:00 AM UTC"
    ]
    }
    ```
### Receive user actions on a ticket

This webservice must be used to return user actions designed in a workflow.

- Preconditions: The user who is passed on the webservice must have access to the system.
The user who is passed on the webservice must have permission to run in the workflow.

!!! example "Receive user actions on a ticket"
    ```tab="URL"
    webmvc/v1/ticket/{ticketId}/flow-actions
    Method: GET
    Possible return codes
    200 – Successful request
    401 - Invalid authentication token or user without access to the resource
    404 – Ticket not found
    Post condition:
    
To send the selected response in the user action, use the Save and Advance webservice (webmvc/servicerequestincident/next)
    Attributes:
    "flowAction": 
    "reasonFlowAction":
    ```

    ```tab="Input Attributes"
    •	authentication-token: Mandatory attribute that receives the authentication token
        o	Put the authentication-token on the Header;
    •	taskId: Mandatory attribute that receives the ticket task number;
        o	Put the taskId as a parameter in the url;
    •	ticketId: Mandatory attribute that receives the ticket number;
    Example of webservice input
    {
        It doesn't apply since the URL will pass all necessary attributes
    }
    ```

    ```tab="Output Attributes"
    •	"id:  Flow action code registered in the flow design;
    •	Name: Flow action name;
    •	Description: Description of the flow action;
    •	requiresReason: Inform whether the reason is mandatory, there are two responses to this attribute:
        o	True: When the reason is mandatory;
        o	False: When the reason is not mandatory;
    •	approvalActionId: Returns the approval response code;
    •	ticketStatusId: Returns the ticket status code;
    {
        "status": "SUCCESS",
        "code": "200",
        "message": "Request processed successfully",
        "payload": [
            {
                "id": 1302,
                "name": "ApproveTicket",
                "description": "Approve Ticket",
                "requiresReason": false,
                "approvalActionId": 1,
                "ticketStatusId": 1
            },
            {
                "id": 1304,
                "name": "DenyTicket",
                "description": "Deny Ticket",
                "requiresReason": false,
                "approvalActionId": 2,
                "ticketStatusId": 3
            }
        ]
    }
    ```
### List ticket attachments

This webservice must be used to return the list of attachments of a ticket to be assisted by the analysts.

- Preconditions: The user who is passed on the webservice must have access to the system.

Note: This document contains all the necessary webservices for an attachment that includes:
- List attachments of a ticket;
- Download the attachment;
- Attach document to the ticket (upload);
- Delete attachment of a ticket


!!! example "List tickets attachment"
    ```tab="URL"
    /webmvc/servicerequestincident/{serviceRequestIncidentId}/attachments
    Method type: GET
    Possible return codes
    200 – Successful request
    401 - Invalid authentication token or user without access to the resource
    404 - Ticket not found
    ```

    ```tab="Input Attributes"
    •	authentication-token: Attribute that receives the application access token:
        o	The token must be put in the header;
    •	serviceRequestIncidentId: Mandatory attribute that receives the ticket number;
        o	The ticket number must be passed in the path, next to the URL;
    Example of webservice input
    {
        Not applicable, note that the input attributes are in the header and in the Path of the url.
    }
    ```

    ```tab="Output Attributes"
    •	"id" – Response that returns the attachment code contained in the ticket;
    •	“name” - Response that returns the attachment description;
    •	"extension" – Response that returns the attachment extension;
    Example of a valid webservice response
    {
        "status": "SUCCESS",
        "code": "200",
        "message": "Request processed successfully",
        "payload": [
            {
                "id": 2253,
                "name": "ei_1598697802049.png.png",
                "extension": "png"
            },
            {
                "id": 2252,
                "name": "ei_1598697788434.jpg.jpg",
                "extension": "jpg"
            },
            {
                "id": 2251,
                "name": "ei_1598697777801.jpg.jpg",
                "extension": "jpg"
            }
    ]
    }
    ``` 
### Download ticket attachments

!!! example "Download ticket attachments"
    ```tab="URL"
    webmvc/servicerequestincident/{serviceRequestIncidentId}/attachments/{documentId} 
    Method type: GET
    Possible return codes
    200 – Successful request
    401 - Invalid authentication token or user without access to the resource
    404 - Document or ticket not found
    ```

    ```tab="Input Attributes"
    •	authentication-token: Attribute that receives the application access token:
        o	The token must be passed in the header;
    •	serviceRequestIncidentId: Mandatory attribute that receives the ticket number;
        o	The ticket number must be passed in the path, next to the URL;
    •	documentId: Mandatory attribute that receives the attachment code contained in the ticket;
        o	The attachment id must be passed in the path, next to the URL;
    Example of webservice input
    {
        Not applicable, note that the input attributes are in the header and in the Path of the url.
    }
    ```

    ```tab="Output Attributes"
    The attachment itself
    Example of a valid webservice response
    {
        Not applicable   
    }
    ```
### Upload ticket attachments

!!! example "Upload ticket attachments"
    ```tab="URL"
    /webmvc/services/request/addAttachments Method type: POST
    Preconditions:
    1.	Check the parameters:
    2.	44 - Upload directory repository path (Eg.: Windows - C:/temp)
    3.	278 - Maximum file size, in bytes, to upload. Default[1073741824] = 1GB
    4.	318 - List of file extensions that cannot be attached (For more than one extension, separate with a semicolon)
    5.	446 - Send attachments in the Ticket notification email? (Eg.: Y or N - Default: 'N')
    Possible return codes
    200 – Successful request
    500 – Mandatory fields not informed
    ```

    ```tab="Input Attributes"
    •	mediaType: Mandatory attribute indicating the conversation format
        o	Important to let it fixed in application/json
    •	requestNumber: Mandatory attribute that receives the request number that will receive the attachment;
    •	attachments: List of file;
    •	name: Mandatory attribute that receives the file name;
    •	content: Mandatory attribute that must be encoded in base64 that receives the contents of the file;
        o	The content is the content of the base64 encoded file;
    •	extension: Mandatory attribute that receives the file extension: txt, jpg, jpeg,
    Example of webservice input 
    {
	    "sessionID": "b7f24d64-5e23-4331-ab89-63403cb00d40",
	    "mediaType": "application/json",
	    "requestNumber": "1351",
	    "attachments": [{
		    "name": "test file",
		    "content": "VGVzdGU=",
		    "extension": "txt"
	    }]
    }
    ```

    ```tab="Output Attributes"
    •	dateTime: Mandatory attribute indicating date and time of execution;
    •	dateTimeMilliseconds: Hour in milliseconds;
    •	operationID: Number of the operation that was performed;
    •	error: Mandatory attribute that indicates if there was an error while running the webservice;
    Example of a valid webservice response
    {
        "dateTime": "2020-05-19 14:56:00",
        "dateTimeMilliseconds": 1589910960717,
        "operationID": 603,
        "error": null
    }
    ```
### Delete ticket attachments

!!! example "Delete ticket attachments"
    ```tab="URL"
    /webmvc/v1/ticket/{ticketId}/attachments/{documentId}
    Methode type: DELETE
    Possible return codes
    200 – Successful request
    401 - Invalid authentication token or user without access to the resource
    404 - Ticket not found
    ```

    ```tab="Input Attributes"
    •	authentication-token: Attribute that receives the application access token:
        o	The token must be passed in the header;
    •	serviceRequestIncidentId: Mandatory attribute that receives the ticket number;
        o	The ticket number must be passed in the path, next to the URL;
    •	documentId: Mandatory attribute that receives the attachment code contained in the ticket;
        o	The attachment id must be passed in the path, next to the URL;
    Example of webservice input
    {
        Not applicable, note that the input attributes are in the header and in the Path of the url.
    }
    ```

    ```tab="Output Attributes"
        {
    Not applicable
    }
    Example of a valid webservice response
    {
        "status": "SUCCESS",
        "code": "200",
    }
    ```
    
    
    

<hr>

<font  Size=2><b>Atualização:</b>09/16/2018 </font>
