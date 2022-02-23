# SOAP-REST-Client
A java based generic light-weight and product-agnostic client with exposed apis used for SOAP and RESTful based webservice API automation.

# About REST:
* REST stands for Representational State Transfer.  It relies on a stateless, client-server, cache able communications protocol -- and in virtually all cases, the HTTP protocol is used. REST is an architecture style for designing networked applications.
* RESTful applications use HTTP requests to post data (create and/or update), read data (e.g., make queries), and delete data. Thus, REST uses HTTP for all four CRUD (Create/Read/Update/Delete) operations.

# REST-Client Principles:
Using Rest-Client user can
* Execute REST Requests for static/dynamic data for all media content types
* Generating Response
* Capturing Body/Header/StatusCode/Error-Message from the Response

# REST-Client API Details :
# 1. RestUtil.java:
**a.RestUtil(String resturl, String username, String password, String contentType):**
* where resturl is the url where to we are suppose to execute the request for example http://cdp.idc.devlab.motive.com:8180/rest
* username and password is the credentials to trigger REST for example restuser/restpassword
* contentType is to support content type in REST for example TEXT_PLAIN, APPLICATION_JSON, APPLICATION_XML

**b.RestResponse executeGet(String context):**
* This method will return the rest response for get, while calling this method to step definition we just need to pass the context for example  /user/106 where user is the component and 106 is the id of the user.

**c.RestResponse executePost (String context, String requestPath):**
* This method will return the rest response for post, while calling this method to step definition we just need to pass the context and resquestPath for example /user as context and /resources/jsonSampleRequest.json as requestPath

**d.RestResponse executePost (String context, String requestPath, String[][] data)
* This method will return the rest response for post, this can be used when we want post dynamic data through user input.

**e.RestResponse executePut (String context, String requestPath)**
* This method will return the rest response for put(to update), while calling this method to step definition we just need to pass the context and resquestPath for example /user as context and /resources/jsonSampleRequest.json as requestPath

**f.RestResponse executeDelete(String context)**
* This method will return the rest response for delete, while calling this method to step definition we just need to pass the context for example  /user/106 where user is the component and 106 is the id of the user.

**g.static int generateRandomDigits(int noOfDigit)**
* This method is useful in the scenario where we want to maintain uniqueness for the specific field for example we want to create different user in our system and it should be unique, this method will generate random number with user input for example you can call this method in you step definition  as static String userName = "user"+RestUtil.generateRandomDigits(4);  so the number will be generated as user and randomly generated digits for exmple user1234 because we have provided as (4) as user input.

# 2. RestResponse.java
This java file will help to Captures Body/Header/StatusCode/Error-Message from the generated Response

