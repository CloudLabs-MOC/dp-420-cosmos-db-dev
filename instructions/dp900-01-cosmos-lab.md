# ラボ 01: Azure Cosmos DB を探索する

## ラボ シナリオ

このラボでは、Azure サブスクリプション内に Core (SQL) API を使用する Azure Cosmos DB データベースをプロビジョニングし、非リレーショナル データの格納にどのように活用できるかを、さまざまな方法で確認します。

## ラボの目的

このラボでは、以下のタスクを実行します。

+ タスク 1: Cosmos DB アカウントを作成する
+ タスク 2: サンプル データベースを作成する
+ タスク 3: アイテムの表示と作成を行う
+ タスク 4: データベースにクエリを実行する

## タスク 1: Cosmos DB アカウントを作成する

このタスクでは、Azure Cosmos DB SQL アカウントをプロビジョニングし、必要な設定を構成した上で、今後の開発に必要な接続情報を取得します。

1. **Azure Portal** ページ上部にある「Search resources, services and docs (G+/)」ボックスに **Azure Cosmos DB (1)** と入力し、サービス一覧から **Azure Cosmos DB (2)** を選択します。

1. **Azure Cosmos DB for NoSQL** の下で **+ Create (1)** を選択し、**Create (2)** をクリックして **Azure Cosmos DB for NoSQL** アカウントを作成します。

1. 以下の詳細を入力します。

    | Setting | Value |
    |----------|----------|
    | Workload type | Learning **(1)** |
    | Subscription | ご利用の Azure サブスクリプションを選択 **(2)** |
    | Resource group | DP-900-Module-3-<inject key="DeploymentID" enableCopy="false"/> **(3)** |
    | Account name | cosmosdb-<inject key="DeploymentID" enableCopy="false"/> **(4)** |
    | Location | 利用可能な任意のリージョンを選択 **(5)** |
    | Capacity mode | Provisioned throughput **(6)** |
    | Apply Free Tier Discount | Apply **(7)** |
    | Limit total account throughput | チェックを外したままにする **(8)** |

1. **Review + create (9)** をクリックします。


1. 構成の検証が完了したら、**Create** を選択します。


1. デプロイが完了するまで待ちます。完了したら、デプロイされたリソースに移動します。


## タスク 2: サンプル データベースを作成する

このタスクでは、事前構成済みの設定を確認しながら、SampleDB データベースとそのコンテナーが作成される過程を観察します。
*この手順を通して、ポータルに表示されるヒントはすべて閉じてください*。

1. **Azure Cosmos DB account** で、左側のメニューから **Overview (1)** を選択し、続いて **Data Explorer (2)** をクリックします。



    >**注:** Welcome What is Cosmos DB というポップアップが表示された場合は、**X** をクリックして閉じてください。

1. **Data Explorer (1)** ページで、**Launch quick start (2)** を選択します。

  

1. **New container** タブで、サンプル データベース用に事前入力された設定を確認し、**OK** を選択します。


1. 画面下部のパネルでステータスを確認し、**SampleDB** データベースとその **SampleContainer** コンテナーが作成されるまで待ちます（1 分程度かかる場合があります）。

   

## タスク 3: アイテムの表示と作成を行う

このタスクでは、Data Explorer インターフェイスを使用して、Cosmos DB コンテナー内の既存アイテムの表示、新しいアイテムの作成、および JSON データの操作方法を紹介します。これは、実際のアプリケーションで開発者がデータを操作・管理する方法を模したものです。

1. **Data Explorer** ページで **SampleDB (1)** を展開し、続いて **SampleContainer (2)** を展開して、**Items (3)** を選択しアイテムの一覧を表示します。

1. 一覧からいずれかのアイテム (4) を選択して JSON 表現を確認し、その後選択を解除して新しいアイテムの作成に進みます。

1. ページ上部の **New Item (5)** をクリックして、新しい空のアイテムを作成します。


1. 新しいアイテムの JSON を以下のように変更し (1)、**Save (2)** をクリックします。


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


1. 新しいアイテムを保存すると、追加のメタデータ プロパティが自動的に付与されることが確認できます。

## タスク 4: データベースにクエリを実行する

このタスクでは、Data Explorer インターフェイスを使用して Cosmos DB コンテナー内のアイテムを作成・表示・クエリする方法を紹介します。これは、実際のアプリケーションで開発者が SDK を使用してデータベースを操作する方法を模したものです。

1. **Data Explorer (1)** ページで、**New SQL Query (2)** アイコンを選択します。


2. 既定でクエリ **SELECT * FROM c (1)** が表示されています。**Execute Query (2)** をクリックします。


1. すべてのアイテムの完全な JSON 表現を含む結果を確認します。


1. クエリを次のように変更します **(1)**:

    ```sql
    SELECT * FROM c
    WHERE CONTAINS(c.name,"Helmet")
    ```

1. **Execute Query (2)** ボタンをクリックして変更後のクエリを実行し、**name** フィールドに **Helmet (3)** というテキストを含むアイテムの JSON エンティティが結果に含まれていることを確認します。


1. 変更を破棄して SQL Query エディターを閉じます。

    >**注:** ここまでで、Azure ポータルの Data Explorer インターフェイスを使用して、Cosmos DB データベース内の JSON エンティティを作成・クエリする方法を確認しました。実際のシナリオでは、アプリケーション開発者は、さまざまなプログラミング言語向けのソフトウェア開発キット (SDK) のいずれかを使用して Core (SQL) API を呼び出し、データベース内のデータを操作します。

    > **おめでとうございます** — ラボを完了しました！ 次に検証を行います。手順は次のとおりです。
    > - 対応するタスクの Validate ボタンをクリックします。成功メッセージが表示されれば、ラボの検証に成功しています。
    > - 表示されない場合は、エラー メッセージを注意深く読み、ラボ ガイドの手順に従って再実行してください。
    > - サポートが必要な場合は、cloudlabs-support@spektrasystems.com までお問い合わせください。24 時間対応でサポートいたします。

    <validation step="0c506d8c-06e7-4eb7-aa88-fed3d24ffcfc" />

## 要約

このラボでは、Azure Cosmos DB でのデータの作成、表示、変更、クエリを実際に体験しました。これは、実際のアプリケーションで非リレーショナル データを格納する際に役立つスキルです。

## レビュー

このラボで完了した項目:
- Cosmos DB アカウントを作成しました
- サンプル データベースを作成しました
- アイテムの表示と作成を行いました
- データベースにクエリを実行しました

## ラボは正常に完了しました
