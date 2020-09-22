Title: Webservices - 4biz
Description: Webservices para uso no 4biz

# Webservices para uso na plataforma 4biz

Este documento tem o propósito de fornecer orientações a respeito dos Web Services disponibilizados para integração com o Gerenciamento de Serviço do 4biz.


Os Web Services foram criados no 4biz para inclusão, atualização, consulta e cancelamento de solicitações de serviço (incidentes e requisições).


## Antes de começar


1.	Antes de se utilizar qualquer operação REST do 4biz, é necessário que o usuário esteja autenticado.
2.	A autenticação é feita através da operação REST login na URL **/services/login**, que recebe um objeto **CtLogin** contendo os atributos **userName**, **password** e **platform**.
3.	O atributo platform deve conter a identificação do site que está solicitando o serviço.
4.	A operação login retorna um valor alfanumérico no atributo **SessionID**. Este mesmo **SessionID** deve ser utilizado nas outras chamadas REST. O objeto retornado contém o código e descrição do erro em caso de problemas na execução da operação login.
5.	O usuário autenticado compõe a chave para sincronização dos dados, quando o atributo **synchronize** tiver o valor **true**.
6.	Os serviços de inclusão e atualização de solicitações contam com o atributo **synchronize**. Quando este atributo for **true**, o cadastro de usuário e o catálogo serviços serão automaticamente criados ou atualizados no 4biz a partir das informações enviadas na solicitação do Web Service.  

!!! Abstract "REGRA"

    Todos os serviços REST criados no 4biz recebem um objeto de entrada e
    retornam um objeto. Em caso de erro, o objeto de retorno contém o código e a
    descrição do erro. Quando não houver erro, além dos atributos definidos para
    cada serviço, o objeto de retorno contém a data e hora de execução e o id da
    operação. O 4biz garante que toda solicitação é registrada na sua base de
    dados e um ID da operação é retornado para o solicitante, mesmo em caso de erro.


## Ações

### Criar um ticket (request_create)

- Pré-condições: **configurar os contratos, grupos, fluxos e permissões**.

!!! example "Criando uma Requisição/Incidente"
    ```tab="URL"
    /services/request/create
    ```

    ```tab="Atributos de Entrada"
    -synchronize - indica se as informações de usuário e/ou serviço serão sincronizadas.
    -sourceRequest - informações da solicitação origem da classe CtRequest, contendo:
     -numberOrigin - número da solicitação no sistema de origem (obrigatório. Este atributo é necessário para que o 4biz mantenha o DE-PARA entre sua base de dados e o número original do sistema origem.
     -type - tipo da solicitação (obrigatório). Valores possíveis: I=Incidente ou R=Requisição.
     -description - descrição do incidente ou requisição (obrigatório).
     -userID - identificação de usuário do solicitante (obrigatório). Será incluído se não existir na base do 4biz e o atributo synchronize for igual a true.
     -contact - dados do solicitante. Obrigatório quando o solicitante não existir no 4biz e o atributo synchronize for igual a true).
     -name - nome do solicitante (obrigatório).
      -phoneNumber - telefone do solicitante (obrigatório).
      -e-mail - e-mail do solicitante (obrigatório).
      -contractID - número do contrato no 4biz (opcional). Se não for informado, o 4biz vai incluir a solicitação vinculada ao contrato default parametrizado no serviço.
      -service - dados do serviço (opcional). Se não for informado, o 4biz vai incluir a solicitação vinculada ao serviço default parametrizado no cadastro de WebService.
          -code - código do serviço. Opcional, se nome do serviço for informado).
          -name - nome do serviço. Obrigatório quando o serviço não existir no 4biz e o atributo synchronize for igual a true.
          -category - categoria do serviço. Obrigatório quando o serviço não existir no 4biz e o atributo synchronize for igual a true.
             -code - código da categoria.
             -name - nome da categoria.
      -urgency - urgência da solicitação (opcional). Valores possíveis: H=Alta, M=Média, L=Baixa. Se não for informada, a urgência será calculada partir dos parâmetros do catálogo de serviço do 4biz.
      -impact of request (opcional). Valores possíveis: H=Alto, M=Médio, L=Baixo. Se não for informado, o impacto será calculado a partir dos parâmetros do catálogo de serviço do 4biz.
      -groupId - sigla do grupo executor no 4biz (opcional). Se não for informada, o grupo executor será obtido a partir dos parâmetros do catálogo de serviço do 4biz.
    ```

    ```tab="Atributos de Saída"
    Os atributos de saída são compostos de todos os atributos de entrada da classe CtRequest, mais as seguintes informações:
      number - número da solicitação criada no 4biz ITSM.
      startSLA - data e hora de início do SLA.
      endSLA - data e hora de término do SLA.
      status - situação da solicitação, contendo:
         code - código da situação.
         name - nome da situação.
      currentTask - tarefa atual, contendo:
         number - número da tarefa
         name - nome da tarefa.
         startDateTime - data e hora de início
         status - situação da tarefa, contendo:
            code: código da situação.
            name: nome da situação.
      userId - login do usuário responsável pela tarefa.
      groupId - sigla do grupo responsável pela tarefa.
    ```

    ```JSON tab="Exemplo JSON"
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
    Supondo que no atributo platform no login foi informado "usuário" e considerando o atributo synchronize igual a true, o 4biz irá:
      - Verificar se existe um DE-PARA do contrato 1 para o "usuário";
      - Incluir o solicitante no cadastro de usuários, caso não exista na base;
      - Incluir o serviço no catálogo de serviços do contrato 1, caso não exista na base e registrar o DE-PARA de serviços para o cliente;
      - Incluir a solicitação com número de origem 9999;
      - Registrar o DE-PARA da solicitação de origem 9999 para o cliente.
    ```

