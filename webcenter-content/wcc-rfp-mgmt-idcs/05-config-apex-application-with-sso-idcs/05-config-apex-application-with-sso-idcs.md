# Configure APEX Application with SSO/IDCS

## Introduction

In this lab, you learn to install and run Oracle APEX Application for RFP Management System.

This lab also covers updating the APEX Rest Datasource urls & credentials for connecting to WebCenter Content Instance

**Estimated Lab Time**: *20 minutes*

### Objectives

In this lab, you will:

- Navigate through Oracle APEX
- Create New Workspace
- Login to Workspace and Install Packaged Application.
- Update Rest Data sources URL and credentials

### Prerequisites

This lab assumes you have:

- A  Paid or LiveLabs Oracle Cloud account
- You have completed:
  - Lab: Prepare Setup ( *Paid Tenants* only)
  - Lab: Setup WCC Marketplace Environment
  - Lab: Initialize Environment

## Task 1: Create New IDCS SAML Client App for Apex Application Login

To create new IDCS SAML Client App, you need log in to IDCS Admin Console with the user having Administrator Privilege

1. On the new *web browser* window , Login to the IDCS Admin Console URL as **IDCS Administrator** User. Details are provided below
    - **URL**
          ```
          <copy>https://idcs-<tenancy-id>.identity.oraclecloud.com/ui/v1/adminconsole</copy>
          ```

         > Note : Replace `"<tenancy-id>"` with your tenancy id for IDCS URL

    - **Username** - same as the user provided during the provisioning of WCC Marketplace instance
          ```
          <copy>wccadmin</copy>
          ```
    - **Password**
          ```
          <copy>WelCwcm123##</copy>
          ```
  ![This image shows the IDCS Admin Console Login Page](./images/01_idcs_saml_client_create_step1.png "IDCS Admin Console Login Page")

2. In the landing page for the Identity Domain on OCI, click on **Take me there** button to open the Identity Domain Admin Console page
  ![This image shows the IDCS Admin Console Landing Page](./images/01_idcs_saml_client_create_step1_2.png "IDCS Admin Console Landing Page")

3. In the **Identity Domain** Page, Click on **Integrated applications** and then **Add application** button
  ![This image shows the Identity Domain Integrated applications Page](./images/01_idcs_saml_client_create_step1_3.png "Identity Domain Integrated applications Page")

4. In the **Add application** Page, Click on **SAML Application** and then **Launch Workflow** button
  ![This image shows the Add application - SAML Application Page](./images/01_idcs_saml_client_create_step1_4.png "Add application - SAML Application Page")

5. Provide the values for New SAML Application as mentioned below and click **Next** Button
    - **Name**: Enter
          ```
          <copy>WCCRFPMGMT_SAML_App</copy>
          ```
    - **Description**: Enter
          ```
          <copy>SAML App for WCC RFP Management Apex Application</copy>
          ```
    - **Authentication and authorization**: Select ***Enforce grants as authorization***
  ![This image shows the Add SAML Application Page](./images/01_idcs_saml_client_create_step1_5_1.png "Add SAML Application Page")
  ![This image shows the Add SAML Application Page](./images/01_idcs_saml_client_create_step1_5_2.png "Add SAML Application Page")

6. In the **Configure single sign-on** section, Click on **Download signing certificate** and **Download identity provider metadata** to download those files & Provide the Entity ID and Assertion consumer URL details as mentioned below *(it would be same for both Entity ID and Assertion consumer URL)*
    - **Entity ID**: Enter
          ```
          <copy>https://localhost:16200/ords/apex_authentication.saml_callback</copy>
          ```
    - **Assertion consumer URL**: Enter
          ```
          <copy>https://localhost:16200/ords/apex_authentication.saml_callback</copy>
          ```
      > Note : Replace `"https://localhost"` with your **hosturl** ( eg: `"http://wcc-rfpmgmt-livelab.livelabs.oraclevcn.com"` or `"https://192.0.0.0"`)

   ![This image shows the Configure single sign-on Page](./images/01_idcs_saml_client_create_step1_6.png "Configure single sign-on Page")

