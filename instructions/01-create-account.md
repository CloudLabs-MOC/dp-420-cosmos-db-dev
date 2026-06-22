# Lab 01 - Azure Cosmos DB SQL API を使って始める

## ラボ シナリオ

Azure Cosmos DB を深く掘り下げる前に、最もよく使用するリソースの作成に関する基本を把握しておくことが重要です。多くのシナリオでは、アカウント、データベース、コンテナー、アイテムの作成に慣れている必要があります。実運用に近いシナリオでは、作成したリソースが正しく動作するか確認するための基本的なクエリをいくつか用意しておくと良いでしょう。

このラボでは、SQL API を使用して新しい Azure Cosmos DB アカウントを作成します。その後、Data Explorer を使ってデータベース、コンテナー、2 つのアイテムを作成し、最後に作成したアイテムをクエリします。

## ラボの目的

このラボで完了するタスク:
- タスク 1: 新しい Azure Cosmos DB アカウントを作成する。
- タスク 2: Data Explorer を使用して新しいデータベースとコンテナーを作成する。
- タスク 3: Data Explorer を使用して新しいアイテムを作成する。
- タスク 4: Data Explorer を使用して基本的なクエリを実行する。

## 推定所要時間: 30 分

## アーキテクチャ図

![image](architecturedia/lab1.png)

## 演習 1: Azure Cosmos DB SQL API アカウントを作成する

### タスク 1: 新しい Azure Cosmos DB アカウントを作成する

Azure Cosmos DB は複数の API をサポートするクラウドベースの NoSQL データベース サービスです。Azure Cosmos DB アカウントを初めてプロビジョニングする際は、アカウントでサポートする API（例: **Mongo API** や **SQL API**）を選択します。

1. 新しい Web ブラウザーのウィンドウまたはタブで、Azure ポータル (``portal.azure.com``) に移動します。

1. サブスクリプションに関連付けられた Microsoft の資格情報を使用してポータルにサインインします。

1. **Azure services** カテゴリで **Create a resource** を選択し、次に **Azure Cosmos DB** を選択します。

    > &#128161; 代替手順: **&#8801;** メニューを展開し **All Services** を選択、**Databases** カテゴリで **Azure Cosmos DB** を選択してから **Create** を選びます。

1. **Select API option** ウィンドウで、**Azure Cosmos DB for NoSQL** セクション内の **Create** オプションを選択します。

1. **Create Azure Cosmos DB Account** ウィンドウの **Basics** タブを確認します。

1. **Basics** タブで、各設定に対して次の値を入力します:

    | **設定** | **値** |
    | --: | :-- |
    | **Subscription** | *既存の Azure サブスクリプション* |
    | **Resource Group** | *DP-420-DeploymentID* |
    | **Account Name** | *グローバルに一意の名前を入力* |
    | **Location** | *利用可能なリージョンを選択* |
    | **Capacity mode** | *プロビジョニング済みスループットを選択* |
    | **Apply Free Tier Discount** | *適用しない* |

    >**注** : DeploymentID は各環境に関連付けられた一意の ID です。値は環境の詳細ページで確認できます。

1. **Review + Create** を選択して **Review + Create** タブに移動し、続けて **Create** を選択します。

    > &#128221; Azure Cosmos DB SQL API アカウントが使用可能になるまでに 10～15 分かかることがあります。

1. **Deployment** ペインを確認します。デプロイが完了すると、ペインに **Deployment successful** のメッセージが表示されます。

1. 引き続き **Deployment** ペイン内で、**Go to resource** を選択します。

### タスク 2: Data Explorer を使って新しいデータベースとコンテナーを作成する

Data Explorer は、Azure ポータル内で Azure Cosmos DB SQL API のデータベースとコンテナーを管理するための主要なツールです。このラボでは、基本的なデータベースとコンテナーを作成します。

1. **Azure Cosmos DB account** ペイン内のリソース メニューから **Data Explorer** を選択します。

