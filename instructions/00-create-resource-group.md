---
lab:
    title: 'ラボ用リソースグループの作成'
    module: 'Setup'
---

# ラボ用の Azure リソース グループを作成する

このラボを完了する前に、新しくデプロイされる Azure リソースを配置するための新しい [リソース グループ][docs.microsoft.com/azure/azure-resource-manager/management/manage-resource-groups-portal] を作成してください。

1. 新しいウェブブラウザーのウィンドウまたはタブで、Azure ポータル（``portal.azure.com``）に移動します。

1. サブスクリプションに紐づいた Microsoft の資格情報を使用してポータルにサインインします。

1. **ホーム** ページで **リソース グループ** を選択します。

    > &#128161; 補足：**&#8801;** メニューを展開し、**すべてのサービス (All Services)** を選択し、**すべて (All)** カテゴリで **リソース グループ** を選択することもできます。

1. **+ 作成** を選択します。

1. **リソース グループの作成** ポップアップで、以下の設定を使用して新しいリソース グループを作成し、残りの設定はすべて既定値のままにします:

    | **設定** | **値** |
    | ---: | :--- |
    | **サブスクリプション** | *既存の Azure サブスクリプション* |
    | **リソース グループ** | *リソース グループに固有の名前を付ける* |
    | **リージョン** | *利用可能な任意のリージョンを選択* |

1. このタスクを続行する前に、デプロイ作業が完了するまで待ちます。

[docs.microsoft.com/azure/azure-resource-manager/management/manage-resource-groups-portal]: https://docs.microsoft.com/azure/azure-resource-manager/management/manage-resource-groups-portal
