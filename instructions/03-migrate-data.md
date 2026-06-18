# Migrate existing data using Azure Data Factory

## Lab scenario

In Azure Data Factory, Azure Cosmos DB is supported as a source of data ingest and as a target (sink) of data output.
In this lab, we will populate Azure Cosmos DB using a helpful command-line utility and then use Azure Data Factory to move a subset of data from one container to another.

## Lab objectives

In this lab, you will complete the following tasks:
- Task 1: Create and seed your Azure Cosmos DB SQL API account.
- Task 2: Create Azure Data Factory resource.

### Task 1: Create and seed your Azure Cosmos DB for NoSQL account

In this task, you'll create and configure an Azure Cosmos DB for NoSQL account, followed by seeding it with a database and container using a command-line utility.

You will use a command-line utility that creates a **cosmicworks** database and a **products** container at **4,000** request units per second (RU/s). Once created, you will adjust the throughput down to 400 RU/s.

To accompany the products container, you will create a **flatproducts** container manually that will be the target of the ETL transformation and load operation at the end of this lab.

1. On Azure Portal page, in Search resources, services and docs (G+/) box at the top of the portal, enter **Azure Cosmos DB (1)**, and then select **Azure Cosmos DB (2)** under services.

   ![06](media/New-image1.png)
   
1. Select **+ Create (1)** under **Azure Cosmos DB for NoSQL** click on **Create (2)** to create  **Azure Cosmos DB for NoSQL** account.

    ![06](media/New-image2.png)

    ![06](media/New-image3.png)

1. Specify the following settings, leaving all remaining settings to their default values, and select **Next: Global Distribution (9)**:

    | Setting | Value |
    |----------|----------|
    | **Workload Type** | *Production* **(1)** |
    | **Subscription** | *Your existing Azure subscription* **(2)** |
    | **Resource group** | *Select an existing Cosmosdb-<inject key="DeploymentID" enableCopy="false"/>* **(3)** |
    | **Account Name** | *sql-<inject key="DeploymentID" enableCopy="false"/>* **(4)** |
    | **Availability Zones** | *Disabled* **(5)** |
    | **Location** | *Select any available location* **(6)** |
    | **Capacity mode** | *Provisioned throughput* **(7)** |
    | **Limit the total amount of throughput that can be provisioned on this account** | *Leave unchecked* **(8)** |

     ![06](media/DB1.png)

1. Click on **Next: Networking** on the Global Distribution page. Select **All networks (1)** for the **Connectivity method** and click on **Review + Create (2)**. 

     ![06](media/DB0002.png)

1. Click on **Create**.

    ![06](media/New-image5.png)

1. Wait for the deployment task to complete before continuing with this task.

1. Once deployment is completed, select **Go to resources**. 

    ![06](media/New-image6.png)

1. In the **Azure Cosmos DB account**, expand **Settings (1)** from the left menu, then select **Keys (2)**.

    ![06](media/DB15.png)

1. This pane contains the connection details and credentials necessary to connect to the account from the SDK. Specifically:

1. In the **Keys (1)** page, navigate to primary connection string and click the **Show (2)** icon to view the connection string, then click the **Copy (3)** icon to copy it and save it into notepad as e will use this in upcoming steps.

    ![06](media/conn-string-2304.png)

1. Keep the browser tab open, as we will return to it later.

1. On the LabVM select the **Visual Studio Code** shortcut.

    ![06](media/visualstudio.png)

1. In **Visual Studio Code**, open a new terminal by selecting **... (ellipses) (1)**, then **Terminal (2)**, and then **New Terminal (3)**.

    ![06](media/New-image36.png)

