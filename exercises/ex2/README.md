# Exercise 2 - Create SAP Fiori application for Flight Evaluation with ABAP Cloud and RAP

In this exercise, you will build the modern SAP Fiori application with ABAP Cloud using RAP, We will use a managed scenario with unmanged save thereby leveraging the Flight Evaluation application logic, transformed to ABAP Cloud in the Exercise 1. The RAP managed scenario with an unmanaged save  is perfectly suited for migration use cases from classic ABAP since it gives you more control over the data persistence and business logic compared to the pure RAP managed approach. You can however still leverage the advantages of the managed runtime since this will take care of the draft handling which is always purely managed.

## 🔴 Important Information

> **📌 Note**   
> We’ve got sessions running in several locations → please pick the one that’s right for you!

> **📌 Replace the two digits to get your group number:**
> * ZDT260_EX_**##** → **01** → **40** → **SAP TechEd Berlin**  (e.g ``ZDT260_EX_19``)
> * ZDT260_EX_**6##** → **600** → **699** → **ASUG Tech-Connect**  (e.g ``ZDT260_EX_623``)


## Exercise 2.1 Generate SAP Fiori RAP application for Flight Evaluation

After completing these steps you will have created the modern SAP Fiori RAP-based Flight Evaluation application.

1. Generate SAP Fiori application on top of the flight evaluations table **`ZFLEVAL_EX_##`** by creating OData UI service. Follow the generator wizard steps on the screenshots below and enter the alias name **FlightEval** for the CDS data definition. Finally open the service binding as prompted in the last popup screen.
   
<br>![](/exercises/ex2/images/dt260_ex2_1_create_odata_service.png)

2.	Click on the **Publish** button, activate the Service Binding **`ZUI_FLEVAL_EX_##_O4`**, select the **FlightEval** and click on the **Preview...**.

<br>![](/exercises/ex2/images/dt260_ex2_11_publsh_odata_service.png)
   
3. The generated SAP Fiori Flight Evaluation application starts. Click on the **Go** button to see the flight evaluations. Currently only the flight evaluations, which you created in the Exercise 1 can be displayed, you cannot add flight evaluations for different airlines en masse, and if you click on the table row to edit a flight evaluation for the customer, then your changes will not be stored in your flight evaluations table **`ZFLEVAL_EX_##`**.
   
<br>![](/exercises/ex2/images/dt260_ex2_2_fiori_display_evaluations.png)

## Exercise 2.2 Add the functionality to enter a flight rating for a customer

After completing these steps, you will be able to enter and store customer flight ratings in the SAP Fiori Flight Evaluation application.

1.	First open the Behavior Definition **`ZR_FLEVAL_EX_##`** and change the authorizations so that **CustomID** and **Name** can not be changed as shown on the screenshot below.  

<br>![](/exercises/ex2/images/dt260_ex2_45_create_update_auth.png)

2.	Let's implement an **Unmanaged Save** because we want to reuse our existing class **`ZCL_FLIGHT_EVALUATION_EX_##`**. In the Behavior Definition **`ZR_FLEVAL_EX_##`** add the source code line **with unmanaged save** to let you (not the RAP framework) manage the table **`ZFLEVAL_EX_##`** with flight evaluations. You will get the error message, telling that it is not possible to specify the persistent table and use unmanaged scenario at the same time. Follow the steps on the screenshots below to comment out the **persistent table**-statement and apply the quick fixes (**Ctrl + 1** shortcut) to generate the required **save_modified** method in the local handler class of the corresponding Behavior Implementation class **`ZBP_FLEVAL_EX_##`**.

<br>![](/exercises/ex2/images/dt260_ex2_3_add_unmanaged_save.png)

3.	Now add the functionality that allows users to enter a flight rating for a customer and save it in the **`ZFLEVAL_EX_##`** flight evaluation table. For this add the following code to the **save_modified** method of the local handler class of your  Behavior Implementation class **`ZBP_FLEVAL_EX_##`**, replace **`##`** with your group number in your code, save and activate it.
   
```abap

    IF create IS NOT INITIAL.

    ELSEIF update IS NOT INITIAL.
      LOOP AT update-flighteval INTO DATA(ls_update).
        DATA eval_obj TYPE REF TO zcl_flight_evaluation_ex_##.
        eval_obj = NEW zcl_flight_evaluation_ex_##(
            i_carrid = ls_update-CarrID
            i_connid = ls_update-ConnID
            i_fldate = ls_update-Fldate
            i_bookid = ls_update-BookID
        ).
        eval_obj->set_flight_rating( ls_update-FlightRating ).
        eval_obj->set_meal_rating( ls_update-MealRating ).
        eval_obj->set_service_rating( ls_update-ServiceRating ).

        eval_obj->save_on_db( ).
      ENDLOOP.
    ELSEIF delete IS NOT INITIAL.
    ELSE. 
    ENDIF.

```