**Sample Test**
![image](https://user-images.githubusercontent.com/17194046/155342235-aff58e21-955d-43c9-a437-ebe36db5aa8f.png)

----

# SOAP-Client Concept :
Web services can be implemented using any programming language on any platform, provided that a standardized XML interface description called Web Services Description Language (WSDL) is available and a standardized messaging protocol called Simple Object Access Protocol (SOAP).
**SOAP:** 
SOAP is a standard protocol defined by the W3C Standard for sending and receiving web service requests and responses.Also uses the XML format to send and receive the request and hence the data is platform independent data.SOAP messages are exchanged between the provider applications and receiving application within the SOAP envelops.
**WSDL:** 
WSDL (Web Services Description Language) is an XML based language which will be used to describe the services offered by a web service.Also describes all the operations offered by the particular web service in the XML format. It may defines how the services can be called, i.e what input value we have to provide and what will be the format of the response it is going to generate for each kind of service.

# SOAP-Client Principles:
A lightweight SOAP-Interface which is capable for :
* Executing SOAP/XML Requests for static/dynamic data
* Generating Response
* Capturing Body/Header/StatusCode/Error-Message from the SOAP-Response
* Response validation data based on XPaths for XML Tags
* Supports WSSE-Password Type : PasswordText/PasswordDigest
* Handles both http and https of EndPoint url

# SOAP-Client API Details :
# A.SoapUtil.java:
A light-weight soap-interface with exposed methods which execute requests and generate response.
**Initialize:**
SoapUtil(String endpoint, String username, String password, String contentType)
Where :
* endpoint : End-point URL of web-service excluding '?WSDL' (ex. http://calum.idc.devlab.motive.com:8010/scope/ScopeNBIService)
* username/password : Credentials to trigger web-service(ex. nbi_user/password)
* contentType : Supported Content-Type for Web-service like: text/xml; charset=UTF-8 application/xml application/json
Please refer 'TestSoapRequest.java' as in 'Sample Script:' section.
# B.SoapResponse.java :
* Captures Body/Header/StatusCode/Error-Message from the generated Response
* Captures Response data based on XPaths for XML Tags
![image](https://user-images.githubusercontent.com/17194046/155344423-6cf0c53a-9f83-4e7d-aa89-6402fe9352b4.png)

# Sample Script:
```java
import org.apache.log4j.Logger;
 // Sample program for SOAP-webervice
public class TestSoap {
    //User Input For Femto :
    public static final String ENDPOINT = "http://calum.idc.devlab.motive.com:8010/scope/ScopeNBIService";
    public static final String USERNAME = "webserviceuser";
    public static final String PASSWORD = "webserviceuser1";
    public static String CONTENTTYPE = "text/xml; charset=UTF-8"; 
    private static String requestFile = "./TestSoapRequest/addSmallCellBasic.xml";
  
    //User Inputs for HDM with WSSE-Password-Type :
    public static final String ENDPOINT1 = "http://figo.idc.devlab.motive.com:7003/NBIServiceImpl/NBIService";
    public static final String USERNAME1 = "nbi_user";
    public static final String PASSWORD1 = "password";
    public static final String PASSWORDTYPE1 = "PasswordText";
    public static String CONTENTTYPE1 = "application/soap+xml;charset=UTF-8";
     
    //Request-XML file
    private static String requestFile1 = "./TestSoapRequest/findDeviceByGUID.xml";
     
    //Xpath to capture response data
    private static String XPath = "//dynamicVariables[1]/name";
  
    //Input-Data as Key/Value pair
    private static String[][] inputData =
            {
                {"name", "TestCustomer9007"},
                {"customerId", "9007"},
                {"oui", "00BEEF"},
                {"serialNumber", "Femto9007"},
                {"smallCellId", "9007"}
            };
  
    private static Logger log = Logger.getLogger(TestSoap.class.getName());
    public TestSoap() { }
 
    //Start Execution
    public static void main(String[] args) throws Exception {
        //Load log-config
        LogUtil.loadConfig();
 
        //Initialize Soap Util for Femto
        SoapUtil soapUtil1 = new SoapUtil(ENDPOINT, USERNAME, PASSWORD, CONTENTTYPE);
 
        //Execute Femto-Request and Handle Response
        SoapResponse response = soapUtil1.executeRequest(requestFile);
        log.debug("Response-Body:"+response.getBody()); //Capture HTTP-Body
        log.debug("Response-statusCode:"+response.getstatusCode()); //Capture HTTP-StatusCode
 
        //Capture Error-Message
        if(response.getstatusCode() != 200){
            log.debug("Error Message:"+response.getErrorMessage());
        }
  
        //Initialize Soap Util for HDM with WSS-PasswordType
        SoapUtil soapUtil2 = new SoapUtil(ENDPOINT1, USERNAME1, PASSWORD1, PASSWORDTYPE1, CONTENTTYPE1);
        SoapResponse response1 = soapUtil2.executeRequest(requestFile);
         
        //Execute HDM-Request and Handle Response for Dynamic input
        SoapResponse response2 = soapUtil2.executeRequest(requestFile, inputData);
         
        log.debug("Response-Body:"+response2.getResponseBody());
        log.debug("Response-statuscode:"+response2.getstatusCode());
         
        String dynamicVariable = response2.getValue(XPath)
        log.debug("Falult-Message:" +dynamicVariable);
  
    }
}
```



