# Lab 2 - Migrate database from an on-premises SQL Server to Azure SQL Database

## Introduction

Modernizing on-premises databases by migrating them to the cloud is a
critical step in adopting Azure's fully managed platform and improving
scalability, security, and maintainability. In this lab, you will
perform an end-to-end migration of a SQL Server database hosted on a
Windows Virtual Machine to **Azure SQL Database**.

## Objective

The objective of this lab is to guide the learners through the
end-to-end process of migrating a SQL Server database hosted on a
Windows Virtual Machine to Azure SQL Database. By completing this lab,
participants will gain hands-on experience in provisioning a SQL Server
VM, preparing the AdventureWorks sample database, configuring the Azure
Database Migration Service (DMS), and executing an offline migration to
Azure SQL Database. This exercise demonstrates how to modernize
on-premises SQL workloads by leveraging Azure’s fully managed database
platform.

## Exercise 1 - Create an On prem SQL Server

Companies have legacy databases which they would need to migrate to
Azure to modernize their applications. We need an On-prem database which
we can migrate to the Azure SQL Database. In this Exercise, you will
create an On-prem Database in Azure SQL Virtual Machine.

1.  Sign in to the **Azure portal** at +++https://portal.azure.com+++
    using the following cloud slice credentials.

    - Username - +++@lab.CloudPortalCredential(User1).Username+++

    - Password - +++@lab.CloudPortalCredential(User1).Password+++
    - TAP Token - +++@lab.CloudPortalCredential(User1).AccessToken+++
 
    ![](./media/image1.png)
    ![](./media/image2.png)
    ![A screenshot of a computer AI-generated content may be incorrect.](./media/image3.png)

2.  Search for and select +++**Azure SQL**+++.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image4.png)

3.  In the Overview page, find **SQL Server on Azure Virtual Machines**
    and select **Show options**.

    ![](./media/image5.png)

4.  Choose **Free SQL Server License: SQL Server 2019 Developer on
    Windows Server 2022** image from the list.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image6.png)

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image7.png)

5.  Select **Create virtual machine** to start creating the VM.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image8.png)

6.  In the Create a virtual machine, select **ResourceGroup1** from the
    **Resource group** dropdown.
    ![](./media/image9.png)

7.  Enter the below details under the Instance details

    - Virtual Machine Name – +++**SQLVM**+++

    - Region – **@lab.CloudResourceGroup(ResourceGroup1).Location** is selected here

    - Availability options – Select **No infrastructure redundancy
    required**

    - Security type – Standard

    - Image – Select **Free SQL Server License: SQL Server 2019 Developer on
    Windows Server 2022**

    - Size – **Standard_D16ds_v4-16 vcpus, 64 GiB memory**

    ![](./media/image10.png)

8.  Under Administrator account, enter the below details and select
    **Next:Disks**.

    - User name – Enter +++sqlserveradmin+++

    - Password – Enter +++sqlPwd981!2@98+++

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image11.png)

9.  Accept the defaults in other pages till the **SQL Server Settings**
    page. In the **SQL Server Settings** page,

    - SQL Authentication – **Enable**

    - Login – Enter +++**sqlserveradmin**+++

    - Passwrod – Enter +++**sqlPwd981!2@98**+++

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image12.png)

10. Select **Review + Create** to create the VM.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image13.png)

11. Click on **Create** once the validation succeeds.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image14.png)

12. Once the deployment is complete, select **Go to resource**. The
    deployment will take around 20 minutes to complete.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image15.png)

13. From the **Overview** page of the VM, look for the **Public IP** and
    **save** the **IP value** in a notepad. This value will be needed to
    **connect** to the **database** in the VM.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image16.png)

14. From the left pane of the VM, select **Network Settings** under
    **Networking**. Select **+ Create port rule** -\> **Inbound port rule**.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image17.png)

15. We will need to add an inbound port rule to allow TCP traffic from
    port 1433.