7. Provide/Update the below values:
      - **Name ID format** :  Select ***Persistent*** from the dropdown list

      - **Name ID** :  Select ***Username*** from the dropdown list

      - **Signing Certificate** :  Upload the ***WCC\_IDCS\_OAUTH\_SIGNIN\_CERT\_FILE*** file  ( Please refer **Appendix 1: Obtain IDCS Client ID, Certificate and Private Key from the Weblogic Credentials** - Section **Obtain IDCS Certificate** on how to retrieve/obtain this file )

   ![This image shows the Add SAML Application Page](./images/01_idcs_saml_client_create_step1_7.png "Add SAML Application Page")

8. In the **Additional configurations** section, provide/update the below values and click **Finish** button
      - **Signed SSO** :  Select ***Assertion and response*** from the dropdown list

      - Checkbox **Include signing certificate in signature** :  ***Checked / Selected***

      - **Signature hashing algorithm** :  Select ***SHA-256*** from the dropdown list

      - Checkbox **Enable single logout** :  ***Un-Checked / Un-Selected***

   ![This image shows the Add SAML Application Page](./images/01_idcs_saml_client_create_step1_8.png "Add SAML Application Page")

9. In the **Application Information** Section, Click on **Copy** link next to the **Application ID** to copy the ID (This will be referred as ***WCC\_IDCS\_WCCRFPMGMT\_SAML\_APP\_ID*** )  and then click on **Activate** button
  ![This image shows the Add application - SAML Application Page](./images/01_idcs_saml_client_create_step1_9.png "Add application - SAML Application Page")

      In the **Activate application** confirmation pop-up dialog box, Click on **Activate Application** button.
      ![This image shows the Activate application - SAML Application Page](./images/01_idcs_saml_client_create_step1_9_2.png "Activate application - SAML Application Page")

10. After the Application is **ACTIVE**, in the **Resources** list, click on **Groups** and click on **Assign groups** button. In the **Assign groups** list, select the below list of group names and click on **Assign** button
      - **admin**
      - **contributor**
      - **guest**
      - **sysmanager**

   ![This image shows the Add Groups Page](./images/01_idcs_saml_client_create_step1_10.png "Add Groups Page")

## Task 2: Configure SAML Sign-In in APEX

To log in to Oracle APEX, you need a Workspace Name, username, and the password created for that Workspace. In this hands-on lab, you log in to your Oracle APEX Workspace.

1. To login to your Oracle APEX Workspace, perform the following steps:
    - Open your browser and enter the **URL** to sign in to the APEX development environment.
      - **URL**
            ```
            <copy>https://localhost:16200/ords/</copy>
            ```

            > Note : Replace `"https://localhost"` with your **hosturl** ( eg: `"https://wcc-rfpmgmt-livelab.livelabs.oraclevcn.com"` or `"https://192.0.0.0"`)

    - The login page appears. Enter the **Workspace Name, Username, and Password**. Click **Sign In**.
      - **Workspace Name**: Enter
            ```
            <copy>WCCRFPMGMT</copy>
            ```
      - **Username**: Enter
            ```
            <copy>ADMIN</copy>
            ```
      - **Password**: Enter
            ```
            <copy>Welcome1</copy>
            ```
  ![Workspace Login](./images/apex_login_workspace_step1.png "Login to APEX Workspace")

2. If its first time Login, Change password for the ADMIN user. You can provide the same Password value and click on **Change Password**.
    > Skip this step if it does not prompt for Change Password

    - **New Password**: Enter
          ```
          <copy>Welcome1</copy>
          ```
    - **Confirm Password**: Enter
          ```
          <copy>Welcome1</copy>
          ```
  ![Reset Password on First Login Page](./images/apex_login_workspace_step2.png "Reset Password on First Login")

