

# kokoni-SampleLineEchoBot

## 前提

・利用可能なAzureアカウントを持っていること

・Lineアカウントを取得済みであること

## 参考

MessageAPIの開設は下記を参考にしてください。

[LINE BOTの作り方を世界一わかりやすく解説（１）【アカウント準備編】](http://qiita.com/yoshizaki_kkgk/items/bd4277d3943200beab26)

AzureFunctionsの環境構築方法は下記を参考にしてください。

[Visual Studioで始めるサーバーレス「Azure Functions」開発入門](http://www.buildinsider.net/pr/microsoft/azure/dictionary06)

## カスタムデプロイ

ARMテンプレートによるカスタムデプロイリンクで環境構築できるようにしました。
下記のリンクをクリックしてデプロイ環境を構築してみてください。

|デプロイ方法|デプロイリンク|
| --------------- |:---------------:|
| Direct Portal Deploy | [![Deploy to Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fkingkino%2Fkokoni-SmapleLineEchoBot%2Fmaster%2FAzureDeploy.json) |

## 設定手順

AzureFuntionsの作成方法についてはここでは省略します。

AzureFunctionsを作成したらHTTPTriggerCsharpテンプレートを選択してFunctionを作成します。

新しい関数からテンプレートの選択に移動し言語をC#に絞込み「HttpTriggerCsharp」を選択しましょう。

関数名は既存の関数と重複しない任意の名前にしてください。

![参考画像01](https://github.com/kingkino/kokoni-SmapleLineEchoBot/blob/master/refer01.png)

関数が作成されたら開発を選択し右側ペインにあるファイルの表示から「run.csx」を選択します。
中央のEditorにソースが表示されるので[本リポジトリのrun.csx](https://github.com/kingkino/kokoni-SmapleLineEchoBot/blob/master/kokoni-SampleLineEchoBot/kokoni-SampleLineEchoBot/HttpTriggerCSharp/run.csx)の内容で上書きします。
上書きをしたらSAVEを押下して保存してください。

![参考画像02](https://github.com/kingkino/kokoni-SmapleLineEchoBot/blob/master/refer02.png)

次に関数のURLを取得します。上段にある「</> Get function URL」を押下し表示されたURLを控えて起きましょう。

![参考画像03](https://github.com/kingkino/kokoni-SmapleLineEchoBot/blob/master/refer03.png)

[LineのMessageAPI管理画面](https://business.line.me/ja/services/bot)に移動します。
ここではWebhook用に関数のURLを登録しAccessTokenを取得します。
Editボタンを押下してWebhookに先ほど控えた関数のURLを登録しましょう。
次にChannelAccessTokenを発行して控えておいてください。

![参考画像06](https://github.com/kingkino/kokoni-SmapleLineEchoBot/blob/master/refer06.png)

AzureFunctionsの設定画面に戻ります。
控えておいたChannelAccessTokenをAppServiceのプロパティに登録します。
Tokenはソースに直接記述しても動きますがセキュリティを考慮してAppServiceのプロパティに追加するほうがいいです。

FunctionAppの設定からAppServiceの設定に移動を押下します。

![参考画像04](https://github.com/kingkino/kokoni-SmapleLineEchoBot/blob/master/refer04.png)

アプリ設定の項目にKey：Valueの形式で登録します。
ソース上で呼び出す時のKEY名を「ChannelAccessToken」としているのでKEYを「ChannelAccessToken」として値のところに控えておいたTokenを貼り付けて保存してください。

![参考画像05](https://github.com/kingkino/kokoni-SmapleLineEchoBot/blob/master/refer05.png)

これで一通りの作業は完了です。

Lineアカウントを友達登録して実行してみてください。入力した文字がそのまま返信されれば成功です。