### Alteração de Informações de um Ticket (create)

!!! example "Alterando informação de Requisição/Incidente"
    ```tab="URL"
    /services/request/create
    ```

    ```tab="Atributos de Entrada"
    ​​synchronize - indica se as informações de usuário e/ou serviço serão sincronizadas.
    request - informações da solicitação origem da classe CtRequest, contendo:
        numberOrigin - número da solicitação no sistema de origem. Obrigatório quando o atributo number não for informado.
        description - descrição do incidente ou requisição (opcional).
        userID - identificação de usuário do solicitante (obrigatório). Será incluído se não existir na base do 4biz e o atributo synchronize for igual a true.
        number - número da solicitação no 4biz (obrigatório).
        contact - dados do solicitante. Obrigatório quando o solicitante não existir no 4biz e o atributo synchronize for igual a true).
            name - nome do solicitante (obrigatório).
            phoneNumber - telefone do solicitante (obrigatório).
            e-mail - e-mail do solicitante (obrigatório).
    ​    service - dados do serviço (opcional).
            code - código do serviço. Opcional, se nome do serviço for informado).
            name - nome do serviço. Obrigatório quando o serviço não existir no 4biz e o atributo synchronize for igual a true.
            category - categoria do serviço. Obrigatório quando o serviço não existir no 4biz e o atributo synchronize for igual a true.
                Code - código da categoria.
                Name - nome da categoria.
         Urgency - urgência da solicitação (opcional). Valores possíveis: H=Alta, M=Média, L=Baixa. Se não for informada, a urgência será calculada partir dos parâmetros do catálogo de serviço do 4biz ITSM.
         impacto da solicitação (opcional). Valores possíveis: H=Alto, M=Médio, L=Baixo. Se não for informado, o impacto será calculado a partir dos parâmetros do catálogo de serviço do 4biz ITSM.
    ```

    ```tab="Atributos de Saída"
    Os mesmos que os atributos de entrada.
    ```

    ```JSON tab="Exemplo JSON"
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

    Supondo que no atributo platform no login foi informado "usuário" e considerando o atributo synchronize igual a true, o 4biz irá:
        Incluir o solicitante no cadastro de usuários, caso não exista na base;
        Incluir o serviço no catálogo de serviços do contrato 1, caso não exista na base e registrar o DE-PARA de serviços para o cliente;
        Alterar o solicitante e serviço da solicitação com número de origem 9999.
    ```

### Alteração da situação de um Ticket (updateStatus)


!!! example "Alteração da situação de um Incidente/Requisição"
    ```tab="URL"
    /services/request/updateStatus
    ```

    ```tab="Atributos de entrada"
    ​​number - número da solicitação no 4biz ITSM. Obrigatório quando o atributo numberOrigin não for informado.
    numberOrigin - número da solicitação no sistema de origem. Obrigatório quando o atributo number não for informado.
    status - situação da solicitação, contendo:
        code - código da situação (obrigatório). Valores possíveis: EmAndamento, Suspensa, Cancelada, Resolvida, Reaberta, Fechada.
        details - complemento da justificativa para alteração da situação (opcional).
    ```

    ```tab="Atributos de saída"
        Os mesmos que os atributos de entrada.
    ```

    ```JSON tab="Exemplo JSON"
       {"numberOrigin": "9999",
       "status": {"code": "Suspended",
       "details": "Integration Testing"}}
    ```

### Consultar Tickets do Solicitante (getByUser)

!!! example "Consultando Incidentes e Requisições do Solicitante"
    ```tab="URL"
    /services/request/getByUser
    ```

    ```tab="Atributos de entrada"
    userID - identificação de usuário do solicitante (obrigatório).
    description - descrição do incidente ou requisição (opcional).
    startDate - data de início da solicitação (opcional).
    endDate - data de finalização da solicitação (opcional).
    service - dados do serviço (opcional).
       code - código do serviço.
       name - nome do serviço.
    contractID - número do contrato no 4biz (opcional).
    status - situação da solicitação (opcional), contendo:
       code - código da situação. Valores possíveis: Em Andamento, Suspensa, Cancelada, Resolvida, Reaberta, Fechada, Reclassificada.
    ```

    ```tab="Atributos de Saída"
    Coleção de objetos da classe CtRequest contendo:
       number - número da solicitação no 4biz ITSM.
       numberOrigin - número da solicitação no sistema de origem.
       type - Tipo da solicitação. Valores possíveis: I=Incidente ou R=Requisição
       description - descrição do incidente ou requisição.
       userID - identificação de usuário do solicitante.
       urgency - urgência da solicitação (opcional). Valores possíveis: H=Alta, M=Média, L=Baixa. Se não for informada, a urgência será calculada partir dos parâmetros do catálogo de serviço do 4biz.
       groupId - sigla do grupo executor no 4biz (opcional). Se não for informada, o grupo executor será obtido a partir dos parâmetros do catálogo de serviço do 4biz.
       startDateTime - data e hora de início da solicitação.
       startSLA - data e hora de início do SLA.
       endSLA - data e hora de término do SLA.
       status - situação da solicitação, contendo:
          code - código da situação.
          name - nome da situação.
    ```

    ```JSON tab="Exemplo JSON"
    {"userID": "john elliot ",
    "startDate": "2015-09-16T03:00:00.000Z",
    "endDate": "2015-09-19T03:00:00.000Z"}
    ```
