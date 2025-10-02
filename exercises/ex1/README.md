# Exercise 1 - Modernize the Flight Evaluation application with ABAP Cloud

The Flight Evaluation application was developed with classic ABAP and is designed to collect customer feedback for the flights of different flight companies with regard to meal, flight and service quality.
The rating is based on the 0-to-5 scale, with 0 representing the lowest possible score (e.g., "very poor" or "unsatisfactory") and 5 representing the highest (e.g., "outstanding"). 
In this exercise, you will modernize the Flight Evaluation application using ABAP Cloud by transforming its application logic to the ABAP Cloud development model. 

## Exercise 1.1 Get to know the Flight Evaluation application and analyze it for ABAP Cloud with ABAP test cockpit (ATC)

After completing these steps you will have understood how the Flight Evaluation application works and executed ABAP test cockpit (ATC) ABAP Cloud readiness checks to get the ATC result list of the incompatible issues with ABAP Cloud.

1. Start the Flight Evaluation application. Run the ABAP program  **`ZFLIGHT_EVALUATION_EX_##`** by selecting it in the Project Explorer and clicking **F8** button. The program will be started in ABAP development tools for Eclipse embedded in the SAP GUI. Enter the carrier id **LH**, connection id **0400** and flight date **16.02.2025** as proposed on the screenshot and click **Execute** button.
   
   <br>![](/exercises/ex1/images/dt260_ex1_1_start_abap_program.png)

2. The ABAP program **`ZFLIGHT_EVALUATION_EX_##`** will display the customer data (like booking id, customer number, customer name) for the selected flight and flight date along with the customer ratings for meal, flight and service. Double-click on a table row and enter ratings for meal, flight and service for the selected customer on the used flight connection. Play around with the program to get to know it: e.g. you can sort the table and enter further customers evaluations.
   
   <br>![](/exercises/ex1/images/dt260_ex1_2_enter_data_abap_program.png)

3. Now it's time to analyze the Flight Evaluation application for ABAP Cloud. Since the application logic is implemented in the ABAP class **`ZCL_FLIGHT_EVALUATION_EX_##`** using the table **`ZFLEVAL_EX_##`** you would need only to analyze the class. You will leave the ABAP program **`ZFLIGHT_EVALUATION_EX_##`** as it is, since for the purpose of modernization in the upcoming exercises you will provide the Flight Evaluation application with the new SAP Fiori UI and will not need the ABAP program any longer. Select the ABAP class **`ZCL_FLIGHT_EVALUATION_EX_##`** in the Project Explorer and use the context menu **Run As->ABAP Test Cockpit With...** with the **ABAP_CLOUD_READINESS** check variant to execute ABAP Cloud readiness checks.
 
   <br>![](/exercises/ex1/images/dt260_ex1_3_run_atc.png)

4. The ABAP test cockpit check results will be displayed in the ATC Problems View and can be used as a basis for your adaptation tasks to ABAP Cloud.
 
   <br>![](/exercises/ex1/images/dt260_ex1_4_atc_results.png)


## Exercise 1.2 Transform the application logic to the ABAP Cloud

After completing these steps you will have adopted the application logic of the Flight Evaluation application in the ABAP class **`ZCL_FLIGHT_EVALUATION_EX_##`** to ABAP Cloud development model.

1.	Take a look at your ABAP test cockpit (ATC) result list. It contains 10 errors and 4 warnings. Some of the ATC findings there have a yellow light bulb indicating, that such findings can be fixed in a semi-automated way by using quick fixes. First apply all available quick fixes by proceeding as follows. Select all findings in the ATC result list (**Ctrl + A** shortcut) and use the context menu **Recommended Quick Fixes**. The wizard will display all ATC findings, which can be adapted with the quick fixes. Click on the **Next** button. The next wizard screen will display the source code of the class in the original state and after applying the quick fxes. ***OPTIONAL:*** *You can review the changes by using* ***Up*** and ***Down*** *buttons on the right upper side of the editors toolbar of the wizard.* Finally click on the **Finish** button to apply all changes.

   <br>![](/exercises/ex1/images/dt260_ex1_5_run_quick_fixes.png)

2. Save and activate your ABAP class **`ZCL_FLIGHT_EVALUATION_EX_##`** and rerun ATC with the **ABAP_CLOUD_READINESS** check variant. After applying the quick fixes you will have out of 10 errors and 4 warnings only 4 remaining errors in your ATC result list, which you would need now to fix manually.
   
   <br>![](/exercises/ex1/images/dt260_ex1_6_atc_result_after_quick_fixes.png)