3. The Workspace home page appears.

  ![Workspace Home Page](./images/apex_login_workspace_step3.png "Workspace Home Page")

## Task 3 : Enable SAML Authentication for APEX Application

1. To login to your Oracle APEX Workspace, perform the following steps:
    - Open your browser and enter the **URL** to sign in to the APEX development environment.
      - **URL**
            ```
            <copy>https://localhost:16200/ords/</copy>
            ```

            > Note : Replace `"https://localhost"` with your **hosturl** ( eg: `"http://wcc-rfpmgmt-livelab.livelabs.oraclevcn.com"` or `"https://192.0.0.0"`)

    - The login page appears. Enter the **Workspace Name, Username, and Password**. Click **Sign In**.
      - **Workspace Name**: Enter
            ```
            <copy>WCCRFPMGMT</copy>
            ```
      - **Username**: Enter
            ```
            <copy>ADMIN</copy>
            ```
      - **Password**: Enter
            ```
            <copy>Welcome1</copy>
            ```
  ![Workspace Login](./images/apex_login_workspace_step1.png "Login to APEX Workspace")

2. Click on **App Builder** , then click on **RFP Response Management System** Application
  ![RFP Response Management System App Details](./images/apex_refresh_catalog_step1_01.png "RFP Response Management System App Details")

3. Navigate to **Shared Components** > Under **Data Sources**, click on **REST Data Sources**
  ![REST Data Sources](./images/apex_refresh_catalog_step1_03.png "REST Data Sources")

4. Click on **quick_search_library** under the **REST Source Name** list
  ![quick_search_library Details](./images/apex_refresh_catalog_step1_04.png "quick_search_library Details")

5. Click on the **REST Source Catalog** tab and then click on **Refresh from Catalog** button
  ![Refresh from Catalog](./images/apex_refresh_catalog_step1_05.png "Refresh from Catalog")

6. Click on the **Refresh from Service Catalog** button
  ![Refresh from Service Catalog](./images/apex_refresh_catalog_step1_06.png "Refresh from Service Catalog")

## Task 4 : Validate APEX Application Login with SSO

1. Login to your Oracle APEX Workspace, using the following steps:

    - Open your browser and enter the **URL** to sign in to the APEX development environment.
      - URL
            ```
            <copy>https://localhost:16200/ords/</copy>
            ```

           > Note : Replace `"https://localhost"` with your **hosturl** ( eg: `"http://wcc-rfpmgmt-livelab.livelabs.oraclevcn.com"` or `"https://192.0.0.0""`)

    - The login page appears. Enter the **Workspace Name, Username, and Password**. Click **Sign In**.
      - **Workspace Name**: Enter
            ```
            <copy>WCCRFPMGMT</copy>
            ```
      - **Username**: Enter
            ```
            <copy>ADMIN</copy>
            ```
      - **Password**: Enter
            ```
            <copy>Welcome1</copy>
            ```
  ![Workspace Login](./images/apex_login_workspace_step1.png "Login to APEX Workspace")

2. Click on the *User Setting* icon next to the User name and click on **Manage Users and Groups**
  ![Manage Users and Groups](./images/add_apex_users_step2.png "Manage Users and Groups")

3. Check if the Users are already present. If not, create the users by clicking on **Create User** button
  ![Create User](./images/add_apex_users_step3.png "Create User")

## Task 5 : Update WCC OAuth info for invoking from APEX Application

1. Login to your Oracle APEX Workspace, using the following steps:

    - Open your browser and enter the **URL** to sign in to the APEX development environment.
      - URL
            ```
            <copy>https://localhost:16200/ords/</copy>
            ```

           > Note : Replace `"https://localhost"` with your **hosturl** ( eg: `"http://wcc-rfpmgmt-livelab.livelabs.oraclevcn.com"` or `"https://192.0.0.0""`)

    - The login page appears. Enter the **Workspace Name, Username, and Password**. Click **Sign In**.
      - **Workspace Name**: Enter
            ```
            <copy>WCCRFPMGMT</copy>
            ```
      - **Username**: Enter
            ```
            <copy>ADMIN</copy>
            ```
      - **Password**: Enter
            ```
            <copy>Welcome1</copy>
            ```
  ![Workspace Login](./images/apex_login_workspace_step1.png "Login to APEX Workspace")