1. **Data Explorer** ペインで **New Container** を選択します。

1. **New Container** ポップアップで、各設定に対して次の値を入力し、**OK** を選択します:

    | **Setting** | **Value** |
    | --: | :-- |
    | **Database id** | *cosmicworks* |
    | **Share throughput across containers** | *Do not select* |
    | **Container id** | *products* |
    | **Partition key** | */categoryId* |
    | **Container throughput (autoscale)** | *Manual* |
    | **RU/s** | *400* |

1. **Data Explorer** ペインに戻り、**cosmicworks** データベース ノードを展開して、階層内の **products** コンテナー ノードを確認します。

### タスク 3: Data Explorer を使って新しいアイテムを作成する

Data Explorer には、Azure Cosmos DB SQL API コンテナー内のアイテムをクエリ、作成、管理するための機能が一式含まれています。ここでは、Data Explorer の生の JSON を使って 2 つの基本アイテムを作成します。

1. **Data Explorer** ペインで **cosmicworks** データベース ノードを展開し、**products** コンテナー ノードを展開してから **Items** を選択します。

1. 引き続き **Data Explorer** ペインで、コマンド バーから **New Item** を選択します。エディターでプレースホルダーの JSON アイテムを次の内容に置き換えます:

    ```
    {
      "categoryId": "4F34E180-384D-42FC-AC10-FEC30227577F",
      "categoryName": "Components, Pedals",
      "sku": "PD-R563",
      "name": "ML Road Pedal",
      "price": 62.09
    }
    ```

1. コマンド バーの **Save** を選択して、最初の JSON アイテムを追加します。

1. **Items** タブに戻り、コマンド バーから **New Item** を選択します。エディターでプレースホルダーの JSON アイテムを次の内容に置き換えます:

    ```
    {
      "categoryId": "75BF1ACB-168D-469C-9AA3-1FD26BB4EA4C",
      "categoryName": "Bikes, Touring Bikes",
      "sku": "BK-T18Y-44",
      "name": "Touring-3000 Yellow, 44",
      "price": 742.35
    }
    ```

1. コマンド バーの **Save** を選択して、2 つ目の JSON アイテムを追加します。

1. **Items** タブで、**Items** ペインに 2 つの新しいアイテムが表示されていることを確認します。

### タスク 4: Data Explorer を使って基本的なクエリを実行する

最後に、Data Explorer には組み込みのクエリエディターがあり、クエリの発行、結果の確認、および要求単位（RU/s）での影響測定に使用されます。

1. **Data Explorer** ペインで **New SQL Query** を選択します。

1. クエリ タブで **Execute Query** を選択し、フィルターなしで全アイテムを選択する標準クエリを表示します。

1. エディター領域の内容を削除します。

1. **Query** タブで、プレースホルダーのクエリを次の内容に置き換えます:

    ```
    SELECT * FROM products p WHERE p.price > 500
    ```

    > &#128221; このクエリは **price** が $500 より大きいすべてのアイテムを選択します。

1. **Execute Query** を選択します。

1. クエリの結果を確認します。結果には単一の JSON アイテムとそのすべてのプロパティが含まれているはずです。

1. **Query** タブで **Query Stats** を選択します。

1. 引き続き **Query** タブで、**Query Statistics** セクション内の **Request Charge** フィールドの値を確認します。

    > &#128221; 通常、この単純なクエリのリクエスト チャージは、コンテナー サイズが小さい場合に約 2～3 RU/s です。

1. Web ブラウザーのウィンドウまたはタブを閉じます。

### レビュー

このラボで行ったこと:
- 新しい Azure Cosmos DB アカウントを作成しました。
- Data Explorer を使用して新しいデータベースとコンテナーを作成しました。
- Data Explorer を使用して新しいアイテムを作成しました。
- Data Explorer を使用して基本的なクエリを実行しました。

### ラボを正常に完了しました

