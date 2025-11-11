# Lab 4 - Migrate a legacy .NET on-premises application to Azure App Service

## Objective

In this lab, you will learn how to migrate an existing on-premises .NET
To-Do List application to Azure App Service using the **App Service
Migration Assistant** tool. You will analyze the on-prem application for
Azure compatibility, provision the necessary Azure resources, and
perform the migration directly from the local environment to the cloud.
By the end of the lab, you will understand how to use App Service
Migration Assistant to modernize legacy .NET applications and host them
in a fully managed Azure platform.

## Exercise 1 – Set up the environment

In this exercise, you will set up the pre-requisites required for
executing this lab

1.  Open **PowerShell** in **Administrator** mode from your system and
    execute the below command to install IIS in your system
    ```
    dism /online /enable-feature /featurename:IIS-WebServer /all /norestart 
    ```
    ```
    dism /online /enable-feature /featurename:IIS-ManagementConsole /all /norestart
    ```
    ![A screenshot of a computer program AI-generated content may be
    incorrect.](./media/image1.png)

3.  Open the **Command Prompt** in the Administrator mode and
    **execute** the below command. This is to change the IIS option to
    accommodate the lab requirements.
    ```
    %windir%\system32\inetsrv\appcmd unlock config -section:system.webServer/handlers
    ```
    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image2.png)

5.  Open File Explorer and navigate to **C:\Labfiles**. Create a folder
    named +++**dotnetwebapp**+++

    ![](./media/image3.png)

6.  Press **Windows+R**, type +++**inetmgr**+++ and select **OK**.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image4.png)

    ![](./media/image5.png)

7.  In the IIS Manager, Right click on **Sites** in the left pane and
    select **Add Website**.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image6.png)

8.  Enter the **Server name** as +++**dotnetwebapp**+++. Select the
    three dots against **Physical path**, navigate to the newly created
    folder **dotnetwebapp** in **C:\Labfiles** and select **OK**.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image7.png)

9.  Select OK in the next screen.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image8.png)

## Exercise 2– Publish the On premise app to IIS Server

In this exercise, you will publish the On prem app to an IIS Server.
This is done in order to migrate the app to Azure in the next step using
the Azure App Service Migration Assistant.

1.  Open **Visual Studio 2022** and select **Open a local folder**.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image9.png)

2.  Select **dotnetwebapp-local** from **C:\LabFiles\Migrate a legacy .NET on-premises application to Azure App Service** and select
    **Select Folder**.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image10.png)

3.  Double click the **DotNetAppSqlDb.sln** solution file to open the
    solution.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image11.png)

4.  Right click on the opened sln file.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image12.png)

5.  Select Publish in order to publish the application to a target.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image13.png)
    **Note:** When we click on Publish it automaticaly gets published but we will do manula publish.

7.  Select **Folder** and click **Next**.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image14.png)

8.  Select the newly created **dotnetwebapp** folder under
    **C:\Labfiles** and click **OK**.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image15.png)

9.  Select **Finish** to publish the app to the target folder.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image16.png)

10.  Once the **Publish profile** creation is successful, select
    **Close**.
      ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image17.png)

11. Back in the **Publish** screen, select **Publish**.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image18.png)

12. Ensure that the Publish is successful

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image19.png)

## Exercise 3 – Create an Azure SQL database

In this exercise, you will create an Azure SQL database, create the
tables required for the application in it, update the config file with
the required updates to connect the Azure SQL Database, and then check
if the application runs on the IIS Server.

1.  Open the Select SQL Deployment option page at
    +++https://portal.azure.com/#create/Microsoft.AzureSQL+++ and sign
    in using the following cloud slice credentials.

    - Username - +++@lab.CloudPortalCredential(User1).Username+++

    - TAP Token - +++@lab.CloudPortalCredential(User1).AccessToken+++

    ![](./media/image20.png)

    ![](./media/image21.png)

    ![](./media/image22.png)

