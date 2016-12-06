---
title: "Formularios en API Connect 5"
categories:
  - APIm
tags:
  - APIm
  - IBM
  - APIConnect
  - x-www-form-urlencoded
excerpt: "Procesado de formularios x-www-form-urlencoded en API Connect 5"
header:
  teaser: /assets/images/apiconnect_logo.png
---

Llevaba tiempo probando, y fracasando en el intento, a poder acceder a los datos de un formulario (x-www-form-urlencoded) desde API Connect.
Aunque este [mensaje](https://developer.ibm.com/answers/questions/165470/ibm-api-management-v31-http-post.html) es muy antiguo y de la versión 3, daba a entender que sólo se permitía trabajar en el ensamblado de un API con JSON o XML, y que otros tipos de mensaje se podían pasar con una política proxy a un *backend*, pero no se podían manipular. La verdad es que las capacidades de mediación de API Connect siempre han sido bastante pobres.

> For assembly resources the request (and response) bodies of the requests must be in XML or JSON format - we don't currently support plain text or form-urlencoded data structures.
It is supported to pass non-XML or non-JSON structures in Proxy resources which you can do by omitting the definition of the structures in the request/response examples, however that does require that you want to do a straight pass-through of the call rather than adjusting the call as you would do in an assembly resources. (Matt Roberts)

El caso es que ni en la versión 4 ni en la 5 lo había logrado, pero al final he encontrado este [ejemplo](https://github.com/IBM-Bluemix/openwhisk-slackapp/blob/master/api/definitions/openwhisk-slackapp-api.yaml) operativo, que he comprobado que funciona en la versión 5.0.5 desplegada ahora mismo en Bluemix~~. Me queda por comprobar el comportamiento en la versión 5.0.4 en la que realicé las últimas pruebas, pero estoy casi seguro de que la variable "request.body" llegaba vacía cuando se invocaba al API con un formulario~~ y en la 5.0.4.

Por si queréis probar, os paso aquí los ficheros necesarios:
* YAML con un API de prueba, con *basepath* 'testform' y una operación POST con *path* 'prueba' (si, para ser purista, debería haber declarado el body como formdata en el swagger)
{% highlight YAML %} 
---
swagger: "2.0"
info:
  x-ibm-name: "testform"
  title: "testForm"
  version: "1.0.0"
schemes:
- "https"
host: "$(catalog.host)"
basePath: "/testform"
consumes:
- "application/json"
produces:
- "application/json"
securityDefinitions:
  clientIdHeader:
    type: "apiKey"
    in: "header"
    name: "X-IBM-Client-Id"
security:
- clientIdHeader: []
x-ibm-configuration:
  testable: true
  enforced: true
  cors:
    enabled: true
  assembly:
    execute:
    - gatewayscript:
        title: "gatewayscript"
        source: "// for application/x-www-form-urlencoded we need to decode the form\
          \ body \nvar requestBody = apim.getvariable(\"request.body\");\nvar blob\
          \ = requestBody.item(0);\nvar bodyAsString = blob.toBuffer().toString();\n\
          // this will be the message.body we will set\nvar formDataAsJSON = {};\n\
          var keyValues = bodyAsString.split(\"&\");\nfor (var index = 0, length =\
          \ keyValues.length; index < length; index++) {\n    var property = keyValues[index].split(\"\
          =\");\n    formDataAsJSON[property[0]] = decodeURIComponent(property[1].replace(\"\
          +\", \"%20\"));\n}\napim.setvariable(\"message.headers.Content-Type\", \"\
          application/json\");\napim.setvariable(\"message.body\", formDataAsJSON);"
  gateway: "datapower-gateway"
  phase: "realized"
paths:
  /prueba:
    post:
      responses:
        200:
          description: "200 OK"
definitions: {}
tags: []
{% endhighlight %} 
* Producto de prueba incluyendo el API anterior
{% highlight YAML %}
---
product: "1.0.0"
info:
  name: "producttestform"
  title: "productTestForm"
  version: "1.0.0"
visibility:
  view:
    enabled: true
    type: "public"
    tags: []
    orgs: []
  subscribe:
    enabled: true
    type: "authenticated"
    tags: []
    orgs: []
apis:
  testform:
    $ref: "testform_1.0.0.yaml"
plans:
  Default:
    title: "Default Plan"
    apis:
      testform: {}
{% endhighlight %} 
* Colección postman con invocación al API anterior pasando un formulario sencillo con 2 campos (a y b). Tendrás que crear variables con tu client-id, el *host* del *gateway* y tu organización y catálogo
{% highlight JSON %} 
{
    "id": "c7a7da77-c75e-10c3-9098-abd5bad40d67",
    "name": "test API Connect",
    "timestamp": 1480892066809,
    "requests": [{
        "collectionId": "c7a7da77-c75e-10c3-9098-abd5bad40d67",
        "id": "88ba1446-2490-6d2f-ab12-b91d3b1adb32",
        "name": "test FORM",
        "description": "",
        "url": "https://{{gwHost}}/{{org}}/{{catalog}}/testform/prueba",
        "method": "POST",
        "headers": "x-ibm-client-id: {{client-id}}\n",
        "data": [{
            "key": "a",
            "value": "1",
            "type": "text"
        }, {
            "key": "b",
            "value": "2",
            "type": "text"
        }],
        "dataMode": "urlencoded",
        "timestamp": 0,
        "version": 2,
        "time": 1480892725384
    }]
}
{% endhighlight %} 
* Aunque va incluido en el API, pongo aquí el código del GatewayScript que accede a los datos del formulario, para presentarlo de forma identada
{% highlight javascript %}
// for application/x-www-form-urlencoded we need to decode the form body 
var requestBody = apim.getvariable("request.body");
var blob = requestBody.item(0);
var bodyAsString = blob.toBuffer().toString();
// this will be the message.body we will set
var formDataAsJSON = {};
var keyValues = bodyAsString.split("&");
for (var index = 0, length = keyValues.length; index < length; index++) {
    var property = keyValues[index].split("=");
    formDataAsJSON[property[0]] = decodeURIComponent(property[1].replace("+", "%20"));
}
apim.setvariable("message.headers.Content-Type", "application/json");
apim.setvariable("message.body", formDataAsJSON);
{% endhighlight %} 