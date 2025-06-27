# Optimize an Azure Cosmos DB for a NoSQL container's indexing policy for common operations

## Lab scenario

For write-heavy workloads or workloads with large JSON objects, it can be advantageous to optimize the indexing policy to only index properties that you know you will want to use in your queries.

In this lab, we will use a test .NET application to insert a large JSON item into an Azure Cosmos DB SQL API container using the default indexing policy and then using an indexing policy that has been tuned slightly.

## Lab objectives

In this lab, you will complete the following tasks:
- Task 1: Prepare your development environment.
- Task 2: Create an Azure Cosmos DB for NoSQL account.
- Task 3: Run the .NET test application using the default indexing policy.
- Task 4: Update the indexing policy and rerun the .NET application.

### Estimated Timing: 60 minutes

## Architecture Diagram

![image](architecturedia/lab23.png)

### Task 1: Prepare your development environment

In this task, you will prepare your development environment for working with Azure Cosmos DB by setting up Visual Studio Code.

1. Start Visual Studio Code (the program icon is pinned to the Desktop).

   ![Visual Studio Code Icon](./media/vscode1.jpg)

1. Click on the **Extensions** view icon on the left sidebar **(1)**, type `C#` in the search bar **(2)**, select the **C# extension by Microsoft** **(3)**, and click **Install (4)** to add language support for C# in Visual Studio Code.

   ![06](media/New-image50.png)

1. Click **File (1)** on the top left of Visual Studio Code, then choose **Open Folder... (2)**.

   ![06](media/New-image51.png)

1.  Navigate to **C:\AllFiles**, select **dp-420-cosmos-db-dev-main**, and click on **Select Folder**.
   
    ![06](media/New-image54.png)

1. When the **Do you trust the author of the files in this folder** pop-up appears, click on **Yes, I trust the authors**.

   ![06](media/lab12-2.png)

### Task 2: Create an Azure Cosmos DB for NoSQL account

In this task, you will provision an Azure Cosmos DB SQL account, configure essential settings, and retrieve the necessary connection details for future development.

Azure Cosmos DB is a cloud-based NoSQL database service that supports multiple APIs. When provisioning an Azure Cosmos DB account for the first time, you will select which of the APIs you want the account to support (for example, **Mongo API** or **NoSQL API**). Once the Azure Cosmos DB for NoSQL account is done provisioning, you can retrieve the endpoint and key and use them to connect to the Azure Cosmos DB for NoSQL account using the Azure SDK for .NET or any other SDK of your choice.

1. On **Azure Portal** page, in Search resources, services and docs (G+/) box at the top of the portal, enter **Azure Cosmos DB**, and then select **Azure Cosmos DB** under services.

   ![06](media/New-image1.png)

1. Select **+ Create** under **Azure Cosmos DB for NoSQL** click on **Create** to create **Azure Cosmos DB for NoSQL** account.

    ![06](media/25-06-l2-1.png)

    ![06](media/25-06-l2-2.png)
   
1.  Specify the following settings, leaving all remaining settings to their default values, and select **Next: Global Distribution (8)**:

    | **Setting**         | **Value** |
    | --------------------|--------------------------------------------------- |
    | **Workload Type**   | *Production* (1) |
    | **Subscription**    | *Your existing Azure subscription* (2) |
    | **Resource group**  | *Select an existing Cosmosdb-<inject key="DeploymentID" enableCopy="false"/>* (3) |
    | **Account Name**    | *sql-<inject key="DeploymentID" enableCopy="false"/>* (4) |
    | **Location**        | *Choose the default region* (6) |
    | **Capacity mode**   | *Serverless* (7) |

     ![06](media/25-06-l2-3.png)

     ![06](media/25-06-25-l9-1.png)
    
1. Click on **Next: Networking** on the Global Distribution page. Select **All networks (1)** for the **Connectivity method** and click on **Review + Create (2)**. 

   ![06](media/25-06-l2-5.png)
   
1. Once validation passed, click on **Create**.

   ![06](media/25-06-25-l9-2.png)
   
1. Wait for the deployment task to complete before continuing with this task. Select **Go to resources**.

   ![06](media/25-06-l2-7.png)

