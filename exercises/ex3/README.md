# Exercise 3 - Analyze the Customer Dashboard application for clean core

The Customer Dashboard application is content-wise connected with the Flight Evaluation application. It was developed with classic ABAP and is designed to display the data of the specific customer along with the collected flight feedback. This application should not be modernized with ABAP Cloud but must become clean core compliant.
In this exercise, you will analyze the Customer Dashboard application for ABAP Cloud and clean core, learn to interpret clean core issues and understand why the new clean core ABAP test cockpit (ATC) checks significantly reduce the number of findings.

## Exercise 3.1 Analyze Customer Dashboard application for ABAP Cloud with ABAP test cockpit (ATC)

After completing these steps you will have understood how the Customer Dashboard application works and executed ABAP Cloud readiness checks with ABAP test cockpit (ATC) to get the ATC result list of the incompatible issues with ABAP Cloud.

1. Start the Customer Dashboard application. For this run the ABAP program  **`ZCUSTOMER_DASHBOARD`** by selecting it in the project explorer and clicking **F8** button. The program will be started in ABAP development tools for Eclipse embedded in the SAP GUI. Enter the customer id **27** as proposed on the screenshot and click **Execute** button. The program will display customer data along with the flight information and flight feedback.
   
   <br>![](/exercises/ex3/images/dt260_ex3_1_start_abap_program.png)
   
2. Analyze the Customer Dashboard application for ABAP Cloud. Select the ABAP program  **`ZCUSTOMER_DASHBOARD`** in the project explorer and use the context menu **Run As->ABAP Test Cockpit With...** with the **ABAP_CLOUD_READINESS** check variant to execute ABAP Cloud readiness checks. You will get 15 issues. Take a look at them in detail and try to understand. Some examples are provided on the screenshots below. These ATC issues occur because of the usage of SAP object types like programs (PROG), or ABAP language constructs (REPORT), or not released SAP APIs and frameworks like ALV Grid (CL_SALV_TABLE) which are nor supported in ABAP Cloud.  
 
   <br>![](/exercises/ex3/images/dt260_ex3_2_analyze_atc_cloud_issues.png)
   
For the adaptation of the code to ABAP Cloud all these ATC findings must be fixed. But in this exercise the Customer Dashboard application must just become clean core compliant, therefore the ATC check procedure must be changed to clean core.  

## Exercise 3.2 Analyze Customer Dashboard application for clean core with ABAP test cockpit (ATC) and get to know Cloudification Repository Viewer

After completing these steps you will have learned how to execute clean core ATC checks and to work with the Cloudification Repository Viewer.

1. Now it's time to analyze the Customer Dashboard application for clean core. Select the ABAP program  **`ZCUSTOMER_DASHBOARD`** in the project explorer and use the context menu **Run As->ABAP Test Cockpit With...** with the **ZDT260_CLEAN_CORE_DEVEOPMENT** check variant to execute clean core checks. 
   
     <br>![](/exercises/ex3/images/dt260_ex3_4_run_atc_clean_core.png)
   
   You will get now musch less ATC issues: just 1 error, 2 warnings and 3 information messages:

   <br>![](/exercises/ex3/images/dt260_ex3_5_atc_clean_core_issues.png) 
   
 Some check results indicate usage of **classic APIs** and **internal APIs** in the code. This is because the new clean core check **Usage of APIs** is based on the clean core usage guidelines for SAP APIs. For usage of released SAP APIs no ATC findings are reported, for usage of an SAP classic APIs the check reports an info message, for usage of an API which is not classified (internal API) the check reports a warning and for usage of an API marked as **no API** an error message is reported. 

