# Exercise 2 - Create SAP Fiori application with ABAP Cloud and RAP for Flight Evaluation

In this exercise, we will build the modern SAP Fiori application with ABAP Cloud using RAP unmanaged scenario on top of the transformed to ABAP Cloud application logic of the Flight Evaluation application. The RAP unmanaged scenario is perfectly suited for migration scenarios from classic ABAP since it gives you more control over the data persistence and business logic compared to the RAP managed approach, where the RAP frameworks handles everything.

## Exercise 2.1 Generate SAP Fiori RAP application for Flight Evaluation

After completing these steps you will have created...

1. Generate SAP Fiori application on top of the flight evaluations table **`ZFLEVAL_EX_##`** by creating OData UI service. Follow the generator wizard steps on the screenshots below and enter the alias name **FlightEval** for the CDS data definition. Finally open the service binding as prompted in the last popup screen.
   
<br>![](/exercises/ex2/images/dt260_ex2_1_create_odata_service.png)

2.	Click on the **Publish** button, select the **FlightEval** and click on the **Preview...**.

<br>![](/exercises/ex2/images/dt260_ex2_11_publsh_odata_service.png)
   
```abap
response->set_text( |Hello ABAP World! | ). 
```
3. The generated SAP Fiori Flight Evaluations application starts. Click on the **Go** button to see the flight evaluations. Currently only the ??? evaluations can be displayed, you cannot add flight evaluations for different airlines en masse, and if you click on the table row to edit a flight evaluation for the customer, then your changes will not be stored in the flight evaluations table.
   
<br>![](/exercises/ex2/images/dt260_ex2_2_fiori_display_evaluations.png)

## Exercise 2.2 Sub Exercise 2 Description

After completing these steps you will have...

1.	Open the Behavior Definition **`ZBP_FLEVAL_EX_##`** and add the source code line **with unmanaged save** to let you manage the persistency table with flight evaluations (not the RAP framework). You will get the error message, telling that it is not possible to specify the persistent table and use unmanaged scenario at the same time. Follow the steps on the screenshots below to comment out the **persistent table**-statement and apply the quick fixes (Ctrl +1 shotcut) to generate the required **save_modified** method and authorization method in the corresponding Behavior Implementation class **`ZBP_FLEVAL_EX_##`**.

<br>![](/exercises/ex2/images/dt260_ex2_3_add_unmanaged_save.png)

2.	Click here.
<br>![](/exercises/ex2/images/02_02_0010.png)

## Exercise 2.3 Sub Exercise 2 Description

After completing these steps you will have...

1.	Enter this code.


2.	Click here.
<br>![](/exercises/ex2/images/02_02_0010.png)

## Summary

You've now ...

Continue to - [Exercise 3 - Excercise 3 ](../ex3/README.md)
