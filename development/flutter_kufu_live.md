### ヨムーノアプリをflutterで作成して得た知見の話

<details>
<summary>フロント関連の話</summary>

##### flutterのwidgetの確認は下記画面サイズのエミュレーターで見ると良い!
  - 5.8インチ(所謂、普通のスマホサイズ)
  - 4.7インチ(小さめのスマホサイズ)
</details>

<details>
<summary>インフラ関連の話</summary>

##### FlutterからAndroidを開く
- FlutterPJからAndroidだけを開いてゴニョゴニョしたいときに役立つ豆知識</br>
  android選択して右クリック > Flutter > Open Android Module in Android Studio</br>
  - Flutterが選択できない時はandroid配下に「(プロジェクト名)_android.iml」を配置してあげると選択できるようになる</br>
    参考) https://minpro.net/open-android-module-in-android-studio-is-disabled


##### アプリの自動デプロイは便利!
- 色々ありましたが今は元気に動いてます。詳しい内容についてはComing Soon...
</details>

<details>
<summary>Google関連の話</summary>

##### AndroidのKeyを吹き飛ばした話
Androidのリリースには署名Keyが必要です。このKeyは自分で作成します。</br>
1回GooglePlayに配信すると署名の変更は認められません。</br>
お試し審査でリリース前にヨムーノをGooglePlayに出しました。</br>
で、無くしました。消し飛んだと言うより上書き? ともかくKeyを紛失してしまったわけです。</br>

と言うわけで、何とかせねばなりません！

【対応方法】</br>
Googleサポートに問い合わせる!

無事に署名の変更に対応して頂きました。Googleさんありがとう。</br>
詳しい内容については、こちら↓からご確認ください。</br>
[GooglePlayStoreで困った](https://github.com/ito-a/app_twitter/blob/main/development/google_faq.md)
</details>