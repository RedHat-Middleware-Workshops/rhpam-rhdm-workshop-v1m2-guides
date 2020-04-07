
# Decision Services

In this section you will learn:

1. How to use the rules that you authored in the previous step;
2. How can you expose your decisions as a service;
3. How to use your automated decisions and rules.

In previous labs we have defined the Business Object Model and the rules and decisions that operate on the model. If you've completed the labs in the previous steps, you can use your existing project.

NOTE: _If you'd prefer to start off fresh you can delete your `ccd-project` project and re-import it using this URL: [https://github.com/RedHat-Middleware-Workshops/rhpam-rhdm-workshop-v1m2-labs-step-3.git](https://github.com/RedHat-Middleware-Workshops/rhpam-rhdm-workshop-v1m2-labs-step-3.git)._

Let's start by having an overview of assets versioning with Business Central.

## Assets Versioning

As you can notice on the diagram below, Red Hat Process Automation Manager is a modular platform to develop and run decisions and processes. Until now we have mostly used **Business Central** workbench to develop rules and business models. All the assets that you create or alter using Business Central are versioned in a repository (a Git-based version control system).

![RHPAM 7 Architecture]({% image_path rhpam-7-architecture.png %}){:width="600px"}

Business Central automatically versions our code within git-based repository, and it also provides a way for us to check the changes and rollback to previous versions necessary.

1. Go to your project library view and select the automated-chargeback rule. Once the editor opens click on the button Latest Version. (**NOTE:** If you re-imported the project then there is probably only 1 version listed).

  ![Business Central Chargeback Versions]({% image_path business-central-chargeback-versions.png %}){:width="600px"}

2. There is also more metadata about your assets stored in the repository, like the name of the user that created the asset, the time and data when it was last modified, etc.

  ![Business Central Chargeback Versions Detail]({% image_path business-central-chargeback-versions-detail.png %}){:width="600px"}

3. Click on any of the versions in the dropdown box and you will see the version of the asset tagged.

  ![Business Central Chargeback Version]({% image_path business-central-chargeback-version.png %}){:width="600px"}

## Understanding the deployment process

After your project is developed, you can build the project in Business Central and deploy it to a configured Process Server. The Process Server is the engine that executes rules and processes. It also allows distributed execution so if, for example, the execution of the case rules will be performed at bank branches, you can deploy as many Process Servers as you need all connected to your Business Central Authoring console.

The Process Server supports the capabilities configured in its server configuration. A server configuration is the template that defines the configuration of a group of Execution Servers (a group can contain zero or more execution servers).

There are 2 things that you can configure through the template:

  - Capabilities: What can you execute in your Process Server (Process, Decisions, Planner rules). They are not mutually exclusive. I.e. an execution server can be enabled with zero or more of these capabilities enabled.
  - Deployment Unit: what package of assets (project, Knowledge JAR) you want to deploy on the server to make available for execution.


Let's have a glance of what happens under the covers during a deployment in a managed Process Server:

1. When you _Build and Deploy_ a project in Business Central, the _Deployment Unit_ (KJAR) is created and pushed to the artifact repository (Maven);
2. After this, a deployment request is sent to the Execution Server;
3. The managed Execution Server receives a deployment request for a specific _Deployment Unit_;
4. The Execution Server fetches the _Deployment Unit_ and tries to initialize it.

Once this tasks are completed, you can start, stop, or remove deployment units using Business Central as needed. You can also create additional _Deployment Units_ from previously built projects and start them on existing or new Process Servers configured in Business Central.

## Deploying your project

Let's check your Process Server using Business Central.

1. Return to the Home screen of the Business Central workbench (by clicking on the _Home_ icon in the upper left screen).

2. Click on _Deploy_. This will open the _Server Configurations_ perspective. Notice which capabilities you have enabled for your Process Server.

![Business Central Process Server Server Configurations]({% image_path business-central-server-configuration.png %}){:width="600px"}

2. Return to the Home screen and select _Design_. Select `MySpace`, next, select your Credit Card Dispute project (`ccd-project`). The Library view should open with a list of all your assets. These assets will be compiled and packaged inside a _KJAR_, a _Deployment Unit_.

3. Click on the _Deploy_ button in the top right corner.

    ![Business Central Deploy]({% image_path business-central-deploy.png %}){:width="600px"}

You will see that the project is first built, meaning the assets are compiled and packaged, and then deployed to a Execution Server container. Go back to the Home screen and select Deploy. You will now see a container running with your newly created decisions.

## Execution

Let's check if the service you deployed is available.

1. Go to the Main Menu and select `Deploy`>`Execution Services`.

2. Click on the URL of the container, and a new tab should open:

  ![Business Central Execution Services Detail]({% image_path business-central-execution-services-detail.png %}){:width="600px"}

3. You may also be prompted for credentials. Use the same credentials you used to log into the Business Central console.
  - user: `pamAdmin`
  - password: `redhatpam1!`

![Business Central Execution Services Info]({% image_path business-central-execution-services-info.png %}){:width="600px"}

4. Notice the Process Service responds with details about the `Kie Container` where your `Deployment Unit` is running.

Congratulations! Now that you have deployed your first business application within the engine, let's learn how about how to automate tests of the rules you created in the Credit Card Dispute project.