### Detalhes do Ticket de um Solicitante (getById)

!!! example "Detalhes da Requisição/Incidente"
    ```tab="URL"
    /services/request/getById
    ```

    ```tab="Atributos de entrada"
    number - número da solicitação no 4biz ITSM. Obrigatório quando o atributo numberOrigin não for informado.
    numberOrigin - número da solicitação no sistema de origem. Obrigatório quando o atributo number não for informado.
    ```

    ```tab="Atributos de saída"
    Atributos de Saída são compostos de todos os Atributos de Entrada da classe CtRequest mais as seguintes informações:
      number - número da solicitação criada no 4biz.
      startSLA - hora e data inicial do SLA.
      endSLA - hora e data final do SLA.
      status - status da solicitação, contendo:
         code - código da situação.
         name - nome da situação.
      currentTask - tarefa atual, contendo:
         number - número da tarefa
         name - nome da tarefa.
         startDateTime - hora e data inicial
         status - situação da tarefa, contendo:
            code: código da situação.
            name: nome da situação.
      userId - login do usuário responsável pela tarefa.
      groupId - acrônimo do grupo responsável pela tarefa.
    ```

    ```JSON tab="Exemplo JSON"
    {"numberOrigin":"9999"}
    ```


### Incluir Ocorrência no Ticket (createOccurrence)

!!! example "Inclui uma ocorrência em uma solicitação"
    ```tab="URL"
    /services/request/createOccurrence
    ```

    ```tab="Atributos de entrada"
    requestNumber - número da solicitação no 4biz. Obrigatório quando o atributo requestNumberOrigin não for informado.
    requestNumberOrigin - número da solicitação no sistema de origem. Obrigatório quando o atributo requestNumber não for informado.
    ocurrence - objeto da classe CtOccurrence, contendo:
        numberOrigin - número da ocorrência no sistema de origem (opcional).
        description - descrição da ocorrência.
        date - data de registro da ocorrência.
        hour - hora de registro da ocorrência no formato HH:MM.
        category - categoria da ocorrência. Valores possíveis: Acompanhamento, Atualização, Diagnostico, Investigação, Memorando, Informação, Retorno, Sintoma, Contorno, Agendamento.
        reason - motivo da ocorrência.
    ```

    ```tab="Atributos de saída"
    Objeto da classe CtOcurrence contendo:
        number - número da ocorrência no 4biz.
        numberOrigin - número da ocorrência no sistema de origem.
        description - descrição da ocorrência.
        date - data de registro da ocorrência.
        Hour - hora de registro da ocorrência no formato HH:MM.
        userID - identificação do usuário responsável pelo registro da ocorrência.
        origin - origem da ocorrência. Valores possíveis: EMAIL, FONE_FAX, VOICE_MAIL, PESSOALMENTE, OUTROS.
        category - categoria da ocorrência. Valores possíveis: Criacao, Acompanhamento, Atualizacao, Diagnostico, Investigacao, Memorando, Informacao, Retorno, Sintoma, Contorno, Execucao, MudancaSLA, Reclassificacao, Agendamento, Suspensao, Reabertura, Direcionamento, Compartilhamento, CancelamentoTarefa, InicioSLA, SuspensaoSLA, Aprovacao, ReativacaoSLA
        elapsedTime - tempo decorrido (para categoria do tipo Execucao)
        reason – motivo da ocorrência.
        task - tarefa associada à ocorrência, contendo:
           number - número da tarefa.
           name - nome da tarefa.
           startDateTime - data e hora de início.
           endDateTime - data e hora de execução.
           status - situação da tarefa, contendo:
              code - código da situação.
              name - nome da situação.
          userId - login do usuário responsável pela execução da tarefa.
    ```

    ```JSON tab="Exemplo JSON"
        {"requestNumberOrigin": "9999",
        "occurrence": {"description": "Occurrence test","category": {"code": "Workaround solution"},
        "date": "2015-08-20T03:00:00.000Z",
        "hour": "2219"}}
    ```

### Consultar ocorrências do Ticket (listOccurrences)

