# Use Case Overview

The requirements that are handed to you:
These requirements are the policies of Pecunia Corp. to handle a Credit Card Dispute.


## Background

The cost of processing a credit card dispute is very high, and also critical from the customer experience perspective.

Usually the credit card holder is stressed to protect the assets trusted to the bank, therefore one of the requirements for the interaction with the dispute system is the constant feedback to the customer, informing the latest status of the dispute. E.g., who is currently processing the dispute, is additional information from the customer required, has the dispute been automatically accepted, has something gone wrong with the dispute, etc.

Most of the complexity with the CC Dispute process comes from the fact that is a multi-step process where every dispute is a one-off situation, the actual outcome of the dispute is a result of the interactions between the different actors and the decision logic. On top of that, the information regarding the case should be available with every interaction. Everyone needs to look at the same data and be observers of changes in it.

The actors that we can identify are:

- _Credit Card Holder: aka Customer_


- _Credit Card Issuer: In this case Pecunia corp._


- _Card processing network:  The organization that oversees the process. Some differ in their procedures than others._


- _Credit Card Acquirer: A financial institution that obtains the rights to the merchant’s account and tasked with getting payment on the merchant’s behalf._


- _Merchant: Seller of the goods and must either fight or accept the chargeback._



We can resume the process in the following diagram:

![CC Dispute Processing]({% image_path business-central-cc-dispute-processing.png %}){:width="600px"}

The basic steps are:

1- _The Credit Card Holder starts a dispute with the CC Issuer._

2- _The CC Issuer needs to decide what type of processing is required for the dispute (automated chargeback or normal processing).Jump to step 3.1. or 3.2._

3.1- _The CC Issuer process the automated chargeback. Jump  to step 5.1._

3.2 - _The CC Issuer needs to do standard processing. A risk assessment for the dispute is required._

4 - _Based on the provided data (credit card holder status, dispute amount, etc.), CC Issuer assesses the risk of the dispute._

4.1- _The CC Issuer requests a manual approval for the dispute from a knowledge worker. Jump to step 5.1. or 5.2_

4.2- _The CC Issuer based on the data resolves the case by either accepting or rejecting the dispute. Jump to step 5.1. or 5.2_

5.1- _The dispute is accepted and the money reimbursed to the CC Holder and the backoffice chargeback for fee transactions started_

5.2- _The dispute is rejected_

6- _The CC Issuer informs the CC Holder of the result._


--------------------------------------------------

### Business Requirements:


There is a point in the process where, depending on a business decision, the processing path bifurcates. The decision making is right now subjective, as a human - in this case a CC Issuer agent- is responsible  to reach a conclusion based on his/her individual knowledge.

Hence there is a decision set that changes the overall processing: the set that determines whether the dispute can be qualified for automated chargeback.

In this use-case, going back and forth in the whole processing chain as shown in the picture, is costly for all the parties involved, plus the amount of the dispute can be less than the cost of processing the dispute, in addition to that the CC Issuer can offer automated chargeback to it's highly loyal customers.

![CC Dispute Processing Backoffice]({% image_path business-central-cc-dispute-processing-backoffice-processing.png %}){:width="600px"}

So the bifurcation point gives Pecunia corp the ability to gain loyalty with strategic customers and avoid cost. this scenario is Automatic vs Standard Processing. The following diagram describes the scenario:

![CC Dispute Processing Automated Chargeback]({% image_path business-central-cc-dispute-processing-automated-chargeback.png %}){:width="600px"}

The decision point in the process is used to determine the risk of the transaction, which is data that will be used when manual approval is required.

![CC Dispute Processing Manual Standard Processing]({% image_path business-central-cc-dispute-processing-manual-standard-processing.png %}){:width="600px"}

The risk of the transaction is determined by the status of customer and the amount of the dispute:

- _For a standard customer, and a dispute amount between 0 and 100, the risk is low._
- _For a standard customer, and a dispute amount between 100 and 500, the risk is medium_
- _For a standard customer, and a dispute amount above 500, the risk is high._
- _For a silver customer, and a dispute amount below 250, the risk is low._
- _For a silver customer, and a dispute amount between 250 and 500, the risk is medium._
- _For a silver customer, and a dispute amount above 500, the risk is high._
- _For a gold customer, and a dispute amount below 500, the risk is low._
- _For a gold customer, and a dispute amount over 500, the risk is medium._


### Functional Solution:

Have business rules that will take into account consistent criteria defined to assess risk and automate processing. The business user must have the ability to change these criteria anytime if needed, and apply the changes according to the release process of Pecunia corp.. 


### Non Functional:

Allow the user to change the criteria without technical assistance. Have the tooling for the user to update the rules but using standard spreadsheet-like decision tables or quasi natural language.