2. Click on the *User Setting* icon next to the User name and click on **Manage Users and Groups**
  ![Manage Users and Groups](./images/add_apex_users_step2.png "Manage Users and Groups")

3. Check if the Users are already present. If not, create the users by clicking on **Create User** button
  ![Create User](./images/add_apex_users_step3.png "Create User")

4. In the **Create User** Page, provide the below values for creating the user **SALES_REP** and click **Create User** button

    - **Username**: Specify
          ```
          <copy>SALES_REP</copy>
          ```
    - **Email Address**: Specify your email address. eg:
          ```
          <copy>sales_rep@email.com</copy>
          ```
    - **First Name**: Specify
          ```
          <copy>Sales</copy>
          ```
    - **Last Name**: Specify
          ```
          <copy>Representative</copy>
          ```
    - **Password**: Specify
          ```
          <copy>Welcome1</copy>
          ```
    - **Confirm Password**: Specify
          ```
          <copy>Welcome1</copy>
          ```
    - **Require Change of Password on First Use** : *Disable/Un-Check*
  ![Create User - SALES_REP](./images/add_apex_users_step4.png "Create User - SALES_REP")

5. Similarly, create the other users as below:

      - **TECHNICAL_ARCHITECT** User
           - **Username**: Specify
                  ```
                  <copy>TECHNICAL_ARCHITECT</copy>
                  ```
           - **Email Address**: Specify your email address. eg:
                  ```
                  <copy>technical_architect@email.com</copy>
                  ```
           - **First Name**: Specify
                  ```
                  <copy>Technical</copy>
                  ```
           - **Last Name**: Specify
                  ```
                  <copy>Architect</copy>
                  ```
           - **Password**: Specify
                  ```
                  <copy>Welcome1</copy>
                  ```
           - **Confirm Password**: Specify
                  ```
                  <copy>Welcome1</copy>
                  ```
           - **Require Change of Password on First Use** : *Disable/Un-Check*

      - **FINANCE** User
           - **Username**: Specify
                  ```
                  <copy>FINANCE</copy>
                  ```
           - **Email Address**: Specify your email address. eg:
                  ```
                  <copy>finance@email.com</copy>
                  ```
           - **First Name**: Specify
                  ```
                  <copy>Finance</copy>
                  ```
           - **Last Name**: Specify
                  ```
                  <copy>Analyst</copy>
                  ```
           - **Password**: Specify
                  ```
                  <copy>Welcome1</copy>
                  ```
           - **Confirm Password**: Specify
                  ```
                  <copy>Welcome1</copy>
                  ```
           - **Require Change of Password on First Use** : *Disable/Un-Check*

      - **LEGAL** User
           - **Username**: Specify
                  ```
                  <copy>LEGAL</copy>
                  ```
           - **Email Address**: Specify your email address. eg:
                  ```
                  <copy>legal@email.com</copy>
                  ```
           - **First Name**: Specify
                  ```
                  <copy>Legal</copy>
                  ```
           - **Last Name**: Specify
                  ```
                  <copy>Advisor</copy>
                  ```
           - **Password**: Specify
                  ```
                  <copy>Welcome1</copy>
                  ```
           - **Confirm Password**: Specify
                  ```
                  <copy>Welcome1</copy>
                  ```
           - **Require Change of Password on First Use** : *Disable/Un-Check*

  ![Create Other Users](./images/add_apex_users_step5.png "Create Other Users")

## Task 6: Verify User Info details from WCC based on Logged-In User