3.	Let's take a look at the explanation of the first error by doublie-clicking and displaying it in the **Details** view. The error description states, that your source code calls the function module **GUID_CREATE**, which is not released for ABAP Cloud. Instead of this the succesor API provided by the class **CL_SYSTEM_UUID** must be used.   
   <br>![](/exercises/ex1/images/dt260_ex1_7_fix_guid.png)

   Replace the method **create_guid** with this code.
   
   ```abap
   METHOD create_guid.
    TRY.
        cl_system_uuid=>create_uuid_x16_static(
          RECEIVING
            uuid = guid
        ).
      CATCH cx_uuid_error.
    ENDTRY.
   ENDMETHOD.
   ```
   Save and activate your class and recheck it by ATC using context menu **Recheck**. You will see, that this ATC error was gone and now you have only 3 errors.
   
   <br>![](/exercises/ex1/images/dt260_ex1_8_atc_recheck.png)

4.	Let's take a look at the explanation of the last error in the ATC result list by doublie-clicking and displaying it in the **Details** view. The error description states, that for the flight date your source code uses the **s_date** Data Element, which is not released for ABAP Cloud.
   
   <br>![](/exercises/ex1/images/dt260_ex1_9_atc_date_issue.png)

   To correct this ATC error you will need to use the released for ABAP Cloud **d** Data Element. Correct your code as follows and recheck it with ATC. Only 2 ATC issues will remain in the result.
   
   <br>![](/exercises/ex1/images/dt260_ex1_10_atc_date_issue_fixed.png)

5. By doublie-clicking on remaining ATC issues and displaying them in the **Details** view you will see, that they are caused by direct accesses in the code to the SAP standard tables **SBOOK** (booking table) and **SCUSTOM** (customers table) in the **SELECT**-statement which combines both tables using JOIN-clause to get the data for the customers evaluations table.  ABAP Cloud development rules prescribe that all SAP standard tables must be accessed not directly but by using the SAP released CDS views. But there is no SAP relased CDS view provided as a successor in the details of this error. Therefore you need to create your own CDS view.
   
   <br>![](/exercises/ex1/images/dt260_ex1_11_atc_tables_issues.png)

6. Create your own CDS view **`ZDT260_C_SBOOK_EX_##`** by proceeding as follows. Go to your ABAP package **`ZDT260_EX_##`** in the Project Explorer and using the context menu select **New->Other ABAP Repository Object->Core Data Services**, choose **Data Definition** and click on the **Next** button. Enter the **Name** **`ZDT260_C_SBOOK_EX_##`** and any **Description**. Click further on the **Next** button and choose **defineViewEntity** from the CDS Data Definition templates. Click on the **Finish** button.

   <br>![](/exercises/ex1/images/dt260_ex1_12_create_cds_view.png)

   Replace the source code in the editor window with this code and replace **`##`** with your group number. Save and activate your CDS view.

   ```abap
   @AbapCatalog.viewEnhancementCategory: [#NONE]
   @AccessControl.authorizationCheck: #NOT_REQUIRED
   @EndUserText.label: 'Data Definition'
   @Metadata.ignorePropagatedAnnotations: true
   @ObjectModel.usageType:{
       serviceQuality: #X,
       sizeCategory: #S,
       dataClass: #MIXED
   }
   define view entity ZDT260_C_SBOOK_EX_## as select from sbook   as booking
       inner join   scustom as customer on customer.id = booking.customid
   
   {
     key cast ( booking.carrid as s_carr_id preserving type )        as CarrId,
     key cast ( booking.connid as s_conn_id preserving type )        as ConnId,
     key cast ( booking.fldate as s_date preserving type )           as Fldate,
     key cast ( booking.bookid as s_book_id preserving type )        as BookId,
         cast ( booking.customid as s_customer preserving type )     as CustomId,
         booking.custtype                                            as Custtype,
         booking.smoker                                              as Smoker,
         @Semantics.quantity.unitOfMeasure: 'Wunit'
         booking.luggweight                                          as Luggweight,
         cast ( booking.wunit as s_weiunit preserving type )         as Wunit,
         booking.invoice                                             as Invoice,
         booking.class                                               as Class,
         @Semantics.amount.currencyCode: 'Forcurkey'
         booking.forcuram                                            as Forcuram,
         cast ( booking.forcurkey as s_currcode preserving type )    as Forcurkey,
         @Semantics.amount.currencyCode: 'Loccurkey'
         booking.loccuram                                            as Loccuram,
         cast ( booking.loccurkey as s_currcode preserving type )    as Loccurkey,
         booking.order_date                                          as OrderDate,
         cast ( booking.counter as s_countnum preserving type )      as Counter,
         cast ( booking.agencynum   as s_agncynum  preserving type ) as Agencynum,
         booking.cancelled                                           as Cancelled,
         booking.reserved                                            as Reserved,
         booking.passname                                            as Passname,
         booking.passform                                            as Passform,
         booking.passbirth                                           as Passbirth,
         customer.name                                               as Name
   }
   ```
