title: Criação de documentação usando Swagger

description: Como criar a documentação de um webservice usando o Swagger

# Criação de documentação usando Swagger


## Procedimento 

Para criação da documentação de um webservice com a utilização de Swagger deve-se utilizar dos seguintes critérios: 

 - A classe que será documentada deve receber a anotação @Api do pacote _io.swagger.annotations.Api_, que consiste na identificação da categoria a qual o webservice se refere, como por exemplo: ticket, requisição de mudança etc. 

 - Dentro da anotação @Api, na propriedade _tags_ é onde se deve colocar a identificação da categoria do webservice a qual recebe um valor do enum _ApiTagsEnum_. 

![imagem 1](/pt-br//webservice/images-swagger/image1.png)

 - O enum _ApiTagsEnum_ é o responsável por guardar as categorias ao qual o webservice pode pertencer. Cada enum é composto pelo valor do enum e uma instãncia da classe Tag (pacote _springfox.documentation.service.Tag_) que recebe o valor do enum e a descrição do que esse enum representa.  

![imagem 1](/pt-br//webservice/images-swagger/image2.png)
 

 - Após a configuração desses cenários, para inserir a documentação do webservice de fato, deve se criar uma interface a qual o webservice relacionado deve implementar. 

![imagem 1](/pt-br//webservice/images-swagger/image3.png)
 
 - Na interface é o lugar onde a documentação será escrita de fato. São utilizadas mais frequentemente as seguintes anotações:  

    **@ApiResponses:** responsável por especificar quais os tipos de retorno que aquele webservice pode ter, desconsiderando as mensagens de sucesso. É possível informar códigos de status de reposta http e personalizar a mensagem do código desejado. 

    **@ApiOperation:** é a anotação responsável por descrever a ação do endpoint desejado. 

    **@ApiParam:** anotação responsável por especificar quais os parâmetros do endpoint e/ou se estes parâmetros são obrigatórios. 

![imagem 1](/pt-br//webservice/images-swagger/image4.png)

 - As demais anotações são as comumente utilizadas em um webservice criado utilizando o spring, que agora são trazidas para a interface para serem implementadas no webservice desejado. 

 - Para verificar se a documentação foi adicionada com sucesso, adicione _webmvc/swagger-ui.html#_ após o  _citsmart/_ ou _4biz_ na url. 

![imagem 1](/pt-br//webservice/images-swagger/image5.png)

 - Também é possível acessar diretamente a documentação do webservice desejado. Basta informar na url o path do webservice que deseja ver a documentação. 

 ![imagem 1](/pt-br//webservice/images-swagger/image6.png)
 

 
