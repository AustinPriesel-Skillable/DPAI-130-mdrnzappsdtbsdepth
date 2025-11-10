### Lab 3 - Migrate on-premise PostgreSQL to Azure Database for PostgreSQL flexible server

### Time Duration – 60 mins

## Scenario

Terra Firm Laboratories is a global bioengineering company that’s the
leading researcher and innovator in genetic and biological science
technology. The company was founded in 1975 with its corporate
headquarters in Palo Alto, CA. Their mission-critical workloads are
currently hosted in an on-premises datacenter and are beginning a
journey to modernize and migrate into the cloud using Microsoft Azure.

The CTO, Dennis Nedry, has kicked off an initiative for the organization
to begin adopting the Microsoft Azure cloud and modernize its
infrastructure. He has already had his team begin assessing their
current environment and what it will take to migrate to the cloud. They
are looking to optimize their technology investments by reducing
technical debt and streamlining operations using Azure
Infrastructure-as-a-Service (IaaS) and Platform-as-a-Service (PaaS)
cloud services.

Terra Firm Laboratories has already completed the first round of
analysis to identify the applications and workloads to begin migrating
first. Out of over 250 workloads they have running on-premises, they
have identified one workload that exemplifies multiple components common
across many of their other workloads. They decided to use a simpler
workload that will still assist in proving out the migration plan as
it’s developed. This workload includes a web-based order system written
in PHP that is running on Red Hat Enterprise Linux (RHEL) servers
connecting to a PostgreSQL database on-premises.

This lab will cover the database migration part alone. In this lab,
attendees will perform the migration of the on-premises database to an
Azure Database for PostgreSQL Flexible Server - a managed database
service.

Azure services and related products

- Azure VMs

- Azure Arc

- Azure Database for PostgreSQL - Flexible Server

- Azure Networking

- Migration service in Azure Database for PostgreSQL

- Azure Data Studio

- Azure Defender

- Azure Firewall

## Solution Architecture

![Diagram showing the proposed solution
architecture](./media/image1.png)

In this lab we are going to:

- Provisioning a Linux Server VM

- Set up a Linux Server for application migration to Azure

- Migrate an on-premises PostgreSQL Database to Azure Database for
  PostgreSQL - Flexible Server

# Exercise 1 - Lab Setup

In this exercise, you will perform steps toward migrating Terra Firm
Laboratories on-premises Linux Servers and PostgreSQL workloads to
Azure. Terra Firm Laboratories needs a new Linux Server VM created in
Azure for hosting their Web application, an on-premises PostgreSQL
database migrated to Azure PostgreSQL PaaS, a secure Linux Server, and
an on-premises Linux Server VM to be Azure Arc-enabled. Terra Firm
Laboratories already has a Hub and Spoke network setup in Azure with
Azure Bastion for enabling secure remote management of Azure VMs using
Azure Bastion. The Azure resources provisioned throughout this lab will
be deployed into this environment.

### Objectives

After completing this exercise, you will be able to:

- Deploy ARM Template

- Verify the on-premises web application

## Task 1 – Create resources

In this task, you will leverage a custom Azure Resource Manager (ARM)
template to deploy the Azure resources and create a simulated
on-premises environment for Terra Firm Laboratories.

1.  Open a browser and navigate to +++https://portal.azure.com/+++. Now,
    sign in with your cloud slice account:

    - Username - +++@lab.CloudPortalCredential(User1).Username+++
    - Password - +++@lab.CloudPortalCredential(User1).Password+++

    ![](./media/image2.png)
    ![](./media/image3.png)
    ![](./media/image4.png)
    ![](./media/image5.png)

2.  Open a new tab in the browser, and navigate to the following link to
    get the ARM template: 

    +++https://github.com/technofocus-pte/MigrateLinuxworkloads/tree/main/resources/deployment+++

3.  Select **Deploy to Azure**. This will open a new browser tab to the
    Azure Portal for custom deployments.
    
    ![The GitHub page with Deploy to
    Azure button highlighted.](./media/image6.png)

4.  Fill in the required ARM template parameters.

    - **Subscription:** Use the default one

    - **Resource group:** Select **ResourceGroup1**

    - **Region:** @lab.CloudResourceGroup(ResourceGroup1).Location

    - **Resource Name Base:** Enter +++terrafirm@lab.LabInstance.Id+++

    - **Password:** Enter +++pass@dmIn234+++

    Select **Review + create**

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image7.png)