1. In the terminal run the following commad to install the [cosmicworks][nuget.org/packages/cosmicworks] command-line tool for global use on your machine.

    ```
    dotnet tool install cosmicworks --global --version 2.*
    ```

    > **Note:** This command may take a couple of minutes to complete. This command will output the warning message (*Tool 'cosmicworks' is already installed') if you have already installed the latest version of this tool in the past.

1. Once the Installation is completed, make sure to close the **Visual Studio Code** and re-open to perform the below command.

1. Run the following command cosmicworks to seed your Azure Cosmos DB account with the following command-line options:

    | **Option**       | **Value** |
    | ---------------- | ----------|
    | **CONNECTION STRING**   | *The Primary Connection String value you copied earlier in this lab* |

    ```
    cosmicworks --connection-string "<CONNECTION_STRING>" --disable-hierarchical-partition-keys 
    ```

    > **Note:** If you get an error while running the above command, **close** the visual studio code and **re-open** to run the above command.

    ![06](media/DB17.png)

1. Wait for the **cosmicworks** command to finish populating the account with a database, container, and items.

    ![06](media/DB16.png)

1. Close the integrated terminal.And close the **Visual Studio Code**.

    ![06](media/DB18.png)

1. Navigate back to the **Azure portal**.

1. On Azure Portal page, in Search resources, services and docs (G+/) box at the top of the portal, enter **Azure Cosmos DB (1)**, and then select **Azure Cosmos DB (2)** under 
   services.

   ![06](media/New-image1.png)

1. Select **sql-<inject key="DeploymentID" enableCopy="false"/>**.

     ![06](media/New-image68.png)

1. Within the **Azure Cosmos DB** account resource **overview page (1)** , navigate to the **Data Explorer (2)** pane. 

    ![06](media/DB04.png)

1. In the **Data Explorer**, expand the **cosmicworks (2)** database node, expand the **products (3)** container node, and then select **Items (4)**.

    ![06](media/DB19.png)

1. Observe and select the various JSON items in the **products** container. These are the items created by the command-line tool used in previous steps.

   ![06](media/New-image12.png)

1. Select the **Scale (1)** tab. In the Scale tab, select **Manual (2)**, update the **required throughput** setting from **4000 RU/s** to **400 RU/s (3)** and then **Save (4)** your changes**.

    ![06](media/step23-2304.png)

1. In the **Data Explorer** pane, select **+ New Container (1)** and select **+ New Container (2)**.

    ![06](media/New-image14.png)

1. In the **New Container** popup, enter the following values for each setting.

    | **Setting**   | **Value** |
    | ------------- | --------- |
    | **Database id** | *Use existing (1)* &vert; *cosmicworks (2)* |
    | **Container id** | *`flatproducts` (3)* |
    | **Partition key** | *`/category` (4)* |
    | **Check** the **Provision dedicated throughput for this container** (5)  |
    | **Container throughput (autoscale)** | *Manual (6)* |
    | **RU/s** | *`400` (7)* |
    
    ![06](media/step25-2304.png)

1. Scroll down and click on **OK**.

   ![06](media/New-image16.png)

1. Back in the **Data Explorer** pane, expand the **cosmicworks** database node and then observe the **flatproducts** container node within the hierarchy.

     ![06](media/New-image17.png)
   
1. Return to the **Home** of the Azure portal.

    > **Congratulations** on completing the lab! Now, it's time to validate it. Here are the steps:
    > - Hit the Validate button for the corresponding task. If you receive a success message, you have successfully validated the lab. 
    > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
    > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out. 

    <validation step="4f0ebcc4-a71c-450a-b7e0-5099feed58d5" />

### Task 2: Create Azure Data Factory resource

In this task, you will create an Azure Data Factory resource and configure it to perform a one-time ETL (Extract, Transform, Load) operation. The goal is to move data from one Azure Cosmos DB NoSQL container (products) to another container (flatproducts), applying a transformation during the process.

Now that the Azure Cosmos DB for NoSQL resources are in place, you will create an Azure Data Factory resource and configure all of the necessary components and connections to perform a one-time data movement from one NoSQL API container to another to extract data, transform it, and load it to another NoSQL API container

1. On Azure portal home page select **+ Create a resource**.
  
     ![06](media/New-image37.png)
   
1. On **Create a resource** page search and select **Azure Data Factory (1)**, and then create a new **Azure Data Factory (2** resource with the following settings, leaving

    ![06](media/New-image18.png)

1. On **Data Factory** select **Create (1)** and select **Data Factory (2)**.

    ![06](media/New-image19.png)

1. Specify the following settings, leaving all remaining settings to their default values. Click on **Next (6)**.

    | **Setting** | **Value** |
    | --- | --- |
    | **Subscription** | *Your existing Azure subscription* **(1)** |
    | **Resource group** | *Cosmosdb-<inject key="DeploymentID" enableCopy="false"/>* **(2)** |
    | **Name** | *datafactory-<inject key="DeploymentID" enableCopy="false"/>* **(3)**|
    | **Region** | *Choose any available region* **(4)**|
    | **Version** | *V2* **(5)**|

    ![06](media/New-image20.png)

1. In **Git configuration** blade select the checkbox for **Configure Git later (1)** and click on  **Review + Create (2)** and select **Create (3)**.

    ![06](media/DB21.png)

    ![06](media/DB23.png)

1. Once resource deployement is completed click on **Go to resource**

    ![06](media/New-image21.png)

1. In the **Resource group**, select the **datafactory** resource from the list.

    ![06](media/DB20.png)

1. Select **Launch studio** under **Azure Data Factory Studio**.

    ![06](media/New-image22.png)
   
    > &#128161; Alternatively, you can navigate to (``adf.azure.com/home``), select your newly created Data Factory resource, and then select the home icon.

1. From the **Home** screen. Select the **Ingest** option to begin the quick wizard to perform a one-time copy data at scale operation and move to the **Properties** step of the wizard.

   ![06](media/New-image23.png)

1. Starting with the **Properties** step of the wizard, in the **Task type** section, select **Built-in copy task (1)**. In the **Task cadence or task schedule** section, select **Run once now (2)** and then select **Next (3)** to move to the **Source** step of the wizard.

    ![06](media/New-image24.png)
   
1. In the **Source** step of the wizard, in the **Source type** list, select **Azure Cosmos DB NoSQL (1)** and in  the **Connection** section, select **+ New connection (2)**.

    ![06](media/New-image25.png)

1. In the **New connection (Azure Cosmos DB for NoSQL)** popup, configure the new connection with the following values, and then select **Create (9)**:

    | **Setting** | **Value** |
    | --- | --- |
    | **Name** | *`CosmosSqlConn`* **(1)** |
    | **Connect via integration runtime** | *AutoResolveIntegrationRuntime* **(2)** |
    | **Authentication method** | *Account key* **(3)** &vert; *Connection string* **(4)** |
    | **Account selection method** | *From Azure subscription* **(5)** |
    | **Azure subscription** | *Your existing Azure subscription* **(6)** |
    | **Azure Cosmos DB account name** | *Your existing Azure Cosmos DB account name you chose earlier in this lab* **(7)** |
    | **Database name** | *cosmicworks* **(8)**|

    ![06](media/New-image26.png)

1. Back in the **Source data store** section, within the **Source tables** section, select **Query (1)** and in the **Table name** list, select **products (2)**.

    ![06](media/New-image27.png)

1. In the **Query** editor, delete the existing content and enter the following query **(1)**:

    ```
    SELECT 
        p.name, 
        p.categoryName as category, 
        p.price 
    FROM 
        products p
    ```

1.  Scroll down and select **Preview data (2)** to test the query's validity. Select **Next (3)** to move to the **Destination** step of the wizard.

    ![06](media/New-image28.png)
   
1. In the **Destination** step of the wizard, in the **Destination type** list, select **Azure Cosmos DB for NoSQL (1)** and in the **Connection** list, select **CosmosSqlConn (2)** then in **Custom query** choose **flatproducts (3)** and then select **Next (4)** to move to the **Settings** step of the wizard.

   ![06](media/New-image29.png)

   >**Note:** It will take a minute to appear.

1. In the **Settings** step of the wizard, in the **Task name** field, enter **`FlattenAndMoveData` (1)**.And then select **Next (2)**

    ![06](media/New-image30.png)

1. Leave all remaining fields to their default blank values and then select **Next** to move to the final step of the wizard.

    ![06](media/New-image31.png)

1. Review the **Summary** of the steps you have selected in the wizard and then select **Finish**.

    ![06](media/New-image32.png)

1. Close your web browser window or tab.

1. Navigate to the **Azure portal**.

1. On Azure Portal page, in Search resources, services and docs (G+/) box at the top of the portal, enter **Azure Cosmos DB (1)**, and then select **Azure Cosmos DB (2)** under services.

   ![06](media/New-image1.png)

1. Select **sql-<inject key="DeploymentID" enableCopy="false"/>**.

1. Within the **Azure Cosmos DB** account resource, navigate to the **Data Explorer (1)** pane. In the **Data Explorer**, expand the **cosmicworks** database node, select the **flatproducts (2)** container node, and then select **New SQL Query (3)**.

    ![06](media/New-image33.png)

1. Delete the contents of the editor area.

1. Create a new SQL query that will return all documents where the **name** is equivalent to **HL Headset (1)** and select **Execute Query (2)**.

    ```
    SELECT 
        p.name, 
        p.category, 
        p.price 
    FROM
        products p
    WHERE
        p.name = 'HL Headset'
    ```

    ![06](media/New-image34.png)

1. Observe the results of the query.

    ![06](media/New-image35.png)

1. Close your web browser window or tab.

    > **Congratulations** on completing the lab! Now, it's time to validate it. Here are the steps:
    > - Hit the Validate button for the corresponding task. If you receive a success message, you have successfully validated the lab. 
    > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
    > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.

   <validation step="e513aa34-ca14-4de5-a2f3-139f051e5c35" />

### Summary 

This lab focuses on migrating data between two Azure Cosmos DB containers using Azure Data Factory. You will first set up an Azure Cosmos DB for NoSQL account and populate it with sample data. Then, you'll create an Azure Data Factory resource to perform a one-time ETL operation, extracting data from one container, transforming it, and loading it into another container. The lab demonstrates how to integrate Azure services for efficient data movement and transformation.
 
### Review

In this lab, you have completed:

- Created and seeded your Azure Cosmos DB NoSQL API account.
- Created Azure Data Factory resource.

### You have successfully completed the lab.
