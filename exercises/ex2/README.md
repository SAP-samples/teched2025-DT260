# Exercise 2 - Create SAP Fiori application with ABAP Cloud and RAP for Flight Evaluation

In this exercise, we will build the modern SAP Fiori application with ABAP Cloud using RAP unmanaged scenario on top of the transformed to ABAP Cloud application logic of the Flight Evaluation application. The RAP unmanaged scenario is perfectly suited for migration scenarios from classic ABAP since it gives you more control over the data persistence and business logic compared to the RAP managed approach, where the RAP frameworks handles everything.

## Exercise 2.1 Generate SAP Fiori RAP application for Flight Evaluation

After completing these steps you will have created...

1. Generate SAP Fiori application on top of the flight evaluations table **`ZFLEVAL_EX_##`** by creating OData UI service. Follow the generator wizard steps on the screenshots below and enter the alias name **FlightEval** for the CDS data definition. Finally open the service binding as prompted in the last popup screen.
   
<br>![](/exercises/ex2/images/dt260_ex2_1_create_odata_service.png)

2.	Click on the **Publish** button, select the **FlightEval** and click on the **Preview...**.

<br>![](/exercises/ex2/images/dt260_ex2_11_publsh_odata_service.png)
   
3. The generated SAP Fiori Flight Evaluations application starts. Click on the **Go** button to see the flight evaluations. Currently only the ??? evaluations can be displayed, you cannot add flight evaluations for different airlines en masse, and if you click on the table row to edit a flight evaluation for the customer, then your changes will not be stored in the flight evaluations table.
   
<br>![](/exercises/ex2/images/dt260_ex2_2_fiori_display_evaluations.png)

## Exercise 2.2 Add the functionality to enter a flight rating for a customer

After completing these steps you will have...

1.	Open the Behavior Definition **`ZBP_FLEVAL_EX_##`** and add the source code line **with unmanaged save** to let you manage the persistency table with flight evaluations (not the RAP framework). You will get the error message, telling that it is not possible to specify the persistent table and use unmanaged scenario at the same time. Follow the steps on the screenshots below to comment out the **persistent table**-statement and apply the quick fixes (Ctrl +1 shotcut) to generate the required **save_modified** method and authorization method in the corresponding Behavior Implementation class **`ZBP_FLEVAL_EX_##`**.

<br>![](/exercises/ex2/images/dt260_ex2_3_add_unmanaged_save.png)

2.	Now we want to become able in our SAP Fiori application to enter a flight evaluation for a customer and save it in the flight evaluation table. For this add the following code to the **save_modified** method of your  Behavior Implementation class **`ZBP_FLEVAL_EX_##`**, save and activate it.
   
```abap

    IF create IS NOT INITIAL.

    ELSEIF update IS NOT INITIAL.
      LOOP AT update-flighteval INTO DATA(ls_update).
        DATA eval_obj TYPE REF TO zcl_flight_evaluation_ex_00.
        eval_obj = NEW zcl_flight_evaluation_ex_00(
            i_carrid = ls_update-CarrID
            i_connid = ls_update-ConnID
            i_fldate = ls_update-Fldate
            i_bookid = ls_update-BookID
        ).
        eval_obj->set_customer_id( ls_update-CustomID ).
        eval_obj->set_customer_name( ls_update-Name ).
        eval_obj->set_flight_rating( ls_update-FlightRating ).
        eval_obj->set_meal_rating( ls_update-MealRating ).
        eval_obj->set_service_rating( ls_update-ServiceRating ).

        eval_obj->save_on_db( ).
      ENDLOOP.
    ELSEIF delete IS NOT INITIAL.
    ELSE. 
    ENDIF.

```
3. Start the SAP Fiori application in preview by opening the Service Binding **`ZUI_FLEVAL_EX_##_O4`** and clicking **Preview...** for the **FlighEval**. Now you can enter flight evaluation data for a customer and it will be stored in the database table **`ZFLEVAL_EX_00`**.
   
## Exercise 2.3 Add the functionality to persist new flight evaluation data 

After completing these steps you will have...

1.	Add the following code to your Behavior Definition **`ZR_FLEVAL_EX_##`** save and activate.
   ```abap
  static action createFlightEval parameter ZDT260_A_FLIGTHEVAL_5;
   ```
2. You will get the syntax error telling that the method **createFlightEval** must be added for the action. Use quick fix (Ctrl +1) to generate the method in the local class of the correspondng Behavior Implementation **`ZBP_FLEVAL_EX_##`**. 

   <br>![](/exercises/ex2/images/dt260_ex2_4_add_static_action.png)

3.	Now use the action by adding the following code to the Behavior Definition **`ZC_FLEVAL_EX_##`**.
  ```abap
  use action createFlightEval;
   ```
