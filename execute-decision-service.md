# Calling your Decision Service

In the previous step we've interacted with the Decision Service via a web-application. In this step we'll have a closer look at the RESTful API used in the communication between the web-application and the Decision Service. We will be using the Swagger API documentation for this.

Swagger provides a standard way to describe and document RESTful APIs. The RESTful API of the PAM execution server allows other platforms, for example rich browser applications, or other middleware systems, to easily communicate with the execution server using open standards.

To access the Swagger page of the execution server, navigate to: http://rhpam7-workshop-kieserver-rhpam7-workshop.[[HOST_SUBDOMAIN]]-80-[[KATACODA_HOST]].http-proxy.katacoda.com/docs

You will see the following page.

<img src="../../assets/middleware/rhpam-7-workshop/kie-server-swagger.png"  width="600" />

In this page, navigate to the section that says **Rules evaluation :: BRM**, and click on the green bar that says *POST /server/containers/instances/{id}
Evaluates rules by executing commands on the rule session*. This will show the API description of this RESTful operation. Click on the *Try it out* button at the right of the panel, this will allow you to enter the values of the request. First we change the *Parameter content type* and the *Response content type* from `application/xml` to `application/json`:

<img src="../../assets/middleware/rhpam-7-workshop/swagger-application-json.png"  width="600" />

This specifies the dataformat that we will be using for our request and response. In this case this is the JSON format.

Next we need to specify the container-id that contains the deployment of the rules that we want to evaluate. The name of our container is `ccd-project_1.0.0`{{copy}}.

Finally, we provide the body of the request. In the body we pass the data, based on our domain model or business model, om which we evaluate the rules. Paste the following request body into the *body* text-area in the panel:

```
{  
   "lookup":null,
   "commands":[  
      {  
         "insert":{  
            "object":{  
               "com.myspace.ccd_project.CreditCardHolder":{  
                  "age":40,
                  "status":"Gold"
               }
            },
            "out-identifier":"ccholder",
            "return-object":true,
            "entry-point":"DEFAULT",
            "disconnected":false
         }
      },
      {  
         "insert":{  
            "object":{  
               "com.myspace.ccd_project.FraudData":{  
                  "totalFraudAmount":1000.0
               }
            },
            "out-identifier":"frauddata",
            "return-object":true,
            "entry-point":"DEFAULT",
            "disconnected":false
         }
      },
      {  
         "fire-all-rules":{  
            "max":-1,
            "out-identifier":null
         }
      }
   ]
}
```

We can see that we pass in the `FraudData`, with a `totalFraudAmount` of 1000.0. We also pass in the `CreditCardHolder` with a *Gold* status.

When we've entered our data, click on the blue *Execute* button to fire the request. When the browser ask for a username and password, use username: `developer`{{copy}} and password: `developer`{{copy}}:

<img src="../../assets/middleware/rhpam-7-workshop/swagger-request.png"  width="600" />

If all goes well, the decision service will reply with the following response:

<img src="../../assets/middleware/rhpam-7-workshop/swagger-response.png"  width="600" />

Note that the rules have qualified this data for *automatic processing* and the risk has been set to *2*:

This concludes the testing of our service. Feel free to test the service with other values for the `CreditCardHolder` and `FraudData`.