16. In the Add inbound security rule pane, select the below values and
    then select **Add**.

    - Source – Select **Any**

    - Source port ranges - \*

    - Destination – Select **Any**

    - Service – Select **Custom**

    - Destination port ranges – Enter +++**1433**+++

    - Protocol – Select **TCP**

    - Action – Select **Allow**

    - Name – Enter +++TCPRule+++

    ![](./media/image18.png)

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image19.png)

17. From the VM’s Overview page, select the drop down next to
    **Connect** and select the **Connect** option.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image20.png)

18. In the **SQLVM|Connect** page, select the **Download RDP file**
    option to download the Remote Desktop Connection file.

    ![](./media/image21.png)

    **Note:** If you get a pop-up, select **Keep**.

19. Double click the downloaded file to connect to the VM. Select
    **Connect** when prompted.

    ![](./media/image22.png)

20. Login using the VM credentials.

    - Username - +++sqlserveradmin+++

    - Password - +++sqlPwd981!2@98+++

    ![](./media/image23.png)
21. Select **Yes**
    ![](./media/image24.png)

22. Once logged in, open web browser and select **Start without your data**.
    ![](./media/image93.png)
    
23. Click **Confrim and Continue**.
    ![](./media/image94.png)
    
24. Click **Continue without Google Data**.
    ![](./media/image95.png)
    
25. Click **Confirm and start browsing**.
    ![](./media/image96.png)
    
27. Open the link +++https://download-directory.github.io/+++.
    ![](./media/image97.png)
    
29. Enter +++https://github.com/Labsrepo/Labfiles+++ and press **Enter**.
    ![](./media/image98.png)
    
31. Open the folder where the file is downloaded and unzip the downloaded folder, and then unzip the adventureworks zip file from it.
    ![](./media/image99.png)
    
    ![](./media/image100.png)
    This will be used as the input data in this exercise.
    
33. Create a folder named **Labfiles** in C drive and paste adventureworks2019.bak file in it.
    ![](./media/image101.png)
    
35. Open the +++SQL Server Management Studio 20+++ from
    the windows Start menu.
    ![A computer screen with a blue background AI-generated content may be
    incorrect.](./media/image25.png)

36. Ensure that the below details are selected, update if the values are
    different.

    - Server name - +++SQLVM+++

    - Authentication Type - Windows Authentication

    - Encryption – Optional

    - Trust Server certificate – Select the checkbox.
    Then  click on **Connect**

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image26.png)

37. The connection succeeds and gets connected to the SQL Server.

    ![A computer screen shot of a blue screen AI-generated content may be
    incorrect.](./media/image27.png)

38. Right click on the Databases – Select **Restore Database**.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image28.png)

39. In the Restore Database page, select **Device** -\> select the
    **three dots** to navigate to the File Explorer in order to upload
    the database.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image29.png)

40. Click on **Add**.

    ![](./media/image30.png)

41. There is a .bak file of the **Adventureworks** database under
    **C:\LabFiles**. Navigate to the path and select
    **AdventureWorks2019.bak** and select **OK**. Select **OK** once the
    file is uploaded.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image31.png)

42. Click **OK**.

    ![](./media/image32.png)

43. In the Restore pane, ensure that the uploaded .bak file is selected
    and then click on **OK**.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image33.png)

44. Once the upload is **successful**, you will get a success message
    that the upload is completed.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image34.png)

45. Now, you have an On-prem SQL Server with the Database Adventure
    Works inside it.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image35.png)

The aim is to migrate this DB to the Azure SQL Database. Now minimise
the VM.

## Exercise 2 - Create an Azure SQL Database

In this exercise, you will create an Azure SQL Database to which you
will migrate your on-prem database.

1.  Close the VM and in a new window open the Select SQL Deployment
    option page at +++https://portal.azure.com/#create/Microsoft.AzureSQL+++ and
    select **Show options**.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image36.png)

2.  Select the **Create SQL Database** option.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image37.png)

3.  In the **Create SQL Database** page, click on **Create New** under
    Resource group – Type in the **Name** as +++**RG4TargetDB**+++.
    Select **OK**.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image38.png)

