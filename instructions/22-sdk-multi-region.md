# Lab 09c - Azure Cosmos DB for NoSQL のレプリケーション戦略を設計・実装する

## ラボのシナリオ

**CosmosClientBuilder** クラスは、SDK クライアントを構築してコンテナーに接続し、操作を実行するように設計されたフルエント クラスです。ビルダーを使用すると、Azure Cosmos DB for NoSQL アカウントが既にマルチリージョン書き込み用に構成されている場合、書き込み操作の優先アプリケーション リージョンを構成できます。

このラボでは、複数のリージョンで Azure Cosmos DB for NoSQL アカウントを構成し、マルチリージョン書き込みを有効にします。その後、SDK を使用して特定のリージョンに対する操作を実行します。

## ラボの目的

このラボでは、次のタスクを完了します:
- タスク 1: 開発環境を準備する。
- タスク 2: Azure Cosmos DB for NoSQL アカウントを作成する。
- タスク 3: SDK から Azure Cosmos DB for NoSQL アカウントに接続する。
- タスク 4: SDK の書き込みリージョンを構成する。

## 推定所要時間: 60 分

## アーキテクチャ図

![image](architecturedia/lab22.png)

## 演習 1: Azure Cosmos DB for NoSQL SDK でマルチリージョン書き込みアカウントに接続する

### タスク 1: 開発環境を準備する

1. Visual Studio Code を起動します（プログラムのアイコンはデスクトップにピン留めされています）。

2. 左側のペインから **Extension (1)** アイコンを選択します。検索バーに **C# (2)** と入力し、表示された **extension (3)** を選択して最後に **Install (4)** をクリックします。

    ![](media/C-hash-extension.png)

3. 画面左上の **file** オプションを選択し、ペインオプションから **Open Folder** を選択して **C:\AllFiles** に移動します。

4. フォルダー **dp-420-cosmos-db-dev** を選択し、**Select Folder** をクリックします。

### タスク 2: Azure Cosmos DB for NoSQL アカウントを作成する

Azure Cosmos DB は複数の API をサポートするクラウドベースの NoSQL データベース サービスです。Azure Cosmos DB アカウントを初めてプロビジョニングするときは、アカウントでサポートする API を選択します（たとえば **API for MongoDB** や **API for NoSQL**）。Azure Cosmos DB for NoSQL アカウントのプロビジョニングが完了したら、エンドポイントとキーを取得して、Azure SDK for .NET やその他の SDK を使用して接続できます。

1. 新しい Web ブラウザー ウィンドウまたはタブで Azure ポータル（``portal.azure.com``）に移動します。

1. サブスクリプションに関連付けられた Microsoft 資格情報でポータルにサインインします。

1. **Azure services** カテゴリで **Create a resource** を選択し、次に **Azure Cosmos DB** を選択します。

    > 💡 代わりに、**≡** メニューを展開し、**All Services** を選択し、**Databases** カテゴリで **Azure Cosmos DB** を選択して **Create** をクリックしてもかまいません。

1. **Select API option** ペインで、**Azure Cosmos DB for NoSQL** セクションの **Create** オプションを選択します。

1. **Create Azure Cosmos DB Account** ペインで、**Basics** タブを確認します。

    | **設定** | **値** |
    | --- | --- |
    | **Subscription** | *既存の Azure サブスクリプション* |
    | **Resource group** | *DP-420-DeploymentID* |
    | **Account Name** | *グローバルに一意の名前を入力* |
    | **Location** | *利用可能なリージョンを選択* |
    | **Capacity mode** | *Provisioned throughput* |
    | **Apply Free Tier Discount** | *Do Not Apply* |

    > **注意**: DeploymentID は各環境に関連付けられた一意の ID です。環境の詳細ページで値を確認できます。

1. **Review + Create** をクリックし、検証が成功したら **Create** をクリックします。

1. このタスクを続行する前に、デプロイメント タスクが完了するまで待ちます。

1. 新しく作成した **Azure Cosmos DB** アカウント リソースに移動し、**Replicate data globally** ペインに移動します。

1. **Replicate data globally** ペインで、アカウントに少なくとも 1 つの追加リージョンを追加します。

1. 引き続き **Replicate data globally** ペイン内で、**Multi-region writes** を有効にして、変更を **Save** します。

1. このタスクを続行する前に、レプリケーション タスクが完了するまで待ちます。

    > **注意:** この操作には約 5〜10 分かかる場合があります。