!!! example "Consultar informações das solicitações/incidentes"
    ```tab="URL"
    /services/request/listOccurrences
    ```

    ```tab="Atributos de entrada"
    requestNumber - número da solicitação no 4biz. Obrigatório quando o atributo requestNumberOrigin não for informado.
    requestNumberOrigin - número da solicitação no sistema de origem. Obrigatório quando o atributo requestNumber não for informado.
    ```

    ```tab="Atributos de saída"
    Objeto da classe CtOcurrence containing:
        number - número do evento na 4biz.
        numberOrigin - número da ocorrência no sistema de origem.
        description - descrição da ocorrência.
        date - data do registro da ocorrência.
        Hour - hora para registrar a ocorrência no formato HH:MM.
        userID - identificação do usuário responsável por registrar a ocorrência.
        origin - origem da ocorrência. Valores possíveis: EMAIL, FONE_FAX, VOICE_MAIL, PERSONALLY, OTHERS.
        category - categoria da ocorrência. Valores possibles: Creation, Monitoring, Update, Diagnosis, Investigation, Memo, Information, Return, Symptom, Outline, Executing, Exchanging, Reclaiming, Reclassification, Schedule, Suspend, Reopen, Targeting, Sharing, Cancellation Task, HomeSLA, SuspendedSLA, Approval, ReactivationSLA
        elapsedTime - tempo gasto (para categoria do tipo de execução)
        reason – motivo da ocorrência.
        task - tarefa associada com a ocorrência, contendo:
           number - número da tarefa.
           name - nome da tarefa.
           startDateTime - hora e data inicial.
           endDateTime - hora e data da execução.
           status - status da tarefa, contendo:
              code - código da localização.
              name - nome da situação.
          userId - login do usuário responsável pela execução da tarefa.
    ```

    ```JSON tab="Exemplo JSON"
    {"requestNumberOrigin":"9999"}
    ```
    
### Listar tickets para atendimento

Esse webservice deve ser utilizado para listar os usuários que podem ser solicitantes na abertura de um ticket.

- Pré-condições: O solicitante deve estar vinculado à um grupo que tenha permissão de criar em um fluxo de trabalho.