1. To login to the WCC RFP Response Management System Application, perform the following steps:

    - Open your browser and enter the **URL** to sign in to the APEX development environment.
      - URL
            ```
            <copy>https://localhost:16200/ords/r/wccrfpmgmt/rfp-response-management-system/</copy>
            ```

           > Note : Replace `"https://localhost"` with your **hosturl** ( eg: `"http://wcc-rfpmgmt-livelab.livelabs.oraclevcn.com"` or `"https://192.0.0.0""`)

      - The login page appears. Enter the **Username, and Password**. Click **Sign In**.
        - **Username**: Enter
              ```
              <copy>ADMIN</copy>
              ```
        - **Password**: Enter
              ```
              <copy>Welcome1</copy>
              ```
  ![Application Login](./images/apex_setup_task5_step1.png "Login to APEX Application")

2. On the left navigation menu, click on **Sections** and click on **Create** button
  ![Navigate to Sections](./images/apex_setup_sections_task5_step2.png "Click on Sections")

3. In the **Manage Section Form** window, enter the below values and click on **Create** Button

    - **Name**: Specify
          ```
          <copy>Title and Summary</copy>
          ```
    - **Description**: Specify
          ```
          <copy>This is sample template for Title and Summary Section</copy>
          ```
    - **Template**: Browse and select the file **Title & summary.docx** from the downloaded **wcc\_rfp\_resources.zip** file ( in **Lab 1 - Prepare Setup**)
    - **Type**: Leave the default as **SECTION**
    - **Display Order**: Specify
          ```
          <copy>10</copy>
          ```
    - **Active**: Leave the default as **Y**
    - **Section Owner**: Specify
          ```
          <copy>Sales Rep</copy>
          ```
    - **Instructions**: Specify
          ```
          <copy>Update the Title and Summary section</copy>
          ```
    - **Expectation**: Specify
          ```
          <copy>Updated with relevant info</copy>
          ```
  ![Create Title and Summary Section](./images/apex_setup_sections_task5_step3.png "Create Title and Summary Section")

4. Similarly, create the below sections as well :

    - **Technical Design** Section
           - **Name**: Specify
                  ```
                  <copy>Technical Design</copy>
                  ```
           - **Description**: Specify
                  ```
                  <copy>This is template for Technical Design Section</copy>
                  ```
           - **Template**: Browse and select the file **Technical Design.docx** from the downloaded **wcc\_rfp\_resources.zip** file ( in **Lab 1 - Prepare Setup**)
           - **Type**: Leave the default as **SECTION**
           - **Display Order**: Specify
                  ```
                  <copy>20</copy>
                  ```
           - **Active**: Leave the default as **Y**
           - **Section Owner**: Specify
                  ```
                  <copy>Technical Architect</copy>
                  ```
           - **Instructions**: Specify
                  ```
                  <copy>Update the Technical Design section</copy>
                  ```
           - **Expectation**: Specify
                  ```
                  <copy>Updated with relevant info</copy>
                  ```
      ![Create Technical Design Section](./images/apex_setup_sections_task5_step4.png "Create Technical Design Section")
    - **Pricing And BOM** Section
           - **Name**: Specify
                  ```
                  <copy>Pricing And BOM</copy>
                  ```
           - **Description**: Specify
                  ```
                  <copy>This is template for Pricing And BOM Section</copy>
                  ```
           - **Template**: Browse and select the file **Finance.docx** from the downloaded **wcc\_rfp\_resources.zip** file ( in **Lab 1 - Prepare Setup**)
           - **Type**: Leave the default as **SECTION**
           - **Display Order**: Specify
                  ```
                  <copy>30</copy>
                  ```
           - **Active**: Leave the default as **Y**
           - **Section Owner**: Specify
                  ```
                  <copy>Finance</copy>
                  ```
           - **Instructions**: Specify
                  ```
                  <copy>Update the Pricing And BOM section</copy>
                  ```
           - **Expectation**: Specify
                  ```
                  <copy>Updated with relevant info</copy>
                  ```
      ![Create Pricing And BOM Section](./images/apex_setup_sections_task5_step4_2.png "Create Pricing And BOM Section")
    - **Legal** Section
           - **Name**: Specify
                  ```
                  <copy>Legal</copy>
                  ```
           - **Description**: Specify
                  ```
                  <copy>This is sample template for Legal Section</copy>
                  ```
           - **Template**: Browse and select the file **Legal_Template.docx** from the downloaded **wcc\_rfp\_resources.zip** file ( in **Lab 1 - Prepare Setup**)
           - **Type**: Leave the default as **SECTION**
           - **Display Order**: Specify
                  ```
                  <copy>40</copy>
                  ```
           - **Active**: Leave the default as **Y**
           - **Section Owner**: Specify
                  ```
                  <copy>Legal</copy>
                  ```
           - **Instructions**: Specify
                  ```
                  <copy>Update the Legal section</copy>
                  ```
           - **Expectation**: Specify
                  ```
                  <copy>Updated with relevant info</copy>
                  ```
      ![Create Legal Section](./images/apex_setup_sections_task5_step4_3.png "Create Legal Section")

