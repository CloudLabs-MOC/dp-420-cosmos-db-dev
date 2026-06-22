
# Azure Data Factory を使って既存データを移行する

## ラボ シナリオ

Azure Data Factory では、Azure Cosmos DB をデータ取り込みのソースおよび出力先（シンク）として利用できます。
このラボでは、コマンドライン ユーティリティを使って Azure Cosmos DB にデータを投入し、その後 Azure Data Factory を使用して、あるコンテナーから別のコンテナーへデータのサブセットを移動します。

## ラボの目的

このラボで完了するタスク:
- タスク 1: Azure Cosmos DB SQL API アカウントを作成してシードする。
- タスク 2: Azure Data Factory リソースを作成する。

### タスク 1: Azure Cosmos DB for NoSQL アカウントの作成とシード

このタスクでは、Azure Cosmos DB for NoSQL アカウントを作成および構成し、続いてコマンドライン ユーティリティを使用してデータベースとコンテナーをシードします。

使用するコマンドライン ツールは、**cosmicworks** データベースおよび **products** コンテナーを **4,000** RU/s（リクエスト単位/秒）で作成します。作成後にスループットを 400 RU/s に下げます。

products コンテナーに加えて、ETL 変換とロード操作のターゲットとなる **flatproducts** コンテナーを手動で作成します。

1. Azure ポータルの上部にある「Search resources, services and docs (G+/)」ボックスに **Azure Cosmos DB (1)** と入力し、サービス一覧から **Azure Cosmos DB (2)** を選択します。

   ![06](media/New-image1.png)
   
1. **Azure Cosmos DB for NoSQL** の下で **+ Create (1)** を選択し、**Create (2)** をクリックして **Azure Cosmos DB for NoSQL** アカウントを作成します。

    ![06](media/New-image2.png)

    ![06](media/New-image3.png)

1. 以下の設定を指定し、残りはデフォルトのままにして **Next: Global Distribution (9)** を選択します:

    | **Setting**         | **Value** |
    | --------------------|--------------------------------------------------- |
    | **Workload Type**   | *Production* (1) |
    | **Subscription**    | *Your existing Azure subscription* (2) |
    | **Resource group**  | *既存の Cosmosdb-<inject key="DeploymentID" enableCopy="false"/> を選択* (3) |
    | **Account Name**    | *sql-<inject key="DeploymentID" enableCopy="false"/>* (4) |
    | **Location**        | *デフォルトのリージョンを選択* (5) |
    | **Capacity mode**   | *Provisioned throughput* (6) |
    | **Apply Free Tier Discount** | *Do Not Apply* (7) |
    | **Limit the total amount of throughput that can be provisioned on this account** | *Unchecked* (8) |

     ![06](media/create-cosmosdb1.1-2304.png)

1. Global Distribution ページで **Next: Networking** をクリックします。**Connectivity method** で **All networks (1)** を選択し、**Review + Create (2)** をクリックします。

     ![06](media/select-networking-2304.png)

1. **Create** をクリックします。

    ![06](media/New-image5.png)

1. デプロイが完了するまで待ちます。

1. デプロイが完了したら **Go to resources** を選択します。

    ![06](media/New-image6.png)

1. **Azure Cosmos DB account** の左メニューで **Settings (1)** を展開し、**Keys (2)** を選択します。

    ![06](media/DB15.png)

1. このペインには、SDK からアカウントに接続するための接続情報と資格情報が含まれています。具体的には:

1. **Keys (1)** ページで **Show (2)** アイコンをクリックして接続文字列を表示し、**Copy (3)** アイコンをクリックしてコピーし、メモ帳などに保存します。今後の手順で使用します。

    ![06](media/conn-string-2304.png)

1. 後で戻るので、ブラウザーのタブは開いたままにしておいてください。

1. LabVM で **Visual Studio Code** のショートカットを選択します。

    ![06](media/visualstudio.png)

1. **Visual Studio Code** で、**... (ellipsis) (1)** → **Terminal (2)** → **New Terminal (3)** の順に選択して新しいターミナルを開きます。

    ![06](media/New-image36.png)