!!! example "Listar requisições/incidentes para atendimento"
    ```tab="URL"
    /services/request/createOccurrence 
    /webmvc/servicerequestincident/searchTickets
    ```
    
    ```tab="Possíveis códigos de retorno"
    200 – Requisição efetuada com sucesso
    401 - Invalid authentication token or user without access to the resource
    406 - Not Acceptable - Business Exception
    ```
    
    ```tab="Atributos de entrada"
    sessionID: Atributo obrigatório que recebe o código da sessão;
    name: Atributo não obrigatório que recebe o nome do solicitante, parte do nome;
	    Se o usuário passar a informação %%%, o sistema retornará todos os solicitantes do sistema.
        •	userId: Atributo obrigatório que recebe o código do usuário logado;
        •	selectedPage: Atributo obrigatório que passa a página a ser traga;
        •	Passar o sessionID no Header;
        •	Filtros permitidos nesse webservice, os atributos descritos abaixo não são obrigatórios:
            o	attendantId: Atributo que permite filtrar pelo nome do atendente;
            o	groupId: Atributo que permite filtrar pelo nome do grupo;
            o	orderBy: Atributo que permite ordenar os registros pelos seguintes parâmetros:
                	TICKET_ID: Número do ticket, 
                	CREATE_DATE: Data de criação, 
                	LIMIT_DATE: Data limite para atendimento do ticket,
            o	orderDirection: Atributo que permite direcionar os tickets de duas formas:
                	ASC: Direciona pelo número do ticket de forma crescente, 
                	DESC: Direciona pelo número do ticket de forma decrescente
            o	requesterId: Atributo que permite filtrar pelo nome do solicitante;
            o	status: Atributo que permite filtrar pelas situações dos tickets:
                	IN_PROGRESS,
                	SUSPENDED, 
                	CANCELED, 
                	SOLVED, 
                	CLOSED 
            o	ticketId: Atributo que permite filtrar pelo número do ticket;
            o	unitId: Atributo que permite filtrar pelo nome da unidade;
    Exemplo de entrada no webservice
    {
        "userId": 4,
        "selectedPage": 1
    }

    Exemplo de entrada no webservice com os atributos não obrigatórios
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


    ```tab="Exemplo JSON"
    { 
    "sessionID": "39738a39-836d-4940-94d5-c9235035bb29",

    "name" : "poll" 
    }  
    Exemplo de entrada no webservice com atributos não obrigatórios
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
    
    ```tab="Atributos de saída"
	" id " – Resposta que retorna o código do solicitante encontrado pela pesquisa;
	“name” - Resposta que retorna o nome do solicitante;
	"email" – Resposta que retorna o endereço de e-mail do solicitante;
	“Unit” – Resposta que retorna
    	Id: Código da unidade;
        Name: Nome da Unidade;
    “places” – Resposta que retorna:
    	Id: Código da localidade;
    	Name: Nome da localidade;
    “phone” – Resposta que retorna o número do telefone do solicitante;
    •	"id" – Resposta que retorna o número do ticket;
    •	“tipo” - Resposta que retorna o tipo de demanda, ou seja, se é uma Requisição (R), Incidente (I) ou Procedimento(P);
    •	"nomePrioridade" – Resposta que retorna o nome da Prioridade dada ao ticket;
    •	“solicitacao” – Resposta que retorna a descrição da atividade solicitada;
    •	“tarefa” – Resposta que retorna a tarefa do fluxo que se encontra o ticket;
    •	“status” – Resposta que retorna a situação da tarefa do ticket listado;
    •	“dataLimite” – Resposta que retorna a data e a hora de encerramento da solicitação de serviço conforme o SLA e calendário vinculado a atividadexcontrato
    •	“statusFluxoNome” - Resposta que retorna a situação do SLA, podendo ser: Normal, A Vencer, Vencido, Suspenso.
    Exemplo de resposta válida do webservice
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
    "tipo": "Incidente",
    "nomePrioridade": "Medium",
    "solicitacao": "Serviço de Incidente",
    "tarefa": "Atender solicitacao",
    "status": "NORMAL",
    "dataLimite": "2020-06-09 09:18:00 AM UTC"
    ]
    }
    ```

    ```tab="Exemplo JSON"
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

### Gravar ticket em atendimento

Esse webservice deve ser utilizado para retornar os tickets para atendimento dos analistas. 

- Pré-condições: Possuir acesso ao sistema e permissão de execução no fluxo de trabalho.

!!! example "Gravar ticket em atendiment"
    ```tab="URL"
    Para apenas gravar o ticket: webmvc/servicerequestincident/save  
    Para gravar e avançar o ticket: webmvc/servicerequestincident/next
    ```  
    
    ```tab="Possíveis códigos de retorno"
    200 – Requisição efetuada com sucesso  
    401 - Invalid authentication token or user without access to the resource  
    406 - Not Acceptable - Business Exception
    ```      
    
    ```tab="Atributos de entrada"    
    authentication-token: Atributo obrigatório que recebe o token de autenticação
    	Passar o authentication-token no Header;
    serviceRequestId: Atributo obrigatório que recebe o número do ticket; 
    taskId: Atributo obrigatório que recebe o número da tarefa do ticket;
    contactEmail: Atributo obrigatório que recebe o endereço de e-mail do solicitante;  
    contactExtension: Atributo não obrigatório que recebe o número do ramal do telefone do solicitante;  
    contactPhone: Atributo não obrigatório que recebe o número do telefone do solicitante;  
    goupId: Atributo não obrigatório que recebe o número do código do grupo que poderá receber o ticket;  
    statusId: Atributo obrigatório que recebe o número do código da situação que o ticket receberá após a gravação do ticket, sendo esses status:  
	In progress (1), Canceled (3) or Resolved (4)  
    originId: Atributo não obrigatório que recebe o número do código de origem que o ticket receberá após a gravação do ticket;  
    causeId: Atributo não obrigatório que recebe o número do código de causa de solução que o ticket receberá após a gravação do ticket;  
    idSolutionCategory: Atributo não obrigatório que recebe o número do código de categoria de solução que o ticket receberá após a gravação do ticket;  
    response: Atributo não obrigatório que recebe a descrição da solução resposta que o ticket receberá após a gravação do ticket;     builderObjects: Atributo não obrigatório que recebe as respostas capturadas caso, o ticket possua um formulário no ticket;  
    quiz: Atributo não obrigatório que recebe as respostas capturadas caso, o ticket possua um questionário no ticket;  
    flowAction: Atributo que recebe a ação de usuário desenhada no fluxo e respondida pelo atendente para solução do ticket;  
    reasonFlowAction: Atributo que recebe a resposta dada ao campo Motivo, quando essa é desenhada no fluxo como obrigatória;  
      
    Exemplo de entrada no webservice:  
    { 
    	"serviceRequestId": 5409, 
	"taskId": 8342, 
	"contactEmail": "teste@teste.com", 
	"contactExtension": "4979",
	"contactPhone": "(64)999177012", 
	"goupId": 21, 
	"statusId": 1, 
	"originId": 59, 
	"causeId": 7, 
	"idSolutionCategory": 13, 
	"response": "Teste de Gravação via webservice", 
    }  
    ```
    
    ```tab="Atributos de saída"  
    " idGrupoAtual " – Resposta que retorna o código do grupo atual do ticket;  
    “idTarefa” - Resposta que retorna o código da tarefa do ticket;  
    " idStatus" – Resposta que retorna o código da atual situação do ticket;  
    “status” – Resposta que retorna a descrição do status do ticket;  
    “dataHoraInicio” – Resposta que retorna a data e hora de criação do ticket;  
    “dataHoraInicioSLA” – Resposta que retorna a data e hora de início do SLA do ticket;
    “dataHoraLimite” – Resposta que retorna a data e hora de encerramento do SLA do ticket;  
    “dataHoraSolicitacao” - Resposta que retorna a data e hora de criação do ticket;  
    Descrição: Resposta que retorna a descrição do ticket;  
    idCategoriaSolucao: Resposta que retorna o código da categoria de solução do ticket;  
    idCausaIncidente: Resposta que retorna o código da causa de solução do ticket;  
    idContrato: Resposta que retorna o código do contrato do ticket;  
    idServico: Resposta que retorna o código do serviço de negócio ou apoio do ticket;  
    idSolicitacaoServico: Resposta que retorna o número do ticket;  
    idSolicitante: Resposta que retorna o código do solicitante do ticket;  
    idUnidade: Resposta que retorna o código da unidade do ticket,  
    impacto: Resposta que retorna a sigla do impacto;  
    resposta: Resposta que retorna dos dizeres da solução resposta;  
    siglaGrupo: Resposta que retorna a sigla do grupo;  
    tarefa: Resposta que retorna a descrição da tarefa;  
    urgencia: Resposta que retorna a sigla da urgência do ticket;  
    idUsuarioResponsavelAtual: Resposta que retorna o código do responsável do ticket;  
    nomeGrupoAtual: Resposta que retorna a descrição do grupo atual do ticket;  
    solicitanteVip: Resposta que retorna se o solicitante é vip ou não, respostas possíveis:  
    	false ou true
    ```  

    ```tab="Exemplo JSON"
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
		"resposta": "Teste de Gravação via webservice", 
		"siglaGrupo": "LEVEL1", 
		"tarefa": "Answer Ticket", 
		"urgencia": "A", 
		"idUsuarioResponsavelAtual": 254,
		"nomeGrupoAtual": "Level 1", 
		"solicitanteVip": false
	}  
    }
    ``` 

### Receber Unidades

Esse webservice deve ser utilizado para retornar as unidades ativas existentes no sistema para seleção na criação de um ticket.

- Pré-condições: Esse webservice sofre alteração de resultados caso o parâmetro 61 - Vincula contratos a unidade (Ex.: S ou N) esteja ativo.

A documentação de desenvolvimento está no Swagger

Exemplo: https://presentation02.citsmartcloud.com/4biz/webmvc/swagger-ui.html#/
    
Para ler essa documentação, é preciso estar logado na aplicação, e essa aplicação precisa estar na versão que possui esses webservices.

!!! example "Receber Unidades"
    ```tab="URL"
    webmvc/ v1/unit
    Método: GET
    Possíveis Códigos de retorno
    200 – Requisição efetuada com sucesso
    401 - Invalid authentication token or user without access to the resource
    404 – Justificativa não encontrada
    ```

    ```tab="Atributos de Entrada"
    •	authentication-token: Atributo obrigatório que recebe o token de autenticação
        o	Passar o authentication-token no Header;
    •	employeeId: Atributo obrigatório que recebe o código do usuário solicitante do ticket:
        o	Passar o atributo como parâmetro;
    •	Limit: Atributo que retorna a quantidade de unidades a serem retornadas na pesquisa;
        o	Passar o atributo como parâmetro;
    •	unitQuery: Atributo não obrigatório que permite pesquisar por parte do nome da unidade;
        o	Passar o atributo como parâmetro;
    Exemplo de entrada no webservice
    {
        Não se aplica uma vez que a URL passará todos os atributos necessários
    }
    ```

    ```tab="Atributos de Saída"
    •	id:  Retorna o código da unidade;
    •	name: Retorna a descrição da unidade;
    Exemplo de saída no webservice
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