5. This shows the summary of all the sections created.

  ![Section Summary](./images/apex_setup_sections_task5_step5.png "Summary of all the sections created")

    **Summary**

    You have now successfully setup the RFP Response Management Application for the RFP Application and User Flow.

You are now ready to **proceed to the next lab**.

## Appendix 1: Obtain IDCS Client ID, Certificate and Private Key from the Weblogic Credentials

- Obtain the already configured IDCS Client ID, Certificate and Private Key for IDCS OAuth Client App using the Weblogic EM Console - Credentials

- This consists of the below steps:
      - **Obtain IDCS Certificate**
      - **Obtain IDCS Private Key**
      - **Obtain IDCS Client ID and Secret**

### **1.1 Obtain IDCS Certificate**

   1. On the new *web browser* window , Login to the Weblogic EM Console URL as **weblogic** User and click on **Sign in** button. Details are provided below
      - **URL**
            ```
            <copy>https://localhost:7001/em</copy>
            ```
        > Note : Replace `"https://localhost"` with your **hosturl** ( eg: `"http://wcc-rfpmgmt-livelab.livelabs.oraclevcn.com"` or `"https://192.0.0.0"`)

      - **Username**
            ```
            <copy>weblogic</copy>
            ```
      - **Password**
            ```
            <copy>Welcome1</copy>
            ```
      ![This image shows Weblogic EM Console Login Page](./images/09_idcs_saml_client_create_step9_1.png "Weblogic EM Console Login Page")

   2. Under the Domain Name, Navigate to **Weblogic Domain**, **Security**, click on **Credentials**
      ![This page shows the EM Console Page Credentials Navigation](./images/09_idcs_saml_client_create_step9_2.png "EM Console Page Credentials Navigation")

   3. In the **Credentials** Page, Click on the triangle Expand icon next to **oracle.wsm.security**
      ![This page shows the EM Console Page Credentials Page](./images/09_idcs_saml_client_create_step9_3.png "EM Console Page Credentials Page")

   4. Select **idcs.signin.cert** and Click on the **Edit** button
      ![This page shows the EM Console Page Credentials Page](./images/09_idcs_saml_client_create_step9_4_1.png "EM Console Page Credentials Page")

   5. Select the entire value specified in the **Credential**, right-click and copy the value
      > Note : This value will be referred as *WCC\_IDCS\_OAUTH\_SIGNIN\_CERT*

      ![This page shows the EM Console Page Credentials Page](./images/09_idcs_saml_client_create_step9_4_1_2.png "EM Console Page Credentials Page")

   6. Open text editor (eg: Notepad), paste the copied cert value *between the below two entries*
      - **Cert entry beginning**
            ```
            <copy>-----BEGIN CERTIFICATE-----</copy>
            ```
      - **Cert entry End**
            ```
            <copy>-----END CERTIFICATE-----</copy>
            ```
      ![This page shows the EM Console Page Credentials Page](./images/09_idcs_saml_client_create_step9_4_1_3.png "EM Console Page Credentials Page")

   7. Save the file as *IDCS_Signin_Cert.crt* ( select **Save as type** as **All files (\*.\*)** )
         > Note : This file will be referred as *WCC\_IDCS\_OAUTH\_SIGNIN\_CERT\_FILE*

      - **IDCS Sign-In File Name**
            ```
            <copy>IDCS_Signin_Cert.crt</copy>
            ```
      ![This page shows the EM Console Page Credentials Page](./images/09_idcs_saml_client_create_step9_4_1_4.png "EM Console Page Credentials Page")

