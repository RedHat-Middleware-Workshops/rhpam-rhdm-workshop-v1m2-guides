
# Authoring Decisions and Rules

In this section you will learn:

1. How to use your Business Object Model to automate decisions and policies.

2. The different types of authoring tools available to you.

3. How to use your automated decisions and rules.


Solving a credit card dispute depends on several variables, like:

- The type of customer
- The amount of the dispute


The knowledge of how to apply these rules and decisions is tacit and lives only in the head of other domain experts like you. In order to automate the process, you will first have to express the business policies that determine how a dispute is handled in the form of rules.

For this particular case, 2 sets of rules are defined for different stages on the process:

## Calculating the Risk

At the moment, all processing is manual. There is a group of agents dedicated to making decisions based on the data of the dispute. This is not only expensive, but also very prone to error and inconsistent.

The cost of processing a dispute for Pecunia Corp. is high and independent of the amount that is being disputed. That is why it's very important to have flexible rules that reduce the processing cost and time. Apart from reducing cost, this will also improve customer customer experience.

At the moment, all processing is manual. There is a group of agents dedicated to making decisions based on the data of the dispute. This is not only expensive, but also very prone to error and inconsistent.

The rules defined for the process are:

- Automatic chargeback is only available to Platinum and Gold Credit Card Holders

- The risk of the transaction is determined by the type of user and the amount of the dispute

      - Standard customer 0-100 risk low risk
      - Standard customer 100-500 medium
      - Standard customer anything above 500 high
      - Gold customer anything less than 500 low risk
      - Gold customer anything more than 800 high risk
      - Silver customer anything between 250-500 medium risk
      - Silver customer anything below 250  low risk



- If the customer billing address is in the state on Texas, California or Florida the dispute should be consider of higher risk.

## The Authoring Tools

We have defined the Business Object Model in the previous lab. As the workshop platform does not support persistence between modules, you need to import the following repository to retrieve a project that contains the model. If you don't know how to import an existing project, you can watch the video on how to import a repository into your workspace

1. Import the rest of the Domain Model by importing the project Domain Model CC Dispute  from the following repository:

https://github.com/MyriamFentanes/business-policies-decisions-scenario-step2




### Decision Tables

A very common way to define the logic behind risk assessment is to store this information in spreadsheets. With Red Hat Process Automation Manager you can use the same spreadsheet approach and make it an executable asset (i.e. a set of rules) in the engine. In this section we are going to create a _Decision Table_ to automate the risk assessment rules that were given to you.

1. First we go back to the Library view and we click on the blue button `Add Asset`.

<img src="../../assets/middleware/rhpam-7-workshop/business-central-decision-table-add-asset.png"  width="600" />

2. We select `Guided Decision Table` from the catalog of assets

<img src="../../assets/middleware/rhpam-7-workshop/business-central-decision-table-add-asset-guided.png"  width="600" />

3. Type the following values on the `Create New Decision Table` wizard

Name: `risk-evaluation`{{copy}}
Package: `com.myspace.ccd_project`{{copy}}
Select the checkbox: Use Wizard

Click ok and Finish

4. You should see the `Guided Decision Table` wizard with an empty table.

<img src="../../assets/middleware/rhpam-7-workshop/business-central-decision-table-new.png"  width="600" />

There are 5 tabs in the wizard:

- Model: The model diagram of the Decision Table
- Columns: Wizard to Add, Edit or Delete columns in your table. Each column is a constraint (LHS) on a property of a Business Model Object, or an action (RHS).
- Overview: Contains the meta-information of your asset: Version, Description, Last Modified, etc.
- Source: Is the actual source code that is generated from the Decision Table Model. In the runtime engine, decision tables are translated into native DRL (Drools Rule Language), where each row in the table is translated into a rule.
- Data Objects: Lists the Business Objects available to the wizard to be used as conditions and/or actions

In our system, the properties evaluated to determine the risk scoring are:

- Status of the Credit Card Holder
- Total Amount disputed from the Fraud Data

Let's add the Credit Card Holder condition column

5. Go to the columns tab and click on the button `Insert Column`, Select `Add Condition` and click Next.

