Title: Informações Webservice
Description:  Webservices no Informações Gerais do CITSmart

# Webservices no CITSmart

<table width='100%'>
	<tr>
		<td><a href='#project-layout' width="30%" ><img src='/img/bn-webservices.png'></a> </td>
		<td class='md-typeset justificar' width="70%" > O Web Service é uma solução usada na integração de sistemas e comunicação entre diferentes aplicativos. Com essa tecnologia é possível que novas aplicações possam interagir com aquelas que já existem e que sistemas desenvolvidos em diferentes plataformas sejam compatíveis.</td>
	</tr>
	<tr>
		<td><a href='#project-layout' width="30%" ><img </a> </td>
		<td class='md-typeset justificar' width="70%" > Web Services são componentes que permitem que os aplicativos enviem e recebam dados no formato XML. Cada aplicativo pode ter sua própria "linguagem", que é traduzida em uma linguagem universal, o formato XML.</td>
	</tr>
</table>


Este documento descreve a implementação do WebService no CITSmart ESP. Chamado Citrest, o serviço da Web usa a implementação RESTEasy do padrão RESTFul. Através de exemplos práticos, os conceitos básicos, estruturas de dados e padrões a serem seguidos na implementação de novos serviços serão apresentados.

## O padrão RestEasy  

Representational State Transfer (REST) descreve as arquiteturas que usam o protocolo HTTP ou protocolos semelhantes, restringindo a interface a um conjunto de operações HTTP conhecidas: GET, POST, PUT e DELETE.

O Citrest usa o RESTEasy, que é uma implementação da especificação JAX-RS que fornece uma API Java para serviços da Web RESTful por meio do protocolo HTTP.

Este documento não tem a intenção de apresentar detalhes sobre a implementação RESTful ou RESTEasy, já que existe extensa documentação na Internet sobre o assunto, como no site http://www.jboss.org/resteasy.

## Modelo de dados 

Uma estrutura foi criada no banco de dados para armazenar os dados necessários para o funcionamento do Citrest. O modelo de dados rest_v2.pdm está localizado no diretório **CitCorporeWeb/Model** .

Todas as tabelas mantidas no Citrest possuem o prefixo Rest_ e possuem relacionamento com outras tabelas no modelo CITSmart: ObjetoNegocio, Grupo, Usuário e ProcessamentoBatch.

## Classes de Recursos  

As Classes de Recursos são classes simples, POJO, contendo anotações JAX-RS para indicar mapeamentos e operações existentes.

As classes Resources devem estar no pacote br.com.centralit.citsmart.rest.resource e seguir o padrão de nomenclatura usado nas outras classes Resources, Rest \ <NomeDoUC \> Resources.java.  

A Classe de Recurso que intercepta a chamada http para o webservice deve ser mapeada para o arquivo web.xml. Por exemplo:

!!! Example "Exemplo" 
    \<context-param\>  
    \<param-name\>resteasy-resources2\</param-name\>  
    \<param-value\>  
      br.com.centralit.citsmart.rest.resource.RestOperationResources  
    ​\</param-value\>  
    \</context-param\>  

Uma nova instância da Classe de Recurso é criada para cada solicitação do recurso. Cada método de recurso recebe como um parâmetro uma instância filho da classe CtMessage.java e retorna um objeto do tipo CtMessageResp. Nesta instância é atribuído o valor do atributo MessageID. Essa instância é passada como um parâmetro para o método Execute da classe de utilitário RestOperationUtil.java.

## Classes de Utilidade

A classe RestOperationUtil.java é responsável por realizar as validações e o direcionamento da solicitação de recurso para a classe responsável pela Operação.

O método Execute (entrada CtMessage) é o método chamado pelas classes de Recursos e recebe como parâmetro uma instância de CtMessage com o atributo MessageID atribuído.

A classe RestOperationUtil obtém junto à classe RestUtil.java uma instância de cada Serviço e executa as seguintes verificações:

-   Verifica se o SessionId existe e não está expirado.
-   Retorna um objeto RestSessionDTO associado ao SessionId.
-   Verifica se a operação existe na tabela Rest_Operation e retorna o objeto RestOperationDTO.
-   Verifica se algum grupo de usuários associado à sessão tem permissão na tabela Rest_Permission.

Depois que essas validações são feitas, a classe executa a Inicialização da Operação, registrando os atributos na tabela Rest_Execution. A tabela RestExecution funciona como uma tabela de Log de Execução. Observe que a execução é criada com o status NotInitiated, ou seja, a operação ainda não foi inicializada. Esse status será atualizado posteriormente de acordo com o resultado da execução da operação.

Depois de executar o log de execução, a classe RestOperationUtil instancia a classe Operation obtida do atributo JavaClass da tabela Rest_Operation e executa a chamada para o método execute.

O método Execute é uma condição do contrato da Interface IRestOperation. As classes que implementam essa interface precisam implementar o método de execução.

Para cada messageID, é feita uma chamada para um método específico para tratamento.

Cada um desses métodos pode fazer chamadas para o CITSmart Services Layer para reutilização de serviços.

## Regras Específicas

Todas as classes responsáveis pela operação do serviço da web devem ser registradas na tabela Rest_Operation.

Cada operação possui uma classe associada que obedece a uma interface padrão RestOperation e é responsável por sua execução.

A classe que executa a operação pode ser do tipo Java ou JavaScript (atributo ClassType). Atualmente, apenas o tipo Java é suportado.

