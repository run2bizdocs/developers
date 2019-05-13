Title: Webservices Information
Description:  Webservices in CITSmart General Information

#Webservices in CITSmart

<table width='100%'>
	<tr>
		<td><a href='#project-layout' width="30%" ><img src='/img/bn-webservices.png'></a> </td>
		<td class='md-typeset justificar' width="70%" > Web Service is a solution used in systems integration and communication between different applications. With this technology it is possible that new applications can interact with those that already exist and that systems developed in different platforms are compatible.</td>
	</tr>
	<tr>
		<td><a href='#project-layout' width="30%" ><img </a> </td>
		<td class='md-typeset justificar' width="70%" > Web Services are components that allow applications to send and receive data in XML format. Each application can have its own "language", which is translated into a universal language, the XML format.</td>
	</tr>
</table>


This document describes the implementation of WebService in CITSmart ESP. Called Citrest, the web service uses the RESTEasy implementation of the RESTFul standard.
Through practical examples, the basic concepts, data structures, and patterns to be followed in the implementation of new services will be presented.

##The Standard RestEasy

Representational State Transfer (REST) describes architectures that use the HTTP protocol or similar protocols, restricting the interface to a set of known HTTP operations: GET, POST, PUT, and DELETE.

Citrest uses RESTEasy, which is an implementation of the JAX-RS specification that provides a Java API for RESTful Web Services through the HTTP protocol.

This document is not intended to present details about the RESTful or RESTEasy implementation, since there is extensive documentation on the internet about the subject, such as in the site [http://www.jboss.org/resteasy](https://www.jboss.org/resteasy).

##Data Model

A structure has been created in the database to store data required for Citrest to function. The rest_v2.pdm data model is located in the**CitCorporeWeb/Model **directory.

All tables maintained in Citrest have the prefix Rest\_ and have relationships with other tables in the CITSmart model: ObjetoNegocio, Grupo, Usuário, and ProcessamentoBatch.

##Classes Resources

The Resources classes are simple classes, POJO, containing JAX-RS annotations to indicate existing mappings and operations.

The Resources classes should be in the package br.com.centralit.citsmart.rest.resource and follow the naming pattern used in the other classes Resources, Rest \<NomeDoUC\> Resources.java.

The resource class that intercepts the http call to the webservice must be mapped to the web.xml file. For example:

!!! Example 
    \<context-param\>  
    \<param-name\>resteasy-resources2\</param-name\>  
    \<param-value\>  
      br.com.centralit.citsmart.rest.resource.RestOperationResources  
    ​\</param-value\>  
    \</context-param\>  

A new instance of the Resource class is created for each request to that resource. Each resource method receives as a parameter a child instance of the CtMessage.java class and returns an object of type CtMessageResp. In this instance is attributed th value of the MessageID attribute. This instance is passed as a parameter to the execute method of the RestOperationUtil.java utility class.

##Utility Classes

The RestOperationUtil.java class is responsible for performing the validations and targeting of the resource request for the class responsible for the Operation.

The execute method (CtMessage input) is the method called by the Resources classes and receives as parameter an instance of CtMessage with the attribute MessageID assigned.

The RestOperationUtil class obtains next to class RestUtil.java an instance of each Service and performs the following verifications:

-   Verifies whether the SessionId exists and is not expired.
-   Returns a RestSessionDTO object associated with the SessionId.
-   Verifies whether the operation exists in the Rest_Operation table and
    returns the RestOperationDTO object.
-   Verifies whether any associated user group with the session has permission
    on the Rest_Permission table.

Once these validations are done, the class performs the Operation Initialization, recording in the Rest_Execution table the attributes. The RestExecution table functions as a Execution Log table. Note that the execution is created with the status NotInitiated, that is, the operation has not yet been initialized. This status will be updated later according to the result of the operation execution.

After performing execution logging, the RestOperationUtil class instantiates the Operation class obtained from the JavaClass attribute of the Rest_Operation table and performs the call to the execute method.

The execute method is a condition of the IRestOperation Interface contract. The classes that implement this interface need to implement the execute method.

For each messageID a call is made to a specific method for treatment.

Each of these methods can make calls to the CITSmart Services Layer for reuse of services.

##Specific Rules

All classes responsible for webservice operation must be registered in the Rest_Operation table.

Each operation has an associated class that obeys a standard RestOperation interface and is responsible for its execution.

The class executing the operation can be of type Java or JavaScript (ClassType attribute). Currently only the Java type is supported.

An operation can be synchronous or asynchronous (OperationType attribute). A synchronous operation is performed immediately when it is called. An asynchronous operation points to batch processing. *Currently, only synchronous operations are supported.*

For a given user to be able to perform an operation, at least one user group must be associated with the operation in the Rest_Permission table.

The Rest_Parameter table stores parameters that can be used to perform operations. Examples of parameters are:

| **idrestparameter** | **Identifier**   | **Description**             |
|---------------------|------------------|-----------------------------|
| **1**               | CONTRACT_ID      | Contract ID                 |
| **2**               | ORIGIN_ID        | Source ID                   |
| **3**               | REQUEST_ID       | Demand type ID for requests |
| **4**               | INCIDENT_ID      | Incident demand type ID     |
| **5**               | DEFAULT_DEPTO_ID | Unit Default ID             |

Each operation can have one or more parameter domains in the Rest_Domain table.

| **idrestparameter** | **idrestoperation** | **value** |
|---------------------|---------------------|-----------|
| **1**               | 1                   | 1         |
| **1**               | 6                   | 1         |
| **2**               | 1                   | 7         |
| **2**               | 6                   | 10        |
| **3**               | 1                   | 1         |
| **3**               | 6                   | 1         |
| **4**               | 1                   | 3         |
| **4**               | 6                   | 3         |
| **5**               | 1                   | 3         |
| **5**               | 6                   | 3         |

Each execution of a particular operation is recorded in the Rest_Execution table. This table records the date and time of the request, the id of the user requesting the execution, the input class, the input data, and the current execution status.

Each result of an execution is recorded in the Rest_Log table. This table records the execution date and time, the output class, the output data, and the execution status.

For the creation of a new resource the developer must follow the following
parameterization steps:

-   Record an operation in the Rest_Operation table and tell which Java class will execute it.
-   Give operation execution permissions to one or more groups in the Rest_Permission table.
-   Record parameters in the Rest_Parameter table.
-   Associate the operation parameter domains in the Rest_Domain table.

##Class Structure

All classes used by Citrest must be defined by specific .XSD. From .XSD, classes can be generated automatically through the eclipse plugin or by xjc.jar, available at initiative 0015 in SharePoint. To generate the classes from xjc, you must use the following command line:

xjc "{path and name of xsd}" -d "{absolute path to src} eg:
D:\\Ambiente\\jboss\\server\\default\\deploy\\CitCorpore.war\\WEB-INF\\src}" -p {pakage name} eg: {br.com.centralit.citsmart.rest.schema}

