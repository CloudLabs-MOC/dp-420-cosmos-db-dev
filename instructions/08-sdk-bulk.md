# Move multiple documents in bulk with the Azure Cosmos DB for NoSQL SDK

## Lab scenario

The easiest way to learn how to perform a bulk operation is to attempt to push many documents to an Azure Cosmos DB for NoSQL account in the cloud. Using the bulk features of the SDK, this can be done with some minor help from the [System.Threading.Tasks][docs.microsoft.com/dotnet/api/system.threading.tasks] namespace.

In this lab, you'll use the [Bogus][nuget.org/packages/bogus/33.1.1] library from NuGet to generate fictional data and place that into an Azure Cosmos DB account.

## Lab objectives

In this lab, you will complete the following tasks:
- Task 1: Create an Azure Cosmos DB for NoSQL account and configure the SDK project.
- Task 2: Bulk inserting twenty-five thousand documents.
- Task 3: Observe the results.

## Estimated Timing: 30 minutes

## Architecture Diagram

![image](architecturedia/lab8.png)

## Prepare your development environment

1. Start Visual Studio Code (the program icon is pinned to the Desktop).

   ![Visual Studio Code Icon](./media/vscode1.jpg)

3. Select the **Extension (1)** icon from the left pane. Enter **C# (2)** in the search bar and select the **Extension (3)** that shows up and finally **Install (4)** on the extension. 

    ![](media/visualstudioo.png)

4. Select the **file** option on the top left of the screen, from the pane options, select **Open Folder** and navigate to **C:\AllFiles**.

5. Select the folder **dp-420-cosmos-db-dev-main** and click on **Select Folder**.

   ![06](media/New-image54.png)

   >**Note:** On **Do you trust the authors of the files in this folder?** pop-up, select **Yes, I trust authors**.

    ![06](media/DB24.png)

### Task 1: Create an Azure Cosmos DB for NoSQL account and configure the SDK project

In this task, you will create an Azure Cosmos DB for NoSQL account, configure it with essential settings, and prepare the SDK project in Visual Studio Code to interact with your newly established database.

1. On Azure Portal page, in Search resources, services and docs (G+/) box at the top of the portal, enter **Azure Cosmos DB (1)**, and then select **Azure Cosmos DB (2)** under services.

   ![06](media/New-image1.png)
   
1. Select **+ Create (1)** under **Azure Cosmos DB for NoSQL** click on **Create (2)** to create  **Azure Cosmos DB for NoSQL** account.

    ![06](media/New-image2.png)

    ![06](media/New-image3.png)

1. Specify the following settings, leaving all remaining settings to their default values and select **Review + create (10)**:

    | **Setting**         | **Value** |
    | --------------------|--------------------------------------------------- |
    | **Workload Type**   | *Production* (1) |
    | **Subscription**    | *Your existing Azure subscription* (2) |
    | **Resource group**  | *Select an existing Cosmosdb-<inject key="DeploymentID" enableCopy="false"/>* (3) |
    | **Account Name**    | *sql-<inject key="DeploymentID" enableCopy="false"/>* (4) |
    | **Location**        | *Choose the default region* (5) |
    | **Capacity mode**   | *Provisioned throughput* (6) |
    | **Apply Free Tier Discount** | *Do Not Apply* (7) |
    | **Limit the total amount of throughput that can be provisioned on this account** | *Unchecked* (8) |

     ![06](media/DB25.png)

1. Click on **Create**.

    ![06](media/New-image5.png)

1. Wait for the deployment task to complete before continuing with this task.

1. Once deployment is completed, select **Go to resources**. 

    ![06](media/New-image6.png)

1. In the **Azure Cosmos DB account**, expand **Settings (1)** from the left menu, then select **Keys (2)**.

    ![06](media/DB15.png)

1. This pane contains the connection details and credentials necessary to connect to the account from the SDK. Specifically:

   -  Copy the **URI** field. You will use this **endpoint** value later in this exercise.

   - Copy the **PRIMARY KEY** field. You will use this **key** value later in this exercise.

       ![06](media/New-image9.png)

1. Within the **Azure Cosmos DB** account resource **overview page (1)** , navigate to the **Data Explorer (2)** pane. 

    ![06](media/DB04.png)

1. In the **Data Explorer (1)** page, click **New (2)**, then select **New Container (3)**.

     ![06](media/DB42.png)

