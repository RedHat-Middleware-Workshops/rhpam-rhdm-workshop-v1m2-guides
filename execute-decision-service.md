# Calling your Decision Service

In the previous step we've interacted with the Decision Service via a web-application. In this step we'll have a closer look at the RESTful API used in the communication between the web-application and the Decision Service. We will be using the Swagger API documentation for this.

Swagger provides a standard way to describe and document RESTful APIs. The RESTful API of the PAM execution server allows other platforms, for example rich browser applications, or other middleware systems, to easily communicate with the execution server using open standards.

To access the Swagger page of the execution server, we first need to get the URL for the execution server.

1. In the Business Central console, click on the home icon at the top.

2. Click on _Deploy_ and click on the execution server

    ![Execution Servers Home]({% image_path business-central-execution-server.png %}){:width="600px"}

3. Copy just the hostname part of the execution server URL

    ![Execution Server Hostname]({% image_path business-central-execution-server-url.png %}){:width="600px"}

4. Open a new browser tab, paste in the hostname from step 3 and append the path `/docs`. In the above screenshot the full URL would be http://rhpam7-kieserver-rhpam-user4.apps.anywhere-98a1.openshiftworkshop.com:80/docs. You will see the following page

![KIE Server Swagger]({% image_path kie-server-swagger.png %}){:width="600px"}

In this page, navigate to the section that says **KIE session assets**, and click on the green bar that says *POST /server/containers/instances/{containerId} Executes one or more runtime commands*. This will show the API description of this RESTful operation. Click on the *Try it out* button at the right of the panel, this will allow you to enter the values of the request. First we change the *Parameter content type* and the *Response content type* from `application/xml` to `application/json`:

![Swaggger Application JSON]({% image_path swagger-application-json.png %}){:width="600px"}

This specifies the dataformat that we will be using for our request and response. In this case this is the JSON format.

Next we need to specify the container-id that contains the deployment of the rules that we want to evaluate. The name of our container is `ccd-project_1.0.0-SNAPSHOT`{{copy}}.

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

When we've entered our data, click on the blue *Execute* button to fire the request. If the browser asks for a username and password, use the same username/password you used to log into Business Central

![Swaggger Request]({% image_path swagger-request.png %}){:width="600px"}

If all goes well, the decision service will reply with the following response:

![Swaggger Response]({% image_path swagger-response.png %}){:width="600px"}

Note that the rules have qualified this data for *automatic processing* and the risk has been set to *2*:

This concludes the testing of our service. Feel free to test the service with other values for the `CreditCardHolder` and `FraudData`.