5.  Click on the **Create** button to start deployment.

    ![](./media/image8.png)

6.  The deployment is now underway. On average, this process can take
    10-20 minutes to complete.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image9.png)

    **Note**: While automation can make things simpler and repeatable,
    sometimes it can fail. If at any time during the ARM template deployment
    there is a failure, review the failure, delete the Resource Group, and
    try the ARM template again. Or review the failures and adjust for errors
    as appropriate.

7.  Once the ARM template is deployed successfully, the status will
    change to complete. Click on **Go to resource group** to open the
    resource group.

    ![](./media/image10.png)

# Task 02 - Configure on-premises web application

In this task, you will configure the web application hosted on the
simulated on-premises APP virtual machine that was provisioned by the
ARM Template deployment.

1.  Select the **On-premises Workload VM** named similar
    to **terrafirm@lab.LabInstance.Id-onprem-workload-vm**.

    ![](./media/image11.png)

2.  In the **Overview** page, go to **Properties**, and under the
    **Networking** section, locate the **Private IP Address** of the VM
    and copy it into Notepad.

    **Note:** You will need this IP address to configure the web application
    to use the database workload server.

    ![](./media/image12.png)

3.  Navigate back to the **ResourceGroup1**, then select
    the **On-premises APP VM** named similar
    to **terrafirm@lab.LabInstance.Id-onprem-app-vm**.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image13.png)

4.  Search for +++**Bastion**+++ in the left-hand menu and then select
    **Bastion**. We will use a bastion host as the method to connect to
    our VMs, as this is a more secure method.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image14.png)

5.  Within the **Bastion** page, enter the following details:

    - **Authentication Type:** VM Password

    - **Username:** Enter +++demouser+++

    - **VM Password:** Enter +++pass@dmIn234+++

    Then click on the **Connect** button to connect with Bastion.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image15.png)

    **Note**: You may need to **allow pop-ups** if they are blocked in
    your browser.

6.  When connected to the VM via the Bastion host, you will get a screen
    like this:

    ![](./media/image16.png)

    **Note**: If you see a pop-up stating “See test and images copied to the
    clipboard”. Click **Allow**.

7.  Once connected via Bastion, enter the following command to install
    the git utility on the server.

    +++sudo yum install git -y+++

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image17.png)

8.  Enter the following command to clone the remote git repository
    holding a script which will configure the web app on the application
    server.

    +++sudo git clone https://github.com/technofocus-pte/TechExcel-Migrate-Linux-workloads.git+++

    ![](./media/image18.png)

9.  You can now run the configuration script by using the following
    command:

    +++sudo bash TechExcel-Migrate-Linux-workloads/resources/deployment/onprem/APP-workload-install.sh+++

    ![](./media/image19.png)
    
    You will get a status message of  “The script as successful”.

    ![](./media/image20.png)

11. Execute the following command to open the **orders.php** file for
    the web application in a text editor. The application needs to be
    configured to connect to the **Azure Database for PostgreSQL Flexible Server** database.

    +++sudo nano /var/www/html/orders.php+++

    ![](./media/image21.png)

    ![](./media/image22.png)

12. Use the **down arrow** key to scroll down in the order.php file
    until you locate $host, $port, $dbname, $user, and $password.  
    
    ![](./media/image23.png)

13. Check the host IP address and configure it to match the **Private IP
    Address** of the **terrafirm@lab.LabInstance.Id-onprem-workload-vm** instance. If the host IP is already correct, skip to steps 13 and 14. And press **Ctrl+X** to exit the editor.

    ![](./media/image24.png)

14. If the host IP is not the same, then replace it with the **Private
    IP Address** of the **terrafirm-onprem-workload-vm** instance that
    was copied in step 2. Then press **Ctrl+X**.

    ![](./media/image25.png)

15. Press +++**Y**+++ to save the modified buffer and then press
    **Enter** to write the changes in the file.

    ![](./media/image26.png)

    ![](./media/image27.png)

16. You are now exited from the orders.php file with the changes saved.

    ![](./media/image28.png)

    You have now learnt some basic Linux commands and configured the web
    application to use the database on an internal network rather than
    across the internet.

# Task 03 - Validate on-premises web application

In this task, you will validate the web application hosted on the
simulated on-premises APP virtual machine that was provisioned by the
ARM Template deployment.

