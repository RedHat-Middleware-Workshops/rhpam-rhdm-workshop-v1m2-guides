# Introduction

In this scenario you will be introduced to decisions and rules, why these concepts and methodologies are important, and what the industry standards are in this area.


**Topics covered in this module:**

- Importing projects in business central;
- Business Object Models;
- Rules: DRL Language, DMN Notation, Decision Tables;
- Testing rules;
- Deploying your first project;
- Exposing and consuming rules via REST API;

More information can be found at in our [documentation](http://docs.redhat.com).

# Use Case Review

The Credit Card Dispute process is not straightforward. It involves different actors inside, and outside of, the company. These actors need to have visibility and control at all times into what is happening during the processing of each dispute.

We have identified the actors involved in the overall CC Dispute process. These actors are outside of the corporation and we can think of them as external entities that we will connect with to get information, but that do not participate in the dispute resolution process. We will see more of that in the Case Management Scenario.

---
**NOTE**

To get an overview of the actors, look into the Use Case Introduction, step 1 of this module.

---

One of the requirements to successfully process a dispute is that all of the parties involved are aware of the dispute status at all times, since they can all influence the final resolution of the dispute. Because different parties will have visibility into, and sometimes control over, the process, we need a domain model that clearly depicts the objects that describe the data and the actors that do interact in the resolution of the Credit Card Dispute process.

So far we can identify 3 major external entities:

- `Card Holder`
- `Fraud Data`
- `Additional Informations`

The difference between these entities and the Actors is that these entities are the ones that contains data that is relevant to the processing of the dispute. This data will come from different actors and will be stored in the dispute process instance as process variables.
