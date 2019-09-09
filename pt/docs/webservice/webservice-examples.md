title: Exemplos de utilização de webservices
Description: Exemplos de utilização de webservices
# Exemplos de utilização de webservices 

##Ticket##

Validar se o solicitante possui permissão para registrar uma solicitação com a atividade escolhida
----------------------------------------------------------------------------------------------------

Este webservice permite ao CITSmart validar se o solicitante do ticket tem permissão para registrar uma solicitação com a
atividade por ele definida.

Pré-condições
----------------

- Configurar a WebService Operation: request_create

- Configurar os contratos, grupos, fluxos e permissões.

Como acessar
-----------------

1. Acessar URL: /citsmart/services/request/create

Procedimento
------------------

1. É necessário configurar no Web service "request_create" :

    - a propriedade name do objeto service deve ter o nome de uma atividade que esteja ativa e em produção no portfólio;
    
    - a propriedade syncronize deve estar true para que seja possível cadastrar um usuário;
    
    - o parâmetro 45 - LDAP - ID Grupo Padrão deve estar configurado com um grupo que tenha permissão para registrar as 
    solicitações no grupo, contrato e fluxo das solicitações que serão registradas.
    
    !!! warning "ATENÇÃO"
    
        Foi adicionado um método, que registra o novo usuário ao grupo setado no parametro.45 = LDAP - ID Grupo Padrão , 
        que é cadastrado no momento em que o WebServide request_create é acionado. Desta forma, o novo empregado recebe as 
        configurações de permissão deste grupo.
        
    - o campo numberOrigin deve ser preenchido sequencialmente, pois há uma validação que impede o registro de uma solicitação,
    quando a chamada envia um numberOrigin já enviado;
    
2. Algumas configurações neste WebService devem estar da seguinte forma: 

    - synchronize":true,
    
    - O service tem que ter a propriedade: "name": "Falha - Ger. Eventos";
    
*Exemplo de chamada*

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
