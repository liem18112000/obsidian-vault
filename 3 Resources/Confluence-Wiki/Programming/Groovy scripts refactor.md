---
ai_hash: ef4e21449a99cbea
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.65
entities: []
relevance: 0.738
source: https://axonivy.atlassian.net/wiki/spaces/LUZCOMP/pages/20675764785/Groovy+scripts+refactor
space: LUZCOMP
status: reference
tags:
- confluence
- programming
- space/luzcomp
title: Groovy scripts refactor
topic: programming
type: source
updated: 2016-07-12
---

# Groovy scripts refactor

> [!info] Imported from Confluence
> Space **LUZCOMP** · updated 2016-07-12 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZCOMP/pages/20675764785/Groovy+scripts+refactor)
> Relevance 0.738 · topic `programming`

Today I have short time to take a look on our current groovy files we have on luzcomp_scripts, there are some duplicated code blocks that I think we should refactor it. 

Some points that we should improve as 

- Get service URLs by resolving hostname / port ( it may come from Kubernetes Cluster) 
- Define common methods that should be used for all scripts in our projects

Below is a class that I think it is useful for us to initialize a REST resource and easy for us to make a call with some common methods GET / PUT / POST / DELETE

1.  Define a RestClient.groovy

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="4eed4471-673c-4d6e-bba4-318fce089a43" macro-name="code" style="border-width: 1px;">

    <div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

    **RestClient.groovy**

    </div>

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    package com.axonivy.scripting;

    import groovyx.net.http.HTTPBuilder;
    import static groovyx.net.http.Method.*;
    import static groovyx.net.http.ContentType.*;
    import groovy.json.*;
    import org.yaml.snakeyaml.Yaml;
    import java.time.LocalDate;
    import java.time.format.DateTimeFormatter;

    public class RestClient {
        private static final String DEFAULT_HOSTNAME = 'localhost';
        private static final String DEFAULT_PORT = '8080';
        private static final String DATE_FORMAT_USING_IN_GROOVY = "dd.MM.yyyy";
        private static final String DATE_TIME_SHORT_FORMAT_CONFORM_TO_ISO_8601 = "yyyy-MM-dd";
        private String envServiceName;
        private String serviceContextPath;
        private String hostname;
        private String port;
        private String serviceURL; 

        public RestClient(String hostname, String port, String serviceContextPath) {
            this.hostname = hostname;
            this.port = port;
            this.serviceContextPath = serviceContextPath;
            this.serviceURL = extractURL(hostname, port, serviceContextPath);
        }

        public RestClient(String envServiceName, String serviceContextPath) {
            this.envServiceName = envServiceName;
            this.serviceContextPath = serviceContextPath;
            this.serviceURL = getServiceURL(envServiceName, serviceContextPath);
        }

        public List getList(String pathInput, LinkedHashMap queryJson) {
            List list;
            HTTPBuilder httpBuilder = new HTTPBuilder();
            httpBuilder.request(this.serviceURL, GET, JSON) { req ->
                uri.path = pathInput;
                uri.query = queryJson;
                requestContentType = JSON;
                headers.Accept = 'application/json;charset=utf-8';
                response.success = { resp, json ->
                    list = json;
                }
            }
            return list;
        }

        public void delete(String pathInput) {
            HTTPBuilder httpBuilder = new HTTPBuilder();
            httpBuilder.request(this.serviceURL, DELETE, JSON) { req ->
                uri.path = pathInput;
                requestContentType = JSON;
                headers.Accept = 'application/json;charset=utf-8';
                response.success = { resp, json ->
                    println "Deleted: " + pathInput;
                }
            }
        }

        public String getId(String pathInput, LinkedHashMap queryJson) {
            HTTPBuilder httpBuilder = new HTTPBuilder();
            httpBuilder.request(this.serviceURL, GET, JSON) { req ->
                uri.path = pathInput;
                uri.query = queryJson;
                requestContentType = JSON;
                headers.Accept = 'application/json;charset=utf-8';
                response.success = { resp, json ->
                    return json.id[0];
                }
            }
        }

        public String post(String pathInput, LinkedHashMap jsonBody){
            HTTPBuilder httpBuilder = new HTTPBuilder();
            httpBuilder.request(this.serviceURL, POST, JSON) { req ->
                uri.path = pathInput;
                body = jsonBody;
                response.success = { resp, json ->
                    return json.id;
                }
            }
        }

        public String put(String pathInput, LinkedHashMap jsonBody){
            HTTPBuilder httpBuilder = new HTTPBuilder();
            httpBuilder.request(this.serviceURL, PUT, JSON) { req ->
                uri.path = pathInput;
                body = jsonBody;
                response.success = { resp, json ->
                    return json.id;
                }
            }
        }

        public LinkedHashMap convertFromReadableArrayListToJson(def sitData){
            Yaml yaml = new Yaml()
            def obj = yaml.load(sitData);
            return obj
        }

        public String modifyDateFormat(String inputDate) {
            String valueAfterChangeFormat = null;
            if (inputDate != null && inputDate != "") {
                LocalDate changedDate = LocalDate.parse(inputDate, DateTimeFormatter.ofPattern(DATE_FORMAT_USING_IN_GROOVY));
                valueAfterChangeFormat = changedDate.format(DateTimeFormatter.ofPattern(DATE_TIME_SHORT_FORMAT_CONFORM_TO_ISO_8601));
                valueAfterChangeFormat += "T00:00:00.000Z";
            }
            return valueAfterChangeFormat;
        }

        public String getServiceURL(String envServiceName, String serviceContextPath) {
            this.hostname = System.getenv(envServiceName + '_SERVICE_HOST') ?: DEFAULT_HOSTNAME;
            this.port = System.getenv(envServiceName + '_SERVICE_PORT') ?: DEFAULT_PORT;
            return extractURL(hostname, port, serviceContextPath);
        }

        public String extractURL(String hostnameParam, String portParam, String serviceContextPathParam) {
            return "http://$hostnameParam:$portParam/$serviceContextPathParam/";
        }

        public String getServiceContextPath() {
            return this.serviceContextPath;
        }
    }
    ```

    </div>

    </div>

2.  How to use the class above, just an example below:

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="8ec888d2-9437-456d-8df2-a5631b9b6396" macro-name="code" style="border-width: 1px;">

    <div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

    **remove_swissdec_contract_data.groovy**

    </div>

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    import com.axonivy.scripting.RestClient;

    luzCompensationClient = new RestClient('LUZ_COMPENSATION_SERVICE', 'luz_compensation');

    List contracts = luzCompensationClient.getList('api/contracts', [all: 'yes']);
    contracts.each {    
        luzCompensationClient.delete('api/contracts/' + it.id);
    };
    ```

    </div>

    </div>

