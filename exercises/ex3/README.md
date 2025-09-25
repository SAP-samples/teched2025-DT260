# Exercise 3 - Analyze the Customer Dashboard application for clean core

The Customer Dashboard application belongs to the the Flight Evaluation application. It was developed with classic ABAP and is designed to display the data of the specific customer along with the collected flight feedback. This application should not be modernized with ABAP Cloud but must become clean core compliant.
In this exercise, you will analyze the Customer Dashboard application for ABAP Cloud and clean core, learn to understand clean core issues and why the new clean core ABAP test cockpit (ATC) checks significantly reduce the number of findings.

## Exercise 3.1 Analyze Customer Dashboard application for ABAP Cloud with ABAP test cockpit (ATC)

After completing these steps you will have understood how the Customer Dashboard application works and executed ABAP test cockpit (ATC) ABAP Cloud readiness checks to get the ATC result list of the incompatible issues with ABAP Cloud.

1. Start the Customer Dashboard application. Run the ABAP program  **`ZCUSTOMER_DASHBOARD`** by selecting it in the project explorer and clicking **F8** button. The program will be started in ABAP development tools for Eclipse embedded in the SAP GUI. Enter the customer id **27** as proposed on the screenshot and click **Execute** button. The program will display customer data along with the flight information and flight feedback.
   
   <br>![](/exercises/ex3/images/dt260_ex3_1_start_abap_program.png)
   
2. Now it's time to analyze the Customer Dashboard application for ABAP Cloud. Select the ABAP program  **`ZCUSTOMER_DASHBOARD`** in the project explorer and use the context menu **Run As->ABAP Test Cockpit With...** with the **ABAP_CLOUD_READINESS** check variant to execute ABAP Cloud readiness checks. You will get 15 issues. Take a look at them in detail and try to understand. Some examples are provided on the screenshots below. These ATC issues occur because of usage of the usage of SAP object types like programs (PROG), or ABAP language constructs (REPORT), or not released SAP APIs and frameworks like ALV Grid (CL_SALV_TABLE) which are nor supported in ABAP Cloud.  
 
   <br>![](/exercises/ex3/images/dt260_ex3_2_analyze_atc_cloud_issues.png)
   
All these ATC findings must be fixed if adapting the custom code to ABAP Cloud but the Customer Dashboard application must just become clean core compliant, therefore the ATC check procedure must be changed to clean core.  
## Exercise 3.2 Execute new clean core checks with ABAP test cockpit (ATC) and get to know Cloudification Repository Viewer
dfdf
dfdf
dfdf
## Exercise 3.3 Understand clean core ABAP test cockpit (ATC) issues
dfdf
dfdf
dfd
## Summary
