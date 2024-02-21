# Batch multiple point operations together with the Azure Cosmos DB NoSQL API SDK

## Lab scenario

The [TransactionalBatch][docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.transactionalbatch] and [TransactionalBatchResponse][docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.transactionalbatchresponse] classes together are the key to composing and decomposing operations into a single logical step. Using these classes, you can write your code to perform multiple operations and then determine if they were completed successfully server-side.

In this lab, you’ll use the SDK, to perform two dual-item operations where you attempt to create two items as a single logical unit.

## Lab objectives

In this lab, you will complete the following tasks:
- Task 1: Create an Azure Cosmos DB for NoSQL account and configure the SDK project
- Task 2: Creating a transactional batch.
- Task 3: Creating an errant transactional batch.

## Estimated Timing: 30 minutes

## Prepare your development environment

1. Start Visual Studio Code (the program icon is pinned to the Desktop).

2. Select the **Extension (1)** icon from the left pane. Enter **C# (2)** in the search bar and select the **extension (3)** that shows up and finally **Install (4)** on the extension. 

    ![](media/C-hash-extension.png)

3. Select the **file** option on the top left of the screen, from the pane options, select **Open Folder** and navigate to **C:\AllFiles**.

4. Select the folder **dp-420-cosmos-db-dev** and click on **Select Folder**.

## Task 1: Create an Azure Cosmos DB for NoSQL account and configure the SDK project

1. In a new web browser window or tab, navigate to the Azure portal (``portal.azure.com``).

1. Sign into the portal using the Microsoft credentials associated with your subscription.

1. Select **+ Create a resource**, search for *Cosmos DB*, and then create a new **Azure Cosmos DB for NoSQL** account resource with the following settings, leaving all remaining settings to their default values:

    | **Setting** | **Value** |
    | ---: | :--- |
    | **Subscription** | *Your existing Azure subscription* |
    | **Resource group** | *Select an existing Cosmosdb-<inject key="DeploymentID" enableCopy="false"/>* |
    | **Account Name** | *sql-<inject key="DeploymentID" enableCopy="false"/>* |
    | **Location** | *Choose any available region* |
    | **Capacity mode** | *Provisioned throughput* |
    | **Apply Free Tier Discount** | *Do Not Apply* |

1. Wait for the deployment task to complete before continuing with this task.

1. Go to the newly created **Azure Cosmos DB** account resource and navigate to the **Keys** pane.

1. This pane contains the connection details and credentials necessary to connect to the account from the SDK. Specifically:

    1. Notice the **URI** field. You will use this **endpoint** value later in this exercise.

    1. Notice the **PRIMARY KEY** field. You will use this **key** value later in this exercise.

1. Return to **Visual Studio Code**.

1. In **Visual Studio Code**, in the **Explorer** pane, browse to the **07-sdk-batch** folder.

1. Open the **script.cs** code file within the **07-sdk-batch** folder.

    >**Note**: The **[Microsoft.Azure.Cosmos][nuget.org/packages/microsoft.azure.cosmos/3.22.1]** library has already been pre-imported from NuGet.

1. Locate the **string** variable named **endpoint**. Set its value to the **endpoint** of the Azure Cosmos DB account you created in previous lab.
  
    ```
    string endpoint = "<cosmos-endpoint>";
    ```

    >**Note**: For example, if your endpoint is: **https&shy;://dp420.documents.azure.com:443/**, then the C# statement would be: **string endpoint = "https&shy;://dp420.documents.azure.com:443/";**.

1. Locate the **string** variable named **key**. Set its value to the **key** of the Azure Cosmos DB account you created in previous lab..

    ```
    string key = "<cosmos-key>";
    ```

    >**Note**: For example, if your key is: **fDR2ci9QgkdkvERTQ==**, then the C# statement would be: **string key = "fDR2ci9QgkdkvERTQ==";**.

1. **Save** the **script.cs** code file.

1. Open the context menu for the **07-sdk-batch** folder and then select **Open in Integrated Terminal** to open a new terminal instance.

    >**Note**: This command will open the terminal with the starting directory already set to the **07-sdk-batch** folder.

1. Add the [Microsoft.Azure.Cosmos][nuget.org/packages/microsoft.azure.cosmos/3.22.1] package from NuGet using the following command:

    ```
    dotnet add package Microsoft.Azure.Cosmos --version 3.22.1
    ```

1. Build the project using the [dotnet build][docs.microsoft.com/dotnet/core/tools/dotnet-build] command:

    ```
    dotnet build
    ```

1. Close the integrated terminal.

