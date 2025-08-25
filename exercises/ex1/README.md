# Exercise 1 - Modernize the Flight Evaluation application with ABAP Cloud

The Flight Evaluation application was developed with classic ABAP and is designed to collect customer feedback for the flights of different flight companies with regard to meal, flight and service quality.
The rating is based on the 0-to-5 scale, with 0 representing the lowest possible score (e.g., "very poor" or "unsatisfactory") and 5 representing the highest (e.g., "outstanding"). 
In this exercise, you will modernize the Flight Evaluation application using ABAP Cloud developement model. 

## Exercise 1.1 Get to know the Flight Evaluation application and analyze it for ABAP Cloud with ABAP test cockpit (ATC)

After completing these steps you will have understood how the Flight Evaluation application works and executed ABAP test cockpit (ATC) ABAP Cloud readiness checks to get the ATC result list of the incompatible issues with ABAP Cloud.

1. Click here.
<br>![](/exercises/ex1/images/dt260_ex1_1_start_abap_program.png)

2. Click here.
<br>![](/exercises/ex1/images/dt260_ex1_2_enter_data_abap_program.png)

3. Click here.
<br>![](/exercises/ex1/images/dt260_ex1_3_run_atc.png)

4. Click here.
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