### Receber justificativa de suspensão

Esse webservice deve ser utilizado para retornar as justificativas de suspensão cadastradas e ativas no sistema.

- Pré-condições: O usuário que é passado no webservice deve possuir permissão de suspender no fluxo de trabalho;

A documentação de desenvolvimento está no Swagger

Exemplo: https://presentation02.citsmartcloud.com/4biz/webmvc/swagger-ui.html#/
    
Para ler essa documentação, o usuário precisa estar logado na aplicação, e essa aplicação precisa estar na versão que possui esses webservices.

!!! example "Receber justificativa de suspensão"
    ```tab="URL"
    webmvc/v1/ticket/justification
    Método: GET
    Possíveis Códigos de retorno
    200 – Requisição efetuada com sucesso
    401 - Invalid authentication token or user without access to the resource
    404 – Justificativa não encontrada
    ```

    ```tab="Atributos de Entrada"
    •	authentication-token: Atributo obrigatório que recebe o token de autenticação
        o	Passar o authentication-token no Header;
    •	type: Atributo obrigatório que recebe uma das siglas abaixo:
        o	Passar o type como parâmetro;
        o	Type (S = Suspension, A = Approval)
            Available values : S, A
            Default value : S
    Exemplo de entrada no webservice
    {
        Não se aplica uma vez que a URL passará todos os atributos necessários
    }
    ```

    ```tab="Atributos de Saída"
    •	id:  Retorna o código da Justificativa de suspensão;
    •	description: Retorna a descrição da Justificativa de suspensão
    Exemplo de saída no webservice
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
### Listar tickets para atendimento

Esse webservice deve ser utilizado para retornar as opções permitidas no fluxo em um determinado grupo.

- Pré-condições: O usuário que é passado no webservice precisa possuir acesso ao sistema.
O usuário que é passado no webservice deve possuir permissão no fluxo de trabalho;

A documentação de desenvolvimento está no Swagger

Exemplo: https://presentation02.citsmartcloud.com/4biz/webmvc/swagger-ui.html#/
    
Para ler essa documentação, o usuário precisa estar logado na aplicação, e essa aplicação precisa estar na versão que possui esses webservices.

!!! example "Listar tickets para atendimento"
    ```tab="URL"
    /webmvc/v1/ticket/{ticketId}/permissions
    Método: GET
    Possíveis Códigos de retorno
    200 – Requisição efetuada com sucesso
    401 - Invalid authentication token or user without access to the resource
    404 – Ticket not found
    ```

    ```tab="Atributos de Entrada"
    •	authentication-token: Atributo obrigatório que recebe o token de autenticação
        o	Passar o authentication-token no Header;
    •	taskId: Atributo obrigatório que recebe o número da tarefa do ticket;
        o	Passar o taskId como parâmetro na url;
    •	ticketId: Atributo obrigatório que recebe o número do ticket;
    Exemplo de entrada no webservice
    {
        Não se aplica uma vez que a URL passará todos os atributos necessários 
    }
    ```

    ```tab="Atributos de Saída"
    •	"id" – Resposta que retorna o número do ticket;
    •	“tipo” - Resposta que retorna o tipo de demanda, ou seja, se é uma Requisição (R), Incidente (I) ou Procedimento(P);
    •	"nomePrioridade" – Resposta que retorna o nome da Prioridade dada ao ticket;
    •	“solicitacao” – Resposta que retorna a descrição da atividade solicitada;
    •	“tarefa” – Resposta que retorna a tarefa do fluxo que se encontra o ticket;
    •	“status” – Resposta que retorna a situação da tarefa do ticket listado;
    •	“dataLimite” – Resposta que retorna a data e a hora de encerramento da solicitação de serviço conforme o SLA e calendário vinculado a atividadexcontrato
    •	“statusFluxoNome” - Resposta que retorna a situação do SLA, podendo ser: Normal, A Vencer, Vencido, Suspenso.
    Exemplo de resposta válida do webservice
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
    "tipo": "Incidente",
    "nomePrioridade": "Medium",
    "solicitacao": "Serviço de Incidente",
    "tarefa": "Atender solicitacao",
    "status": "NORMAL",
    "dataLimite": "2020-06-09 09:18:00 AM UTC"
    ]
    }
    ```
### Receber ações de usuário em um ticket

Esse webservice deve ser utilizado para retornar as ações de usuário desenhadas em um fluxo de trabalho.

- Pré-condições: O usuário que é passado no webservice precisa possuir acesso ao sistema.
O usuário que é passado no webservice deve possuir permissão de execução no fluxo de trabalho;

A documentação de desenvolvimento está no Swagger

Exemplo: https://presentation02.citsmartcloud.com/4biz/webmvc/swagger-ui.html#/
    
Para ler essa documentação, o usuário precisa estar logado na aplicação, e essa aplicação precisa estar na versão que possui esses webservices.

!!! example "Receber ações de usuário em um ticket"
    ```tab="URL"
    webmvc/v1/ticket/{ticketId}/flow-actions
    Método: GET
    Possíveis Códigos de retorno
    200 – Requisição efetuada com sucesso
    401 - Invalid authentication token or user without access to the resource
    404 – Ticket não encontrado
    Pós condição:
    Para enviar a resposta selecionada na ação de usuário, utilize o webservice de Gravar e Avançar (webmvc/servicerequestincident/next)
    Atributos:
    "flowAction": 
    "reasonFlowAction":
    ```

    ```tab="Atributos de Entrada"
    •	authentication-token: Atributo obrigatório que recebe o token de autenticação
        o	Passar o authentication-token no Header;
    •	taskId: Atributo obrigatório que recebe o número da tarefa do ticket;
        o	Passar o taskId como parâmetro na url;
    •	ticketId: Atributo obrigatório que recebe o número do ticket;
    Exemplo de entrada no webservice
    {
        Não se aplica uma vez que a URL passará todos os atributos necessários
    }
    ```

    ```tab="Atributos de Saída"
    •	"id:  Código da ação de fluxo cadastrada no desenho de fluxo;
    •	Name: Nome da ação de fluxo;
    •	Description: Descrição da ação de fluxo;
    •	requiresReason: Informa se o motivo é obrigatório, existe duas respostas para esse atributo:
        o	True: Quando o motivo for obrigatório;
        o	False: Quando o motivo não for obrigatório;
    •	approvalActionId: Retorna o código da resposta de aprovação;
    •	ticketStatusId: Retorna o código da situação do ticket;
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
### Listar anexos dos tickets