<img src="../../assets/middleware/rhpam-7-workshop/business-central-add-condition.png"  width="600" />

6. We need to define which object is going to be evaluated. Click on `Create new Fact Pattern`. Select `CreditCardHolder` as the Fact type and define a variable called `holder`{{copy}} as the _Binding_. Click _Next_.

<img src="../../assets/middleware/rhpam-7-workshop/business-central-create-pattern.png"  width="600" />

7. The calculation type is the type of evaluation that we are going to apply. In this case it will be against literal values. Select `Literal value` and click _Next_.

8. Select the field `status` and click _Next_.

<img src="../../assets/middleware/rhpam-7-workshop/business-central-create-pattern-field.png"  width="600" />

9. Next we select the operator for the constraint. Select `equal to` from the drop down menu and click _Next_.

<img src="../../assets/middleware/rhpam-7-workshop/business-central-create-pattern-field-operator.png"  width="600" />

10. Since there are only 3 possible status, we are going to configure the _Value list_ with the following values, set the _Default value_ to `Standard` and then click Next.

`"Standard,Silver,Gold"`{{copy}}

<img src="../../assets/middleware/rhpam-7-workshop/business-central-create-pattern-field-values.png"  width="600" />

11. We can now configure the label of the column

Header: `Status`{{copy}}

Click Finish and go back to the `Model` tab in the editor. You should see the newly created column.

<img src="../../assets/middleware/rhpam-7-workshop/business-central-create-pattern-field-header.png"  width="600" />

12. Repeat the same steps to add 2 more columns:
  - Pattern: `FraudData`
  - Calculation Type: Literal value
  - property: `totalFraudAmount`{{copy}}
  - operation:
    - `greater than` for one column. Use header name: "Minimum Amount"
    - `less than or equal to` for the second column. Use header nam "Maximum Amount"

Note that for the second column you don't need to create a new fact pattern, you can reuse the existing one.

At the end your decision table should look like this:

<img src="../../assets/middleware/rhpam-7-workshop/business-central-decision-table-columns.png"  width="600" />

13. Next go to the `Columns` tab and Click on `Insert Column`. This time we are adding an action, the Right-Hand-Side of a rule, that will be fired if the conditions are met. Select `Set the value of a field` and click next.

14. We want to set the risk scoring property of the `FraudData` object. So in the dropdown menu select the object `FraudData` bound to the variable `data`.Click Next.

<img src="../../assets/middleware/rhpam-7-workshop/business-central-decision-table-columns-action-data.png"  width="600" />

15.  Select the field `disputeRiskData` and click Next. We don't have a list of values so click Next. Type `Risk Scoring`{{copy}} as the header for the column and click Finish.

<img src="../../assets/middleware/rhpam-7-workshop/business-central-decision-table-columns-action-data-finish.png"  width="600" />

16. Go back to your `Model` tab, which should show the following decision table.

<img src="../../assets/middleware/rhpam-7-workshop/business-central-decision-table-columns-action-data-finish-model.png"  width="600" />

We are now going to add the actual constraints and actions, i.e. the actual rules. Looking at our requirements, the first constraint is defined as:

_For a standard customer, and a dispute amount between 0 and 100, the risk is low._

There are 4 levels of risk: low, medium, high and very-high. We will defined these risk-levels as integers: 1,2,3, and 4.

Click on the button Insert and select append row from the dropdown menu.

<img src="../../assets/middleware/rhpam-7-workshop/business-central-decision-table-append-row.png"  width="600" />

Click on the Description cell of the new row and type "_Low risk standard costumer_". Use the following values for the other columns:

Description:`Low risk standard costumer`{{copy}}
Status:`Standard`{{copy}}
Minimum Amount:`0`{{copy}}
Max Amount:`100`{{copy}}
Risk Scoring:`1`{{copy}}

Your decision table should look like this. Click Save.

<img src="../../assets/middleware/rhpam-7-workshop/business-central-decision-table-first-row.png"  width="600" />

Apply the same procedure for the rest of the rules. At the end you should have something like the following:


