
Why a decision service?
-----------------------

You will learn in this section:


1- How to use the rules that you author on the last section.

2- How can you expose your decisions as a service.

3- How to use your automated decisions and rules.




***Understanding the deployment process ***
-----------------------------------

In previous labs we have defined the Business Object Model and the rules and decisions that operate on the model. To import all of the assets back into your space you need to import the following repository. You can watch the video on how to import a repository into your workspace

1- Import the rest of the Domain Model by importing the project Domain Model CC Dispute  from the following repository:

https://github.com/MyriamFentanes/business-policies-decisions-scenario-step3.git

Now we will deploy the rules to process a Credit Card (CC) Dispute.

**Background**

In previous sections we learned that Red Hat Process Automation Manager is a modular platform to develop and run decisions and processes. In the diagram below you can see that the component where you as a Business Domain Expert have been authoring your rules is Business Central for authoring, this is the web console with all the tooling for the different types of users. All the assets that you created are stored in a repository where they are versioned.

<img src="../../assets/middleware/rhpam-7-workshop/rhpam-7-architecture.png" width="600" />

1- Go to your project library view and select the automated-chargeback rules, 1- once the editor opens click on the button Latest Version.

<img src="../../assets/middleware/rhpam-7-workshop/business-central-chargeback-versions.png"  width="600" />

2- There is also more metadata store on the repository about your assets, like the name of the user that created the asset, the time and data when it was last modified, etc.

<img src="../../assets/middleware/rhpam-7-workshop/business-central-chargeback-versions-detail.png"  width="600" />

3- Click on any of the versions in the dropdown box and you will see the version of the asset tagged.

<img src="../../assets/middleware/rhpam-7-workshop/business-central-chargeback-version.png"  width="600" />

**Packaging**

After your project is developed, you can build the project in Business Central and deploy it to the configured Process Server, which is where the rules and processes execute. The Process Server is a component that allows things like distributed execution. If for example, the execution of the case rules will be performed at bank branches, you can deploy as many Process Servers as you need all connected to your Business Central Authoring console.


1- Return to Home and select Deploy, you will see your server configurations, a server configuration is the template or blueprint  that holds the configuration of a group of Process Servers, there are 2 things that you configure through the template:

   - Capabilities: What can you execute in your Process Server (Process, Decisions, Planner rules) they are not mutually exclusive.
   - Deployment unit: what package of assets do you want to execute.

The services in a project are published for use by sending a deployment unit to a configured Process Server. When you build and deploy a project in Business Central, the deployment unit is created automatically in the configured server. You can start, stop, or remove deployment units in Business Central as needed. You can also create additional deployment units from previously built projects and start them on existing or new Process Servers configured in Business Central.


2- Return to Home and select Design. Select your project credit-card-dispute. The Library view will open with a list of all your assets. These assets will be compiled and packaged inside a kjar or deployment unit. Click on Deploy

<img src="../../assets/middleware/rhpam-7-workshop/business-central-deploy.png"  width="600" />

You will see that the project is first built, meaning the assets are compiled and packaged and then deployed to a Execution Server Container. Go back to Home and then select Deploy you will see now a container running with your new created decisions.


**Executing**

1- Go to the Main Menu and select Deploy>Execution Services

<img src="../../assets/middleware/rhpam-7-workshop/business-central-execution-services-detail.png"  width="600" />

Click on the URL of the container and you will be able to see where your service is running as well as the configuration details.

<img src="../../assets/middleware/rhpam-7-workshop/business-central-execution-services-info.png"  width="600" />

