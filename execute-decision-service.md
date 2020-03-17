# Invoking your Decision Service

In the previous step we've interacted with the Decision Service via a web-application. In this step we'll have a closer look at the RESTful API used in the communication between the web-application and the Decision Service. We will be using the Swagger API documentation for this.

Swagger provides a standard way to describe and document RESTful APIs. The RESTful API of the PAM execution server allows other platforms, other applications, to easily communicate with the execution server using open standards.

To access the Swagger page of the execution server, we first need to get the URL for the execution server.

1. In the OpenShift console, open the `Topology` view of the `rhpam-userX` project. Click on the `rhpam7-kieserver` to open the engine in another tab.

    ![Execution Server Route]({% image_path kie-server-route.png %}){:width="600px"}

2. A new browser tab should open. Append to the end of the URL, `/docs`. TThe full URL will look soomething like http://insecure-rhpam7-kieserver-rhpam-user1.apps.cluster-rio-d6c5.rio-d6c5.example.opentlc.com/docs/. You will see the following page

![KIE Server Swagger]({% image_path kie-server-swagger.png %}){:width="600px"}

3. In this page, navigate to the section that says **KIE session assets**, and click on the green bar that says *POST /server/containers/instances/{containerId} Executes one or more runtime commands*. This will show the API description of this RESTful operation.

4. Click on the *Try it out* button at the right of the panel, this will allow you to enter the values of the request.

5. First we change the *Parameter content type* and the *Response content type* from `application/xml` to `application/json`. This specifies the data format that we will be using for our request and response. In this case this is the JSON format.

![Swaggger Application JSON]({% image_path swagger-application-json.png %}){:width="600px"}

6. Next we need to specify the container-id that contains the deployment of the rules that we want to evaluate. The name of our container is `ccd-project_1.0.0-SNAPSHOT`{{copy}}.

7. Finally, we provide the body of the request. In the body we pass the data, based on our domain model or business model, on which we evaluate the rules. Paste the following request body into the *body* text-area in the panel:

```
{  
   "lookup":"ccd-ksession-stateless",
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

8. After inputing the data above, click on the blue *Execute* button to fire the request.
_If the browser asks for a username and password, use the same username/password you used to log into Business Central_

![Swaggger Request]({% image_path swagger-request.png %}){:width="600px"}

If all goes well, the decision service will reply with the following response:

![Swaggger Response]({% image_path swagger-response.png %}){:width="600px"}

Note that the rules have qualified this data for *automatic processing* and the risk has been set to *1*:

Feel free to test the service with other values for the `CreditCardHolder` and `FraudData` and check the output.

This concludes the testing of our service using REST API's to interact directly with the engine.