1. ターミナルで次のコマンドを実行し、[cosmicworks][nuget.org/packages/cosmicworks] コマンドライン ツールをグローバルにインストールします。

    ```
    dotnet tool install cosmicworks --global --version 2.*
    ```

    > **注:** このコマンドの実行には数分かかることがあります。既に最新バージョンがインストールされている場合は、警告メッセージ（*Tool 'cosmicworks' is already installed*）が出力されます。

1. インストールが完了したら、**Visual Studio Code** を閉じて再度開いてください。

1. 次のコマンドを実行して、Azure Cosmos DB アカウントにデータをシードします。オプションは次のとおりです:

    | **Option**       | **Value** |
    | ---------------- | ----------|
    | **CONNECTION STRING**   | *このラボで先ほどコピーした Primary Connection String の値* |

    ```
    cosmicworks --connection-string "<CONNECTION_STRING>" --disable-hierarchical-partition-keys 
    ```

    > **注:** 上記コマンドでエラーが発生した場合は、**Visual Studio Code を閉じて再度開いて**から再実行してください。

    ![06](media/DB17.png)

1. **cosmicworks** コマンドがデータベース、コンテナー、アイテムの作成を完了するまで待ちます。

    ![06](media/DB16.png)

1. 統合ターミナルを閉じ、**Visual Studio Code** も閉じます。

    ![06](media/DB18.png)

1. **Azure ポータル** に戻ります。

1. Azure ポータルの上部にある「Search resources, services and docs (G+/)」ボックスに **Azure Cosmos DB (1)** と入力し、サービス一覧から **Azure Cosmos DB (2)** を選択します。

   ![06](media/New-image1.png)

1. **sql-<inject key="DeploymentID" enableCopy="false"/>** を選択します。

     ![06](media/New-image68.png)

1. **Azure Cosmos DB** アカウント リソースの **overview page (1)** で、**Data Explorer (2)** ペインに移動します。

    ![06](media/DB04.png)

1. **Data Explorer** で、**cosmicworks (2)** データベース ノードを展開し、**products (3)** コンテナー ノードを展開してから、**Items (4)** を選択します。

    ![06](media/DB19.png)

1. **products** コンテナー内のさまざまな JSON アイテムを確認して選択します。これらは前の手順で使用したコマンドライン ツールによって作成されたアイテムです。

   ![06](media/New-image12.png)

1. **Scale (1)** タブを選択します。Scale タブで **Manual (2)** を選択し、**required throughput** 設定を **4000 RU/s** から **400 RU/s (3)** に変更してから、変更内容を **Save (4)** します。

    ![06](media/step23-2304.png)

1. **Data Explorer** ペインで **+ New Container (1)** を選択し、**+ New Container (2)** を選択します。

    ![06](media/New-image14.png)

1. **New Container** ポップアップで、各設定に対して次の値を入力します。

    | **Setting**   | **Value** |
    | ------------- | --------- |
    | **Database id** | *既存を使用 (1)* &vert; *cosmicworks (2)* |
    | **Container id** | *`flatproducts` (3)* |
    | **Partition key** | *`/category` (4)* |
    | **Provision dedicated throughput for this container (5)** をチェック |
    | **Container throughput (autoscale)** | *Manual (6)* |
    | **RU/s** | *`400` (7)* |
    
    ![06](media/step25-2304.png)

1. 下にスクロールして **OK** をクリックします。

   ![06](media/New-image16.png)

1. **Data Explorer** ペインに戻り、**cosmicworks** データベース ノードを展開して、階層内の **flatproducts** コンテナー ノードを確認します。

     ![06](media/New-image17.png)
   
1. Azure ポータルの **Home** に戻ります。

    > **ラボが完了しました。お疲れ様でした!** では、検証を行います。以下の手順を実行してください:
    > - 対応するタスクの検証ボタンをクリックします。成功メッセージが表示されれば、ラボの検証に成功しています。
    > - そうでない場合は、エラー メッセージを注意深く読み、ラボ ガイドの指示に従ってステップを再度実行してください。
    > - ご不明な点がございましたら、cloudlabs-support@spektrasystems.com までお問い合わせください。24 時間対応でサポートいたします。

    <validation step="4f0ebcc4-a71c-450a-b7e0-5099feed58d5" />