4.  Enter the **Database name** as +++**adventwkdb**+++ and select
    **Create new** under **Server**.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image39.png)

5.  Enter the below details and click **OK**.

    - Server name - +++mysqlserver789698+++

    - Location – East US2

    - Authentication method – Use SQL authentication

    - Server admin login - +++tgsqlserveradmin+++

    - Password - +++sqlPwd981!2@98+++

    ![](./media/image40.png)

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image41.png)

6.  Back in the Create SQL Database page, update the below details and
    select **Next: Networking.**

    - Want to use elastic pool – Select **No**

    - Workload environment – Select **Devlopment**

    - Backup storage redundancy – Select **Locally-redundant backup
      storage**

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image42.png)

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image43.png)

7.  In the Networking page, select the below options and select **Next :
    Security**.

    - Connectivity method – Select **Public endpoint**

    - Add current client IP address – **Yes**

    - Connection policy – Select the **Default** option

    ![A screenshot of a computer screen AI-generated content may be
    incorrect.](./media/image44.png)

8.  Accept the defaults in the **Security** page and then click on
    **Next : Additional Settings**. Accept the defaults in the other
    pages and navigate to the **Review + create** page and click on
    **Review + create**.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image45.png)

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image46.png)

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image47.png)

9.  Once the validation completes, select **Create** to create the
    resource.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image48.png)

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image49.png)

10. Once the deployment succeeds, select **Go to resource**.

    ![](./media/image50.png)

    ![](./media/image51.png)

11. From the overview page, copy the value of the **Server name** and
    save it in a notepad for usage in future exercise.

    ![](./media/image52.png)

## Exercise 3 – Create Data Migration Service

In this exercise, you will create a Data Migration Service using which
you will be migrating the database.

1.  From the database page, select **Query editor** from the left pane.

    ![](./media/image53.png)

2.  Login using

    - Login - +++**tgsqlserveradmin**+++

    - Password - +++**sqlPwd981!2@98**+++

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image54.png)

3.  To create the login and user on the target Azure SQL Database, run
    the following script
    ```
    CREATE USER \[migrationuser\] WITH PASSWORD = 'sqlPwd981!2@98'; ALTER
    ROLE db_owner ADD MEMBER \[migrationuser\];
    ```

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image55.png)

    **Note:** If you get a pop-up stating “Your unsaved edits will be
    discarded”. Then click **OK**.

4.  Navigate to the Home page of the Azure portal. From the search bar,
    search for and select +++**Azure Database Migration Service+++**

    ![](./media/image56.png)

5.  Select the **Start a new migration** option.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image57.png)

6.  Enter the below details and select **Select**.

    - Source server type – Select **SQL Server**

    - Target server type – Select **Azure SQL Database**

    - Database Migration Service – Select **Database Migration Service**

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image58.png)

7.  In the **Create Data Migration Service** page, enter the below
    details and select **Review + create**.

    - Subscription – Select your assigned **subscription**
    - Resource group – Select **RG4TargetDB**
    - Location – **East US 2** is used here.
    - Migration Service Name – Enter +++**dbMigrate**+++

    ![](./media/image59.png)

8.  Once the validation passes, select **Create**.

    ![](./media/image60.png)

9.  Click on **Go to resource** to go to the created Migration Service
    resource.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image61.png)

10. A self-hosted integration runtime is required to access your source
    database backups, check the validity of the backup set, and upload
    backups to Azure storage account.

11. In the **dbMigrate** resource that you just created, select the
    **View Integration runtime** option.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image62.png)

12. Select Configure integration runtime option.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image63.png)

13. Select the **Download and install the integration runtime** option
    and click on Download in the browser that opens.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image64.png)

    ![A screenshot of a computer error AI-generated content may be
    incorrect.](./media/image65.png)

14. Select the first option and click on **Download**.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image66.png)

15. Double click on the downloaded file and follow the prompts to
    complete the installation.

    ![](./media/image67.png)

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image68.png)

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image69.png)

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image70.png)

