title: Change SLA via Rhino script
Description: It allows to change the SLA via rhino script
# Change SLA via Rhino script

This functionality is intended to change the SLA through Rhino script.

Procedure
------------

1. Access the main menu System \>
    Batch Processing;

2. Click on "New";

3. Complete the fields available, being:

    - type: RhinoScript;
    

    !!! Abstract "NOTE"

        Cron expression: it defines the routine execution time and its content,
        where it'll be described the context of the routine that will be executed in the tool.

    - content: see script in the next section.


4. Click on "Save".

Rhino Script
------------

```java
var importNames = JavaImporter();
importNames.importPackage(Packages.java.util);
importNames.importPackage(Packages.java.lang);
importNames.importPackage(Packages.java.sql);
importNames.importPackage(Packages.br.com.centralit.citcorpore.negocio);
importNames.importPackage(Packages.br.com.centralit.citcorpore.integracao);
importNames.importPackage(Packages.br.com.centralit.citcorpore.bean);
importNames.importPackage(Packages.br.com.citframework.util);
importNames.importPackage(Packages.br.com.citframework.comparacao);
importNames.importPackage(Packages.br.com.citframework.integracao);
importNames.importPackage(Packages.br.com.centralit.bpm.integracao);
importNames.importPackage(Packages.br.com.centralit.bpm.dto);
importNames.importPackage(Packages.br.com.centralit.citcorpore.bpm.negocio);
importNames.importPackage(Packages.br.com.citframework.excecao);
var jdbcEngine = new importNames.JdbcEngine(execucaoFluxo.getTransacao(),null);
var solicitacaoServicoService = new importNames.SolicitacaoServicoServiceEjb();
serviceRequest.setPrazohhAnterior(serviceRequest.getPrazoHH() );
serviceRequest.setPrazommAnterior(serviceRequest.getPrazoMM() );
serviceRequest.setRegistradoPor( usuario.getLogin() );
serviceRequest.setIdJustificativa(parseInt(4) );
serviceRequest.setComplementoJustificativa("ANS alterado via fluxo ITSM");
serviceRequest.setPrazoHH( parseInt(99) );
serviceRequest.setPrazoMM( parseInt(0) );
solicitacaoServicoService.updateSLA(serviceRequest,
execucaoFluxo.getTransacao());
```


!!! tip "About"

    <b>Product/Version:</b> CITSmart ESP | 8.00 &nbsp;&nbsp;
    <b>Updated:</b>01/30/2019 - Anna Martins
