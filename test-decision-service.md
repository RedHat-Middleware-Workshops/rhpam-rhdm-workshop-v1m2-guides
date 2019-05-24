# Test your Decision Service

Now that you've created your Decision Service, and have deployed it on the Red Hat Process Automation Manager execution server, it's time to test the service. There are various ways in which you can test a Decision Service. Common practice is to write test-cases for individual rules, groups of rules, and your entire service. These test-cases can then be automatically executed when the code and rules of your service are compiled and packaged. This provides the guarantee that your service is tested properly before it's deployed into a production environment. It ensures that the logic that is executed and the decision that are made are correct and according to specification. Red Hat Process Automation Manager provides full support for these kind of testing scenarios.

Red Hat Process Automation Manager contains a sophisticated _Test Scenario_, in which you can build various test scenarios for your rules.

## Test Scenario


1. Open the project's Asset Library view. Click on the _Add Asset_ button. Select _Decision_ in the asset filter in the upper left of the screen, this will filter out the decision assets.
2. Select the `Test Scenario` tile. Give the scenario the name `risk-evaluation-tests` and set the package to `com.myspace.ccd_project`. Set the _Source Type_ to `Rule`.

    ![Test Scenario Create]({% image_path test-scenario-create.png %}){:width="600px"}

3. The scenario testing tool uses the concept in which _Given_ a specific set of input data, we _Expect_ a certain result. To implement our test scenario we therefore have to specify the input of our rules (_Given_) and the results we expect our rules to produce for the given input data.

4. In the editor, click on the _Data Objects_ tab. If everything is correct, there should be 3 data types listed: `AdditionalInformation`, `CreditCardHolder` and `FraudData`.

5. Go back to the _Model_ tab. To test our rules, we need to provide the input data and the expected output. Our decision table operates on 2 datatypes, `CreditCardHolder` and `FraudData`. We therefore first need to create a column for our `CreditCardHolder` in the _Given_ part of the scenario testing table. Click on the _INSTANCE 1_ field in the table. On the right-hand-side of the editor, expand the _Data Object_ `CreditCardHolder`, select the `status` field and click on the _Add_ button.

    ![Test Scenario Add Given CCH Status]({% image_path test-scenario-add-given-cch-status.png %}){:width="600px"}

    The column in the _Given_ section will be configured to represent that `status` field of the `CreditCardHolder` object.

6. To add an additional column in the _Given_ section of the table, right-click on the _Given_ column and select `Insert column right`.

    ![Test Scenario Given Insert Column Right]({% image_path test-scenario-given-insert-column-right.png %}){:width="600px"}

7.


## Test via Web Application

In the previous exercise, we used the _Test Scenario_ tooling to test the rules in our Credit Card Dispute project. Now we will test the deployed service via the REST API it exposes. We will test our service via a simple web-application that we've provided for you. The application allows you to enter the data of the credit-card holder, and the data of the line item. The data is submitted to the Decision Server, which will calculate the risk of the transaction and determine whether the data can be automatically processed.

![ReactJS App]({% image_path reactjs-app.png %}){:width="600px"}

Access the application at: http://react-web-app-rhpam7-workshop.[[HOST_SUBDOMAIN]]-80-[[KATACODA_HOST]].http-proxy.katacoda.com/docs

Enter the following details:
- Name: Jim
- Age: 52
- Status: Gold
- Description: Delta Airlines
- Amount: 1000

Next, click on the _Submit_ button. The application will send a RESTful request to the Decision Server. If everything is working correctly,m the Decision Server will send a result that will be displayed in the application:

![ReactJS App Request Response]({% image_path reactjs-app-request-response.png %}){:width="600px"}

We can see that the `riskRating` has been set to **2** and the transaction is eligible for automated processing. Feel free to test your Decision Service with different values to see if all the use-cases you've implemented in your rules are covered.

We have now successfully tested our decision service. In the next step we will take a closer look at the RESTful API exposed by the service, and we will see how we can test the services via the API documentation page.
