### GooglePlayStore関連でのやらかしまとめ

アプリ部は色々やらかしてます。</br>
これからアプリを始めるよって人安心してください。大体のことは先輩方経験済みです。</br>
以下、経緯と対応方法についてまとめます。</br>

### 【管理者アカウントが紛失したときの対処法】
###### 経緯
ある時、 Androidのautomated deploymentを作成していた。</br>
StagingはFirebase。ProductionはGooglePlayStoreにデプロイすると言うGitHubAction</br>
GooglePlayStoreにデプロイするには何やらKeyが必要らしい。</br>
そのKeyは管理者アカウントでGoogleConsoleに入ると見れるって!</br>
管理者アカウントって何です? ...誰も知らない
###### 対処方法
1.Googleサポートにチャットで連絡</br>
2.Googleからアカウント所有者として登録されたメールアドレスの変更はできない。</br>
新規アカウントをご登録の上、既存のアプリを新規デベロッパーアカウントに移行していただくしかないとの返答。</br>
3.新規のデベロッパーアカウントの作成をDVSで行った。</br>
4.PDF形式で下記内容を英文で作成してGoogleに送付した。
```
該当のアプリを所有しており移行を希望するという内容の文章（どのような文章でも構いません）
該当のアプリのパッケージ名
移行先のアカウント及び取引ID
```
5.数日後に無事に新しいアカウントへの移行が済みました。

### 署名Keyを消し飛ばしたときの対処法
###### 経緯
Androidには署名Keyが必要です。これは各自が生成します。</br>
GooglePlayStoreに1回登録すると変更はできません。</br>
ある日、お試し審査でアプリをGooglePlayStoreに出しました。</br>
そして2回目のお試し審査で署名Keyが違うと弾かれました。</br>
つまり署名Keyの紛失...

###### 対処方法
1.Googleサポートにチャットで連絡</br>
2.Googleから当該アプリはGooglePlayアプリ署名にご登録済みのためアップロード鍵の再登録が必要との返答</br>
3.以下コマンドで新しい署名Keyを生成して、その鍵の証明書をPEM形式でエクスポートしてGoogleに返す
```
// 新しい署名Keyを生成
keytool -genkeypair -alias upload -keyalg RSA -keysize 2048 -validity 9125 -keystore upload.jks -storetype JKS

// PEM形式でエクスポート
keytool -export -rfc -alias PEMtest -file upload_certificate.pem -keystore /Users/tester/Desktop/Key/PEMtest
```
4.GoogleサポートにPEM形式でエクスポートしたファイルを返送した。数日で再登録されました。

### 最後に

覚えてるのは２件です。また、何かやりましたら追記の予定です。

GooglePlayStore関連の事はGoogleに聞くのが一番早いです!</br>
取り敢えず落ち着いてGoogleサポートに連絡してみましょう。</br>

経験的に、メールよりチャットの方が早い気がします。</br>
そして、Googleは日本人が対応してくれるので、日本語で大丈夫です。</br>

Appleは英語と聞いた事があります。</br>
真実か知りませんが気になる人はiOSエンジニアに聞いてみましょう。</br>