<br>![](/exercises/ex2/images/dt260_ex2_41_use_static_action.png)

4.	To see the **createFlightEval** button on the UI you need to add the following code to the Metadata Extension **`ZC_FLEVAL_EX_##`**.
  ```abap
   ,{ type: #FOR_ACTION, dataAction: 'createFlightEval', label: 'Create Flight Eval Data' } 
   ```
<br>![](/exercises/ex2/images/dt260_ex2_42_use_static_action_mde.png)

5. Finally add the logic to enter flight evaluation data. Go to the local types of the Behavior Implementation **`ZBP_FLEVAL_EX_##`** and add the local class **lcl_buffer**.
   
   ```abap
     CLASS lcl_buffer DEFINITION.
     PUBLIC SECTION.
       TYPES: BEGIN OF gty_buffer,
                carrid TYPE z_carrid,
                connid TYPE z_connid,
                fldate TYPE d,
              END OF gty_buffer.
       TYPES gtt_buffer TYPE TABLE OF gty_buffer WITH EMPTY KEY.
   
       CLASS-DATA buffer TYPE STANDARD TABLE OF gty_buffer WITH EMPTY KEY.
      ENDCLASS.
   ```
Use the **lcl_buffer** class in the **save_modified** method by adding this code to the **ELSE** clause.
```abap
   LOOP AT lcl_buffer=>buffer INTO DATA(buffer).
     DATA(lt_evaluation_data) =
         zcl_flight_evaluation_ex_00=>create_flight_evaluation( i_carrid = buffer-carrid
                                                                i_connid = buffer-connid
                                                                i_fldate = buffer-fldate ).
   ENDLOOP.
```
<br>![](/exercises/ex2/images/dt260_ex2_44_use_lcl_buffer.png)

Save and activate the Behavior Implementation **`ZBP_FLEVAL_EX_##`**.

6. Start the SAP Fiori application in preview by opening the Service Binding **`ZUI_FLEVAL_EX_##_O4`** and clicking **Preview...** for the **FlighEval**. Now you can fill the flight evaluation table with additional  flight evaluations from the SBOOK table. Just follow the steps on the screenshots below.
   
<br>![](/exercises/ex2/images/dt260_ex2_5_fiori_create_evaldata_button.png)

## Exercise 2.4 Add visualisation with asterisks for customer ratings.

After completing these steps you will have...

1.	Open the Metadata Extension **`ZC_FLEVAL_EX_##`** and add replace the source code after the **Name;** for meal, flight and service ratings with the following code:
   
```abap
    @UI.lineItem: [ {
    position: 70 ,
    importance: #MEDIUM,
    label: 'Meal Rating'
    ,type: #AS_DATAPOINT
  } ]
  @UI.identification: [ {
    position: 70 ,
    label: 'Meal Rating'
    ,type: #AS_DATAPOINT
  } ]
  @UI: { dataPoint: { title: 'Meal Rating', targetValue: 5, visualization: #RATING }}
  MealRating;

  @UI.lineItem: [ {
    position: 80 ,
    importance: #MEDIUM,
    label: 'Flight Rating'
    ,type: #AS_DATAPOINT
  } ]
  @UI.identification: [ {
    position: 80 ,
    label: 'Flight Rating'
    ,type: #AS_DATAPOINT
  } ]
  @UI: { dataPoint: { title: 'Flight Rating', targetValue: 5, visualization: #RATING }}
  FlightRating;

  @UI.lineItem: [ {
    position: 90 ,
    importance: #MEDIUM,
    label: 'Service Rating'
    ,type: #AS_DATAPOINT
  } ]
  @UI.identification: [ {
    position: 90 ,
    label: 'Service Rating'
    ,type: #AS_DATAPOINT
  } ]
  @UI: { dataPoint: { title: 'Service Rating', targetValue: 5, visualization: #RATING }}
  ServiceRating;
```
Save and activate the Metadata Extension **`ZC_FLEVAL_EX_##`**.

2.	Start the SAP Fiori application in preview by opening the Service Binding **`ZUI_FLEVAL_EX_##_O4`** and clicking **Preview...** for the **FlighEval**. Now you see the asterisks for the meal, flight and service ratings.
   
<br>![](/exercises/ex2/images/dt260_ex2_6_fiori_asterisks.png)

4. Follow the steps on the screenshots below to edit a flight evaluation for a selected customer.

<br>![](/exercises/ex2/images/dt260_ex2_7_edit_eval.png)

5. Add the ratings for the selected customer and save them in the table.
   
<br>![](/exercises/ex2/images/dt260_ex2_8_add_eval.png)

## Summary

You've now ...

Continue to - [Exercise 3 - Excercise 3 ](../ex3/README.md)