1.  Navigate back to Azure Portal, open the ResourceGroup1, then select
    the **On-premises APP VM** named similar
    to **terrafirm@lab.LabInstance.Id-onprem-app-vm**.

    ![](./media/image29.png)

2.  In the **Overview** window, go to **Properties**. Under the
    **Networking** section, locate the VM’s **Public IP Address** and
    copy it into Notepad.

    ![](./media/image30.png)

3.  Open a new browser window, then navigate to the
    following **http:// URL** to access the simulated on-premises web
    application provisioned for this lab. Be sure to replace
    the **ip-address** placeholder with the Public IP Address for
    the VM. For example: http://172.206.126.43

    +++http://ip-address+++

    **Note:** You should get the Red Hat Enterprise Linux Test Page 

    ![](./media/image31.png)

4.  When the web page loads, enter the following at the end of the URL.
    For example: "http://172.206.126.43/orders.php"

    +++http://ip-address/orders.php+++  
    
    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image32.png)

Now, things are ready for you to go through the lab.

# Exercise 2 - Migrate a PostgreSQL Database

In this exercise, you will migrate the on-premises PostgreSQL database
for the web application workload to Azure. The migration service in
Azure Database for PostgreSQL will be used to perform the database
migration from the PostgreSQL server on-premises to the Azure Database
for PostgreSQL service.

### Objectives

After you complete this exercise, you will be able to:

- Create an Azure Database for PostgreSQL - Flexible Server instance

- Create a database hosted in this instance, which will be the target
  for migration

- Set up a migration project in Azure Database for PostgreSQL Flexible
  Server

- Migrate your database from on-premises to Azure Database for
  PostgreSQL Flexible Server

## Task 01 - Create Azure Database for PostgreSQL

In this task, you will create a new PostgreSQL database that will be the
target for the database migration.

1.  Navigate back to the Azure portal-\>Home-\>Create a resource.

    ![](./media/image33.png)

2.  In the **Marketplace** window, search for +++**PostgreSQL
    flexible**+++, then select **Azure Database for PostgreSQL Flexible
    Server** from the search results.

    ![](./media/image34.png)

3.  Click **Create** and then select **Azure Database for PostgreSQL
    Flexible Server**.

    ![A screenshot of a phone AI-generated content may be
    incorrect.](./media/image35.png)

4.  On the **New Azure Database for PostgreSQL Flexible
    Server** pane, select the following values:

    1.  **Subscription:** Keep default

    2.  **Resource group:** Select **ResourceGroup1**

    3.  **Server name:** Enter
        +++**terraform-postgresql-db@lab.LabInstance.Id**+++

    4.  **Region:** Select
        @lab.CloudResourceGroup(ResourceGroup1).Location

    5.  **PostgreSQL version:** Keep the default, as it always selects
        the latest version

    6.  **Workload type:** Select **Development**

    ![](./media/image36.png)

5.  Under **Compute + storage,** click **Configure server**.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image37.png)

6.  On the Compute + storage window, under the **compute** section,
    choose the following:

    1.  **Compute tier:** General Purpose (2-64 vCores) - Balanced
        configuration for most common workloads.

    2.  **Compute Size:** Standard_D4ds_v4 (4 vCores, 16GiB memory, 6400
        max iops)

    ![](./media/image38.png)

7.  Under the **High availability** section, choose **Disabled (99.9%
    SLA)** and then click **Save.**

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image39.png)

8.  On the **New Azure Database for PostgreSQL** **Flexible Server**
    window, under the **Authentication** section, enter the following
    details:

    1.  **Authentication method**: Choose **PostgreSQL authentication
        only**

    2.  **Admin username**: Enter +++pgadmin+++

    3.   **Password**: Enter +++passadmin3@2+++

    Select **Next: Networking**

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image40.png)

9.  In the **Networking** tab, under **Public access**, clear the
    checkbox to disable public access.![](./media/image41.png)

10. Under **Private endpoint** section, select **Create private
    endpoint.** ![](./media/image42.png)

