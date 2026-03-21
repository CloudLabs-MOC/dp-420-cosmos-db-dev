# Lab 01: Explore Azure Cosmos DB

## Lab scenario
In this lab, you'll provision an Azure Cosmos DB database in your Azure subscription, and explore the various ways you can use it to store non-relational data.

## Lab objectives

In this lab, you will perform the following tasks:

+ Task 1: Create a Cosmos DB account
+ Task 2: Create a sample database
+ Task 3: View and create items
+ Task 4: Query the database

### Exercise 1: Explore Azure Cosmos DB

To use Cosmos DB, you must provision a Cosmos DB account in your Azure subscription. In this exercise, you'll provision a Cosmos DB account that uses the core (SQL) API.

#### Task 1: Create a Cosmos DB account
In this task, you will provision an Azure Cosmos DB SQL account, configuring essential settings and retrieving the necessary connection details for future development.

1. On **Azure Portal** page, in Search resources, services and docs (G+/) box at the top of the portal, **enter Azure Cosmos DB (1)**, and then select **Azure Cosmos DB (2)** under services.

   ![06](media/New-image1.png)
   
1. Select **+ Create (1)** under **Azure Cosmos DB for NoSQL** click on **Create (2)** to create **Azure Cosmos DB for NoSQL** account.

    ![06](media/New-image2.png)

    ![06](media/New-image3.png)

1. Enter the following details:

   - **Workload type (1)**: Select **Learning**  
   - **Subscription (2)**: Select your Azure subscription  
   - **Resource group (3)**: Select **DP-900-Module-3-<inject key="DeploymentID" enableCopy="false"/>**  
   - **Account name (4)**: Enter **cosmosdb-<inject key="DeploymentID" enableCopy="false"/>**  
   - **Location (5)**: Select any available location  
   - **Capacity mode (6)**: Select **Provisioned throughput**  
   - **Apply Free Tier Discount (7)**: Select **Apply**  
   - **Limit total account throughput (8)**: Leave unchecked  

1. Click **Review + create (9)**.

    ![](media/DB1.png)

1.  When the configuration has been validated, select  **Create**.

    ![](media/DB2.png)

1.  Wait for deployment to complete. Then go to the deployed resource.

    ![](media/DB3.png)

### Task 2 : Create a sample database

This task involves reviewing pre-configured settings and observing the creation process of the SampleDB database and its container.
*Throughout this procedure, close any tips that are displayed in the portal*.

1. In the **Azure Cosmos DB account**, select **Overview (1)** from the left menu, then click **Data Explorer (2)**.

     ![](media/DB4.png)

    >**Note** If you see a pop up saying Welcome What is Cosmos DB click on **X** to close it

     ![](media/DB5.png)

1. In the **Data Explorer (1)** page, select **Launch quick start (2)**.

     ![](media/DB6.png)

1. In the **New container** tab, review the pre-populated settings for the sample database, and then select **OK**.

    ![](media/DB7.png)

1. Observe the status in the panel at the bottom of the screen until the **SampleDB** database and its **SampleContainer** container has been created (which may take a minute or so).

    ![](media/DB8.png)

### Task 3 : View and create items

This task demonstrates how to view existing items, create new items, and work with JSON data in a Cosmos DB container using the Data Explorer interface, simulating how developers interact with and manage data in real world applications.

1. In the **Data Explorer** page, expand **SampleDB (1)**, then expand **SampleContainer (2)**, and select **Items (3)** to view the list of items.

1. Select any item from the list (4) to view its JSON representation, then unselect it and proceed to create a new item.

1. At the top of the page, click **New Item (5)** to create a new blank item.

    ![](media/DB9.png)

1. Modify the JSON for the new item as shown below (1), then click **Save (2)**.

    
    ```json
    {
        "name": "Road Helmet,45",
        "id": "123456789",
        "categoryID": "123456789",
        "SKU": "AB-1234-56",
        "description": "The product called \"Road Helmet,45\" ",
        "price": 48.74
    }
    ```

    ![](media/DB10.png)
    
1.  After saving the new item, notice that additional metadata properties are added automatically.

### Task 4  : Query the database

This task demonstrates how to create, view, and query items in a Cosmos DB container using the Data Explorer interface, simulating how developers would interact with the database using SDKs in real-world applications.

1. In the **Data Explorer (1)** page, select the **New SQL Query (2)** icon.

    ![](media/DB11.png)

2. The query **SELECT * FROM c (1)** is present by default, click **Execute Query (2)**.

     ![](media/DB12.png)

1. Review the results, which includes the full JSON representation of all items.

     ![](media/DB13.png)

1. Modify the query as follows **(1)**:
   
    ```sql
    SELECT *
    FROM c
    WHERE CONTAINS(c.name,"Helmet")
    ```

1. Click **Execute Query (2)** button to run the revised query and review the results, which includes JSON entities for any items with a **name** field containing the text **Helmet (3)** .

    ![](media/DB14.png)
    
1. Close the SQL Query editor, discarding your changes.
    
    >**Note**: You've seen how to create and query JSON entities in a Cosmos DB database by using the data explorer interface in the Azure portal. In a real scenario, an application developer would use one of the many programming language specific software development kits (SDKs) to call the core (SQL) API and work with data in the database.
    
    > **Congratulations** on completing the lab! Now, it's time to validate it. Here are the steps:
    > - Hit the Validate button for the corresponding task. If you receive a success message, you have successfully validated the lab. 
    > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
    > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out. 

    <validation step="0c506d8c-06e7-4eb7-aa88-fed3d24ffcfc" />

## Summary 

In this lab you gained hands-on experience with creating, viewing, modifying, and querying data in Azure Cosmos DB, which is valuable for storing non-relational data in real-world applications.

## Review
In this lab, you have completed:
- Create a Cosmos DB account
- Create a sample database
- View and create items
- Query the database
  
## You have successfully completed this lab