2.  Select **Show options**.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image23.png)

3.  Select the **Create SQL Database** option.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image24.png)

4.  In the **Create SQL Database** page, select **ResourceGroup1** as a
    resource group.

    ![](./media/image25.png)

5.  Enter the **Database name** as +++**DotNetAppSqlDb**+++ and select
    **Create new** under **Server**.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image26.png)

6.  Enter the below details and click **OK**.

    - Server name - +++mysqlserver@lab.labinstanceid()+++

    - Location – @lab.CloudResourceGroup(ResourceGroup1).Location

    - Authentication method – Use SQL authentication

    - Server admin login - +++tgsqlserveradmin+++

    - Password - +++sqlPwd981!2@98+++

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image27.png)

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image28.png)

7.  Back in the Create SQL Database page, update the below details and
    select **Next: Networking.**

    - Want to use elastic pool – Select **No**

    - Workload environment – Select **Development**

    - Backup storage redundancy – Select **Locally-redundant backup
      storage**

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image29.png)

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image30.png)

8.  In the Networking page, select the below options and select **Next :
    Security**.

    - Connectivity method – Select **Public endpoint**

    - Add current client IP address – **Yes**

    - Connection policy – Select the **Default** option

    ![A screenshot of a computer screen AI-generated content may be
    incorrect.](./media/image31.png)

9.  Accept the defaults in the **Security** page and then click on
    **Next : Additional Settings**. Accept the defaults in the other
    pages and navigate to the **Review + create** page and click on
    **Review + create**.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image32.png)

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image33.png)

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image34.png)

10. Once the validation completes, select **Create** to create the
    resource.

    ![](./media/image35.png)

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image36.png)

11. Once the deployment succeeds, select **Go to resource**.

    ![](./media/image37.png)

    ![](./media/image38.png)

12. From the overview page, copy the value of the **Server name** and
    save it in a notepad for usage in future exercise.

    ![](./media/image39.png)

13. Open the **mysqlserver@lab.LabInstance.Id** resource. Select the
    check box to enable **Allow Azure services and resources to access
    this server**. Select **Save**.

    ![](./media/image40.png)

12. Open SQL Server Management Studio (SSMS) from your VM. Select **File
    -\> Connect Object Explorer**.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image41.png)

13. Enter the below details and select **Connect**.

    - Server Name - Enter the server name you saved in the earlier step

    - Authentication – **SQL Server Authentication**

    - User Name – +++**tgsqlserveradmin**+++

    - Password - +++**sqlPwd981!2@98**+++

    - Encrypt – Select **Optional**

    - Trust Server Certificate – Select the **Check box**

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image42.png)

14. Once connected, expand the **Server -\> Databases** and select the
    Database **DotNetAppSqlDb** and then select **New Query**.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image43.png)

15. In the **New Query** tab, enter the below query and **execute** it.

    ```
    USE [DotNetAppSqlDb] 

    GO 
    SET ANSI_NULLS ON 
    
    GO 
    SET QUOTED_IDENTIFIER ON 
    
    GO 
    CREATE TABLE [dbo].[__MigrationHistory]( 
    
    [MigrationId] [nvarchar](150) NOT NULL, 
    
    [ContextKey] [nvarchar](300) NOT NULL, 
    
    [Model] [varbinary](max) NOT NULL, 
    
    [ProductVersion] [nvarchar](32) NOT NULL, 
    
     CONSTRAINT [PK_dbo.__MigrationHistory] PRIMARY KEY CLUSTERED  
    
    ( 
    
        [MigrationId] ASC, 
        
        [ContextKey] ASC 
    
    )WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY] 
    
    ) ON [PRIMARY] TEXTIMAGE_ON [PRIMARY] 
    GO 
    ```

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image44.png)

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image45.png)