3. SAP offers recommendations for release independent upgrade stable SAP APIs (classic APIs), which shall be used in classic ABAP developments and provides the list of classic APIs  as **objectClassifications_SAP.json** on the **GitHub - SAP/abap-atc-cr-cv-s4hc**. This information is used by the ATC to govern the usage of classic APIs in custom code developments. The classic APIs are based on the SAP recommendations for classic ABAP development technologies, reuse services and application specific frameworks, which should be utilized in classic ABAP developments (see the SAP Note [3578329](https://me.sap.com/notes/3578329) for more details).

Let's inspect classic APIs using the Cloudification Repository viewer on GitHub. For this click on the link below to open the viewer: [https://sap.github.io/abap-atc-cr-cv-s4hc/?version=objectClassifications_SAP.json](https://sap.github.io/abap-atc-cr-cv-s4hc/?version=objectClassifications_SAP.json).

4. After clicking on the **Show Filter Bar** you can filter the display. Let's find the ALV Grid classic API **CL_SALV_TABLE**, which was reported by the clean core ATC checks. For this select state *Classic API*, Software Component *SAP_BASIS* and Application Component *BC-SRV-ALV* and you will get the list of all ALV Grid classic APIs including the **CL_SALV_TABLE**.

   <br>![](/exercises/ex3/images/dt260_ex3_7_cloud_repo_alvgrid.png)

5. Now display all classic BAPIs available in the area of Material Management - Purchase Requisition. For this select State *Classic API*, Object Type *FUNC*, Software Component *S4CORE* and Application Component *MM-PUR-REQ*.

   <br>![](/exercises/ex3/images/dt260_ex3_8_cloud_repo_bapi.png)

6. Now display all **No API** available in the area of Material Management - Purchasing - Goods Receipts. For this select State *No API*, Software Component *S4CORE* and Application Component *MM-PUR-GF*.

    <br>![](/exercises/ex3/images/dt260_ex3_11_cloud_repo_noapi.png)
   
7. The SAP objects, which are classified as **No API** should not be used in custom code and replaced by SAP released APIs or classic APIs (if successor is available, it is listed in the object classification JSON file). Click on the table row with the API **BAPI_PO_CREATE". You will see that it has successor **BAPI_PO_CREATE1** (click on that table row), which is classic API.

    <br>![](/exercises/ex3/images/dt260_ex3_12_cloud_repo_successor.png)

8. Play auround with the Cloudification Repository Viewer looking for the APIs, for example look for the API which you are familiar with, to verify if it was nominated as a classic API. By the way you can also choose another repository under **Repository Selection** and look for example for released APIs for the particular SAP S/4HANA edition.
     
## Exercise 3.3 Understand clean core ABAP test cockpit (ATC) issues

After completing these steps you will have learned how the clean core ATC check variant looks like and how to understand clean core ATC check results.

1. Open the ATC clean core check variant **ZDT260_CLEAN_CORE_DEVEOPMENT** (Open ABAP development Object (Ctrl + Shift + A)) and take a look at it. This check variant contains all necessary clean core ATC checks. You will see, that the **Usage of APIs** check is using as input for the classic APIs the **objectClassifications_SAP.json** which you inspected with the Cloudification Repository Viewer in the Exercise 3.2. You can learn how to set up the clean core ATC check variant for your own custom development in the blog post [ABAP test cockpit (ATC) recommendations for governance of clean core ABAP development](https://community.sap.com/t5/technology-blog-posts-by-sap/abap-test-cockpit-atc-recommendations-for-governance-of-clean-core-abap/ba-p/14186130).

   <br>![](/exercises/ex3/images/dt260_ex3_9_cloud_cc_atc_variant.png)

2. Take a look again at your last clean core ATC check results executed with the variant **ZDT260_CLEAN_CORE_DEVEOPMENT** in the Exercise 3.2. To make the code clean core compliant you would need to resolve the ATC clean core issues in the following way. Instead of update on the SAP table use released or classic SAP API, read access to the SAP table should be replaced with released CDS view or classic API, usages of classic APIs are clean core conform. More recommendations are provided in the chapter 5 of the [ABAP Extensibility Guide](https://www.sap.com/documents/2022/10/52e0cd9b-497e-0010-bca6-c68f7e60039b.html).
   
    <br>![](/exercises/ex3/images/dt260_ex3_10_analyze_cc_atc_findings.png)
   

## Summary
You've now got to know the SAPGUI-based Customer Dashboard application, analyzed it for ABAP Cloud readiness and for clean core using ABAP test cockpit (ATC) and experienced how the ATC errors go down when using the new ATC clean core checks based on usage guidelines for SAP APIs instead of ABAP Cloud readiness checks based on usage of released APIs. You have learned how to work with the Cloudification Repository on the GitHub and look up the classic APIs. Finally you have received the guidance how to fix clean core ATC issues.
