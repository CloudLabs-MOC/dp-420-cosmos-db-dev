---
lab:
    title: 'ラボ環境のセットアップ'
    module: 'セットアップ'
---

# ローカルラボ環境のセットアップ

理想的には、これらのラボはホストされたラボ環境で実施してください。ご自身のコンピュータで実施する場合は、以下のソフトウェアをインストールすることで進められます。ご自身の環境では予期しないダイアログや動作が発生することがあり得ます。ローカル環境の構成は多岐にわたるため、コースチームは個別の環境で発生する問題についてサポートできない場合があります。

## Windows のインストール

> &#128221; 以下の手順は Windows 10 コンピュータ向けです。Linux や MacOS でも実行できます。選択した OS に合わせてラボ手順を調整する必要があるかもしれません。

### Windows 10 (OS)

1. Windows 10 をインストールします（*version 2004 以降*）。

1. 利用可能なすべての更新プログラムを適用します。

### Edge

1. 以下から最新の Microsoft Edge をインストールしてください: [microsoft.com/edge].

### .NET 6 SDK

1. SDK（ランタイムではなく）を以下からダウンロードしてインストールしてください: [dotnet.microsoft.com/download/dotnet/6.0].

### PowerShell 7

1. 以下からダウンロードしてインストールしてください: [github.com/powershell/powershell/releases].

### Git

1. 以下からダウンロードしてインストールしてください: [git-scm.com/downloads].

    - インストーラーではデフォルトのオプションを使用してください。

### Windows Terminal

1. 以下からダウンロードしてインストールしてください: [github.com/microsoft/terminal/releases].

1. **PowerShell** を既定のターミナルとして設定してください

### Visual Studio Code (and extensions)

### Visual Studio Code（および拡張機能）

1. 以下からダウンロードしてインストールしてください: [code.visualstudio.com/download].

    - インストーラーではデフォルトのオプションを使用してください。

1. インストール後、Visual Studio Code を起動してください。

1. **Extensions**（拡張機能）メニューで、Microsoft の以下の拡張機能を検索してインストールしてください:

    - [C#][marketplace.visualstudio.com/ms-dotnettools.csharp]

### Azure Cosmos DB エミュレーター

1. 以下からダウンロードしてインストールしてください: [docs.microsoft.com/azure/cosmos-db/local-emulator].
    - インストーラーではデフォルトのオプションを使用してください。

[code.visualstudio.com/download]: https://code.visualstudio.com/download
[docs.microsoft.com/azure/cosmos-db/local-emulator]: https://docs.microsoft.com/azure/cosmos-db/local-emulator#download-the-emulator
[dotnet.microsoft.com/download/dotnet/6.0]: https://dotnet.microsoft.com/download/dotnet/6.0
[git-scm.com/downloads]: https://git-scm.com/downloads
[github.com/microsoft/terminal/releases]: https://github.com/microsoft/terminal/releases/latest
[github.com/powershell/powershell/releases]: https://github.com/powershell/powershell/releases/latest
[marketplace.visualstudio.com/ms-dotnettools.csharp]: https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp
[microsoft.com/edge]: https://microsoft.com/edge
