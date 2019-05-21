# Use Case Overview

The requirements that are handed to you:
These requirements are the policies of Pecunia Corp. to handle a Credit Card Dispute.


## Background

The cost of processing a credit card dispute is very high, and also critical from the customer experience perspective.

Usually the credit card holder is stressed to protect the assets trusted to the bank, therefore one of the requirements for the interaction with the dispute system is the constant feedback to the customer, informing the latest status of the dispute. E.g., who is currently processing the dispute, is additional information from the customer required, has the dispute been automatically accepted, has something gone wrong with the dispute, etc.

Most of the complexity with the CC Dispute process comes from the fact that is a multi-step process where every dispute is a one-off situation, the actual outcome of the dispute is a result of  the interactions between the different actors and the decision logic. On top of that, the information regarding the case, has to be the input and output of every interaction, everybody need to look at the same data and be observers of changes in it.

The actors that we can identify are:

- _Credit Card Holder: aka Customer_


- _Credit Card Issuer: In this case Pecunia corp._


- _Card processing network:  The organization that oversees the process. Some differ in their procedures than others._


- _Credit Card Acquirer: A financial institution that obtains the rights to the merchant’s account and tasked with getting payment on the merchant’s behalf._


- _Merchant: Seller of the goods and must either fight or accept the chargeback._



We can resume the process in the following diagram:



<img src="../../assets/middleware/rhpam-7-workshop/business-central-cc-dispute-processing.png"  width="600" />


The basic steps are:

1- _The Credit Card Holder starts a dispute with the CC Issuer._

2- _The CC Issuer needs to decide what type of processing is required for the dispute (automated chargeack or normal processing).Jump to step 3.1. or 3.2._

3.1- _The CC Issuer process the automated chargeback. Jump  to step 5.1._

3.2 - _The CC Issuer needs to do standard processing, contact the Card Processing network to start the dispute, the Credit Processing Network then contacts the CC Acquirer that requests evidence to the merchant and a formal response to the dispute._

3.3 - _The Merchant send the evidence and response to the CC Issuer_

4- _The CC Issuer assess the risk of the dispute._

4.1- _The CC Issuer requests a manual approval for the dispute from a knowledge worker. Jump to step 5.1. or 5.2_

4.2- _The CC Issuer based on the data resolves the case. Jump to step 5.1. or 5.2_

5.1- _The dispute is accepted and the money reimbursed to the CC Holder and the backoffice chargeback for fee transactions started_

5.2- _The dispute is rejected_

6- _The CC Issuer informs the CC Holder of the result._


--------------------------------------------------

### Business Requirements:


There are two points in the process where depending on a business decision, the processing path bifurcates. The decision making is right now subjective, as a human - in this case a CC Issuer agent- is responsible  to reach a conclusion based on his/her individual knowledge.

Hence there are two decision's sets that change the overall processing making: One set that determines whether the dispute can be qualified for automated chargeback, and a set that determines the risk of the dispute for manual approval and resolution.

For the first scenario, going back and forth in the whole processing chain as shown in the picture, is costly for all the parties involved, plus the amount of the dispute can be less than the cost of processing the dispute, in addition to that the CC Issuer can offer automated chargeback to it's highly loyal customers.


<img src="../../assets/middleware/rhpam-7-workshop/business-central-cc-dispute-processing-backoffice-processing.png"  width="600" />


So the first bifurcation point gives Pecunia corp the ability to gain loyalty with strategic customers and avoid cost, this scenario is Automatic vs Standard Processing. The following diagram describes the scenario:



<img src="../../assets/middleware/rhpam-7-workshop/business-central-cc-dispute-processing-automated-chargeback.png"  width="600" />

The second use case has the decisions to determine the risk of the transaction and if a manual approval is required.

<img src="../../assets/middleware/rhpam-7-workshop/business-central-cc-dispute-processing-manual-standard-processing.png"  width="600" />

Once that is decided that the dispute will be processed in a standard way, by contacting all the chain of CC transaction processing (3) we have the next bifurcation in step 4 of the processing, based on the case information we need too determine if a dispute needs a manual approval, to such effect  we have the following rule:

_Every amount larger than 1000 should be manually approved._

An also at this point we need to determine the risk profile of the dispute, this risk scoring will be part of the input for both the manual approval path and the automated resolution.

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

Allow the user to change the criteria without technical assistance. Have the tooling for the user to update the rules but using standard spreadsheet-like decision tables or cuasi natural language.
