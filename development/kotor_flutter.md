#### KotorのFlutter版

#### Riverpodを学んでみよう
1.Riverpodの導入
```
 dependencies:
  flutter:
    sdk: flutter
  flutter_riverpod: ^2.1.1 ←　追加
```
flutter pud get 忘れずに!

2.Riverpodのサポートプラグイン導入

AndroidStudio > Preference > Plugin >`Flutter Riverpod Snippets`

3.いろいろなプロバイダ

| 種類                     | 例                     |
|------------------------|-----------------------|
| Provider               | 定数                    |
| StateProvider          | 変数                    |
| FutureProvider         | Api呼び出し               |
| StreamProvider         | Api呼び出し結果のStream      |
| StateNotifierProvider  | イミュータブルで複雑なステートオブジェクト |
| ChangeNotifierProvider | ミュータブルで複雑なステートオブジェクト  |

4. プロバイダ修飾子は2種類

- autoDispose
- family
```
final userProvider = FutureProvider.autoDispose.family<User, int>((ref, userId) async {
  return fetchUser(userId);
});
```
複数の修飾子を同時に使用することも可能

5. refの呼び出しと使い方

Providerはrefを用いて他のProviderとやりとりをします。</br>
refの取得方法
```
 StatelessWidget を ConsumerWidget に置き換えるのが基本です。
 ⇨ ほぼ同等のもので、buildメゾットの第２パラメーターが存在するだけの違い
 
 StatefulWidget+State の代わりに ConsumerStatefulWidget+ConsumerState を継承する
  ⇨ build メソッドに ref オブジェクトは渡されていませんが使用可能です
  
 HookWidget の代わりに HookConsumerWidget を継承する
  ⇨ flutter_hooksを利用するユーザー向け 
```
Riverpodさんとrefは超仲良し。refオブジェクトが居ないと何もできないんだ

refの使い方
```
ref.watch: 値が変化すると、その値に依存するウィジェットやプロバイダの更新が行われる。
ref.listen: 値が変化するたびに呼び出されるコールバック関数（画面遷移、ダイアログの表示など）を登録する。
ref.read: 監視はしない。クリックイベント等の発生時に、その時点での値を取得する場合に使用できる
```
公式さんは`ref.watch`推し!</br>
`ref.read`の利用は可能な限り避けて、以下`ref.watch`が使えない場所に限り利用して欲しい。</br>
`ref.watch`は`onPressed`などの非同期での使用を禁止。</br>
また`initState`を始め`State`のライフサイクルメソッド内での使用も避けて</br>
これらの場所は`ref.read`を使って欲しいとのこと。</br>
`ref.read`は`buildメソッド`の中で使わないこと!</br>

6.SampleのCounterアプリを変更してみる
```
// buildメソッド内でreadを利用してるけど、watchが使えないPress内なのでOK
final counterProvider = StateProvider((ref) => 0);

class MyApp extends ConsumerWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final counter = ref.watch(counterProvider);
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: const Text('Example'),
        ),
        body: Center(
          child: Column(
            children: [
              Text(counter.toString()),
              ElevatedButton(
                onPressed: () => ref.read(counterProvider.notifier).state++,
                child: const Text('button'),
              )
            ],
          ),
        ),
      ),
    );
  }
}
```

`onPressed: () => ref.watch(counterProvider.state).state++,`と書く人もいるみたいですが、現在は非推奨です。

#### lintの追加
lintってデフォルトで導入されるんじゃなかったけ?と思いつつ入ってないので導入。</br>

設定方法
```
app/
  analysis_options.yaml (lintのルールを設定するファイル)
```

```
dev_dependencies:
  flutter_lints: ^2.0.1
  pedantic_mono: ^1.20.1
```
→ DartAnalysisで結果を確認できるようになる。(結果: 2warning 58hints)

#### iOSの設定
- xcodeにAppId(アカウント: dvsuser@da-vinci-studio.net)の追加が必要