Esse webservice deve ser utilizado para retornar a lista dos anexos de um  tickets para atendimento dos analistas.

- Pré-condições: O usuário que é passado no webservice precisa possuir acesso ao sistema.

A documentação de desenvolvimento está no Swagger

Exemplo: https://presentation02.citsmartcloud.com/4biz/webmvc/swagger-ui.html#/
    
Para ler essa documentação, o usuário precisa estar logado na aplicação, e essa aplicação precisa estar na versão que possui esses webservices.

Observação: Esse documento contém todos os webservices necessários para anexo que inclui:
- Listar anexos de um ticket;
- Realizar download do anexo;
- Anexar documento ao ticket (upload);
- Deletar anexo do ticket


!!! example "Listar anexos dos tickets"
    ```tab="URL"
    /webmvc/servicerequestincident/{serviceRequestIncidentId}/attachments
    Método tipo: GET
    Possíveis Códigos de retorno
    200 – Requisição efetuada com sucesso
    401 - Invalid authentication token or user without access to the resource
    404 - Ticket not found
    ```

    ```tab="Atributos de Entrada"
    •	authentication-token: Atributo que recebe o token de acesso à aplicação:
        o	O token deve ser passado no header;
    •	serviceRequestIncidentId: Atributo obrigatório que recebe o número do ticket;
        o	O número do ticket deve ser passado no path, junto à URL;
    Exemplo de entrada no webservice
    {
        Não se aplica, veja que os atributos de entrada estão no header e no Path da url.
    }
    ```

    ```tab="Atributos de Saída"
    •	"id" – Resposta que retorna o código do anexo contido no ticket;
    •	“name” - Resposta que retorna a descrição do anexo;
    •	"extension" – Resposta que retorna a extensão do anexo;
    Exemplo de resposta válida do webservice
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
### Realizar download de anexos de um ticket

!!! example "Realizar download de anexos de um ticket"
    ```tab="URL"
    webmvc/servicerequestincident/{serviceRequestIncidentId}/attachments/{documentId} 
    Método tipo: GET
    Possíveis Códigos de retorno
    200 – Requisição efetuada com sucesso
    401 - Invalid authentication token or user without access to the resource
    404 - Document or ticket not found
    ```

    ```tab="Atributos de Entrada"
    •	authentication-token: Atributo que recebe o token de acesso à aplicação:
        o	O token deve ser passado no header;
    •	serviceRequestIncidentId: Atributo obrigatório que recebe o número do ticket;
        o	O número do ticket deve ser passado no path, junto à URL;
    •	documentId: Atributo obrigatório que recebe o código do anexo contido no ticket;
        o	O id do anexo deve ser passado no path, junto à URL;
    Exemplo de entrada no webservice
    {
        Não se aplica, veja que os atributos de entrada estão no header e no Path da url.
    }
    ```

    ```tab="Atributos de Saída"
    O próprio anexo
    Exemplo de resposta válida do webservice
    {
        Não se aplica   
    }
    ```
### Upload anexos dos tickets

!!! example "Upload anexos dos tickets"
    ```tab="URL"
    /webmvc/services/request/addAttachments Método tipo: POST
    Pré Condição:
    1.	Verificar os parâmetros:
    2.	44 - Diretório Upload repositório path (Ex.: Windows - C:/temp)
    3.	278 - Tamanho máximo de arquivo, em bytes, para upload. Default[1073741824] = 1GB
    4.	318 - Lista de extensões de arquivos que não poderão ser anexados (Para mais de uma extensão separar por ponto e vírgula)
    5.	446 - Enviar anexos no e-mail de notificação do Ticket? (Ex.: S ou N - Default: 'N')
    Possíveis Códigos de retorno
    200 – Requisição efetuada com sucesso
    500 – Campos obrigatórios não informados
    ```

    ```tab="Atributos de Entrada"
    •	mediaType: Atributo obrigatório que indica o formato da conversa
        o	Importante deixar fixo em application/json
    •	requestNumber: Atributo obrigatório que recebe o número da requisição que receberá o anexo;
    •	attachments: Lista de arquivo;
    •	name: Atributo obrigatório que recebe o nome do arquivo;
    •	content: Atributo obrigatório que deverá estar codificado em base64 que recebe o conteúdo do arquivo;
        o	O content é o conteúdo do arquivo codificado em base64;
    •	extension: Atributo obrigatório que recebe a extensão do arquivo: txt, jpg, jpeg,
    Exemplo de entrada no webservice 
    {
	    "sessionID": "b7f24d64-5e23-4331-ab89-63403cb00d40",
	    "mediaType": "application/json",
	    "requestNumber": "1351",
	    "attachments": [{
		    "name": "arquivo teste",
		    "content": "VGVzdGU=",
		    "extension": "txt"
	    }]
    }
    ```

    ```tab="Atributos de Saída"
    •	dateTime: Atributo obrigatório que indica data e hora de execução;
    •	dateTimeMilliseconds: Hora em milissegundos;
    •	operationID: Número da operação que foi realizada;
    •	error: Atributo obrigatório que indica se houve erro durante a execução do webservice;
    Exemplo de resposta válida do webservice
    {
        "dateTime": "2020-05-19 14:56:00",
        "dateTimeMilliseconds": 1589910960717,
        "operationID": 603,
        "error": null
    }
    ```
### Deleta anexos dos tickets

!!! example "Deleta anexos dos tickets"
    ```tab="URL"
    /webmvc/v1/ticket/{ticketId}/attachments/{documentId}
    Método tipo: DELETE
    Possíveis Códigos de retorno
    200 – Requisição efetuada com sucesso
    401 - Invalid authentication token or user without access to the resource
    404 - Ticket not found
    ```

    ```tab="Atributos de Entrada"
    •	authentication-token: Atributo que recebe o token de acesso à aplicação:
        o	O token deve ser passado no header;
    •	serviceRequestIncidentId: Atributo obrigatório que recebe o número do ticket;
        o	O número do ticket deve ser passado no path, junto à URL;
    •	documentId: Atributo obrigatório que recebe o código do anexo contido no ticket;
        o	O id do anexo deve ser passado no path, junto à URL;
    Exemplo de entrada no webservice
    {
        Não se aplica, veja que os atributos de entrada estão no header e no Path da url.
    }
    ```

    ```tab="Atributos de Saída"
        {
    Não se aplica
    }
    Exemplo de resposta válida do webservice
    {
        "status": "SUCCESS",
        "code": "200",
    }
    ```

