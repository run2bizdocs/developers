title: Creating documents using Swagger

description: How to create webservice documentation using Swagger 

# Creating documents using Swagger


## Procedure

To create documentation for webservices using Swagger you must follow a few step: 

 - The class that will be documented should receive the @Api note of the package _io.swagger.annotations.Api_, which consists of the identification of which category the webservice is related to, for example: ticket, change request, etc. 

 - The identification should be put inside the @Api note, in the property _tags_, which will receive a value in enum ApiTagsEnum. 

![imagem 1](/en-us/webservice/images-swagger/image1.png)

 - The enum _ApiTagsEnum_ is responsible for storing the categories the webservice has. Each enum is composed by the enum value and a Tag class instance (package _springfox.documentation.service.Tag_), which receives the enum value and the description of the enum.  

![imagem 1](/en-us/webservice/images-swagger/image2.png)
 

 - After configuring these scenarios, to actually insert the webservice documentation, you must create an interface that the webservice should implement. 

![imagem 1](/en-us/webservice/images-swagger/image3.png)
 
 - The interface is where the documentation will be written. These are the most used notations:  

    **@ApiResponses:** responsible for specifying what kind of responses the webservice can have, not considering success messages. It is possible to inform http response status codes and customize the message of the codes. 

    **@ApiOperation:** responsible for describing the endpoint action. 

    **@ApiParam:** responsible for specifying the endpoint parameters and/or if these parameters are required.

![imagem 1](/en-us/webservice/images-swagger/image4.png)

 - The other notations are commonly used in a webservice created with spring, which will be brought to the interface and implemented on the webservice. 

 - To check if the documentation was created succesfully, add _webmvc/swagger-ui.html#_ after  _citsmart/_ or _4biz_ on the URL. 

![imagem 1](/en-us/webservice/images-swagger/image5.png)

 - It is also possible to access the documentation of the webservice directly. Just type the webservice path on the url to see it. 

 
![imagem 1](/en-us/webservice/images-swagger/image6.png)
 

 
