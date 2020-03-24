Title: Webservices for use on 4biz
Description: 4biz Webservices

# Webservices for use on 4biz

This document is intended to provide guidance regarding the Web Services made available for integration with CTSmart ITSM Service Management.

Web Services have been created in CTSmart for inclusion, updating, consultation and cancellation of service requests (incidents and requisitions).


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

<hr>
<font  Size=2><b>Produto/Versão:</b> 4biz | 8.00</font> &nbsp; &nbsp;
<font  Size=2><b>Atualização:</b>12/12/2018 - Andre Luiz de Oliveira Fernandes</font>
