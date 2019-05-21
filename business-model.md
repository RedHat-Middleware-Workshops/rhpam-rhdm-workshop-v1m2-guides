
# Business Object Model

In this section you will learn

1. What is the Business Object Model.

2. How to create it inside the Business Central Web Console.

3. How to import a project into your space.

The credit card dispute process is not  straightforward, it involves different actors inside and outside of the company. These actors need to have visibility at all times into what  is happening during the processing of each dispute.


We have identified the actors involved in the overall CC Dispute process, these actors are outside of the corporation and we can think of them as  the external entities, that we will connect to, to get information, but do not participate in the dispute resolution, we will see more of that in the Case Management Scenario.

Note: To get a reminder of the actors look into the Use Case overview or step 1 of this scenario.

One of the requirements to successfully process a dispute is that all of the parties involved are aware of the dispute status at all times. Since they can all influence the final resolution of the dispute. Because different parties will have visibility and sometimes control over the process we need a domain model that clearly depicts the objects that describe the data and the actors that do interact in the resolution of the Credit Card Dispute process.

So far we can identify 3 major entities:

- Card Holder
- Fraud Data
- Additional Information

_The difference between these entities and the Actors is that this entities are the ones that contain data that is relevant to process the dispute. This data will come from the different actors and then it will be stored in process variables._

## The Business Domain Context

You as business domain expert, need to define what is the domain model for the business capability you are trying to automate. Eric Evans coined the term Domain Driven Design that holds 3 main guiding principles: Focus on the core domain; explore models in a creative collaboration of domain practitioners and software practitioners; and speak a ubiquitous language within an explicitly bounded context. You can learn more about this design approach in his book.
So the first and very important task to automate a core business capability is to create a definition of the business entities within the context of Credit Card Dispute, in this case the first entity is the Credit Card Holder, the definition in the context of our use case maybe totally different from the definition inside Pecunia Corp. but it will be common  among the team of business and technology experts.

Creating the Card Holder entity.

1. Login to business-central web console at http://rhpam7-workshop-rhpamcentr-rhpam7-workshop.[[HOST_SUBDOMAIN]]-80-[[KATACODA_HOST]].http-proxy.katacoda.com/

<img src="../../assets/middleware/rhpam-7-workshop/business-central-console.png" width="600" />

2. Select Design from the main menu, you will be redirected to your working space, this is the sandbox for you to define your projects and within those projects assets.

3. Click on Add Project to create a new Project in your space called MySpace, when the Add Project wizard opens up type in
`credit-card-dispute`{{copy}} as the name of the project, this project is your business boundary that encapsulates a business capability and then type
`Assets to Automate credit card dispute process`{{copy}} as the description of your project.

Once the creation of your project is complete you should see the following page with the project content, empty so far.

<img src="../../assets/middleware/rhpam-7-workshop/business-central-asset-empty-project.png"  width="600" />

4. In the upper right corner you will see a blue button called `Add Asset`. An asset is any Rule, Process, Decision Table, Data Object, Data Form, etc. Click on the `Add Asset` button and you will be presented with a catalog of the wizards to create assets.

<img src="../../assets/middleware/rhpam-7-workshop/business-central-asset-catalog.png" width="600" />

5. Select the wizard for _Data Object_ from the catalog to create your business object model for the Credit Card Holder, type `CardHolder` {{copy}} as the name of the object and select com.myspace.ccd_project as the Package. Click OK

<img src="../../assets/middleware/rhpam-7-workshop/business-central-CCD-object-new.png" width="600" />

6- You will see the new created object with no properties, lets click on the `+add field` button to start adding the properties to our CreditCardHolder.


<img src="../../assets/middleware/rhpam-7-workshop/business-central-CCD-object-new-empty.png" width="600" />


7- Click  in the window for New Field create a new property with the following values:

Age `age`{{copy}}

Identifier: `age`{{copy}}

Label: `Age`{{copy}}

Description: `Age of the Credit Card Holder`{{copy}}

Type: Integer

<img src="../../assets/middleware/rhpam-7-workshop/business-central-CCD-object-new-properties.png" width="600" />

The first step to automate a process or decision is to define and specify the Business Object Model, here you just created the Entity Credit Card Holder, these entities will be used to store the information that you need to make decisions.

Now you will import the rest of the Business Object Model from a predefined repository, once you had successfully imported it, examine the other entities in your project.

8- Watch the video on how to import a repository into your workspace.

[Youtube Importing a project](https://youtu.be/LUOw1TrYDWs)


9- Create the rest of the Domain Model by importing the project Domain Model CC Dispute  from the following repository:

https://github.com/MyriamFentanes/business-policies-decisions-scenario-step2.git