### **1.2 Obtain IDCS Private Key**

   1. On the new *web browser* window , Login to the Weblogic EM Console URL as **weblogic** User and click on **Sign in** button. Details are provided below
      - **URL**
            ```
            <copy>https://localhost:7001/em</copy>
            ```
        > Note : Replace `"https://localhost"` with your **hosturl** ( eg: `"http://wcc-rfpmgmt-livelab.livelabs.oraclevcn.com"` or `"https://192.0.0.0"`)

      - **Username**
            ```
            <copy>weblogic</copy>
            ```
      - **Password**
            ```
            <copy>Welcome1</copy>
            ```
      ![This image shows Weblogic EM Console Login Page](./images/09_idcs_saml_client_create_step9_1.png "Weblogic EM Console Login Page")

   2. Under the Domain Name, Navigate to **Weblogic Domain**, **Security**, click on **Credentials**
      ![This page shows the EM Console Page Credentials Navigation](./images/09_idcs_saml_client_create_step9_2.png "EM Console Page Credentials Navigation")

   3. In the **Credentials** Page, Click on the triangle Expand icon next to **oracle.wsm.security**
      ![This page shows the EM Console Page Credentials Page](./images/09_idcs_saml_client_create_step9_3.png "EM Console Page Credentials Page")

   4. Select **idcs.signin.privatekey** and Click on the **Edit** button
      ![This page shows the EM Console Page Credentials Page](./images/09_idcs_saml_client_create_step9_4_2.png "EM Console Page Credentials Page")

   5. Select the entire value specified in the **Credential**, right-click and copy the value
      > Note : This value will be referred as *WCC\_IDCS\_OAUTH\_PRIVATE\_KEY*

      ![This page shows the EM Console Page Credentials Page](./images/09_idcs_saml_client_create_step9_4_2_2.png "EM Console Page Credentials Page")

   6. Open text editor (eg: Notepad), paste the copied cert value *between the below two entries*
      - **Key entry beginning**
            ```
            <copy>-----BEGIN RSA PRIVATE KEY-----</copy>
            ```
      - **Key entry End**
            ```
            <copy>-----END RSA PRIVATE KEY-----</copy>
            ```
      ![This page shows the EM Console Page Credentials Page](./images/09_idcs_saml_client_create_step9_4_2_3.png "EM Console Page Credentials Page")

   7. Save the file as *IDCS_Signin_Private.key* ( select **Save as type** as **All files (\*.\*)** )
            > Note : This file will be referred as *WCC\_IDCS\_OAUTH\_PRIVATE\_KEY\_FILE*

      - **IDCS Sign-In Private Key File Name**
            ```
            <copy>IDCS_Signin_Private.key</copy>
            ```
      ![This page shows the EM Console Page Credentials Page](./images/09_idcs_saml_client_create_step9_4_2_4.png "EM Console Page Credentials Page")

