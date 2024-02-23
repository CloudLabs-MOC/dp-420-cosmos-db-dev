# Search data using Azure AI Search and Azure Cosmos DB for NoSQL

## Lab scenario

Azure Cognitive Search combines a search engine as a service with deep integration with AI capabilities to enrich the information in the search index.

In this lab, you will build an Azure Cognitive Search index that automatically indexes data in an Azure Cosmos DB SQL API container and enriches the data using the Azure Cognitive Services Translator functionality.

## Lab objectives

In this lab, you will complete the following tasks:
- Task 1: Create an Azure Cosmos DB for NoSQL account.
- Task 2: Send your Azure Cosmos DB for NoSQL account with sample data.
- Task 3: Create an Azure AI Search resource.
- Task 4: Build indexer and index for Azure Cosmos DB for NoSQL data.
- Task 5: Validate index with example search queries.

## Estimated Timing: 30 minutes

### Task 1: Create an Azure Cosmos DB for NoSQL account

Azure Cosmos DB is a cloud-based NoSQL database service that supports multiple APIs. When provisioning an Azure Cosmos DB account for the first time, you will select which of the APIs you want the account to support (for example, **Mongo API** or **NoSQL API**). Once the Azure Cosmos DB for NoSQL account is done provisioning, you can retrieve the endpoint and key and use them to connect to the Azure Cosmos DB for NoSQL account using the Azure SDK for .NET or any other SDK of your choice.

1. Inside the LabVM, double click on the **Azure Portal** shortcut.

    ![](media/azureportal.png)

1. On the **Sign-in into Microsoft Azure** tab you will see the login screen, in that enter the following email/username and then click on **Next**. 
   * Email/Username: <inject key="AzureAdUserEmail"></inject>
   
     ![04](media/04.png)
     
1. Now enter the following password and click on **Sign in**.
   * Password: <inject key="AzureAdUserPassword"></inject>
   
     ![05](media/05.png)
     
        >**Note:** If you see the **Help us protect your account** dialog box, then select the **Skip for now** option.

        ![06](media/06.png)
  
1. If you see the pop-up **Stay Signed in?**, click No

1. If you see the pop-up **You have free Azure Advisor recommendations!**, close the window to continue the lab.

1. If a **Welcome to Microsoft Azure** popup window appears, click **Maybe Later** to skip the tour.

1. Select **+ Create a resource**, search for *Cosmos DB*, select **Azure Cosmos DB**.

1. Select **create** under **Azure Cosmos DB for NoSQL**. Then create a new **Azure Cosmos DB for NoSQL** account resource with the following settings, leaving all remaining settings to their default values, and select **Review + create**:

    | **Setting** | **Value** |
    | ---: | :--- |
    | **Subscription** | *Your existing Azure subscription* |
    | **Resource group** | **Cosmosdb-<inject key="DeploymentID" enableCopy="false"/>** |
    | **Account Name** | *Enter a globally unique name* |
    | **Location** | *Choose any available region* |
    | **Capacity mode** | *Serverless* |

1. On the **Review + Create** page, select **Create**.

1. Wait for the deployment task to complete before continuing with this task.

1. Select **Go to resources**. On the newly created **Azure Cosmos DB** account under **Settings** navigate to the **Keys** pane.

1. This pane contains the connection details and credentials necessary to connect to the account from the SDK. Specifically:

    1. Note the **URI** field, in the notepad. You will use this **endpoint** value later in this exercise.

    1. Note the **PRIMARY KEY** field, in the notepad. You will use this **key** value later in this exercise.

    1. Note the **PRIMARY CONNECTION STRING** field, in the notepad. You will use this **connection string** value later in this exercise.

1. From the left-navigation menu, select **Data Explorer** from the resource menu.

1. In the **Data Explorer** pane, select **New Container**.

1. In the **New Container** popup, enter the following values for each setting, and then select **OK**:

    | **Setting** | **Value** |
    | --: | :-- |
    | **Database id** | *Create new* &vert; *``cosmicworks``* |
    | **Container id** | *``products``* |
    | **Partition key** | *``/categoryId``* |

1. Back in the **Data Explorer** pane, expand the **cosmicworks** database node and then observe the **products** container node within the hierarchy.

    ![06](media/cosmosdbproducts.png)

    > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Navigate to the Lab Validation Page, from the upper right corner in the lab guide section.
   > - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at labs-support@spektrasystems.com. We are available 24/7 to help you out.

### Task 2: Send your Azure Cosmos DB for NoSQL account with sample data