1. In the **New Container** pane, enter the following details and click **OK (6)**.

    | **Setting** | **Value** |
    | :--- | :--- |
    | **Database id** | Create new \| `cosmicworks` **(1)** |
    | **Share throughput across containers** | Unchecked **(2)** |
    | **Container id** | `products` **(3)** |
    | **Partition key** | `/categoryId` **(4)** |
    | **Container throughput** | Autoscale \| `4000` **(5)** |

    ![06](media/DB44.png)

1. Return to **Visual Studio Code**.. 

1. In **Visual Studio Code**, in the **Explorer** pane, browse to the **08-sdk-bulk** folder.

1. In **Visual Studio Code**, in the **06-sdk-crud (1)** folder open the empty **script.cs (2)** code file.

    ![06](media/DB45.png)

    >**Note**: The **[Microsoft.Azure.Cosmos][nuget.org/packages/microsoft.azure.cosmos/3.22.1]** library has already been pre-imported from NuGet.

1. Locate the **string** variable named **endpoint**. Set its value to the **endpoint** of the Azure Cosmos DB account you created in the previous lab.
  
    ```
    string endpoint = "<cosmos-endpoint>";
    ```

    >**Note**: For example, if your endpoint is: **https&shy;://dp420.documents.azure.com:443/**, then the C# statement would be: **string endpoint = "https&shy;://dp420.documents.azure.com:443/";**.

1. Locate the **string** variable named **key**. Set its value to the **key** of the Azure Cosmos DB account you created in the previous lab.

    ```
    string key = "<cosmos-key>";
    ```

    >**Note**: For example, if your key is: **fDR2ci9QgkdkvERTQ==**, then the C# statement would be: **string key = "fDR2ci9QgkdkvERTQ==";**.

1. Press **Ctrl+S** to **Save** the script.cs code file.

1. Open the context menu for the **08-sdk-bulk (1)** folder and then select **Open in Integrated Terminal (2)** to open a new terminal instance.
 
    ![](media/DB46.png)

   >**Note**: This command will open the terminal with the starting directory already set to the **08-sdk-bulk** folder.

1. Add the [Microsoft.Azure.Cosmos][nuget.org/packages/microsoft.azure.cosmos/3.22.1] package from NuGet using the following command:

    ```
    dotnet add package Microsoft.Azure.Cosmos --version 3.22.1
    ```

1. Build the project using the [dotnet build][docs.microsoft.com/dotnet/core/tools/dotnet-build] command:

    ```
    dotnet build
    ```

1. Close the integrated terminal.

    > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
    > - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
    > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
    > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.

    <validation step="8b577a01-0d31-4606-8273-71efbf77241f" />

### Task 2: Bulk inserting twenty-five thousand documents

In this task, we will try to insert a lot of documents to see how this works. In our internal testing, this can take approximately 1-2 minutes if the lab virtual machine and Azure Cosmos DB for NoSQL account are relatively close to each other geographically speaking.

1. Return to the editor tab for the **script.cs** code file.

1. Create a new instance of the [CosmosClientOptions][docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.cosmosclientoptions] named **options** class with the **AllowBulkExecution** property set to a value of **true**:

    ```
    CosmosClientOptions options = new () 
    { 
        AllowBulkExecution = true 
    };
    ```

1. Create a new instance of the **CosmosClient** class named **client** passing in the **endpoint**, **key**, and **options** variables as constructor parameters:

    ```
    CosmosClient client = new (endpoint, key, options); 
    ```

1. Use the [GetContainer][docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.cosmosclient.getcontainer] method of the **client** variable to retrieve the existing container using the name of the database (*cosmicworks*) and the name of the container (*products*):

    ```
    Container container = client.GetContainer("cosmicworks", "products");
    ```

1. Use this special sample code to generate **25,000** fictitious products using the **Faker** class from the Bogus library imported from NuGet.

    ```
    List<Product> productsToInsert = new Faker<Product>()
        .StrictMode(true)
        .RuleFor(o => o.id, f => Guid.NewGuid().ToString())
        .RuleFor(o => o.name, f => f.Commerce.ProductName())
        .RuleFor(o => o.price, f => Convert.ToDouble(f.Commerce.Price(max: 1000, min: 10, decimals: 2)))
        .RuleFor(o => o.categoryId, f => f.Commerce.Department(1))
        .Generate(25000);
    ```

    >**Note**: The [Bogus][nuget.org/packages/bogus/33.1.1] library is an open-source library used to design fictitious data to test user interface applications and is great for learning how to develop bulk import/export applications.

