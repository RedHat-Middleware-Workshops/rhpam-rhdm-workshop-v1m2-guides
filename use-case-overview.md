<!-- # Use Case Review

The Credit Card Dispute process is not straightforward. It involves different actors inside, and outside of, the company. These actors need to have visibility and control at all times into what is happening during the processing of each dispute.

We have identified the actors involved in the overall CC Dispute process. These actors are outside of the corporation and we can think of them as external entities that we will connect with to get information, but that do not participate in the dispute resolution process. We will see more of that in the Case Management Scenario.

One of the requirements to successfully process a dispute is that all of the parties involved are aware of the dispute status at all times, since they can all influence the final resolution of the dispute. Because different parties will have visibility into, and sometimes control over, the process, we need a domain model that clearly depicts the objects that describe the data and the actors that do interact in the resolution of the Credit Card Dispute process.

So far we can identify 3 major external entities:

- `Card Holder`
- `Fraud Data`
- `Additional Informations`

The difference between these entities and the Actors is that these entities are the ones that contains data that is relevant to the processing of the dispute. This data will come from different actors and will be stored in the dispute process instance as process variables. -->

# Use Case Overview

You are a business automation specialist consultant who was hired by a credit card issuer company, Pecunia Corp. Your task now is to automate the decisions and rules of a _Credit Card Dispute_ process.

## Credit Card Dispute - Transaction Risk

Every dispute has a possible risk to be incorrect or to be a fraud. Based on existing data, it is possible to determine the set that determines whether the dispute can be qualified for automated chargeback.

For Pecunia Corp., depending on the customer status and value disputed, the risks can be higher or lower. The bank handled the following rules to you:

- For a **standard** customer, and a dispute amount **between 0 and 100**, the risk is **low**.;
- For a **standard** customer, and a dispute amount **between 100 and 500**, the risk is **medium**;
- For a **standard** customer, and a dispute amount **above 500**, the risk is **high**.
- For a **silver** customer, and a dispute amount **below 250**, the risk is **low**.
- For a **silver** customer, and a dispute amount **between 250 and 500**, the risk is **medium**.
- For a **silver** customer, and a dispute amount **above 500**, the risk is **high**.
- For a **gold** customer, and a dispute amount **below 500**, the risk is **low**.
- For a **gold** customer, and a dispute amount **over 500**, the risk is **medium**.

In your implementation, the business users must have the ability to change criteria anytime if needed, and apply the changes according to the release processes of Pecunia Corp.

You should also consider that the user can change the criteria without technical assistance. Pecunia Corp., business users cannot code, therefore, your solution should allow them to update the rules using standard spreadsheet-like decision tables or quasi natural language.

Make sure you also deliver automated tests for your rules.