1. Navigate to the **Data Explorer** pane under your Cosmos DB resource. In the **Data Explorer** pane, select **+ New Container (1)** and then click **+ New Container (2)**.

   ![06](media/New-image107.png)

1. In the **New Container** popup, enter the following values for each setting, and then select **OK (5)**:

   | **Setting** | **Value** |
   | :-- | :-- |
   | **Database id** | *Create new*  **(1)**&vert; *``cosmicworks``* **(2)** |
   | **Container id** | *``products``*  **(3)**|
   | **Partition key** | *``/categoryId``*  **(4)**|

   ![06](media/New-image108.png)

1. Back in the **Data Explorer (1)** pane, expand the **cosmicworks (2)** database node and then observe the **products (3)** container node within the hierarchy.

   ![06](media/25-06-25-l9-6.png)

1. From the left navigation menu under **Settings (1)** section, navigate to the **Keys (2)** pane.

   ![06](media/25-06-l2-8.png)

1. This pane contains the connection details and credentials necessary to connect to the account from the SDK. Specifically:

   - Record the value of the **URI** field. You will use this **endpoint** value later in this exercise.

   - Record the value of the **PRIMARY KEY** field. You will use this **key** value later in this exercise.

     ![06](media/25-06-25-l3-3.png)

1. Return to **Visual Studio Code**.

   > **Congratulations** on completing the lab! Now, it's time to validate it. Here are the steps:
   > - Hit the Validate button for the corresponding task. If you receive a success message, you have successfully validated the lab. 
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.

   <validation step="0e380f12-d3fe-4670-a788-3fa3a3687768" />

### Task 3: Run the .NET test application using the default indexing policy

In this task, you will run a pre-built .NET application that inserts a large JSON object into an Azure Cosmos DB for NoSQL container.

1. In the **Explorer** pane, browse to the **23-index-optimization** folder.

1. Right-click on the **23-index-optimization (1)** folder and then select **Open in Integrated Terminal (2)** to open a new terminal instance.

   ![06](media/25-06-25-l11-1.png)
   
   > **Note:** This command will open the terminal with the starting directory already set to the **23-index-optimization** folder.

