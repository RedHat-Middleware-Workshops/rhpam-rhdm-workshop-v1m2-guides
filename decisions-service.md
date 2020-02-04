
# Decision Services

In this section you will learn:

1. How to use the rules that you authored in the previous step.
2. How can you expose your decisions as a service.
3. How to use your automated decisions and rules.


## Understanding the deployment process

In previous labs we have defined the Business Object Model and the rules and decisions that operate on the model. If you've completed the labs in the previous steps, you can use your existing project.

If you'd prefer to start off fresh you can delete your project & re-import it following these steps:

1. Delete the current project

    1. At the top of the screen under the main heading, click the _ccd-project_ to bring you back to the homepage for the project

    ![Business Central Breadcrumb bar ccd project]({% image_path business-central-breadcrumb-bar-ccd-project.png %}){:width="600px"}

    2. Delete the project by clicking the hamburger menu & selecting _Delete Project_

    ![Business Central Delete CCD Project]({% image_path business-central-delete-ccd-project.png %}){:width="600px"}

    3. Type in _ccd-project_ and click `Delete Project`
    4. If asked you can `Discard unsaved changed and proceed`

2. Import the project
    1. Click the `Import Project` button
    2. Enter [https://github.com/RedHat-Middleware-Workshops/rhpam-rhdm-workshop-v1m2-labs-step-3.git](https://github.com/RedHat-Middleware-Workshops/rhpam-rhdm-workshop-v1m2-labs-step-3.git) as the _Repository URL_ and click `Import`
    3. On the _Import Projects_ screen, select the _ccd-project_ and click `Ok`

    ![Business Central Delete CCD Project]({% image_path business-central-import-ccd-project.png %}){:width="600px"}

We will now deploy the rules on to the Execution Server to process a Credit Card (CC) Dispute.

## Background

In previous sections we've learned that Red Hat Process Automation Manager is a modular platform to develop and run decisions and processes. In the diagram below you can see that the component in which you, as a Business Domain Expert, have been authoring your rules is the Business Central Workbench. This is the web-based workbench that provides the tooling for the different types of users. All the assets that you create are stored in a repository (a Git-based version control system) where they are versioned.

![RHPAM 7 Architecture]({% image_path rhpam-7-architecture.png %}){:width="600px"}

1. Go to your project library view and select the automated-chargeback rule. Once the editor opens click on the button Latest Version. (**NOTE:** If you re-imported the project then there is probably only 1 version listed)

![Business Central Chargeback Versions]({% image_path business-central-chargeback-versions.png %}){:width="600px"}

2- There is also more metadata about your assets stored in the repository, like the name of the user that created the asset, the time and data when it was last modified, etc.

![Business Central Chargeback Versions Detail]({% image_path business-central-chargeback-versions-detail.png %}){:width="600px"}

3- Click on any of the versions in the dropdown box and you will see the version of the asset tagged.

![Business Central Chargeback Version]({% image_path business-central-chargeback-version.png %}){:width="600px"}

## Packaging

After your project is developed, you can build the project in Business Central and deploy it to the configured Process Server, which is where the rules and processes execute. The Process Server is a component that allows things like distributed execution. If for example, the execution of the case rules will be performed at bank branches, you can deploy as many Process Servers as you need all connected to your Business Central Authoring console.


1. Return to the Home screen of the Business Central workbench (by clicking on the _Home_ icon in the upper left screen). Click on _Deploy_. This will open the _Server Configurations_ perspective. A server configuration is the template, or blueprint, that defines the configuration of a group of Execution Servers (a group can contain zero or more execution servers). There are 2 things that you can configure through the template:

    - Capabilities: What can you execute in your Process Server (Process, Decisions, Planner rules). They are not mutually exclusive. I.e. an execution server can be enabled with zero or more of these capabilities enabled.
    - Deployment Unit: what package of assets (project, Knowledge JAR) you want to deploy on the server to make available for execution.

    The services in a project are published by sending a deployment request for a specific _Deployment Unit_ to a configured Execution Server. When you _Build and Deploy_ a project in Business Central, the _Deployment Unit_ (KJAR) is created and pushed to the asset repository (Maven). After this, a deployment request is sent to the Execution Server. The Execution Server will fetch the _Deployment Unit_ . You can start, stop, or remove deployment units in Business Central as needed. You can also create additional _Deployment Units_ from previously built projects and start them on existing or new Process Servers configured in Business Central.

2. Return to the Home screen and select _Design_. Select your Credit Card Dispute project (`ccd-project`). The Library view will open with a list of all your assets. These assets will be compiled and packaged inside a _KJAR_ or _Deployment Unit_. Click on the _Deploy_ button in the top right corner.

    ![Business Central Deploy]({% image_path business-central-deploy.png %}){:width="600px"}

You will see that the project is first built, meaning the assets are compiled and packaged, and then deployed to a Execution Server container. Go back to the Home screen and elect Deploy. You will now see a container running with your newly created decisions.


## Execution

Go to the Main Menu and select Deploy>Execution Services

![Business Central Execution Services Detail]({% image_path business-central-execution-services-detail.png %}){:width="600px"}

Click on the URL of the container and you will be able to see where your service is running as well as the configuration details. You may also be prompted for credentials. Use the same credentials you used to log into the Business Central console.

![Business Central Execution Services Info]({% image_path business-central-execution-services-info.png %}){:width="600px"}
