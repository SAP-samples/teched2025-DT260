# Exercise 1 - Modernize the Flight Evaluation application with ABAP Cloud

The Flight Evaluation application was developed with classic ABAP and is designed to collect customer feedback for the flights of different flight companies with regard to meal, flight and service quality.
The rating is based on the 0-to-5 scale, with 0 representing the lowest possible score (e.g., "very poor" or "unsatisfactory") and 5 representing the highest (e.g., "outstanding"). 
In this exercise, you will modernize the Flight Evaluation application using ABAP Cloud by transforming its application logic to the ABAP Cloud development model. 

## Exercise 1.1 Get to know the Flight Evaluation application and analyze it for ABAP Cloud with ABAP test cockpit (ATC)

After completing these steps you will have understood how the Flight Evaluation application works and executed ABAP test cockpit (ATC) ABAP Cloud readiness checks to get the ATC result list of the incompatible issues with ABAP Cloud.

1. Start the Flight Evaluation application. Run the ABAP program  **`ZFLIGHT_EVALUATION_EX_##`** by selecting it in the project explorer and clicking **F8** button. The program will be started in ABAP development tools for Eclipse embedded in the SAP GUI. Enter the carrier id **LH**, connection id **0400** and flight date **16.02.2025** as proposed on the screenshot and click **Execute** button.
   
<br>![](/exercises/ex1/images/dt260_ex1_1_start_abap_program.png)

2. The flight evaluation program will display the customer data (like booking id, customer number, customer name) for the selected flight and flight date along with the customer ratings for meal, flight and service. Double-click on a table row and enter a customer rating for meal, flight and service of the selected customer on the used flight connection. Play around with the flight evaluation application to get to know it: you can sort the table and enter further customers evaluations.
   
<br>![](/exercises/ex1/images/dt260_ex1_2_enter_data_abap_program.png)

3. Now it's time to analyze the Flight Evaluation application for ABAP Cloud. Since the application logic is implemented in the ABAP class **`ZCL_FLIGHT_EVALUATION_EX_##`** using the table **`ZFLEVAL_EX_##`** you would need only to analyze the class. You will leave the ABAP program as **`ZFLIGHT_EVALUATION_EX_##`** as it is, since for the purpose of modernization in the upcoming exercises you will provide the Flight Evaluation application with the new SAP Fiori UI and will not need the ABAP program any longer. Select the ABAP class **`ZCL_FLIGHT_EVALUATION_EX_##`** in the project explorer end use the context menu **Run As->ABAP Test Cockpit With...** with the **ABAP_CLOUD_READINESS** check variant to execute ABAP Cloud readiness checks.
 
<br>![](/exercises/ex1/images/dt260_ex1_3_run_atc.png)

4. The ABAP test cockpit check results will be displayed in the ATC Problems View and can be used as a basis for your adaptation tasks to ABAP Cloud.
 
<br>![](/exercises/ex1/images/dt260_ex1_4_atc_results.png)


## Exercise 1.2 Transform the application logic to the ABAP Cloud

After completing these steps you will have...

1.	Enter this code.
```abap
DATA(lt_params) = request->get_form_fields(  ).
READ TABLE lt_params REFERENCE INTO DATA(lr_params) WITH KEY name = 'cmd'.
  IF sy-subrc <> 0.
    response->set_status( i_code = 400
                     i_reason = 'Bad request').
    RETURN.
  ENDIF.

```

2.	Click here.
<br>![](/exercises/ex1/images/01_02_0010.png)

## Exercise 1.3 Move the ABAP Cloud ready development objects to the ABAP Cloud development package

After completing these steps you will have created...

1. Click here.
<br>![](/exercises/ex1/images/01_01_0010.png)

2.	Insert this line of code.
```abap
response->set_text( |Hello World! | ). 
```

## Summary

You've now ...

Continue to - [Exercise 2 - Exercise 2 Description](../ex2/README.md)