### Task 2: Creating a transactional batch

First, let’s create a simple transactional batch that makes two fictional products. This batch will insert a worn saddle and a rusty handlebar into the container with the same “used accessories” category identifier. Both items have the same logical partition key, ensuring that we will have a successful batch operation.

1. Return to the editor tab for the **script.cs** code file.

1. Create a **Product** variable named **saddle** with a unique identifier of **0120**, a name of **Worn Saddle**, and a category identifier of **9603ca6c-9e28-4a02-9194-51cdb7fea816**:

    ```
    Product saddle = new("0120", "Worn Saddle", "9603ca6c-9e28-4a02-9194-51cdb7fea816");
    ```

1. Create a **Product** variable named **handlebar** with a unique identifier of **012A**, a name of **Rusty Handlebar**, and a category identifier of **9603ca6c-9e28-4a02-9194-51cdb7fea816**:

    ```
    Product handlebar = new("012A", "Rusty Handlebar", "9603ca6c-9e28-4a02-9194-51cdb7fea816");
    ```

1. Create a variable of type **PartitionKey** named **partitionKey** passing in **9603ca6c-9e28-4a02-9194-51cdb7fea816** as a constructor parameter:

    ```
    PartitionKey partitionKey = new ("9603ca6c-9e28-4a02-9194-51cdb7fea816");
    ```

1. Invoke the [CreateTransactionalBatch][docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.container.createtransactionalbatch] method of the **container** variable passing in the **partitionkey** variable as a method parameter and using the fluent syntax to invoke the [CreateItem<>][docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.transactionalbatch.createitem] generic methods passing in the **saddle** and **handlebar** variables as items to create in individual operations and store the result in a variable named **batch** of type **TransactionalBatch**:

    ```
    TransactionalBatch batch = container.CreateTransactionalBatch(partitionKey)
        .CreateItem<Product>(saddle)
        .CreateItem<Product>(handlebar);
    ```

1. Within a using statement, asynchronously invoke the **ExecuteAsync** method of the **batch** variable and store the result in a variable of type **TransactionalBatchResponse** named **response**:

    ```
    using TransactionalBatchResponse response = await batch.ExecuteAsync();
    ```

1. Invoke the static **Console.WriteLine** method to output the value of the **StatusCode** property of the **response** variable:

    ```
    Console.WriteLine($"Status:\t{response.StatusCode}");
    ```

1. Once you are done, your code file should now include:
  
    ```
    using System;
    using Microsoft.Azure.Cosmos;
    
    string endpoint = "<cosmos-endpoint>";
    string key = "<cosmos-key>";
    
    CosmosClient client = new CosmosClient(endpoint, key);
        
    Database database = await client.CreateDatabaseIfNotExistsAsync("cosmicworks");
    Container container = await database.CreateContainerIfNotExistsAsync("products", "/categoryId", 400);

    Product saddle = new("0120", "Worn Saddle", "9603ca6c-9e28-4a02-9194-51cdb7fea816");
    Product handlebar = new("012A", "Rusty Handlebar", "9603ca6c-9e28-4a02-9194-51cdb7fea816");
    
    PartitionKey partitionKey = new ("9603ca6c-9e28-4a02-9194-51cdb7fea816");
    
    TransactionalBatch batch = container.CreateTransactionalBatch(partitionKey)
        .CreateItem<Product>(saddle)
        .CreateItem<Product>(handlebar);
    
    using TransactionalBatchResponse response = await batch.ExecuteAsync();
    
    Console.WriteLine($"Status:\t{response.StatusCode}");
    ```

1. **Save** the **script.cs** code file.

1. In **Visual Studio Code**, open the context menu for the **07-sdk-batch** folder and then select **Open in Integrated Terminal** to open a new terminal instance.

1. Build and run the project using the **[dotnet run][docs.microsoft.com/dotnet/core/tools/dotnet-run]** command:

    ```
    dotnet run
    ```

1. Observe the output from the terminal. The status code should be an HTTP 200 **OK**.

1. Close the integrated terminal.

### Task 3: Creating an errant transactional batch

Now, let’s create a transactional batch that will error purposefully. This batch will attempt to insert two items that have different logical partition keys. We will create a flickering strobe light in the “used accessories” category and a new helmet in the “pristine accessories” category. By definition, this should be a bad request and return an error when performing this transaction.

1. Return to the editor tab for the **script.cs** code file.

