---
lab:
    title: 'Azure リソース プロバイダーを有効にする'
    module: 'Setup'
---

# Azure リソース プロバイダーを有効にする

Azure サブスクリプションに登録する必要があるリソース プロバイダーがいくつかあります。これらが登録されていることを確認するために、次の手順に従ってください。

1. 新しい Web ブラウザーのウィンドウまたはタブで、Azure ポータル (``portal.azure.com``) に移動します。

1. サブスクリプションに関連付けられている Microsoft 資格情報を使用してポータルにサインインします。

1. **Home** ページで、**Subscriptions** を選択します。

    > &#128161; 代わりに、**&#8801;** メニューを展開し、**All Services** を選択し、**All** カテゴリで **Subscriptions** を選択します。

1. Azure サブスクリプションを選択します。

    > &#128221; 複数のサブスクリプションがある場合は、Azure Pass を利用して作成したサブスクリプションを選択してください。

1. サブスクリプションのブレードで、**Settings** セクションの **Resource providers** を選択します。

1. リソース プロバイダーの一覧で、次のプロバイダーが登録済みであることを確認します:
    - [Microsoft.DocumentDB][docs.microsoft.com/azure/templates/microsoft.documentdb/databaseaccounts]
    - [Microsoft.Insights][docs.microsoft.com/azure/templates/microsoft.insights/components]
    - [Microsoft.KeyVault][docs.microsoft.com/azure/templates/microsoft.keyvault/vaults]
    - [Microsoft.Search][docs.microsoft.com/azure/templates/microsoft.search/searchservices]
    - [Microsoft.Web][docs.microsoft.com/azure/templates/microsoft.web/sites]

    > &#128221; プロバイダーが登録されていない場合は、そのプロバイダーを選択してから **Register** を選択します。

1. Web ブラウザーのウィンドウまたはタブを閉じます。

[docs.microsoft.com/azure/templates/microsoft.documentdb/databaseaccounts]: https://docs.microsoft.com/azure/templates/microsoft.documentdb/databaseaccounts
[docs.microsoft.com/azure/templates/microsoft.insights/components]: https://docs.microsoft.com/azure/templates/microsoft.insights/components
[docs.microsoft.com/azure/templates/microsoft.keyvault/vaults]: https://docs.microsoft.com/azure/templates/microsoft.keyvault/vaults
[docs.microsoft.com/azure/templates/microsoft.search/searchservices]: https://docs.microsoft.com/azure/templates/microsoft.search/searchservices
[docs.microsoft.com/azure/templates/microsoft.web/sites]: https://docs.microsoft.com/azure/templates/microsoft.web/sites