### タスク 2: Azure Data Factory リソースを作成する

このタスクでは、Azure Data Factory リソースを作成し、1 回限りの ETL（抽出、変換、読み込み）操作を実行するように構成します。目標は、1 つの Azure Cosmos DB NoSQL コンテナー（products）から別のコンテナー（flatproducts）へデータを移動し、その過程で変換を適用することです。

Azure Cosmos DB for NoSQL リソースの準備ができたので、Azure Data Factory リソースを作成し、1 つの NoSQL API コンテナーから別のコンテナーへ 1 回限りのデータ移動を実行するために必要なすべてのコンポーネントと接続を構成して、データを抽出、変換、および別の NoSQL API コンテナーにロードします。

1. Azure ポータルのホームページで **+ Create a resource** を選択します。
  
     ![06](media/New-image37.png)
   
1. **Create a resource** ページで検索して **Azure Data Factory (1)** を選択し、次の設定で新しい **Azure Data Factory (2)** リソースを作成します（その他はデフォルトのままにします）。

    ![06](media/New-image18.png)

1. **Data Factory** で **Create (1)** を選択し、**Data Factory (2)** を選びます。

    ![06](media/New-image19.png)

1. 以下の設定を指定し、残りはすべてデフォルトのままにして **Next (6)** をクリックします。

    | **設定** | **値** |
    | --- | --- |
    | **Subscription** | *既存の Azure サブスクリプション* **(1)** |
    | **Resource group** | *Cosmosdb-<inject key="DeploymentID" enableCopy="false"/>* **(2)** |
    | **Name** | *datafactory-<inject key="DeploymentID" enableCopy="false"/>* **(3)**|
    | **Region** | *任意の利用可能なリージョンを選択* **(4)**|
    | **Version** | *V2* **(5)**|

    ![06](media/New-image20.png)

1. **Git configuration** ブレードで **Configure Git later (1)** のチェックボックスを選択し、**Review + Create (2)** をクリックして **Create (3)** を選択します。

    ![06](media/DB21.png)

    ![06](media/DB23.png)

1. リソースのデプロイが完了したら **Go to resource** をクリックします。

    ![06](media/New-image21.png)

1. リソース グループで、一覧から **datafactory** リソースを選択します。

    ![06](media/DB20.png)

1. **Azure Data Factory Studio** の下で **Launch studio** を選択します。

    ![06](media/New-image22.png)
   
    > 💡 代替方法として、``adf.azure.com/home`` に移動し、作成した Data Factory リソースを選択してからホーム アイコンを選択することもできます。

1. **Home** 画面から、**Ingest** オプションを選択して、ワンタイムの大規模データ コピー操作を行うクイックウィザードを開始し、ウィザードの **Properties** ステップに移動します。

   ![06](media/New-image23.png)

1. ウィザードの **Properties** ステップで、**Task type** セクションから **Built-in copy task (1)** を選択します。**Task cadence or task schedule** セクションで **Run once now (2)** を選択し、**Next (3)** を選択してウィザードの **Source** ステップに進みます。

    ![06](media/New-image24.png)
   
1. **Source** ステップで、**Source type** リストから **Azure Cosmos DB NoSQL (1)** を選択し、**Connection** セクションで **+ New connection (2)** を選択します。

    ![06](media/New-image25.png)

1. **New connection (Azure Cosmos DB for NoSQL)** ポップアップで、次の値を使用して新しい接続を構成し、**Create (9)** を選択します:

    | **設定** | **値** |
    | --- | --- |
    | **Name** | *`CosmosSqlConn`* **(1)** |
    | **Connect via integration runtime** | *AutoResolveIntegrationRuntime* **(2)** |
    | **Authentication method** | *Account key* **(3)** | *Connection string* **(4)** |
    | **Account selection method** | *From Azure subscription* **(5)** |
    | **Azure subscription** | *既存の Azure サブスクリプション* **(6)** |
    | **Azure Cosmos DB account name** | *このラボで先に作成した既存の Azure Cosmos DB アカウント名* **(7)** |
    | **Database name** | *cosmicworks* **(8)**|

    ![06](media/New-image26.png)