You will use a command-line utility that creates a **cosmicworks** database and a **products** container. The tool will then create a set of items that you will observe using the change feed processor running in your terminal window.

1. On the LabVM select the **Visual Studio Code** shortcut.

    ![06](media/visualstudio.png)

1. In **Visual Studio Code**, open the **Terminal** menu by selecting **... (ellipses) (1)** > **Terminal (2)** > **New Terminal (3)** to open a new terminal with your existing instance.

    ![06](media/terminal.png)

1. Install the [cosmicworks][nuget.org/packages/cosmicworks] command-line tool for global use on your machine.

    ```
    dotnet tool install cosmicworks --global --version 1.*
    ```

    >**Note:** This command may take a couple of minutes to complete. This command will output the warning message (*Tool 'cosmicworks' is already installed') if you have already installed the latest version of this tool in the past.

1. Run cosmicworks to seed your Azure Cosmos DB account with the following command-line options:

    | **Option** | **Value** |
    | --- | --- |
    | **--endpoint** | *The endpoint value you copied earlier in this lab* |
    | **--key** | *The key value you coped earlier in this lab* |
    | **--datasets** | *product* |

    ```
    cosmicworks --endpoint <cosmos-endpoint> --key <cosmos-key> --datasets product
    ```

    > **For example:** if your endpoint is: **https&shy;://dp420.documents.azure.com:443/** and your key is: **fDR2ci9QgkdkvERTQ==**, then the command would be:
    > ``cosmicworks --endpoint https://dp420.documents.azure.com:443/ --key fDR2ci9QgkdkvERTQ== --datasets product``

    >**Note**: If your getting error, close the visual studio code and reopen it and try to run the command once again.

1. Wait for the **cosmicworks** command to finish populating the account with a database, container, and items.

1. Close the integrated terminal.

1. Close **Visual Studio Code**.

### Task 3: Create an Azure AI Search resource

Before continuing with this exercise, you must first create a new Azure Cognitive Search instance.

1. In a new web browser window or tab, navigate to the Azure portal. Go back to the **Home** page.

1. Select **+ Create a resource**, search for *AI Search*, and then create a new **Azure AI Search** account resource with the following settings, leaving all remaining settings to their default values:

    | **Setting** | **Value** |
    | ---: | :--- |
    | **Subscription** | *Your existing Azure subscription* |
    | **Resource group** | **Cosmosdb-<inject key="DeploymentID" enableCopy="false"/>** |
    | **Name** | *Enter a globally unique name* |
    | **Location** | *Choose any available region* |

    
    >**Note:** If it shows any subscriptions errors, select **Next: Scale**, and select **Previous**.

1. Click on **Review + Create** and after validation get Success click on **Create**.

1. Wait for the deployment task to complete before continuing with this task.

1. Go to the newly created **Azure AI Search** account resource.

    > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Navigate to the Lab Validation Page, from the upper right corner in the lab guide section.
   > - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at labs-support@spektrasystems.com. We are available 24/7 to help you out.

### Task 4: Build indexer and index for Azure Cosmos DB for NoSQL data

You will create an indexer that indexes a subset of data in a specific Azure Cosmos DB for NoSQL container on an hourly basis.

1. From the **AI Search** resource blade, select **Import data**.

    ![06](media/importdata.png)

1. In the **Connect to your data** step of the **Import data** wizard, in the **Data Source** list, select **Azure Cosmos DB (1)**.

1. Configure the data source with the following settings, leaving all remaining settings to their default values:

    | **Setting** | **Value** |
    | ---: | :--- |
    | **Data source name** | **products-cosmossql-source (2)** |
    | **Connection string** | **Choose an existing connection of the Azure Cosmos DB for NoSQL account created earlier (3)** |
    | **Database** | **cosmicworks (4)** |
    | **Collection** | **products (5)** |

    ![06](media/importyourdata.png)

1. In the **query** field, enter the following SQL query to create a materialized view of a subset of your data in the container:

    ```sql
    SELECT 
        p.id, 
        p.categoryId, 
        p.name, 
        p.price,
        p._ts
    FROM 
        products p 
    WHERE 
        p._ts > @HighWaterMark 
    ORDER BY 
        p._ts
    ```

1. Select the **Query results ordered by _ts** checkbox.

    >**Note:** This checkbox lets Azure AI Search know that the query sorts results by the **_ts** field. This type of sorting enables incremental progress tracking. If the indexer fails, it can pick right back up from the same **_ts** value since the results are ordered by the timestamp.

1. Select **Next: Add cognitive skills**.

1. Select **Skip to: Customize target index**.

1. In the **Customize target index** step of the wizard, configure the index with the following settings, leaving all remaining settings to their default values:

    | **Setting** | **Value** |
    | ---: | :--- |
    | **Index name** | *``products-index``* |
    | **Key** | *id* |

1. In the field table, configure the **Retrievable**, **Filterable**, **Sortable**, **Facetable**, and **Searchable** options for each field using the following table:

    ![06](media/categoryid.png)

1. Select **Next: Create an indexer**.

1. In the **Create an indexer** step of the wizard, configure the indexer with the following settings, leaving all remaining settings to their default values:

    | **Setting** | **Value** |
    | ---: | :--- |
    | **Name** | *``products-cosmosdb-indexer``* |
    | **Schedule** | *Hourly* |

1. Select **Submit** to create the data source, index, and indexer.

    >**Note:** You may be required to dismiss a survey popup after creating your first indexer.

1. From the **AI Search** resource blade, from the left navigation menu, select **Indexers (1)** tab to observe the result of your first indexing operation.

1. Wait for the **products-cosmosdb-indexer** indexer to have a status of **Success (2)** before continuing with this task.

    >**Note:** You may need to use the **Refresh** option to update the blade if it does not update automatically.

    ![06](media/indexers.png)

1. Navigate to the **Indexes** tab and then select the **products-index** index.

    > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Navigate to the Lab Validation Page, from the upper right corner in the lab guide section.
   > - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at labs-support@spektrasystems.com. We are available 24/7 to help you out.

### Task 5: Validate index with example search queries

Now that your materialized view of the Azure Cosmos DB for NoSQL data is in the search index, you can perform a few basic queries that take advantage of the features in Azure AI Search.

> **Note:** This lab is not intended to teach the Azure AI Search syntax. These queries were curated to showcase some of the features available in the search index and engine.

1. In the **Search explorer** tab, select the **View** pulldown and then select the **JSON view**.

1. Notice the in the **JSON query editor** the syntax of the default JSON search query that returns all possible results using a **\*** (wildcard) operator.

   ```json
   {
       "search": "*"
   }
   ```

1. Select the **Search** button to perform the search.

1. Observe that this search query returns all possible results.

1. In the **JSON query editor**, enter the following query and then select **Search**:

    ```json
    {
        "search": "touring 3000"
    }
    ```

1. Observe that this search query returns results that contain either the terms **touring** or **3000** giving a higher score to results that contain both terms. The results are then sorted in descending order by the **@search.score** field.

1. In the **JSON query editor**, enter the following query and then select **Search**:

    ```json
    {
        "search": "red"
        , "count": true
    }
    ```

1. Observe that this search query returns results with the term **red**, but also now includes a metadata field indicating the total count of results even if they are not all included in the same page.

1. In the **JSON query editor**, enter the following query and then select **Search**:

    ```json
    {
        "search": "blue"
        , "count": true
        , "top": 6
    }
    ```

1. Observe that this search query only returns a set of six results at a time even though there are more matches server-side.

1. In the **JSON query editor**, enter the following query and then select **Search**:

    ```json
    {
        "search": "mountain"
        , "count": true
        , "top": 25
        , "skip": 50
    }
    ```

1. Observe that this search query skips the first 50 results and returns a set of 25 results. If this was a paginated view in a client-side application, you could infer that this would be the third "page" of results.

1. In the **JSON query editor**, enter the following query and then select **Search**:

    ```json
    {
        "search": "touring"
        , "count": true
        , "filter": "price lt 500"
    }
    ```

1. Observe that this search query only returns results where the value of the numeric price field is less than 500.

1. In the **JSON query editor**, enter the following query and then select **Search**:

    ```json
    {
        "search": "road"
        , "count": true
        , "top": 15
        , "facets": ["price,interval:500"]
    }
    ```

1. Observe that this search query returns a collection of facet data that indicates how many items belong to each category even if they are not all present in the current page of results. In this example, the matching items are broken down into numeric price categories in intervals of 500. This is typically used to populate filters and navigation aids in client-side applications.

    ![06](media/products-index.png)

1. Close your web browser window or tab.

### Review

In this lab, you have completed:

- Created an Azure Cosmos DB for NoSQL account.
- Sended your Azure Cosmos DB for NoSQL account with sample data.
- Created an Azure AI Search resource.
- Built indexer and index for Azure Cosmos DB for NoSQL data.
- Validated index with example search queries.

### You have successfully completed the lab