1. 作成した追加リージョンの名前を少なくとも 1 つ記録します。この値は後でこの演習で使用します。

1. リソース ブレードで、**Data Explorer** ペインに移動します。

1. **Data Explorer** ペインで **New Container** を選択します。

1. **New Container** のポップアップで、各設定に次の値を入力し、**OK** を選択します。

    | **設定** | **値** |
    | --- | --- |
    | **Database id** | *Create new* | *cosmicworks* |
    | **Share throughput across containers** | *Do not select* |
    | **Container id** | *products* |
    | **Partition key** | */categoryId* |
    | **Container throughput** | *Manual* | *400* |

1. **Data Explorer** ペインに戻り、**cosmicworks** データベース ノードを展開して、階層内の **products** コンテナー ノードを確認します。

1. リソース ブレードで、**Keys** ペインに移動します。

1. このペインには、SDK からアカウントに接続するために必要な接続情報と資格情報が含まれています。具体的には:

    1. **URI** フィールドの値を記録します。この **endpoint** 値は後でこの演習で使用します。

    1. **PRIMARY KEY** フィールドの値を記録します。この **key** 値は後でこの演習で使用します。

1. Web ブラウザー ウィンドウまたはタブを閉じます。

### タスク 3: SDK から Azure Cosmos DB for NoSQL アカウントに接続する

新しく作成したアカウントの資格情報を使用して、SDK クラスに接続し、新しいデータベースとコンテナー インスタンスを作成します。次に、Data Explorer を使用して、これらのインスタンスが Azure ポータルに存在することを確認します。

1. **Visual Studio Code** の **Explorer** ペインで、**22-sdk-multi-region** フォルダーに移動します。

1. **22-sdk-multi-region** フォルダーのコンテキスト メニューを開き、**Open in Integrated Terminal** を選択して新しいターミナル インスタンスを開きます。

    > **注意:** このコマンドは、開始ディレクトリが **22-sdk-multi-region** フォルダーに設定された状態でターミナルを開きます。

1. [dotnet build][docs.microsoft.com/dotnet/core/tools/dotnet-build] コマンドを使用してプロジェクトをビルドします:

    ```
    dotnet build
    ```

    > **注意:** **endpoint** および **key** 変数が現在未使用であるというコンパイラー警告が表示される場合があります。この警告は、このタスクでこれらの変数を使用するため、無視しても問題ありません。

1. 統合ターミナルを閉じます。

1. **product.cs** コード ファイルを開きます。

1. **Product** レコードとその対応するプロパティを確認します。このラボでは特に **id**、**name**、**categoryId** プロパティを使用します。

1. **Visual Studio Code** の **Explorer** ペインに戻り、**script.cs** コード ファイルを開きます。

    > **注意:** **[Microsoft.Azure.Cosmos][nuget.org/packages/microsoft.azure.cosmos/3.22.1]** ライブラリは、NuGet から既に事前にインポートされています。

1. **endpoint** という名前の **string** 変数を見つけます。これを、先ほど作成した Azure Cosmos DB アカウントの **endpoint** に設定します。
  
    ```
    string endpoint = "<cosmos-endpoint>";
    ```

    > **注意:** たとえば、エンドポイントが **https&shy;://dp420.documents.azure.com:443/** の場合、C# の文は **string endpoint = "https&shy;://dp420.documents.azure.com:443/";** となります。

1. **key** という名前の **string** 変数を見つけます。これを、先ほど作成した Azure Cosmos DB アカウントの **key** に設定します。

    ```
    string key = "<cosmos-key>";
    ```

    > **注意:** たとえば、キーが **fDR2ci9QgkdkvERTQ==** の場合、C# の文は **string key = "fDR2ci9QgkdkvERTQ==";** となります。

1. **script.cs** コード ファイルを **Save** します。

### タスク 4: SDK の書き込みリージョンを構成する

フルエント **WithApplicationRegion** メソッドは、ビルダー クラスを使用して後続の操作の優先リージョンを構成するために使用されます。

1. **endpoint** および **key** 変数をコンストラクターのパラメーターとして渡し、[CosmosClientBuilder][docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.fluent.cosmosclientbuilder] クラスの新しいインスタンスを **builder** という名前で作成します:

    ```
    CosmosClientBuilder builder = new (endpoint, key);
    ```