11. On the **Create private endpoint** window, enter the following
    details:

    1.  **Subscription:** Keep default

    2.  **Resource group:** ResourceGroup1

    3.  **Location**: @lab.CloudResourceGroup(ResourceGroup1).Location

    4.  **Name:** Enter +++post-priv-endpoint+++

    5.  **Virtual network:** Select
        **terraform@lab.LabInstance.Id-spoke-vnet(ResourceGroup1)**

    6.  **Subnet**: Select **default**(10.2.0.0/24)

    7.  **Integrate with privet DNS zone:** Select No (You will use an
        IP address rather than a DNS entry when connecting.)

    8.  Click **OK**

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image43.png)

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image44.png)

12. Select **Review + create**.

    ![](./media/image45.png)

13. Select **Create** to provision the service.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image46.png)

14. Click **Create server without firewall rules** - as you will use the
    private endpoint for access.
    
    ![A screenshot of a computer AI-generated
    content may be incorrect.](./media/image47.png)

15. Wait for deployment to complete; it will take 5-10 mins. Once
    provisioning has finished, click on **Go to resource**.

    ![](./media/image48.png)

16. In the Overview tab of the **Azure Database for PostgreSQL flexible
    server,**  copy and save the **Server name** in Notepad for use
    later.

    ![](./media/image49.png)

17. On the left menu pane,
    under **Settings,** select **Databases**.
    
    ![Databases link is
    hihghlighted.](./media/image50.png)

18. Select **+ Add** to create a new database.

    ![Add database button is
    highlighted.](./media/image51.png)

19. On the **Create Database** window, enter +++**northwind**+++ in
    the Name field, then select **Save**. This will create a new
    PostgreSQL database that will be the target for the database
    migration.
    
    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image52.png)

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image53.png)

## Task 02 - Migrate your database to Azure Database for PostgreSQL flexible server

In this task, you will set up a migration project and configure the
Source and Target connections. You will then execute and monitor a
migration of your on-premises PostgreSQL database into Azure Database
for PostgreSQL - flexible server.

1.  Select **Migration** from the menu on the left of the flexible
    server blade.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image54.png)

2.  Click on the **+ Create**.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image55.png)

    **Note**: If the **+ Create** option is unavailable,
    select **Compute + storage** and change the compute tier to
    either **General Purpose** or **Memory Optimized** and try to create
    the Migration process again. After the Migration is successful, you
    can change the compute tier back to **Burstable**.

3.  On the **Setup** tab, enter each field as follows:

    1.  **Migration name:** +++Migration-northwind+++

    2.  **Source server type:** On-premise Server.

    3.  **Migration option:** Validate and Migrate.

    4.  **Migration option:** Offline

    5.  Select **Next: Runtime server \>**

    ![](./media/image56.png)

    **Note**: The Runtime server \> button might be enabled after 20-30 mins.

4.  We will **not** use a Runtime Server, so just select **Next: Source
    server \>**.

5.  On the **Source server** tab, enter each field as follows:

    1.  Server name: The public IP address of the
        “terraform-onprem-workload-vm”.

        ![A screenshot of a computer AI-generated content may be
        incorrect.](./media/image57.png)

    2.  **Port:** 5432

    3.  **Server admin login name:** rootuser(the VM has been setup with an
        admin user called rootuser)

    4.  **Password:** Enter+++123rootpass456+++

    5.  **SSL mode:** Prefer.

    6.  Click on the **Connect to source** option to validate the
        connectivity details provided.

    7.  Click on the **Next: Target server\>** button to progress.

        ![](./media/image58.png)

6.  The connectivity details should be automatically completed for the
    target server we are migrating to.

    1.  In the password field -  Enter +++passadmin3@2+++

    2.  Click on the **Connect to target** option to validate the
        connectivity details provided.

    3.  Click on the **Next: Databases to validate or migrate
        \>** button to progress.

    ![](./media/image59.png)

7.  On the **Databases to validate or migrate** tab, select the
    **northwind** database because you want to migrate to the flexible
    server. Then click on the **Next : Summary \>** button to progress
    and review the data provided

    ![](./media/image60.png)

8.  On the **Summary** tab, review the information and then click
    the **Start Validation and Migration** button to start the migration
    to the flexible server.

    ![](./media/image61.png)

9.  On the **Migration** tab, you can monitor the migration progress by
    using the **Refresh** button in the top menu to view the progress
    through the validation and migration process.
    
    ![Progress of the
    migration to Azure Database for PostgreSQL Flexible
    Server.](./media/image62.png)

10. By clicking on the **Migration-northwind** activity, you can view
    detailed information about the migration activity’s progress.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image63.png)
