# Exercise 3 - Analyze the Customer Dashboard application for clean core

The Customer Dashboard application belongs to the the Flight Evaluation application. It was developed with classic ABAP and is designed to display the data of the specific customer along with the collected flight feedback. This application should not be modernized with ABAP Cloud but must become clean core compliant.
In this exercise, you will analyze the Customer Dashboard application for ABAP Cloud and clean core, learn to understand clean core issues and why the new clean core ABAP test cockpit (ATC) checks significantly reduce the number of findings.

## Exercise 3.1 Analyze Customer Dashboard application for ABAP Cloud with ABAP test cockpit (ATC)

After completing these steps you will have understood how the Customer Dashboard application works and executed ABAP test cockpit (ATC) ABAP Cloud readiness checks to get the ATC result list of the incompatible issues with ABAP Cloud.

1. Start the Customer Dashboard application. Run the ABAP program  **`ZCUSTOMER_DASHBOARD`** by selecting it in the project explorer and clicking **F8** button. The program will be started in ABAP development tools for Eclipse embedded in the SAP GUI. Enter the customer id **27** as proposed on the screenshot and click **Execute** button. The program will display customer data along with the flight information and flight feedback.
   
   <br>![](/exercises/ex3/images/dt260_ex3_1_start_abap_program.png)
   
2. Analyze the Customer Dashboard application for ABAP Cloud. Select the ABAP program  **`ZCUSTOMER_DASHBOARD`** in the project explorer and use the context menu **Run As->ABAP Test Cockpit With...** with the **ABAP_CLOUD_READINESS** check variant to execute ABAP Cloud readiness checks. You will get 15 issues. Take a look at them in detail and try to understand. Some examples are provided on the screenshots below. These ATC issues occur because of usage of the usage of SAP object types like programs (PROG), or ABAP language constructs (REPORT), or not released SAP APIs and frameworks like ALV Grid (CL_SALV_TABLE) which are nor supported in ABAP Cloud.  
 
   <br>![](/exercises/ex3/images/dt260_ex3_2_analyze_atc_cloud_issues.png)
   
All these ATC findings must be fixed if adapting the custom code to ABAP Cloud but the Customer Dashboard application must just become clean core compliant, therefore the ATC check procedure must be changed to clean core.  

## Exercise 3.2 Analyze Customer Dashboard application for clean core with ABAP test cockpit (ATC) and get to know Cloudification Repository Viewer

After completing these steps you will have learned how to execute clean core ATC checks and to work with the Cloudification Repository Viewer.

1. Now it's time to analyze the Customer Dashboard application for clean core. Select the ABAP program  **`ZCUSTOMER_DASHBOARD`** in the project explorer and use the context menu **Run As->ABAP Test Cockpit With...** with the **ZDT260_CLEAN_CORE_DEVEOPMENT** check variant to execute clean core checks. You will get now just 1 error, 2 warnings and 3 information ATC issues.
   
     <br>![](/exercises/ex3/images/dt260_ex3_4_run_atc_clean_core.png)
   
 Some check results indicate usage of **classic APIs** and **internal APIs** in the code. This is because the new clean core check **Usage of APIs** is based on the usage guidelines for APIs. For usage of released SAP APIs no ATC findings are reported, for usage of an SAP classic APIs the check reports an info message, for usage of an API which is not classified (internal API) the check reports a warning and for usage of an API marked as **no API** an error message is reported. 

3. SAP offers recommendations for release independent upgrade stable SAP APIs (classic APIs), which shall be used classic ABAP developments and provides the list of classic APIs  as **objectClassifications_SAP.json** on the **GitHub - SAP/abap-atc-cr-cv-s4hc**. This information is used by the ATC to govern the usage of classic APIs in custom code developments. The classic APIs are based on the SAP recommendations for classic ABAP development technologies, reuse services and application specific frameworks, which should be utilized in classic ABAP developments (see the SAP Note 3578329 for more details).

Let's inspect classic APIs using the Cloudification Repository viewer on GitHub. For this click on the link below to open the viewer: [https://sap.github.io/abap-atc-cr-cv-s4hc/?version=objectClassifications_SAP.json](https://sap.github.io/abap-atc-cr-cv-s4hc/?version=objectClassifications_SAP.json).

4. After clicking on the **Show Filter Bar** you can filter the display. Let's find the ALV Grid classic API **CL_SALV_TABLE**, which was reported by the clean core ATC checks. For this select state *Classic API*, Software Component *SAP_BASIS* and Application Component *BC-SRV-ALV* and you will get the list of all ALV Grid classic APIs including the **CL_SALV_TABLE**.

   <br>![](/exercises/ex3/images/dt260_ex3_7_cloud_repo_alvgrid.png)

5. 
   
## Exercise 3.3 Understand clean core ABAP test cockpit (ATC) issues
dfdf
dfdf
dfd
## Summary
