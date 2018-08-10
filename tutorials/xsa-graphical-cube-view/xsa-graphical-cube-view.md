---
title: SAP HANA XS Advanced, Creating a Cube Calculation View
description: Creating a Graphical Calculation View with a Cube data type
primary_tag: products>sap-hana
auto_validation: true
tags: [  tutorial>intermediate, topic>sql, products>sap-hana, products>sap-hana\,-express-edition   ]
---
## Prerequisites  
- **Proficiency:** Intermediate
- **Tutorials:** [Create a calculation view of data type Dimension](https://www.sap.com/developer/tutorials/xsa-graphical-view.html)


## Details
### You will learn  
- How to create a calculation view with a Cube data type
- Use a star join to combine data from a dimension calculation view and a table
- Perform currency conversion


### Time to Complete
**15 Min**.

---

[ACCORDION-BEGIN [Step 1: ](Create a new Calculation view of type Cube)]

Create a new calculation view in your models folder.

![Create calculation view](1.png)

Call it `PURCHASE_ORDERS`, choose `CUBE` as a data category and mark the flag for `With Star Join`

![Call it PO](2.png)

[DONE]



[ACCORDION-BEGIN [Step 2: ](Join header and items)]

Drag and drop a join node and add click on the **+** sign to add the data sources

![New Join node](join.gif)

Type `PO` and choose both `PO.Header` and `PO.Item` and click **Finish**.

![Call it PO](3.png)

Create a inner join using `PURCHASEORDERID` and set the cardinality as `1..n`

![Call it PO](4.png)

In the mapping tab, add the following columns to the output: `PURCHASEORDERID`, `HISTORY.CHANGEDAT`, `PRODUCT.PRODUCTID`, `CURRENCY` and `GROSSAMOUNT`.

![Mapping](5.png)

Finally, rename the node to `PO`.

[DONE]

[ACCORDION-END]

[ACCORDION-BEGIN [Step 3: ](Join Purchase Orders with products dimension)]

Connect the output of the join to the `Star Join`. Use the **+** sign to search and select `PRODUCTS`.

![Create star join](star.gif)

Join the `PRODUCTID` and `PRODUCT.PRODUCTID` as an inner join.

![Mapping](6.png)

[DONE]

[ACCORDION-END]

[ACCORDION-BEGIN [Step 4: ](Create an input parameter)]

In the Input Parameters tab, create an input parameter named `IP_O_TARGET_CURRENCY`.

![Mapping](7.png)

Configure it as `NVARCHAR`, length 3 with a Semantic type of Currency.
Flag it as mandatory and set a default value of `USD`.

![Mapping](8.png)

[DONE]

[ACCORDION-END]


[ACCORDION-BEGIN [Step 5: ](Configure semantics for currency conversion)]

Go into the mapping tab and double click on the `PO` parent node to add all of the fields to the output. Add the `GROSSAMOUNT` a second time.

![Mapping](9.png)

Change the name of the duplicate column to `OriginalGrossAmount`.

![Mapping](10.png)

Click the semantics node and assign semantics for `GROSSAMOUNT`.

![Mapping](11.png)

Choose **Amount with Currency Code** and configure as follows:

- Display Currency: column, choose `GROSSAMOUNT_CURRENCY`
- Check the `Conversion` and `Decimal shift` boxes

![Mapping](12.png)

Click on the `definition` tab and use the following configuration

![Mapping](13.png)

**Save** the view.

[DONE]

[ACCORDION-END]


[ACCORDION-BEGIN [Step 6: ](Build and preview)]

Build the `db` module and return to the database explorer

![Mapping](15.png)

Navigate to the column view, right-click on it and select **Open Data**.

![Final view](16.gif)

[VALIDATE_1]

[ACCORDION-END]


---