The .XSD should be in the br.com.centralit.citsmart.rest.xsd package and thegenerated classes should be in the br.com.centralit.citsmart.rest.schema package. In these packages there are already several .XSD and several classes used by Mobile that can be used as an example.

##Class CtError

The CtError class is referenced by the other classes used to execute the Citrest operations.

##Classe CtLogin E CtLoginResp

Every running operation on Citrest requires a SessionID returned by login. The login is implemented in class
br.com.centralit.citsmart.rest.resource.RestOperationResources and has as input an object of class CtLogin.

As a result of the login, the SessionID or a CtError object is returned by login through the CtLoginResp class.

##Error Treatment

The error handling of any execute method must obey the encapsulation pattern of the CtError object implemented in the RestOperationUtil class.

!!! example
    CtNotificationGetReasonsResp resp = **new** CtNotificationGetReasonsResp();  
    CtNotificationGetReasons input = (CtNotificationGetReasons) message;  
    **if** (input.getTaskId() == **null**) {  
    resp.setError(RestOperationUtil.*buildError*( RestEnum.*INPUT_ERROR*, "Id da tarefa não informado"));  
    **return** resp;  
    }  
    ...  
    } **catch** (Exception e) {  
    e.printStackTrace();  
    resp.setError(RestOperationUtil.*buildError*(e));  
    **return** resp;  
    }

##Practical Example

For ease of understanding, this section details the implementation and operation of the GetByUser service used in Mobile. It is responsible for returning the list of requisitions and incidents in a given user's work portfolio.

The following steps were followed for its implementation:

1.  The XSD of the CtNotificationGetByUser and CtNotificationGetByUserResp classes were defined in the file br.com.centralit.citsmart.rest.xsd.MobileNotification.XSD
2.  The classes were generated in the package br.com.centralit.citsmart.rest.schema by xjc.jar
3.  The following entries have been added in the Web.xml file of the CITSmart project:

!!! example ""
    \<context-param\>  
    \<param-name\>resteasy-resources3\</param-name\>  
    \<param-value\>  
      br.com.centralit.citsmart.rest.resource.RestMobileResources  
    \</param-value\>  
    \</context-param\>  
    \<servlet-mapping\>  
    \<servlet-name\>resteasy-servlet\</servlet-name\>  
    \<url-pattern\>/mobile/\*\</url-pattern\>  
    \</servlet-mapping\>  

These entries specify that a new resource class exists for RESTEasy and any URL containing /mobile/ will be intercepted by the web service servlet.

1.  The RestMobileResources class was created as follows:
!!! Example ""  
    \@Path("/mobile")  
    **public** **class** RestMobileResources {  
    \@POST  
    \@Path("/notification/getByUser")  
    **public** Response getnotificationByUser(CtNotificationGetByUser input) {  
    input.setMessageID("notification_getByUser");  
    **return** RestOperationUtil.*execute*(input);  
    }  

The web.xml configuration and implementation above determine that  
http://.../mobile/notification/getByUser calls will be intercepted by the
RestMobileResources class.

1.  The run of the RestOperationUtil class directs execution to the RestMobile class because it is the class that is associated with the notification_getByUser operation in the Rest_Operation table. In the RestMobile class, there is the execute method that obeys the default interface:
!!! Example ""  
    public CtMessageResp execute(RestSessionDTO restSessionDto, RestExecutionDTO restExecutionDto, RestOperationDTO restOperationDto, CtMessage message) throws AXBException


<hr>
<font  Size=2><b>Produto/Versão:</b> CITSmart | 8.00</font> &nbsp; &nbsp;
<font  Size=2><b>Atualização:</b>13/12/2018 - Andre Luiz de Oliveira Fernandes</font>
	