1. Create a new generic **List<>** of type **Task** named **concurrentTasks**:

    ```
    List<Task> concurrentTasks = new List<Task>();
    ```

1. Create a for-each loop that will iterate over the list of products that was generated earlier in this application:

    ```
    foreach(Product product in productsToInsert)
    {
    }
    ```

1. Within the for-each loop, create a **Task** to asynchronously insert a product into Azure Cosmos DB for NoSQL being sure to explicitly specify the partition key and to add the task to a list of tasks named **concurrentTasks**:

    ```
    concurrentTasks.Add(
        container.CreateItemAsync(product, new PartitionKey(product.categoryId))
    );   
    ```

1. After the foreach loop, asynchronously await the result of **Task.WhenAll** on the **concurrentTasks** variable:

    ```
    await Task.WhenAll(concurrentTasks);
    ```

1. Use the built-in **Console.WriteLine** static method to print a static message of **Bulk tasks complete** to the console:

    ```
    Console.WriteLine("Bulk tasks complete");
    ```

1. Once you are done, your code file should now include:
  
    ```
    using System;
    using System.Collections.Generic;
    using System.Threading.Tasks;
    using Bogus;
    using Microsoft.Azure.Cosmos;
    
    string endpoint = "<cosmos-endpoint>";
    string key = "<cosmos-key>";
    
    CosmosClientOptions options = new () 
    { 
        AllowBulkExecution = true 
    };
    
    CosmosClient client = new (endpoint, key, options);  
    
    Container container = client.GetContainer("cosmicworks", "products");
    
    List<Product> productsToInsert = new Faker<Product>()
        .StrictMode(true)
        .RuleFor(o => o.id, f => Guid.NewGuid().ToString())
        .RuleFor(o => o.name, f => f.Commerce.ProductName())
        .RuleFor(o => o.price, f => Convert.ToDouble(f.Commerce.Price(max: 1000, min: 10, decimals: 2)))
        .RuleFor(o => o.categoryId, f => f.Commerce.Department(1))
        .Generate(25000);
        
    List<Task> concurrentTasks = new List<Task>();
    
    foreach(Product product in productsToInsert)
    {    
        concurrentTasks.Add(
            container.CreateItemAsync(product, new PartitionKey(product.categoryId))
        );
    }
    
    await Task.WhenAll(concurrentTasks);   

    Console.WriteLine("Bulk tasks complete");
    ```

1. Press **Ctrl+S** to **Save** the script.cs code file.

1. Open the context menu for the **08-sdk-bulk (1)** folder and then select **Open in Integrated Terminal (2)** to open a new terminal instance.
 
    ![](media/DB46.png)

1. Build and run the project using the **[dotnet run][docs.microsoft.com/dotnet/core/tools/dotnet-run]** command:

    ```
    dotnet run
    ```

1. The application should run silently, it should take approximately one to two minutes to run before completing silently.

    ![](media/DB47.png)

1. Close the integrated terminal.

1. Close **Visual Studio Code**.

    > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
    > - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
    > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
    > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.

    <validation step="5bf085fc-efec-4bde-9f59-38074db269a2" />

### Task 3: Observe the results

Now that you have sent 25,000 items to Azure Cosmos DB let’s go and look at the Data Explorer.

1. Back in the  Azure portal (``portal.azure.com``) in the browser window, select the **Azure Cosmos DB account** resource you created in this lab.

1. Within the **Azure Cosmos DB** account resource **overview page (1)** , navigate to the **Data Explorer (2)** pane. 

    ![06](media/DB04.png)

1. In the **Data Explorer**, expand the **cosmicworks (2)** database node, expand the **products (3)** container node.

    ![06](media/DB41.png)

1. Select the **products** container node within the **API for NoSQL** navigation tree, and click on **... (1)** then select **New SQL Query (2)**.

   ![](media/DB48.png)

1. Delete the contents of the editor area.

1. Enter the query as shown below **(1)**, then click **Execute Query (2)**.

    ```
    SELECT COUNT(1) FROM items
    ```

     ![](media/DB49.png)

1. And view the **results (3)** observe the count of the items in your container.

### Review

In this lab, you have completed:

- Create an Azure Cosmos DB for NoSQL account and configure the SDK project
- Bulk inserted twenty-five thousand documents.
- Observed the results.

### Summary
In this lab, you have learnt to bulk insert 25,000 fictional documents into an Azure Cosmos DB for NoSQL account using the Azure Cosmos DB SDK and the Bogus library, while also configuring the necessary environment and observing the results

### You have successfully completed the lab