4. Start the SAP Fiori application in preview by opening the Service Binding **`ZUI_FLEVAL_EX_##_O4`** and clicking **Preview...** for the **FlighEval**. Now you can enter flight rating data for a customer and it will be stored in your flight evaluations table **`ZFLEVAL_EX_##`**.
   
## Exercise 2.3 Add the functionality to add new flight evaluation data 

After completing these steps, you will be able to create new flight evalations in the SAP Fiori Flight Evaluation application. For this the new button **Create Flight Eval Data** will be added to the UI.

1. Add the logic to enter the flight evaluation data by adding the local handler class **lcl_buffer** to the Behavior Implementation class **`ZBP_FLEVAL_EX_##`** (under the tab **Local Types** of the editor bottom bar)
   
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
2. Use the **lcl_buffer** class in the **save_modified** method by adding this code to the **ELSE** clause of the **IF**-statement. Replace **`##`** in the code with your group number.
```abap
   LOOP AT lcl_buffer=>buffer INTO DATA(buffer).
     DATA(lt_evaluation_data) =
         zcl_flight_evaluation_ex_##=>create_flight_evaluation( i_carrid = buffer-carrid
                                                                i_connid = buffer-connid
                                                                i_fldate = buffer-fldate ).
   ENDLOOP.
```
<br>![](/exercises/ex2/images/dt260_ex2_44_use_lcl_buffer.png)

Save and activate the Behavior Implementation **`ZBP_FLEVAL_EX_##`**.

3.	Add the following code to your Behavior Definition **`ZR_FLEVAL_EX_##`** save and activate.
   ```abap
  static action createFlightEval parameter ZDT260_A_FLIGHTEVAL_5;
   ```
4. You will get the syntax error telling that the method **createFlightEval** must be added for the action. Use quick fix (**Ctrl + 1** shortcut) to generate the method in the local handler class of the correspondng Behavior Implementation class **`ZBP_FLEVAL_EX_##`**. 

   <br>![](/exercises/ex2/images/dt260_ex2_4_add_static_action.png)

5. Add the following code as the  **createFlightEval** method implementation.
```abap
    LOOP AT keys INTO DATA(key).
      INSERT VALUE #( carrid = key-%param-CarrId
                      connid = key-%param-ConnId
                      fldate = key-%param-fldate ) INTO TABLE lcl_buffer=>buffer.
    ENDLOOP.
```
   
6.	Now use the action by adding the following code to the Behavior Definition **`ZC_FLEVAL_EX_##`**.
  ```abap
  use action createFlightEval;
   ```
<br>![](/exercises/ex2/images/dt260_ex2_41_use_static_action.png)

7.	To see the **Create Flight Eval Data** button on the UI you need to add the following code to the Metadata Extension **`ZC_FLEVAL_EX_##`**.
  ```abap
   ,{ type: #FOR_ACTION, dataAction: 'createFlightEval', label: 'Create Flight Eval Data' } 
   ```
<br>![](/exercises/ex2/images/dt260_ex2_42_use_static_action_mde.png)

8. Start the SAP Fiori application in preview by opening the Service Binding **`ZUI_FLEVAL_EX_##_O4`** and clicking **Preview...** for the **FlighEval**. Now you can fill the flight evaluations table with additional flight evaluations from the SAP standard SBOOK table. Just follow the steps on the screenshots below.
   
<br>![](/exercises/ex2/images/dt260_ex2_5_fiori_create_evaldata_button.png)

## Exercise 2.4 Change visualisation towards star rating of flights

After completing these steps the SAP Fiori Flight Evaluation application will be able to display star ratings for meal, flight and service quality of flights.

1.	Open the Metadata Extension **`ZC_FLEVAL_EX_##`** and replace the source code after the **Name** for meal, flight and service ratings with the following code:
   
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

2.	Start the SAP Fiori application in preview by opening the Service Binding **`ZUI_FLEVAL_EX_##_O4`** and clicking **Preview...** for the **FlighEval**. Now you see the stars for the meal, flight and service ratings.
   
<br>![](/exercises/ex2/images/dt260_ex2_6_fiori_asterisks.png)

4. Follow the steps on the screenshots below to edit a flight evaluation for a selected customer.

<br>![](/exercises/ex2/images/dt260_ex2_7_edit_eval.png)

5. Add the ratings for the selected customer and save them in the table.
   
<br>![](/exercises/ex2/images/dt260_ex2_8_add_eval.png)

## Summary

You have now built a modern SAP Fiori Flight Evaluation application with ABAP Cloud and RAP unmanaged scenario and unmanaged save reusing the logic from the SAPGUI-based Flight Evaluation application transformed to ABAP Cloud. You can display flight evaluations, create new flight evaluations and edit customer ratings for the meal, flight and service.
Continue to - [Exercise 3 - Analyze the Customer Dashboard application for clean core](../ex3/README.md)

