title: Alterar SLA via script Rhino
Description: Permite alterar SLA via script rhino
# Alterar SLA via script Rhino

Esta funcionalidade tem por objetivo alterar o SLA através de script Rhino.

Procedimento
------------

1. Acessar a funcionalidade através da navegação no menu principal Sistema \>
    Processamento Batch;

2. Clicar no botão "Novo";

3. Preencher os campos disponibilizados, sendo:

    - tipo: RhinoScript;
    

    !!! Abstract "NOTA"

        Expressão cron: define o horário de execução da rotina e o conteúdo da
        rotina, onde será descrito o contexto da rotina a ser executada na
        ferramenta.

   - conteúdo: ver script na próxima seção.

4. Clicar em "Gravar".

Script Rhino
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

    <b>Product/Version:</b> CITSmart | 8.00 &nbsp;&nbsp;
    <b>Updated:</b>01/30/2019 - Anna Martins