16. Similar to the last step, execute the below query.

    ```
    USE [DotNetAppSqlDb] 
    GO 
    SET ANSI_NULLS ON 
    
    GO 
    SET QUOTED_IDENTIFIER ON 
    GO 
    
    CREATE TABLE [dbo].[Todoes]( 
    
    [ID] [int] IDENTITY(1,1) NOT NULL, 
    
    [Description] [nvarchar](max) NULL, 
    
    [CreatedDate] [datetime] NOT NULL, 
    
    CONSTRAINT [PK_dbo.Todoes] PRIMARY KEY CLUSTERED  
    ( 
        [ID] ASC 
    )WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY] 
    ) ON [PRIMARY] TEXTIMAGE_ON [PRIMARY] 
    GO 
    ```

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image46.png)

17. The above 2 commands create the tables required in the Database.

18. When you published the app to a folder in the last exercise, the
    target folder would have got populated with the app related data.

19. Open **Web.config** file from **C:\Labfiles\dotnetwebapp** and
    replace the content under the tag **Connection String** (in the Line
    number 12) as below, after replacing the ":Server Name" with the
    Server name value you saved from the Azure earlier.

    ```
    <add name="MyDbConnection"  

         connectionString="Server=tcp:<Server Name>,1433;Database=DotNetAppSqlDb;User
    ID=tgsqlserveradmin;Password=sqlPwd981!2@98;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;MultipleActiveResultSets=True;"  

         providerName="System.Data.SqlClient" /> 

    ```

    ![A screenshot of a computer program AI-generated content may be
    incorrect.](./media/image47.png)

## Exercise 4 - Create a Azure Migrate Project

An Azure Migrate Project needs to be created in the Azure portal before
starting to migrate.

1.  From the Azure portal **Home** page search bae, search for and
    select +++**Azure Migrate**+++.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image49.png)

2.  From the left pane, select Web apps.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image50.png)

3.  Select +++**ResourceGroup1**+++ under Resource group. Enter the Project name as +++**AzureMigrateProj**+++, select your region
    and select **Create**.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image51.png)

4.  Ensure that the project is created.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image52.png)

## Exercise 5 - Migrate to Azure

In this exercise, you will migrate the app that is available on the IIS
Server to Azure App Service. You have the Migration Assistant tool on
your VM and you can start to assess and migrate the application.

1.  Open the App Service Migration Assistant. The tool will show you the
    websites that are running in IIS.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image53.png)

2.  From the list of available sites, choose **dotnetwebapp** and click
    **Next**.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image54.png)

3.  The tool will now assess the application to see if it can run in
    Azure and if anything needs to be changed. This will result in
    an **Assessment Report** like in the image below.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image55.png)

4.  Select **Next** to proceed.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image56.png)

5.  Select **Copy Code and Open Browser**. Follow the prompts and login
    using your Azure crendetials.

    ![A screenshot of a login page AI-generated content may be
    incorrect.](./media/image57.png)

1.  In the next screen, select the **AzureMigrateProj** migration
    project that you created earlier.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image58.png)

2.  Select **Next**.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image59.png)

3.  Enter the below details and select **Migrate**.

    - Resource Group – Select **Use existing** and select
    **ResourceGroup1**.

    - Destination Site Name - +++**todoitemswa@lab.LabInstance.Id**+++

    - App Service Plan – Select **Create New**

    - Region – Select your nearest region

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image60.png)

4.  From the Migration Results page, select **Go to your website link**.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image61.png)

5.  This opens the app from the Azure website.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image62.png)

6.  Go to the Azure portal and check the resources that are created
    under the **ResourceGroup1** Resource group.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image63.png)

## Summary

This lab demonstrated how to migrate a To-Do List .NET application from
an on-premises environment to Azure App Service with the **App Service
Migration Assistant**. You assessed the application’s readiness for
Azure, created the required App Service resources, and executed the
migration process. Finally, you validated the application running in the
cloud. Through this exercise, you gained practical experience with a
streamlined migration approach that leverages Azure App Service’s
scalability, monitoring, and simplified management to bring traditional
web applications into the cloud.
