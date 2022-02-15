# getstepsystem

このシステムは使用者の歩数を取得し、slackに流す、という動作をしています。
当初、研究室内のコミュニケーションを向上させるため、また、DERCの実験で使用するために作成を行いました。
以下のサイトを主に参考にしています。
https://qiita.com/kusunamisuna/items/669fa324d4612dfdd7bf


## システム概要
使用者自身のスマートフォンやスマートウォッチで歩数を記録、Googlefitに同期させて、サーバーが夜に一括で使用者の記録された歩数を取得し、GoogleSpreadSheetに保存すると同時に、Slackで共有を行っています。
Googlefiに同期させるということが大事です。2021年時点ではサーバーが歩数を取得できるツールはGooglefitしかありませんでした。アンドロイドであれば、取得した歩数がGooglefitに自動的に保存されるのですが、iPhoneではデフォルトアプリの「ヘルスケア」とGooglefitの歩数を同期させる必要があります。
GoogleFitに保存してもらったのちに、定期実行設定されたpythonファイルを夜に走らせます。
![image](https://user-images.githubusercontent.com/91872741/153996261-1956bfbd-8b1a-44f8-9bd5-40fbeb8e85ea.png)


## 準備
・pythonファイルで各使用者の歩数を取得するためには、各Googleアカウントのアクセスコードを取得する必要があります。これは「getaccescode.py」で取得し、保存しておきます。（僕はsecretに保存していました。）

・slackのワークスペースに入ってもらうようにしてください。

・pythonファイルからGoogleSpreadSheetに保存するために、準備が必要です。
https://tanuhack.com/operate-spreadsheet/

・pythonファイルからSlackに投稿するために、準備が必要です。
https://blog.imind.jp/entry/2020/03/07/231631


## ファイルの説明
これまでに説明したファイルもありますが、ここで一括で説明しときます。
getfit.py...メインのファイルです。

secret...各被験者のアクセスコードを保存してあります。個人情報のため、ファイルの中は空にしてあります。

getaccescode.py...各被験者のアクセスコードを取得するためのファイルです。走らせるとURLを得ることができるので、そのURLを使用者に踏んでもらって、アクセスコードを取得するための、キー？みたいなものを発行してもらいます。キーを実行環境に打ち込めば、アクセスコードが保存されます。ここで注意してもらいたいのはキーを発行してもらっても15分で使えなくなります。

renkei_spreadsheet.json...名前かっこ悪いですが、直す時間がなくてこのまま公表します...。pythonからSpreadSheetを操作するために必要なファイルです。これも個人情報なので、中身は消してあります。

スプレッドシート例...保存用のスプレッドシートの例です。

他は、使用者に操作方法を説明するためのpdfです。


## 作成者から一言
まず、クソコードですみません。本当に。
執筆時点ですでにpythonのslackパッケージが非推奨になっており、そのうち使えなくなると思います。slack_sdkやwebhookなどが代わりに使えるので、使えなくなったらそちらを当たってみてください。いずれにしても単純なシステムなので、すぐに創れると思います。iphoneからGoogleFitの同期は手動でしかできませんでしたが、おそらくそろそろ自動での同期が可能になるのかなと思います。また、SpreadSheetに保存していますが、普通にDBに保存した方が楽です。なので、これを参考にしていろいろカスタマイズして作成してみてください。