### **1.3 Obtain IDCS Client ID and Secret**

   1. On the new *web browser* window , Login to the Weblogic EM Console URL as **weblogic** User and click on **Sign in** button. Details are provided below
      - **URL**
            ```
            <copy>https://localhost:7001/em</copy>
            ```
        > Note : Replace `"https://localhost"` with your **hosturl** ( eg: `"http://wcc-rfpmgmt-livelab.livelabs.oraclevcn.com"` or `"https://192.0.0.0"`)

      - **Username**
            ```
            <copy>weblogic</copy>
            ```
      - **Password**
            ```
            <copy>Welcome1</copy>
            ```
      ![This image shows Weblogic EM Console Login Page](./images/09_idcs_saml_client_create_step9_1.png "Weblogic EM Console Login Page")

   2. Under the Domain Name, Navigate to **Weblogic Domain**, **Security**, click on **Credentials**
      ![This page shows the EM Console Page Credentials Navigation](./images/09_idcs_saml_client_create_step9_2.png "EM Console Page Credentials Navigation")

   3. In the **Credentials** Page, Click on the triangle Expand icon next to **oracle.wsm.security**
      ![This page shows the EM Console Page Credentials Page](./images/09_idcs_saml_client_create_step9_3.png "EM Console Page Credentials Page")

   4. Select **idcs.credentials** and Click on the **Edit** button
      ![This page shows the EM Console Page Credentials Page](./images/09_idcs_saml_client_create_step9_4_3.png "EM Console Page Credentials Page")

   5. Select the entire value for the WCC IDCS OAuth Client ID specified in the **User Name**, right-click and copy the value. This value will be referred as *WCC\_IDCS\_OAUTH\_CLIENT\_ID*
      ![This page shows the EM Console Page Credentials Page](./images/09_idcs_saml_client_create_step9_4_3_2.png "EM Console Page Credentials Page")

   6. To obtain the WCC IDCS OAuth Client Secret, please follow the below steps.
      > Note: This value will be referred as *WCC\_IDCS\_OAUTH\_CLIENT\_SECRET*

      1. Under the Domain Name, Navigate to **Weblogic Domain**, click on **System MBean Browser**
            ![This page shows the EM Console Page Credentials Navigation](./images/09_idcs_saml_client_create_step9_6_1.png "EM Console Page Credentials Navigation")

      2. In the System MBean Browser,
         - Navigate to **Application Defined MBeans** -> **com.oracle.jps** -> **Domain:** *Domain_Name* -> **JpsCredentialStore**. Double click on the **JpsCredentialStore** MBean.
         - On the **Application Defined MBeans: JpsCredentialStore** Page, navigate to **Operations** tab and select/click the **getPortableCredential** operation.
            ![This page shows the EM Console Page Credentials Navigation](./images/09_idcs_saml_client_create_step9_6_2.png "EM Console Page Credentials Navigation")

      3. Specify the parameters. The first parameter is the name of the map. The second parameter is the key within a map. Then click **Invoke** to perform operation.
            - Value for **p1**
                  ```
                  <copy>oracle.wsm.security</copy>
                  ```
            - Values for **p2**
                  ```
                  <copy>idcs.credentials</copy>
                  ```
            ![This page shows the EM Console Page Credentials Navigation](./images/09_idcs_saml_client_create_step9_6_3.png "EM Console Page Credentials Navigation")

      4. Once the operation gets executed successfully, the values are listed in the **Return Value** section. Copy/Note the value for **password** under **Data**
            > Note : This value will be referred as *WCC\_IDCS\_OAUTH\_CLIENT\_SECRET*

         ![This page shows the EM Console Page Credentials Navigation](./images/09_idcs_saml_client_create_step9_6_4.png "EM Console Page Credentials Navigation")

## Acknowledgements

- **Authors-** Senthilkumar Chinnappa, Senior Principal Solution Engineer, Oracle WebCenter Content
- **Contributors-** Senthilkumar Chinnappa, Mandar Tengse , Parikshit Khisty
- **Last Updated By/Date-** Senthilkumar Chinnappa, July 2024