<img src="../../assets/middleware/rhpam-7-workshop/business-central-decision-table-complete.png"  width="600" />



## Guided Rules

Guided Rules are one of the various types of rules you can create in Business Central. Once you have defined the Business Object Model, you can create rules that check conditions on the properties of these objects, rules that define conditions on combinations of objects, etc. For example, you can define a rule with a constraint on a Credit Card Holder's age, his/her status, riskRating, etc. If the condition or conditions are met, the rule is set to be _matched_ and becomes eligible for _firing_. When the rule fires, it executes the action defined in the rule. The action is the _THEN_ part of the rule, or what is also called the rule's Right-Hand-Side (RHS).

In the case of the rules for automatic chargeback we are evaluating only the Credit Card Holder. So lets create the rule.
First we need to tell the rule what object or collection of objects is going to be evaluated. Rules have a very basic syntax, and basically consist of 3 parts. You have the _WHEN_ section, also known as the Left Hand Side (LHS) or Constraint. This is the part of the rule in which you define the discrimination criteria that is applied to the Credit Card Holders objects to discriminate the card holders that qualify for an automated chargeback.


1. Select the project ccd-project in the space MySpace

<img src="../../assets/middleware/rhpam-7-workshop/business-central-asset-ccd-project.png"  width="600" />

2. You will see the Domain Object Model as the only assets listed. Click on the blue button `Add Asset` on the right upper corner of the Library View.

<img src="../../assets/middleware/rhpam-7-workshop/business-central-ccd-bom-project.png"  width="600" />

3. Int the "Add Asset" screen, select "Decision" from the drop-down filter menu to filter on decision assets.

<img src="../../assets/middleware/rhpam-7-workshop/business-central-add-assets-filter.png"  width="600" />

4. Select `Guided Rule` from the filtered catalog of Wizards.

5. Set the following data in the creation wizard:

Name: `automated-chargeback`{{copy}}

Package: `com.myspace.ccd_project`{{copy}}

<img src="../../assets/middleware/rhpam-7-workshop/business-central-guided-rule-new.png"  width="600" />

6. Click ok. You should see a banner in green telling you that the asset was success fully created. The UI will display the wizard that allows you to author your rule.

<img src="../../assets/middleware/rhpam-7-workshop/business-central-guided-rule-new-wizard.png"  width="6

1. You will see 4 tabs in the wizard panel. Select the tab that says "Data Objects"

<img src="../../assets/middleware/rhpam-7-workshop/business-central-guided-rule-import-data-object.png"  width="600" />

2. You should see 4 items listed: `AdditionalInformation`, `CreditCardHolder`, `FraudData`, and `Number`. These are shown by default as the rule is created in the same folder/package as these data objects. If `CreditCardHolder` is not lister, click on the blue _New Item_ button to import it.

<img src="../../assets/middleware/rhpam-7-workshop/business-central-guided-rule-import-data-object-new.png"  width="600" />

3. Return to the _Model_ tab and Click on the green cross to the right of the word _WHEN_.

<img src="../../assets/middleware/rhpam-7-workshop/business-central-guided-rule-new-fact.png"  width="600" />

4. Select the object `CreditCardHolder`, and click ok. We are now telling the rule engine that every time there is a CreditCardHolder we will activate this rule.

<img src="../../assets/middleware/rhpam-7-workshop/business-central-guided-rule-new-fact-select.png"  width="600" />

In order to match the criteria of the functional requirement, we need to add a restriction on one the card holder's properties. Automated chargeback is only approved for CC Holders that have the `status` _Gold_ or _Platinum_.

5. Click on the condition `There is a Credit Card Holder`, a new wizard will open. We are now going to add a restriction on a field, in this case the `status` of the CC Holder

<img src="../../assets/middleware/rhpam-7-workshop/business-central-guided-rule-new-property-select.png"  width="600" />

6. From the dropdown box we select that the status `is contained in the list`, and add the literal value of _Gold_ and _Platinum_, separated by a comma. TIP: You can also add enumerations containing these values to have them pre-populated for you.