1. **Source data store** セクションに戻り、**Source tables** 内で **Query (1)** を選択し、**Table name** リストから **products (2)** を選択します。

    ![06](media/New-image27.png)

1. **Query** エディターで既存の内容を削除し、次のクエリを入力します **(1)**:

    ```
    SELECT 
        p.name, 
        p.categoryName as category, 
        p.price 
    FROM 
        products p
    ```

1. 下にスクロールして **Preview data (2)** を選択し、クエリの妥当性をテストします。**Next (3)** を選択してウィザードの **Destination** ステップに進みます。

    ![06](media/New-image28.png)
   
1. **Destination** ステップで、**Destination type** リストから **Azure Cosmos DB for NoSQL (1)** を選択し、**Connection** リストから **CosmosSqlConn (2)** を選択して、**Custom query** で **flatproducts (3)** を選択し、**Next (4)** を選択してウィザードの **Settings** ステップに進みます。

   ![06](media/New-image29.png)

   > **注意:** 表示されるまでに少し時間がかかる場合があります。

1. ウィザードの **Settings** ステップの **Task name** フィールドに **`FlattenAndMoveData`(1)** と入力し、**Next (2)** を選択します。

    ![06](media/New-image30.png)

1. 残りのフィールドはすべて空のままにして **Next** を選択し、ウィザードの最終ステップに進みます。

    ![06](media/New-image31.png)

1. ウィザードで選択した手順の **Summary** を確認し、**Finish** を選択します。

    ![06](media/New-image32.png)

1. Web ブラウザーのウィンドウまたはタブを閉じます。

1. **Azure ポータル** に戻ります。

1. Azure ポータルの上部にある「Search resources, services and docs (G+/)」ボックスに **Azure Cosmos DB (1)** と入力し、サービス一覧から **Azure Cosmos DB (2)** を選択します。

   ![06](media/New-image1.png)

1. **sql-<inject key="DeploymentID" enableCopy="false"/>** を選択します。

1. **Azure Cosmos DB** アカウント リソース内で **Data Explorer (1)** ペインに移動します。**Data Explorer** で **cosmicworks** データベース ノードを展開し、**flatproducts (2)** コンテナー ノードを選択して **New SQL Query (3)** を選択します。

    ![06](media/New-image33.png)

1. エディター領域の内容を削除します。

1. 名前が **HL Headset** と等しいすべてのドキュメントを返す新しい SQL クエリを作成し、**Execute Query (2)** を選択します。

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

1. クエリ結果を確認します。

    ![06](media/New-image35.png)

1. Web ブラウザーのウィンドウまたはタブを閉じます。

    > **おめでとうございます** — ラボを完了しました！ 検証手順は次のとおりです:
    > - 対応するタスクの検証ボタンをクリックします。成功メッセージが表示されれば、ラボの検証に成功しています。
    > - そうでない場合は、エラーメッセージを注意深く読み、ラボ ガイドの指示に従って手順を再実行してください。
    > - サポートが必要な場合は、cloudlabs-support@spektrasystems.com までお問い合わせください。24時間対応でサポートを提供しています。

   <validation step="e513aa34-ca14-4de5-a2f3-139f051e5c35" />

### 要約

このラボは、Azure Data Factory を使用して 2 つの Azure Cosmos DB コンテナー間でデータを移行する方法に焦点を当てています。まず Azure Cosmos DB for NoSQL アカウントをセットアップしてサンプルデータを投入します。次に、1 回限りの ETL 操作を実行するための Azure Data Factory リソースを作成し、あるコンテナーからデータを抽出、変換して別のコンテナーにロードします。本ラボでは、効率的なデータ移動と変換のために Azure サービスを統合する方法を示します。
 
### レビュー

このラボで完了した項目:

- Azure Cosmos DB NoSQL API アカウントを作成し、データをシードしました。
- Azure Data Factory リソースを作成しました。

### ラボは正常に完了しました。