3.  One more example for making swissdec contracts

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="5cfec331-e112-4f49-9ad9-1011a0c16304" macro-name="code" style="border-width: 1px;">

    <div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

    **swissdec_contract_data.groovy**

    </div>

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    import com.axonivy.scripting.RestClient;

    luzCompensationClient = new RestClient('LUZ_COMPENSATION_SERVICE', 'luz_compensation');
    luzPersonClient = new RestClient('LUZ_PERSON_SERVICE', 'luz_person');

    companyId = luzCompensationClient.getId('api/companies', [name: 'Muster AG']);
    sitCodes = ["1000", "1010", "1212", "1410", "1961", "3000", "3032", "5000", "9010", "5010", "5020","5030", "5040", "5050", "5046", "9020", "9021", "9022", "9023", "9030", "9031", "9040", "9050", "9052"];

    sitTemporals = [];
    sitCodes.each{
        sitObj = new LinkedHashMap();
        sitObj.put("salaryItemTypeUri", "/luz_compensation/api/salaryItemTypes/" + luzCompensationClient.getId('api/salaryItemTypes', [code: (it)]));
        sitTemporals.add(sitObj);
    }

    LinkedHashMap getContractInfo(String employeeName, LinkedHashMap overwriteContractInfo) {
        employeeNameEncoded = java.net.URLEncoder.encode(employeeName).replaceAll("\\+","%20");
        employeeId = luzCompensationClient.getId('api/employees', [name: employeeNameEncoded]); 
        contractInfo = 
            [
                "contractTemporals": [["monthlySalary": 73000.00, "bvgDeduction": -5400.00, "engagementLevel": 100,  "insuranceAllocations": [ ["insuranceAllocation": "UVG1/A1"], ["insuranceAllocation": "UVGZ1/11"], ["insuranceAllocation": "KTG1/11"] ]]],           
                "employeeUri": "/luz_compensation/api/employees/" + employeeId,
                "companyUri": "/luz_compensation/api/companies/" + companyId,
                "salaryItemTypes": sitTemporals
            ];  
        overwriteContractInfo.each {key, value ->      
            contractInfo.put(key, value);
        }
        return contractInfo;
    }

    String insertContract(String employeeName, LinkedHashMap contractInfo) {
        return luzCompensationClient.post('api/contracts', getContractInfo(employeeName, contractInfo));
    } 
     

    insertContract('Bosshard Peter', ["entryDate": "2010-07-01T00:00:00.000Z"]);
    insertContract('Aebi Anna', ["entryDate": "2013-02-01T00:00:00.000Z", "exitDate": "2013-03-27T00:00:00.000Z"]);
    insertContract('Casanova Renato', ["entryDate": "2013-02-27T00:00:00.000Z"]);
    insertContract('Degelo Lorenz', ["entryDate": "2013-02-28T00:00:00.000Z", "exitDate": "2013-03-01T00:00:00.000Z"]);
    insertContract('Duss Regula', ["entryDate": "2012-05-01T00:00:00.000Z", "exitDate": "2013-10-31T00:00:00.000Z"]);
    insertContract('Combertaldi Renato', ["entryDate": "2011-06-01T00:00:00.000Z"]);
    insertContract('Egli Anna', ["entryDate": "2012-12-01T00:00:00.000Z"]);
    insertContract('Estermann Michael', ["entryDate": "2013-01-01T00:00:00.000Z", "exitDate": "2013-12-31T00:00:00.000Z"]);
    insertContract('Farine Corinne', ["entryDate": "2011-08-01T00:00:00.000Z", "exitDate": "2013-02-28T00:00:00.000Z"]);
    insertContract('Ganz Heinz', ["entryDate": "2013-02-01T00:00:00.000Z", "exitDate": "2013-10-31T00:00:00.000Z"]);
    insertContract('Herz Monica', ["entryDate": "2012-11-01T00:00:00.000Z", "exitDate": "2013-03-31T00:00:00.000Z"]);
    insertContract('Inglese Rosa', ["entryDate": "2012-10-01T00:00:00.000Z", "exitDate": "2013-03-31T00:00:00.000Z"]);
    insertContract('Jung Claude', ["entryDate": "2013-10-01T00:00:00.000Z", "exitDate": "2013-10-31T00:00:00.000Z"]);
    insertContract('Kaiser Beat', ["entryDate": "2010-01-01T00:00:00.000Z"]);
    insertContract('Lusser Pia', ["entryDate": "2012-08-01T00:00:00.000Z"]);
    insertContract('Martin René', ["entryDate": "2013-03-01T00:00:00.000Z"]);
    insertContract('Nestler Paula', ["entryDate": "2013-01-01T00:00:00.000Z"]);
    insertContract('Nunez Maria', ["entryDate": "2013-01-01T00:00:00.000Z"]);
    insertContract('Ott Hans', ["entryDate": "2012-09-01T00:00:00.000Z"]);
    insertContract('Paganini Maria', ["entryDate": "2010-06-01T00:00:00.000Z"]);
    insertContract('Ganz Edith', ["entryDate": "2011-03-01T00:00:00.000Z", "exitDate": "2013-02-27T00:00:00.000Z"]);
    insertContract('Lamon René', ["entryDate": "2012-03-15T00:00:00.000Z"]);
    insertContract('Rieder Catia', ["entryDate": "2005-07-01T00:00:00.000Z"]);
    insertContract('Fankhauser Markus', ["entryDate": "1977-08-01T00:00:00.000Z", "exitDate": "2012-12-31T00:00:00.000Z"]);
    insertContract('Burri Heidi', ["entryDate": "1977-09-02T00:00:00.000Z", "exitDate": "2012-10-31T00:00:00.000Z"]);
    insertContract('Moser Johann', ["entryDate": "1972-03-01T00:00:00.000Z", "exitDate": "2012-12-31T00:00:00.000Z"]);
    insertContract('Zahnd Anita', ["entryDate": "1992-02-01T00:00:00.000Z", "exitDate": "2012-12-31T00:00:00.000Z"]);
    insertContract('Racine Susette', ["entryDate": "1967-04-01T00:00:00.000Z", "exitDate": "2012-12-31T00:00:00.000Z"]);
    insertContract('Perret Michelle', ["entryDate": "2010-10-01T00:00:00.000Z", "exitDate": "2012-10-31T00:00:00.000Z"]);
    insertContract('Schüpbach Ernst', ["entryDate": "2013-02-20T00:00:00.000Z", "exitDate": "2013-03-15T00:00:00.000Z"]);
    ```

    </div>

    </div>

%% ai-graph-start %%

**Related notes:**
- [[Delete company - Old way]]
- [[15. Update companies by tenant id]]
- [[14. Create companies by tenant id]]
- [[RESTful API, Postman,]]
- [[Run Script Resync hidden wiget]]

%% ai-graph-end %%