1. Build the project using the [dotnet build](https://docs.microsoft.com/dotnet/core/tools/dotnet-build) command.

   ```
   dotnet build
   ```

   > **Note:** You may see a compiler warning that the **endpoint** and **key** variables are current unused. You can safely ignore this warning, as you will use these Variables in this task.

1. Close the integrated terminal.

1. Open the **script.cs** code file.

1. Locate the **string** variable named **endpoint**. Set its value to the **endpoint** of the Azure Cosmos DB account you created earlier.

   ```
   string endpoint = "<cosmos-endpoint>";
   ```

   > **For example:** if your endpoint is: **https&shy;://dp420.documents.azure.com:443/**, then the C# statement would be: **string endpoint = 
   "https&shy;://dp420.documents.azure.com:443/";**.

1. Locate the **string** variable named **key**. Set its value to the **key** of the Azure Cosmos DB account you created earlier.

   ```
   string key = "<cosmos-key>";
   ```

   > **For example:** if your key is: **fDR2ci9QgkdkvERTQ==**, then the C# statement would be: **string key = "fDR2ci9QgkdkvERTQ==";**.

1. **Save** the **script.cs** code file.

1. In **Visual Studio Code**, right-click on the **23-index-optimization** folder and then select **Open in Integrated Terminal** to open a new terminal instance.

   ![06](media/25-06-25-l11-1.png)

1. Build and run the project using the [dotnet run](https://docs.microsoft.com/dotnet/core/tools/dotnet-run) command.
   ```
   dotnet run
   ```
1. Observe the output from the terminal. The item's unique identifier and the operation's request charge (in RUs) should be printed to the console.

   ![06](media/25-06-25-l11-2.png)

1. Build and run the project at least two more times using the **[dotnet run][docs.microsoft.com/dotnet/core/tools/dotnet-run]** command. Observe the RU charge in the 
   console output:

   ```
   dotnet run
   ```
1. Leave the integrated terminal open.

   > **Congratulations** on completing the lab! Now, it's time to validate it. Here are the steps:
   > - Hit the Validate button for the corresponding task. If you receive a success message, you have successfully validated the lab. 
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out. 

   <validation step="e487cd8b-7edb-4b80-a103-3036d37a92b4" />

### Task 4: Update the indexing policy and rerun the .NET application

In this task, you will assume that our future queries focus primarily on the name and categoryName properties. To optimize for our large JSON item, you will exclude all other fields from the index by creating an indexing policy that starts by excluding all paths. Then the policy will selectively include specific paths.

1. Return to your web browser.

1. Within the **Azure Cosmos DB** account resource, navigate to the **Data Explorer (1)** pane.

1. In the **Data Explorer**, expand the **cosmicworks (2)** database node, expand the **products (3)** container node, and then select **Settings (4)**.

   ![06](media/25-06-25-l11-3.png)

1. In the **Settings** tab, navigate to the **Indexing Policy (5)** section.

   ![06](media/25-06-25-l11-4.png)

1. Observe the default indexing policy:

   ```
     {
        "indexingMode": "consistent",
        "automatic": true,
        "includedPaths": [
          {
            "path": "/*"
          }
        ],
        "excludedPaths": [
          {
            "path": "/\"_etag\"/?"
          }
        ]
      }    
   ```

1. Replace the indexing policy with this **modified JSON object (1)** and then **Save (2)** the changes:

   ```
      {
         "indexingMode": "consistent",
         "automatic": true,
         "includedPaths": [
           {
             "path": "/name/?"
           },
           {
             "path": "/categoryName/?"
           }
         ],
         "excludedPaths": [
           {
             "path": "/*"
           },
           {
             "path": "/\"_etag\"/?"
           }
         ]
      }
   ```

   ![06](media/25-06-25-l11-5.png)

1. Return to **Visual Studio Code**. Return to the open terminal.

1. Build and run the project at least two more times using the **[dotnet run][docs.microsoft.com/dotnet/core/tools/dotnet-run]** command. Observe the new RU charge in the console output, which should be significantly less than the original charge. Since you are not indexing all the item properties, your write cost is significantly lower when updating the index. This, however, can cost you greatly if your reads will need to query on properties that are not indexed.

   ```
   dotnet run
   ```
   ![06](media/25-06-25-l11-6.png)
   
   > **Note:** If you are not seeing an updated RU charge, you may need to wait a couple of minutes.

1. Return to your web browser.

   >**Note:** If the **Indexing Policy** page is not open, go to **Data Explorer**, expand the **cosmicworks** database node, expand the **products** container node, 
    select **Settings** and navigate to the **Indexing Policy** section.

1. Replace the indexing policy with this modified JSON object and then **Save** the changes:

   ```
    {
      "indexingMode": "none"
    }
   ```

   ![06](media/25-06-25-l11-7.png)

1. Close your web browser window or tab.

1. Return to **Visual Studio Code**. Return to the open terminal.

1. Build and run the project at least two more times using the **[dotnet run][docs.microsoft.com/dotnet/core/tools/dotnet-run]** command. Observe the new RU charge in 
   the console output, which should be much less than the original charge.  How can this be? Since this script measures the RUs when you write the item, by choosing to 
   have no index, there is no overhead in maintaining that index. The flip side to this is that while your writes will generate fewer RUs, your reads will be very costly.

   ```
   dotnet run
   ```

   ![06](media/25-06-25-l11-8.png)

   > **Note:** If you are not seeing an updated RU charge, you may need to wait a couple of minutes.

1. Close **Visual Studio Code**.

### Summary

In this lab, you explored how to optimize the indexing policy of an Azure Cosmos DB for NoSQL container for write-heavy workloads, particularly with large JSON objects. The goal was to enhance performance by limiting the fields that are indexed, reducing the RU (Request Unit) charge during write operations.

### Review

In this lab, you have completed:

- Prepared your development environment.
- Created an Azure Cosmos DB for NoSQL account.
- Ran the test .NET application using the default indexing policy.
- Updated the indexing policy and rerun the .NET application.

### You have successfully completed the lab