<img src="../../assets/middleware/rhpam-7-workshop/business-central-guided-rule-new-property-select-values.png"  width="600" />

7. Go back to the _Data Objects_ tab. If the `FraudData` data object has not been imported yet, complete the same procedure, to import it. Go back to the _Model_ tab and add a constraint on the `FraudData` object the same way as we did before. We don't need to put a constraint on any property of the `FraudData`, we just need to make sure that it's there.

<img src="../../assets/middleware/rhpam-7-workshop/business-central-guided-rule-check-fraud-data.png"  width="600" />

8. When you want to modify the data in the objects of the Business Model or facts, you need to be able to reference the matched object from within the rule. To allow this, the object needs to be bound to a variable inside the rule. This makes the object accessible in both the left-hand-side (LHS) and  right-hand-side (RHS) through the variable. Click on the fact declaration `There is FraudData`, the wizard to modify the constraints will open.

9. In the "Variable name" field at the bottom of the form, type `data`{{copy}} as the name of the variable that you want to bind the `FraudData` object to. Click on the _Set_ button.

<img src="../../assets/middleware/rhpam-7-workshop/business-central-guided-rule-modify-fraud-data.png"  width="600" />

Now we are going to set the property of automated chargeback to true on the `FraudData` object, so the dispute can be processed accordingly. Since this is the decision we are making, and thus the _action_ of the rule, we will define this as the THEN clause,  also known as the Right Hand Side (RHS) or Action section of our rule.

All of the information of the CC dispute is stored in facts. These facts can live in a session that the engine will keep in memory. So every time you evaluate a new fact, or change something to an existing fact, you will have all of the Objects in the session available in the process of decision making. In the RHS, or action, part of the rule you can change the values of any property on the objects that you can reference via the variables, or even create and add new objects/facts to the session (this is usually referred to as _inferring_ new data or information). Every time a property in an object changes, all of the decisions in which this property is used will be reevaluated to make sure that no other rule needs to be applied
10. Click on the green arrow next to the _WHEN_ keyword. When the `Add new action` wizard opens select `Change field values of data`, select the variable that you created before, and click on `+ok`.

<img src="../../assets/middleware/rhpam-7-workshop/business-central-guided-rule-modify-fraud-data-wizard.png"  width="600" />

11. Now we are going to set the value of the property `automated` to `true`, indicating that an automatic chargeback applies. Click on  the action `Set value of FraudData [data]` and select the field `automated`. Click on the pencil icon to the right and assign a literal value to the property.

<img src="../../assets/middleware/rhpam-7-workshop/business-central-guided-rule-modify-fraud-automated.png"  width="600" />

12. select `true` as the value for the automated property (this is the default value for booleans, so the property is probably already set to `true`). Note that since the type of data is boolean, you can only choose between `true` and `false`.

<img src="../../assets/middleware/rhpam-7-workshop/business-central-guided-rule-modify-fraud-automated-true.png"  width="600" />

13. To validate that everything is correct, click on the _Validate_ button on the right and you should see a green "Item successfully validated!" message. Next, click on "Save" to save the rule.

<img src="../../assets/middleware/rhpam-7-workshop/business-central-guided-rule-validate.png"  width="600" />


You have created your first Business Rule using the Guided editor



## Decision Model & Notation (DMN)

Red Hat Process Automation Manager 7 supports the Decision Model & Notation (DMN) v1.2 standard. This means that models created in the DMN v1.1 or v1.2 specification can be imported into, and executed on, RHPAM. This allows you to create DMN decision models in other DMN editors, for example Trisotech's Digital Enterprise Suite, and execute then in RHPAM. In the following image we can see some examples of the types of diagrams you can create to define , in this case, the rules to calculate risk.

<img src="../../assets/middleware/rhpam-7-workshop/business-central-trisotech-dmn.png"  width="600" />

DMN uses a language business friendly called FEEL or Friendly Enough Expression Language.

<img src="../../assets/middleware/rhpam-7-workshop/business-central-dmn-feel.png"  width="600" />

DMN is out of scope for this workshop. However, the specification provides and additional, interesting, and standard way to model and execute decisions in your business applications.