1. Delete the following lines of code:

    ```
    Product saddle = new("0120", "Worn Saddle", "9603ca6c-9e28-4a02-9194-51cdb7fea816");
    Product handlebar = new("012A", "Rusty Handlebar", "9603ca6c-9e28-4a02-9194-51cdb7fea816");
    
    PartitionKey partitionKey = new ("9603ca6c-9e28-4a02-9194-51cdb7fea816");
    
    TransactionalBatch batch = container.CreateTransactionalBatch(partitionKey)
        .CreateItem<Product>(saddle)
        .CreateItem<Product>(handlebar);
    
    using TransactionalBatchResponse response = await batch.ExecuteAsync();
    
    Console.WriteLine($"Status:\t{response.StatusCode}");
    ```

1. Create a **Product** variable named **light** with a unique identifier of **012B**, a name of **Flickering Strobe Light**, and a category identifier of **9603ca6c-9e28-4a02-9194-51cdb7fea816**:

    ```
    Product light = new("012B", "Flickering Strobe Light", "9603ca6c-9e28-4a02-9194-51cdb7fea816");
    ```

1. Create a **Product** variable named **helmet** with a unique identifier of **012C**, a name of **New Helmet**, and a category identifier of **0feee2e4-687a-4d69-b64e-be36afc33e74**:

    ```
    Product helmet = new("012C", "New Helmet", "0feee2e4-687a-4d69-b64e-be36afc33e74");
    ```

1. Create a variable of type **PartitionKey** named **partitionKey** passing in **9603ca6c-9e28-4a02-9194-51cdb7fea816** as a constructor parameter:

    ```
    PartitionKey partitionKey = new ("9603ca6c-9e28-4a02-9194-51cdb7fea816");
    ```

1. Invoke the **CreateTransactionalBatch** method of the **container** variable passing in the **partitionkey** variable as a method parameter and using the fluent syntax to invoke the **CreateItem<>** generic methods passing in the **light** and **helmet** variables as items to create in individual operations and store the result in a variable named **batch** of type **TransactionalBatch**:

    ```
    TransactionalBatch batch = container.CreateTransactionalBatch(partitionKey)
        .CreateItem<Product>(light)
        .CreateItem<Product>(helmet);
    ```

1. Within a using statement, asynchronously invoke the **ExecuteAsync** method of the **batch** variable and store the result in a variable of type **TransactionalBatchResponse** named **response**:

    ```
    using TransactionalBatchResponse response = await batch.ExecuteAsync();
    ```

1. Invoke the static **Console.WriteLine** method to output the value of the **StatusCode** property of the **response** variable:

    ```
    Console.WriteLine($"Status:\t{response.StatusCode}");
    ```

1. Once you are done, your code file should now include:
  
    ```
    using System;
    using Microsoft.Azure.Cosmos;
    
    string endpoint = "<cosmos-endpoint>";
    string key = "<cosmos-key>";
    
    CosmosClient client = new CosmosClient(endpoint, key);
        
    Database database = await client.CreateDatabaseIfNotExistsAsync("cosmicworks");
    Container container = await database.CreateContainerIfNotExistsAsync("products", "/categoryId", 400);

    Product light = new("012B", "Flickering Strobe Light", "9603ca6c-9e28-4a02-9194-51cdb7fea816");
    Product helmet = new("012C", "New Helmet", "0feee2e4-687a-4d69-b64e-be36afc33e74");
    
    PartitionKey partitionKey = new ("9603ca6c-9e28-4a02-9194-51cdb7fea816");
    
    TransactionalBatch batch = container.CreateTransactionalBatch(partitionKey)
        .CreateItem<Product>(light)
        .CreateItem<Product>(helmet);
    
    using TransactionalBatchResponse response = await batch.ExecuteAsync();
    
    Console.WriteLine($"Status:\t{response.StatusCode}");
    ```

1. **Save** the **script.cs** code file.

1. In **Visual Studio Code**, open the context menu for the **07-sdk-batch** folder and then select **Open in Integrated Terminal** to open a new terminal instance.

1. Build and run the project using the **[dotnet run][docs.microsoft.com/dotnet/core/tools/dotnet-run]** command:

    ```
    dotnet run
    ```

1. Observe the output from the terminal. The status code should either be an HTTP 400 **Bad Request** or 409 **Conflict**. This occured because all items within the transaction did not share the same partition key value as the transactional batch.

1. Close the integrated terminal.

1. Close **Visual Studio Code**.

### Review

In this lab, you have completed:

- Create an Azure Cosmos DB for NoSQL account and configure the SDK project.
- Created a transactional batch.
- Created an errant transactional batch.

### You have successfully completed the lab
