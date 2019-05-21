# Test your Decision Service

Now that you've created your Decision Service, and have deployed it on the Red Hat Process Automation Manager execution server, it's time to test the service. There are various ways in which you can test a Decision Service. Common practice is to write test-cases for individual rules, groups of rules, and your entire service. These test-cases can then be automatically executed when the code and rules of your service are compiled and packaged. This provides the guarantee that your service is tested properly before it's deployed into a production environment. It ensures that the logic that is executed and the decision that are made are correct and according to specification. Red Hat Process Automation Manager provides full support for these kind of testing scenarios.

In this step of our scenario however, we will simply test our service via a simple web-application that we've provided for you. The application allows you to enter the data of the credit-card holder, and the data of the line item. The data is submitted to the Decision Server, which will calculate the risk of the transaction and determine whether the data can be automatically processed.

<img src="../../assets/middleware/rhpam-7-workshop/reactjs-app.png"  width="600" />

Access the application at: http://react-web-app-rhpam7-workshop.[[HOST_SUBDOMAIN]]-80-[[KATACODA_HOST]].http-proxy.katacoda.com/docs

Enter the following details:
- Name: Jim
- Age: 52
- Status: Gold
- Description: Delta Airlines
- Amount: 1000

Next, click on the _Submit_ button. The application will send a RESTful request to the Decision Server. If everything is working correctly,m the Decision Server will send a result that will be displayed in the application:

<img src="../../assets/middleware/rhpam-7-workshop/reactjs-app-request-response.png"  width="600" />

We can see that the `riskRating` has been set to **2** and the transaction is eligible for automated processing. Feel free to test your Decision Service with different values to see if all the use-cases you've implemented in your rules are covered.

We have now successfully tested our decision service. In the next step we will take a closer look at the RESTful API exposed by the service, and we will see how we can test the services via the API documentation page.