1. このラボで以前に作成した追加リージョンの名前を使用して、**string** 型の **region** という名前の新しい変数を作成します。たとえば、Azure Cosmos DB for NoSQL アカウントを **East US** リージョンに作成し、さらに **Brazil South** を追加した場合、文字列変数は次のようになります:

    ```
    string region = "Brazil South"; 
    ```

    > **注意:** 代わりに、さまざまな Azure リージョンに対する組み込みの文字列プロパティを含む [Microsoft.Azure.Cosmos.Regions][docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.regions] 静的クラスを使用できます。

1. **builder** 変数で [WithApplicationRegion][docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.fluent.cosmosclientbuilder.withapplicationregion] メソッドを **region** パラメーターで呼び出し、[Build][docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.fluent.cosmosclientbuilder.build] メソッドをフルーエントにチェーンして呼び出します。結果を **CosmosClient** 型の **client** という名前の変数に格納し、`using` 文で囲みます:

    ```
    using CosmosClient client = builder
        .WithApplicationRegion(region)
        .Build();
    ```

1. **client** 変数の **GetContainer** メソッドを使用して、データベース名 (*cosmicworks*) とコンテナー名 (*products*) を指定して既存のコンテナーを取得します:

    ```
    Container container = client.GetContainer("cosmicworks", "products");
    ```

1. 新しい **Guid** 値を生成し、その結果を文字列として格納することで、**id** と **categoryId** という 2 つの **string** 変数を作成します:

    ```
    string id = $"{Guid.NewGuid()}";
    string categoryId = $"{Guid.NewGuid()}";
    ```

1. **id** 変数、文字列値 **Polished Bike Frame**、および **categoryId** 変数をコンストラクター パラメーターとして渡して、**Product** 型の **item** という名前の新しい変数を作成します:

    ```
    Product item = new (id, "Polished Bike Frame", categoryId);
    ```

1. **container** 変数の **CreateItemAsync<>** メソッドを非同期に呼び出し、**item** 変数をパラメーターとして渡し、結果を **response** という名前の変数に格納します:

    ```
    var response = await container.CreateItemAsync<Product>(item);
    ```

1. 静的 **Console.WriteLine** メソッドを呼び出して、レスポンスの HTTP ステータス コードと要求課金（リクエスト ユニット単位）を出力します:

    ```
    Console.WriteLine($"Status Code:\t{response.StatusCode}");
    Console.WriteLine($"Charge (RU):\t{response.RequestCharge:0.00}");
    ```

1. 完了すると、コード ファイルには次の内容が含まれているはずです:

    ```
    using Microsoft.Azure.Cosmos;
    using Microsoft.Azure.Cosmos.Fluent;

    string endpoint = "<cosmos-endpoint>";
    string key = "<cosmos-key>";    

    CosmosClientBuilder builder = new (endpoint, key);            
    
    string region = "West Europe";
    
    using CosmosClient client = builder
        .WithApplicationRegion(region)
        .Build();
    
    Container container = client.GetContainer("cosmicworks", "products");
    
    string id = $"{Guid.NewGuid()}";
    string categoryId = $"{Guid.NewGuid()}";
    Product item = new (id, "Polished Bike Frame", categoryId);
    
    var response = await container.CreateItemAsync<Product>(item);
    
    Console.WriteLine($"Status Code:\t{response.StatusCode}");
    Console.WriteLine($"Charge (RU):\t{response.RequestCharge:0.00}");
    ```

1. **script.cs** コード ファイルを **Save** します。

1. **Visual Studio Code** で **22-sdk-multi-region** フォルダーのコンテキスト メニューを開き、**Open in Integrated Terminal** を選択して新しいターミナル インスタンスを開きます。

1. **[dotnet run][docs.microsoft.com/dotnet/core/tools/dotnet-run]** コマンドを使用してプロジェクトをビルドし、実行します:

    ```
    dotnet run
    ```

1. ターミナルの出力を確認します。HTTP ステータス コードと要求課金（RU）がコンソールに表示されるはずです。

1. 統合ターミナルを閉じます。

1. **Visual Studio Code** を閉じます。

### レビュー

このラボでは、次の作業を完了しました:

- 開発環境を準備しました。
- Azure Cosmos DB for NoSQL アカウントを作成しました。
- SDK から Azure Cosmos DB for NoSQL アカウントに接続しました。
- SDK の書き込みリージョンを構成しました。

### このラボは正常に完了しました