16. Once the installation is complete, the **Integration Runtime** asks
    for a **Authentication Key**. Copy the same from the Azure portal as
    in the screenshot below and paste it in the **Integration Runtime
    Configuration Manager** and click on **Register**.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image71.png)

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image72.png)

17. Once the registration is successful, **Finish** the Configuration
    Manager.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image73.png)
18. Click **Close**.
    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image74.png)

19. Once this is done, after few minutes, the status of the Integration
    runtime will change to **Online**. Proceed to the next step once it
    is **Online**.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image75.png)

## Exercise 4 - Migrate SQL Server to Azure SQL Database (offline)

In this exercise, you will perform the most important part – **The
Migration**. You will use the Database Migration System (DMS) in Azure
to perform the same.

1.  From the dbMigrate Overview page, select **New Migration**.

    ![](./media/image76.png)

2.  In the **Select new migration scenario**, select the below details
    and select **Select**.

    - Source server type – Select **SQL Server**

    - Target server type – Select **Azure SQL Database**

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image77.png)

3.  In the **Azure SQL Database Offline Migration Wizard**, enter the
    below details and select **Next: Connect to source SQL Server**

    - Source Infrastructure Type – Select **Virtual Machine**

    - Resource group – Select **ResourceGroup1**

    - Location – **@lab.CloudResourceGroup(ResourceGroup1).Location** is used here

    - SQL Server Instance Name – Give the **VM’s IP address** (This will be
    a unique value)

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image78.png)

4.  In the next screen, enter the below details and then select **Next:
    Select Databases for migration**.

    - Source server name – Enter the VM IP address

    - Authentication type – Select **SQL Authentication**

    - User name – Enter +++sqlserveradmin+++

    - Password – Enter +++sqlPwd981!2@98+++

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image79.png)

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image80.png)

5.  Next, select **AdventureWorks2019** database and select Next:
    Connect to targe Azure SQL Database.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image81.png)

6.  In the next screen, the details of the target DB are populated.
    Check these, enter the password as +++sqlPwd981!2@98+++ and select
    **Next: Map source and target databases**.

    ![A screenshot of a computer screen AI-generated content may be
    incorrect.](./media/image82.png)

7.  Select the Target database as +++**adventwkdb**+++ and select
    **Next: Select Database tables to migrate**

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image83.png)

8.  In the next page, you can select either one of the tables for
    testing, or select all the tables and then select **Next: Database
    migration summary**. Check the details and select the **Start
    migration** option.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image84.png)

    ![](./media/image85.png)

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image86.png)

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image87.png)

    The migration takes around 30 minutes to complete. Once the migration is
    complete, the status changes to **Succeeded**.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image88.png)

## Exercise 5 - Check the migrated Database

Now that the migration is complete, you should check the database that
is migrated to Azure. We will use the **SQL Server Management Studio**
to do the same.

1.  Open the **SSMS** from your system (not the Azure VM).

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image89.png)

2.  Select **Connect** -\> **Database Engine**. Fill in the details as
    below and then select **Connect**.

    - Server Name – Ente the **Server name** that you saved after creating
    the Azure SQL Database

    - Administration – Select **SQL Server Authentication**

    - User Name – Enter +++**tgsqlserveradmin**+++

    - Password – Enter +++**sqlPwd981!2@98**+++

    - Encrypt – Select **Optional** and check mark **Trust Server
    Certification**.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image90.png)

3.  Once connected, check the databases.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image91.png)

4.  You will find the migrate Adventureworks Database that got migrated
    from the SQL Server.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image92.png)

## Summary

In this lab, you created a SQL Server VM in Azure, deployed the
AdventureWorks database, and prepared it for migration. You then set up
and used the Azure Database Migration Service (DMS) to perform an
offline migration of the database from the SQL Server VM to Azure SQL
Database. Through this process, you learned the key steps involved in
assessing readiness, configuring source and target environments, and
executing a secure and reliable migration. This hands-on exercise
highlighted best practices for migrating SQL Server workloads to Azure
SQL Database, enabling a smoother transition to a fully managed cloud
environment.
