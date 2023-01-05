### Flutterのテストやってみた

#### テストの種類
- ユニット
  - Classごとの単体テスト 
  - apiの疎通等もモックを使ってテストできる
- ウィジェット
  - 文字通りWidgetのテスト
  - マッチャーを使って特定の文字やボタンが描画されているかをテストする
- インテグレーション

#### テストファイルの命名
テストをしたいファイル名の後ろに’_test’ を付けて命名する必要があるらしい
```
  lib/
    user.dart
  test/
    user_test.dart
```
```
  lib/
    model/
       user.dart
  test/
   model/
    user_test.dart
```

#### テストの実行
```
下記コマンドで全てのテストを実行
  flutter test
  
特定のテストファイルを実行
  flutter test test/XXXXXX_test.dart
```
### ユニットテスト
<details>
<summary>超基本編</summary>

```
// テスト対象のClass
class Counter {
  int value = 0;
  void increment() => value++;
  void decrement() => value--;
}
```
```
// テストコード
import 'package:flutter_test/flutter_test.dart';
import 'package:sample_app_test/counter.dart';

void main() {
  group('Counter', () {
    test('最初は０から始まるか', () {
      expect(Counter().value, 0);
    });

    test('Counterクラスのincrementを実行したときのvalueは１か', () {
      final counter = Counter();

      counter.increment();

      expect(counter.value, 1);
    });

    test('Counterクラスのdecrementedを実行したときのvalueは-１か', () {
      final counter = Counter();

      counter.decrement();

      expect(counter.value, -1);
    });
  });
}
```
解説
- groupは複数のテストを書くときに囲む。１つのテストだけなら下記表記でも可
```
  test('Counterクラスのincrementを実行したときのvalueは１か', () {
    final counter = Counter();
    counter.increment();
    expect(counter.value, 1);
  });
```

</details>

<details>
<summary>非同期編</summary>
テストライブラリとして有名なのはmockitoだが、mocktailも登場してる<br>
初心者にはmocktailがオススメとのこと<br>
 
こちら絶賛迷宮入りなのでComing Soon...
</details>

### ウィジェットテスト

<details>
<summary>超基本編</summary>

```
// テスト対象のWidget
void main() {
  runApp(const MyApp(
    title: 'Example',
  ));
}

class MyApp extends StatefulWidget {
  const MyApp({
    super.key,
    required this.title,
  });

  final String title;

  @override
  State<MyApp> createState() => _MyAppState();
}

class _MyAppState extends State<MyApp> {
...
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
    theme: ThemeData(
    primarySwatch: Colors.blue,
    ),
    home: Scaffold(
    appBar: AppBar(
    title: Text(widget.title),
  ),
...
```
```
// テストコード
import 'package:flutter_test/flutter_test.dart';
import 'package:sample_app_test/main.dart';

void main() {
testWidgets('ツールバーに「T」が表示される', (WidgetTester tester) async {
  await tester.pumpWidget(const MyApp(title: 'T'));
  final titleFinder = find.text('T');
  expect(titleFinder, findsOneWidget);
});
}
```
解説
- 今回はマッチャー(findsOneWidget)を利用してますが、色々あるみたいです。ちょっと調べた。
```
- findsOneWidget: ウィジェットが１つ見つかる
- findsNothing: ウィジェットが見つからない
- findsWidgets: 1つ以上のウィジェットが見つかった
- findsNWidgets: 特定の数のウィジェットが見つかった
- findsAtLeastNWidgets:少なくとも特定の数のウィジェットを見つけた
```
</details>