7. Replace the **SELECT FROM SBOOK**-statement with this code, using your new CDS view, and replace **`##`** with your group number.

   ```abap
       SELECT FROM zdt260_c_sbook_ex_## AS booking FIELDS *
         WHERE booking~carrid = @i_carrid
           AND booking~connid = @i_connid
           AND booking~fldate = @i_fldate
           INTO CORRESPONDING FIELDS OF TABLE @it_eval.
   ```

   Save and activate your class and execute **Recheck** on your ATC result list. You will see, that all your ATC findings were resolved and the ATC result list is now empty.

   <br>![](/exercises/ex1/images/dt260_ex1_13_replace_cds.png)

8. Run the Flight Evaluation application again to verify that it works exactly as before by executing the ABAP program  **`ZFLIGHT_EVALUATION_EX_##`** and repeating the steps 1-2 from the **Exercise 1.1**.

## Exercise 1.3 Move the ABAP Cloud ready development objects to the ABAP Cloud development package

After completing these steps you will have switched the ABAP language version to *ABAP for Cloud Development* and moved your ABAP Cloud ready development objects to your ABAP Cloud package **`ZDT260_EX_##_5`**. Before you move your ABAP Cloud ready artifacts to your ABAP Cloud development package, you need to switch their language version to *ABAP for Cloud Development*.
   
1. Open **Properties** view (menu **Window->Show View->Properties**). Display the ABAP class **`ZCL_FLIGHT_EVALUATION_EX_##`** and the table **`ZFLEVAL_EX_##`** subsequently in the **Properties** view (context menu in the project explorer **Show In -> Properties**) and on the ***General*** tab switch the language version using the **Edit..** button to *ABAP for Cloud Development*. Save and activate.

   <br>![](/exercises/ex1/images/dt260_ex1_14_switch_lgv_5.png)


2. Before moving your development artifacts you need to release your transports, containing them. For this open the Transport Organizer using menu **Window -> Show view -> Other...** and selecting **ABAP -> Transport Organizer**. Release your transports.

   <br>![](/exercises/ex1/images/dt260_ex1_17_release_transports.png)
   
3.	Move the ABAP class **`ZCL_FLIGHT_EVALUATION_EX_##`** and the table **`ZFLEVAL_EX_##`** to the ABAP Cloud development package **`ZDT260_EX_##_5`** by selecting subsequently the context menu **Change Package Assignment...** and providing **`ZDT260_EX_##_5`**
as a new package on the wizard. Clicking the **Next** button will complete the package assignement.

   <br>![](/exercises/ex1/images/dt260_ex1_15_chg_pkg_assign_cloud.png)

4.	As a result your ABAP Cloud ready artifacts the ABAP class **`ZCL_FLIGHT_EVALUATION_EX_##`** and the table **`ZFLEVAL_EX_##`** are now moved to the ABAP Cloud development package **`ZDT260_EX_##_5`** whereby the ABAP program  **`ZFLIGHT_EVALUATION_EX_##`** and the CDS view **`ZDT260_C_SBOOK_EX_##`** remain in the standard ABAP package **`ZDT260_EX_##`**

   <br>![](/exercises/ex1/images/dt260_ex1_16_chg_pkg_assign_cloud_result.png)

5. Now activate the table **`ZFLEVAL_EX_##`** and the ABAP class **`ZCL_FLIGHT_EVALUATION_EX_##`**. While activating the class you will get the error message stating hat the class has syntax errors and requestng you to run syntax check. Execute syntax check for the ABAP class **`ZCL_FLIGHT_EVALUATION_EX_##`** (shortcut **Ctrl + F2**) and take a look at the syntax errors. They all are caused by accesses to the CDS view **`ZDT260_C_SBOOK_EX_##`**. The rules of ABAP Cloud development prescribe, that ABAP Cloud artefacts can only access development objects, which are released for ABAP Cloud. The CDS view must be released for internal ABAP Cloud consumption in the system. Let's do it in the next step.

   <br>![](/exercises/ex1/images/dt260_ex1_14_activate_syntax_errors.png)
   
6. Go to your CDS view **`ZDT260_C_SBOOK_EX_##`** in the project explorer and click the context menu **API State -> Add Use System-Internally (Contract C1)...** . Follow the wizard by clicking the **Next** btton and **Finish** to release the CDS view for use in ABAP Cloud development internally in this development system.

   <br>![](/exercises/ex1/images/dt260_ex1_13_release_cds.png)
   
7. Finally you can activate the ABAP class **`ZCL_FLIGHT_EVALUATION_EX_##`**.
   
## Summary

You've now got to know the SAPGUI-based Flight Evaluation application, analyzed its application logic for ABAP Cloud readiness, adapted the related development artifacts to the ABAP Cloud, created your own custom CDS view for missing SAP released CDS view for ABAP Cloud and moved ABAP Cloud artifacts to the ABAP Cloud development package.
Continue to - [Exercise 2 - Create SAP Fiori application with ABAP Cloud and RAP for Flight Evaluation
](../ex2/README.md)