Uma operação pode ser síncrona ou assíncrona (atributo OperationType). Uma operação síncrona é executada imediatamente quando é chamada. Uma operação assíncrona aponta para o processamento em lote.**Atualmente, somente operações síncronas são suportadas.**

Para que um determinado usuário possa executar uma operação, pelo menos um grupo de usuários deve estar associado à operação na tabela Rest_Permission.

A tabela Rest_Parameter armazena os parâmetros que podem ser usados para executar operações. Exemplos de parâmetros são:

| **idrestparameter** | **Identificador**   | **Descrição**             |
|---------------------|------------------|-----------------------------|
| **1**               | CONTRACT_ID      | Contract ID                 |
| **2**               | ORIGIN_ID        | Source ID                   |
| **3**               | REQUEST_ID       | Demand type ID for requests |
| **4**               | INCIDENT_ID      | Incident demand type ID     |
| **5**               | DEFAULT_DEPTO_ID | Unit Default ID             |

Cada operação pode ter um ou mais domínios de parâmetros na tabela Rest_Domain.

| **idrestparameter** | **idrestoperation** | **valor** |
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

Cada execução de uma operação específica é registrada na tabela Rest_Execution. Essa tabela registra a data e a hora da solicitação, o ID do usuário que está solicitando a execução, a classe de entrada, os dados de entrada e o status atual da execução.

Cada resultado de uma execução é registrado na tabela Rest_Log. Essa tabela registra a data e hora de execução, a classe de saída, os dados de saída e o status de execução.

Para a criação de um novo recurso, o desenvolvedor deve seguir as seguintes etapas de parametrização:

-   Registre uma operação na tabela Rest_Operation e diga qual classe Java irá executá-la.
-   Conceda permissões de execução de operação para um ou mais grupos na tabela Rest_Permission.
-   Registre os parâmetros na tabela Rest_Parameter.
-   Associe os domínios do parâmetro de operação na tabela Rest_Domain.

## Estrutura de Classe

Todas as classes usadas pelo Citrest devem ser definidas por .XSD específico. A partir de .XSD, as classes podem ser geradas automaticamente pelo plug-in do eclipse ou pelo xjc.jar, disponível na iniciativa 0015 no SharePoint. Para gerar as classes do xjc, você deve usar a seguinte linha de comando:

xjc "{path and name of xsd}" -d "{absolute path to src} eg:
D:\\Ambiente\\jboss\\server\\default\\deploy\\CitCorpore.war\\WEB-INF\\src}" -p {pakage name} eg: {br.com.centralit.citsmart.rest.schema}

O .XSD deve estar no pacote br.com.centralit.citsmart.rest.xsd e as classes geradas devem estar no pacote br.com.centralit.citsmart.rest.schema. Nesses pacotes já existem várias classes .XSD e várias usadas pelo Mobile que podem ser usadas como exemplo.

## Classe CtError

A classe CtError é referenciada pelas outras classes usadas para executar as operações do Citrest.

## Classe CtLogin E CtLoginResp

Toda operação em execução no Citrest requer um SessionID retornado pelo login. O login é implementado na classe br.com.centralit.citsmart.rest.resource.RestOperationResources e tem como entrada um objeto da classe CtLogin.

Como resultado do login, o objeto SessionID ou CtError é retornado pelo login por meio da classe CtLoginResp.

## Tratamento de Erro

O tratamento de erros de qualquer método de execução deve obedecer ao padrão de encapsulamento do objeto CtError implementado na classe RestOperationUtil.

!!! example "Exemplo"
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

## Exemplo Prático

Para facilitar a compreensão, esta seção detalha a implementação e a operação do serviço GetByUser usado no Mobile. É responsável por retornar a lista de requisições e incidentes no portfólio de trabalho de um determinado usuário.

As seguintes etapas foram seguidas para sua implementação:

1.  O XSD das classes CtNotificationGetByUser e CtNotificationGetByUserResp foi definido no arquivo br.com.centralit.citsmart.rest.xsd.MobileNotification.XSD
2.  As classes foram geradas no pacote br.com.centralit.citsmart.rest.schema por xjc.jar
3.  As entradas a seguir foram incluídas no arquivo Web.xml do projeto CITSmart:

!!! example "Exemplo"
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

Essas entradas especificam que existe uma nova classe de recurso para o RESTEasy e qualquer URL que contenha / mobile / será interceptada pelo servlet de serviço da web.

1.  A classe RestMobileResources foi criada da seguinte forma:
!!! Example ""  
    \@Path("/mobile")  
    **public** **class** RestMobileResources {  
    \@POST  
    \@Path("/notification/getByUser")  
    **public** Response getnotificationByUser(CtNotificationGetByUser input) {  
    input.setMessageID("notification_getByUser");  
    **return** RestOperationUtil.*execute*(input);  
    }  

A configuração e implementação web.xml acima determinam que
As chamadas http: //.../mobile/notification/getByUser serão interceptadas pela classe RestMobileResources.


1.  A execução da classe RestOperationUtil direciona a execução para a classe RestMobile porque é a classe associada à operação notification_getByUser na tabela Rest_Operation. Na classe RestMobile, existe o método execute que obedece à interface padrão:
!!! Example ""  
    public CtMessageResp execute(RestSessionDTO restSessionDto, RestExecutionDTO restExecutionDto, RestOperationDTO restOperationDto, CtMessage message) throws AXBException


<hr>
<font  Size=2><b>Produto/Versão:</b> CITSmart | 8.00</font> &nbsp; &nbsp;
<font  Size=2><b>Atualização:</b>07/01/2019 - João Pelles Junior</font>
	







