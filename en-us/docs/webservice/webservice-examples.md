title: Examples of webservices usage
Description: Examples of webservices usage
# Examples of webservices usage 

Validate if requester has permission to register a request with the chosen activity
---------------------------------------------------------------------------------------

This webservice allows CITSmart to validate whether the ticket requesters is allowed to register 
a request with an activity defined by them.

Preconditions
----------------

- Configure WebService Operation: request_create

- Configure contracts, groups, flows and permissions.

How to access it
-----------------

1. Access the URL: /citsmart/services/request/create

Procedure
------------------

1. It's necessary to configure in the Webservice "request_create":

    - service property name must have the name of an activity that is active and in production in the portfolio;
    
    - the syncronize property must be true for a user to be registered;
    
    - parameter 45 - LDAP - ID Default Group must be configured with a group that has permission to record requests 
    in the group, contract, and flow of requests that will be registered.
    
    !!! warning "ATTENTION"
    
        A method has been added, which registers the new user to the group set in parameter.45 = LDAP - Default Group ID, 
        which is registered at the time WebServide request_create is started.This way, the new employee receives the 
        permission settings from this group.
        
    - the numberOrigin field must be completed in sequentially, as there is a validation that prevents the registration of 
    a request when the ticket sends an already sent numberOrigin;
    
2. Some settings in this WebService should be as follows: 

    - synchronize":true,
    
    - Service must have the property: "name": "Failure - Event Mgmt.";
    
*Example of call*

    ```sh
    {
    "sessionID":"61ec91e9-5397-466c-a342-08bbcd0e76a8",
    "synchronize":true,
    "sourceRequest":{
      "userID":"UsuarioCadastrado",
      "type":"I",
      "description": "Falha - Ger. Eventos",
      "contact":{
         "name":"Usuario Cadastrado",
         "phoneNumber":"000000000000",
         "email":"usuario@citsmart.com",
         "department":"CR 07.014/13.027 - Equipe SM: Desenvolvimento e Inovação (HSM)"
      },
      "service":{
         "code":"170",
         "name": "Falha - Ger. Eventos",
        "category":{
          "name":"[CITSmart] Produtos"
        }
      },
      "contractID":"2",
      "numberOrigin":"52362"
    }
    }
  ```
    
!!! tip "About"

    <b>Product/Version:</b> CITSmart | 8.00 &nbsp;&nbsp;
    <b>Updated:</b>09/09/2019 – Larissa Lourenço
