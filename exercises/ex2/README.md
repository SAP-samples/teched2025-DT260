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

1.	Enter this code.
```abap
DATA(lt_params) = request->get_form_fields(  ).
READ TABLE lt_params REFERENCE INTO DATA(lr_params) WITH KEY name = 'cmd'.
  IF sy-subrc = 0.
    response->set_status( i_code = 200
                     i_reason = 'Everything is fine').
    RETURN.
  ENDIF.

```

2.	Click here.
<br>![](/exercises/ex2/images/02_02_0010.png)

## Summary

You've now ...

Continue to - [Exercise 3 - Excercise 3 ](../ex3/README.md)
